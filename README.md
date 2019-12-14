# 纯净 Android 公约 V0.8   
  
#### 公约仍处于初步阶段，欢迎提交各种修改建议。（建议直接使用 Pull request 提交）  
#### 官方app已更新！  
  
[申请加入公约][] | [公约成员][] | [开发者Wiki][] | [文章列表][] | [SDK黑名单][]  
[公约官方 App（亦为公约范例）][] | [琴梨梨的小站][]  
  
## 0x0 总章  
0. 纯净 Android 公约旨在规范（而非限制）Android app 开发，改善 Android 生态环境和用户体验。  
1. 纯净 Android 公约为开发者自愿加入，开发者可为其一个或多个 app 加入，也可在加入后随时选择退出。  
2. 只有完全符合公约的 App 可以加入本公约。  
3. 加入公约后， App 可在任意位置注明已加入本公约，当然也可以不注明。  
4. 公约会伴随技术发展不断调整，若 App 未能跟进公约的更新，会被移出公约。  
5. __在编号后标有`*`的为建议支持特性，这些特性可以有效改善 App 性能和用户体验，但不支持也不会影响加入公约。__  
  
## 0x1 项目主体  
0. 必须设置至少 26 的 target sdk。（针对通过逆向修改的官方弃坑类 App 可放宽至 23）  
1. 如果 App 存在 `native lib`，必须提供 64 位 `lib`。（针对目前没有 64 位的 Unity 游戏可放宽）  
2. 安装包不得存在任何形式的加固或其他影响性能的安全措施。（推荐使用较为复杂的混淆保障安全）  
3. 安装包必须已经对齐。（`zipalign`）  
4. 大于1M的安装包使用 v2 或 v3 签名。（这有助于改善安装速度）  
5. 安装包的图标必须适配自适应图标。  
6. 若项目针对设备包括 7.x 设备，应适配圆形图标。（对于 8.0 以上设备，自适应图标亦可单独设计一个圆形版本，但在图标尺寸设计合理的情况下不是必须的）  
  
## 0x2 推送  
0. 推送必须有开关，用户应能自行选择是否接收推送。  
1. 关闭推送后不得在后台保留推送服务。  
2. 在能使用系统级统一推送服务（gcm，推必达等）时不得在后台保留推送服务。  
3. 推送服务所属进程必须为独立的子进程。  
4. 推送通知的图标不得出现“大黑块”。（在原生UI上，推送图标是单色的，所以推送图标不应该添加蒙板）  
5. 若你的推送内容有多个类别，必须适配 `Notification Channel`。

## 0x3 数据收集  
0. 数据收集包括对设备信息，App 使用记录，网络信息，错误日志等的收集。`Bugly`,`Crashlytics` 等错误收集服务属于数据收集。  
1. 数据上传必须有开关，用户应能自行选择是否上传数据，若用户拒绝上传数据，数据仅能存储在本地。  
2. App 仅收集自身数据，不得收集其他无关数据，如设备已安装 App 列表等。  
3. 数据上传服务仅允许在 App 前台运行时上传数据。  
4. 除非得到用户允许，数据上传服务仅允许使用 WiFi 上传数据，不得消耗用户流量。  

## 0x4 权限  
0. 不得申请超越 App 功能的权限。  
1. 不得为了获取 IMEI 而申请电话权限。（推荐读取设备 id 和谷歌框架 id 代替）  
2. 用户拒绝权限不得影响其他无需该权限功能的运行。  
3. 仅允许在用户打开需要权限的功能时申请权限，不得在启动时一次性申请全部权限（除非该 app 所有功能都需要全部权限）。  
4. 项目 `manifest` 中不得出现重复的权限。（建议在使用较多 sdk 的项目中留心剔除重复权限）  
5. 项目 `manifest` 中不得出现根本没有用到的权限。  
6. 在申请权限时，应向用户说明权限用途。  
  
## 0x5 文件存储  
0. 对于主动的共享文件，使用 `FileProvider` 而不是直接传递文件 `Uri`。  
1. 对于无需共享的文件，存储于内置或外置 data 目录内。  
2. 对于缓存文件，存储于内置或外置 cache 目录内。  
3. 对于被动的共享文件（如保存图片），应在外置公共存储内以易辨识的目录命名存储，不得直接存储于根目录下，单个 App 仅允许在根目录下建立一个子文件夹，若需要额外分类，请在子文件夹内再建立子文件夹。  
4. 对于不是缓存的可清理文件，app 应提供清理功能。（自动清理或允许用户手动清理）  
5. `*`提高缓存的复用率，不要缓存仅会使用一次的内容。  
  
## 0x6 自启动与后台  
0. 若 App 需要自启动，必须提供开关，用户应自行选择是否允许自启动。  
1. 不得在非用户允许场景下自启动。  
2. 在没有运行功能的情况下，后台不得消耗 cpu 资源。（后台静置 12 小时，cpu 时间应小于 0:05）  
3. 非功能必须（如音乐播放，上传下载等）不得申请常驻后台。    
  
## 0x7 功能特性  
1. 对于上架 Google Play 的 app，使用分割（`split`）。（这有助于减少用户下载消耗的流量）  
2. 尽量避免内存泄漏。尤其针对`native`层，必须做好内存回收机制。  
3. `*`对于文档类和聊天类功能，支持 `documentLaunchMode` 多窗口。  
4. `*`对于除了游戏以外的 App，建议支持分屏。  
5. `*`为功能提供开关，允许用户关闭一些用不到的功能。  
6. `*`尽可能支持多核处理，充分发挥设备性能。  
7. `*`合理使用子线程异步处理，避免在主线程上长时间处理任务而触发 ANR。  
8. 除了浏览器可以使用内置 Webview 内核外，使用系统 WebView 内核而不是 uc 或 x5 内核。  
  
## 0x8 App 资源文件
0. `*`对图片等资源进行无损压缩。  
1. `*`使用 `Vector` 矢量代替传统位图。  
2. `*`逐步用 `webp` 取代 `png`。  
3. `*`对于大量使用的图标或商标等资源，使用含有特殊图案的字体文件代替传统位图。  
  
## 0x9 广告  
0. 所有广告必须提供关闭方式。  
1. 广告不得遮挡任何按钮。  
  
## 0xa 显示  
0. 兼容全面屏等各种比例屏幕。  
1. 支持横屏，横屏下不得出现按钮遮挡或比例异常等问题。  
2. 在平板电脑等大尺寸屏幕设备上显示内容不变形，模糊，错位等导致不美观甚至影响使用。  
3. `*`兼容自由式多窗口模式。  
4. 视频类应用支持 `Miracast`。  
5. 视频类应用支持画中画模式（指8.0+系统自带画中画）。  
6. 使应用界面与导航栏，通知栏衔接自然。  

[申请加入公约]: HowToApply.md
[公约成员]: ApprovedList.md
[开发者Wiki]: https://github.com/qinlili23333/PureAndroid/wiki
[文章列表]: article/list.md
[SDK黑名单]: SDKblacklist.md
[公约官方 App（亦为公约范例）]: https://github.com/qinlili23333/PureAndroid/releases/tag/Apk
[琴梨梨的小站]: https://qinlili.bid
