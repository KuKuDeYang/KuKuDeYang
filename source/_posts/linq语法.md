---
title: linq语法
tags: [linq,c#,字典]
copyright: true
typora-root-url: linq语法
date: 2023-03-02 14:45:40
categories: 技术连连看	
description: linq基本语法
cover: cover.png
---

###  LINQ中的八大关键字

| **关键字** | **说明**                                                     |
| ---------- | ------------------------------------------------------------ |
| from       | 指定范围变量和数据源                                         |
| where      | 根据bool表达式从数据源中筛选数据                             |
| select     | 指定查询结果中的元素所具有的类型或表现形式                   |
| group      | 对查询结果按照键值进行分组(IGrouping<TKey,TElement>)         |
| into       | 提供一个标识符，它可以充当对join、group或select子句结果的引用 |
| orderby    | 对查询出的元素进行排序(ascending/descending)                 |
| join       | 按照两个指定匹配条件来Equals连接两个数据源（join on equals） |
| let        | 产生一个用于存储查询表达式中的子表达式查询结果的范围变量     |

### 准备工作

~~~c#
List<User> list1 = new List<User>()
            {
                new User("杨无敌",17){ },
                new User("杨无敌",18){ },
                new User("杨潇洒",18){ },
                new User("杨高尚",20){ },
            };
            List<User> list2 = new List<User>()
            {
                new User("杨无敌",19){ },
                new User("杨无敌",20){ },
                new User("杨潇洒",19){ },
                new User("杨倜傥",18){ },
            };
            List<User> list3 = new List<User>()
            {
                new User("杨无敌",19){ },
                new User("杨无敌",20){ },
                new User("杨无敌",21){ },
                new User("杨无敌",22){ },
                new User("杨无敌",20){ },
                new User("杨潇洒",19){ },
                new User("杨潇洒",19){ },
                new User("杨潇洒",19){ },
                new User("杨倜傥",18){ },
            };
~~~

### from用法

#### from简单用法

~~~c#
var res = from user in list1
           where user.name == "杨无敌"
           select user;

~~~

#### 复合from用法

~~~c#
//无条件判断时 结果数量为两表的笛卡尔积中对象的数量即 4*4=16 条数据
var res1 = from user1 in list1
            from user2 in list2 
            select user1;
foreach(var user in res1)
{
    Console.WriteLine($"{user.name}+{user.age}");
};

//  输出结果
/*
	杨无敌+17
    杨无敌+17
    杨无敌+17
    杨无敌+17
    杨无敌+18
    杨无敌+18
    杨无敌+18
    杨无敌+18
    杨潇洒+18
    杨潇洒+18
    杨潇洒+18
    杨潇洒+18
    杨高尚+20
    杨高尚+20
    杨高尚+20
    杨高尚+20
*/
~~~

### join用法

#### 简单join用法（内连接）

~~~c#
var res2 = (from user1 in list1
            join user2 in list2
            on user1.name equals user2.name
            select user1).ToList();
foreach (User user in res2)
{
    Console.WriteLine($"{user.name}+{user.age}");
}

//输出结果
//输出结果的数量为：符合条件的数量的乘积再相加，例如user1中杨无敌两条符合，user2中杨无敌两条符合，最终杨无敌数量为4条，
//                                            user1中杨潇洒一条符合，user2中杨潇洒一条符合，最终杨潇洒数量为1条，最终再把各个符合条件的加起来4+1=5条数据；
//输出结果的值：取决于select中的代码赋值     
/*
	杨无敌+17
    杨无敌+17
    杨无敌+18
    杨无敌+18
    杨潇洒+18
*/
~~~

#### join实现左外连接

~~~c#
//若要在 LINQ 中执行左外部联接，请结合使用 DefaultIfEmpty 方法与分组联接，指定要在某个左侧元素不具有匹配元素时生成的默认右侧元素。 
//可以使用 null 作为任何引用类型的默认值，也可以指定用户定义的默认类型。
var res22 = from user1 in list1
    join user2 in list2 on user1.name equals user2.name into users
    from user in users.DefaultIfEmpty()
    select new
{
    name = user1.name,
    age = user == null ? 0 : user1.age
};
foreach(var user in res22)
{
    Console.WriteLine($"{user.name}+{user.age}");
}

//输出结果
//结果相对于内连接 还多了左表中不符合条件的数据
/*
	杨无敌+17
    杨无敌+17
    杨无敌+18
    杨无敌+18
    杨潇洒+18
    杨高尚+0
*/
~~~

### let用法

~~~c#
//let子句用于在LINQ表达式中存储子表达式的计算结果。let子句创建一个范围变量来存储结果，
//变量被创建后，不能修改或把其他表达式的结果重新赋值给它。此范围变量可以再后续的LINQ子句中使用。
var res3 = (from user in list1
            let age = user.age
            let name = user.name
            where age > 17 && name.Equals( "杨无敌")
            select user).ToList();
foreach(User user in res3)
{
    Console.WriteLine($"{user.name}+{user.age}");
}

//输出结果
/*
	杨无敌+18
*/
~~~

### group by用法

#### 单列分组

~~~c#
var res4 = (from user in list2
            group user by user.name into g
            select new
            {
                name = g.Key,
                age = g.Sum(t => t.age)
            }).ToList();
foreach(var user in res4)
{
    Console.WriteLine($"{user.name}+{user.age}");
}
~~~



#### 多列分组

~~~c#
//如需按多列分组可使用new  参考  group t by new {t.MaterialID, t.ProductID} into grp
//into 用法  查询延续子句可以接受查询的一部分结构并赋予一个名字，从而可以在查询的另一部分中使用
//key 表示用来分组的列，这里表示name和age两列组成的集合
//group by where 的使用顺序是    where->groupby
var res5 = (from user in list3
            where user.age <= 20
            group user by new { user.name, user.age } into grouptmp
            select new
            {
                key = grouptmp.Key,
                name = grouptmp.Key.name,
                age = grouptmp.Sum(t => t.age)
            }).ToList();

foreach (var user in res5)
{
    Console.WriteLine($"{user.key}+{user.name}+{user.age}");
}

//输出结果
/*
	{ name = 杨无敌, age = 19 }+杨无敌+19
    { name = 杨无敌, age = 20 }+杨无敌+40
    { name = 杨潇洒, age = 19 }+杨潇洒+57
    { name = 杨倜傥, age = 18 }+杨倜傥+18
*/
~~~

### orderby用法

~~~c#
//与where关键字不分前后 默认是ascending，可以设置倒序 descending
var res6 = (from user in list2
            orderby user.age descending
            select user).ToList();
foreach(var user in res6)
{
    Console.WriteLine($"{user.name}+{user.age}");
}

//输出结果
/*
	杨无敌+20
    杨无敌+19
    杨潇洒+19
    杨倜傥+18
*/
~~~

### select用法

#### 简单用法

~~~c#
var res7 = from user in list1
    select user;
~~~



#### 匿名类型形式

~~~c#
var res8 = from user in list1
    select new
{
    name = user.name,
    age = user.age
};
~~~

***

THE END
