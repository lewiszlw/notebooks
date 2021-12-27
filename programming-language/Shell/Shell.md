# 变量

# 数组
定义数组
```
array_name = (value1 value2 ... valuen)
```
Shell 数组用括号来表示，元素用"空格"符号分割开。

# 函数

# 条件语句

# 循环语句
遍历数组
1. 标准的for循环
```
for(( i=0;i<${#array[@]};i++)) 
#${#array[@]}获取数组长度用于循环
do
    echo ${array[i]};
done;
```
2. for … in
```
# 不带数组下标
for element in ${array[@]}
#也可以写成for element in ${array[*]}
do
    echo $element
done

# 带数组下标
for i in "${!arr[@]}";   
do   
    printf "%s\t%s\n" "$i" "${arr[$i]}"  
done
```
3. While循环法：
```
i=0  
while [ $i -lt ${#array[@]} ]  
#当变量（下标）小于数组长度时进入循环体
do  
    echo ${ array[$i] }  
    #按下标打印数组元素
    let i++  
done 
```

