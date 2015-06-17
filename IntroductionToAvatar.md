Avatar - 淘宝阿凡达 基于软件包的应用服务克隆系统



## 系统介绍 ##

淘宝阿凡达是一个基于软件包管理的应用快照和克隆系统，利用软件包管理器的特性实现了对应用节点快照和还原功能，它可以把应用服务节点的软件包状态抓取下来存成文本文件，用户可以根据这个文本文件记录的信息使用阿凡达程序从淘宝的软件仓库服务器中获取相应的软件包安装到操作系统上，从而达到了应用克隆的功能。

一个容易理解的比喻是——“阿凡达系统可以提取应用服务的DNA信息并克隆出该应用服务”。

系统设计目的是为了简化部署流程、提高部署精度和速度；对于日常生产运维的直接收益就是“将现有复杂的安装命令简化至一条\*短\*命令”，从而大大降低了安装部署时操作失误引发的风险和复杂命令和不同场景行为差异带来的困惑；通过自身的功能和特征，阿凡达系统将使得工程师对应用和服务的重塑、还原、克隆、回滚等工作变得异常简单，使得开发到测试、生产之间的应用传递环节和过程变得更加简洁透明和轻松，达到大幅降低沟通和管理成本的战略目标。

## 系统功能范围 ##
  * 承担的功能：用于进行操作系统软件包、应用软件包的安装部署、还原及回滚
  * 不承担的功能：操作系统本身的安装 应用软件使用上的配置调整和改动（视应用配置复杂程度而定）

## 系统功能概要 ##

  1. 构建应用的软件包模型的存储形式为"单一文本文件"
  1. 文件版本控制和日志记录
  1. 软件包模型信息可由中央服务器任意获取
  1. 多用户操作及用户操作权限控制
  1. 命名空间支持Range表达式
  1. 软件包快照抓取方式CLI和WEB
  1. 可通过CLI或WEB提交软件包快照文件


## 系统架构图 ##

![http://taobao-avatar.googlecode.com/files/SystemStructure.png](http://taobao-avatar.googlecode.com/files/SystemStructure.png)

## 用户/系统流程图 ##



## 软件包与软件仓库 ##

淘宝目前使用的是RPM软件包，用yum作为软件包分发服务器 软件包信息可以在用户所建yum server上查询

## 命名空间(namespace)说明 ##

命名空间可以认为是一个应用模型的名称，作为识别avatar的标示

按主机名 例如：

`dumyhost.cm4`

主机组 例如：

`item_center_cm4`

自定义命名 例如：

`httpd-server`

语法支持range, 例如：

`abc[1-10,15].cm5`

## 系统软件包和应用软件包的分离 ##

RedHat系统上安装软件包数量庞大，我们利用RPM的Group已有命名和指定自定义软件包的方式实现 "石头"与"沙子"的分离，

## 软件包状态信息格式 ##

  * namespace 即为 命名空间

  * timestamp 为 该状态信息最后更改时间

  * base\_rpms\_num 为 系统软件包的数量

需为完整描述(包名-版本号-发行号.架构), 例如：

> libdbi-dbd-pgsql-0.8.1a-1.2.2.x86\_64

  * cust\_rpms\_num 为 自定义软件包的数量

需为完整描述(包名-版本号-发行号.架构), 例如：

> tsar-2.1.0-1250.el5.x86\_64

  * base\_rpms 为 系统软件包明细

  * cust\_rpms 为 自定义软件包明细

  * diff\_files 为 所要打patch的文件明细

格式为 文件名1 空格 文件名2 例：

`/usr/local/tsar/conf/tsar.conf.diff /home/a/share/ECPM/Merge/conf/merge.conf.diff`

**diff实体内容 在###END OF AVATAR###后为diff实体内容**

格式为：文件名 换行 diff内容 换行 下一个文件名 换行 内容 …… 例：

`/home/a/share/ECPM/Merge/conf/merge.conf.diff`

## 阿凡达信息的继承功能 ##

配置继承是阿凡达系统提供的一个便利功能，能够基于已有的avatar信息进行增删配置的条目

例：


## Web管理界面 ##

可以在Web界面上添加、搜索、编辑Avatar信息

## 用户使用手册 ##

注意：应用克隆尽量在对等操作系统之间进行已达到准确、快捷效果

  1. 构建应用服务原型机
  1. 使用RPM软件包安装应用服务程序
  1. 编辑应用服务配置

  * 获取应用原型机软件快照

  * 安装快照抓取程序avatar
`yum install avatar -b current`
  * 抓取快照
`avatar -n [namespace]`

  * 使用快照克隆
  * 安装克隆程序walle
`yum install avatar -b current`
  * 克隆应用
`walle -n [namespace]`

## 应用情景与案例 ##

  * 直接抓取提交(以主机名做namespace) avatar
  * 保存avatar至本地 avatar -s
  * 提交已抓获avatar文件 avatar -f [filename](filename.md)
  * 指定namespace 抓取 avatar -n
  * 根据默认avatar还原(以主机名做namespace) walle
  * 根据avatar文件还原 walle -n [namespace](namespace.md)
  * 编辑avatar文件 vi [namespace](namespace.md).avatar
  * 在线编辑avatar
  * 版本控制 覆盖提交/根据版本还原 walle -n [namespace](namespace.md) -r [revision](revision.md)
  * 批量部署/安装/还原/回滚
`assh -h host[1-100].cm4 "wall -n click_page"`

## Avatar配置存储目录 TreeOfSouls ##


## API接口 ##


GET Avatar 用于获取软件包模型信息内容

参数 namespace 命名空间 revision 版本号 不指定则为last revision


POST Avatar 用于接收软件包信息内容


## 版本说明 ##

avatar 和 walle属于与系统版本架构无关的程序，avatar需要LWP/MIME/Term/HTTP等常见PerlModule，通常已安装，如不存通过yum会自动安装 walle需要bash > 3.0

name-major-junior-patch-release

major是主版本，现在定义为 0

junior是次版本现在定义为 1

patch是补丁版本

release代表发行程序用途 目前内测版是alpha

版本号随发行递增