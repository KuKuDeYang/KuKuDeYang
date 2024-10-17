---
title: ado.net操作mysql数据库
tags: [ado.net,c#,mysql]
copyright: true
typora-root-url: ado-net操作mysql数据库
date: 2023-02-24 09:11:59
categories: 技术连连看	
description: 使用ado.net对mysql数据库的crud
cover: cover.png
---

###  **首先是流程图**

<img src="流程图.png" style="zoom:50%;" />

### 准备工作

#### **引用（有用的没有的都放这了）**

~~~c#
using System;
using System.Reflection;
using System.IO;
using Microsoft.Office.Interop.Excel;
using MySqlConnector;
using System.Threading;
using System.Linq;
using System.Data;
using SData = System.Data;
using System.Text;
using System.Collections.Generic;
using NPOI.SS.UserModel;
using NPOI.XSSF.UserModel;
using NPOI.HSSF.UserModel;
using CsvHelper;
using System.Globalization;
~~~



#### **全局变量一览**

~~~c#
static string connectionString = "server=127.0.0.1;port=3306;user=root;password=root; database=test;charset=utf8;SslMode=none";
DataTable excel = ExcelUtil.ExcelToTable("C:\\Users\\baizh\\Desktop\\test_user.xlsx");
~~~

#### **表格数据一览**

![excel表格内容](表格数据.png)

### 不使用适配器

~~~c#
using (MySqlConnection myconn = new MySqlConnection(connectionString))
{
    string user_name = "杨代码";
    string sqlSelect = "select user_price from test_user where user_name=@user_name";
    string sqlDelete = "delete from test_user where user_name=@user_name";
    string sqlUpdate = "update test_user set user_price = 114514 where user_name=@user_name";
    string sqlInsert = "insert into test_user(user_name,user_price,user_date) values ('杨代码',110110,'2023-02-02')";
    MySqlCommand command = new MySqlCommand(sqlSelect, myconn);
    command.Parameters.AddWithValue("@user_name", user_name);

    myconn.Open();

    var res = command.ExecuteScalar();
    //var res = command.ExecuteNonQuery();

    Console.WriteLine(res);
}
~~~

关于ExecuteScalar和ExecuteNonQuery和ExecuteReader的用法

- ExecuteScalar 方法执行一个SQL命令返回结果集的第一列的第一行。它经常用来执行SQL的COUNT、AVG、MIN、MAX 和 SUM 函数，这些函数都是返回单行单列的结果集，用于增删改时返回值没有意义。

- ExecuteNonQuery方法用来执行INSERT、UPDATE、DELETE和其他没有返回值得SQL命令，用于增删改时返回值为更改的行数，用于查询时返回值为-1。
- ExecuteReader 返回一个DataReader对象,可以调用DataReader的方法和属性迭代处理结果集。它是一个快速枚举数据库查询结果的机制，是只读、只进的。对SqlDataReader.Read的每次调用都会从结果集中返回一行。

使用DataReader读取数据

~~~c#
using (MySqlConnection myConn = new MySqlConnection(connectionString))
{
    string sql = "select * from test_user";
    MySqlCommand command = new MySqlCommand(sql, myConn);

    myConn.Open();

    MySqlDataReader reader = command.ExecuteReader();
    if (reader.HasRows)
    {
        while (reader.Read())
        {
            for (int i = 0; i < reader.FieldCount; i++)
            {
                Console.WriteLine(reader[i] + "   ");
            }
            Console.WriteLine();
        }
        //使程序等待按键，并且在用户按键之前阻止屏幕，将用户按下的键输出在控制台;
        Console.ReadKey();
    }
}
~~~

输出结果为数据库中的数据按行显示。

### 使用适配器

#### 按行操作（数据不可重复，否则会覆盖）

~~~c#
foreach(DataRow excelRow in excel.Rows)
{
    using (MySqlConnection myConn = new MySqlConnection(connectionString))
    {
        string sql = "select * from test_user where user_name=@user_name";
        MySqlCommand command = new MySqlCommand(sql,myConn);
        command.Parameters.AddWithValue("@user_name",excelRow["姓名"].ToString());
        MySqlDataAdapter adapter = new MySqlDataAdapter(command);
        MySqlCommandBuilder cb = new MySqlCommandBuilder(adapter);

        myConn.Open();

        DataTable dt = new DataTable();
        adapter.Fill(dt);

        DataRow dr = null;
        if(dt.Rows.Count == 0)
        {
            //新增
            dr = dt.NewRow();
            dr["user_name"] = excelRow["姓名"].ToString();
            dt.Rows.Add(dr);
        }
        else
        {
            //更新
            dr = dt.Rows[0];
        }
        dr["user_price"] = Convert.ToDecimal( excelRow["金额"].ToString() );
        Console.WriteLine(excelRow["时间"]);
        dr["user_date"] = DateTime.ParseExact( excelRow["时间"].ToString(),"yyyy-MM-dd",null );

        adapter.Update(dt);
    }
}
~~~

#### 按整体操作（数据可重复）

~~~c#
HashSet<string> names = new HashSet<string>();
foreach(.DataRow dr in excel.Rows)
{
    names.Add(dr["姓名"].ToString());
}
foreach(string user_name in names)
{
    using(MySqlConnection myConn = new MySqlConnection(connectionString))
    {
        string sql = "select * from test_user where user_name=@user_name";
        MySqlCommand command = new MySqlCommand(sql, myConn);
        command.Parameters.AddWithValue("@user_name", user_name);
        MySqlDataAdapter adapter = new MySqlDataAdapter(command);
        MySqlCommandBuilder cb = new MySqlCommandBuilder(adapter);

        myConn.Open();

        DataTable dt = new DataTable();
        adapter.Fill(dt);
        DataTable excelDt = find(user_name);

        //更新
        foreach(DataRow dr in dt.Rows)
        {
            foreach(DataRow excelDr in excelDt.Rows)
            {
                if(excelDr.RowState == DataRowState.Deleted)
                {
                    continue;
                }
                dr["user_price"] = Convert.ToDecimal(excelDr["金额"].ToString());
                dr["user_date"] = DateTime.ParseExact(excelDr["时间"].ToString(), "yyyy-MM-dd", null);
                excelDr.Delete();
                break;
            }
        }
        //新增
        foreach(DataRow excelDr in excelDt.Rows)
        {
            if (excelDr.RowState == DataRowState.Deleted)
            {
                continue;
            }
            DataRow newRow = dt.NewRow();
            newRow["user_name"] = excelDr["姓名"].ToString();
            newRow["user_price"] = Convert.ToDecimal(excelDr["金额"].ToString());
            newRow["user_date"] = DateTime.ParseExact(excelDr["时间"].ToString(), "yyyy-MM-dd", null);
            dt.Rows.Add(newRow);
        }
        adapter.Update(dt);
    }
}

DataTable find(string user_name)
{
    DataTable excelDt = excel.Clone();
    foreach (DataRow dr in excel.Rows)
    {
        if (dr["姓名"].ToString() == user_name)
        {
            excelDt.Rows.Add(dr.ItemArray);
        }
    }
    return excelDt;
}
~~~

### 读取csv格式文件，使用list修改数据库

~~~c#
string filePath = "C:\\Users\\baizh\\Desktop\\test_user.csv";
string[] user_names = { "杨怂蛋", "杨无语", "杨难受", "杨回忆" };
using (var reader = new StreamReader(filePath, Encoding.Default))
{
    using (var csv = new CsvReader(reader, CultureInfo.InvariantCulture))
    {
        var items = csv.GetRecords<Item>().ToList();

        foreach (string user_name in user_names)
        {
            using (MySqlConnection myconn = new MySqlConnection(connectionString))
            {
                string sql = "select * from test_user where user_name=@user_name";
                MySqlCommand command = new MySqlCommand(sql, myconn);
                command.Parameters.AddWithValue("@user_name", user_name);
                MySqlDataAdapter adapter = new MySqlDataAdapter(command);
                MySqlCommandBuilder cb = new MySqlCommandBuilder(adapter);

                myconn.Open();

                DataTable dt = new DataTable();
                adapter.Fill(dt);
                var res = (from item in items
                           where item.user_name == user_name
                           select item).ToList();

                //更新
                foreach (DataRow dr in dt.Rows)
                {
                    var item2 = res.FirstOrDefault(p => decimal.Compare(p.user_price, Convert.ToDecimal(dr["user_price"].ToString())) == 0);
                    if (item2 == null)
                    {
                        continue;
                    }
                    dr["user_price"] = item2.user_price;
                    dr["user_date"] = item2.user_date;
                    res.Remove(item2);
                }

                //新增
                foreach (Item item in res)
                {
                    DataRow newDr = dt.NewRow();
                    newDr["user_name"] = item.user_name;
                    newDr["user_price"] = item.user_price;
                    newDr["user_date"] = item.user_date;
                    dt.Rows.Add(newDr);
                }
                adapter.Update(dt);
            }
        }
    }
}
~~~

