# 1.字符串

旧版 .xls

> "Provider=Microsoft.Jet.OLEDB.4.0;" + "Data Source=" + fileName + ";" + ";Extended Properties=\'Excel 8.0;HDR=Yes;IMEX=1\'"

新版 .xlsx

> "Provider=Microsoft.ACE.OLEDB.12.0;" + "Data Source=" + fileName + ";" + ";Extended Properties=\'Excel 12.0;HDR=Yes;IMEX=1\'"

# 2.

```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Data.OleDb;
using System.Data;

namespace Exsel操作
{
    class Program
    {
        static void Main(string[] args)
        {
            string fileName = "test2.xls";
            string connectionStr = "Provider=Microsoft.Jet.OLEDB.4.0;" + "Data Source=" + fileName + ";" + ";Extended Properties=\'Excel 8.0;HDR=Yes;IMEX=1\'";
            //1.创建连接到数据源的对象
            OleDbConnection connection = new OleDbConnection(connectionStr);
            //2.打开连接
            connection.Open();


            string sql = "select * from [Sheet1$]"; //这是一个查询命令
            //3.创建一个连接器
            OleDbDataAdapter adapter = new OleDbDataAdapter(sql, connection);

            DataSet dataSet = new DataSet();//用来存放数据 用来存放DataTable

            adapter.Fill(dataSet); //把查询的结果DataTable放到(填充到)dataSet里面

            connection.Close();


            //4.取得数据
            DataTableCollection tableCollection = dataSet.Tables; //获得当前集合中所有表格

            DataTable table = tableCollection[0]; //因为我们只在dataSet中放置了一张表格,所以这里取得索引为0的就是我们刚刚查到的表格;

            //取得表格中的数据
            //取得table中的所有行
            DataRowCollection rowCollection = table.Rows;


            foreach (DataRow row in rowCollection)
            {
                //取得row中前4列
                for (int i = 0; i < 4; i++)
                {
                    Console.Write(row[i]+" ");
                }
                Console.WriteLine();
            }




        }
    }
}

```

