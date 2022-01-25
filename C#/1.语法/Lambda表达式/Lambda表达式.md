# Lambda表达式

## 1. Lambda表达式

- Lambda表达式就是匿名方法

  ```c#
  // => 读成goes to
  Action action = () => Console.WriteLine("无参数");
  Action<string> action = str => Console.WriteLine(str);
  Func<int, string> func = (num) => { return num.ToString(); };
  ```



## 2. LINQ

- **LINQ**: 语言集成查询 -- 用来做查询的一些操作类库
  1. LINQ to Object 负责对象的查询
  2. LINQ to XML 负责XML的查询
  3. LINQ to ADO.NET 负责数据库的查询

- 扩展方法的方式
  ```c#
  var list = StudentList.Where(s => s.Age <= 20);
  ```

- 关键词的方式
  ```c#
  var list = from s in StudentList where s.Age <= 20 select s;
  ```

- 自己编写LINQ
  ```c#
  public static class ExtendMethod
  {
      public static List<T> MyWhere<T>(this List<T> resource, Func<T, bool> func)
      {
          var list = new List<T>();
          foreach(var item in resource)
          {
              if(func.Invoke(item))
              {
                  list.Add(item);
              }
          }
          return list;
      }
  }
  
  var list = StudentList.MyWhere(s => s.Age <= 20);
  ```

