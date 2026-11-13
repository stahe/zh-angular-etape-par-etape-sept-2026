# Angular 框架分步入门指南

📖 **阅读教程：[https://stahe.github.io/zh-angular-etape-par-etape-sept-2026/](https://stahe.github.io/zh-angular-etape-par-etape-sept-2026/)**

本课程将教你如何使用 [Angular](https://angular.dev) 22 框架构建一个 Web 应用程序：一个 **单页应用程序**（SPA），其页面通过服务器提供的 JSON 数据在 **浏览器中** 渲染。

本课程是[《NestJS Web 框架分步入门》](https://stahe.github.io/zh-nestjs-html-sept-2026/)的后续课程，提供了不同的视角，并遵循[《Vue.js 框架分步入门》](https://stahe.github.io/ vuejs-etape-par-etape-sept-2026/): 采用相同的服务器和页面，但采用 Angular 风格编写。

| NestJS 课程 | Angular 课程 |
|---|---|
| 服务器生成 HTML 页面（Handlebars） | 浏览器生成页面（Angular） |
| 浏览器显示接收到的内容 | 服务器仅返回 JSON |
| 控制器、视图、`res.render` | 组件、路由器、服务 |
| 守护器 `JwtAuthGuard`、`RolesGuard` | 路由器守护器（服务器维护自己的） |
| 由服务器读取的字典 | 客户端的字典（ngx-translate）；服务器仅返回键 |
| cookie 中的闪烁消息 | 信号服务中的闪烁消息 |

不过，页面保持不变：它们与之前使用其他框架介绍过的 **RdvMedecins** 应用程序中的页面完全相同。

## 教学方法：先看多个简短示例，再进行案例研究

由于 Angular 的功能比 Vue.js 更丰富，本课程围绕 **25 个简短示例** 展开，每个示例专注于一个概念。它们共同构成一个 Angular 工作区：只需执行一次 `npm install` 命令，随后输入 `npm start <示例>` 即可运行其中一个示例。

| 章节 | 内容 | 示例 |
|---|---|---|
| 入门 | Angular 工作区、信号（`signal`、`computed`、`effect`、`linkedSignal`）、模板（`@if`、`@for`、`@let`）、事件、`ngModel`、信号驱动表单、验证、响应式表单、管道 | 01–09 |
| 组件 | `input()`、`output()`、`model()`、内容投影、生命周期、服务与依赖注入、指令、确认对话框 | 10–16 |
| 路由 | 路由器、参数、查询、延迟加载、导航守护 | 17–18 |
| 可观察对象与共享状态 | RxJS（`debounceTime`、`switchMap`、`toSignal`）、信号服务、`localStorage` | 19–20 |
| 国际化 | ngx-translate：参数、复数形式、日期、金额 | 21 |
| 服务器：一个黑匣子 | JSON 服务器安装、其 API、48 个 `curl` 示例 | – |
| 与服务器的通信 | `HttpClient`、`ng serve` 代理、`httpOnly` Cookie、拦截器、API 访问层、`httpResource`、附加到字段的服务器错误 | 22–25 |

每个示例均附有完整的代码、逐行注释以及执行截图。

## 服务器：一个黑盒子

该服务器即上一课中的 NestJS 服务器，其控制器返回 **JSON**——与 Vue.js 客户端相同。本课将其视为一个 **黑盒**：我们安装它、研究其 API，并使用 `curl` 向其发送请求——但无需阅读其源代码（对于好奇的读者，源代码已提供并附有注释）。

- 所有错误都采用相同的格式：`{ "statusCode": 409, "cle": "ERRORS. LOGIN_TAKEN", "params": {...}, "champs": {...} }` ——仅显示**键**，绝不显示实际文本；
- 通过 `httpOnly` / `sameSite=strict` Cookie 中的 JWT 令牌进行身份验证：客户端 JavaScript 永远无法看到该令牌；
- CSRF 保护：`sameSite` Cookie，且所有 POST 请求必须采用 JSON 格式；
- CAPTCHA 提供“测试模式”，允许使用 `curl` 查询 API。


## 案例研究：RdvMedecins Angular 客户端

一个用于 **在医疗机构预约就诊** 的完整应用程序，附带 **所有** 文件（约 50 个）的列表及注释。

- **现代 Angular**：独立组件、信号、无需 zone.js 的变更检测、基于信号且带有自定义控件（`FormValueControl`）的表单，以及按需加载的页面。
- **三种角色**：`ADMIN`（管理医生和客户）、`DOCTOR`（安排和取消预约）、`USER`（患者：为自己预约，管理个人账户）。
- **隐私保护**：患者绝不会获知其他患者的姓名——服务器不会发送此类信息。
- **URL 中包含完整页面状态**： `/agenda?idMedecin=1&jour=2026-10-05&reserver=7` 在按下 F5 后会重新打开预约窗口；“上一页/下一页”按钮可正常使用。
- **服务器端验证**：表单在每个字段下方显示 API 返回的错误信息；包括乐观锁定、姓名重复、登录名已被占用等。
- **会话**： 按 F5 后恢复（`GET /api/auth/moi`）；过期管理集中于单一位置（HTTP 拦截器，401 响应）。
- **法语 / 英语**，已翻译的标签页标题，支持键盘操作的确认窗口，Bootstrap 5。
- **部署**：编译后的客户端由 JSON 服务器本身提供（同源，无需 CORS）。

## 技术栈

Angular 22.2（独立组件、信号、信号驱动表单、无区域模式）· TypeScript 6.0 · Angular CLI · RxJS 7.8 · ngx-translate 18 · Bootstrap 5.3 · 服务器端：NestJS 10 · TypeORM · MySQL 8 / MariaDB · Passport JWT · svg-captcha

## 先决条件

- 具备 JavaScript（或 TypeScript）、HTML 及 HTTP 协议的基础知识。
- Node.js 24（或至少 22.22.3）、安装了 Angular Language Service 扩展的 Visual Studio Code，以及用于运行 JSON 服务器的 MySQL 服务器（例如 Windows 上的 Laragon）。安装说明详见课程附录。

## 作者

本课程、其中的示例、JSON 服务器以及案例研究均由 [Anthropic](https://www.anthropic.com) 的 AI **Claude** 应 **Serge Tahé** 的请求编写（2026 年 9 月）
