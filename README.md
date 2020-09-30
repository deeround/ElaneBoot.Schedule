# ElaneBoot.Schedule

一个Quartz.NET集成UI的类库。

## Quartz.NET

Quartz.NET是NET的开源作业调度系统。

Quartz.NET是一个功能齐全的开源作业调度系统，可用于从最小的应用程序到大型企业系统。

Quartz.NET目前支持NETFramework和NETCore。

Quartz.NET中文文档完善 [文档地址](https://www.quartz-scheduler.net/documentation/quartz-3.x/quick-start.html#download-and-install)

## Quartz.NET集成UI版

目前开源作业调度系统还有Hangfire可以选择。

其他开源作者制作的带UI的Quartz.NET系统。

## 系统特点

将UI资源文件作为嵌入式资源集成在项目中，直接引用一个包就可以了，不管项目升级还是使用做到更简单。

增加了常用作业添加、删除、修改、停止、启动、日志功能。

## 开发技术

- Visual Studio 2019
- .NET Core 2.1
- Quartz 3.0.7
- Razor Page
- Bootstrap

## 开箱即用

开箱即用，几乎不用编码。

持久化支持多种数据库，自动创建表结构，默认内置SQLite数据库。

目前Job实现了HttpJob定时调用API完成任务的执行。

## 使用步骤

新增一个NET Core 2.1的web项目，添加ElaneBoot.Schedule的nuget包引用。

修改Startup.cs

~~~ c#
 public class Startup
    {
        public Startup(IConfiguration configuration)
        {
            Configuration = configuration;
        }

        public IConfiguration Configuration { get; }

        // This method gets called by the runtime. Use this method to add services to the container.
        public void ConfigureServices(IServiceCollection services)
        {
            services.AddApiSchedule(Configuration);
            services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
        }

        // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
        public void Configure(IApplicationBuilder app, IHostingEnvironment env)
        {
            if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
            }

            app.UseMvc();
            app.UseApiSchedule(Configuration);
        }
    }
~~~

访问http://localhost:端口/Schedule

## 数据库配置

持久化支持多种数据库，自动创建表结构，默认内置SQLite数据库。

示例：切换数据库为Oracle

添加Oracle数据库驱动Oracle.ManagedDataAccess.Core的nuget包引用

修改appsettings.json

~~~ json
{
  "Logging": {
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "AllowedHosts": "*",
  "Database": [
    {
      "Name": "Oracle",
      "ConnectionString": "Data Source=(DESCRIPTION =(ADDRESS = (PROTOCOL = TCP)(HOST = 数据库IP)(PORT = 1521))(CONNECT_DATA =(SERVER = DEDICATED)(SERVICE_NAME = 数据库实例名)));User ID=用户名;Password=密码;",
      "ConnectionType": "Oracle.ManagedDataAccess.Client.OracleConnection,Oracle.ManagedDataAccess",
      "UseParameterPrefixInSql": true,
      "UseParameterPrefixInParameter": true,
      "ParameterPrefix": ":",
      "UseQuotationInSql": false,
      "Debug": false
    }
  ]
}

~~~

