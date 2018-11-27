---
layout: post
title: iOS Develop Guideline
date: 2016-05-03 18:00:00
tags: iOS
categories: iOS
---

**本规范因为Xcode版本的更迭，部分内容已经不太适用。本文仅供参考。**

以下是我们公司在iOS开发前需要了解的开发规范, 总结自日常的开发过程和团队所有的经验.

## 开发工具

以下列举了我们公司在iOS开发中经常使用到工具, 并附上地址, 原则上需要全部安装

*	[Xcode 历史版本](https://developer.apple.com/downloads/)
* 	[RubyGems (Ruby环境)](https://ruby.taobao.org/)
* 	[Alcatraz (Xcode开发工具安装包)](http://alcatraz.io/)

*以下插件均可在Alcatraz插件安装包中下载安装*

*	[Peckham (头文件快捷导入)](https://github.com/markohlebar/Peckham)
* 	[cocoapods plugin (cocoapods插件)](https://github.com/kattrali/cocoapods-xcode-plugin)
*	[VVDocumenter (文档快捷注释)](https://github.com/Zrocky/VVDocumenter-Xcode)

## 文件规范

```
- Project
 	- Classes
 		- Tool
 		- Expand
 			- Macros
 				- Macro.h
 				- A Macro.h
 				- B Macro.h
 			- Const
 				- Const.h
 				- A Const.h
 				- B Const.h
			- DataBase
 			- CoreData
 		- Category
 			- UI
 				- UIView
 				- UIButton
 				- UIViewController
 				- UITableView
 				- UIImage
 			- Data
 				- NSDate
 				- NSString
 		- NetworkCenter
 			- APIManager
 				- A Module
 				- B Module
 			- Foundation
 		- Vender
 	- Resource
 		- Image
 		- Font
 		- Video
 		- Audio
```

## 代码区块规范

### Controller代码区块规范

```
#pragma mark - life cycle
- (void)viewDidLoad {
	[super viewDidLoad];

    [self setupLayouts];
}
- (void)setupLayouts {
}
- (void)viewWillAppear:(BOOL)animated {
     [super viewWillAppear:animated];
}
#pragma mark - delegate
#pragma mark - event response
#pragma mark - public methods
#pragma mark - private methods
#pragma mark - setter and getter
```

### View代码区块规范

```
#pragma mark - life cycle
- (instancetype)initWithFrame:(CGRect)frame {
     if (self = [super initWithFrame:frame]) {
         [self setupSubviews];
         [self setupLayouts];
     }
     return self;
}
- (void)setupSubviews {
}
- (void)setupLayouts {
}
#pragma mark - delegate
#pragma mark - event response
#pragma mark - public methods
#pragma mark - private methods
#pragma mark - setter and getter
```

