# 案件精灵常用代码片段整理

## 生成随机数
```
Function random_number(min_num, max_num)
    // 生成随机数
    If max_num > min_num Then
        Randomize
        Dim my_value
        my_value = Int(((max_num - min_num + 1) * Rnd()) + min_num)
        TracePrint "生成的随机数为："&my_value&"！"
        random_number = my_value             
    Else
        TracePrint "请确保最大数大于最小数!!"
    End If
End Function
```

