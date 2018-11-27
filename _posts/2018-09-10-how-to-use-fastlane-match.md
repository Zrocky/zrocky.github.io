---
layout: post
title: fastlane之使用match同步证书和配置文件
date: 2018-09-10 09:26:40
tags: [Fastlane, match]
categories: iOS
---


在开发过程中，证书和配置文件的管理一直是一个让人头痛的问题，不管是Xcode自动创建的众多让人摸不着头脑的配置文件，还是不断被其它人`revoke`的证书，这些场景无不让人想要一个简单、彻底的解决方案， `match`正是为解决这些问题而开发的。

`match`的方案是只创建一份`code sign`所需文件，并使用`Git`在团队内共享它们。

`match`是`https://codesigning.guide`概念的实现。`match`创建所有必需的证书和配置文件，并将它们存储在单独的git存储库中。每个有权访问仓库的团队成员都可以使用这些凭据进行代码签名。`match`还会自动修复损坏和过期的凭据。这是跨团队共享签名凭据的最简单方法。



## 基本使用

### 准备

1. 创建一个Git仓库**(务必保证是私有的，安全很重要)**, 取名为`certificates`       
2. `(非必要)`创建一个共享的Apple Developer账号
3. 在项目目录中执行`Shell`命令:

```shell
fastlane match init
```
    
![match_init](/../assets/images/post/2018-09-07-match_init.gif)
    
执行命令后会要求输入Git仓库地址, 可以是`https://或git`URL（如果你是使用SSH对Git进行身份验证，则需要使用gitURL，否则在尝试匹配时将看会身份验证错误）。

`fastlane match init`命令不会获取或修改你的证书和配置文件, 只会在`./fastlane`目录生成一个`Matchfile`文件。  
    
`Matchfile`内配置示例:
    
```shell
git_url("https://github.com/fastlane/certificates")
 
app_identifier("tools.fastlane.app")
# username("user@fastlane.tools")
```
    
文件内记录了证书和配置文件的Git仓库地址及`match`命令的相关配置，可以根据你的需求来修改它们。
        
### 生成证书和配置文件

> 在首次运行前, 可以使用`fastlane match nuke`命令清除现有的配置文件和证书。[(了解更多)](#清除现有配置文件和证书)

你可以使用以下命令来生成证书:

```shell
# 开发证书及配置文件
fastlane match development 

# 生成证书及内部分发配置文件
fastlane match adhoc

# 生产证书及配置文件
fastlane match appstore
```

![match_appstore_small](/../assets/images/post/2018-09-10-match_appstore_small.gif)

这将创建一个新的证书和配置文件（如果需要的话）并将它们存储在你的[Git仓库](# Git Repo)中。

配置文件将安装在`~/Library/MobileDevice/Provisioning Profiles`中，证书和私钥安装在你的钥匙串中。  

#### 处理多个标识符

如果你有多个标识符，则可以将同步操作换成以下命令执行，不同标识符之间使用逗号隔开

```shell
fastlane match appstore -a tools.fastlane.app,tools.fastlane.app.watchkitapp 
```

当然，你也可以使用`lane`来处理以上操作：

```shell
lane :certificates do
  match(app_identifier: ["com.krausefx.app1", "com.krausefx.app2", "com.krausefx.app3"], type: appstrore, readonly: true)
end
```

 这样，你就只需执行`fantlane certificates`命令即可完成操作
 
### 其他成员执行同步

其他成员需要同步证书和配置文件，只需执行相同的命令：

```shell
fastlane match development
```

它会识别Git仓库中是否有现成的证书和配置文件，如果有，就会安装仓库中现有的证书和配置文件。

你甚至可以在某个模式下执行`match`操作，`readonly`用来确保不会生成证书和配置文件

```shell
fastlane match development --readonly
```

## 扩展使用

### 在lane中使用match

添加`match`方法到`Fastfile`中，则可以使用`lane`来快速同步证书和配置文件

```shell
match(type: "appstore")

match(git_url: "https://github.com/fastlane/certificates",
      type: "development")

match(git_url: "https://github.com/fastlane/certificates",
      type: "adhoc",
      app_identifier: "tools.fastlane.app")

match(git_url: "https://github.com/fastlane/certificates",
      type: "enterprise",
      app_identifier: "tools.fastlane.app")

# _match_ should be called before building the app with _gym_
gym
# ...
```

### 注册新设备

通过使用`match`，每次将新设备添加到Ad Hoc或开发配置文件时都会节省大量的时间。将`match`与`register_devices`操作结合使用。

```shell
lane :beta do
  register_devices(devices_file: "./devices.txt")
  match(type: "adhoc", force_for_new_devices: true)
end
```

通过使用`force_for_new_devices`参数，`match`将检查自上次运行匹配以来，设备计数是否已更改，并在必要时自动重新生成配置文件。你也可以使用`force: true`参数强制每次运行时都重新生成配置文件。  

**要点**：App Store配置文件将会忽略`force_for_new_devices`参数，因为这种模式不包含任何设备信息。  

如果你不使用`fastlane`，还可以使用命令行中的`force_for_new_devices`选项：

```shell
fastlane match adhoc --force_for_new_devices
```

### 清除现有配置文件和证书

如果你的Apple Developer账户中有大量无效、过期或者Xcode自动生成的配置文件和证书，可以使用`match nuke`命令撤销证书和配置文件，不必担心，App Store / TestFlight中已有的应用程序仍然可以使用，推送证书也不在清理范围内。清除账户后，你就可以从零开始，运行`match`生成证书和配置文件。

要撤消特定环境的所有证书和配置文件：

```shell
fastlane match nuke development
fastlane match nuke distribution
fastlane match nuke enterprise
```

![match_nuke](/../assets/images/post/2018-09-10-match_nuke.gif)

`match`会在操作前会向你确认操作是否执行

### Git 仓库

第一次运行`match`后，你的Git仓库将包含2个目录：
   
* `certs`文件夹包含所有带有私钥的证书
* `profiles`文件夹包含所有配置文件

此外，`match`还生成了一个README.md文件，让新的团队成员更容易上手：

![github_repo](/../assets/images/post/2018-09-10-github_repo.png)

### 参数

键 | 描述 | 默认值
--- | --- | ---
git_url | 包含所有证书的git仓库的URL |  
git_branch | 使用特定的git分支 | master
type | 定义配置文件类型，可以是appstore，adhoc，development，enterprise | development
app_identifier |	应用的捆绑标识符（以逗号分隔）| *
username |	您的Apple ID用户名 | *
keychain_name | 钥匙串应该导入项目	| login.keychain
keychain_password	 | 首次访问新mac上的证书时可能需要这样做。对于登录/默认钥匙串，这是您的帐户密码 |  	
readonly	 | 仅获取现有证书和配置文件，不生成新证书和配置文件 | false
team_id	| 如果您在多个团队中，您的Developer Portal团队的ID	| *
git_full_name | 要提交的git用户全名 |  
git_user_email	git | 用户电子邮件提交 | 
team_name | 如果您在多个团队中，您的Developer Portal团队的名称 | *
verbose | 打印出额外的信息和所有命令 | false
force	 | 每次运行匹配时都要更新配置文件 | false
skip_confirmation | 在核弹期间禁用确认提示，并回答是 | false
shallow_clone | 对存储库进行浅层克隆（将历史记录截断为1个版本）| false
clone_branch_directly | 克隆指定的分支，而不是整个repo。这要求分支已经存在。否则命令将失败 |false
force_for_new_devices | 如果开发人员门户上的设备计数已更改，请续订配置文件。忽略配置文件类型'appstore' | false
skip_docs | 跳过为创建的git存储库生成README.md | false
platform	 | 设置供应配置文件的平台以使用（即ios，tvos） | ios
template_name | 配置文件模板的名称。如果开发者帐户具有配置配置文件模板（又名：自定义权利），则可以通过在创建/编辑配置文件时检查权利下拉列表来找到模板名称（例如“Apple Pay Pass Suppression Development”） | 

## 参考链接

1. [match官方文档](https://docs.fastlane.tools/actions/match/)


