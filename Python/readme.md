# Python-base

- Data Type
    - 十六进制用0x前缀和0-9，a-f表示，例如：0xff00，0xa5b4c3d2
    - Python允许在数字中间以_分隔，因此，写成10_000_000_000和10000000000是完全一样的。
    - 科学计数法:12300就是1.23e4
    - 字符串是以单引号'或双引号"括起来的任意文本
        - 内部既包含 ' 又包含 " 怎么办？可以在引号前面加转义字符 \ 来标识: 'I\'m \"OK\"!'
        - \n表示换行，\t表示制表符。字符\本身也要转义，所以\\表示的字符就是\。
        - Python还允许用r''表示''内部的字符串默认不转义
        ```
            >>> print('\\\t\\')
            \       \
            >>> print(r'\\\t\\')
            \\\t\\
        ```
    - 布尔值
        - 一个布尔值只有True、False两种值
        - 布尔值可以用and、or和not运算。
    - 空值
        - 空值是Python里一个特殊的值，用None表示。None不能理解为0
    - 常量
        - 在Python中，通常用全部大写的变量名表示常量:  PI = 3.14159265359
    - 除法
        - / 除法计算结果是浮点数。 
        - // 除法只取结果的整数部分。
        - % 除法只取结果得余数部分。

- 字符串和编码
    - 最早只有127个字符被编码到计算机里，也就是大小写英文字母、数字和一些符号，这个编码表被称为ASCII编码.
    - 中国制定了GB2312编码. 中文需要两个字节
    - Unicode把所有语言都统一到一套编码里.
        - 汉字 '中' 用Unicode编码是十进制的20013，二进制的01001110 00101101。
        - A的Unicode编码是00000000 01000001。
    - 本着节约的精神，又出现了把Unicode编码转化为“可变长编码”的UTF-8编码。
        - UTF-8编码把一个Unicode字符根据不同的数字大小编码成1-6个字节，
        - 常用的英文字母被编码成1个字节，汉字通常是3个字节，只有很生僻的字符才会被编码成4-6个字节。
    - Python提供了ord()函数获取字符的整数表示，chr()函数把编码转换为对应的字符：
        ``` 
        >>> ord('A')
            65
        >>> ord('中')
            20013
        >>> chr(66)
            'B'
        ```
    - 计算str包含多少个字符，可以用len()函数: >>> len('ABC')   = 3
        - 如果换成bytes，len()函数就计算字节数：>>> len('中文'.encode('utf-8')) 6
        - 1个中文字符经过UTF-8编码后通常会占用3个字节，而1个英文字符只占用1个字节。
    - 由于Python源代码也是一个文本文件，所以，当你的源代码中包含中文的时候，在保存源代码时，就需要务必指定保存为UTF-8编码。
        ```
        #!/usr/bin/env python3
        # -*- coding: utf-8 -*-
        ```
        - 第一行注释是为了告诉Linux/OS X系统，这是一个Python可执行程序，Windows系统会忽略这个注释；
        - 第二行注释是为了告诉Python解释器，按照UTF-8编码读取源代码，否则，你在源代码中写的中文输出可能会有乱码。
    - 格式化:
        - **%运算符**
            ```
            >>> 'Hi, %s, you have $%d.' % ('Michael', 1000000)
                'Hi, Michael, you have $1000000.'
            
               占位符       替换内容
                %d	    |    整数
                %f	    |    浮点数
                %s	    |    字符串
                %x	    | 十六进制整数
          
            ```
            - 用%%来表示一个%
        - **format**()
            - 它会用传入的参数依次替换字符串内的占位符{0}、{1}
            ```
            >>> 'Hello, {0}, 成绩提升了 {1:.1f}%'.format('小明', 17.125)
                'Hello, 小明, 成绩提升了 17.1%'
            ```
        - **f-string**
            - 以f开头的字符串，称之为f-string.字符串如果包含{xxx}，就会以对应的变量替换
                ``` 
                    >>> r = 2.5
                    >>> s = 3.14 * r ** 2
                    >>> print(f'The area of a circle with radius {r} is {s:.2f}')
                     The area of a circle with radius 2.5 is 19.62
                ```
                - {r}被变量r的值替换，{s:.2f}被变量s的值替换，并且:后面的.2f指定了格式化参数（即保留两位小数）
- list和tuple
    - **list** : 是一种有序的集合，可以随时添加和删除其中的元素。
        ``` 
            >>> classmates = ['Mike', 'Bob', 'Tom']
            >>> classmates
                ['Mike', 'Bob', 'Tom']
        ```
        - 用len()函数可以获得list元素的个数:`len(classmates) = 3`
        - 用索引来访问list中每一个位置的元素，索引从0开始：`classmates[0] = 'Mike'`
        - 可以往list中追加元素到末尾,**append**()：
            ```
               classmates.append('Adam')
               classmates = ['Mike', 'Bob', 'Tom', 'Adam']
            ```
        - 元素插入到指定的位置, **insert**():
            ``` >>> classmates.insert(1, 'Jack')
                >>> classmates
                    ['Mike', 'Jack', 'Bob', 'Tom', 'Adam']
            ```
        - 删除list指定位置的元素，用**pop**()
            ```
                classmates.pop(1)
                classmates = ['Mike', 'Bob', 'Tom', 'Adam']
            ```
        - 空的list，它的长度为0：`>>> L = [] >>> len(L) = 0`
    - **tuple**
        - 元组：tuple。tuple一旦初始化就不能修改
            - 因为tuple不可变，所以代码更安全。如果可能，能用tuple代替list就尽量用tuple。
            - 只有1个元素的tuple定义时必须加一个逗号,，来消除歧义：`t = (1,)`
        - “可变的”tuple：
            ```
                >>> t = ('a', 'b', ['A', 'B'])
                >>> t[2][0] = 'X'
                >>> t[2][1] = 'Y'
                >>> t
                    ('a', 'b', ['X', 'Y'])
            ```
            - 表面上看，tuple的元素确实变了，但其实变的不是tuple的元素，而是list的元素.
- 条件判断
    - **if()**
        ```
            if <条件判断1>:
                <执行1>
            elif <条件判断2>:
                <执行2>
            elif <条件判断3>:
                <执行3>
            else:
                <执行4>
        --------------------------
            age = 3
            if age >= 18:
                print('adult')
            elif age >= 6:
                print('teenager')
            else:
                print('kid')
        ```
        - 不要少写了冒号`:`。
- 循环  
    - **for..in..循环**
        ``` 
            numbers = [1, 2, 3]
            for number in numbers:
                print(number)
            ------------------------
            for i in range(0,5):    
                print(i)            # 0,1,2,3,4
        ```
        
    - **While循环**
        ``` 
            n=0
            while n < 100:
                if n == 10:
                    continue    # continue语句会直接继续下一轮循环，后续的print()语句不会执行
                if n == 20:
                    break       # break语句会结束当前循环
                print(n)
                n += 1
        ```
- dict和set
    - **dict**
        - Python内置了字典：dict的支持
            ``` 
                >>> d = {'Michael': 95, 'Bob': 75, 'Tracy': 85}
                >>> d['Michael']
                    95
            ```
        - 一个key只能对应一个value. `赋值： d['Tom'] = 90`
        - 判断key是否存在：
            - **in** 
                `'Bob' in d  -> True`
            - **get**
                `d.get('jerry')  -> None`
        - 删除一个key，用pop(key)方法，对应的value也会从dict中删除.
            ``` 
            >>> d.pop('Bob')
                    75
            >>> d
                {'Michael': 95, 'Tracy': 85} 
            ```
        - dict有以下几个特点：
            - 查找和插入的速度极快，不会随着key的增加而变慢；
            - 需要占用大量的内存，内存浪费多。
        - 而list相反：
            - 查找和插入的时间随着元素的增加而增加；
            - 占用空间小，浪费内存很少。
    - **set**
        - set和dict类似，也是一组key的集合，但不存储value。由于key不能重复，所以，在set中，没有重复的key。
        - 创建一个set，需要提供一个list作为输入集合：
        ```
        >>> s = set([1, 2, 3])
        >>> s
            {1, 2, 3}
        ```
            - 显示的顺序也不表示set是有序的！
        - 重复元素在set中自动被过滤：
            ``` 
            >>> s = set([1, 1, 2, 2, 3, 3])
            >>> s
                {1, 2, 3} 
            ```
        - add(key)方法可以添加元素到set中，可以重复添加，但不会有效果： 
            ``` 
            >>> s.add(2)
            >>> s
                {1, 2, 3} 
            ```
        - 通过remove(key)方法可以删除元素：
            ``` 
            >>> s.remove(2)
            >>> s
                {1, 3} 
            ```
        - 两个set可以做数学意义上的交集、并集等操作：`s1 & s2, s1 | s2`

# 函数

- 调用函数
    - 调用一个函数，需要知道函数的名称和参数
        - 求绝对值的函数abs，只有一个参数。`abs(-20) = 20`
        - 函数max()可以接收任意多个参数，并返回最大的那个。 `max(1,2,3) = 3`
    - 数据类型转换
        - Python内置的常用函数还包括数据类型转换函数
        - int()函数可以把其他数据类型转换为整数。    `int(1.1) = 1`

- 定义函数
    - 在Python中，定义一个函数要使用**def**语句
        - 依次写出函数名、括号、括号中的参数和冒号:
        - 在缩进块中编写函数体，
        - 函数的返回值用return语句返回.
            ```
            def my_abs(x):
                if x >= 0:
                    return x
                else:
                    return -x
            ```
    - 导入函数
        - 把my_abs()的函数定义保存为abstest.py文件
        - 用from abstest import my_abs来导入my_abs()函数
            ` from abstest import my_abs `
            - 注意abstest是文件名（不含.py扩展名）
    - 空函数
        - 定义一个什么事也不做的空函数，可以用pass语句：
        ```
            def nop():
                pass
        ```
- 函数的参数
    - 位置参数
        - 按照位置顺序依次赋的参数. `x, n为位置参数`
        ```
          def power(x, n):
                s = 1
                while n > 0:
                    n = n - 1
                    s = s * x
                return s
        ```
    - 默认参数
        - 提前为参数设置的默认值。 `def power(x, n=2):`
            - 调用时，可以不再为默认参数赋值.
            - **默认参数必须指向不变对象！**
    - 可变参数
        - 可变参数就是传入的参数个数是可变的
            ```
              def calc(*numbers):
                sum = 0
                for n in numbers:
                    sum = sum + n * n
                return sum 
              >>> calc(1, 2, 3)
                  14
            ```        
            - `*numbers`表示把`numbers`这个list的所有元素作为可变参数传进去。
    - 关键字参数
        - 关键字参数允许你传入0个或任意个含参数名的参数
            - 这些关键字参数在函数内部自动组装为一个**dict**
            ```
              def person(name, age, **kw):
                  print('name:', name, 'age:', age, 'other:', kw)
          --------------------------------------------------------------------
              >>> person('Adam', 45, gender='M', job='Engineer')
                  name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'}
            
              >>> extra = {'city': 'Beijing', 'job': 'Engineer'}
              >>> person('Jack', 24, **extra)
                  name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer'}
            ```
    - 命名关键字参数
        - 只接收city和job作为关键字参数.
            ```
            def person(name, age, *, city, job):
                print(name, age, city, job)
            ```
            - `*`后面的参数被视为命名关键字参数。
            - **命名关键字参数必须传入参数名**，这和位置参数不同。    
            - 命名关键字参数可以有缺省值: `def person(name, age, *, city='Beijing', job):`
    - 参数组合
        - 参数定义的顺序必须是：必选参数、默认参数、可变参数、命名关键字参数和关键字参数
            - ```
              def f2(a, b, c=0, *, d, **kw):
                  print('a =', a, 'b =', b, 'c =', c, 'd =', d, 'kw =', kw)
              ```
- 递归函数
    - 一个函数在内部调用自身本身，这个函数就是递归函数。
        - 例如阶乘：
            ```
              def fact(n):
                  if n==1:
                      return 1
                  return n * fact(n - 1)
          ---------------------------------
            ===> fact(5)
            ===> 5 * fact(4)
            ===> 5 * (4 * fact(3))
            ===> 5 * (4 * (3 * fact(2)))
            ===> 5 * (4 * (3 * (2 * fact(1))))
            ===> 5 * (4 * (3 * (2 * 1)))
            ===> 5 * (4 * (3 * 2))
            ===> 5 * (4 * 6)
            ===> 5 * 24
            ===> 120
            ```
        - 递归调用的次数过多，会导致栈溢出。            
