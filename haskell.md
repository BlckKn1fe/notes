# Haskell 快速入门

维基百科：

>Haskell是一种标准化的，通用的纯函数式编程语言，有惰性求值和强静态类型。它的命名源自美国逻辑学家哈斯凯尔·加里，他在数理逻辑方面上的工作使得函数式编程语言有了广泛的基础。在Haskell中，“函数是第一类对象”。作为一门函数编程语言，主要控制结构是函数。



Haskell 中一旦变量赋值，不可修改，immutable



# 快速使用

GHC 为 Haskell 的编译器，全称为  Glasgow Haskell Compiler

GHCi 全称为  Glasgow Haskell Compiler Interactive

在 terminal 输入 ```ghci``` 进入交互式界面

|  指令   |          功能          |
| :-----: | :--------------------: |
|   :l    |     load，加载文件     |
|   :r    |     reload，重加载     |
| :t func | 查看一个方程的定义情况 |



# 基本操作

以下为一门编程语言最基本的一些操作



## 注释

单行注释：

```haskell
-- 注释，通过双横线实现
```

多行注释：

```haskell
{-
	这是多行注释，通过花括号和双横线实现
-}
```



## 基本数据类型

Haskell 中有以下数据类型

```haskell
-- Int 2^63 - 2^63 
maxInt = maxBound :: Int  -- 最大边界值

-- Float
-- Double 基本用这个，Float 有精度问题

-- Boolean True/False
flag :: Boolean
flag = True

-- Char 单引号
letter :: Char
letter = 'a'

-- Tuple

```



## 数学操作符

主要注意，带负号的必须加括号



## Pre/Infix Operator

以 mod 举例：

```haskell
modEx = mod 5 4  -- 这里就是 5 % 4，prefix
modEx2 = 5 `mod` 4  -- 等价于上面的，注意是反引号，infix
```



## 常用 Build-In Function

```haskell
-- PI
piVal = pi

-- 平方
squared2_2 = 2**2  -- 这种会有小数点
squared2_2 = 2^2  -- 这种没有小数点

-- round
roundVal = round 9.9999

-- truncate
truncateVal = truncate 9.9999

-- ceiling
ceilingVal = ceiling 9.9999

```



## 逻辑运算符

&& 为 and

|| 为 or

主要注意，not 后面要用括号括起来



# List 基础

List 在 Haskell 中非常重要，Haskell 中的 List 结构为 **LinkedList**

**List 只能存相同类型的元素**

## 生成 List

**生成 1 .. n 的 list：**

```haskell
--- list = [1..n]
zeroToTen = [1..10]

-- 设置步长，[a, a+s ... n]
evenList = [2, 4 .. 20]
```



**生成 character list：**

```haskell
 letterList = ['A', 'C' .. 'Z']
```



**生成 infinite list：**

这个生成好之后，我们需要多少，它就会提供给我们多少元素

```haskell
-- 省略号后不加其他元素
infinPow10 = [10, 20 ..]
```



**生成重复元素的 list：**

```haskell
listOf2s = repeat 2  -- 这个 repeat 生成的是一个 inifinit list
```



**生成固定重复次数的元素的list：**

使用 `replicate` 函数，第一个参数为次数，第二个参数为想要重复的元素

```haskell
listOfTen2s = replicate 10 2
```



**生成循环 List：**

比如想要生成一个 `[1, 2, 3, 4, 5, 1, 2, 3, 4, 5 ...]` 这样的 List

```haskell
cycleList = cycle [1, 2, 3, 4, 5]  -- 这个也是生成一个 inifinit list
```





## List 拼接

```haskell
list1 = [1, 2, 3]

list2 = list1 ++ [4, 5, 6]
```

List 和 List 之间使用 ```++``` 来拼接



## List 拼接元素

```haskell
list1 = [1, 2, 3]

list2 = 4 : 5 : list1  -- 这里的顺序不能改变，元素在前
```

Combine 元素使用 ```:``` 



## 获取 List 长度

使用 length 内置函数

```haskell
list = [1, 2, 3]

len = length list
```



## 反转 List

通过 reverse 内置函数

```haskell
list = [1, 2, 3]

revList = reverse list
```



## 判断 List 空的情况

使用 null 内置函数

```haskell
list = [1, 2, 3]

isEmpty = null list
```



## 获取特定 index 的元素

通过 ```!!``` 来获取

```haskell
list = [1, 2, 3]

secondNum = list !! 1
```



## 获取首/尾元素

通过 head/last 来获取

```haskell
list = [1, 2, 3, 4, 5]

firstNum = head list  -- 1

lastNum = last list  -- 5
```



## 获取除首/尾以外的元素

```init``` 获取除最后一个元素以外的其他元素：

```hask
list = [1, 2, 3, 4, 5]

listWithoutLastEl = init list
```



```tail``` 获取除第一个元素以外的其他元素：

```haskell
list = [1, 2, 3, 4, 5]

listWithoutFirstEl = tail list
```



## 获取特定数量的元素（和其剩余元素）

使用 ```take``` 来获得一个 List  中特定数量的元素（从前往后）

```haskell
list = [1, 2, 3, 4, 5]

first2Els = take 2 list  -- [1, 2]
```

获取除前特定数量元素以外的元素，使用 `drop`

 ```haskell
list = [1, 2, 3, 4, 5]

elsWithoutFirst2 = drop 2 list  -- [3, 4, 5]
 ```



## 判断某元素是否在 List 中

通过使用 `elem` 函数，可以用 prefix 或 infix 的方式

```haskell
list = [1, 2, 3, 4, 5]

is3InList = elem 3 list  -- prefix
is3InList2 = 3 `elem` list  -- infix
```



## 获取 List 中最值

获取最大值使用 `maximum`：

```haskell
list = [1, 2, 3, 4, 5]

maxVal = maximum list  -- 5
```



获取最小值使用 `minimum`：
```haskell
list = [1, 2, 3, 4, 5]

minVal = minimum list  -- 1
```



## 相加/相乘 List 中所有值

相加通过 `sum`：

```haskell
list = [1, 2, 3]

summation = sum list  -- 6
```



相乘通过 `product`：
```haskell
list = [2, 3, 4]

multiplication = product list  -- 24
```



# List 进阶



## 进阶生成 List

```haskell
-- 生成 3^n
pow3List = [3^n | n <- [1..10]]

-- 生成乘法表，注意括号位置！
mulTable = [[x * y | x <- [1..10]] | y <- [1..10]]
```





## 对每个元素操作/筛选

可以对每一个元素进行加减乘除等操作，也可以筛选出特定条件的数字，通过：

`[ x(operation) | x <- list , conditions ...]`

x 表示 list 中的单个的元素，可以在 ```|``` 左侧对其进行操作；

conditions 可以使用多个，用逗号隔开

```haskell
list = [1..200] -- 生成一个 1 到 200 的list

listTimes3 = [x * 3 | x <- list]  -- 每个元素乘 3

divisBy9N13 = [x | x <- list, x `mod` 13 == 0, x `mod` 9 == 0]  -- 可以被 13 和 9 整除的元素

```



## 合并 List

通过 `zipWith` 内置函数，实现两个 List 之间的互相操作，如下：

```haskell
list1 = [1, 2, 3]
list2 = [4, 5, 6]

sumOfList = zipWith (+) list1 list2  -- [5, 7, 9]
```

如果，其中一个 List 长于另外一个的话，则多出的部分会被忽略



## Filter

filter 内置函数可以过滤掉一些不符合条件的元素：

```haskell
list = [3, 1, 2, 5, 7, 4, 10]

listGreaterThan5 = filter (>5) list

```





# Tuple

在 tuple 中可以存不同类型的元素



## 获取首尾元素

获取第一个/最后一个元素通过 `fst` / `snd` 内置函数

```haskell
bobSmith = ("Bob Smith", "123 street", 32)

bobName = fst bobSmith
```



## zip 生成 tuple

当两个 List 用 zip 进行合并的时候（不是 zipWith），会把内容合并成 tuple

```haskell
name = ["Bob", "Tom", "Mary"]
address = ["123 Main", "234 North", "666 Center"]

nameNAddress = zip name address
-- [("Bob","123 Main"),("Tom","234 North"),("Mary","666 Center")]
```





# 函数

假设定义一个叫做 addMe 的函数，传入两个参数，并且把两个参数相加

```haskell
addMe :: Int -> Int -> Int
-- funcName param1 param2 = operations (returned value)

addme x y = x + y
```

addMe 为函数名

前面两个 Int 表两个参数的类型

最后一个 Int 表示 return type



# 递归

首先定义一个方程和一个 List

```haskell
list = [1, 2, 3, 4, 5]

times4 :: Int -> Int
time4 x = 4 * x
```

上面的 times4 表示一个函数，接收一个整数，并且对其乘 4，然后接下来定义另外一个函数

```haskell
mulBy4 :: [Int] -> [Int]
mulBy4 [] = []
mulBy4 (x:xs) = times4 x : mulBy4 xs  
```

上面的 x 表示 List 中的第一个元素，xs 表示剩下的；`mulBy4 xs` 即递归调用，base case 为 `mulBy4 [] = []`

以上，即是 `map` 内置函数的实现原理



# 条件判断

通过 if 来判断：

```haskell
doubleEvenNumber y = 
    if (mod y 2) == 1
        then y
    else
        y * 2
```

也可以通过 case 来判断：

```haskell
getLevel :: Int -> String 
getLevel n = case n of
    3 -> "Vital"
    2 -> "Medium"
    1 -> "Low"
    _ -> "Wrong"
```



# Enumerated Type

有点像定义一个枚举一样

```haskell
-- 定义性别 Enum，有 Male 和 Female，后面的 Show 相当于让它实现 toString 方法
data Gender = Male | Female deriving Show 

-- 判断是否为男性
isMale :: Gender -> Bool
isMale Male = True
isMale _    = False
```



# 定义类

Haskell 中定义一个类也是很简单的，首先定义其属性值：

```haskell
data Person = Person String Int  -- 可以看成一个构造函数
			deriving Show

-- 创建一个 Person 对象
Bob :: Person
Bob = Person "Bob" 23

-- 一个 getter，在 Haskell 里称作 projection
getAge :: Person -> Int
getAge (Person _ age) = age  -- 这里需要第几个属性就保留哪个即可

```

