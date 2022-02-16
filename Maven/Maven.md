# Maven

[包地址](https://mvnrepository.com/tags/maven)

## Maven简介

```
* 项目构建： 提供标准的、跨平台的自动化项目构建工具
* 依赖管理： 方便快捷的管理项目依赖的资源（jar包），避免资源间的版本冲突
* 统一开发结构： 提供标准的、统一的项目结构
```

![1640241666(1)](./1640241666(1).jpg)

 

## 基本概念

### 仓库

```
仓库的分类：
* 本地仓库
* 远程仓库
	* 中央仓库
	* 私服
```

![1640243424(1)](./1640243424(1).jpg)

### 坐标

```
* 什么是坐标
	* Maven 中坐标用于描述仓库中资源的位置
	* https://repo1.maven.org/maven2/
* Maven坐标主要组成
	groupld: 当前Maven项目隶属组织名称
	artifactld: 当前Maven项目名称
	version: 当前Maven项目版本
	packaging: 打包方式
* Maven坐标的作用：
	唯一标识，定位资源位置，通过坐标找到资源并下载
```

### 仓库配置

```
* 修改maven下的conf.setting.xml

* 配置本地仓库(资源下到哪里, 防止资源包下到了c盘下)
	<localRepository>D:\software\maven\repository</localRepository>

* 配置阿里镜像仓库(需要资源下载源，提升速度)
    <mirror>
      <id>alimaven</id>
      <name>aliyun maven</name>
      <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
      <mirrorOf>central</mirrorOf>        
    </mirror>
```

