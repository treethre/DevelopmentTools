## 前言

[Scoop](https://p3terx.com/go/aHR0cHM6Ly9naXRodWIuY29tL2x1a2VzYW1wc29uL3Njb29w) 是一个 Win­dows 包管理工具，类似于 De­bian 的 `apt`、ma­cOS 的 `homebrew`。它由开源社区驱动，体验可能是是目前所有 Win­dows 包管理工具中最好的。对开发者来说，包管理器能非常方便的部署开发环境，比如 Python 、Node.js 。而对于像博主这样的普通的计算机使用者来说，可以方便的安装一些常用软件，尤其是开源软件，免去了手动去官网下载的繁琐步骤，而且后续对软件进行升级只需要输入一行命令，非常便捷。

## 环境要求

- Windows 7 SP1 + / Windows Server 2008+
- [PowerShell 5](https://p3terx.com/go/aHR0cHM6Ly9ha2EubXMvd21mNWRvd25sb2Fk)（或更高版本，包括 [PowerShell Core](https://p3terx.com/go/aHR0cHM6Ly9kb2NzLm1pY3Jvc29mdC5jb20vZW4tdXMvcG93ZXJzaGVsbC9zY3JpcHRpbmcvaW5zdGFsbC9pbnN0YWxsaW5nLXBvd2Vyc2hlbGwtY29yZS1vbi13aW5kb3dzP3ZpZXc9cG93ZXJzaGVsbC02)）和 [.NET Framework 4.5](https://p3terx.com/go/aHR0cHM6Ly93d3cubWljcm9zb2Z0LmNvbS9uZXQvZG93bmxvYWQ)（或更高版本）
- Windows 用户名为英文（Windows 用户环境变量中路径值不支持中文字符）
- **正常、快速**的访问 GitHub 并下载资源

## 安装 Scoop

Scoop 默认使用普通用户权限，其本体和安装的软件默认会放在 `%USERPROFILE%\scoop`(即 `C:\Users\用户名\scoop`)，使用管理员权限进行全局安装 (`-g`) 的软件在 `C:\ProgramData\scoop`。如果有自定安装路径的需求，那么要提前设置好环境变量，否则后续再改不是一件容易的事情。

- 打开 PowerShell
- 设置用户安装路径

```bash
$env:SCOOP='D:\Scoop'
[Environment]::SetEnvironmentVariable('SCOOP', $env:SCOOP, 'User')
```

- 设置全局安装路径（需要管理员权限）

```bash
$env:SCOOP_GLOBAL='D:\Scoop_Global'
[Environment]::SetEnvironmentVariable('SCOOP_GLOBAL', $env:SCOOP_GLOBAL, 'Machine')
```

- 设置允许 PowerShell 执行本地脚本

```bash
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

- 安装 Scoop

```bash
iwr -useb get.scoop.sh | iex
```

- 没安装过 Git 则需要安装。

```bash
scoop install git
```

## 基础使用

最基础的使用方法和其它包管理器类似，这里就不做赘述了，直接上命令列表：

- `scoop search <app>` - 搜索软件
- `scoop install <app>` - 安装软件
- `scoop info <app>` - 查看软件详细信息
- `scoop list` - 查看已安装软件
- `scoop uninstall <app>` - 卸载软件，`-p`删除配置文件。
- `scoop update` - 更新 scoop 本体和软件列表
- `scoop update <app>` - 更新指定软件
- `scoop update *` - 更新所有已安装的软件
- `scoop checkup` - 检查 scoop 的问题并给出解决问题的建议
- `scoop help` - 查看命令列表
- `scoop help <command>` - 查看命令帮助说明

## 进阶使用

### 添加 bucket

所有的包管理器都会有相应的软件仓库 ，而 bucket 就是 Scoop 中的软件仓库。细心的你可能会发现 `scoop` 翻译为中文是 “舀”，而 `bucket` 是 “水桶”，所以安装软件可以理解为从水桶里舀水，似乎很形象的说。

Scoop 默认软件仓库（main bucket）软件数量是有限的，但是可以进行额外的添加。通过 `scoop bucket known` 命令可以查看官方认可的 bucket：

```bash
# 查看官方支持的 bucket
scoop bucket known
# main
# extras
# versions
# nightlies
# nirsoft
# php
# nerd-fonts
# nonportable
# java
# games
# jetbrains

# 查看帮助
scoop bucket help
# scoop bucket: cmd 'help' not supported
# Usage: scoop bucket add|list|known|rm [<args>]

# 添加 bucket
scoop bucket add extras
scoop bucket add java
```

以上官方认可的 bucket 可以通过下面这个命令直接添加：

```none
scoop bucket add <bucketname>
```

[extras](https://p3terx.com/go/aHR0cHM6Ly9naXRodWIuY29tL2x1a2VzYW1wc29uL3Njb29wLWV4dHJhcw) 涵盖了大部分因为种种原因不能被收录进主仓库的常用软件，这个是强推荐添加的。

```none
scoop bucket add extras
```

#### 第三方 bucket

添加第三方 bucket

```none
scoop bucket add <bucketname> https://github.com/xxx/xxx
```

从第三方 bucket 中安装软件

```none
scoop install <bucketname>/<app>
```

### 清理安装包缓存

Scoop 会保留下载的安装包，对于卸载后又想再安装的情况，不需要重复下载。但长期累积会占用大量的磁盘空间，如果用不到就成了垃圾。这时可以使用 `scoop cache` 命令来清理。

- `scoop cache show` - 显示安装包缓存
- `scoop cache rm <app>` - 删除指定应用的安装包缓存
- `scoop cache rm *` - 删除所有的安装包缓存

如果你不希望安装和更新软件时保留安装包缓存，可以加上 `-k` 或 `--no-cache` 选项来禁用缓存：

- `scoop install -k <app>`
- `scoop update -k *`

### 删除旧版本软件

当软件被更新后 Scoop 还会保留软件的旧版本，更新软件后可以通过 `scoop cleanup` 命令进行删除。

- `scoop cleanup <app>` - 删除指定软件的旧版本
- `scoop cleanup *` - 删除所有软件的旧版本

与安装软件一样，删除旧版本软件的同时也可以清理安装包缓存，同样是加上 `-k` 选项。

- `scoop cleanup -k <app>` - 删除指定软件的旧版本并清除安装包缓存
- `scoop cleanup -k *` - 删除所有软件的旧版本并清除安装包缓存

### 全局安装

全局安装就是给系统中的所有用户都安装，且环境变量是系统变量，对于需要设置系统变量的一些软件就需要全局安装，比如 Node.js、Python ，否则某些情况会出现无法找到命令的问题。

使用 `scoop install <app>` 命令加上 `-g` 或 `--global` 选项可对软件进行全局安装，全局安装需要管理员权限，所以需要提前以管理员权限运行的 Pow­er­Shell 。更简单的方式是先安装 `sudo`，然后用 `sudo` 命令来提权执行：

```none
scoop install sudo
sudo scoop install -g <app>
```

> 达成在 Win­dows 上使用`sudo`的成就

使用 `scoop list` 命令查看已装软件时，全局安装的软件末尾会有 `*global*` 标志。

```none
➜ scoop list
Installed apps:

  7zip 19.00
  adb 30.0.0
  aria2 1.35.0-1
  busybox 3466-g53c09d0e1
  CascadiaCode-NF 2.1.0 [nerd-fonts]
  colortool 1904.29002
  dark 3.11.2 *global*
  ffmpeg 4.2.3
  figlet 1.0-go
  FiraCode-NF 2.1.0 [nerd-fonts]
  git 2.26.2.windows.1 *global*
  innounp 0.49
  iperf3 3.1.3
  lessmsi 1.6.91 *global*
  lxrunoffline 3.4.1 [extras]
  nano 4.9-4
  neofetch 7.0.0
  nodejs-lts 12.17.0 *global*
  python 3.8.3 *global*
  rclone 1.52.0
  rufus 3.10 [extras]
  screentogif 2.24.2 [extras]
  sudo 0.2020.01.26
```

此外对于全局软件的更新和卸载等其它操作，都需要加上 `-g` 选项：

- `sudo scoop update -g *` - 更新所有软件（且包含全局软件）
- `sudo scoop uninstall -g <app>` - 卸载全局软件
- `sudo scoop uninstall -gp <app>` - 卸载全局软件（并删除配置文件）
- `sudo scoop cleanup -g *` - 删除所有全局软件的旧版本
- `sudo scoop cleanup -gk *` - 删除所有全局软件的旧版本（并清除安装包包缓存）

### 代理设置

Scoop 默认使用的是系统代理，如果你想手动指定代理，可以输入下面的命令。需要注意的是只支持 http 协议。

```none
scoop config proxy localhost:1080
```

> 设置完可以通过`scoop config proxy`查看。

如果你想取消代理，那么输入下面的命令，这将会恢复使用系统代理。

```none
scoop config rm proxy
```

### 开启多线程下载

使用 Scoop 安装 Aria2 后，Scoop 会自动调用 Aria2 进行多线程加速下载。

```none
scoop install aria2
```

使用 `scoop config` 命令可以对 Aria2 进行设置，比如 `scoop config aria2-enabled false` 可以禁止调用 Aria2 下载。以下是与 Aria2 有关的设置选项：

- `aria2-enabled`: 开启 Aria2 下载，默认`true`
- [`aria2-retry-wait`](https://p3terx.com/go/aHR0cHM6Ly9hcmlhMi5naXRodWIuaW8vbWFudWFsL2VuL2h0bWwvYXJpYTJjLmh0bWwjY21kb3B0aW9uLXJldHJ5LXdhaXQ): 重试等待秒数，默认`2`
- [`aria2-split`](https://p3terx.com/go/aHR0cHM6Ly9hcmlhMi5naXRodWIuaW8vbWFudWFsL2VuL2h0bWwvYXJpYTJjLmh0bWwjY21kb3B0aW9uLXM): 单任务最大连接数，默认`5`
- [`aria2-max-connection-per-server`](https://p3terx.com/go/aHR0cHM6Ly9hcmlhMi5naXRodWIuaW8vbWFudWFsL2VuL2h0bWwvYXJpYTJjLmh0bWwjY21kb3B0aW9uLXg): 单服务器最大连接数，默认`5` ，最大`16`
- [`aria2-min-split-size`](https://p3terx.com/go/aHR0cHM6Ly9hcmlhMi5naXRodWIuaW8vbWFudWFsL2VuL2h0bWwvYXJpYTJjLmh0bWwjY21kb3B0aW9uLWs): 最小文件分片大小，默认`5M`

博主在这里推荐以下优化设置，单任务最大连接数设置为 `32`，单服务器最大连接数设置为 `16`，最小文件分片大小设置为 `1M`

```none
scoop config aria2-split 32
scoop config aria2-max-connection-per-server 16
scoop config aria2-min-split-size 1M
```

## 常用命令总结

看到这里一定有很多小伙伴已经懵逼了，最后总结一波 Scoop 在日常使用中的常用命令：

```bash
# 更新 scoop 及软件包列表
scoop update

## 安装软件 ##
# 非全局安装（并禁止安装包缓存）
scoop install -k <app>
# 全局安装（并禁止安装包缓存）
sudo scoop install -gk <app>
# 查看某软件执行命令位置
scoop which {{name}}
# 搜索某软件
scoop search {{name}}
# 打开某软件官网
scoop home {{name}}

## 卸载软件 ##
# 卸载非全局软件（并删除配置文件）
scoop uninstall -p <app>
# 卸载全局软件（并删除配置文件）
sudo scoop uninstall -gp <app>

## 更新软件 ##
# 更新所有非全局软件（并禁止安装包缓存）
scoop update -k *
# 更新所有软件（并禁止安装包缓存）
sudo scoop update -gk *


# 检查潜在的问题
scoop checkup
# 查看状态
scoop status

## 垃圾清理 ##
# 删除所有旧版本非全局软件（并删除软件包缓存）
scoop cleanup -k *
# 删除所有旧版本软件（并删除软件包缓存）
sudo scoop cleanup -gk *
# 清除软件包缓存
scoop cache rm *
```

### scoop 安装我在使用的软件

```bash
# 启用 aria2c 加速下载，不过可能会导致下载失败

# 建议先安装
scoop install innounp
scoop install extras/vcredist2010   # erroor: missing MSVCR100.dll
scoop install extras/vcredist2015

# dev env
scoop install git
scoop install openssh # because: https://stackoverflow.com/questions/49926386/openssh-windows-bad-owner-or-permissions
scoop install nodejs # test: node npm
scoop install yarn # depend on npm
scoop install python # test: python pip
scoop install go

# java
scoop bucket add java

# install java lastest openjdk
scoop install openjdk 

# 如果你想使用 jdk8 你可以使用 openjdk 的发行版本
scoop install adopt8-upstream

# 切换到 jk8, 默认只会使用最后一次安装的版本
scoop reset adopt8-upstream

java -version
# 目前在 scoop java bucket 里的 jdk 版本
chrome https://github.com/ScoopInstaller/Java/tree/master/bucket


# CLI tools
scoop install wget
scoop install ffmpeg
scoop install aria2
scoop install youtube-dl
scoop install vim

# apps im
scoop install telegram

# apps tools
scoop install googlechrome  # Chrome
scoop install everything    # 文件搜索
scoop install 7zip  # 压缩
scoop install dismplusplus  # dism++
scoop install calibre   # 书籍管理
scoop install snipaste  # 截图工具
scoop install libreoffice-stable    # office
```

### 如果你想安装微信或者网易云

```bash
scoop bucket add dodorz https://github.com/dodorz/scoop-bucket
scoop install dodorz/NeteaseMusic
scoop install dodorz/wechat
```

**wechat.json**

```bash
{
    "homepage": "https://weixin.qq.com/",
    "description": "Free messaging and calling app.",
    "version": "2.9.0",
    "license": {
        "identifier": "EULA",
        "url": "https://weixin.qq.com/cgi-bin/readtemplate?lang=zh_CN&t=weixin_agreement&s=default"
    },
    "url": "https://dldir1.qq.com/weixin/Windows/WeChatSetup.exe#/dl.7z",
    "shortcuts": [
        [
            "wechat.exe",
            "WeChat"
        ]
    ],
    "post_install": [
        "Remove-Item \"$dir\\`$PLUGINSDIR\" -Force -Recurse",
        "Remove-Item \"$dir\\`$_15_\" -Force -Recurse",
        "Remove-Item \"$dir\\`$R5\" -Force -Recurse"
    ],
    "checkver": "微信 ([\\d.]+) for Windows 发布",
    "notes": "We don't persist your WeChat data, they are still storaged in '%APPDATA%\\Tencent\\WeChat'."
}
```

**neteasemusic.json**

```bash
{
    "homepage": "https://music.163.com/",
    "description": "The official NetEase Cloud Music client.",
    "version": "2.7.1.198242",
    "license": {
        "identifier": "EULA",
        "url": "https://music.163.com/html/web2/service.html"
    },
    "url": "https://d1.music.126.net/dmusic/obj/w5zCg8OCw6fCn2vDicOl/809710492/7805/2019112318441/cloudmusicsetup2.7.1.198242.exe#/dl.7z",
    "hash": "md5:991ae324e2ff261295f5fb4caeff55d9",
    "post_install": "Remove-Item \"$dir\\`$PLUGINSDIR\" -Force -Recurse",
    "bin": "cloudmusic.exe",
    "shortcuts": [
        [
            "cloudmusic.exe",
            "Netease Cloud Music"
        ]
    ],
    "checkver": {
        "url": "https://h404bi.azurewebsites.net/ncmversion.php",
        "jp": "$.updateFiles[0].url",
        "regex": "https://d1.music.126.net/dmusic/cloudmusicsetup([\\d.]+)\\.exe"
    },
    "autoupdate": {
        "url": "https://d1.music.126.net/dmusic/cloudmusicsetup$version.exe#/dl.7z",
        "hash": {
            "url": "https://h404bi.azurewebsites.net/ncmversion.php",
            "jp": "$.updateFiles[0].hash"
        }
    },
    "notes": "We don't persist your CloudMusic data, they are still storaged in '%LOCALAPPDATA%\\Netease\\CloudMusic'."
}
```

## 使用 DISM++ 检查 WIN10 环境

```bash
# 现在 scoop 安装 Dism
# 扫描全部系统文件并和官方系统文件对比，扫描计算机中的不一致情况
Dism /Online /Cleanup-Image /ScanHealth
# 这条命令必须在前一条命令执行完以后，发现系统文件有损坏时使用
Dism /Online /Cleanup-Image /CheckHealth
# 不同的系统文件还原成官方系统源文件
DISM /Online /Cleanup-image /RestoreHealth
# 完成重启后
sfc /SCANNOW
```

## VSCode extensions

```bash
# scoop install vscode

code --extensions-dir <dir>
    Set the root path for extensions.
code --list-extensions
    List the installed extensions.
code --show-versions
    Show versions of installed extensions, when using --list-extension.
code --install-extension (<extension-id> | <extension-vsix-path>)
    Installs an extension.
code --uninstall-extension (<extension-id> | <extension-vsix-path>)
    Uninstalls an extension.
code --enable-proposed-api (<extension-id>)
    Enables proposed API features for extensions. Can receive one or more extension IDs to enable individually.
```

安装我使用的 VSCode extentions

```bash
code --list-extensions
# dbaeumer.vscode-eslint
# eamodio.gitlens
# MS-CEINTL.vscode-language-pack-zh-hans
# ms-python.python
# ms-vscode-remote.remote-wsl
# yzhang.markdown-all-in-one

# code --install-extension {{extension-id}}
```

> Where are extensions installed?# Extensions are installed in a per user extensions folder. Depending on your platform, the location is in the following folder:

- Windows %USERPROFILE%.vscode\extensions
- macOS ~/.vscode/extensions
- Linux ~/.vscode/extensions

## 安装 WSl

```bash
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```
