# 使用

- [启动工具](#启动工具)

- [工具交互](#工具交互)

- [预置功能](#预置功能)

- [配置属性](#配置属性)

- [脚本概述](#脚本概述)

- [脚本拓展](#脚本拓展)

- [命令行传参](#命令行传参)

## 启动工具

将需要处理的文件对象 **转发** 给工具的启动脚本 `launch.[cmd|sh]` ，工具启动后，会根据该文件对象的类型列出可用功能表，用户输入需要应用的功能序号即可。

> **转发** 即以文件对象的路径作为参数启动工具。

在 Windows 系统上，还可通过下列几种 **快速启动** 方式将需要处理的文件对象转发给工具。

*  打开方式
	
	对于特定类型的文件，可以将 `launch.cmd` 作为该类型文件的打开方式，系统会将所选文件转发给工具。
	
	选择设置打开方式的文件类型，唤出右键菜单，依次选择 ⌈ 打开方式 ⌋ - ⌈ 查找其他应用 ⌋ - ⌈ 更多应用 ⌋ - ⌈ 在这台电脑上查找其他应用 ⌋ ，在弹出的窗口内进入主目录，并选择其中的 `launch.cmd` 。
	
	将启动脚本设置为该类型文件的 ⌈ 默认打开方式 ⌋ 后，双击该类型文件即可将之转发给工具。

* 拖拽打开
	
	将需要处理的文件或目录拖拽至主目录内的 `launch.cmd` 文件图标上释放，这相当于以 `launch.cmd` 作为该文件或目录的打开方式。
	
	方便起见，可在桌面上创建一个快捷链接指向 `launch.cmd` 。

* 资源管理器拓展
	
	安装 `Windows Explorer Extension` 后，选择任意文件或目录并唤出右键菜单，会出现 `⌈ TwinStar ToolKit ⌋` 选项菜单，它为大多数功能提供了快捷入口。
	
	选项菜单中的首项 `⌈ Launch ⌋` 是工具的启动入口，点击后系统会将所选文件或目录转发给工具。
	
	选项菜单中的其他项则是各项功能的直接入口，点击后可直接对所选文件或目录执行对应的功能，而无需在工具启动后手动输入功能序号，更为便捷。

## 工具交互

工具运行时会向用户输出消息，或请求用户输入一些参数。

### 输出

不同类型的通知具有不同颜色，并以相应颜色的实心圆图标作为标志图标：

* `⚪` 常规（暗色主题）

* `⚫` 常规（亮色主题）

* `🔵` 信息

* `🟡` 警告

* `🔴` 错误

* `🟢` 成功

* `🟣` 输入

### 输入

六种通知类型中，`🟣` 表示工具向用户请求输入参数，工具将停止其他工作并等待用户输入，直到用户输入完毕后键入回车以结束该行输入。

请求输入时，还会显示一个前导字符作为参数类型的提示，输入值类型及格式如下：

* `C` 确认值
	
	单个字符 ⌈ y ⌋ 或 ⌈ n ⌋ ，表示 ⌈ 是 ⌋ 与 ⌈ 否 ⌋ 。

* `N` 数字
	
	一个十进制数字。

* `I` 整数
	
	一个十进制整数，不可包含小数点。

* `Z` 尺寸
	
	一个无符号十进制整数，后跟随一个二进制单位（ b = 2^0 , k = 2^10 , m = 2^20 , g = 2^30 ），一般用于表示存储容量。
	
	> 例如 ⌈ 4k ⌋ 表示 4096 字节的存储容量。

* `O` 选项
	
	通过整数序号选择一个可选项。

* `S` 字符串
	
	一行文本。

* `S` 输入路径
	
	指向磁盘上一个已存在的文件或目录的路径。
	
	> 也可输入 `:s` 唤起文件选择窗口（仅限 `Windwos CLI` ）。
	> 
	> 如果在输入了一段相对路径，则该路径是相对于工具的工作目录 `+ <home>/workspace` 计算的。

* `S` 输出路径
	
	指向磁盘上一个不存在的路径。工具一般会自动生成默认输出路径，但在默认输出路径已被占用时会请求输入新的输出路径。
	
	> 也可输入 `:t` 移动已有文件至工具的回收目录 `+ <home>/trash` ；\
	> 或是输入 `:d` 删除已有文件；\
	> 或是输入 `:o` 覆写已有文件。
	> 
	> 如果在输入了一段相对路径，则该路径是相对于工具的工作目录 `+ <home>/workspace` 计算的。

## 预置功能

默认情况下，工具已经提供了丰富的功能，如 RTON 编解码、RSB 解包，详见 [功能](./method.md) 章节。

## 配置属性

在脚本目录 `+ <home>/script` 中，有些脚本文件拥有配套的配置文件，其名称与对应的脚本文件相同，但以 `.json` 为拓展名。

例如，脚本目录下的一个脚本 `- Entry/Entry.js` 拥有配置文件，配置文件为 `- Entry/Entry.json` 。

配置属性会影响工具的某些行为，如 JSON 的输出格式。

用户可以自行修改配置文件，各个配置文件的修改规则，在 [功能](./method.md) 章节中有详细的说明。

> 配置文件描述了一个 JSON 对象，该对象的类型由对应的脚本定义，可以在配套脚本源项目中查看对应的 `.ts` 源码。

> **对于大多用户，预置功能已经足够使用，不必往下看；但如果你希望通过自定义脚本来提高你的工作效率，可以继续。**\
> **（以下内容面向有编程基础的用户。）**

## 脚本概述

本工具以 `TypeScript` 作为脚本语言。JS 引擎为第三方开源项目 `quickjs` 。

简单来说，外壳 `Shell` 所做的工作只有两件：

1. 加载 ⌈ 核心库 ⌋ ，其中了定义了面向 JS 的 `Core interface` 。

2. 执行 ⌈ 主脚本 ⌋ ，其路径通过启动脚本被设定为 `- <home>/script/main.js` 。

工具的各类功能都是由脚本以直接或间接调用 `Core interface` 的方式提供给用户的，它提供了文件读写、数据操作等基本功能。

> `Core interface` 的接口具有严格的类型限制，因此，在开发层面，应使用 `TypeScript` 作为开发语言，并编译为 JS 供工具使用。其 `.d.ts` 声明包含在配套脚本源项目中。

工具要求主脚本计算出的值为一个函数，即脚本层的 ⌈ 主函数 ⌋ ，其拥有如下类型：

```ts
/** 主函数；返回值为null时，表示执行成功，为string时，表示执行失败，返回值为错误信息 */
type JS_MainFunction = (
    /** 启动参数 */
    argument: Array<string>,
) => null | string;
```

工具传递启动参数以调用该函数，直到该函数执行完毕，工具结束运行。

## 脚本拓展

根据前述，用户可以自行重写主脚本的运行逻辑，但更推荐在已有的配套脚本基础上拓展开发，它已为用户提供了丰富的功能与良好的交互机制。

若要进行脚本拓展开发，请查看本项目的 [Script 模块](https://github.com/twinkles-twinstar/TwinStar.ToolKit/tree/master/Script) 。

## 命令行传参

用户可以在启动工具时传入启动参数，若未提供启动参数，则会在运行时要求用户输入。启动参数如下格式：

```
[
    <input>
    [ -disable_filter ]
    [ -method <method-id> ]
    [ -argument <argument-json> ]
]...
```

* `<input>`

指定了命令的输入数据，一般是文件或目录的路径。

* `[ -disable_filter ]`

用于禁用 ⌈ 候选功能过滤 ⌋ 。

默认情况下，工具在处理一项命令时会根据其类型（主要通过拓展名）筛选出可用功能供用户选择；

若禁用 ⌈ 候选功能过滤 ⌋ ，则将列出所有功能供用户选择。

* `[ -method \<method-id> ]`

指定需执行的功能，后跟该功能的 ID ，若未指定功能，将在运行时等待用户选择功能。

* `[ -argument \<argument-json> ]`

指定需要传给功能工作函数的参数，后跟一个 JSON 字符串，且必须可解析为一个 Object 。

> 例 - 使用工具解码桌面上的 `test.pam` 文件：
> 
> `> .\launch.cmd "C:\Users\TwinStar\Desktop\test.pam" "-method" "animation.popcap_animation.decode"`
> 
> 该命令以 `test.pam` 文件的路径作为 `<input>` ，并指定需执行的功能为 `PopCap-Animation 解码` 。执行完毕后，可在桌面看到一个名为 `test.pam.json` 的新文件，即为解码后的 PAM 数据。