# ORM工具

## 安装ef工具
``` 
// 安装最新版
dotnet add package Microsoft.EntityFrameworkCore

// 自选版本在后面加 -v <版本号>
dotnet add package Microsoft.EntityFrameworkCore -v 3.1

// 安装 EF Core 工具
dotnet tool install --global dotnet-ef
```

## 迁移
```
// 创建新迁移
dotnet ef migrations add <迁移名>

// 让 EF 创建数据库并从迁移中创建架构
dotnet ef database update
```

dotnet ef migrations add 
```
    -o|--output-dir <PATH>                 存放文件的目录。路径是相对于项目目录的。默认为 Migrations
    --json                                 JSON输出。使用 --prefix-output以编程方式解析
    -n|--namespace <NAMESPACE>             要使用的名称空间。默认情况下匹配目录
    -c|--context <DBCONTEXT>               要使用的数据库上下文
    -p|--project <PROJECT>                 要使用的项目。默认为当前工作目录
    -s|--startup-project <PROJECT>         要使用的启动项目。默认为当前工作目录
    --framework <FRAMEWORK>                目标框架。默认为项目中的第一个
    --configuration <CONFIGURATION>        要使用的配置
    --runtime <RUNTIME_IDENTIFIER>         要使用的运行时
    --msbuildprojectextensionspath <PATH>  MSBuild 项目扩展路径。默认为 obj
    --no-build                             不要构建项目。打算在构建是最新的时候使用
    -h|--help                              显示帮助信息
    -v|--verbose                           显示详细的输出
    --no-color                             不要再着色输出
    --prefix-output                        Prefix output with level
```

现在学的 Web Api 和以前的 MVC 不同，**模型**和**数据库上下文**都在新建的类库里，所以迁移时要引用项目  
```
dotnet ef migrations add <迁移名> -s <path>

dotnet ef database update -s <path>
```