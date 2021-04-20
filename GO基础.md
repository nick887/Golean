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

## 2.语法

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

  

