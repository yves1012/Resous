# Pandas介绍

## 什么是Pandas

一个开源的Python类库：用于数据分析、数据处理、数据可视化，其主要特点有：
* 高性能。
* 容易使用的数据结构。
* 容易使用的数据分析工具。

## Pandas读取数据
### 读取纯文本文件
#### 读取CSV，使用默认的标题行、逗号分隔符
```
import pandas as pd

ratings = pd.read_csv('./ratings.csv')  # 使用pd.read_csv读取数据
print(ratings.head())  # 查看前几行数据
print(ratings.shape)  # 查看数据的形状，返回(行数、列数)
print(ratings.columns)  # 查看列名列表
print(ratings.index)  # 查看索引列
print(ratings.dtypes)  # 查看每列的数据类型
```

#### 读取txt文件，自己指定分隔符、列名
```
import pandas as pd

vuv = pd.read_csv(
    './access_pvuv.txt',
    sep="\t",
    header=None,
    names=['pdate', 'pv', 'uv']
)
print(vuv)
```

### 读取Excel文件
```
import pandas as pd

pvuv = pd.read_excel('./access_pvuv.xlsx')
print(pvuv)
```

### 读取MySQL数据库
```
import pandas as pd
import pymysql

conn = pymysql.connect(
        host='127.0.0.1',
        user='root',
        password='12345678',
        database='test',
        charset='utf8'
    )
mysql_page = pd.read_sql("select * from crazyant_pvuv", con=conn)
print(mysql_page)
```

## Pandas数据结构

### Series
Series是一种类似于一维数组的对象，它由一组数据（不同数据类型）以及一组与之相关的数据标签（即索引）组成。

#### 仅有数据列表即可产生最简单的Series
```
s1 = pd.Series([1, 'a', 5.2, 7])
print(s1)

0      1    # 左侧为索引，右侧是数据
1      a
2    5.2
3      7
dtype: object

print(s1.index)  # 获取索引
print(s1.values)  # 获取数据
```

#### 创建一个具有标签索引的Series
```
s2 = pd.Series([1, 'a', 5.2, 7], index=['d', 'b', 'a', 'c'])
print(s2.index)
```

#### 使用Python字典创建Series
```
sdata = {'Ohio': 35000, 'Texas': 72000, 'Oregon': 16000, 'Utah': 5000}
s3 = pd.Series(sdata)
print(s3)
```

#### 根据标签索引查询数据
类似Python的字典dict。

### DataFrame
DataFrame是一个表格型的数据结构：

* 每列可以是不同的值类型（数值、字符串、布尔值等）。
* 既有行索引index,也有列索引columns。
* 可以被看做由Series组成的字典。

创建dataframe最常用的方法，见读取纯文本文件、excel、mysql数据库等方法。

#### 根据多个字典序列创建dataframe
```
data = {
    'state': ['Ohio', 'Ohio', 'Ohio', 'Nevada', 'Nevada'],
    'year': [2000, 2001, 2002, 2001, 2002],
    'pop': [1.5, 1.7, 3.6, 2.4, 2.9]
}
df = pd.DataFrame(data)
```

### 从DataFrame中查询出Series
* 如果只查询一行、一列，返回的是pd.Series。
* 如果查询多行、多列，返回的是pd.DataFrame。

```
import pandas as pd
import numpy as np

data = {
    'state': ['Ohio', 'Ohio', 'Ohio', 'Nevada', 'Nevada'],
    'year': [2000, 2001, 2002, 2001, 2002],
    'pop': [1.5, 1.7, 3.6, 2.4, 2.9]
}
df = pd.DataFrame(data)
print(df['year'])  # 查询一列，结果是一个pd.Series
print(df[['year', 'pop']])  # 查询多列，结果是一个pd.DataFrame
print(df.loc[1])  # 查询一行，结果是一个pd.Series
print(df.loc[:2])  # 查询多行，结果是一个pd.DataFrame，注意：含尾
```

## Pandas查询数据
Pandas查询数据的方法主要有以下几种：
* df.loc方法，根据行、列的标签值查询。
* df.iloc方法，根据行、列的数字位置查询。
* df.where方法。
* df.query方法。

其中，df.loc方法既能查询，又能覆盖写入，强烈推荐，其中，Pandas使用df.loc查询数据的方法：
* 使用单个label值查询数据。
* 使用值列表批量查询。
* 使用数值区间进行范围查询。
* 使用条件表达式查询。
* 调用函数查询。

注意:
* 以上查询方法，既适用于行，也适用于列。
* 注意观察降维dataFrame>Series>值。

### 读取数据
```
import pandas as pd

df = pd.read_csv('./beijing_tianqi_2018.csv')
df.set_index('ymd', inplace=True)  # 设定索引为日期，方便按日期筛选
df.loc[:, "bWendu"] = df["bWendu"].str.replace("℃", "").astype('int32')  # 将温度后的℃符号替换掉
df.loc[:, "yWendu"] = df["yWendu"].str.replace("℃", "").astype('int32')
```

### 使用单个label值查询数据
行或者列，都可以只传入单个值，实现精确匹配。
```
print(df.loc['2018-01-03', 'bWendu'])  # 得到单个值
print(df.loc['2018-01-03', ['bWendu', 'yWendu']])  # 得到一个Series
```

### 使用值列表批量查询
```
print(df.loc[['2018-01-03', '2018-01-04', '2018-01-05'], 'bWendu'])  # 得到Series
print(df.loc[['2018-01-03', '2018-01-04', '2018-01-05'], ['bWendu', 'yWendu']])  # 得到DataFrame
```

### 使用数值区间进行范围查询
注意：区间既包含开始，也包含结束。
```
print(df.loc['2018-01-03':'2018-01-05', 'bWendu'])  # 行index按区间
print(df.loc['2018-01-03', 'bWendu':'fengxiang'])  # 列index按区间
print(df.loc['2018-01-03':'2018-01-05', 'bWendu':'fengxiang'])  # 行和列都按区间查询
```

### 使用条件表达式查询
```
print(df.loc[df["yWendu"] < -10, :])  # 简单条件查询，最低温度低于-10度的列表
# 查询最高温度小于30度，并且最低温度大于15度，并且是晴天，并且天气为优的数据
print(df.loc[(df["bWendu"] <= 30) & (df["yWendu"] >= 15) & (df["tianqi"] == '晴') & (df["aqiLevel"] == 1), :])
```

### 调用函数查询
```
def query_my_data(df):
    return df.index.str.startswith("2018-09") & (df["aqiLevel"] == 1)

print(df.loc[query_my_data(df), :])
```

## Pandas怎样新增数据列
在进行数据分析时，经常需要按照一定条件创建新的数据列，然后进行进一步分析，常用方法有：
* 直接赋值。
* df.apply方法。
* df.assign方法。
* 按条件选择分组分别赋值。

### 直接赋值
```
import pandas as pd

df = pd.read_csv('./beijing_tianqi_2018.csv')
df.set_index('ymd', inplace=True)  # 设定索引为日期，方便按日期筛选
df.loc[:, "bWendu"] = df["bWendu"].str.replace("℃", "").astype('int32')  # 将温度后的℃符号替换掉
df.loc[:, "yWendu"] = df["yWendu"].str.replace("℃", "").astype('int32')

df.loc[:, "wencha"] = df["bWendu"] - df["yWendu"]
print(df.head())
```

### df.apply方法
Apply a function along an axis of the DataFrame.
```
import pandas as pd

df = pd.read_csv('./beijing_tianqi_2018.csv')
df.set_index('ymd', inplace=True)  # 设定索引为日期，方便按日期筛选
df.loc[:, "bWendu"] = df["bWendu"].str.replace("℃", "").astype('int32')  # 将温度后的℃符号替换掉
df.loc[:, "yWendu"] = df["yWendu"].str.replace("℃", "").astype('int32')


def get_wendu_type(x):
    if x["bWendu"] > 33:
        return '高温'
    if x["yWendu"] < -10:
        return '低温'
    return '常温'


df.loc[:, "wendu_type"] = df.apply(get_wendu_type, axis=1)  # 注意需要设置axis==1，这是series的index是columns
df["wendu_type"].value_counts()  # 查看温度类型的计数
print(df.head())
```

### df.assign方法
```
import pandas as pd

df = pd.read_csv('./beijing_tianqi_2018.csv')
df.set_index('ymd', inplace=True)  # 设定索引为日期，方便按日期筛选
df.loc[:, "bWendu"] = df["bWendu"].str.replace("℃", "").astype('int32')  # 将温度后的℃符号替换掉
df.loc[:, "yWendu"] = df["yWendu"].str.replace("℃", "").astype('int32')

df.assign(
    yWendu_huashi=lambda x: x["yWendu"] * 9 / 5 + 32,
    # 摄氏度转华氏度
    bWendu_huashi=lambda x: x["bWendu"] * 9 / 5 + 32
)
```

### 按条件选择分组分别赋值
按条件先选择数据，然后对这部分数据赋值新列。
```
import pandas as pd

df = pd.read_csv('./beijing_tianqi_2018.csv')
df.set_index('ymd', inplace=True)  # 设定索引为日期，方便按日期筛选
df.loc[:, "bWendu"] = df["bWendu"].str.replace("℃", "").astype('int32')  # 将温度后的℃符号替换掉
df.loc[:, "yWendu"] = df["yWendu"].str.replace("℃", "").astype('int32')

df['wencha_type'] = ''
df.loc[df["bWendu"] - df["yWendu"] > 10, "wencha_type"] = "温差大"
df.loc[df["bWendu"] - df["yWendu"] <= 10, "wencha_type"] = "温差正常"
print(df["wencha_type"].value_counts())
```

## Pandas数据统计函数

### 汇总类统计
```
import pandas as pd

df = pd.read_csv('./beijing_tianqi_2018.csv')
df.set_index('ymd', inplace=True)  # 设定索引为日期，方便按日期筛选
df.loc[:, "bWendu"] = df["bWendu"].str.replace("℃", "").astype('int32')  # 将温度后的℃符号替换掉
df.loc[:, "yWendu"] = df["yWendu"].str.replace("℃", "").astype('int32')

print(df.describe())  # 一下子提取所有数字列统计结果
print(df["bWendu"].mean())  # 查看单个Series的数据
```

### 唯一去重
一般不用于数值列，而是枚举、分类列。
```
print(df['fengxiang'].unique())

['东北风' '北风' '西北风' '西南风' '南风' '东南风' '东风' '西风']
```

### 按值计数
```
print(df["fengxiang"].value_counts())

南风     92
西南风    64
北风     54
西北风    51
东南风    46
东北风    38
东风     14
西风      6
```

### 相关系数和协方差
用途（超级厉害）：

* 两只股票，是不是同涨同跌？程度多大？正相关还是负相关？
* 产品销量的波动，跟哪些因素正相关、负相关，程度有多大？

对于两个变量X、Y：

* 协方差：衡量同向反向程度，如果协方差为正，说明X，Y同向变化，协方差越大说明同向程度越高；如果协方差为负，说明X，Y反向运动，协方差越小说明反向程度越高。
* 相关系数：衡量相似度程度，当他们的相关系数为1时，说明两个变量变化时的正向相似度最大，当相关系数为－1时，说明两个变量变化的反向相似度最大。

```
print(df.cov())  # 协方差矩阵

              bWendu      yWendu          aqi   aqiLevel
bWendu    140.613247  135.529633    47.462622   0.879204
yWendu    135.529633  138.181274    16.186685   0.264165
aqi        47.462622   16.186685  2697.364564  50.749842
aqiLevel    0.879204    0.264165    50.749842   1.060485
```

```
print(df.corr())  # 相关系数矩阵

            bWendu    yWendu       aqi  aqiLevel
bWendu    1.000000  0.972292  0.077067  0.071999
yWendu    0.972292  1.000000  0.026513  0.021822
aqi       0.077067  0.026513  1.000000  0.948883
aqiLevel  0.071999  0.021822  0.948883  1.000000
```

```
print(df["aqi"].corr(df["bWendu"]))  # 单独查看空气质量和最高温度的相关系数
print(df["aqi"].corr(df["bWendu"] - df["yWendu"]))  # 空气质量和温差的相关系数
```

## Pandas对缺失值的处理
Pandas使用这些函数处理缺失值：

* isnull和notnull：检测是否是空值，可用于df和series。
* dropna：丢弃、删除缺失值。
* —> axis : 删除行还是列，{0 or ‘index’, 1 or ‘columns’}, default 0。
* —> how : 如果等于any则任何值为空都删除，如果等于all则所有值都为空才删除。
* —> inplace : 如果为True则修改当前df，否则返回新的df。
* fillna：填充空值。
* —> value：用于填充的值，可以是单个值，或者字典（key是列名，value是值）。
* —> method : 等于ffill使用前一个不为空的值填充forword。
* —> fill；等于bfill使用后一个不为空的值填充backword fill。
* —> axis : 按行还是列填充，{0 or ‘index’, 1 or ‘columns’}。
* —> inplace : 如果为True则修改当前df，否则返回新的df。

### 特殊Excel的读取、清洗、处理
```
import pandas as pd

df = pd.read_excel('./student_excel.xlsx', skiprows=2)  # 读取excel的时候，忽略前几个空行
print(df.isnull())  # 检测空值
print(df["分数"].isnull())  # 检测某列空值
print(df.loc[df["分数"].notnull(), :])  # 筛选没有空分数的所有行
df.dropna(axis="columns", how='all', inplace=True)  # 删除掉全是空值的列
print(df)
df.dropna(axis="index", how='all', inplace=True)  # 删除掉全是空值的行
df.fillna({"分数": 0})  # 将分数列为空的填充为0分
print(df)
df.loc[:, '姓名'] = df['姓名'].fillna(method="ffill") # 将姓名的缺失值填充
```

## Pandas的SettingWithCopyWarning报警

### 问题复现
```
import pandas as pd

df = pd.read_csv('./beijing_tianqi_2018.csv')
df.loc[:, "bWendu"] = df["bWendu"].str.replace("℃", "").astype('int32')  # 替换掉温度的后缀℃
df.loc[:, "yWendu"] = df["yWendu"].str.replace("℃", "").astype('int32')

# 问题复现
condition = df["ymd"].str.startswith("2018-03")  # 只选出3月份的数据用于分析
df[condition]["wen_cha"] = df["bWendu"]-df["yWendu"]

# /Users/yves/.local/share/virtualenvs/DataAnalysis-ePf6BtZr/bin/python /Users/yves/Documents/Github/Python/DataAnalysis/Chapter1.py
# /Users/yves/Documents/Github/Python/DataAnalysis/Chapter1.py:19: SettingWithCopyWarning:
# A value is trying to be set on a copy of a slice from a DataFrame.
# Try using .loc[row_indexer,col_indexer] = value instead
#
# See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
#   df[condition]["wen_cha"] = df["bWendu"]-df["yWendu"]
#
# Process finished with exit code 0
```

### 原因
发出警告的代码 df[condition]["wen_cha"] = df["bWendu"]-df["yWendu"]

相当于：df.get(condition).set(wen_cha)，第一步骤的get发出了报警，链式操作其实是两个步骤，先get后set，get得到的dataframe可能是view也可能是copy，pandas发出警告。

核心要诀：pandas的dataframe的修改写操作，只允许在源dataframe上进行，一步到位。

[官网文档](https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy)

### 解决方法
将get+set的两步操作，改成set的一步操作。
```
condition = df["ymd"].str.startswith("2018-03")  # 只选出3月份的数据用于分析

df.loc[condition, "wen_cha"] = df["bWendu"]-df["yWendu"]
print(df[condition].head())
```

## Pandas数据排序

Series的排序：Series.sort_values(ascending=True, inplace=False)

参数说明：

* ascending：默认为True升序排序，为False降序排序
* inplace：是否修改原始Series

DataFrame的排序：DataFrame.sort_values(by, ascending=True, inplace=False)

参数说明：
* by：字符串或者List<字符串>，单列排序或者多列排序
* ascending：bool或者List
* inplace：是否修改原始DataFrame

