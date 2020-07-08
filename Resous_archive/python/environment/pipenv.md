# Pipenv 包管理工具安装

## pipenv 是什么
pipenv 是 python 官方推荐的包管理工具，集成了 virtualenv、pyenv 和 pip 三者的功能于一身，类似于 php 中的 composer。

我们知道，为了方便管理 python 的虚拟环境和库，通常使用较多的是 virtualenv 、pyenv 和 pip，但是他们不够好用或者说不够偷懒。于是 requests 的作者 Kenneth Reitz 开发了用于创建和管理 python 虚拟环境的工具 —- pipenv。

它能够自动为项目创建和管理虚拟环境，从 Pipfile 文件中添加或者删除包，同时生成 Pipfile.lock 文件来锁定安装包的版本和依赖信息，避免构建错误。

pipenv 主要解决了以下问题：
- 不用再单独使用 virtualenv、pyenv 和 pip 了，现在它们结合到了一起。
- 不用再维护 requirement.txt 了，使用 Pipfile 和 Pipfile.lock 来代替。
- 可以在开发环境使用多个 python 版本。
- 在安装的 pyenv 条件下，可以自动安装需要的 python 版本。
- 安全，广泛地使用 Hash 校验，能够自动曝露安全漏洞。
- 随时查看图形化的依赖关系。

## 安装 pipenv
### 使用 pip 安装
```
$ pip install --user pipenv
```
这个命令在用户级别（非系统全局）下安装 pipenv。如果安装后 shell 提示找不到 pipenv 命令，你需要添加当前 Python 用户主目录的 bin 目录到 PATH 环境变量。

#### 碰到问题
显示 “Pipenv: Command Not Found” 错误时，按如下操作进行：
```
# 环境变量中添加如下配置
PYTHON_BIN_PATH="$(python3 -m site --user-base)/bin"
PATH="$PATH:$PYTHON_BIN_PATH"
```


### 使用 brew 安装
Mac 下使用 brew 安装软件应该是最方便的了，推荐使用：
```
brew install pipenv
```

升级 pipenv：
```
brew upgrade pipenv
```

## 常用命令
pipenv 具有的选项：
```
Options:
  --where             Output project home information.
  --venv              Output virtualenv information.
  --py                Output Python interpreter information.
  --envs              Output Environment Variable options.
  --rm                Remove the virtualenv.
  --bare              Minimal output.
  --completion        Output completion (to be eval'd).
  --man               Display manpage.
  --support           Output diagnostic information for use in GitHub issues.
  --site-packages     Enable site-packages for the virtualenv.  [env var: PIPENV_SITE_PACKAGES]
  --python TEXT       Specify which version of Python virtualenv should use.
  --three / --two     Use Python 3/2 when creating virtualenv.
  --clear             Clears caches (pipenv, pip, and pip-tools).  [env var: PIPENV_CLEAR]
  -v, --verbose       Verbose mode.
  --pypi-mirror TEXT  Specify a PyPI mirror.
  --version           Show the version and exit.
  -h, --help          Show this message and exit.
```

pipenv 可使用的命令参数：
```
Commands:
  check      Checks for security vulnerabilities and against PEP 508 markers provided in Pipfile.
  clean      Uninstalls all packages not specified in Pipfile.lock.
  graph      Displays currently-installed dependency graph information.
  install    Installs provided packages and adds them to Pipfile, or (if no packages are given), installs all packages from Pipfile.
  lock       Generates Pipfile.lock.
  open       View a given module in your editor.
  run        Spawns a command installed into the virtualenv.
  shell      Spawns a shell within the virtualenv.
  sync       Installs all packages specified in Pipfile.lock.
  uninstall  Un-installs a provided package and removes it from Pipfile.
  update     Runs lock, then sync.
```

一些例子：
```
Usage Examples:
   Create a new project using Python 3.7, specifically:
   $ pipenv --python 3.7

   Remove project virtualenv (inferred from current directory):
   $ pipenv --rm

   Install all dependencies for a project (including dev):
   $ pipenv install --dev

   Create a lockfile containing pre-releases:
   $ pipenv lock --pre

   Show a graph of your installed dependencies:
   $ pipenv graph

   Check your installed dependencies for security vulnerabilities:
   $ pipenv check

   Install a local setup.py into your virtual environment/Pipfile:
   $ pipenv install -e .

   Use a lower-level pip command:
   $ pipenv run pip freeze
```

## pipenv 使用过程
### 创建环境，安装指定 python 的版本信息：
```
yvesdeMacBook-Air:Python yves$ mkdir GeneralCrawler
yvesdeMacBook-Air:Python yves$ cd GeneralCrawler
yvesdeMacBook-Air:GeneralCrawler yves$ pipenv install
```
如果指定了 --two 或者 --three 选项参数，则会使用 python2 或者 python3 的版本安装，否则将使用默认的 python 版本来安装。当然也可以指定准确的版本信息：
```
yvesdeMacBook-Air:GeneralCrawler yves$ pipenv install --two
yvesdeMacBook-Air:GeneralCrawler yves$ pipenv install --three
yvesdeMacBook-Air:GeneralCrawler yves$ pipenv install --python 3.6.4
```
pipenv 会自动扫描系统寻找合适的版本信息，如果找不到的话，同时又安装了 pyenv 的话，则会自动调用 pyenv 下载对应版本的 python， 否则会报错。

这时候在当前 GeneralCrawler 环境下生成 Pipfile 和 Pipfile.lock 两个环境初始化文件。

### 进入|退出环境：
进入环境：
```
yvesdeMacBook-Air:GeneralCrawler yves$ pipenv shell 
```
退出环境：
```
(GeneralCrawler) bash-3.2$ exit
```

### 安装第三方包：
在安装第三方包之前，我们可以修改镜像源(建议使用清华源：https://pypi.tuna.tsinghua.edu.cn/simple )来加快下载速度：
```
(GeneralCrawler) bash-3.2$ vim Pipfile
url = "https://pypi.tuna.tsinghua.edu.cn/simple"
```
此时我们测试安装 requests 包：
```
(GeneralCrawler) bash-3.2$ pipenv install requests
```
此时，Pipfile 里有最新安装的包文件的信息，如名称、版本等。用来在重新安装项目依赖或与他人共享项目时，你可以用 Pipfile 来跟踪项目依赖。

Pipfile 是用来替代原来的 requirements.txt 的，内容类似下面这样。source 部分用来设置仓库地址，packages 部分用来指定项目依赖的包，dev-packages 部分用来指定开发环境需要的包，这样分开便于管理。
```
[[source]]
name = "pypi"
url = "https://pypi.tuna.tsinghua.edu.cn/simple"
verify_ssl = true

[dev-packages]

[packages]
requests = "*"

[requires]
python_version = "3.6"
```

Pipfile.lock 则包含你的系统信息，所有已安装包的依赖包及其版本信息，以及所有安装包及其依赖包的 Hash 校验信息。
```
{
    "_meta": {
        "hash": {
            "sha256": "223fe2288079b4bdf4932ad9f98e6e26b68e9468f2b83dff8dfa78a36cef768b"
        },
        "pipfile-spec": 6,
        "requires": {
            "python_version": "3.6"
        },
        "sources": [
            {
                "name": "pypi",
                "url": "https://pypi.tuna.tsinghua.edu.cn/simple",
                "verify_ssl": true
            }
        ]
    },
    "default": {
        "certifi": {
            "hashes": [
                "sha256:017c25db2a153ce562900032d5bc68e9f191e44e9a0f762f373977de9df1fbb3",
                "sha256:25b64c7da4cd7479594d035c08c2d809eb4aab3a26e5a990ea98cc450c320f1f"
            ],
            "version": "==2019.11.28"
        },
        "chardet": {
            "hashes": [
                "sha256:84ab92ed1c4d4f16916e05906b6b75a6c0fb5db821cc65e70cbd64a3e2a5eaae",
                "sha256:fc323ffcaeaed0e0a02bf4d117757b98aed530d9ed4531e3e15460124c106691"
            ],
            "version": "==3.0.4"
        },
        "idna": {
            "hashes": [
                "sha256:c357b3f628cf53ae2c4c05627ecc484553142ca23264e593d327bcde5e9c3407",
                "sha256:ea8b7f6188e6fa117537c3df7da9fc686d485087abf6ac197f9c46432f7e4a3c"
            ],
            "version": "==2.8"
        },
        "requests": {
            "hashes": [
                "sha256:11e007a8a2aa0323f5a921e9e6a2d7e4e67d9877e85773fba9ba6419025cbeb4",
                "sha256:9cf5292fcd0f598c671cfc1e0d7d1a7f13bb8085e9a590f48c010551dc6c4b31"
            ],
            "index": "pypi",
            "version": "==2.22.0"
        },
        "urllib3": {
            "hashes": [
                "sha256:a8a318824cc77d1fd4b2bec2ded92646630d7fe8619497b142c84a9e6f5a7293",
                "sha256:f3c5fd51747d450d4dcf6f923c81f78f811aab8205fda64b0aba34a4e48b0745"
            ],
            "version": "==1.25.7"
        }
    },
    "develop": {}
}
```

现在安装另一个包，再次查看这两个文件的内容。你会发现 Pipfile 现在包含两个安装包了，Pipfile.lock 也包含了所有已安装包的依赖包及其版本信息，以及所有安装包及其依赖包的 Hash 校验信息。每次你安装新的依赖包，这两个文件都会自动更新。

### 安装指定版本包：
```
(GeneralCrawler) bash-3.2$ pipenv install requests==2.22.0
```

### 安装开发环境下的包：
加 --dev 表示包括 Pipfile 的 dev-packages 中的依赖。
```
(GeneralCrawler) bash-3.2$ pipenv install requests==2.22.0 --dev
```

### 卸载第三方包：
```
(GeneralCrawler) bash-3.2$ pipenv uninstall requests
```

### 更新安装包：
```
(GeneralCrawler) bash-3.2$ pipenv update requests
(GeneralCrawler) bash-3.2$ pipenv update // 更新所有包，会删除所有软件包然后重新安装最新的版本
```

### 查看虚拟环境目录：
```
(GeneralCrawler) bash-3.2$ pipenv --venv
/Users/yves/.local/share/virtualenvs/GeneralCrawler-flvgS1J0
```

### 查看项目根目录：
```
(GeneralCrawler) bash-3.2$ pipenv --where
/Users/yves/Documents/GitHub/Python/GeneralCrawler
```

### 检查软件包的完整性
pipenv 可以帮你检查已安装的软件包有没有安全漏洞，运行下面的命令：
```
(GeneralCrawler) bash-3.2$ pipenv check
Checking PEP 508 requirements…
Passed!
Checking installed package safety…
All good!
```

### 查看依赖树
```
(GeneralCrawler) bash-3.2$ pipenv graph
requests==2.22.0
  - certifi [required: >=2017.4.17, installed: 2019.11.28]
  - chardet [required: >=3.0.2,<3.1.0, installed: 3.0.4]
  - idna [required: >=2.5,<2.9, installed: 2.8]
  - urllib3 [required: >=1.21.1,<1.26,!=1.25.1,!=1.25.0, installed: 1.25.7]
```

### 锁定版本
```
(GeneralCrawler) bash-3.2$ pipenv lock // 更新 lock 文件锁定当前环境的依赖版本
Locking [dev-packages] dependencies…
Locking [packages] dependencies…
✔ Success! 
Updated Pipfile.lock (ef768b)!
```

### 环境变量管理
如果你开发调试时需要配一堆环境变量，可以写到 .env 文件中，在 pipenv shell 进入虚拟环境时，它会帮你把这些环境变量加载好，非常方便。

例如写一个 .env 文件：
```
echo "FOO=hello foo" > .env
```
之后 pipenv shell 进入虚拟环境，echo $FOO 就能看环境变量的值 hello foo 已经设置好了。
