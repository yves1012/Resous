# 创建第一个 Django 应用

## 创建项目
创建项目目录并安装虚拟环境：
```
yvesdeMacBook-Air:Python yves$ mkdir Propaganda  // 创建项目目录
yvesdeMacBook-Air:Python yves$ cd Propaganda
yvesdeMacBook-Air:Python yves$ pipenv install  // 安装虚拟环境
Warning: the environment variable LANG is not set!
We recommend setting this in ~/.profile (or equivalent) for proper expected behavior.
Creating a virtualenv for this project…
Pipfile: /Users/yves/Documents/GitHub/Python/Propaganda/Pipfile
Using /usr/local/bin/python3 (3.6.4) to create virtualenv…
⠧ Creating virtual environment...Already using interpreter /usr/local/bin/python3
Using base prefix '/Library/Frameworks/Python.framework/Versions/3.6'
New python executable in /Users/yves/.local/share/virtualenvs/Propaganda-ojQAzsrM/bin/python3
Also creating executable in /Users/yves/.local/share/virtualenvs/Propaganda-ojQAzsrM/bin/python
Installing setuptools, pip, wheel...
done.

✔ Successfully created virtual environment! 
Virtualenv location: /Users/yves/.local/share/virtualenvs/Propaganda-ojQAzsrM
Creating a Pipfile for this project…
Pipfile.lock not found, creating…
Locking [dev-packages] dependencies…
Locking [packages] dependencies…
Updated Pipfile.lock (ca72e7)!
Installing dependencies from Pipfile.lock (ca72e7)…
  🐍   ▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉ 0/0 — 00:00:00
To activate this project's virtualenv, run pipenv shell.
Alternatively, run a command inside the virtualenv with pipenv run.
```

启动开发环境并安装Django：
```
yvesdeMacBook-Air:Propaganda yves$ pipenv shell  // 启动虚拟环境
Launching subshell in virtual environment…
bash-3.2$  . /Users/yves/.local/share/virtualenvs/Propaganda-ojQAzsrM/bin/activate
(Propaganda) bash-3.2$ pipenv install Django  // 安装Django
Installing Django…
Adding Django to Pipfile's [packages]…
✔ Installation Succeeded 
Pipfile.lock (12ffd6) out of date, updating to (ca72e7)…
Locking [dev-packages] dependencies…
Locking [packages] dependencies…
✔ Success! 
Updated Pipfile.lock (12ffd6)!
Installing dependencies from Pipfile.lock (12ffd6)…
  🐍   ▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉ 4/4 — 00:00:04
```

Django常见命令有创建项目、创建应用、创建超级用户、数据表创建及更新、启动服务器等，这些命令都包含在 django-admin.py 和 manage.py 里。除此以外 manage.py 还包含其它有用的命令，基本包含：
```
创建新项目：django-admin startproject project_name
创建新应用：python manage.py startapp app_name
检测模型变化生成新的数据库迁移文件：python manage.py makemigrations [app_name]
同步数据库与模型：python manage.py migrate
启动服务器：python manage.py runserver
创建超级用户：python manage.py createsuperuser
修改用户密码：python manage.py changepassword username
打开交互终端：python manage.py shell（dbshell指数据库交互）
查看当前版本：python manage.py version
清空数据库内容只留下空表：python manage.py flush	
搜集静态文件：python manage.py collectstatic
```

创建Django项目并启动验证是否成功：
```
(Propaganda) bash-3.2$ django-admin startproject Propaganda  // 创建项目
(Propaganda) bash-3.2$ ll  // 项目文件结构
drwxr-xr-x   7 yves  staff   224 Dec  6 21:06 ./
drwxr-xr-x  12 yves  staff   384 Dec  6 20:34 ../
-rw-r--r--   1 yves  staff   168 Dec  6 20:37 Pipfile
-rw-r--r--   1 yves  staff  1639 Dec  6 20:37 Pipfile.lock
drwxr-xr-x   7 yves  staff   224 Dec  6 20:56 Propaganda/
-rwxr-xr-x   1 yves  staff   630 Dec  6 20:56 manage.py*
(Propaganda) bash-3.2$ python manage.py runserver  // 启动项目
Watching for file changes with StatReloader
Performing system checks...

System check identified no issues (0 silenced).

You have 17 unapplied migration(s). Your project may not work properly until you apply the migrations for app(s): admin, auth, contenttypes, sessions.
Run 'python manage.py migrate' to apply them.

December 06, 2019 - 13:14:35
Django version 3.0, using settings 'Propaganda.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```
在浏览器输入 http://127.0.0.1:8000/ 看到Django启动页即表示项目创建成功。

## 创建应用与细节优化
### 应用创建
项目创建成功并验证通过后就可以创建相关应用：
```
(Propaganda) bash-3.2$ python manage.py startapp chatriq
```

创建成功后需要在 settings.py 文件中加入相应配置：
```
INSTALLED_APPS = [
    'chatriq'
]
```

### 项目细节优化
在项目根目录下创建 apps 包目录，用于统一管理后期创建的应用，这需要在 settings.py 文件里新增一条配置：
```
sys.path.insert(0, os.path.join(BASE_DIR, 'apps'))  # 将apps目录加到python的搜索目录中去
```

修改 settings.py 文件里的配置：
```
LANGUAGE_CODE = 'zh-hans'
TIME_ZONE = 'Asia/Shanghai'
USE_TZ = False
```

在根目录下创建 static 目录用于存放静态文件，创建 templates 文件夹用于存放模板文件，并在 settings.py 文件中添加配置：
```
STATIC_URL = '/static/'
STATICFILES_DIRS = (
    os.path.join(BASE_DIR, 'static'),
)
```

最终，目录结构如下：
```
(Propaganda) bash-3.2$ ll
-rw-r--r--   1 yves  staff   168 Dec  6 20:37 Pipfile
-rw-r--r--   1 yves  staff  1639 Dec  6 20:37 Pipfile.lock
drwxr-xr-x   8 yves  staff   256 Dec  6 21:43 Propaganda/
drwxr-xr-x   4 yves  staff   128 Dec  6 21:41 apps/
-rw-r--r--   1 yves  staff     0 Dec  6 21:14 db.sqlite3
-rwxr-xr-x@  1 yves  staff   630 Dec  6 20:56 manage.py
drwxr-xr-x   2 yves  staff    64 Dec  6 21:26 static/
drwxr-xr-x   2 yves  staff    64 Dec  6 21:26 templates/
```

## 数据库配置
在 settings.py 文件中可以配置项目连接的数据库信息，由于本项目暂时不涉及数据库相关操作。
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}
```

生成表迁移数据并在数据库中创建对应表文件：
```
(Propaganda) bash-3.2$ python manage.py makemigrations
No changes detected
(Propaganda) bash-3.2$ python manage.py migrate
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, sessions
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying admin.0003_logentry_add_action_flag_choices... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying auth.0009_alter_user_last_name_max_length... OK
  Applying auth.0010_alter_group_name_max_length... OK
  Applying auth.0011_update_proxy_permissions... OK
  Applying sessions.0001_initial... OK
```

至此，Django项目应用创建完毕，后续更新编码过程中的细节与注意事项。