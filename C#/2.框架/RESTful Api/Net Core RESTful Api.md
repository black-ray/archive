# .Net Core RESTful Web Api



# 1. RESTful API基础

### 1.1. 特点

- ##### 无状态

  一次调用就会返回结果。不存在类似打开连接访问数据库然后关闭连接这种依赖于上次调用的情况。WebSoket这样的持续性连接有状态的连接就不属于RESTful。

- ##### 面向资源

   无论数据还是服务，在RESTful中一切都是资源。URL中只会出现名词，不会出现动词。

   正确：api/v1/touristRoutes                      错误：api/v1/GetTouristRoutes

- ##### 使用HTTP动词，表达数据操作

   | 动词    | 意义     | URL                                             | 请求主体                          | 相应主体                         |
   | ------- | -------- | ----------------------------------------------- | --------------------------------- | -------------------------------- |
   | GET     | 查看     | /api/touristRoutes<br />/api/touristRoutes/{id} | -                                 | 旅游路线列表<br />单一的旅游路线 |
   | POST    | 创建     | /api/touristRoutes                              | 单一的旅游路线                    | 单一的旅游路线                   |
   | PUT     | 更新     | /api/touristRoutes/{id}                         | 单一的旅游路线                    | 单一的旅游路线 或 空             |
   | PATCH   | 部分更新 | /api/touristRoutes/{id}                         | 旅游线路的<br />JsonPatchDocument | 单一的旅游路线 或 空             |
   | DELETE  | 删除     | /api/touristRoutes/{id}                         | -                                 | -                                |
   | HEAD    |          | /api/touristRoutes<br />/api/touristRoutes/{id} | -                                 | -                                |
   | OPTIONS |          |                                                 |                                   |                                  |

- ##### HATOAS 超媒体即应用状态引擎

- ##### 6个约束限制

1. Client-Server：前后端分离
2. 无状态：请求独立
3. 分层系统：代码分层
4. 统一接口：数据统一，API自我发现
5. 可缓存
6. 按需代码

### 1.2. API架构

<img src="Net Core RESTful Api.assets/14-1 【理解】分页与项目架构浅析.mp4_20210904_225550.654.jpg" style="zoom: 50%;" />



# 2. 配置文件

### 2.1. 服务依赖

- ##### Dependencies : 项目以及框架的服务依赖

  在框架下引用了AspNetCore.App 和 NETCore.App 两个框架

  NETCore.App是整个项目的基础框架，包含了对代码的运行，编译，部署的处理

  AspNetCore.App是基于基础框架引入的应用层框架，包含了应用层的服务。包括认证服务，授权服务，诊断服务，HTTP请求处理服务，文件访问，日志记入，依赖注入等等

  

### 2.2. 运行时设置

- ##### 运行时设置 appsettings.json

  ```json
  {
    // 日志设置
    "Logging": {
      "LogLevel": {
        "Default": "Information",
        "Microsoft": "Warning",
        "Microsoft.Hosting.Lifetime": "Information"
      }
    },
    // 托管服务器设置
    "AllowedHosts": "*"
  }
  ```

- ##### 开发环境运行时设置 appsettings.Development.json

  ```json
  {
    "Logging": {
      "LogLevel": {
        "Default": "Information",
        "Microsoft": "Warning",
        "Microsoft.Hosting.Lifetime": "Information"
      }
    }
  }	
  ```



### 2.3. 项目启动信息

- ##### Properties/launchSettings.json

  ```json
  {
    "$schema": "http://json.schemastore.org/launchsettings.json",
    "iisSettings": {
      "windowsAuthentication": false,
      "anonymousAuthentication": true,
      "iisExpress": {
        "applicationUrl": "http://localhost:30914",
        "sslPort": 44345
      }
    },
    "profiles": {
      // IIS 服务器设置
      "IIS Express": {
        "commandName": "IISExpress",
        "launchBrowser": true,
        "launchUrl": "swagger",
        "environmentVariables": {
          "ASPNETCORE_ENVIRONMENT": "Development"
        }
      },
      "RESTful.API": {
        // .net core KESTREL 服务器设置
        "commandName": "Project",
        "dotnetRunMessages": "true",
        "launchBrowser": true,
        "launchUrl": "swagger",
        "applicationUrl": "https://localhost:5001;http://localhost:5000",
        "environmentVariables": {
          "ASPNETCORE_ENVIRONMENT": "Development"
        }
      }
    }
  }
  ```

- ##### 项目服务器

  ISS服务器: 只能运行在Windows上 

  KESTREL服务器 （文件名一致）: 可以跨平台
  
  

# 3. 主函数

```c#
public class Program
{
	public static void Main(string[] args)
    {
        // 创建运行虚拟托管服务器
        CreateHostBuilder(args).Build().Run();
    }

    // 创建运行托管服务器
    public static IHostBuilder CreateHostBuilder(string[] args) =>
        // CreateDefaultBuilder执行过程：
        // 1.查看程序运行的环境 currentDirectory
        // 2.启用相应的配置文件 appsettings
        // 3.加载程序集 assembly 运行程序所有核心代码
        // 4.设置环境变量 日志 系统的反转控制 IOC容器
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                // asp.net core config信息 如注入依赖，配置中间件，处理请求通道，都会使用Startup文件来进行管理
                webBuilder.UseStartup<Startup>();
            });
}
```



# 4. 启动类

### 4.1. 概述

config信息 如注入依赖，配置中间件，处理请求通道，都会使用Startup文件来进行管理

```c#
public class Startup
{
    // appsettings 配置信息
    // using Microsoft.Extensions.Configuration;
    public IConfiguration Configuration { get; }
    
    public Startup(IConfiguration configuration)
    {
        Configuration = configuration;
    }
    
    // ConfigureServices: 注入各种服务组件的依赖
    // 将自己的服务注入到IOC容器中
    public void ConfigureServices(IServiceCollection services)
    {
        // 注册MVC Controller控制器组件
        services.AddControllers();
        
        // 安装Swagger NuGet Swashbuckle.AspNetCore
        services.AddSwaggerGen(c =>
            {
                c.SwaggerDoc("v1", new OpenApiInfo { Title = "RESTful.API", Version = "v1" });
            });
    }

    // Configure: 创立中间件MiddleWare，设置Http请求通道request pipeline
    // 请求通道通过IApplicationBuilder创建
    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
            app.UseSwagger();
            app.UseSwaggerUI(c => c.SwaggerEndpoint("/swagger/v1/swagger.json", "RestfulAgain v1"));
        }

        // 调用HTTPS重定向中间件, 这样就可以把所有的HTTP请求转换为HTTPS.
        app.UseHttpsRedirection();

        app.UseRouting();

        app.UseAuthorization();

        app.UseEndpoints(endpoints =>
        {   
            // MVC路由映射中间件 使用MVC的映射代替直接对路由进行映射
            endpoints.MapControllers();
        });
    }
}
```

### 4.2. 依赖注入ConfigureServices

#### 4.2.1. 概述

- ##### 功能

  注入各种服务组件的依赖。程序运行时调用，将我们自己的服务注入到IOC容器中

#### 4.2.2. 生命周期函数

AddSingleton、AddScoped、AddTransient 都可以依赖注入数据仓库，只是注入的时间和实例存在的时间有区别

```c#
public void ConfigureServices(IServiceCollection services)
{
    services.AddTransient<ITransientTestService, TransientTestService>();
	services.AddScoped<IAddScopedTestService, AddScopedTestService>();
	services.AddSingleton<IAddSingletonTestService, AddSingletonTestService>(); 
}
```

- ##### AddTransient

  瞬时

  会在每一次发起请求的时候创建一个全新的实例，而请求结束以后会自动注销这个实例

  优点：每一次请求都会初始化一个全新的实例，而不同的请求之间，实例中的数据是完全独立互不影响 

- ##### AddSingleton 

  单例

  系统的启动的时候有且仅创建一个实例，之后系统每次处理请求都会使用同一个实例

  优点：便于管理 内存占用少 效率高 

  缺点：每次请求共享了数据通道，造成数据污染

- ##### AddScoped

  作用域

  引入事务管理Transaction，将一系列的请求操作整合起来，放在一个事务之中

  这个事务有且仅创建一个实例，在事务结束以后，系统会自动销毁实例
  
  优点：介于AddTransient AddSingleton之间。

### 4.3. 请求管道Configure

#### 4.3.1. 概述

- ##### 功能

  创立中间件MiddleWare，设置Http请求通道request pipeline

- ##### 参数

  IApplicationBuilder 创建请求通道

  IWebHostEnvironment 托管服务器的环境变量
  
  环境变量: 开发环境Development 集成环境 Intergration 测试环境 Testing 预发布环境 Staging 生产环境 Production

#### 4.3.2. **请求通道和中间件** 

- ##### 请求通道request pipeline
  
  请求通道检查处理http请求，交由中间件处理。
  在请求传递的过程中，如检查用户是否登录，URL路径是否正确，访问出错的时候该如何抛出异常等。
  每一个中间件都接受上一个中间件的输出，并把自己的处理结果传递给下一个中间件。
  通过对中间件有顺序的组合排列，就形成了Http请求处理的管道


- ##### 中间件MiddleWare

  组装到应用程序的管道中，用来处理请求和相应的软件。

- ##### http请求传递

  每个中间件都可以截获修改传递请求对象

  在特定情况下某些中间件可以做短路处理，直接向前端输出请求对象 

  请求通道的设置是有顺序要求的

  ```c#
  // 短路处理例
  app.UseEndpoints(endpoints =>
  {
      // 短路处理例：MapGet映射，截获指定路径
      endpoints.MapGet("/test", async context =>
      {
          throw new Exception("test");
          //await context.Response.WriteAsync("Hello from test!");
       });
  
      endpoints.MapGet("/", async context =>
      {
          await context.Response.WriteAsync("Hello World!");
      });
  });
  ```

### 4.4 注册MVC框架依赖

- ##### Startup.cs

  ```c#
  public void ConfigureServices(IServiceCollection services)
  {
      // 控制器组件
      services.AddControllers();
  }
  
  public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
  {
      app.UseEndpoints(endpoints =>
      {   
          // MVC路由映射中间件 使用MVC的映射代替直接对路由进行映射
          endpoints.MapControllers();
      });
  }
  ```

### 4.5 跨域处理

- ##### 跨域

  跨域是指一个域下的文档或者脚本试图去请求访问另一个域下的资源。而我们一般说的跨域是指浏览器同源策略限制。

  当一个请求url的协议、域名、端口三者之间任意一个与当前页面url不同即为跨域

- ##### Startup.cs

  ```c#
  public void ConfigureServices(IServiceCollection services)
  {
      services.AddCors(options =>
  	{
          options.AddPolicy("cors", policy => 
  		{
              // 限制为指定的域才可访问 有多个可以用','隔开
              policy.WithOrigins("http://localhost:21632")
              // 允许任何的Header头部标题
              .AllowAnyHeader()
              // 允许跨域策略允许所有的方法：GET/POST/PUT/DELETE 等方法  
              // 如果进行限制需要 AllowAnyMethod("GET","POST") 这样来进行访问方法的限制
  			.AllowAnyMethod()
              // 设置凭据来源 不可与AllowAnyOrigin 一起同时使用
  			.AllowCredentials();
              //AllowAnyOrigin 允许任何来源，一般不会设定所有源都可以访问，而是只开放给对应的ip使用的。  
  		});
      });
  }
  
  public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
  {
      app.UseEndpoints(endpoints =>
      {   
          app.UseCors("cors");
      });
  }
  ```

  

#  5. 数据库

### 5.1. 数据模型和数据库

- ##### 数据模型和数据库的关系

  |             逻辑概念              |       物理概念       |
  | :-------------------------------: | :------------------: |
  |            Entity 实体            |       Table 表       |
  |          Attribute 属性           |   Column 字段，列    |
  | ER（Entity Relationship）实体关系 | Foreign Key 外键关系 |
  |          Model 数据模型           |        Schema        |

- ##### 数据模型的作用

  1. 映射数据库，对象化数据
  2. 获取数据，更新数据，传递数据，保存数据
  3. 处理业务逻辑，视为Business Layer业务层
  4. VIew Model与视图数据绑定

  一般创建一个Models的文件夹去存放与数据相关的代码

  ```c#
  public class User
  {
      // Entity 属性
      public int Id { get; set; }
      public string FirstName { get; set; }
      public string LastName { get; set; }
      // 不需要映射到数据库
      public string UserName { get { return FirstName + LastName; }; }
      public string ShortDescription { get; set; }
      public decimal Phone { get; set; }
      public string Email { get; set; }
      public bool IsEmailConfirm { get; set; }
      
      // 业务逻辑
      // 不会映射到数据库
      public void ConfirmEmail(string token)
      {
          if(isTokenValid)
          {
              this.IsEmailConfirm = true;
          }
      }
  }
  ```

### 5.2. EF Core概述

- ##### EF特点

  1. 跨平台
  2. 建模 。可以创建具有不同数据类型属性的实体数据模型，将会利用创建的模型，查询或保存底层数据
  3. 可以使用LINQ，从底层检索数据，也会跟踪提交到数据库的数据。可以异步对数据库提交保存
  4. 并发。 在默认情况下使用乐观锁，避免做的修改被其他用户覆盖
  5. 事务 。在查询或保存数据时，会自动执行事务管理
  6. 缓存。提供第一级的缓存，重复查询将从缓存中返回数据
  7. 数据迁移

- ##### EF的组件

  EDM实体数据模型：数据模型和数据模型对数据库的映射

  模型与数据库如何沟通映射：

  1. 通过LINQ和Entity SQL 将对象提供给Object Service（对象服务）
  2. 对象服务是访问数据库并返回数据的主要入口，它负责数据的实例化，把数据转化为实例对象传递给Data Provider（ 数据供应）
  3. 数据供应的主要责任是将LINQ或者Entity SQL转化为真正的SQL查询语言，然后使用ADO.Net（数据库通讯）与数据库进行通讯 


### 5.3.  数据库部署(EF Core)

- ##### Docker安装SQLServer

  ```bash
  docker pull mircosoft/mssql-server-linux
  # 接受最终用户许可协议，用户名SA和密码PaSSword12!，运行的端口1433是默认端口
  docker run -e "ACCECT_EULA=Y" -e "SA_PASSWORD=PaSSword12! -p 1433:1433 -d mircosoft/mssql-server-linuxdocker ps
  ```

- ##### 数据库映射

  Nuget：Microsoft.EntityFrameworkCore

  创建数据库映射工具：AppDbContext 上下文关系对象

  ```c#
  using Microsoft.EntityFrameworkCore;
  public class AppDbContext: DbContext{
      // 注入DbContextOptions实例
      public AppDbContext(DbContextOptions<AppDbContext> options) : base(options)
      {
      }
      // 模型映射到数据库 DbSet设置模型的映射
      // 每一个数据模型都要使用一个DbSet来映射给一张数据库的表
      public DbSet<TouristRoute> TouristRoutes { get; set; }
      public DbSet<TouristRoutePicture> TouristRoutePictures { get; set; }
  }
  ```

- ##### IOC容器中注册数据库上下文关系对象

  Nuget SqlServer的Data Provider拓展包：Microsoft.EntityFrameworkCore.SqlServer

  ```c#
  using Microsoft.EntityFrameworkCore;
  public void ConfigureServices(IServiceCollection services)
  {
      services.AddDbContext<AppDbContext>(options =>
  	{
  	// 调用MSSQL
          options.UseSqlServer("Server=localhost; Database=StudentDb; User Id=sa; Password=PaSSword12!;");         // 传入数据库连接字符串
          // options.UseSqlServer(Configuration["DbContext:ConnectionString"]);
          // 使用vs自带的数据库工具配置 Catalog里需要改成自己数据库的名称
          // options.UseSqlServer("Server=localhost; Database=Data Source=(localdb)\\MSSQLLocalDB;Initial Catalog=StudentDb;Integrated Security=True;Connect Timeout=30;Encrypt=False;TrustServerCertificate=False;ApplicationIntent=ReadWrite;MultiSubnetFailover=False");     
      });
  }
  ```

### 5.4. 模型验证

  添加数据库限制，主键信息，以及外键联系信息等

  ```c#
// 对数据模型做数据库限定
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;
public class TouristRoute {
    
    [Key]					// 数据库主键信息[key]
    public Guid Id { get; set; }
    [Required]				// 不能为空
    [MaxLength(100)]		// 长度限制
    public string Title { get; set; }
    [Required]
    [MaxLength(1500)]
    public string Description { get; set; }
    // 存到数据库里保留两位小数[Column]
    // Column内部加限定
    [Column(TypeName = "decimal(18, 2)")]
    public decimal OriginalPrice { get; set; }
    // 数据范围限制
    [Range(0.0, 1.0)]
    public double? DiscountPresent { get; set; }
    public DateTime CreateTime { get; set; }
    public DateTime? UpdateTime { get; set; }
    public DateTime? DepartureTime { get; set; }
    // 字符串最大长度限制
    [MaxLength]	
    public string Features { get; set; }
    [MaxLength]
    public string Fees { get; set; }
    [MaxLength]
    public string Notes { get; set; }
    public ICollection<TouristRoutePicture> TouristRoutePictures { get; set; } 	=
        new List<TouristRoutePicture>();
    public double? Rating { get; set; }
    public TravelDays? TravelDays { get; set; }
    public TripType? TripType { get; set; }
    public DepartureCity? DepartureCity { get; set; }
}

public class TouristRoutePicture {
    [Key]
    // 数据库自增类型
    [DatabaseGenerated(DatabaseGeneratedOption.Identity)]
    public int Id { get; set; }
    [MaxLength(100)]
    public string Url { get; set; }
    // EntityFramework在映射数据库的时候会自动把每个模型的主键以类名加上Id的形式作为外键关联
    // 如TouristRoute下面的主键Id，用在外链连系的时候就会被叫做TouristRouteId
    [ForeignKey("TouristRouteId")] 
    public Guid TouristRouteId { get; set; }
    // 外键关系对象
    public TouristRoute TouristRoute { get; set; }
}
  ```

### 5.5. 创建数据库

- ##### 数据库创建工具

  Nuget : Entity Framework Core Tools

- ##### 数据库创建模式

  1. DataBase First

     提前先建立好数据库，然后和数据库相连接

  2. Model First

     先创建好数据模型models，然后根据models自动生成数据库

  3. Code First

     不创建实体模型，而根据DatabaseContext数据库上下文关系对象，来创建数据库

- ##### 创建数据库流程（Mode First创建模式）

  1. build项目文件。只有预先完成项目的构建，EF core 才能从DatabaseContext中找到各自模型的类型，完成最后的数据库创建

  2. 创建数据库

     - 使用 visual studio 

       1. 在Package Manage Console(PMC)中输入 add-migration XXXXX（迁移的命名）

          项目中多了MIgration文件夹。日期接迁移名的文件中的Up函数是执行数据库变化操作。Down是删除数据库变化操作

       2. 在Package Manage Console(PMC)中输入 update-database 

          执行刚刚创建的数据库迁移代码

     - 使用dotnet命令行

       1. WINDOWS的场合全局安装ef工具 dotnet tool install --global dotnet-ef
       2. dotnet ef migration add XXXXX（迁移的命名）
       3. dotnet ef database update

- ##### 使用Fluent API创建（Code First）

  ```c#
   public class RoutineDbContext : DbContext
   {
       public RoutineDbContext(DbContextOptions<RoutineDbContext> options) : base(options){}
       public DbSet<Company> Companies { get; set; }
       public DbSet<Employee> Employees { get; set; }
       // 重写DbContext父类方法，用来控制数据库和模型映射
       protected override void OnModelCreating(ModelBuilder modelBuilder)
       {
  		modelBuilder.Entity<Company>().Property(x => x.Name).IsRequired().HasMaxLength(100);
          modelBuilder.Entity<Company>().Property(x => x.Country).HasMaxLength(50);
          modelBuilder.Entity<Company>().Property(x => x.Industry).HasMaxLength(50);
          modelBuilder.Entity<Company>().Property(x => x.Product).HasMaxLength(100);
          modelBuilder.Entity<Company>().Property(x => x.Introduction).HasMaxLength(500);
  
          modelBuilder.Entity<Employee>().Property(x => x.EmployeeNo).IsRequired().HasMaxLength(10);
          modelBuilder.Entity<Employee>().Property(x => x.FirstName).IsRequired().HasMaxLength(50);
          modelBuilder.Entity<Employee>().Property(x => x.LastName).IsRequired().HasMaxLength(50);
  
          modelBuilder.Entity<Employee>().HasOne(x => x.Company).WithMany(x => x.Employees).HasForeignKey(x => x.CompanyId).OnDelete(DeleteBehavior.Cascade);
       }
   }
  ```

- ##### 重写SaveChanges方法

  ```c#
  public class Student: BaseEntity
  {
      public int StudentID { get; set; }
      public string Stude+ntName { get; set; }
      public DateTime? DateOfBirth { get; set; }
      public decimal Height { get; set; }
      public float Weight { get; set; }
  }
  
  public class BaseEntity{ 
      public DateTime CreatedDate { get; set; }
      public DateTime UpdatedDate { get; set; }    
  }
  
  public class ApiDbContext : DbContext
  {
      public override int SaveChanges()
      {
     		var entries = ChangeTracker.Entries().Where(e => e.Entity is BaseEntity && (e.State == EntityState.Added || e.State == EntityState.Modified));
      	    
     		foreach (var entityEntry in entries)
     		{
      	   	((BaseEntity)entityEntry.Entity).UpdatedDate = DateTime.Now;
  	
      	   	if (entityEntry.State == EntityState.Added)
      	   	{
      	       	((BaseEntity)entityEntry.Entity).CreatedDate = DateTime.Now;
      	   	}
     		}
     		return base.SaveChanges();
  	}
  }
  ```

- ##### 添加种子数据

  - 手动添加种子数据

    ```c#
    public class AppDbContext : IdentityDbContext<ApplicationUser>
    {
        public AppDbContext(DbContextOptions<AppDbContext> options) : base(options){ }
        public DbSet<TouristRoute> TouristRoutes { get; set; }
        public DbSet<TouristRoutePicture> TouristRoutePictures { get; set; }
        // 重写DbContext父类方法，用来控制数据库和模型映射
        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            // 调用modelBuilder来手动构建数据模型和数据库表的关系
            // modelBuilder是在创建数据模型与数据表映射的时候，补充说明用的
            // 数据模型和数据库表的名字不一定要一致，可以使用modelBuilder变更成自己喜欢的名字，创建自定义的映射关系
            // 在定义数据模型中没有使用DataAnnotations来声明约束，也可以在modelBuilder里来添加数据库限制，主键外键关系
            // modelBuilder.Entity定义种子数据模型 =》HasData函数提供模型的数据支撑
            modelBuilder.Entity<TouristRoute>().HasData(new TouristRoute
    			{
    			Id = Guid.NewGuid(),
    			Title = "ceshititle",
    			Description = "shuoming",
    			OriginalPrice = 0,
    			CreateTime = DateTime.UtcNow
    			});
            base.OnModelCreating(modelBuilder);
        }
    }
    ```

  - 使用json文件添加种子数据

    ```c#
    using System.IO;
    using System.Reflection;
    using Newtonsoft.Json;
    
    public class AppDbContext : IdentityDbContext<ApplicationUser>
    {
    	public AppDbContext(DbContextOptions<AppDbContext> options) : base(options){ }
        
        public DbSet<TouristRoute> TouristRoutes { get; set; }
        public DbSet<TouristRoutePicture> TouristRoutePictures { get; set; }
        
        // 重写DbContext父类方法，用来控制数据库和模型映射
        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            // 当前项目的文件夹地址，使用C#的反射机制
            // Path.GetDirectoryName(Assembly.GetExecutingAssembly().Location 获取项目程序集地址
            var touristRouteJsonData = File.ReadAllText(Path.GetDirectoryName(Assembly.GetExecutingAssembly().Location) + @"/Database/touristRoutesMockData.json");
            
            // 数据从字符串转化为json对象，最后转化为数据模型
            // Newtonsoft.json的JsonConvert进行数据转换
            // JsonConvert.DeserializeObject<反序列化以后的类型>(需要被反序列化的数据)
            IList<TouristRoute> touristRoutes = JsonConvert.DeserializeObject<IList<TouristRoute>>(touristRouteJsonData);
            
            modelBuilder.Entity<TouristRoute>().HasData(touristRoutes);
            
            base.OnModelCreating(modelBuilder);
        }
    }
    ```

  添加种子数据的流程就是做一次新的数据迁移。需要添加迁移，更新



### 5.6. 用户数据库

#### 5.6.1. 添加用户数据库

- ##### 添加默认用户模型

  - Nuget Microsoft.AspNetCore.Identity.EntityFrameworkCore

  - 注册身份认证框架的服务依赖

    Startup.cs

    ```c#
    public void ConfigureServices(IServiceCollection services){
    	// 注册身份认证框架的服务依赖 泛型参数1：用户数据类型 参数2：用户角色的数据模型
    	services.AddIdentity<IdentityUser, IdentityRole>()
    	// 连接上下文关系对象
    	.AddEntityFrameworkStores<AppDbContext>();
    }
    ```

  - 添加上下文关系对象 继承IdentityDbContext

    ```c#
    // IdentityUser 默认的身份认证数据结构。
    // IdentityUser 定义了用户的基本信息，会被映射到数据库中，会自动添加与数据库用户表相对应的映射关系。如果数据库表还不存在，
    // IdentityDbContext会帮我们更新数据库，自动添加用户表
    // 如果默认的IdentityUser不能满足需求，也可以继承IdentityUser来重载修改用户的数据模型
    public class AppDbContext : IdentityDbContext<IdentityUser> {}
    ```

  - 数据迁移。添加用户表

- ##### 定制用户模型

  ApplicationUser.cs

  ```c#
  // 继承IdentityUser 用户模型
  public class ApplicationUser : IdentityUser
  {
      public string Address { get; set; }
      public ShoppingCart ShoppingCart { get; set; }
      public ICollection<Order> Orders { get; set; }
      // 继承IdentityUser 用户模型 需要绑定 AspNetUserRoles， AspNetUserClaims， AspNetUserLogins， AspNetUserTokens 的对应关系
      // 使用virtual关键词 重载四张表
      // 因为是重新绑定父类的成员关系，所以必须保证和父类的成员变量名称一致
      // AspNetUserRoles 保存用户角色
      // 和用户表多对多的关系，一个用户可以有多个角色，一个角色也可以分配给多个用户
      // 系统真正的用户角色数据保存在AspNetRoles表中
      // AspNetRoleClaims 是使用基于Claim的方法，来管理角色的权限
      public virtual ICollection<IdentityUserRole<string>> UserRoles { get; set; } 
      // AspNetUserClaims 保存直接与用户绑定的相关权限声明
      public virtual ICollection<IdentityUserClaim<string>> Claims { get; set; }
      // AspNetUserLogins 处理第三方登录
      public virtual ICollection<IdentityUserLogin<string>> Logins { get; set; }
      // AspNetUserTokens 用于记录用户Session的
      public virtual ICollection<IdentityUserToken<string>> Tokens { get; set; }
  }
  ```

  AppDbContext.cs

  ```c#
  public class AppDbContext : IdentityDbContext<ApplicationUser> {}
  ```

  Startup.cs

  ```c#
  public void ConfigureServices(IServiceCollection services)
  {
      services.AddIdentity<ApplicationUser, IdentityRole>().AddEntityFrameworkStores<AppDbContext>();
  }
  ```

- ##### 添加用户种子数据

  AppDbContext.cs

  ```c#
  protected override void OnModelCreating(ModelBuilder modelBuilder){
      // .................
      // 初始化用户与角色的种子数据
      // 1.更新用户与角色的外键
      modelBuilder.Entity<ApplicationUser>(u =>
  		// 一对多的关系
  		u.HasMany(x => x.UserRoles)
  		// 每一个UserRoles都可以映射成一个外键关系
  		.WithOne().HasForeignKey(ur => ur.UserId).IsRequired()
  	);
      // 2.添加管理员角色
      var adminRoleId = "308660dc-ae51-480f-824d-7dca6714c3e2";
      modelBuilder.Entity<IdentityRole>().HasData(
          new IdentityRole()
          {
              Id = adminRoleId,
              Name = "Admin",
              NormalizedName = "Admin".ToUpper()
          }
      );
      // 3.添加用户
      var adminUserId = "90184155-dee0-40c9-bb1e-b5ed07afc04e";
      ApplicationUser adminUser = new ApplicationUser
      {
          Id = adminUserId,
          UserName = "admin@fakexicheng.com",
          NormalizedUserName = "admin@fakexicheng.com".ToUpper(),
          Email = "admin@fakexicheng.com",
          NormalizedEmail = "admin@fakexicheng.com".ToUpper(),
          TwoFactorEnabled = false,
          EmailConfirmed = true,
          PhoneNumber = "123456789",
          PhoneNumberConfirmed = false
      };
      // 创建hash工具
      var ph = new PasswordHasher<ApplicationUser>();
      adminUser.PasswordHash = ph.HashPassword(adminUser, "Fake123$");
      modelBuilder.Entity<ApplicationUser>().HasData(adminUser);
      // 4.给用户加入管理员角色
      modelBuilder.Entity<IdentityUserRole<string>>().HasData(
          new IdentityUserRole<string>()
          {
              RoleId = adminRoleId,
              UserId = adminUserId
          }
      );
      // 调用modelBuilder来手动构建数据模型和数据库表的关系
      base.OnModelCreating(modelBuilder);
  }
  ```

#### 5.6.2. 用户注册

- ##### 用户注册

  ```c#
  public class RegisterDto
  {
      [Required]
      public string Email { get; set; }
      [Required]
      public string Password { get; set; }
      [Required]
      [Compare(nameof(Password), ErrorMessage = "密码输入不一致")]
      public string ConfirmPassword { get; set; }
  }
  
  [ApiController]
  [Route("auth")]
  public class AuthenticateController : ControllerBase
  {
      private readonly IConfiguration _configuration;
      private readonly UserManager<ApplicationUser> _userManager;
      
      public AuthenticateController(IConfiguration configuration, UserManager<ApplicationUser> userManager)
      {
          _configuration = configuration;
          _userManager = userManager;
      }
      
      [AllowAnonymous]
      [HttpPost("register")]
      public async Task<IActionResult> Register([FromBody] RegisterDto registerDto)
      {
          // 1 使用用户名创建用户对象
          var user = new ApplicationUser()
          {
              UserName = registerDto.Email,
              Email = registerDto.Email
          };
          // 2 hash密码，保存用户
          // Create函数，第一个参数：user对象， 第二个参数：需要hash的密码
          var result = await _userManager.CreateAsync(user, registerDto.Password);
          if(!result.Succeeded)
          {
              return BadRequest();
          }
          // 4 return
          return Ok();
      }
  }
  ```








### 5.7. 购物车资源

- **为什么使用LineItem**

  当我们添加产品到购物车中，当用户购买的时候，订单模型中就会创建一个新订单，购物车的产品也就转移到订单中。

  商品已经卖出去了，这时商家修改了商品的价格怎么办。这时商品的历史价格也会发生调整。

  这时候就需要LineItem去处理购物车订单商品的联动关系了

  CartLineItem 表示购物车中包含的某一项产品的数据。

  加入LineItem后处理的流程：

  之前只有购物车，商品，订单三个模块，新加一个价格模块。产品不要直接添加进购物车，而是先进去价格模块处理。

  在价格模块中新创建一个LineItem对象，然后把商品的信息复制进去。

  于是当添加进购物车的时候，添加的不再是商品模块而是LineItem。下单所转移的也是LineItem而不是商品。

  ```c#
  public class ShoppingCart
  {
      [Key]
      public Guid Id { get; set; }
      public string UserId { get; set; }
      public ApplicationUser User { get; set; }
      public ICollection<LineItem> ShoppingCartItems { get; set; }
  }
  public class LineItem
  {
      [Key]
      [DatabaseGenerated(DatabaseGeneratedOption.Identity)]
      public int Id { get; set; }
      [ForeignKey("TouristRouteId")]
      public Guid TouristRouteId { get; set; }
      // TouristRoute是由外键TouristRouteId引用进来的
      public TouristRoute TouristRoute { get; set; }
      public Guid? ShoppingCartId { get; set; }
      //public Guid? OrderId { get; set; }
      [Column(TypeName = "decimal(18, 2)")]
      public decimal OriginalPrice { get; set; }
      [Range(0.0, 1.0)]
      public double? DiscountPresent { get; set; }
  }
  ```

- **购物车初始化**

  需要在用户创建的时候，初始化购物车数据



### 5.8. LINQ函数和EF函数

- ##### Queryable 延迟执行

  IQueryable是C#中linq to SQL语句的返回类型，简单来说是可以叠加运行Linq语句，最后统一访问数据库，而这个处理过程叫做延迟执行

  Linq表达式创建的IQueryable变量没有真正的执行数据库的操作，Linq表达式返回的IQueryable结果并不是查询结果，而是延迟执行的表达式

  直到最后一步使用聚合操作以后，终结IQueryable，才会真正执行数据库的操作

  聚合操作： .Tolist() .SingleOrDefult() .FirstOrDefault() .Count()

   延迟执行的目的：为后续动态表达提供可能。减少数据库的执行次数。

  ```c#
  public IEnumerable<TouristRoute> GetTouristRoutes(string keyword)
  {
      //创建的IQueryable变量还没有执行数据库的操作
      IQueryable<TouristRoute> result = _context.TouristRoutes.Include(t => t.touristRoutePictures);
      if(!string.isNullOrWhiteSpace(keyword))
      {
          keyword = keyword.Trim();
          result.Where(t => t.Title.Contains(keyword));
      }
      // ToList 会进行数据库访问
      return result.ToList();
  }
  ```

- ##### FirstOrDefalut

  取序列中满足条件的第一个元素，如果没有元素满足条件，则返回默认值

  ```c#
  public TouristRoute GetTouristRoute(Guid touristRouteId)
  {
      return _context.TouristRoutes.FirstOrDefault(n => n.Id == touristRouteId);
  }
  ```

- ##### Any/AnyAsync

  判断数据是否存在

  ```c#
  public bool TouristRouteExists(Guid touristRouteId)
  {
      return _context.TouristRoutes.Any(t => t.Id == touristRouteId);
  }
  ```

- ##### Include

  EF中连接两个表的方法，表示两张表通过外键进行连接

  ```c#
  public IEnumerable<TouristRoute> GetTouristRoutes()
  {
      return _context.TouristRoutes.Include(t => t.touristRoutePictures);
  }
  ```

  include vs join :

  ​	join 是不通过外键， 手动指定表连接的属性

  ​	使用include和join可以实现数据的立即加载 Eager Load

  ​	延时加载 ：Lazy Load 不使用include和join进行表连接

- ##### ThenInclude

  根据刚刚连接的表的数据，指定要进一步连接的其他相关表数据。

  ```c#
  public async Task<ShoppingCart> GetShoppingCartByUserId(string UserId)
  {
      return await _context.ShoppingCarts
          .Include(s => s.User)
          .Include(s => s.ShoppingCartItems).ThenInclude(li => li.TouristRoute)
          .Where(s => s.UserId == UserId)
          .FirstOrDefaultAsync();
  }
  ```

- ##### Add

  添加

  ```c#
  public void AddTouristRoute(TouristRoute touristRoute)
  {
      if(touristRoute==null)
      {
          throw new ArgumentNullException(nameof(touristRoute));
      }
      _context.TouristRoutes.Add(touristRoute);
  }
  ```

- ##### Remove

  删除

  ```c#
  public void DeleteTouristRoute(TouristRoute touristRoute)
  {
      _context.TouristRoutes.Remove(touristRoute);
  }
  ```

- ##### RemoveRange

  批量删除

  ```c#
  public void DeleteTouristRoutes(IEnumerable<TouristRoute> touristRoutes)
  {
      _context.TouristRoutes.RemoveRange(touristRoutes);
  }
  ```

- ##### SaveChanges/SaveChangesAsync

  保存数据库

  ```c#
  public bool Save()
  {
      // 保存数据库
      return (_context.SaveChanges() >= 0);
  }
  ```

- ##### OrderBy

  排序

  ```c#
  result = result.OrderBy(t => t.OriginalPrice);
  // 倒序result = result..OrderBy(p => p.Status).ThenByDescending(p=>p.ApplyTime);
  // 正序result = result..OrderBy(p => p.Status).ThenBy(p=>p.ApplyTime);
  ```




### 5.9. Stateless状态机

- ##### 添加订单系统状态机

  - Nuget Stateless

  - 在数据模型中创建使用

    ```c#
    using Stateless;
    
    // 订单的现态
    // 变量名称为名称形容词
    public enum OrderStateEnum
    { 
        Pending,  //订单已生成
        Processing, // 支付处理中
        Completed, // 交易成功
        Declined, // 交易失败
        Cancelled, // 订单取消
        Refund // 已退款
    }
    
    // 触发下一个状态的订单的动作
    // 变量名称为动词
    public enum OrderStateTriggerEnum
    {
        PlaceOrder, // 支付
        Approve, // 支付成功
        Reject, // 支付失败
        Cancel, // 取消
        Return // 退货
    }
    
    public class Order 
    {
       public Order()
       {
           // 状态机初始化
           StateMachineInit();
       }
        
    	[Key]
    	public Guid Id { get; set; }
    	public string UserId { get; set; }
    	public ApplicationUser User { get; set; }
    	public ICollection<LineItem> OrderItems { get; set; }
    	public OrderStateEnum State { get; set; }
    	public DateTime CreateDateUTC { get; set; }
    	public string TransactionMetadata { get; set; }    // 第三方支付数据
        // 状态机变量
        // 第一个泛型参数-状态：订单的状态，第二个参数-触发动作：订单状态的触发动作
        StateMachine<OrderStateEnum, OrderStateTriggerEnum> _machine;
        
        // 初始化状态机函数
        private void StateMachineInit()
        {
            // 初始化构造函数参数：初始化的状态
            _machine = new StateMachine<OrderStateEnum, OrderStateTriggerEnum>(OrderStateEnum.Pending);
            
            // 配置触发状态以及状态转换
            // 初始状态：订单已生成
            _machine.Configure(OrderStateEnum.Pending)
                // 触发状态：支付，状态：支付处理中
    		   .Permit(OrderStateTriggerEnum.PlaceOrder, OrderStateEnum.Processing)
                // 触发状态：取消，状态：订单取消
    		   .Permit(OrderStateTriggerEnum.Cancel, OrderStateEnum.Cancelled);
    
            // 初始状态：支付处理中
    		_machine.Configure(OrderStateEnum.Processing)
               // 触发状态：支付成功，状态：交易成功
    		   .Permit(OrderStateTriggerEnum.Approve, OrderStateEnum.Completed)
               // 触发状态：支付失败，状态：交易失败
    		   .Permit(OrderStateTriggerEnum.Reject, OrderStateEnum.Declined);
    
            // 初始状态：交易失败
    		_machine.Configure(OrderStateEnum.Declined)
              // 触发状态：支付，状态：支付处理中
    		  .Permit(OrderStateTriggerEnum.PlaceOrder, OrderStateEnum.Processing);
    
            // 初始状态：交易成功
    		_machine.Configure(OrderStateEnum.Completed)
              // 触发状态：退货，状态：已退款
    		  .Permit(OrderStateTriggerEnum.Return, OrderStateEnum.Refund);
        }
    }
    ```



# 6. 仓库模式

- ##### 仓库模式Repository

  可以帮我们使用对象化的数据，而不用去理会数据是怎么保存的，甚至连保存形式都可以忽略。

  ###### 接口

  ```c#
  public interface IStudentRepository
  {
      public Student GetStudent(Guid studentId);
      public IEnumerable<Student> GetStudents();
  }
  ```

  ###### 实现类

  ```c#
  public class StudentRepository: IStudentRepository
  {
      // 注入数据库连接器
      private readyonly AppContext _context;
      
      public StudentRepository(AppContext context)
      {
          _context = context;
      }
      
      public Student GetStudent(Guid studentId)
      {
          return _context.Students.FirstOrDefault(n => n.Id == studentId);
      }
      
      public IEnumerable<Student> GetStudents()
      {
          return _context.Students;
      }
  }
  ```

  ###### 依赖注入

  ```c#
  public void ConfigureServices(IServiceCollection services)
  {
      services.AddTransient<IStudentRepository, StudentRepository>();
  }
  ```

- **使用模型和数据仓库的好处**

  1. 业务逻辑与数据模型紧密藕合，减少分层，降低代码数量
  2. 完全剥离数据库业务，可以更专注于实现业务逻辑
  3. 面向对象的编程，数据转化为对象 






# 7. 数据传输对象(DTO)

### 7.1. 概述

- ##### 数据模型(Model) 和 数据传输对象(DTO)

  Model面向业务

  DTO则是面向界面，面向UI

  ```c#
  // 返回数据类型
  public class TouristRouteDto
  {
      public Guid Id { get; set; }
      public string Title { get; set; }
      public string Description { get; set; }
      // 计算方式：原价 * 折扣
      // OriginalPrice * DiscountPresent
      public decimal Price { get; set; }
      public decimal OriginalPrice { get; set; }
      public double? DiscountPresent { get; set; }
      public DateTime CreateTime { get; set; }
      public DateTime? UpdateTime { get; set; }
      public DateTime? DepartureTime { get; set; }
      public string Features { get; set; }
      public string Fees { get; set; }
      public string Notes { get; set; }
      public double? Rating { get; set; }
      public string TravelDays { get; set; }
      public string TripType { get; set; }
      public string DepartureCity { get; set; }
      // DTO和Model的嵌套映射
      // 如果映射的类型是已经在profile中注册过的对象，那不需要做其他的配置，AutoMapper会接管所以的配置
      public ICollection<TouristRoutePictureDto> TouristRoutePictures { get; set; }
  }
  ```

### 7.2. 数据验证

- ##### 数据注释 Data Annotation

  ```c#
  using System.ComponentModel.DataAnnotations;
  public class TouristRouteForCreationDto
  {
      // ErrorMessage返回指定错误信息
      [Required(ErrorMessage = "title 不可为空")]
      [MaxLength(100)]
      public string Title { get; set; }
      [Required]
      [MaxLength(1500)]
      public string Description { get; set; }
      public decimal Price { get; set; }
      public DateTime CreateTime { get; set; }
      public DateTime? UpdateTime { get; set; }
  }
  ```

- **验证接口 IValidatableObject**

  ```c#
  public class TouristRouteForCreationDto: IValidatableObject
  {
     	[Required(ErrorMessage = "title 不可为空")]
  	[MaxLength(100)]
  	public string Title { get; set; }
  	[Required]
  	[MaxLength(1500)]
  	public string Description { get; set; }
      
      // 成员变量等价的数据验证
      // 实现接口方法
      public IEnumerable<ValidatableObject> Validate(ValidationContext validationContext)
      {   
          if (Title == Description)
          {
              // ValidationResult可以提供错误消息，并返回类名或者成员变量名称的对象
              yield return new ValidationResult(
                  // 第一参数：错误信息
                  "路线名称必须与路线描述不同",
                  // 第二参数：抛出异常的错误路径
                  new[] { "TouristRouteForCreationDto" }
              );
          }
      }
  }
  ```

- **验证特性 ValidationAttributes**

  ```c#
  // 子类 继承父类
  public class TouristRouteForCreationDto : TouristRouteForManipulationDto
  {
  }
  
  // 子类 继承父类
  public class TouristRouteForUpdateDto : TouristRouteForManipulationDto
  {
      // 重写数据验证规则
      [Required(ErrorMessage = "更新必备")]
      [MaxLength(1500)]
      public override string Description { get; set; }
  }
  
  // 父类
  // 考虑到父类以后不会被调用，设置成抽象类abstract
  [TouristRouteTitleMustBeDifferentFromDescriptionAttribute] // 类级别的数据验证
  public abstract class TouristRouteForManipulationDto
  {
      [Required(ErrorMessage = "title 不可为空")]
  	[MaxLength(100)]
  	public string Title { get; set; }
  	[Required]
  	[MaxLength(1500)]
  	public virtual string Description { get; set; }  
  }
  
  using System.ComponentModel.DataAnnotations;
  public class TouristRouteTitleMustBeDifferentFromDescriptionAttribute: ValidationAttribute
  {
      // 重写IsValid函数
      // 参数1：输入的数据对象
      // 参数2：验证的上下文关系对象 ValidationContext
      // ValidationContext所访问的不再是属性级别的数据，而是Class类级别的数据
      protected override ValidationResult IsValid(
          object value, 
          ValidationContext validationContext
      )
      {
          // 通过上下文关系对象，获取当前的对象
          var touristRouteDto = (TouristRouteForManipulationDto)validationContext.ObjectInstance;
          if (touristRouteDto.Title == touristRouteDto.Description)
          {
              // ValidationResult可以提供错误消息，并返回类名或者成员变量名称的对象
              return new ValidationResult(
                  // 第一参数：错误信息
                  "路线名称必须与路线描述不同",
                  // 第二参数：抛出异常的错误路径
                  new[] { "TouristRouteForCreationDto" }
              );
          }
          return ValidationResult.Success;
      }
  }
  ```

### 7.3. AutoMapper

- ##### 配置及使用

  1. Nuget  AutoMapper.Extensions.Microsoft.DependencyInjection

  2. 注册AutoMapper依赖

     Startup.cs

     ```c#
     public void ConfigureServices(IServiceCollection services)
     {
         // 注入AutoMapper 参数：当前系统的程序集
         // 会自动扫描程序集里所以包含映射关系的profile文件
         // 通过调用AppDomain.CurrentDomain.GetAssemblies()，会吧所有的profile文件加载到当前的AppDomain中
         // 系统的映射配置就是通过profile文件管理
         // 扫描profile文件
         services.AddAutoMapper(AppDomain.CurrentDomain.GetAssemblies());
     }
     ```

  3. 创建profile文件

     ```c#
     // 继承AutoMapper的Profile父类
     public class TouristRouteProfile : Profile
     {
         // 在构造函数中完成映射配置
         public TouristRouteProfile()
         {
             // CreateMap第一个泛型参数是模型对象，第二个是目标对象
             // 会自动映射两个目标中名字相同的字段，找不到匹配的字段会被忽略，使用空或NUll代替
             CreateMap<TouristRoute, TouristRouteDto>()
                 // ForMember做字段的投影（把资源对象中的一个或几个数据经过一定的变化和计算传递给目标对象）
                 .ForMember(
                 // 第一个参数 目标对象
                 dest => dest.Price,
                 // 第二个参数 原始对象
                 // MapFrom函数进行投影，参数就是原始对象自己
                 opt => opt.MapFrom(src => src.OriginalPrice * (decimal)(src.DiscountPresent ?? 1))
             	)
                 // 枚举变字符串
                 .ForMember(
                 dest => dest.TravelDays,
                 opt => opt.MapFrom(src => src.TravelDays.ToString())
             	)
                 .ForMember(
                 dest => dest.TripType,
                 opt => opt.MapFrom(src => src.TripType.ToString())
             	)
                 .ForMember(
                 dest => dest.DepartureCity,
                 opt => opt.MapFrom(src => src.DepartureCity.ToString())
             	);
             // 把TouristRouteForCreationDto映射给TouristRoute
             CreateMap<TouristRouteForCreationDto, TouristRoute>()
                 .ForMember(
                 // 目标对象
                 dest => dest.Id,
                 // opt数据源
                 // 新建Guid
                 opt => opt.MapFrom(src => Guid.NewGuid())
             	);
             CreateMap<TouristRouteForUpdateDto, TouristRoute>();
         }
     }
     ```

  4. 控制器使用

     ```c#
     using AutoMapper;
     public class TouristRoutesController : ControllerBase
     {
         private ITouristRouteRepository _touristRouteRepository;
         // AutoMapper的注入
         private readonly IMapper _mapper;
         public TouristRoutesController(ITouristRouteRepository touristRouteRepository, IMapper mapper)
         {
             _touristRouteRepository = touristRouteRepository;
             _mapper = mapper;
         }
         [HttpGet]
         public IActionResult GerTouristRoutes()
         {
             var touristRoutes = _touristRouteRepository.GetTouristRoutes();
             if(touristRoutes == null || touristRoutes.Count() <= 2)
             {
                 return NotFound("找不到");
             }
             // Map函数的第一个泛型参数是目标结果，第二个参数是原始数据
             var touristRouteDto = _mapper.Map<IEnumerable<TouristRoute>>(touristRoutes);
             return Ok(touristRouteDto);
         }
         [HttpGet("{touristRouteId}")]
         public IActionResult GetTouristRouteById(Guid touristRouteId)
         {
             var touristRoute = _touristRouteRepository.GetTouristRoute(touristRouteId);
             if(touristRoute == null)
             {
                 return NotFound("找不到");
             }
             // Map函数的第一个泛型参数是目标结果，第二个参数是原始数据
             var touristRouteDto = _mapper.Map<TouristRouteDto>(touristRoute);
             return Ok(touristRouteDto);
         }
         [HttpPut("{touristRouteId}")]
         public IActionResult UpdateTouristRoute([FromRoute]Guid touristRouteId,
                                                 [FromBody] TouristRouteForUpdateDto
                                                 touristRouteForUpdateDto)
         {
             if (!(await _touristRouteRepository.TouristRouteExistsAsync(touristRouteId)))
             {
                 return NotFound("找不到");
             }
             var touristRouteFromRepo = await
                 _touristRouteRepository.GetTouristRouteAsync(touristRouteId);
             // Map函数第一个参数是元数据，第二个是目标数据
             _mapper.Map(touristRouteForUpdateDto, touristRouteFromRepo);
             await _touristRouteRepository.SaveAsync();
             return NoContent();
         }
     }
     ```

- **嵌套映射**

  只要变量名称相同，AutoMapper就会自动进行映射，进行映射时，会同时对第二级第三级等等都做映射。



# 8. 控制器

### 8.1. 概述

1. 类名要Controller结尾

2. 继承ControllerBase父类 (继承父类可以使用父类的大量方法)

   父类Controller比ControllerBase多了对视图VIEW的支持

3. [Controller]属性限定

   ```c#
   [Route("api/[controller]")] // api/touristroute
   [ApiController]
   public class TouristRoutesController : ControllerBase
   {
       private ITouristRouteRepository _touristRouteRepository;
   
       // 数据库仓库的依赖注入 通过构造函数注入依赖服务。通过传入接口，来注入实例
       public TouristRoutesController(ITouristRouteRepository touristRouteRepository)
       {
           _touristRouteRepository = touristRouteRepository;
       }
   
       [HttpGet]
       public IActionResult GerTouristRoutes()
       {
           var routes = _touristRouteRepository.GetTouristRoutes();
           return Ok(routes);
       }
   
       // api/touristroutes/{touristRouteId}
       [HttpGet("{touristRouteId}")]
       public IActionResult GetTouristRouteById(Guid touristRouteId)
       {
           return Ok(_touristRouteRepository.GetTouristRoute(touristRouteId));
       }
   }
   ```

### 8.2. Action函数

- ##### 概述

  在Asp.Net Core中 API的接口函数都被称为Action函数

  默认情况下，Action函数会自动去匹配控制器的路由，去处理HTTP GET方法



# 9. 路由

### 9.1. 路由特性

- ##### 概述

  ```c#
  // [controller]对应着Controller类名的前半部分
  [Route("api/[controller]")] // 转化成 api/touristroutes
  [ApiController]
  public class TouristRoutesController : ControllerBase
  {
      // api/touristroute
      // 会自动把路由地址直接映射到这个Action函数
      [HttpGet]
      public IActionResult GerTouristRoutes()
      {
      }
      
      // api/touristroutes/{touristRouteId}
      // 控制器的路由特性会映射到这个URL的前半部分，Action函数只需要负责URL的后半部分
      // 需要映射动态变量，需要加上花括号
      // 可以使用[HttpGet("{touristRouteId:Guid}")] 来限制类型
      [HttpGet("{touristRouteId}")]
      public IActionResult GetTouristRouteById(Guid touristRouteId)
      {
      }
  }
  ```



# 10. HTTP状态码控制

### 10.1. 概述

| 级别 | 概述          | 描述                                     | 常见状态码                                                   |
| ---- | ------------- | ---------------------------------------- | ------------------------------------------------------------ |
| 1XX  | Informational | 信息性状态码，表示接受的请求正在处理     |                                                              |
| 2XX  | Success       | 成功状态码，表示请求正常处理完毕         | 200 OK, 201 Created, 204 No Content                          |
| 3XX  | Redirection   | 重定向状态码，表示需要客户端进行附加操作 | 301/302 Moved Permanently, 304 Not Modified                  |
| 4XX  | Client Error  | 客户端错误状态码，表示服务器无法处理请求 | 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found |
| 5XX  | Server Error  | 服务器错误状态码，表示服务器处理请求出错 | 500 Internal Server Error, 502 Bad Gateway                   |

### 10.2. 返回正确的状态码

- ##### 200 Ok

- ##### 201 Created：创建数据成功

  ```c#
  [HttpPost]
  public IActionResult CreateTouristRoute([FromBody] TouristRouteForCreationDto touristRouteForCreationDto)
  {
      var touristRouteModel = _mapper.Map<TouristRoute>(touristRouteForCreationDto);   
      _touristRouteRepository.AddTouristRoute(touristRouteModel);
      _touristRouteRepository.Save();
      var touristRouteToReturn = _mapper.Map<TouristRouteDto>(touristRouteModel);
      // 对于成功的POST请求创建资源，返回201：CREATE
      // 内建函数CreatedAtRoute三个参数
      // 参数1：routeName api的路径名
      // 参数2：routeValue api的路径的参数 参数类型是对象
      // 参数3：post请求成功以后需要输出的数据
      return CreatedAtRoute("GetTouristRouteById", new { touristRouteId = touristRouteToReturn.Id, touristRouteToReturn });
      // 返回结果的Headers中的Location属性中存放着上文定义的API路径
  }
  ```

- ##### 204 NoContent：后端处理成功，前端不需要内容

- ##### 400 BadRequest：错误请求

- ##### 401 Unauthorized：用户没有登录，没法访问

- ##### 403 Forbidden：用户登录了，没有访问的权限

- ##### 404 NotFound：请求的资源不存在

  ```c#
  public IActionResult GerTouristRoutes()
  {	
      var routes = _touristRouteRepository.GetTouristRoutes();
      if(routes == null || routes.Count() <= 0)    
      {
          return NotFound("没有旅游线路");
      }
      return Ok(routes);
  }
  ```

- ##### 406 Unacceptable：前端请求的格式后端无法识别

- ##### 422 Unprocessable Entity：请求格式正确，但是由于含有语义错误，无法响应

  ```c#
  // Startup.cs
  public void ConfigureServices(IServiceCollection services)
  {    
      services.AddControllers(setupAction => 
  	{        
          setupAction.ReturnHttpNotAcceptable = true;    
      }).AddXmlDataContractSerializerFormatters();     
      // 可以控制Api Controller 行为的服务	
      .ConfigureApiBehaviorOptions(setupAction =>
  	{
          // 配置 非法模型响应工厂  验证数据是否非法的过程
          setupAction.InvalidModelStateResponseFactory = context => 
              // 参数 context 上下文关系对象
          {
              // ModelState 内建的全局变量
              // ModelState 是键值对类型的结构，包含当前数据模型的状态，以及该模型相应的数据验证逻辑
              var problemDetail = new ValidationProblemDetails(context.ModelState)
              {
                  Type = "无所谓",
                  Title = "数据验证失败",
                  Status = StatusCodes.Status422UnprocessableEntity,
                  Detail = "请看详细说明",
                  Instance = context.HttpContext.Request.Path // 请求路径
              };
              // 追踪ID ： 上下文关系对象中的traceId
              problemDetail.Extensions.Add("traceId", context.HttpContext.TraceIdentifier);           
              // 数据验证错误返回422：请求格式正确，但是由于含有语义错误，无法相应
              // 422代替400
              return new UnprocessableEntityObjectResult(problemDetail)
              {
                  // 问题类型 告诉前端响应主体的内容是有问题的
                  ContentTypes = { "application/problem+json" }
              };
          };
      });
  }
  ```
  



# 11. 参数特性

### 11.1. 概述

| Attribue      | 参数来源                                                |
| ------------- | ------------------------------------------------------- |
| [FromQuery]   | 请求url的参数字符串                                     |
| [FromBody]    | 请求主体数据                                            |
| [FromForm]    | 请求主体的表单数据<br />(IFormFIle,IFormFileCOllection) |
| [FromRoute]   | MVC架构下的Route路由的URL的参数                         |
| [FromService] | 数据来源于已注入的服务依赖                              |

### 11.2. FromQuery

[FromQuery] 参数来自地址栏，参数与URL片段使用？相隔开。也就是说FromQuery标签将会提取URL中？后的字符串

api/search?pageNumber=1&query=埃及

```c#
// 匹配参数名不一致的情况
[FromQuery(Name="xxx")]// api/touristRoute?keyword=传入的参数
[HttpGet]
public IActionResult GetTouristRoutes([FromQuery] string keyword)
{
    var touristRouteFromRepo = _touristRouteRepository.GetTouristRoutes(keyword);
}
```

多个关键词的情况下，建立参数组合；URL中用&连接

```c#
// 需要多个关键词的情况下，建立参数组合
public class TouristRouteResourceParamaters
{
    public string Keyword { get; set; }
    public string RatingOperator { get; set; }
    public int? RatingValue { get; set; }
    private string _rating;
    public string Rating
    {
        get { return _rating; }
        set
        {
            if (!string.IsNullOrWhiteSpace(value))
            {
                Regex regex = new Regex(@"([A-Za-z0-9\-]+)(\d+)");
                Match match = regex.Match(value);
                if (match.Success)
                {
                    RatingOperator = match.Groups[1].Value;
                    RatingValue = Int32.Parse(match.Groups[2].Value);
                }
            }
            _rating = value;
        }
    }
}

[HttpGet]
public IActionResult GetTouristRoutes([FromQuery] TouristRouteResourceParamaters paramaters)
{
    var touristRouteFromRepo = _touristRouteRepository.GetTouristRoutes(paramaters.keyword, paramaters.Rating);
}
```

### 11.3. FromRoute

[FromRoute]是直接写在URL中，作为URL片段的一部分。是不带？不带&的查询字符串

api/touristRoutes/123456

### 11.4.  FromBody

[FromBody]负责接收请求主体，也就是请求Body里面的数据

### 11.5. 数据分页

#### 11.5.1. 概述

- ##### 分页参数

  - 参数：查询字符串 api/touristRoutes?当前页=1&每页大小=20

  - 分页参数需要默认值和阈限值

    比如当前默认为1；每页大小默认为10，且不得超过50

  - 分页的工作需要在数据库中完成

- ##### 分页思路

  ```c#
  // 1. 跳过一定量的数据 skip
  var skip = (pageNumber -1) * pageSize;result = result.Skip(skip);
  // 2. 以pagesize为标准显示一定量的数据
  result = result.Take(pageSize);
  ```

#### 11.5.2. 模组分页

  - ##### 分页参数处理器

    ```c#
    public class PaginationResourceParamaters
    {
        private int _pageNumber = 1;
        public int PageNumber
        {
            get
            {
                return _pageNumber;
            }
            set
            {
                if (value >= 1)
                {
                    _pageNumber = value;
                }
            }
        }
        private int _pageSize = 10;
        const int maxPageSize = 50;
        public int PageSize
        {
            get
            {
                return _pageSize;
            }
            set
            {
                if (value >= 1)
                {
                    _pageSize = (value > maxPageSize) ? maxPageSize : value;
                }
            }
        }
    }
    ```

  - ##### 分页模组

    ```c#
    public class PaginationList<T> : List<T>
    {
        public int TotalPages { get; private set; }
        public int TotalCount { get; private set; }
        public bool HasPrevious => CurrentPage > 1;
        public bool HasNext => CurrentPage < TotalPages;
        // 当前页
        public int CurrentPage { get; set; }
        // 每页数据量
        public int PageSize { get; set; }
        public PaginationList(int totalCount, int currentPage, int pageSize, List<T> items)
        {
            CurrentPage = currentPage;
            PageSize = pageSize;
            // 因为继承了 List<T> 数据结构，所以可以使用addRange来添加自己的列表数据
            AddRange(items);
            TotalCount = totalCount;
            TotalPages = (int)Math.Ceiling(totalCount / (double)pageSize);
        }
        // 使用工厂模式，调用构造函数，初始化
        // 工厂模式函数一般都会作为一个公开的静态函数，定义为类的成员
        // IQueryable<T> result 参数为 数据库延迟执行
        public static async Task<PaginationList<T>> CreateAsync(
            int currentPage, int pageSize, IQueryable<T> result)
        {
            var totalCount = await result.CountAsync();
            // pagination
            // skip
            var skip = (currentPage - 1) * pageSize;
            result = result.Skip(skip);
            // 以pagesize为标准显示一定量的数据
            result = result.Take(pageSize);
            // 读取数据库
            var items = await result.ToListAsync();
            return new PaginationList<T>(totalCount, currentPage, pageSize, items);
        }
    }
    ```

  - ##### 使用

    ```c#
    [HttpGet(Name = "GetOrders")]
    [Authorize(AuthenticationSchemes = "Bearer")]
    public async Task<IActionResult> GetOrders([FromQuery] PaginationResourceParamaters paramters)
    {
        // 1.获取当前用户
        var userId = _httpContextAccessor.HttpContext.User.FindFirst(ClaimTypes.NameIdentifier).Value;
        // 2 使用用户id来获取订单历史记录
        var orders = await _touristRouteRepository.GetOrdersByUserId(
            userId, paramters.PageSize, paramters.PageNumber);
        return Ok(_mapper.Map<IEnumerable<OrderDto>>(orders));
    }
    public async Task<PaginationList<Order>> GetOrdersByUserId(
        string userId, int pageSize, int pageNumber){
        IQueryable<Order> result = _context.Orders.Where(o => o.UserId == userId);
        // PaginationList作为返回类型
        return await PaginationList<Order>.CreateAsync(pageSize, pageNumber, result);
    }
    ```

#### 11.5.3. 分页导航

  - #####  概述

    - 数据列表将会出现在响应主体中，而分页信息与数据列表分开，以metadata元数据的形式在header中输出

    - 分页元数据pagination metadata 保存在自定义相应头部：x-pagination 

      请求使用的是application/json的目的是获取资源，如果数据列表和分页信息放在一起的话会造成媒体类型不匹配

      ```c#
      {
          "上一页": "https://api.github.com/resource?.page=2",
          "下一页": "https://api.github.com/resource?.page=4",
          "当前页": 3,
          "总页数": 15,
          "总数据量": 200,
      }
      ```

  - ##### 配置urlHelper

    - 配置urlHelper

      Startup.cs

      ```c#
      public void ConfigureServices(IServiceCollection services)
      {
          // 配置urlHelper；必须使用单例注册
          services.AddSingleton<IActionContextAccessor, ActionContextAccessor>();
      }
      ```
    
      TouristRoutesController.cs

      ```c#
      // 依赖注入
      private readonly IUrlHelper _urlHelper;
      public TouristRoutesController(
          IUrlHelperFactory urlHelperFactory,
          IActionContextAccessor actionContextAccessor)
      {
          _urlHelper = urlHelperFactory.GetUrlHelper(actionContextAccessor.ActionContext);
      }
      ```
    
    - 生成旅游资源URL

      ```c#
      // 生产旅游资源URL
      private string GenerateTouristRouteResourceURL(
          TouristRouteResourceParamaters paramaters,  // 旅游路线的参数处理器
          PaginationResourceParamaters paramaters2,   // 分页参数处理器
          ResourceUrlType type    // URL类型
      )  
      {
          // 通过URL类型，自动生成上一页路径或者下一页路径
          return type switch
          {
              // urlHelper 管理url的配置，使用Link函数生成绝对路径的URL，名称参数就是http方法标签定义的字符串名称
              ResourceUrlType.PreviousPage => _urlHelper.Link("GetTouristRoutes",
              new
              {
                  fields = paramaters.Fields,
                  orderBy = paramaters.OrderBy,
                  keyword = paramaters.Keyword,
                  rating = paramaters.Rating,
                  pageNumber = paramaters2.PageNumber - 1,
                  pageSize = paramaters2.PageSize
              }),
              ResourceUrlType.NextPage => _urlHelper.Link("GetTouristRoutes",
              new
              {
                  fields = paramaters.Fields,
                  orderBy = paramaters.OrderBy,
                  keyword = paramaters.Keyword,
                  rating = paramaters.Rating,
                  pageNumber = paramaters2.PageNumber + 1,
                  pageSize = paramaters2.PageSize
              }),
              _ => _urlHelper.Link("GetTouristRoutes",
              new
              {
                  fields = paramaters.Fields,
                  orderBy = paramaters.OrderBy,
                  keyword = paramaters.Keyword,
                  rating = paramaters.Rating,
                  pageNumber = paramaters2.PageNumber,
                  pageSize = paramaters2.PageSize
              })
          };
      }
      ```
    
    - ##### 配置x-pagination并使用

      ```c#
      [HttpGet(Name = "GetTouristRoutes")]
      [HttpHead]
      public async Task<IActionResult> GerTouristRoutes(
          [FromQuery] TouristRouteResourceParamaters paramaters,
          [FromQuery] PaginationResourceParamaters paramaters2)
      {
          /******* 数据验证等 省略  **********/
           
          // 取得数据列表
          var touristRoutesFromRepo = await _touristRouteRepository
                      .GetTouristRoutesAsync(
                          paramaters.Keyword,
                          paramaters.RatingOperator,
                          paramaters.RatingValue,
                          paramaters2.PageSize,
                          paramaters2.PageNumber,
                          paramaters.OrderBy
                      );
          // DTO
          var touristRoutesDto = _mapper.Map<IEnumerable<TouristRouteDto>>(touristRoutesFromRepo);
      
          // 生成前一页URL
          var previousPageLink = touristRoutesFromRepo.HasPrevious
                      ? GenerateTouristRouteResourceURL(paramaters, paramaters2, ResourceUrlType.PreviousPage)
                      : null;
      
          // 生成后一页URL
          var nextPageLink = touristRoutesFromRepo.HasNext
                      ? GenerateTouristRouteResourceURL(paramaters, paramaters2, ResourceUrlType.NextPage)
                      : null;
      
          // x-pagination
          var paginationMetadata = new
                  {
                      previousPageLink,
                      nextPageLink,
                      totalCount = touristRoutesFromRepo.TotalCount,
                      pageSize = touristRoutesFromRepo.PageSize,
                      currentPage = touristRoutesFromRepo.CurrentPage,
                      totalPages = touristRoutesFromRepo.TotalPages
                  };
          
      	// 添加自定义的响应头部
      	// 参数是json字符串格式的分页元数据
      	Response.Headers.Add("x-pagination", Newtonsoft.Json.JsonConvert.SerializeObject(paginationMetadata));
          
            /******* 返回等 省略  **********/
      }
      ```

### 11.6 数据排序

- ##### 排序思路

  URL：api/touristRoutes?orderBy=rating desc, originalPrice desc

  Orderby字符串 > DTO属性 > Model dto

  数据排序必须在数据库中进行

- ##### 通过属性映射动态排序

  - 通过映射动态排序

    ```c#
    public async Task<PaginationList<TouristRoute>> GetTouristRoutesAsync
    {
        // 数据取得以上省略
        if (!string.IsNullOrWhiteSpace(orderBy))
        {
            // 1. 从dto中成员变量的名称和字符串映射到数据模型的字段
            // 获取旅游路线的映射字典
            var touristRouteMappingDictionary = _propertyMappingService
                .GetPropertyMapping<TouristRouteDto, TouristRoute>();
            // 2.执行排序
            // 使用LINQ的动态插件system.linq.dynamic ，可以使用字符串去排序
            // 参数1：orderBy字符串；参数2：字符串映射到模型的字典类型法则
            result = result.ApplySort(orderBy, touristRouteMappingDictionary);
        }
        return await PaginationList<TouristRoute>.CreateAsync(pageNumber, pageSize, result);
    }
    ```

    检查传入的排序字符串是否存在

    ```c#
    // IPropertyMappingService propertyMappingService
    if (!_propertyMappingService.IsMappingExists<TouristRouteDto, TouristRoute>(paramaters.OrderBy))
    {
        return BadRequest("请输入正确的排序参数");
    }
    ```

  - 创建属性映射服务

    ```c#
    // 与属性映射字典的key相匹配的value
    public class PropertyMappingValue
    {
        // 匹配模型的映射属性列表
        // 将会被映射的目标属性
        public IEnumerable<string> DestinationProperties { get; private set; }
        public PropertyMappingValue(IEnumerable<string> destinationProperties)
        {
            DestinationProperties = destinationProperties;
        }
    }
    ```

    ```c#
    public interface IPropertyMapping { }
    // 最终在数据仓库中使用的映射字典
    public class PropertyMapping<TSource, TDestination> : IPropertyMapping
    {
        // 字典类型映射列表
        public Dictionary<string, PropertyMappingValue> _mappingDictionary { get; set; }
        public PropertyMapping(Dictionary<string, PropertyMappingValue> mappingDictionary)
        {
            _mappingDictionary = mappingDictionary;
        }
    }
    ```

    ```c#
    public interface IPropertyMappingService
    {
        Dictionary<string, PropertyMappingValue> GetPropertyMapping<TSource, TDestination>();
        bool IsMappingExists<TSource, TDestination>(string fields);
        bool IsPropertiesExists<T>(string fields);
    }
    
    // 属性映射服务
    public class PropertyMappingService : IPropertyMappingService
    {
        // 属性映射字典列表
        // 属性的字符串名称为键，模型的属性为值
        private Dictionary<string, PropertyMappingValue> _touristRoutePropertyMapping =
            new Dictionary<string, PropertyMappingValue>(StringComparer.OrdinalIgnoreCase)
        {
            { "Id", new PropertyMappingValue(new List<string>(){ "Id" }) },
            { "Title" , new PropertyMappingValue(new List<string>(){ "Title" }) },
            { "Rating", new PropertyMappingValue(new List<string>(){ "Rating" }) },
            { "OriginalPrice", new PropertyMappingValue(new List<string>(){ "OriginalPrice" })  }
        };
        // 包含DTO中字段与字符串名称的对应关系 将DTO映射到数据模型
        // 会将映射的字段提取出来，放入字典类型的数据结构中。
        // 如果DTO的字段和模型的字段不一致，所以字典的类型应该使用数据模型中字段的字符串的名称
        private IList<IPropertyMapping> _propertyMappings = new List<IPropertyMapping>();
        public PropertyMappingService()
        {
            // 在构建函数中，通过私有成员变量PropertyMapping字典，从数据源touristRouteDto向目标对象TouristRoute添加新的PropertyMapping关系
            _propertyMappings.Add(
                new PropertyMapping<TouristRouteDto, TouristRoute>(_touristRoutePropertyMapping));
        }
        // 定位字典列表 从一个数据源映射到另一个数据目标
        // Dictionary泛型参数1：属性字符串的名称；泛型参数2：目标模型的属性
        public Dictionary<string, PropertyMappingValue> GetPropertyMapping<TSource, TDestination>()
        {
            // 获得匹配的映射对象
            var matchingMapping = _propertyMappings.OfType<PropertyMapping<TSource, TDestination>>();
            if (matchingMapping.Count() == 1)
            {
                return matchingMapping.First()._mappingDictionary;
            }
            throw new Exception(
                $"Cannot find exact property mapping instance for <{typeof(TSource)}, {typeof(TDestination)}>");
        }
        public bool IsMappingExists<TSource,TDestination>(string fields)
        {
            var propertyMapping = GetPropertyMapping<TSource, TDestination>();
            if (string.IsNullOrWhiteSpace(fields))
            {
                return true;
            }
            // 逗号来分隔字段字符串
            var fieldsAfterSplit = fields.Split(',');
            foreach (var field in fieldsAfterSplit)
            {
                // 去掉空格
                var trimmedField = field.Trim();
                // 获得属性名称字符串
                var indexOfFirstSpace = trimmedField.IndexOf(" ");
                var propertyName = indexOfFirstSpace == -1 ?
                    trimmedField : trimmedField.Remove(indexOfFirstSpace);
                if (!propertyMapping.ContainsKey(propertyName))
                {
                    return false;
                }
            }
            return true;
        }
        // 需要映射的字符串是否存在
        public bool IsPropertiesExists<T>(string fields)
        {
            if (string.IsNullOrWhiteSpace(fields))
            {
                return true;
            }
            // 逗号来分隔字段字符串
            var fieldsAfterSplit = fields.Split(',');
            foreach (var field in fieldsAfterSplit)
            {
                // 获得属性名称字符串
                var propertyName = field.Trim();
                var propertyInfo = typeof(T)
                    .GetProperty(propertyName,
                                 BindingFlags.IgnoreCase | BindingFlags.Public | BindingFlags.Instance);
                // 如果T中没找到对应的属性
                if (propertyInfo == null)
                {
                    return false;
                }
            }
            return true;
        }
    }
    ```

    ```c#
    // Startup的
    ConfigureServicesservices.AddTransient<IPropertyMappingService, PropertyMappingService>();
    ```

  - 创建排序扩展方法

    OrderBy通过字符串进行排序：Nuget System.Linq.Dynamic.Core

    ```c#
    using System.Linq.Dynamic.Core;
    public static class IQueryableExtensions
    {
        public static IQueryable<T> ApplySort<T>(
            this IQueryable<T> source,
            string orderBy,
            Dictionary<string,
            PropertyMappingValue> mappingDictionary
        )
        {
            if (source == null)
            {
                throw new ArgumentNullException("source");
            }
            if (mappingDictionary == null)
            {
                throw new ArgumentNullException("mappingDictionary");
            }
            if (string.IsNullOrWhiteSpace(orderBy))
            {
                return source;
            }
            var orderByString = string.Empty;
            var orderByAfterSplit = orderBy.Split(',');
            foreach (var order in orderByAfterSplit)
            {
                var trimmedOrder = order.Trim();
                // 通过字符串“desc”来判断升序还是降序
                var orderDescending = trimmedOrder.EndsWith(" desc");
                // 删除升序或降序字符串“asc” or “desc”来获得属性的名称
                var indexOfFirstSpace = trimmedOrder.IndexOf(" ");
                var propertyName = indexOfFirstSpace == -1
                    ? trimmedOrder
                    : trimmedOrder.Remove(indexOfFirstSpace);
                if (!mappingDictionary.ContainsKey(propertyName))
                {
                    throw new ArgumentException($"Key mapping for {propertyName} is missing");
                }
                // 在映射字典中通过属性字符串获得属性对象
                var propertyMappingValue = mappingDictionary[propertyName];
                if (propertyMappingValue == null)
                {
                    throw new ArgumentNullException("propertyMappingValue");
                }
                // 循环中对比所有映射属性的类型
                // 需要反向处理，否则顺序错误
                foreach (var destinationPropery in propertyMappingValue.DestinationProperties.Reverse())
                {
                    // 给IQUeryable 添加排序字符串
                    orderByString = orderByString +
                        (string.IsNullOrWhiteSpace(orderByString) ? string.Empty : ", ")
                        + destinationPropery
                        + (orderDescending ? " descending" : " ascending");
                }
            }
            // orderby通过传入字符串排序
            return source.OrderBy(orderByString);
        }
    }
    ```

### 11.7 数据塑形

- ##### 什么是数据塑形

  定制化选择性后端输出数据的技术

  api/touristroutes?fileds=title,orginalPrice

- ##### 使用动态类型进行数据塑形

  ExpandoObject：可以在程序运行的时候,动态的添加或者删除一个类的成员变量，也就是说可以动态的处理对象的字段

  1. 创建处理动态类型的扩展方法

     - 列表资源的扩展

       ```c#
       // 数据列表类型的扩展
       public static class IEnumerableExtensions
       {
           public static IEnumerable<ExpandoObject> ShapeData<TSource>(
               this IEnumerable<TSource> source,
               string fields
           )
           {
               if (source == null)
               {
                   throw new ArgumentNullException(nameof(source));
               }
               var expandoObjectList = new List<ExpandoObject>();
               // 避免在列表中遍历数据， 创建一个属性信息列表
               var propertyInfoList = new List<PropertyInfo>();
               if (string.IsNullOrWhiteSpace(fields))
               {
                   // 希望返回动态类型对象ExpandoObject所有的属性
                   // 保存TSource中输入类型数据中所有属性的信息
                   var propertyInfos = typeof(TSource).GetProperties(
                       BindingFlags.IgnoreCase | BindingFlags.Public | BindingFlags.Instance);
                   propertyInfoList.AddRange(propertyInfos);
               }
               else
               {
                   // 逗号来分隔字段字符串
                   var fieldsAfterSplit = fields.Split(',');
                   foreach (var field in fieldsAfterSplit)
                   {
                       // 去掉首尾多余的空格，获得属性名称
                       var propertyName = field.Trim();
                       var propertyInfo = typeof(TSource).GetProperty(
                           propertyName,
                           BindingFlags.IgnoreCase | BindingFlags.Public | BindingFlags.Instance);
                       if (propertyInfo == null)
                       {
                           throw new Exception($"属性 {propertyName} 找不到" + $"{typeof(TSource)}");
                       }
                       propertyInfoList.Add(propertyInfo);
                   }
               }
               // 遍历数据源
               foreach (TSource sourceObject in source)
               {
                   // 创建动态类型对象， 创建数据塑性对象
                   var dataShapedObject = new ExpandoObject();
                   // 遍历属性信息列表
                   foreach (var propertyInfo in propertyInfoList)
                   {
                       // 获得对应属性的真实数据
                       var porpertyValue = propertyInfo.GetValue(sourceObject);
                       // 数据塑形类型转化成字典类型才可以添加
                       ((IDictionary<string, object>)dataShapedObject).
                           Add(propertyInfo.Name, porpertyValue);
                   }
                   expandoObjectList.Add(dataShapedObject);
               }
               return expandoObjectList;
           }
       }
       ```

     - 单一资源的扩展

       ```c#
       // 性能的考虑需要把 IEnumerableExtensions 和 ObjectExtensions 分离开。
       public static class ObjectExtensions
       {
           public static ExpandoObject ShapeData<TSource>(this TSource source, string fields)
           {
               if (source == null)
               {
                   throw new ArgumentNullException(nameof(source));
               }
               var dataShapedObject = new ExpandoObject();
               if (string.IsNullOrWhiteSpace(fields))
               {
                   var propertyInfos = typeof(TSource).GetProperties(
                       BindingFlags.IgnoreCase |BindingFlags.Public | BindingFlags.Instance);
                   foreach (var propertyInfo in propertyInfos)
                   {
                       var propertyValue = propertyInfo.GetValue(source);
                       ((IDictionary<string, object>)dataShapedObject)
                       .Add(propertyInfo.Name, propertyValue);
                   }
                   return dataShapedObject;
               }
               var fieldsAfterSplit = fields.Split(',');
               foreach (var field in fieldsAfterSplit)
               {
                   var propertyName = field.Trim();
                   var propertyInfo = typeof(TSource)
                       .GetProperty(propertyName,
                                    BindingFlags.IgnoreCase | BindingFlags.Public |
                                    BindingFlags.Instance);
                   if (propertyInfo == null)
                   {
                       throw new Exception($"Property {propertyName} wasn't found " +                    $"on {typeof(TSource)}");
                   }
                   var propertyValue = propertyInfo.GetValue(source);
                   ((IDictionary<string, object>)dataShapedObject)
                   .Add(propertyInfo.Name, propertyValue);
               } 
               return dataShapedObject;
           }
       }
       ```

       

  2. 添加数据塑形参数

     ```c#
     public string Fields;
     ```

  3. 利用扩展组件进行数据塑形

     ```c#
     touristRouteDto.ShapeData(Fields);
     ```

  4. 数据塑性参数检查

     ```c#
     public bool IsPropertiesExists<T>(string fields){
         if (string.IsNullOrWhiteSpace(fields))
         {
             return true;
         }
         // 逗号来分隔字段字符串
         var fieldsAfterSplit = fields.Split(',');
         // 检查每一个塑形参数是否合法
         foreach (var field in fieldsAfterSplit)
         {
             // 获得属性名称字符串
             var propertyName = field.Trim();
             var propertyInfo = typeof(T)
                 .GetProperty(propertyName,
                              BindingFlags.IgnoreCase | BindingFlags.Public | BindingFlags.Instance);
             // 如果T中没找到对应的属性
             if (propertyInfo == null)
             {
                 return false;
             }
         }
         return true;
     }
     ```

     ```c#
     // 调用
     if (!_propertyMappingService.IsPropertiesExists<TouristRouteDto>(paramaters.Fields))
     {
         return BadRequest("请输入正确的塑性参数");
     }
     ```

     

# 12. HTTP请求

### 12.1. HTTP请求特性

[HttpGet], [HttpPost], [HttpDelete], [HttpPut], [HttpPatch], [HttpHead], [HttpOption] 

需要映射动态变量，需要加上花括号。变量名称后面加：数据类型，可以现在类型

使用Name关键字，定义API的名称

```c#
[HttpPost("{touristRouteId:Guid}", Name = "UpdateTouristRoute")]
```

### 12.2. GET请求

### 12.3. POST请求

#### 12.3.1. 添加资源

```c#
[HttpPost]
public IActionResult CreateTouristRoute([FromBody] TouristRouteForCreationDto touristRouteForCreationDto){
    var touristRouteModel = _mapper.Map<TouristRoute>(touristRouteForCreationDto);
    _touristRouteRepository.AddTouristRoute(touristRouteModel);
    _touristRouteRepository.Save();
    var touristRouteToReturn = _mapper.Map<TouristRouteDto>(touristRouteModel);
    return CreatedAtRoute("GetTouristRouteById", new { touristRouteId = touristRouteToReturn.Id }, touristRouteToReturn);
}
```

### 12.4. PUT请求

#### 12.4.1. 全部更新

对某个资源所有的字段进行更新

当某些字段被忽略后，信息则会被删除

```c#
[HttpPut("{touristRouteId}")]
public async Task<IActionResult> UpdateTouristRoute(
    [FromRoute]Guid touristRouteId,
    [FromBody] TouristRouteForUpdateDto touristRouteForUpdateDto)
{
    if (!(await _touristRouteRepository.TouristRouteExistsAsync(touristRouteId)))
    {
        return NotFound("旅游路线找不到");
    }
    var touristRouteFromRepo = await _touristRouteRepository.GetTouristRouteAsync(touristRouteId);
    // 1. model映射成dto
    // 2. 更新dto数据
    // 3. 更新后dto映射回model
    _mapper.Map(touristRouteForUpdateDto, touristRouteFromRepo);
    
    // 更新数据库 : _touristRouteRepository.Save()
    // 在数据仓库中，我们调用的是底层的ORM框架entityframework，在entityframework中数据模型touristRouteFromRepo是根据上下文关系对象context来追踪的
    // 当我们在执行_mapper.Map时，数据模型中的数据其实已经被修改了，而这个时候数据模型中的追踪状态也就相应的发生了变化，	 // 模型的追踪状态是由entityframework上下文关系对象context自我管理的，所以到了最后执行 _touristRouteRepository.Save()数据库保存操作的时候，
    // 模型的追踪状态就会随着context的保存，把更新后状态写入数据库
    await _touristRouteRepository.SaveAsync();
    
    // 204NoContent
    return NoContent();
}
```



### 12.5. PATCH请求

#### 12.5.1. JSON 补丁文档

- ##### JSON Patch （JSON 补丁文档）

  ```json
  // PATCH api/touristRoutes/{id}
  [
      {"op": "replace", "path": "/title", "value": "巴厘岛一日游"},
      {"op": "replace", "path": "/originalPrice", "value": "999"},
      {"op": "remove", "path": "/discountPercent"},
      {"op": "replace", "path": "/picture/url", "value": "../images/123456.png" }	// 嵌套
  ]
  ```

- ##### JSON Patch操作(op)

  add 添加， remove 删除， replace 替换， move 转移， copy复制，test测试

  除了以上6个以外，JSON Patch不允许有其他的自定义的操作

  目标资源数据：patch

  顺序不重要，一次patch所有请求对象都是等价的

- ##### JSON Patch 配置

  1. Nuget Microsoft.AspNetCore.JsonPatch

     Nuget Microsoft.AspNetCore.Mvc.NewtonsoftJson 用于实现JSON和JSON Patch绑定的处理

  2. 注入项目依赖

     Startup.cs

     ```c#
     public void ConfigureServices(IServiceCollection services)
     {
         services.AddControllers(setupAction =>
     	{
     		setupAction.ReturnHttpNotAcceptable = true;
         })
         // JsonPatch框架
     	.AddNewtonsoftJson(setupAction =>
     	{
             // 配置json序列化设置
             setupAction.SerializerSettings.ContractResolver =
                 new CamelCasePropertyNamesContractResolver();
         })
     	.AddXmlDataContractSerializerFormatters();
     }
     ```

#### 12.5.2. 部分更新

  对某个资源所选的某几个字段部分更新

  ```c#
  using Microsoft.AspNetCore.JsonPatch;
  [HttpPatch("{touristRouteId}"]
  public async Task<IActionResult> PartiallyUpdateTouristRoute(
      [FromRoute]Guid touristRouteId,
      [FromBody] JsonPatchDocument<TouristRouteForUpdateDto> patchDocument)
  {
      if (!(await _touristRouteRepository.TouristRouteExistsAsync(touristRouteId)))
      {
          return NotFound("旅游路线找不到");
      }
      var touristRouteFromRepo = await _touristRouteRepository.GetTouristRouteAsync(touristRouteId);
      var touristRouteToPatch = _mapper.Map<TouristRouteForUpdateDto>(touristRouteFromRepo);
      // 给原始的数据打上补丁
      // JsonPatchDocument内建函数ApplyTo来进行补丁
      // 执行patchDocument.ApplyTo后touristRouteToPatch就包含本地数据了
      // 第二个参数全局变量ModelState
      patchDocument.ApplyTo(touristRouteToPatch, ModelState);
      // 因为数据验证的对象不是Dto而是JsonPatchDocument，Dto中的数据验证起不到作用。使用ModelState来处理数据验证
      // TryValidateModel校验数据是否合法，参数是打过补丁的Dto。会调用ModelState去做数据验证，验证成功返回true
      // ModelState通过patchDocument.ApplyTo函数与DTO进行绑定的
      // 数据库的验证规则是DataAnnotations来定义的
      if (!TryValidateModel(touristRouteToPatch))
      {
          // ValidationProblem可以给前端提供ModelState所包含的验证规则的具体信息
          return ValidationProblem(ModelState);
      }
      _mapper.Map(touristRouteToPatch, touristRouteFromRepo);
      await _touristRouteRepository.SaveAsync();
      return NoContent();
  }
  ```



### 12.6. DELETE请求

#### 12.6.1. 删除资源

```c#
[HttpDelete("{touristRouteId}"]
public async Task<IActionResult> DeleteTouristRoute(
    [FromRoute] Guid touristRouteId)
{
    if (!(await _touristRouteRepository.TouristRouteExistsAsync(touristRouteId)))
    {
        return NotFound("旅游路线找不到");
    }
    var touristRoute = await _touristRouteRepository.GetTouristRouteAsync(touristRouteId);
    _touristRouteRepository.DeleteTouristRoute(touristRoute);
    await _touristRouteRepository.SaveAsync();
    return NoContent();
}
```

#### 12.6.2. 批量删除

```c#
// 形式：api/touristRoutes/(1,2,3,4)
// "({touristIDs})" 参数列表
[HttpDelete("({touristIDs})")]
[Authorize(Roles = "Admin")]
public async Task<IActionResult> DeleteByIDs(
    [ModelBinder(BinderType = typeof(ArrayModelBinder))][FromRoute]IEnumerable<Guid> touristIDs)
{
    if(touristIDs == null)
    {
        return BadRequest();
    }
    var touristRoutesFromRepo = await _touristRouteRepository.GetTouristRoutesByIDListAsync(touristIDs);
    _touristRouteRepository.DeleteTouristRoutes(touristRoutesFromRepo);
    await _touristRouteRepository.SaveAsync();
    return NoContent();
}
// 将字符串的参数转化为Guid的列表
// 继承IModelBinder接口 ：自定义的数据绑定模型， 需要接口定义的实现BindModelAsync函数
public class ArrayModelBinder : IModelBinder
{
    // 参数ModelBindingContext：
    // 上下文关系对象，这个对象内包含了绑定所以的元数据，可以利用这些元数据在函数中实现字符串与数据的类型绑定
    public Task BindModelAsync(ModelBindingContext bindingContext)
    {
        // Our binder works only on enumerable types
        if (!bindingContext.ModelMetadata.IsEnumerableType)
        {
            bindingContext.Result = ModelBindingResult.Failed();
            return Task.CompletedTask;
        }
        
        // Get the inputted value through the value provider
        var value = bindingContext.ValueProvider
            .GetValue(bindingContext.ModelName).ToString();
        
        // If that value is null or whitespace, we return null
        if (string.IsNullOrWhiteSpace(value))
        {
            bindingContext.Result = ModelBindingResult.Success(null);
            return Task.CompletedTask;
        }
        
        // The value isn't null or whitespace,
        // and the type of the model is enumerable.
        // Get the enumerable's type, and a converter
        var elementType = bindingContext.ModelType.GetTypeInfo().GenericTypeArguments[0];
        var converter = TypeDescriptor.GetConverter(elementType);
        
        // Convert each item in the value list to the enumerable type
        var values = value.Split(new[] { "," }, StringSplitOptions.RemoveEmptyEntries)
            .Select(x => converter.ConvertFromString(x.Trim()))
            .ToArray();
        
        // Create an array of that type, and set it as the Model value
        var typedValues = Array.CreateInstance(elementType, values.Length);
        values.CopyTo(typedValues, 0);
        bindingContext.Model = typedValues;
        
        // return a successful result, passing in the Model
        bindingContext.Result = ModelBindingResult.Success(bindingContext.Model);
        return Task.CompletedTask;
    }
}
```



# 13. 用户登录(JWT)

### 13.1. 概述

- ##### 无状态登录 JWT

  1. 用户登录(客户端)    →     POST api/login {email, password}     →    服务器
  2. 客户端                     ←      返回JWT                                                  ←    使用密码加密JWT(服务器)
  3. 访问资源                 →      请求携带JWT                                          →    服务器
  4. 客户端                     ←      如果JWT有效，则返回资源                   ←    使用相同密码的验证JWT

- ##### Session 和 JWT

  1. Session需要保存在服务器上，Session ID 保存在前端的cookie中
  2. JWT信息只需要保存在客户端
  3. JWT有无状态登录优势，可以分布式部署

- ##### JWT优点

  1. 无状态，简单，方便，完美支持分布式部署
  2. 非对称加密，Token安全性高

- ##### JWT缺点

  1. 由于无状态的特征，token一经发布则无法取消，无法被服务器禁用
  2. 明文传递，Token安全性低，但是使用https可以解决


### 13.2. 配置使用

- ##### 配置JWT

  1. Nuget Microsoft.AspNetCore.Authentication.JwtBearer

  2. 配置JWT的私钥

     appsettings.json

     ```json
     // 认证属性
     "Authentication": {
       // 私钥
       "SecretKey": "suibianzifuchaun",
       // 发布者
       "Issuer": "fakexiecheng.com",
       // 接收者
       "Audience": "fakexiecheng.com"
     }
     ```

  3. 注入JWT的身份认证服务

     Startup.cs

     ```c#
     public void ConfigureServices(IServiceCollection services)
     {
     	// 注册身份认证服务，服务的参数需要填入默认的身份认证类型 -》 JWT的认证类型
     	services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
     	    // 配置JWT认证
     	    .AddJwtBearer(options =>
     	    {
     	        var secretByte = Encoding.UTF8.GetBytes(Configuration["Authentication:SecretKey"]);
     	        // JWT的认证参数
     	        options.TokenValidationParameters = new TokenValidationParameters()
     	        {
     	            // 验证Token的发布者
     	            // 只有定义的发布者所发出的Token才会被接受
     	            ValidateIssuer = true,
     	            ValidIssuer = Configuration["Authentication:Issuer"],
     
     	            // 验证Token的持有者
     	            ValidateAudience = true,
     	            ValidAudience = Configuration["Authentication:Audience"],
     
     	            // 验证Token是否过期
     	            ValidateLifetime = true,
     
     	            // 传入Token的私钥，并且进行加密
     	            IssuerSigningKey = new SymmetricSecurityKey(secretByte)
     	        };
     	    });
     
         services.AddControllers(setupAction =>
         {
             setupAction.ReturnHttpNotAcceptable = true;
         })
     }
     
     public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
     {
     	// 你在哪？
         app.UseRouting();
         // 你是谁？
         app.UseAuthentication();
         // 你可以干什么？有什么权限？
         app.UseAuthorization();
         
         app.UseEndpoints(endpoints =>
         {   
             // MVC路由映射中间件 使用MVC的映射代替直接对路由进行映射
             endpoints.MapControllers();
         });
     }
     ```

- ##### 用户登录，获取JWT Token

  ```c#
  [ApiController]
  [Route("auth")]
  public class AuthenticateController : ControllerBase
  {
      private readonly IConfiguration _configuration;
      // Identity框架 SignInManager ，UserManager
      private readonly UserManager<ApplicationUser> _userManager;
      private readonly SignInManager<ApplicationUser> _signInManager;
      
      public AuthenticateController(
          IConfiguration configuration,
          UserManager<ApplicationUser> userManager,
          SignInManager<ApplicationUser> signInManager)
      {
          _configuration = configuration;
          _userManager = userManager;
          _signInManager = signInManager;
      }
      
      // AllowAnonymous 允许任何人访问
      [AllowAnonymous]
      [HttpPost("login")]
      public async Task<IActionResult> login([FromBody] LoginDto loginDto)
      {
          // 1 验证用户名密码
          // 使用SignInManager 验证用户名和密码
          // 函数参数1：用户名， 参数2：密码， 参数3,4：如果尝试多次登录失败，要不要把账号锁起来
          var loginResult = await _signInManager.PasswordSignInAsync(
              loginDto.Email,
              loginDto.Password,
              false,
              false);
          if (!loginResult.Succeeded)
          {
              return BadRequest();
          }
          var user = await _userManager.FindByNameAsync(loginDto.Email);
          
          // 2 创建jwt
          // 第一部分 header
          // 定义了Token编码的算法
          var signingAlgorithm = SecurityAlgorithms.HmacSha256;
          
          // payload
          // claims可以理解为JWT自定义的payload数据
          // claim是资源的所有权。在用户授权中，可以表述用户的身份，说明用户的角色，表示用户所具有的权限。
          // claim是用户授权中，最小不可分割单位，使用的灵活度相当高，可以自由绑定组合，甚至是互斥
          var claims = new List<Claim>
          {
              // 关于用户ID的Claim在JWT中称为Sub
              // 第一个参数为用户的ID，第二个参数是用户ID的准确信息
              new Claim(JwtRegisteredClaimNames.Sub, user.Id)
          };
          
          //获取用户的角色
          var roleNames = await _userManager.GetRolesAsync(user);
          
          // 用户角色列表转化为Claim
          foreach (var roleName in roleNames)
          {
              // 为每个角色创建Claim
              var roleClaim = new Claim(ClaimTypes.Role, roleName);
              claims.Add(roleClaim);
          }
          
          // signiture 数字签名
          // 私钥可以保存在配置文件中
          // 将私钥根据UTF8编码输出字节
          var secretByte = Encoding.UTF8.GetBytes(_configuration["Authentication:SecretKey"]);
          
          // 使用非对称算法对私钥进行加密
          var signingKey = new SymmetricSecurityKey(secretByte);
          
          // 使用设置的HS256算法来验证非对称加密后的私钥
          var signingCredentials = new SigningCredentials(signingKey, signingAlgorithm);
          
          // 创建JWT Token
          var token = new JwtSecurityToken(
              // issuer 谁发布的这个Token
              issuer: _configuration["Authentication:Issuer"],
              // audience 这个Token将发布给谁
              audience: _configuration["Authentication:Audience"],
              // payload数据，Claims数组
              claims,
              // 发布时间
              notBefore: DateTime.UtcNow,
              // 有效期
              expires: DateTime.UtcNow.AddDays(1),
              // 数字签名
              signingCredentials
          ) ;
          
          // JwtSecurityTokenHandler 以字符串的形式输出Token
          var tokenStr = new JwtSecurityTokenHandler().WriteToken(token);
          // 3 return 200 ok + jwt
          return Ok(tokenStr);
      }
  }
  ```

- ##### 添加API授权

  ```c#
  [HttpPost(Name = "CreateTouristRoute")]
  // 除非登录用户否则无法访问
  // 当我们使用Identity框架进行多角色验证的时候，验证中间件并不是JWT Token，所以必须在Action函数上显式的指定使用JWT Bearer的方式验证
  [Authorize(AuthenticationSchemes = "Bearer")]
  // 添加角色
  [Authorize(Roles = "Admin")]
  public async Task<IActionResult> CreateTouristRoute(
      [FromBody] TouristRouteForCreationDto touristRouteForCreationDto){}
  ```

- ##### 获取登录用户信息

  ```c#
  public class ShoppingCartController : ControllerBase
  {
      // Http上下文关系对象
      private readonly IHttpContextAccessor _httpContextAccessor;
      public ShoppingCartController(IHttpContextAccessor httpContextAccessor)
      {
          _httpContextAccessor = httpContextAccessor;
      }
      public async Task<IActionResult> GetShoppingCart()
      {
          // FindFirst函数参数中使用JWT中定义的用户ID
          var UserId = _httpContextAccessor.HttpContext.User.FindFirst(ClaimTypes.NameIdentifier).Value;
          // ............
      }
  }
  ```







#  14. 内容协商

- ##### 内容协商与数据格式

  允许客户端和服务器通过协商来决定互相之间的数据传输格式，语言等内容

  请求头部的媒体类型定义"accept"和"Content-type"。ASP.NET Core支持内容协商，自动化处理

  如果前端请求的格式后端无法识别的时候，返回错误代码406 unacceptable

- ##### 处理不支持的请求数据格式

  - Startup.cs

    ```c#
    public void ConfigureServices(IServiceCollection services){
        services.AddControllers(setupAction =>
    	{
            // ReturnHttpNotAcceptable：false 忽略请求头部MediaType，默认Json格式
            setupAction.ReturnHttpNotAcceptable = true;
        })
        // 3.0之前设置XML输出格式：为控制器添加对XML的支持，可以生产以XML为结构的数据
        // setupAction.OutputFormatters.Add(
        //     new XmlDataContractSerializerOutputFormatter()
        // );
        // 3.0之后： AddXmlDataContractSerializerFormatters 设置无论是Input还是Output的所有跟XML有关的配置
    	.AddXmlDataContractSerializerFormatters();
    }
    ```



# 15. HATEOAS

### 15.1. 概述

- ##### HATEOAS 超媒体即应用状态引擎

  就是在响应中包含其他资源的链接。客户端可以用这些连接和服务器交互，在处理前后端交互的时候，客户端不需要提前知道服务或者是工作流程的步骤，不需要再为不同的资源提前准备好URL，甚至可以在不破坏与服务器交互的情况下更改URL。

  打破了客户端和服务器之间严格的契约，客户端可以更加智能的自适应，而REST服务本身的演化和更新也变得相对更加容易了

- ##### HATEOAS 示例

  ```json
  {
      "id": 1,
      "title": "自由行",
      "price": "5999.8",
      "Links": [ 
          {
              "href": "https://localhost:3001/api/TouristRoutes/1",
              "rel": "self",
              "method": "GET"
          },
          {
              "href": "https://localhost:3001/api/TouristRoutes/1",
              "rel": "更新",
              "method": "PUT"
          },
          {
              "href": "https://localhost:3001/api/TouristRoutes/1",
              "rel": "局部更新",
              "method": "PATCH"
          },
          {
              "href": "https://localhost:3001/api/TouristRoutes/1",
              "rel": "删除",
              "method": "DELETE"
          },
      ]
  }
  ```

- ##### Link 连接信息

  link是HATEOAS的核心

  href：用户可以用来检索资源或者改变应用状态的URL

  rel：用来描述资源和url的关系。比如，”self“表示了url自我描述的关系

  method：访问这个URL需要的http方法

  ```c#
  public class LinkDto
  {
      public string Href { get; set; }
      public string Rel { get; set; }
      public string Method { get; set; }
  
      public LinkDto(string href, string rel, string method)
      {
          Href = href;
          Rel = rel;
          Method = method;
      }
  }
  ```


### 15.2. 响应主体添加连接信息

  - ##### 添加供应商特定媒体类型

    application/vnd.mycompany.hateoas+json

    ```c#
    public void ConfigureServices(IServiceCollection services)
    {
    	// 全局注册自定义媒体
    	services.Configure<MvcOptions>(config => {
    	    // 输出格式媒体处理器
    	    var outputFormatter = config.OutputFormatters
    	    .OfType<NewtonsoftJsonOutputFormatter>()?.FirstOrDefault();
    
    	    if (outputFormatter != null)
    	    {
    	        outputFormatter.SupportedMediaTypes
    	        .Add("application/vnd.aleks.hateoas+json");
    	    }
    	});
    }
    ```

  - ##### 响应主体添加连接信息

    ```c#
    public async Task<IActionResult> GerTouristRoutes(
        [FromQuery] TouristRouteResourceParamaters paramaters,
        FromHeader(Name ="Accept")] string mediaType   // 从请求头部获得媒体类型
    )
    {
        // 解析头部媒体类型
        // 如果有可能请求头部信息"ACCEPT"不止一个，这时需要使用TryParseList
    	// MediaTypeHeaderValue 基于 Microsoft.Net.Http.Headers
        if (!MediaTypeHeaderValue.TryParse(mediaType, out MediaTypeHeaderValue parseMediatype))
        {
            return BadRequest();
        }
        
        // 取得数据省略
        
        if (parseMediatype.MediaType == "application/vnd.aleks.hateoas+json")
        {
            var linkDto = CreateLinks(paramaters);
            
            var result = new
             {
                 value = List,
                 links = linkDto
             };
    
             return Ok(result);
        }
        
        return Ok(List);
    }
    ```




# 16. 部署

### 16.1 ISS服务器托管

- ##### 安装启用ISS服务

  启用服务：Windows功能(optionalfeatures)中，找到并勾选Internet Information Services，重启电脑

  查看：打开 Internet Information Services(IIS)管理器，找到网站文件夹

- ##### 配置asp.net托管服务

  下载安装 asp.net core runtime windows hosting bundle

  点击IIS管理器的模块选项，其中AspNetCoreModuleV2就是托管组件

- ##### 发布项目

  保存文件夹：C:\inetpub\wwwroot\项目名称

  发布：在vs中发布项目，所有编译后的内容都复制到上述文件夹中

- ##### 部署网站

  IIS管理器 中 右键点击网站文件夹，选择添加网站

### 16.2 容器化部署

- ##### Docker的核心概念

  1. Registry镜像仓库
  2. Image镜像
  3. Container容器

  镜像仓库 ==  拉取镜像 docker pull ==>  镜像  ==  容器化部署 docker run ==> 容器

  镜像仓库 <==  推送镜像 docker push ==  镜像  <==  保存镜像 docker commit == 容器

- ##### 添加Docker支持

  vs中右键项目名称，选择添加，选择添加Docker。在项目根目录会生产一个Dockerfile的文件。因为Dockerfile已经在当前项目路径中了，所以需要修改Dockerfile中不必要的路径。

  要注意：如果数据库连接字符串地址为localhost，需要更换到docker中数据库运行的IP地址

  构建对象：
  
  ```bash
  #构建项目
  docker build -t myapi .
  
  #查看当前所有的镜像
  docker images
  
  #容器化镜像
  docker run -d -run myapi -p -8080:80 myapi
  
  #检查当前所有启动的容器
  docker ps
  
  #检查当前所有的容器
  docker ps -a
  
  #检查日志
  docker logs myapi
  
  #查找IP地址
  docker inspect bridge
  
  #关停容器
  docker stop {container id} 
  
  #开启容器
  docker start {container id} 
  
  #删除容器
  docker rm {container id}
  
  #删除镜像
  docker rmi {container id}
  ```
  

### 16.3 部署阿里云

1. 启动ECS

2. 安装配置Docker

   ```bash
   #准备工作
   yum update
   yum install epel-release -y
   yum clean all
   yum list
   
   #安装Docker
   yum install docker-io -y
   ```

3. 启用服务

   ```bash
   #启动Docker
   systemctl start docker
   
   #查看docker安装结果
   docker info
   ```

4. 容器化部署SQL Server

   ```sh
   #下载SQL Server 镜像
   docker pull microsoft/mssql-server-linux
   
   #运行SQL Server 镜像
   docker run -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=PaSSword12!' -p 1433:1433 -d microsoft/mssql-server-linux
   
   #查看运行状况
   docker ps
   ```

5. 远程初始化数据库

   1. 添加SQL Server的访问规则，复制公网的可访问IP地址，修改本地项目中的数据库连接字符串地址
   2. 在本地初始化数据库 dotnet ef database update

6. 在本地构建镜像

   ```bash
   #构建镜像
   docker build -t myapi .
   
   #查看镜像
   docker images
   ```

7. 搭建私有镜像仓库

   创建镜像仓库，代码源选择本地仓库

8. 推送镜像

   根据镜像仓库的操作指南

9. 拉取并部署镜像

   根据镜像仓库的操作指南

10. 测试









 

















































