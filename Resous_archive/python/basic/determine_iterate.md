# 判断与循环

## 条件判断
计算机之所以能做很多自动化的任务，因为它可以自己做条件判断。

比如，输入用户年龄，根据年龄打印不同的内容，在Python程序中，用if语句实现：
```
age = 20
if age >= 18:
    print('your age is', age)
    print('adult')
```
根据Python的缩进规则，如果if语句判断是True，就把缩进的两行print语句执行了，否则，什么也不做。

也可以给if添加一个else语句，意思是，如果if判断是False，不要执行if的内容，去把else执行了：
```
age = 3
if age >= 18:
    print('your age is', age)
    print('adult')
else:
    print('your age is', age)
    print('teenager')
```
当然上面的判断是很粗略的，完全可以用elif做更细致的判断：
```
age = 3
if age >= 18:
    print('adult')
elif age >= 6:
    print('teenager')
else:
    print('kid')
```
if判断条件还可以简写，比如写：
```
if x:
    print('True')
```
只要x是非零数值、非空字符串、非空list等，就判断为True，否则为False。

## 循环
Python的循环有两种，一种是for...in循环，依次把list或tuple中的每个元素迭代出来，看例子：
```
names = ['Michael', 'Bob', 'Tracy']
for name in names:
    print(name)
```
执行这段代码，会依次打印names的每一个元素：
```
Michael
Bob
Tracy
```
所以for x in ...循环就是把每个元素代入变量x，然后执行缩进块的语句。

再比如我们想计算1-10的整数之和，可以用一个sum变量做累加：
```
sum = 0
for x in [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]:
    sum = sum + x
print(sum)
```
如果要计算1-100的整数之和，从1写到100有点困难，幸好Python提供一个range()函数，可以生成一个整数序列，再通过list()函数可以转换为list。比如range(5)生成的序列是从0开始小于5的整数：
```
>>> list(range(5))
[0, 1, 2, 3, 4]
```
range(101)就可以生成0-100的整数序列，计算如下：
```
sum = 0
for x in range(101):
    sum = sum + x
print(sum)
```
第二种循环是while循环，只要条件满足，就不断循环，条件不满足时退出循环。比如我们要计算100以内所有奇数之和，可以用while循环实现：
```
sum = 0
n = 99
while n > 0:
    sum = sum + n
    n = n - 2
print(sum)
```
在循环中，break语句可以提前退出循环。
```
n = 1
while n <= 100:
    if n > 10: # 当n = 11时，条件满足，执行break语句
        break # break语句会结束当前循环
    print(n)
    n = n + 1
print('END')
```
在循环过程中，也可以通过continue语句，跳过当前的这次循环，直接开始下一次循环。
```
n = 0
while n < 10:
    n = n + 1
    if n % 2 == 0: # 如果n是偶数，执行continue语句
        continue # continue语句会直接继续下一轮循环，后续的print()语句不会执行
    print(n)
```