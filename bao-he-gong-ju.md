# 包和工具

### 1. Go语言包简介

***

Go自带100多个包，可以为大多数应用程序提供基础。Go社区是一个茁壮成长的生态环境，其中鼓励包设计、共享、重用以及改进，已经发布的很多包，可以在[https://godoc.org](https://godoc.org)找到。

包管理系统的目的是为了对关联的特性进行分类，组织成便于理解和修改的单元，使其与程序的其它包保持独立，从而有助于设计和维护大型程序。

Go的包管理类似于Java的依赖管理，比如常用的：maven，不同的是，Go的包管理不需要指定GAV坐标，而是需要调用者主动下载包文件。

包通过控制名字是否导出使其对包外可见来提供封装能力，限制包成员的可见性，从而隐藏API后面的辅助函数和类型，允许包的维护者修改包的实现而不影响包外部的代码。限制变量的可见性也可以隐藏变量，这样使用者仅可以通过导出函数来对其访问和更新，它们可以保留自己的不变量以及在并发程序中实现互斥访问。

### 2. 导入路径

***

每一个包都通过一个唯一的的字符串进行标识，它称为导入路径。如下：

```
import (  "fmt"  "math/rand")
```

### 3.包的声明

***

在每一个Go源文件的开头都需要进行包生明，它的主要目的是当该包被其他包引入的时候作为其默认的标识符。

例如，math/rand包中每一个文件的开头都是package rand，这样当你导入这个包时，可以访问它的成员，比如：rand.Int、rand.Float64等。

```
package main​import( "fmt" "math/rand")​func main(){    fmt.Println(rand.Int())}
```

通常来说，默认的包名就是包导入路径名的最后一段，因此即使两个包的导入路径不同，它 们依然可能有一个相同的包名。例如，math/rand包和crypto/rand包的包名都是rand。如果我们想同时导入两个有着名字相同的包，例如math/rand包和crypto/rand包，那么导入声 明必须至少为一个同名包指定一个新的包名以避免冲突。这叫做导入包的重命名。

```
// 导入包的重命名只影响当前的源文件。其它的源文件如果导入了相同的包，可以用导入包原// 本默认的名字或重命名为另一个完全不同的名字。import(  "crypto/rand"  mrand "math/rand")
```

### 4. 空导入

***

如果导入的包的名字没有在文件中引用，就会引起一个编译错误。但是有时候，我们必须导入一个包，这仅仅是为了利用其副作用：对包级别的变量执行初始化表达式求值，并执行它的init函数。为了防止：“unused import”错误，我们必须使用一个重命名导入，它使用一个替代的名字\_,这表示导入的内容为空白标识符。通常情况下，空白标识符不可能被引用。

```
import _ "image/png" *// 注册PNG解码器*
```

### 5. 工具

***

Go工具将不同种类的的工具合并为一个命名集，它是一个包管理器（类似于apt或者rpm）,它可以查询包的作者，计算其依赖关系，从远程版本控制系统下载它们。

以下列出了常用的命令：

```
$ go        bug         start a bug report    build       compile packages and dependencies    clean       remove object files and cached files    doc         show documentation for package or symbol    env         print Go environment information    fix         update packages to use new APIs    fmt         gofmt (reformat) package sources    generate    generate Go files by processing source    get         add dependencies to current module and install them    install     compile and install packages and dependencies    list        list packages or modules    mod         module maintenance    run         compile and run Go program    test        test packages    tool        run specified go tool    version     print Go version    vet         report likely mistakes in packages
```

### 6. 工作空间的组织

***

大部分用户必须进行的唯一配置是：GOPATH环境变量，它指定工作空间的根目录。

GOPATH下有三个子目录：

**src** 子目录包含源文件，每一个包放在一个目录中，该目录相对于$GOPATH/src的名字是包的导入路径。

**pkg** 子目录是构建工具存储编译后的包的位置。

**bin** 目录是存放可执行程序的目录。

### 7. 包的下载

***

如果使用go工具，包的导入路径不仅指示了如何在本地空间中找到他的位置，还指明了通过互联网使用go get来获取和更新它的位置。

go get命令可以下载单一的包，也可以使用...符号来下载子树或仓库。

go get命令已经支持多个流行的代码托管站点，如GitHub、BitBucket、launchpad,并且可以向版本控制系统发出合适的请求。

### 8. 包的构建

go build命令编译每一个命令行参数中的包。如果包的名字是main,go build调用链接器在当前目录中创建可执行程序，可执行程序的名字取自包的导入路径的最后一段。

包可以通过指定目录来指定，可以使用导入路径或者一个相对目录名，目录必须以.或..开头。

如下所示：

```
cd anywherego build gopl.io/njn/jbnj
```

\


