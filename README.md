MyBookStore (ABP 8.3.4 + Pure Admin 后端)
本项目是基于 ABP Framework 8.3.4 构建的后端服务，采用领域驱动设计（DDD）架构，并为对接 Pure Admin (Vue3) 进行了配置优化。

🛠 开发环境要求
SDK: .NET 8.0+

工具:

ABP CLI (dotnet tool install -g Volo.Abp.Cli)

EF Core Tools (dotnet tool install -g dotnet-ef --version 8.0.0)

数据库: SQL Server (实例名: 127.0.0.1)

🚀 快速启动指南
1. 配置数据库连接
请修改以下两个项目中的 appsettings.json：

src/MyBookStore.DbMigrator/appsettings.json

src/MyBookStore.HttpApi.Host/appsettings.json

确保 ConnectionStrings:Default 指向您的 SQL Server 实例：

JSON
"Default": "Server=127.0.0.1;Database=MyBookStore;Trusted_Connection=True;TrustServerCertificate=True"
2. 初始化数据库与种子数据
在根目录下执行：

Bash
cd src/MyBookStore.DbMigrator
dotnet run
注意: 如果遇到 dotnet-ef 报错，请参考下方 常见问题 部分。

3. 启动后端 API
Bash
cd src/MyBookStore.HttpApi.Host
dotnet run
成功后访问：https://localhost:xxxx/swagger (端口号见 launchSettings.json)。

默认账号: admin

默认密码: 1q2w3E*

🎨 对接 Pure Admin 配置
1. 跨域设置 (CORS)
打开 src/MyBookStore.HttpApi.Host/appsettings.json，在 App:CorsOrigins 中添加前端地址：

JSON
"App": {
  "CorsOrigins": "http://localhost:8848,http://localhost:5173"
}
2. 身份验证
Pure Admin 需要通过调用以下接口获取令牌：

Token 接口: /connect/token

Grant Type: password (开发环境常用) 或 authorization_code

📂 项目结构说明
.Domain: 核心业务实体（Entity）与业务规则。

.EntityFrameworkCore: 数据库上下文、模型映射及迁移脚本。

.Application: 业务逻辑实现与 DTO 转换。

.HttpApi.Host: RESTful API 宿主程序，集成 Swagger 与 OpenIddict 授权。

.DbMigrator: 数据库管理工具（迁移与初始化种子数据）。

🔧 常见问题修复
关于 dotnet-ef 安装失败
如果 DbMigrator 报错找不到 dotnet-ef 或包损坏，请手动清理并重新安装：

dotnet tool uninstall -g dotnet-ef

dotnet nuget locals all --clear

dotnet tool install -g dotnet-ef --version 8.0.0

验证：输入 dotnet ef 看到独角兽图标即成功。

📈 后续开发路线
[ ] 在 .Domain 定义第一个业务实体（如 Book.cs）。

[ ] 使用 dotnet ef migrations add 生成数据库迁移。

[ ] 在 .Application 中通过继承 CrudAppService 快速暴露 API。

[ ] 在 Pure Admin 中封装 Axios 以对接 ABP 自动生成的动态 API。