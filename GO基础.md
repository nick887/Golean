# GO基础

## 1.基础知识

### 1.1项目结构

```shell
src
└── go_code
    └── project_1
        ├── main
        │   └── main.go
        └── package
```

- go中每个文件都要属于一个包
- go项目的编译`go build *.go`生成一个可执行文件
- `go build -o abc *.go`自定义输出文件名字
- 直接运行程序`go run *.go`
- 执行流程

![avatar](https://github.com/nick887/GoLearnPic/blob/main/%E6%88%AA%E5%B1%8F2021-04-20%20%E4%B8%8B%E5%8D%884.32.18.png)

### 1.2开发注意事项

1. 源文件已`.go`结尾
2. 执行入口是`main`函数
3. **go语言自动在语句后加**`;`
4. 不能把多条语句放在同一行
5. 文件中存在定义了未用到的变量与未用到的包，编译无法通过

### 1.3常见转义字符

```
\t :制表
\n :换行
\\ :\
\" :"
\r :回车,回到当前行的最前面开始输出，覆盖之前内容
```

### 1.4注释

```go
//行注释
/*块注释
*/
```

### 1.5代码风格格式化

`gofmt *.go`

### 1.6GO参考链接

文档中文网：https://studygolang.com/pkgdoc

## 2.类型

### 2.1变量

- 使用变量的三种方式
  1. `var a int`显示声明，不初始化则为默认值
  2. 根据值自动判断类型
  3. 使用`:=`省略`var`自动判断类型

- 多变量声明

  同一种类型

  `var i1,i2,i3 int`

  不同类型

  `var i1, i2, i3 = 100, "2308", 10.111`

  `i1, i2, i3 := 100, "2308", 10.111`

- 声明变量的另一种方式

  ```
  var (
  i1 = 100
  i2 = 300
  i3 = "400"
  )
  ```

- 函数外定义的变量为全局变量

- 变量在第一次赋值后就确定了数据类型，之后赋值的类型要与确定的数据类型相同

### 2.2变量数据类型

- 基本数据类型

  - 数值型

    - 整数类型

      `int int8 int16 int32 int64 uint uint8 uint16 uint32 uint64 byte`

    - 浮点类型

  - 字符型(使用byte[]来保存单个字母字符)

  - 布尔型

  - 字符串

- 复杂数据类型

  - 指针
  - 数组
  - 结构
  - 管道
  - 函数
  - 切片
  - 接口
  - map

### 2.3整形

1. 整形默认声明为`int`

2. 查看变量类型

   `fmt.Printf("%T", a)`

3. 查看变量字节大小

   `unsafe.Sizeof(a1)`

### 2.4浮点型

分为32位与64位

### 2.5字符

go的字符串是由字节组成而不是传统的字符

使用`byte`保存，输出形式为整数的ascii码

格式化输出使用`%c`

字符之可以存储ascii码中存在的字符

字符使用`''`

### 2.6字符串

注意点:

1. 字符串不可变

2. 使用utf-8

3. 使用反引号输出反应号内内容，不转义不报错

   ```
   a := `"ab"""c"`
   fmt.Println(a)
   ```

4. 可以当拼接字符串过长时写成如下形式

   ```
   a := `"ab会撒娇大法c"` +
   "asdfjbas" +
   "呜呜呜呜"
   ```

   注意`+`留在单行末尾，否则报错,因为go自动在每行后加入`;`

### 2.7基本数据类型默认值

```
int 0
float32 0
float64 0
bool false
string ""
```

### 2.8 基本数据类型转换

- 所有转换都需要需要显示转换

示例：

```go
var a float64 = 10.000000001
b := int8(a)
fmt.Printf("%T %d", b, b)
```

注意点：

1. 原变量类型并未改变
2. int64转换int32编译不报错，按溢出处理

- 基本数据类型与string类型的转换(重要)

1. `fmt.Sprintf("%参数",表达式)`返回转换后的字符串

   ```java
   	var num1 int = 99
   	var num2 float64 = 10.1
   	b := false
   	myChar := 'a'
   	var s string
   
   	//第一种方式
   	s = fmt.Sprintf("%d", num1)
   	fmt.Println(s)
   	s = fmt.Sprintf("%f", num2)
   	fmt.Println(s)
   	s = fmt.Sprintf("%t", b)
   	fmt.Println(s)
   	s = fmt.Sprintf("%c", myChar)
   	fmt.Println(s)
   ```

2. `strconv`包下的函数

   ```go
   	s = strconv.FormatInt(int64(num1), 10)
   	fmt.Println(s)
   //简单的转换函数
   	s = strconv.Itoa(10)
   	fmt.Println(s)
   	//'f'代表格式,10代表精度,64表示表示是float64
   	s = strconv.FormatFloat(num2, 'f', 10, 64)
   	fmt.Println(s)
   	s = strconv.FormatBool(b)
   	fmt.Println(s)
   ```

- `string`转基本类型

  ```go
  	//s为0，false，False，FALSE都合法
    s = "0"
  	x, err := strconv.ParseBool(s)
  	fmt.Println(x, err)
  //其他可通过查文档找到
  ```

  ⚠️：当数据类型无效时直接转为0或者false，要确保传入参数有效

### 2.9指针

和c语言大致相同

### 2.10值类型与引用类型

基础类型与数组，结构体为值类型

指针，slice切片，map，管道，interface为引用类型

值类型特点：

1. 变量存储值，内存通常在栈中分配

引用类型特点：

1. 存储地址引用，由GC回收，内存通常在堆上分配

### 2.11命名规范

1. 由字母数字`_`组成
2. 数字不可以开头
3. 严格区分大小写
4. 单独的`_`仅能作为占位符使用，不能作为标识符

### 2.12其他规范

1. 包名与文件夹名称相同
2. 采用驼峰命名法
3. 首字母大写为公有，小写为私有，适用于变量，函数

### 2.13常量

用`const`修饰

批量赋值

```go
const(
	a=100
	b
	c)//此时b,c都是100
```

`iota`使用

```go
const(
	a=iota
	b=100
	c=iota)//a=0,b=100,c=2 const每增加一行常量声明iota+1
```



## 3运算

### 3.1算数运算符

和c语言的运算符基本类似

⚠️：

1. `++` `--`操作需要独立出来,不能写在控制，赋值语句中
2. go中没有`--a`与`++a`这样的操作

### 3.2关系运算符

与java基本相同

### 3.3逻辑运算符

与java基本相同

短路机制与java相同

### 3.4赋值运算符

与java基本相同

其他略，基本相同

## 4IO

### 4.1获取用户输入

```go
var s string
fmt.Scanln(&s)
fmt.Println(s)
//第二种
var s string
var s1 string
fmt.Scanf("%s %s", &s, &s1)
fmt.Println(s, s1)
```

推荐使用第一种

## 5流程控制

⚠️：

1. if,for后没有括号
2. 没有while，用for实现 for后不加参数，通过break终止

## 6打包与工具链

### 6.1重命名导入

```go
import myfmt "github.com/nick/fmt"
```

### 6.2init函数

每个包包含任意多个init函数，会在程序开始执行前调用

### 6.3常用命令

```shell
go doc 包名//显示文档
```

## 7.数组、切片与映射

### 7.1数组基本功能

- 声明与初始化

  `var array [5]int`

  声明后数组长度与类型不变

- 初始化

  1. `array := [5]int{1, 2, 3, 4, 5}`
  2. 自动设置长度``array := [...]int{1, 2, 3, 4, 5}`
  3. 声明特定位置值`array := [5]int{1:5,2:7}`

- 数组赋值

  两个数组长度与数据类型一样可以互相赋值`array1=array2`

- 多维数组

  ```go
  // 声明一个二维整型数组，两个维度分别存储 4 个元素和 2 个元素
      var array [4][2]int
  // 使用数组字面量来声明并初始化一个二维整型数组
  array := [4][2]int{{10, 11}, {20, 21}, {30, 31}, {40, 41}}
  // 声明并初始化外层数组中索引为 1 个和 3 的元素 
  array := [4][2]int{1: {20, 21}, 3: {40, 41}}
  // 声明并初始化外层数组和内层数组的单个元素 
  array := [4][2]int{1: {0: 20}, 3: {1: 41}}
  ```

- 函数传递数组时最好传递数组的指针

  `func foo(array *[1e6]int)`

### 7.2切片

#### 7.2.1初始化

- 创建`slice := make([]string, 5)`指定底层数组容量与长度都是5
- 指定不同的容量与长度`slice := make([]int,3, 5)`长度为3，容量为5，不允许容量小于长度
- 通过切片字面量声明切片`slice := []string{"Red", "Blue", "Green", "Yellow", "Pink"}`
- `slice := []string{99: ""}`创建长度容量100的切片
- 空切片`slice := []int{}`可以用来表示空集合

#### 7.2.2使用

- 赋值`slice[1] = 25`

- 使用切片创建切片`newSlice := slice[1:3]`

- 容量为5的切片的子切片`slice[1:3]`

  子切片的容量为5-1=4

  子切片的长度为3-1=2

- append操作

![avatar](https://github.com/nick887/GoLearnPic/blob/main/%E6%88%AA%E5%B1%8F2021-04-21%20%E4%B8%8A%E5%8D%8810.13.26.png)

- 限制切片容量

  `slice := source[2:3:4]`

  对于`source[i:j:k]`长度为j-i,容量为k-i

- 限制容量与长度相同可以创建一个新的数组而不是在原有共享底层数组上修改

- 将一个切片增加到另一个切片`append(s1,s2...)`

- 迭代切片

  ```go
  for index, value := range slice {
    fmt.Println("index:", index, "value:", value)
  }//注意每次迭代时是创建元素副本，不会对原数组有影响
  ```

- 内置函数`len()`与`cap()`

  len返回长度，cap返回切片容量

- 组合切片

  切片指向子切片，子切片指向底层数组

### 7.3映射

#### 7.3.1创建与初始化

- `dict :=make(map[string]int)`键类型为string，值类型为int
- `dict := map[string]string{"red":"#da1337}`
- 切片，函数，包含切片的结构类型不能作为键

#### 7.3.2使用

- 只声明的map不能添加键值对
- 判断键是否存在`value,exists :=color["blue"]` exists为bool值
- 判断键是否存在，返回值为对应类型的零值时也不存在

#### 7.3.3迭代

```go
colors := map[string]string{
  "red":  "no",
  "blue": "yes",
}

for key, value := range colors {
  fmt.Println(key, value)
}
```

#### 7.3.4删除

`delete(colors,"red")`删除

## 8类型系统

### 8.1自定义类型

- 定义类型

  ```go
  type user struct {
  	name       string
  	email      string
  	ext        string
  	privileged bool
  }//定义类型
  ```

- 创建

  ```go
  var bill user//属性初始化为0
  bill := user{
    name:       "bill",
    email:      "sakfm",
    ext:        "af",
    privileged: false,
  }//自定义初始值
  bill := user{
    "bill",
    "sakfm",
    "af",
    false,
  }//值的顺序需要和结构声明顺序一致
  ```

- 基于已有类型将其作为新类型的说明

  `type Duration int64`

  不能直接赋值int64会报错，需要使用`dur := Duration(10)`

### 8.2方法

- 定义

  ```go
  func (u user) show() {
  	fmt.Println(u.name)
  	fmt.Println(u.email)
  }
  ```

- 使用

  实例话结构体，调用方法即可

- 分为值引用与指针引用

  值引用创建值的副本，对原属性不影响，指针引用直接改变属性值

- 指针引用例子

  ```go
  func (u *user) show() {
  	u.name = "nick"
  }
  bill := &user{
    "bill",
    "sakfm",
    "af",
    false,
  }
  fmt.Println(bill.name)
  
  bill.show()
  
  fmt.Println(bill.name)
  ```

- ⚠️：不论bill时user实例的值还是地址，都可以调用方法，是否需要访问实例属性只需要在定义函数时确定是否为指针

### 8.3接口

- 实现的接口方法调用对象是指针的情况下，结构体实例化对象的指针是实现了接口的对象，对象的值不是实现接口的对象
- 实现接口方法调用方法是值的情况下，实例化对象与指针都可以是实现接口的对象
- 实现了同一个接口的`a`的类型都可以在传参时用`a`类型表示(多态)

### 8.4嵌入类型

```go
type user struct {
	name  string
	email string
}

func (u user) notify() {
	fmt.Println(u.name, u.email)
}

type admin struct {
	user
	level string
}
//赋值
a := admin{
  user: user{
    name:  "sdaf",
    email: "sadkfjkasf",
  },
  level: "asdfafk",
}
//简单赋值
a := admin{
  user{
    "sdaf",
    "sadkfjkasf",
  },
  "asdfafk",
}
//可以直接通过结构体名称访问方法与变量
```

- 内部类型实现接口之后外部类型对应实现
- 外部类型与内部类型实现同一个接口后优先使用外部实现方法

### 8.5可见性

类型首字母小写为其他包不可见

短类型的如`type a int`这种形式的可以通过本地方法调用返回一个该类型的值

调用方法`a := pkg.New(10)`这种形式

属性首字母小写则其他包调用是不可见

若内部类型属性可见而类型不可见，外部包调用时可以将内部类型的属性视为外部类型的属性进行调用

## 9并发

### 9.1goroutine