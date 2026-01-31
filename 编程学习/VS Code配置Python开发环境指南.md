10分钟搞定！VS Code配置Python开发环境指南（2025新版）

## 前言

在Python开发领域，Visual Studio Code（简称VSCode）凭借其轻量级、高扩展性和强大的社区支持，已成为开发者首选的编辑器之一。相比传统的IDE（如PyCharm、Jupyter Notebook），VSCode不仅具备代码高亮、智能提示、调试支持等基础功能，还能通过丰富的插件生态实现虚拟环境管理、Jupyter交互式编程、Git版本控制等高级功能。

本文将手把手教你如何在VSCode中配置Python开发环境，涵盖从基础安装到进阶配置的完整流程，并附上常见问题解决方案，帮助你快速构建一个高效、稳定的Python开发环境。

![](https://pic3.zhimg.com/v2-8d03a625d4f97c5462b038569be3374c_r.jpg)

*c7f91d9c2020209ba59539b7c8ed3f3*



## 一、基础环境搭建

### 1.1 安装必备软件

- **VS Code官方下载** ：可参考之前文章([https://blog.csdn.net/qq_17153885/article/details/147530379?spm=1001.2014.3001.5501](https://blog.csdn.net/qq_17153885/article/details/147530379?spm=1001.2014.3001.5501))
- **Python环境安装** ：可参考之前文章([https://blog.csdn.net/qq_17153885/article/details/145812460?spm=1001.2014.3001.5501](https://blog.csdn.net/qq_17153885/article/details/145812460?spm=1001.2014.3001.5501))

‍

### 1.2 初始配置

1.  打开VS Code，安装中文语言包（可选）：
 

- 快捷键 `Ctrl+Shift+X` 打开扩展商店
- 搜索"Chinese Language Pack"并安装
-  设置工作区：
 
-  创建项目文件夹（如`PythonProjects`）
 
- 通过`文件 > 打开文件夹`载入工作目录

‍

在电脑中创建一个`PythonProjects`文件夹，存放你编写的Python代码

![](https://pic2.zhimg.com/v2-e6ef560fa693beb26f42e80bbf7e06d1_r.jpg)

*image*



![](https://pic3.zhimg.com/v2-f60456e1502b9be598dcd1786d39d910_r.jpg)

*image*



![](https://picx.zhimg.com/v2-d1131c2d6f5484b9bf00d77aafcb4989_r.jpg)

*image*



![](https://pic4.zhimg.com/v2-b073535f2614bfcca358524d342ea729_r.jpg)

*image*



‍

## 二、Python开发核心配置

### 2.1 安装Python扩展

VSCode本身并不直接支持Python开发，但我们可以通过安装插件来实现这一功能。

在 VS Code 的插件市场，搜索 “Python”，安装由微软官方维护的 Python 扩展。该插件提供了代码智能补全、调试支持、Jupyter Notebook 集成等核心功能，是 Python 开发的必备工具。

1. 打开VSCode，点击左侧活动栏中的“扩展”（Extensions）图标。
2. 在搜索框中输入“Python”，然后选择由Microsoft开发的官方插件进行安装。
3. 安装完成后，VSCode会自动为你启用Python语言支持，并显示相关功能。

![](https://pica.zhimg.com/v2-1137105d7f482243327f725d8818e690_r.jpg)

*image*



![](https://pic4.zhimg.com/v2-48552ef149ded8fae44c4558730279f3_r.jpg)

*image*



### 2.2 配置Python解释器

在VSCode中，你可以为每个工作区或项目配置一个特定的Python解释器。为了确保使用正确的Python版本，需进行如下设置：

1. 打开VSCode，按下`Ctrl+Shift+P`（Windows/Linux）或`Cmd+Shift+P`（macOS）打开命令面板。
2. 输入“Python: Select Interpreter”并选择该命令。
3. VSCode会列出当前系统中安装的所有Python解释器，选择你想要使用的版本。如果你使用虚拟环境，请选择对应的虚拟环境解释器。

对于每个项目，建议使用虚拟环境来管理依赖，避免与全局Python环境产生冲突。

‍

1. 按`Ctrl+Shift+P`打开命令面板
2. 输入"Python: Select Interpreter"，选择已安装的Python版本
3. 验证配置：新建`.py`文件，输入`print("Hello jihaishibei")`，按F5运行

![](https://pic1.zhimg.com/v2-2557130dc982c522db476291656f5478_r.jpg)

*image*



![](https://pic2.zhimg.com/v2-11334c0bc29ae15a457e69543fbdd587_r.jpg)

*image*




![](https://picx.zhimg.com/v2-ffd904a05dc33c92f5cf1485d7babcdd_r.jpg)

*image*



![](https://pic4.zhimg.com/v2-b9d295633d7c3521eb2dc9122c8b20ad_r.jpg)

*image*



### 2.3 虚拟环境管理

在VSCode中使用虚拟环境是管理项目依赖的最佳实践。通过虚拟环境，你可以为每个项目隔离不同的包和依赖，避免不同项目之间的依赖冲突。

**为什么需要虚拟环境？** 

通过一个对比表格说明必要性：

| 场景        | 使用系统Python | 使用虚拟环境                 |
| --------- | ---------- | ---------------------- |
| 依赖管理      | 所有项目共享相同包  | 每个项目独立依赖库              |
| 版本冲突      | 可能出现版本不兼容  | 隔离环境避免冲突               |
| 生产部署      | 需手动记录依赖    | 通过requirements.txt精准还原 |
| 多Python版本 | 切换困难       | 可自由指定解释器版本             |

‍

### 2.3.1 创建虚拟环境

以下是创建虚拟环境的步骤：

1. 打开终端（`Ctrl+` `）并进入你的项目目录。
2. 使用以下命令创建虚拟环境（假设使用Python 3）：

```text
python -m venv venv
```

‍

具体解释：

① python：调用 Python 解释器

② -m：表示要运行一个模块（module）

③ venv：这是 Python 内置的虚拟环境模块

④ .venv：这是你要创建的虚拟环境的目录名称（前面的点表示在 Unix-like 系统中这是一个隐藏目录），指定虚拟环境将被创建在当前目录下的 .venv 文件夹中【这个名称可以更改】

‍

例如，我的文件夹是D:\code\PythonProjects，则创建后的虚拟环境文件夹为D:\code\PythonProjects\venv\

如果想设置多个虚拟环境，可以设置不同的名称。例如：

```python
python -m venv myenv310
```

那么，创建的虚拟环境文件夹就是：D:\code\PythonProjects\myenv310

![](https://picx.zhimg.com/v2-12c79f1e93a4ae98a2182a0ee01ac271_r.jpg)

*image*






这将在当前目录下创建一个名为`venv`的虚拟环境文件夹。

![](https://picx.zhimg.com/v2-2a4f6d5d7787447f82a0ededcbbee487_r.jpg)

*image*



点击“yes”，配置运行为虚拟环境

（如下图，点击右下角，也可以进行切换）

![](https://pic3.zhimg.com/v2-67b3713a44570e8fc59e9c8ceed3541a_r.jpg)

*image*



‍

生成的虚拟环境

![](https://pic3.zhimg.com/v2-1cb2ae373c63440462bede69dda82742_r.jpg)

*image*



### 2.3.2 激活虚拟环境

激活虚拟环境：

- Windows：

`.\venv\Scripts\activate` * macOS/Linux：

`source venv/bin/activate` 

![](https://pic4.zhimg.com/v2-ff3ab3126dd69ddfb7db56f015bd9ccb_r.jpg)

*image*



可能遇到不能执行脚本的错误：这是因为执行策略可帮助你防止执行不信任的脚本，我们把安全策略改了就行。

可以以管理员身份打开powershell，并执行set-executionpolicy remotesigned，提示是否更改执行策略时，输入“y”并回车即可执行脚本。

‍

（首先用管理员模式运行 Windows PowerShell ， 然后输入`get-ExecutionPolicy` 取得shell的当前执行策略，看到返回了 `Restricted` 默认值）

![](https://pica.zhimg.com/v2-2deb6f92693a1ba3be75af9997d24fb8_r.jpg)

*image*



‍

![](https://pica.zhimg.com/v2-3dec89e94d6f20fcaa2330855d8f7576_r.jpg)

*image*



‍

再次执行，成功

![](https://picx.zhimg.com/v2-9f1c328e070ea092b26eee578d48df05_r.jpg)

*image*

激活后，你会看到终端提示符变成了虚拟环境的名称，表示你已成功进入虚拟环境。

‍

### 2.3.3 安装Python包

在虚拟环境中安装所需的Python包非常简单，只需要使用`pip`命令即可。例如：

```bash
pip install numpy
```

可以在VSCode的终端中直接运行此命令来安装依赖。

‍

‍

## 三、高效开发功能配置

### 3.1 配置调试器

### 配置

VSCode提供强大的调试功能，支持Python代码的单步执行、变量监视、断点设置等。配置调试器的步骤如下：

1. 打开VSCode中的“调试”视图（左侧活动栏中的播放按钮图标）。
2. 点击“创建一个launch.json文件”链接，选择“Python”作为环境。
3. VSCode会自动生成一个`launch.json`配置文件，你可以根据需要修改调试配置。

常见的调试配置包括：

- 配置断点：你可以在代码行号旁边点击设置断点，调试时程序会在这些位置暂停。
- 调试控制台：可以查看程序输出和变量值。
- 调试配置：修改`launch.json`文件以支持不同的调试场景，例如运行脚本、调试测试等。

![](https://picx.zhimg.com/v2-1027877f62100c95ac130668a3701eb3_r.jpg)

*image*



![](https://pica.zhimg.com/v2-65218a64c3d63bb6e52ead6167770f78_r.jpg)

*image*



![](https://pic3.zhimg.com/v2-5aa80e2205237823e57d87a2971be4ba_r.jpg)

*image*



‍

如果想要在启动调试后，自动停止在程序入口，那么在launch.json中添加下面设置：

```bash
"stopOnEntry": true
```

‍

### 设置断点与调试

打开想要调试的python文件，然后将鼠标移动到行号的左侧（我这里选的是第3行），可以看到一个断点的标志，点击即可在该行设置断点：

![](https://pic3.zhimg.com/v2-b253ccc664a5a26fe774918e1e03d8ce_r.jpg)

*image*



‍

"Start Debugging"来启动调试，启动调试之后，程序停在了刚才设置的断点处：

![](https://pic3.zhimg.com/v2-60766742e3289cb9ee3e9d6e60722c88_r.jpg)

*image*



‍

![](https://picx.zhimg.com/v2-ca13b528cbe116848fb348fa2a1465ef_r.jpg)

*image*






接着可以使用调试面板中的调试按钮来调试我们的程序了，调试面板中的按钮从左到右依次是：

![](https://pic2.zhimg.com/v2-169cb46a38d8192990442073a6a1eba7_r.jpg)

*image*

按钮1：移动按钮，拖住后可以移动调试控制面板；
按钮2：继续运行，对应快捷键F5；
按钮3：单步跳过，对应快捷键F10；
按钮4：单步调试，对应快捷键F11；
按钮5：单步跳出，对应快捷键Shift+F11；
按钮6：重新运行，对应快捷键Ctrl+Shift+F5；
按钮7：停止调试，对应快捷键Shift+F5；

![](https://pic2.zhimg.com/v2-d2a659bc20674f8c0d86c3ef75c32095_r.jpg)

*image*



![](https://pic2.zhimg.com/v2-e504feb7d9836f894b8e703cca5cec0b_r.jpg)

*image*



### 3.2 代码质量工具

### 配置flake8代码规范检查

- 安装：`pip install flake8`

![](https://pica.zhimg.com/v2-c1dc9dd0c794352d2d679c9325fad164_r.jpg)

*image*

 * 修改用户设置（settings.json）：

![](https://picx.zhimg.com/v2-4e106967d286718050b61db78e60293f_r.jpg)

*image*



![](https://pic4.zhimg.com/v2-5b377d051bc7e2499da469f08262c0df_r.jpg)

*image*



![](https://pica.zhimg.com/v2-2ebf499c34524786b3b3affadbb48730_r.jpg)

*image*



```json
{
    "python.linting.flake8Enabled": true,
    "python.linting.enabled": true
}
```

![](https://pic4.zhimg.com/v2-17373a94426854e1cc2a409e4a1cee35_r.jpg)

*image*



### 自动格式化配置

- 安装autopep8：`pip install autopep8`

![](https://pic3.zhimg.com/v2-17183a98f08f9d6ae93545a495497be0_r.jpg)

*image*



- 设置保存时自动格式化：

```json
{
    "editor.formatOnSave": true,
    "python.formatting.provider": "autopep8"
}
```

![](https://pic1.zhimg.com/v2-b6c0f3a7879d881d739df63e44dcf0fc_r.jpg)

*image*



‍

## 结束语

通过本文的介绍，相信你已经掌握了如何在VSCode中配置Python开发环境。如果你对 VS Code 配置 Python 开发环境还有疑问，或想探索更多实用功能（如插件推荐、调试技巧、代码优化等），欢迎关注【技海拾贝】的后续内容！记得点赞、收藏或留言交流，说不定下一个编程灵感就来自你的分享哦！让我们一起用 VS Code 开启高效 Python 开发之旅，轻松搞定需求！  

![](https://pica.zhimg.com/v2-39aec5ef7c1817da7216c182884280cc_1440w.jpg)

*1706858207711*



‍

‍

