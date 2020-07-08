# 文件解析

### TXT文件解析
#### 打开文件
```
f = open(filename, access_mode='r', buffering=-1)

# filename:文件名
# access_mode：打开方式（r读，w写，a追加）
# buffering：默认值
```

#### 文件操作
```
f = open('./test.txt', 'r')
for line in f.readlines():
    print(line)
f.close()

# read()读取整个文件到字符串变量中
# readline()读取文件中的一行，然后返回整行（包括行结束符）到字符串变量中
# readlines()读取整个文件，返回一个字符串列表，列表中的每个元素都是一个字符串，代表一行
```

```
# write()将字符串输出到文件中
f = open('./test.txt', 'w')
f.write('welcome to my house')
f.close()

# writelines()将字符串列表写入文件，行结束符并不会自动被加入，需要手动在每行的结尾加入行结束符
f = open('./test.txt', 'w')
w = ['hello\n', 'world']
f.writelines(w)
f.close()
```

### JSON文件解析
JSON(JavaScript Object Notation) 是一种轻量级的数据交换格式。它是基于ECMAScript的一个子集。 JSON采用完全独立于语言的文本格式，但是也使用了类似于C语言家族的习惯(包括C、C++、Java、JavaScript、Perl、Python等)。这些特性使JSON成为理想的数据交换语言。易于人阅读和编写，同时也易于机器解析和生成(一般用于提升网络传输速率)。

json的[官方文档](http://json.org)。

#### API

API | Annotation
---|---
json.load( )   | 把文件打开，并把字符串变换为数据类型
json.loads( )  | 将 字符串 转换为 字典
json.dump( , ) | 将数据写入json文件中
json.dumps( )  | 将python中的 字典 转换为 字符串

#### 文件转换
```
import json

dict_data = {"City": "Nanjing", "Year": 2018, "Province": "Jiangshu"}

# dumps：将python中的 字典 转换为 字符串
str_1 = json.dumps(dict_data)
assert isinstance(str_1, str)

# loads: 将 字符串 转换为 字典
dict_2 = json.loads(str_1)
assert isinstance(dict_2, dict)
```

#### 文件操作
```
import json

dict_data = {"City": "Nanjing", "Year": 2018, "Province": "Jiangshu"}

# dump: 将数据写入json文件中
with open("./test.json", "w") as dump_f:
    json.dump(dict_data, dump_f, ensure_ascii=False, indent=4)

# load:把文件打开，并把字符串变换为数据类型
with open("./test.json", "r") as load_f:
    dict_3 = json.load(load_f)
assert isinstance(dict_3, dict)
```

### Excel文件解析
#### 四种方法使用范围比较

 \ | xlrd | xlwt | XlsxWriter | openpyxl 
---|---|---|---|---
 介绍 | 读取xls文件 | 写出xls文件 | 创建xlsx文件 | 读写xlsx、xlsm文件 
 读 | √ | × | × | √ 
 写 | × | √ | √ | √ 
 xls | √ | √ | × | × 
 xlsx | × | × | √ | √ 

注：Excel2003即XLS文件有大小限制即65536行256列，所以不支持大文件，而Excel2007以上即XLSX文件的限制则为1048576行16384列。

#### xlwt写入xls文件
```
import xlwt

book = xlwt.Workbook()  # 新建工作簿
table = book.add_sheet('Over', cell_overwrite_ok=True)  # 如果对同一单元格重复操作会发生overwrite Exception，cell_overwrite_ok为可覆盖
sheet = book.add_sheet('Test')  # 添加工作页
sheet.write(1, 1, 'A')  # 行，列，属性值,（1,1）为B2元素，从0开始计数
style = xlwt.XFStyle()  # 新建样式
font = xlwt.Font()  # 新建字体
font.name = 'Times New Roman'
font.bold = True
style.font = font  # 将style的字体设置为font
table.write(0, 0, 'Test', style)
book.save(filename_or_stream='excel_test.xls')  # 一定要保存
```

#### xlrd读取xls文件
```
import xlrd

data = xlrd.open_workbook('./excel_test.xls')  # 打开工作簿
print(data.sheet_names())  # 输出所有页的名称
table = data.sheets()[0]  # 获取第一页
# table = data.sheet_by_index(0)  # 通过索引获得第一页
# table = data.sheet_by_name('Over')  # 通过名称来获取指定页
nrows = table.nrows  # 为行数，整形
ncolumns = table.ncols  # 为列数，整形
print(nrows)
print(ncolumns)
print(type(nrows))
print(table.row_values(0))  # 输出第一行值，为一个列表
# 遍历输出所有行值
for row in range(nrows):
    print(table.row_values(row))
# 输出某一个单元格值
print(table.cell(0, 0).value)
print(table.row(0)[0].value)
```

#### 综合使用python-excel三大模块完成Excel内容追加写入
```
import xlwt, xlrd
from xlutils.copy import copy

data = xlrd.open_workbook('excel_test.xls', formatting_info=True)
excel = copy(wb=data)  # 完成xlrd对象向xlwt对象转换
excel_table = excel.get_sheet(0)  # 获得要操作的页
table = data.sheets()[0]
nrows = table.nrows  # 获得行数
ncols = table.ncols  # 获得列数
values = ["E", "X", "C", "E", "L"]  # 需要写入的值
for value in values:
    excel_table.write(nrows, 1, value)  # 因为单元格从0开始算，所以row不需要加一
    nrows = nrows + 1
excel.save('excel_test.xls')
```

#### XlsxWriter
```
import xlsxwriter

workbook = xlsxwriter.Workbook('demo1.xlsx')  # 创建一个Excel文件
worksheet = workbook.add_worksheet()  # 创建一个工作表sheet对象
worksheet.set_column('A:A', 20)  # 设定第一列（A）宽度为20像素
bold = workbook.add_format({'bold': True})  # 定义一个加粗的格式对象
# 向单元格写入数据
worksheet.write('A1', 'Hello')  # 向A1单元格写入'Hello'
worksheet.write('A2', 'World', bold)  # 向A2单元格写入'World'并使用bold加粗格式
worksheet.write('B2', u'中文字符', bold)  # 向B2单元格写入中文并使用加粗格式
worksheet.write(2, 0, 10)  # 用行列表示法（行列索引都从0开始）向第2行、第0列（即A3单元格）和第3行、第0列（即A4单元格）写入数字
worksheet.write(3, 0, 20)
worksheet.write(4, 0, '=SUM(A3:A4)')  # 求A3、A4单元格的和并写入A5单元格，由此可见可以直接使用公式
# worksheet.insert_image('B5', './welcome.jpg')  # 在B5单元格插入图片
workbook.close()  # 关闭并保存文件
```

#### 使用openpyxl写入xlsx文件
```
import openpyxl

data = openpyxl.Workbook()  # 新建工作簿
data.create_sheet('Sheet1')  # 添加页
# table = data.get_sheet_by_name('Sheet1') # 获得指定名称页
table = data.active  # 获得当前活跃的工作页，默认为第一个工作页
table.cell(1, 1, 'Test')  # 行，列，值 这里是从1开始计数的
data.save('excel_test.xlsx')  # 一定要保存
```

#### 使用openpyxl读取xlsx文件
```
import openpyxl

data = openpyxl.load_workbook('excel_test.xlsx')  # 读取xlsx文件
table = data['Sheet']  # 获得指定名称的页
nrows = table.rows  # 获得行数，类型为迭代器
ncols = table.columns  # 获得列数，类型为迭代器
for row in nrows:
    print(row)  # 包含了页名，cell，值
    line = [col.value for col in row]  # 取值
    print(line)
# 读取单元格
print(table.cell(1, 1).value)
```

#### 综合使用openpyxl对Excel内容追加写入
```
import openpyxl

data = openpyxl.load_workbook('excel_test.xlsx')
# print(data.sheetnames)  # 输出所有工作页的名称
# 取第一张表
sheetnames = data.sheetnames
table = data[sheetnames[0]]
# table = data.active
# print(table.title)  # 输出表名
nrows = table.max_row  # 获得行数
ncolumns = table.max_column  # 获得列数
print(nrows, ncolumns)
values = ['E', 'X', 'C', 'E', 'L']
for value in values:
    table.cell(nrows + 1, 1).value = value
    nrows = nrows + 1
data.save('excel_test.xlsx')
```
