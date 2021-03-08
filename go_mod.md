# go mod

[TOC]

## 命令集合

- **go mod init** [module名]：生成 go.mod 文件（最后那个module名是必加的，不加可能会出bug）
- **go mod download** : 下载 go.mod 文件中指明的所有依赖
- **go mod tidy**: 整理现有的依赖
- **go mod graph**: 查看现有的依赖结构
- **go mod edit** : 编辑 go.mod 文件
- **go mod vendor** : 导出项目所有的依赖到vendor目录
- **go mod verify**: 校验一个模块是否被篡改过
- **go mod why**: 查看为什么需要依赖某模块

## 文件go.mod

```
module project1/main

go 1.13

require (
	github.com/labstack/echo v3.3.10+incompatible
	github.com/labstack/gommon v0.3.0 // indirect
	github.com/stretchr/testify v1.5.1 // indirect
	golang.org/x/crypto v0.0.0-20200317142112-1b76d66859c6 // indirect
	project1/help v0.0.0

)

replace project1/help => ../help

```

go.mod 是启用了 Go moduels 的项目所必须的最重要的文件，它描述了当前项目（也就是当前模块）的元信息，每一行都以一个动词开头，目前有以下 5 个动词:

- module：用于定义当前项目的模块路径。
- go：用于设置预期的 Go 版本。
- require：用于设置一个特定的模块版本。（不可使用相对路径）
- exclude：用于从使用中排除一个特定的模块版本。（只适用于go.mod文件所在的模块）
- replace：用于将一个模块版本替换为另外一个模块版本。（只适用于go.mod文件所在的模块，替换者可以是相对路径）

## 文件go.sum

```
github.com/davecgh/go-spew v1.1.0 h1:ZDRjVQ15GmhC3fiQ8ni8+OwkZQO4DARzQgrnXU1Liz8=
github.com/davecgh/go-spew v1.1.0/go.mod h1:J7Y8YcW2NihsgmVo/mv3lAwl/skON4iLHjSsI+c5H38=
github.com/labstack/echo v3.3.10+incompatible h1:pGRcYk231ExFAyoAjAfD85kQzRJCRI8bbnE7CX5OEgg=
github.com/labstack/echo v3.3.10+incompatible/go.mod h1:0INS7j/VjnFxD4E2wkz67b8cVwCLbBmJyDaka6Cmk1s=
... ...
```

go.sum 是类似于比如 dep 的 Gopkg.lock 的一类文件，它详细罗列了当前项目直接或间接依赖的所有模块版本，并写明了那些模块版本的 SHA-256 哈希值以备 Go 在今后的操作中保证项目所依赖的那些模块版本不会被篡改。

我们可以看到一个模块路径可能有如下两种：

```
github.com/davecgh/go-spew v1.1.0 h1:ZDRjVQ15GmhC3fiQ8ni8+OwkZQO4DARzQgrnXU1Liz8=
github.com/davecgh/go-spew v1.1.0/go.mod h1:J7Y8YcW2NihsgmVo/mv3lAwl/skON4iLHjSsI+c5H38=
```

前者为 Go modules 打包整个模块包文件 zip 后再进行 hash 值，而后者为针对 go.mod 的 hash 值。他们两者，要不就是同时存在，要不就是只存在 go.mod hash。

那什么情况下会不存在 zip hash 呢，就是当 Go 认为肯定用不到某个模块版本的时候就会省略它的 zip hash，就会出现不存在 zip hash，只存在 go.mod hash 的情况。

## go mod下的import【待修改】

必须使用`模块名/包名`方法导入包

## 一般使用场景

创建项目后在项目main文件所在目录使用命令`go mod init`建立go.mod文件

当使用第三方库时使用`go mod tidy`命令更新依赖

## 使用过去的版本

使用replace将go.mod中的新版本换为旧版本即可：

原来的go.mod:

```
module github.com/aceld/modules_test

go 1.14

require github.com/aceld/zinx v0.0.0-20200306023939-bc416543ae24 // indirect
```

使用`go mod edit`命令修改go.mod文件：

```shell
go mod edit -replace=zinx@v0.0.0-20200306023939-bc416543ae24=zinx@v0.0.0-20200221135252-8a8954e75100
```

修改后的go.mod文件：

```text
module github.com/aceld/modules_test

go 1.14

require github.com/aceld/zinx v0.0.0-20200306023939-bc416543ae24 // indirect

replace zinx v0.0.0-20200306023939-bc416543ae24 => zinx v0.0.0-20200221135252-8a8954e75100
```

## 解决goland import报红但是可以运行的办法

取消`enable go module`后重新`enable`然后重新打开这个项目

![image-20200320223129066](C:\E\typora\Pictures\image-20200320223129066.png)

## 环境变量GO111MODULE

这个环境变量主要是 Go modules 的开关，主要有以下参数：

- auto：只在项目包含了 go.mod 文件时启用 Go modules，在 Go 1.13 中仍然是默认值
- on：无脑启用 Go modules，推荐设置，未来版本中的默认值，让 GOPATH 从此成为历史。
- off：禁用 Go modules。

## 环境变量GOPROXY

这个环境变量主要是用于设置 Go 模块代理，主要如下：

- 它的值是一个以英文逗号 “,” 分割的 Go module proxy 列表
  - 作用：用于使 Go 在后续拉取模块版本时能够脱离传统的 VCS 方式从镜像站点快速拉取。它拥有一个默认：`https://proxy.golang.org,direct`，但很可惜 `proxy.golang.org` 在中国无法访问，故而建议使用 `goproxy.cn` 作为替代，可以执行语句：`go env -w GOPROXY=https://goproxy.cn,direct`。
  - 设置为 “off” ：禁止 Go 在后续操作中使用任 何 Go module proxy。

刚刚在上面，我们可以发现值列表中有 “direct” ，它又有什么作用呢。其实值列表中的 “direct” 为特殊指示符，用于指示 Go 回源到模块版本的源地址去抓取(比如 GitHub 等)，当值列表中上一个 Go module proxy 返回 404 或 410 错误时，Go 自动尝试列表中的下一个，遇见 “direct” 时回源，遇见 EOF 时终止并抛出类似 “invalid version: unknown revision...” 的错误。

## 环境变量GOSUMDB

它的值是一个 Go checksum database，用于使 Go 在拉取模块版本时(无论是从源站拉取还是通过 Go module proxy 拉取)保证拉取到的模块版本数据未经篡改，也可以是“off”即禁止 Go 在后续操作中校验模块版本

- 格式 1：`<SUMDB_NAME>+<PUBLIC_KEY>`。
- 格式 2：`<SUMDB_NAME>+<PUBLIC_KEY> <SUMDB_URL>`。
- 拥有默认值：`sum.golang.org` (之所以没有按照上面的格式是因为 Go 对默认值做了特殊处理)。
- 可被 Go module proxy 代理 (详见：Proxying a Checksum Database)。
- `sum.golang.org` 在中国无法访问，故而更加建议将 GOPROXY 设置为 `goproxy.cn`，因为 `goproxy.cn` 支持代理 `sum.golang.org`。

## 环境变量GONOPROXY/GONOSUMDB/GOPRIVATE

这三个环境变量都是用在当前项目依赖了私有模块，也就是依赖了由 GOPROXY 指定的 Go module proxy 或由 GOSUMDB 指定 Go checksum database 无法访问到的模块时的场景

- 它们三个的值都是一个以英文逗号 “,” 分割的模块路径前缀，匹配规则同 path.Match。
- 其中 GOPRIVATE 较为特殊，它的值将作为 GONOPROXY 和 GONOSUMDB 的默认值，所以建议的最佳姿势是只是用 GOPRIVATE。

在使用上来讲，比如 `GOPRIVATE=*.corp.example.com` 表示所有模块路径以 `corp.example.com` 的下一级域名 (如 `team1.corp.example.com`) 为前缀的模块版本都将不经过 Go module proxy 和 Go checksum database，需要注意的是不包括 `corp.example.com` 本身。



