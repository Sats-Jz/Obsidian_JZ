---
title: "Custom Widgets"
source: "https://docs.waveterm.dev/customwidgets#the-structure-of-a-widget"
author:
published:
created: 2026-04-15
description: "Wave allows users to create their own widgets to uniquely customize their experience for what works for them. While we plan on greatly expanding on this in the future, it is already possible to make some widgets that you can access at the press of a button. All widgets can be created by modifying the /config/widgets.json file. By adding a widget to this file, it is possible to add widgets to the widget bar. By default, the widget bar looks like this:"
tags:
  - "wave"
---
Wave允许用户创建自己的小部件，根据自己的需求定制体验。虽然我们计划未来大幅扩展，但现在已经可以制作一些小部件，只需按下按钮即可访问。所有小部件都可以通过修改文件来创建。通过向该文件添加小部件，可以向小部件栏添加小部件。默认情况下，控件栏看起来如下： `<WAVETERM_HOME>/config/widgets.json` ![默认控件栏](https://docs.waveterm.dev/assets/images/all-widgets-default-403c62ed6423577594013d99d27ce387.webp)

通过添加额外的小部件，可以得到一个小部件条，看起来像这样：

![一个带有自定义控件的组件栏](https://docs.waveterm.dev/assets/images/all-widgets-extra-ae2b35c20a6eb516d39f68a628666845.webp)

## 小部件的结构

所有小部件的结构大致类似于下面的示例：

```json
"<widget name>": {
    "icon": "<font awesome icon name>",
    "label": "<the text label of the widget>",
    "color": "<the color of the label>",
    "blockdef": {
        "meta": {
            "view": "term",
            "controller": "cmd",
            "cmd": "<the actual cli command>"
        }
    }
}
```

这包括几个不同的部分。首先，每个小部件都有独特的识别名称。与该名称相关的值为外部 。下面用红色勾勒出： `WidgetConfigType`

![一个控件示例，外部键标记为 WidgetConfigType，内部键标记为 MetaTSType。在示例中，外层键分别是图标、标签、颜色和blockdef。内层键分别是视图、控制器和命令键。](https://docs.waveterm.dev/assets/images/widget-example-9405de1ab6480e51a15eb879ebe94dcb.webp)

这些数据在所有类型的小部件之间是共享的。也就是说，所有小部件——无论类型如何——都会使用相同的键来实现这一点。被认可的密钥如下： `WidgetConfigType`

| 说明 | 描述 |
| --- | --- |
| “显示：顺序 " | （可选）如果你想让控件的顺序与文件中提供的顺序不同，可以用数字覆盖控件的顺序。默认为0。 `widgets.json` |
| “偶像” | （可选）一个字体的名字， [超棒的图标](#font-awesome-icons) 。默认为 。 `"browser"` |
| “颜色” | （可选）一个表示颜色的字符串，类似于CSS中使用的颜色。包含十六进制代码和自定义CSS属性。这默认是颜色波形，用于区分文本，区别于其他文本。开箱即用。 `"var(--secondary-text-color)"` `"#c3c8c2"` |
| “标签” | （可选）一个表示出现在控件下方标签的字符串。如果钥匙没填好，悬停时它还会作为提示。默认情况下它是空的。 `"description"` |
| “描述” | （可选）对该小部件的功能描述。如果指定了，这个功能在悬停时会提示。默认情况下它是空的。 |
| “放大” | （可选）一个布尔值，表示小部件是否应该以放大方式启动。默认情况下是错误的。 |
| “阻挡防御” | 这里定义了小部件的非视觉部分。注意，所有后续定义都发生在这个元对象内的一个元对象内。 |

> [!-info] -info
> 信息
> 
> **字体超棒图标**
> 
> [Font Awesome](https://fontawesome.com/search) 提供了大量实用的图标，你可以在应用中作为小部件图标使用。最简单的方法就是你只需输入图标名称，它就会被使用。例如，字符串 会提供一个包含房屋的图标。我们还允许您通过修改图标名称，应用几种不同的样式如下： `"house"`
> 
> | 节目形式 | 描述 |
> | --- | --- |
> | <图标名称> | 是那个没有附加样式的普通图标。 |
> | solid@<图标名称> | 将职业添加到图标中，用填充颜色填充内容，而不是让它变成背景。 `fa-solid` |
> | regular@<图标名称> | 将类别添加到图标中，确保内容不会有填充色，而是使用标准轮廓。 `fa-regular` |
> | brands@<图标名称> | 这是为与品牌关联的图标添加所需类别的必要条件。没有这个，品牌图标渲染不正常。这对非品牌图标不适用。 `fa-brands` |

其他选项属于内层（图中用蓝色描边）。这里包含了关于小部件实际工作原理的所有细节。有效密钥因不同类型的小部件而异。下面将对它们进行更详细的探讨。 `MetaTSType`

## 终端和CLI控件

终端小部件，或称CLI小部件，是一种简单地打开终端并执行CLI命令的小部件。它们通常看起来像下面的例子：

```json
{
    <... other widgets go here ...>,
    "<widget name>": {
        "icon": "<font awesome icon name>",
        "label": "<the text label of the widget>",
        "color": "<the color of the label>",
        "blockdef": {
            "meta": {
                "view": "term",
                "controller": "cmd",
                "cmd": "<the actual cli command>"
            }
        }
    },
    <... other widgets go here ...>
}
```

它采用了所有小部件共有的常见选项。这些钥匙可以包括以下列出的钥匙： `WidgetConfigType` `MetaTSType`

| 说明 | 描述 |
| --- | --- |
| "view" | A string that specifies the general type of widget. In the case of custom terminal widgets, this must be set to.`"term"` |
| "controller" | A string that specifies the type of command being used. For more persistent shell sessions, set it to "shell". For one off commands, set it to. When is set, the widget has an additional refresh button in its header that allows the command to be re-run.`"cmd"` `"cmd"` |
| "cmd" | (optional) When the is set to, this option provides the actual command to be run. Note that because it is run as a command, there is no shell session unless you are launching a command that contains a shell session itself. Defaults to an empty string.`"controller"` `"cmd"` |
| "cmd:args " | (optional, array of strings) arguments to pass to the `cmd` |
| "cmd:shell " | (optional) if cmd:shell if false (default), then we use + (suitable to pass to ). if cmd:shell is true, then we just use, and cmd can include spaces, and shell syntax (like pipes or redirections, etc.) `cmd` `cmd:args` `execve` `cmd` |
| "cmd:interactive " | (optional) When the is set to \`"term", this boolean adds the interactive flag to the launched terminal. Defaults to false.`"controller"` |
| "cmd:login " | (optional) When the is set to, this boolean adds the login flag to the term command. Defaults to false.`"controller"` `"term"` |
| "cmd:runonstart " | (optional) The command will rerun when the block is created or the app is started. Without it, you must manually run the command. Defaults to true. |
| "cmd:runonce " | (optional) Runs on start, but then sets "cmd:runonce" and "cmd:runonstart" to false (so future runs require manual restarts) |
| "cmd:clearonstart " | (optional) When the cmd runs, the contents of the block are cleared out. Defaults to false. |
| "cmd:closeonexit " | (optional) Automatically closes the block if the command successfully exits (exit code = 0) |
| "cmd:closeonexitforce " | (optional) Automatically closes the block if when the command exits (success or failure) |
| "cmd:closeonexitdelay | (optional) Change the delay between when the command exits and when the block gets closed, in milliseconds, default 2000 |
| "cmd:env " | (optional) A key-value object represting environment variables to be run with the command. Defaults to an empty object. |
| "cmd:cwd " | (optional) A string representing the current working directory to be run with the command. Currently only works locally. Defaults to the home directory. |
| "cmd:nowsh " | (optional) A boolean that will turn off wsh integration for the command. Defaults to false. |
| "cmd:jwt " | (optional) A boolean that forces adding JWT token to the environment. Required for running waveapps as widgets (both local and remote). Defaults to false. |
| "term:localshellpath " | (optional) Sets the shell used for running your widget command. Only works locally. If left blank, wave will determine your system default instead. |
| "term:localshellopts " | (optional) Sets the shell options meant to be used with. This is useful if you are using a nonstandard shell and need to provide a specific option that we do not cover. Only works locally. Defaults to an empty string.`"term:localshellpath"` |
| "cmd:initscript " | (optional) for "shell" controller only. an init script to run before starting the shell (can be an inline script or an absolute local file path) |
| cmd:initscript.sh" | (optional) same as but applies to bash/zsh shells only `cmd:initscript` |
| cmd:initscript.bash" | (optional) same as but applies to bash shells only `cmd:initscript` |
| cmd:initscript.zsh" | (optional) same as but applies to zsh shells only `cmd:initscript` |
| cmd：initscript.pwsh” | （可选）与PWSH/Powershell壳相同，但仅适用于PWSH/Powershell `cmd:initscript` |
| cmd：initscript.fish” | （可选）与鱼壳相同，但仅适用于鱼壳 `cmd:initscript` |

### 本地壳控件示例

如果你的机器上安装了多个 shell，有时你可能会想用非默认的 shell。对于这种情况，为每个模型创建一个小部件很容易。

假设你想用一个小部件来启动一个 shell。安装完成后，你可以定义一个小部件： `fish` `fish`

```json
{
    <... other widgets go here ...>,
    "fish" : {
        "icon": "fish",
        "color": "#4abc39",
        "label": "fish",
        "blockdef": {
            "meta": {
                "view": "term",
                "controller": "shell",
                "term:localshellpath": "/usr/local/bin/fish",
                "term:localshellopts": "-i -l"
            }
        }
    },
    <... other widgets go here ...>
}
```

这会在控件栏上添加一个图标，你可以点击它来启动运行 shell 的终端。 `fish` ![示例鱼类控件](https://docs.waveterm.dev/assets/images/widget-example-fish-93cc72dea7d188b4ef1018c208e48805.webp)

> [!-info] -info
> 信息
> 
> 这可能不在你的道路上。如果这是真的，将 作为 的值就不行了。在这种情况下，你需要提供一条直接的路径。这通常会在类似 的地方，但你的系统可能不同。 `fish` `"fish"` `"term:localshellpath"` `"/usr/local/bin/fish"`

如果你想对 Powershell Core 或 之类的软件做同样的操作，可以定义小部件为 `pwsh`

```json
{
    <... other widgets go here ...>,
    "pwsh" : {
        "icon": "rectangle-terminal",
        "color": "#2671be",
        "label": "pwsh",
        "blockdef": {
            "meta": {
                "view": "term",
                "controller": "shell",
                "term:localshellpath": "pwsh"
            }
        }
    },
    <... other widgets go here ...>
}
```

这会在控件栏上添加一个图标，你可以点击它来启动运行 shell 的终端。 `pwsh` ![示例pwsh小部件](https://docs.waveterm.dev/assets/images/widget-example-pwsh-b93b7eef440b6cabb45a7d7f5136fa53.webp)

> [!-info] -info
> info
> 
> It is possible that is not in your path. If this is true, using as the value of will not work. In these cases, you will need to provide a direct path to it. This could be somewhere like on a Unix system or on Windows. but it may be different on your system. Also note that both and work on Windows, but only works on Unix systems.`pwsh` `"pwsh"` `"term:localshellpath"` `"/usr/local/bin/pwsh"` `"C:\Program Files\PowerShell\7\pwsh.exe"` `pwsh.exe` `pwsh` `pwsh`

### 远程壳组件示例

如果你想为某个连接（SSH或WSL）打开终端小部件，可以使用Meta密钥。连接键的数值应该和connections.json（或者你连接下拉菜单里的值）相符。请注意，你应只使用规范名称（不要使用你设置的任何自定义“display：name”）。对于WSL可能看起来像，对于SSH连接，可能看起来像。 `connection` `wsl://Ubuntu` `user@remotehostname`

```json
{
    <... other widgets go here ...>,
    "remote-term": {
        "icon": "rectangle-terminal",
        "label": "remote",
        "blockdef": {
            "meta": {
                "view": "term",
                "controller": "shell",
                "connection": "<connection>"
            }
        }
    },
    <... other widgets go here ...>
}
```

### 示例 Cmd 控件

这里有一些简单的命令文控件作为示例。

假设我想要一个打开后能运行 speedtest-go 的小部件。然后，我可以定义一个小部件

```json
{
    <... other widgets go here ...>,
    "speedtest" : {
        "icon": "gauge-high",
        "label": "speed",
        "blockdef": {
            "meta": {
                "view": "term",
                "controller": "cmd",
                "cmd": "speedtest-go --unix",
                "cmd:clearonstart": true
            }
        }
    },
    <... other widgets go here ...>
}
```

这会在控件栏上添加一个图标，你可以点击它来启动执行该命令的终端。 `speedtest-go --unix` ![示例速度测试小部件](https://docs.waveterm.dev/assets/images/widget-example-speed-fa4e8f8d77c23137f3ec9f47c1f9ad06.webp)

使用是实现这一点的最简单方法。 虽然不是必须的，但它会让每次执行该命令（可以通过右键点击头部并选择）时，之前的内容都会被清除。 `"cmd"` `"controller"` `"cmd:clearonstart"` `Force Controller Restart`

现在假设我想运行一个 TUI 应用，举个例子。事实证明，你大致可以做到同样的事情： `dua`

```json
{
    <... other widgets go here ...>,
    "dua" : {
        "icon": "brands@linux",
        "label": "dua",
        "blockdef": {
            "meta": {
                "view": "term",
                "controller": "cmd",
                "cmd": "dua"
            }
        }
    },
    <... other widgets go here ...>
}
```

This adds an icon to the widget bar that you can press to launch a terminal running the command. `dua` ![The example speedtest widget](https://docs.waveterm.dev/assets/images/widget-example-dua-955666c4a1b05fa0c9b1ef78b770b767.webp)

Because this is a TUI app that does not return anything when closed, the option doesn't change the behavior, so it has been excluded.`"cmd:clearonstart"`

## Web Widgets

Sometimes, it is desireable to open a page directly to a website. That can easily be accomplished by creating a custom widget. They have the following form in general:`"web"`

```json
{
    <... other widgets go here ...>,
    "<widget name>": {
        "icon": "<font awesome icon name>",
        "label": "<the text label of the widget>",
        "color": "<the color of the label>",
        "blockdef": {
            "meta": {
                "view": "web",
                "url": "<url of the first webpage>"
            }
        }
    },
    <... other widgets go here ...>
}
```

The takes the usual options common to all widgets. The can include the keys listed below:`WidgetConfigType` `MetaTSType`

| Key | Description |
| --- | --- |
| "view" | A string that specifies the general type of widget. In the case of custom web widgets, this must be set to.`"web"` |
| “URL” | 这个字符串是当前页面的网址。作为小部件的一部分，它将作为小部件起始的页面。如果没有指定，这个选项会默认为全局可配置，而这在全新安装时 [https://github.com/wavetermdev/waveterm](https://github.com/wavetermdev/waveterm) 。 `"web:defaulturl"` |
| “钉子之星” | （可选）这个字符串是首页按钮会带你去的URL。如果没有指定，这个选项会默认为全局可配置，而这在全新安装时 [https://github.com/wavetermdev/waveterm](https://github.com/wavetermdev/waveterm) 。 `"web:defaulturl"` |

### 示例网页小部件

比如你想要一个小部件，它会自动从YouTube开始，并以YouTube作为首页。这可以通过以下方式实现：

```json
{
    <... other widgets go here ...>,
    "youtube" : {
        "icon": "brands@youtube",
        "label": "youtube",
        "blockdef": {
            "meta": {
                "view": "web",
                "url": "https://youtube.com",
                "pinnedurl": "https://youtube.com"
            }
        }
    },
    <... other widgets go here ...>
}
```

这会在控件栏中添加一个图标，你可以点击它在YouTube首页启动网页控件。 ![示例速度测试小部件](https://docs.waveterm.dev/assets/images/widget-example-youtube-28ed33db3228f9163a468c138be8c385.webp)

或者，比如你想要一个网页小部件，打开时像书签一样打开 GitHub，但之后会用 Google 作为首页。这可以通过以下方式轻松实现：

```json
{
    <... other widgets go here ...>,
    "github" : {
        "icon": "brands@github",
        "label": "github",
        "blockdef": {
            "meta": {
                "view": "web",
                "url": "https://github.com",
                "pinnedurl": "https://google.com"
            }
        }
    },
    <... other widgets go here ...>
}
```

这会在控件栏上添加一个图标，你可以点击它来在GitHub首页启动网页控件。

## Sysinfo 小组件

Sysinfo 小部件被有意限制在我们会随着时间扩展的极小可能数值子集。但你仍然可以配置自己的版本——比如默认加载不同的图。该小部件的一般形式如下：

```json
{
    <... other widgets go here ...>,
    "<widget name>": {
        "icon": "<font awesome icon name>",
        "label": "<the text label of the widget>",
        "color": "<the color of the label>",
        "blockdef": {
            "meta": {
                "view": "sysinfo",
                "graph:numpoints": <the max number of points in the graph>,
                "sysinfo:type": <the name of the plot collection>,
            }
        }
    },
    <... other widgets go here ...>
}
```

它采用了所有小部件共有的常见选项。这些钥匙可以包括以下列出的钥匙： `WidgetConfigType` `MetaTSType`

| 说明 | 描述 |
| --- | --- |
| “视图” | 一个字符串，指定了小部件的一般类型。对于自定义的 sysinfo 控件，必须将此值设置为 。 `"sysinfo"` |
| "graph:numpoints " | The maximum amount of points that can be shown on the graph. Equivalently, the number of seconds the graph window covers. This defaults to 100. |
| "sysinfo:type " | A string representing the collection of types to show on the graph. Valid values for this are,,, and. Note that these are case sensitive. If no value is provided, the plot will default to showing.`"CPU"` `"Mem"` `"CPU + Mem"` `All CPU` `"CPU"` |

### Example Sysinfo Widgets

Suppose you have a build process that lasts 3 minutes and you'd like to be able to see the entire build on the sysinfo graph. Also, you would really like to view both the cpu and memory since both are impacted by this process. In that case, you can set up a widget as follows:

```json
{
    <... other widgets go here ...>,
    "3min-info" : {
        "icon": "circle-3",
        "label": "3mininfo",
        "blockdef": {
            "meta": {
                "view": "sysinfo",
                "graph:numpoints": 180,
                "sysinfo:type": "CPU + Mem"
            }
        }
    },
    <... other widgets go here ...>
}
```

This adds an icon to the widget bar that you can press to launch the CPU and Memory plots by default with 180 seconds of data. ![The example speedtest widget](https://docs.waveterm.dev/assets/images/widget-example-3mininfo-164f3ae5998bde9ade92558d4e1f06a0.webp)

Now, suppose you are fine with the default 100 points (and 100 seconds) but would like to show all of the CPU data when launched. In that case, you can write:

```json
{
    <... other widgets go here ...>,
    "all-cpu" : {
        "icon": "chart-scatter",
        "label": "all-cpu",
        "blockdef": {
            "meta": {
                "view": "sysinfo",
                "sysinfo:type": "All CPU"
            }
        }
    },
    <... other widgets go here ...>
}
```

This adds an icon to the widget bar that you can press to launch All CPU plots by default.

## Overriding Default Widgets

Wave ships with 5 default widgets in the widgets bar (terminal, files, web, ai, and sysinfo). You can modify or remove these by overriding their config in widgets.json. The names of the 5 widgets, in order, are:

- `defwidget@terminal`
- `defwidget@files`
- `defwidget@web`
- `defwidget@ai`
- `defwidget@sysinfo`

To remove any of them, just set that key to in your widgets.json file.`null`

To see their definitions, to copy/paste them, or to understand how they work, you can view all of their definitions on [GitHub - default widgets.json](https://github.com/wavetermdev/waveterm/blob/main/pkg/wconfig/defaultconfig/widgets.json)