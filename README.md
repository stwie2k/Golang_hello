# 在Ubuntu上使用golang



---
Go语言是一门强类型的通用编程语言。它的基础语法与C语言很类似，但对于变量的声明有所不同，也对其他的一些优秀编程语言有所借鉴。另外，Go语言支持垃圾回收。

与C++相比，Go语言并不包括如异常处理、继承、泛型、断言、虚函数等功能，但增加了 Slice 型、并发、管道、垃圾回收、接口（Interface）等特性的语言级支持。

Go语言对并发编程的支持是天生的、自然的和高效的。Go语言为此专门创造出了一个关键字“go”。使用这个关键字，我们就可以很容易的使一个函数被并发的执行。

Go是一门以软件工程为目的的语言设计:


 - 快速编译
 - 严格的依赖管理
 - 代码风格的强一致性
 - 偏向组合而不是继承


 
由于go语言的种种特点，其成为了云计算和服务计算开发的首选。所以借此机会，我们也学习了go语言在linux系统下的安装编写，在这里逐一介绍。
 
---------------------


## 一、安装vscode
作为一款轻量级的文本编辑器，vscode一出世就引发了许多人的关注。事实上，在使用了多款编辑器之后，我发觉vscode的速度在其中是数一数二，因此这次在linux上也选择其作为go语言开发的平台。
要安装vscode，首先要安装umake。打开终端，使用下列命令，通过官方PPA来安装Ubuntu Make：

    sudo add-apt-repository ppa:ubuntu-desktop/ubuntu-make
    sudo apt-get update
    sudo apt-get install ubuntu-make
之后再通过umake来vscode

    umake web visual-studio-code

但在这一步却出现报错
![此处输入图片的描述][1]

提示架构冲突。索性直接去微软官网下载deb安装，简单快捷。这样我们就可以直接打开vscode了。
![此处输入图片的描述][2]


## 二、安装Golang

1、下载安装

 - 终端输入命令



    sudo apt-get install golang

 - 查看go版本 
![此处输入图片的描述][3]


2、环境配置

 - 查看环境变量
![此处输入图片的描述][4]
 
默认的时候，GOPATH是没有配置好的，那么我们怎么修改这些环境变量呢

首先，我们可以任选一个位置建一个文件夹，比如

    mkdir /home/kloijm30/go
    
之后打开配置文件

    sudo gedit /etc/profile 

在最后添加export GOPATH=/home/kloijm30/go
以及export PATH=$PATH:/home/kloijm30/go/bin


## 三、创建程序并编译

1、编译并运行简单的程序，首先要选择包路径（我们在这里使用 github.com/user/hello），并在你的工作空间内创建相应的包目录：

    $ mkdir $GOPATH/src/github.com/user/hello

然后用vscode新建文档hello.go

    package main
    
    import "fmt"
    
    func main() {
    	fmt.Printf("Hello, world.\n")
    }
然后直接在vscode中打开终端，进入目录输入

    go run hello.go
这样就可以看到运行结果了

![此处输入图片的描述][5]

2、构建安装程序
在任何位置输入命令

    $ go install github.com/user/hello
这样在bin文件夹中就可以看到名为hello的二进制文件了
![此处输入图片的描述][6]


之后我们就可以直接输入二进制文件名来运行了
![此处输入图片的描述][7]


## 四、编写一个Golang库

1、先选择包路径（我们将使用 github.com/user/stringutil） 并创建包目录：

    $ mkdir $GOPATH/src/github.com/user/stringutil
    
2、在该目录中创建名为 reverse.go 的文件，内容如下：

    // stringutil 包含有用于处理字符串的工具函数。
    package stringutil
    
    // Reverse 将其实参字符串以符文为单位左右反转。
    func Reverse(s string) string {
    	r := []rune(s)
    	for i, j := 0, len(r)-1; i < len(r)/2; i, j = i+1, j-1 {
    		r[i], r[j] = r[j], r[i]
    	}
    	return string(r)
    }
3、修改hello.go文件为

    package main
    
    import (
    	"fmt"
    
    	"github.com/user/stringutil"
    )
    
    func main() {
    	fmt.Printf(stringutil.Reverse("!oG ,olleH"))
    }
4、再重新安装hello.go

    go install github.com/user/hello

此时运行hello
![此处输入图片的描述][8]

输出了逆向的字符，说明自己的库使用成功。

此时的空间格局为
![此处输入图片的描述][9]


## 五、Go语言测试
Go拥有一个轻量级的测试框架，它由 go test 命令和 testing 包构成。

你可以通过创建一个名字以 _test.go 结尾的，包含名为 TestXXX 且签名为 func (t *testing.T) 函数的文件来编写测试。 

在 $GOPATH/src/github.com/user/stringutil 中创建reverse_test.go 

    package stringutil
    
    import "testing"
    
    func TestReverse(t *testing.T) {
    	cases := []struct {
    		in, want string
    	}{
    		{"Hello, world", "dlrow ,olleH"},
    		{"Hello, 世界", "界世 ,olleH"},
    		{"", ""},
    	}
    	for _, c := range cases {
    		got := Reverse(c.in)
    		if got != c.want {
    			t.Errorf("Reverse(%q) == %q, want %q", c.in, got, c.want)
    		}
    	}
    }
    
   用go test测试
   ![此处输入图片的描述][10]
   
   这样便表示测试成功了。
   

  [8]: https://i.loli.net/2018/09/24/5ba8fd957319b7]: https://i.loli.net/2018/09/24/5ba8ee1e8f013.png


  [1]: https://i.loli.net/2018/09/24/5ba8b15eed940.png
  [2]: https://i.loli.net/2018/09/24/5ba8b231c3b54.png
  [3]: https://i.loli.net/2018/09/24/5ba8b7a4200f5.png
  [4]: https://i.loli.net/2018/09/24/5ba8b80b811ac.png
  [5]: https://i.loli.net/2018/09/24/5ba8eb831470d.png
  [6]: https://i.loli.net/2018/09/24/5ba8edf7c8dd5.png
  [7]: https://i.loli.net/2018/09/24/5ba8ee1e8f013.png
  [8]: https://i.loli.net/2018/09/24/5ba8fddbb9953.png
  [9]: https://i.loli.net/2018/09/24/5ba8fddbb9953.png
  [10]: https://i.loli.net/2018/09/27/5bacf69d2ef5e.png
