# golang笔记

[TOC]

# 零、问题

1. golang slice怎么保持cap不变的情况下将len设置为0

   ```go
   a := []int {1,2,3,4}
   a = a[0:0]
   fmt.Println(len(a), cap(a)) // 0 4
   ```




## 犯过的错

1. 切片内直接存放结构体而不是指针
2. 不检查是否ok直接对map内的指针进行使用
3. 写错关键算式
4. 链表尾结点没有设置next为nil

# 一、入门

1. Go语言不需要在语句或者声明的末尾添加分号，除非一行上有多条语句。实际上，编译器会主动把特定符号后的换行符转换为分号，因此换行符添加的位置会影响Go代码的正确解析

2. golang没有指针运算，不能对指针进行加减

3. 名字的开头字母的大小写决定了名字在包外的可见性。如果一个名字是大写字母开头的，那么它将是导出的，也就是说可以被外部的包访问。

4. Go语言将数据类型分为四类：**基础类型**、**复合类型**、**引用类型**和**接口类型**
   - 基础类型：：数字、字符串和布尔型
   - 复合类型：数组和结构体
   - 引用类型：指针、切片、字典、函数、通道（它们都是对程序中一个变量或状态的间接引用。这意味着对任一引用类型数据的修改都会影响所有该引用的拷贝。）
   - 接口类型：接口
   
5. 变量才可以取地址，值才不可以取地址

6. **表达式不可取地址**

   ```go
   a := 2.2
   b := &int(a)
   // Cannot take the address of 'int(a)'
   ```

## 基础语法

### for的三种循环方式

```go
for initialization; condition; post {
    // zero or more statements
}

// a traditional "while" loop
for condition {
    // ...
}

// a traditional infinite loop
for {
    // ...
}
```

### 变量声明的三种方式

实践中一般使用前两种形式中的某个，初始值重要的话就显式地指定变量的类型，否则使用隐式初始化。

```go
s := ""

var s string
s = ""

var s = ""
```


### switch语句（不需要加break）

```go
switch coinflip() {
case "heads":
    heads++
case "tails":
    tails++
default:
    fmt.Println("landed on edge!")
}
```

Go语言里的switch还可以不带操作对象（译注：switch不带操作对象时默认用true值代替，然后将每个case的表达式和true值进行比较）；可以直接罗列多种条件，像其它语言里面的多个if else一样，下面是一个例子：

```go
func Signum(x int) int {
    switch {
    case x > 0:
        return +1
    default:
        return 0
    case x < 0:
        return -1
    }
}
```

### 关键字

```
break      default       func     interface   select
case       defer         go       map         struct
chan       else          goto     package     switch
const      fallthrough   if       range       type
continue   for           import   return      var
```

### 预定义的名字

```
内建常量: true false iota nil

内建类型: int int8 int16 int32 int64
          uint uint8 uint16 uint32 uint64 uintptr
          float32 float64 complex128 complex64
          bool byte rune string error

内建函数: make len cap new append copy close delete
          complex real imag
          panic recover
```

### golang运算符优先级

```
优先级     运算符
 7      ^ !
 6      * / % << >> & &^
 5      + - | ^
 4      == != < <= >= >
 3      <-
 2      &&
 1      ||
```

# 二、程序结构

## 变量

1. 返回函数中局部变量的地址也是安全的

2. new函数，表达式new(T)将创建一个T类型的匿名变量，初始化为T类型的零值，然后返回变量地址，返回的指针类型为`*T`

   ```Go
   p := new(int)   // p, *int 类型, 指向匿名的 int 变量
   fmt.Println(*p) // "0"
   *p = 2          // 设置 int 匿名变量的值为 2
   fmt.Println(*p) // "2"
   ```

3. 编译器会自动选择在栈上还是在堆上分配局部变量的存储空间，但可能令人惊讶的是，这个选择并不是由用`var`还是`new`声明变量的方式决定的。

4. 对于两个值是否可以用`==`或`!=`进行相等比较的能力也和可赋值能力有关系：对于任何类型的值的相等比较，第二个值必须是对第一个值类型对应的变量是可赋值的

## 类型

1. 对于每一个类型T，都有一个对应的类型转换操作T(x)，用于将x转为T类型（如果T是指针类型，可能会需要用小括弧包装T，比如`(*int)(0)`）

# 三、基础数据类型

1. 基础数据类型包括：整型，浮点数，复数，布尔型，字符串，常量

## 整型

1. golang提供了`int8`、`int16`、`int32`和`int64`四种截然不同大小的有符号整数类型，分别对应8、16、32、64bit大小的有符号整数，与此对应的是`uint8`、`uint16`、`uint32`和`uint64`四种无符号整数类型。还有两种一般对应特定CPU平台机器字大小的有符号和无符号整数`int`和`uint`，这两种类型都有同样的大小，32或64bit，不同的编译器即使在相同的硬件平台上可能产生不同的大小。
2. Unicode字符`rune`类型是和int32等价的类型，通常用于表示一个Unicode码点。`byte`也是`uint8`类型的等价类型，byte类型一般用于强调数值是一个原始的数据而不是一个小的整数。还有一种无符号的整数类型`uintptr`，没有指定具体的bit大小但是足以容纳指针。`uintptr`类型只有在底层编程时才需要。
3. 在Go语言中，%取模运算符的符号和被取模数的符号总是一致的，因此`-5%3`和`-5%-3`结果都是-2， 整数除法会向着0方向截断余数。

## 浮点数

1. Go语言提供了两种精度的浮点数，float32和float64。浮点数的范围极限值可以在math包找到。常量math.MaxFloat32表示float32能表示的最大数值，大约是 3.4e38。通常应该优先使用float64类型，因为float32类型的累计计算误差很容易扩散

2. math包中除了提供大量常用的数学函数外，还提供了IEEE754浮点数标准中定义的特殊值的创建和测试：正无穷大和负无穷大，分别用于表示太大溢出的数字和除零的结果；还有NaN非数，一般用于表示无效的除法操作结果0/0或Sqrt(-1)。NaN和任何数都是不相等的。而正负无穷是可以参与比较的。

   ```Go
   var z float64
   fmt.Println(z, -z, 1/z, -1/z, z/z) // "0 -0 +Inf -Inf NaN"
   ```

## 字符串

1. 单引号包裹起来的字符是int32类型，而对字符串的索引是byte类型，所以需要显式类型转换后对比

2. 文本字符串通常被解释为采用**UTF8编码**的**Unicode码点（rune）序列**

3. 内置的len函数可以返回一个字符串中的字节数目（不是rune字符数目），索引操作s[i]返回第i个字节的**字节值**， 第i个字节并不一定是字符串的第i个字符

4. 因为字符串是不可修改的，因此尝试修改字符串内部数据的操作也是被禁止的：

   ```Go
   s[0] = 'L' // compile error: cannot assign to s[0]
   ```

   不变性意味着如果两个字符串共享相同的底层数据的话也是安全的，这使得复制任何长度的字符串代价是低廉的。

   因为字符串不可变，所以一个字符串可以被多个线程共享，是线程安全的。

   不可用变使得string适合作为map的键。

5. 一个原生的字符串面值形式是`` `...` ``，使用反引号代替双引号。

6. 标准库中有四个包对字符串处理尤为重要：bytes、strings、strconv和unicode包

   - strings包提供了许多如字符串的查询、替换、比较、截断、拆分和合并等功能。
   - bytes包也提供了很多类似功能的函数
   - strconv包提供了布尔型、整型数、浮点数和对应字符串的相互转换，还提供了双引号转义相关的转换。
   - unicode包提供了IsDigit、IsLetter、IsUpper和IsLower等类似功能，它们用于给字符分类。

7. Go语言的range循环在处理字符串的时候，会自动隐式解码UTF8字符串，返回的是rune类型

### ASCII常见转义

```
\a      响铃
\b      退格
\f      换页
\n      换行
\r      回车
\t      制表符
\v      垂直制表符
\'      单引号（只用在 '\'' 形式的rune符号面值中）
\"      双引号（只用在 "..." 形式的字符串面值中）
\\      反斜杠
```

### UTF-8

1. UTF8是一个将Unicode码点编码为字节序列的变长编码。现在已经是Unicode的标准。UTF8编码使用1到4个字节来表示每个Unicode码点，ASCII部分字符只使用1个字节，常用字符部分使用2或3个字节表示。每个符号编码后第一个字节的高端bit位用于表示编码总共有多少个字节。如果第一个字节的高端bit为0，则表示对应7bit的ASCII字符，ASCII字符每个字符依然是一个字节，和传统的ASCII编码兼容。如果第一个字节的高端bit是110，则说明需要2个字节；后续的每个高端bit都以10开头。更大的Unicode码点也是采用类似的策略处理。

    ```
    0xxxxxxx                             runes 0-127    (ASCII)
    110xxxxx 10xxxxxx                    128-2047       (values <128 unused)
    1110xxxx 10xxxxxx 10xxxxxx           2048-65535     (values <2048 unused)
    11110xxx 10xxxxxx 10xxxxxx 10xxxxxx  65536-0x10ffff (other values unused)
    ```

2. Unicode转义也可以使用在rune字符中。下面三个字符是等价的，\u 后面是 4 个十六进制字符，\U 后面则是 8 个：

    ```
    '世' '\u4e16' '\U00004e16'
    ```

3. 对于小于256的码点值可以写在一个十六进制转义字节中，例如`\x41`对应字符'A'，但是对于更大的码点则必须使用`\u`或`\U`转义形式。因此，`\xe4\xb8\x96`并不是一个合法的rune字符，虽然这三个字节对应一个有效的UTF8编码的码点。

4. 每一个UTF8字符解码，不管是显式地调用utf8.DecodeRuneInString解码或是在range循环中隐式地解码，如果遇到一个错误的UTF8编码输入，将生成一个特别的Unicode字符`\uFFFD`，在印刷中这个符号通常是一个黑色六角或钻石形状，里面包含一个白色的问号"?"。

5. 将一个整数转型为字符串意思是生成以只包含对应Unicode码点字符的UTF8字符串：

    ```Go
    fmt.Println(string(65))     // "A", not "65"
    fmt.Println(string(0x4eac)) // "京"
    ```


#### UTF-8的特点

1. 完全兼容ASCII码，并且可以自动同步：它可以通过向前回朔最多3个字节就能确定当前字符编码的开始字节的位置。
2. 它也是一个前缀编码，所以当从左向右解码时不会有任何歧义也并不需要向前查看
3. 没有任何字符的编码是其它字符编码的子串，或是其它编码序列的字串，因此搜索一个字符时只要搜索它的字节编码序列即可，不用担心前后的上下文会对搜索结果产生干扰。
4. 。同时UTF8编码的顺序和Unicode码点的顺序一致，因此可以直接排序UTF8编码序列。

## 常量

1. 如果是批量声明的常量，除了第一个外其它的常量右边的初始化表达式都可以省略，如果省略初始化表达式则表示使用前面常量的初始化表达式写法，对应的常量类型也一样的。例如：

   ```go
   const (
       a = 1
       b
       c = 2
       d
   )
   
   fmt.Println(a, b, c, d) // "1 1 2 2"
   ```

2.  常量声明可以使用iota常量生成器初始化，它用于生成一组以相似规则初始化的常量，但是不用每行都写一遍初始化表达式。在一个const声明语句中，在第一个声明的常量所在的行，iota将会被置为0，然后在每一个有常量声明的行加一。

   ```Go
   type Weekday int
   
   const (
       Sunday Weekday = iota
       Monday
       Tuesday
       Wednesday
       Thursday
       Friday
       Saturday
   )
   ```

3. Go语言的常量有个不同寻常之处。虽然一个常量可以有任意一个确定的基础类型，例如int或float64，或者是类似time.Duration这样命名的基础类型，但是许多常量并没有一个明确的基础类型。这里有六种未明确类型的常量类型，分别是无类型的布尔型、无类型的整数、无类型的字符、无类型的浮点数、无类型的复数、无类型的字符串。

# 四、复合数据类型

## 数组

1. 在数组字面值中，如果在数组的长度位置出现的是“...”省略号，则表示数组的长度是根据初始化值的个数来计算。

   ```go
   q := [...]int{1, 2, 3}
   ```

2. 数组可以指定一个**索引和对应值列表**的方式初始化

   ```Go
   type Currency int
   
   const (
       USD Currency = iota // 美元
       EUR                 // 欧元
       GBP                 // 英镑
       RMB                 // 人民币
   )
   
   symbol := [...]string{USD: "$", EUR: "€", GBP: "￡", RMB: "￥"}
   
   fmt.Println(RMB, symbol[RMB]) // "3 ￥"
   ```

3. 在这种形式的数组字面值形式中，初始化索引的顺序是无关紧要的，而且没用到的索引可以省略，和前面提到的规则一样，未指定初始值的元素将用零值初始化。例如下面定义了一个含有100个元素的数组r，最后一个元素被初始化为-1，其它元素都是用0初始化。

   ```Go
   r := [...]int{99: -1}
   ```

4. 如果一个数组的元素类型是可以相互比较的，那么数组类型也是可以相互比较的

## 切片Slice

1. slice是一个轻量级的数据结构，一个slice由三个部分构成：指针、长度和容量。指针指向第一个slice元素对应的底层数组元素的地址，要注意的是slice的第一个元素并不一定就是数组的第一个元素。长度对应slice中元素的数目；长度不能超过容量，容量一般是从slice的开始位置到底层数据的结尾位置。内置的len和cap函数分别返回slice的长度和容量。多个slice之间可以共享底层的数据，并且引用的数组部分区间可能重叠。实际上是一个类似下面结构体的聚合类型：

   ```Go
   type IntSlice struct {
       ptr      *int
       len, cap int
   }
   ```

2. 和数组不同的是，slice之间不能比较，因此我们不能使用==操作符来判断两个slice是否含有全部相等元素。

3. 一个零值的slice等于nil， 可以用[]int(nil)类型转换表达式来生成一个对应类型slice的nil值。

   ```Go
   var s []int    // len(s) == 0, s == nil
   s = nil        // len(s) == 0, s == nil
   s = []int(nil) // len(s) == 0, s == nil
   s = []int{}    // len(s) == 0, s != nil
   ```

4. 内置的append函数则可以追加多个元素，甚至追加一个slice。

   ```Go
   var x []int
   x = append(x, 1)
   x = append(x, 2, 3)
   x = append(x, 4, 5, 6)
   x = append(x, x...) // append the slice x
   fmt.Println(x)      // "[1 2 3 4 5 6 1 2 3 4 5 6]"
   ```

5. 一个slice可以用来模拟一个stack。最初给定的空slice对应一个空的stack，然后可以使用append函数将新的值压入stack：

   ```Go
   stack = append(stack, v) // push v
   ```

   stack的顶部位置对应slice的最后一个元素：

   ```Go
   top := stack[len(stack)-1] // top of stack
   ```

   通过收缩stack可以弹出栈顶的元素

   ```Go
   stack = stack[:len(stack)-1] // pop
   ```

6. 对slice取索引时，新的子切片len为取的元素数目，cap为取的第一个元素到始祖slice的末尾元素的总个数合。进行append操作的时候也会在始祖slice上append修改。

   ```go
   a := []int {1,2,3,4}
   b := a[0:2]
   c := b[0:1]
   fmt.Println(len(b), cap(b)) // 2 4
   fmt.Println(len(c), cap(c)) // 1 4
   _ = append(b, 666)
   fmt.Println(a)              // [1 2 666 4]
   fmt.Println(b)              // [1 2]
   ```

7. golang slice保持cap不变的情况下将len设置为0

   ```go
   a := []int {1,2,3,4}
   a = a[0:0]
   fmt.Println(len(a), cap(a)) // 0 4
   ```

## Map

1. 在Go语言中，一个map就是一个哈希表的引用

2. 使用内置的delete函数可以删除元素

3. map中的元素并不是一个变量，因此我们不能对map的元素进行取址操作。禁止对map元素取址的原因是map可能随着元素数量的增长而重新分配更大的内存空间，从而可能导致之前的地址无效。

4. 对map也可以使用`len`函数

5. 要想遍历map中全部的key/value对的话，可以使用range风格的for循环实现，Map的迭代顺序是不确定的

   ```Go
   for name, age := range ages {
       fmt.Printf("%s\t%d\n", name, age)
   }
   ```

6. 如果要按顺序遍历key/value对，我们必须显式地对key进行排序，可以使用sort包的Strings函数对字符串slice进行排序。下面是常见的处理方式：

   ```Go
   import "sort"
   
   var names []string
   for name := range ages {
       names = append(names, name)
   }
   sort.Strings(names)
   for _, name := range names {
       fmt.Printf("%s\t%d\n", name, ages[name])
   }
   ```

6. map类型的零值是nil，也就是没有引用任何哈希表。向一个nil值的map存入元素将导致一个panic异常。

   ```Go
   var ages map[string]int
   fmt.Println(ages == nil)    // "true"
   fmt.Println(len(ages) == 0) // "true"
   
   names := map[string]string{} 
   fmt.Println(names == nil) // "false"
   ```

7. 可以通过ok确定元素是否存在

   ```Go
   age, ok := ages["bob"]
   ```

8. Go语言中并没有提供一个set类型，但是map中的key也是不相同的，可以用map实现类似set的功能。

   ```Go
    seen := make(map[string]bool) // a set of strings
   ```

9. slice不能做key但是可以通过函数将slice转化为string后做key，这种技术同样可以用于其他的类型

   ```Go
   func k(list []string) string { return fmt.Sprintf("%q", list) }
   ```

## 结构体

1. 指向结构体的指针也可以通过点操作符访问结构体成员

2. 匿名结构体

   ```go
   a := struct {
   		name string
   		age int
   	}{
   		"jack",
   		2,
   	}
   ```

3. 结构体字面值有两种写法，一种是不指定成员名，一种是指定的，前者一般在较小的结构体中使用，更常用的是后者的写法，两种写法不能混用

4. 不能企图在外部包中初始化结构体中未导出的成员。

   ```Go
   package p
   type T struct{ a, b int } // a and b are not exported
   
   package q
   import "p"
   var _ = p.T{a: 1, b: 2} // compile error: can't reference a, b
   var _ = p.T{1, 2}       // compile error: can't reference a, b
   ```

5. 如果结构体的全部成员都是可以比较的，那么结构体也是可以比较的，那样的话两个结构体将可以使用==或!=运算符进行比较。可比较的结构体类型和其他可比较的类型一样，可以用于map的key类型。

6. 结构体支持结构体嵌入

   ```Go
   type Point struct {
       X, Y int
   }
   
   type Circle struct {
       Center Point
       Radius int
   }
   
   type Wheel struct {
       Circle Circle
       Spokes int
   }
   ```

7. Go语言有一个特性让我们只声明一个成员对应的数据类型而不指名成员的名字；这类成员就叫匿名成员。匿名成员的数据类型必须是命名的类型或指向一个命名的类型的指针。得益于匿名嵌入的特性，我们可以直接访问叶子属性而不需要给出完整的路径：

   ```Go
   type Circle struct {
       Point
       Radius int
   }
   
   type Wheel struct {
       Circle
       Spokes int
   }
   
   var w Wheel
   w.X = 8            // equivalent to w.Circle.Point.X = 8
   w.Y = 8            // equivalent to w.Circle.Point.Y = 8
   w.Radius = 5       // equivalent to w.Circle.Radius = 5
   w.Spokes = 20
   ```

8. 不幸的是，结构体字面值并没有简短表示匿名成员的语法， 因此下面的语句都不能编译通过：

   ```Go
   w = Wheel{8, 8, 5, 20}                       // compile error: unknown fields
   w = Wheel{X: 8, Y: 8, Radius: 5, Spokes: 20} // compile error: unknown fields
   ```

9. 结构体字面值必须遵循形状类型声明时的结构

   ```Go
   w = Wheel{Circle{Point{8, 8}, 5}, 20}
   
   w = Wheel{
       Circle: Circle{
           Point:  Point{X: 8, Y: 8},
           Radius: 5,
       },
       Spokes: 20, // NOTE: trailing comma necessary here (and at Radius)
   }
   ```

10. **外层的结构体不仅仅是获得了匿名成员类型的所有成员，而且也获得了该类型导出的全部的方法**。

11. 可以通过**接收器+匿名成员类型**的方法访问成员：

    ```go
    type hp struct{ sort.IntSlice }
    func (h hp) Less(i, j int) bool  { return h.IntSlice[i] > h.IntSlice[j] }
    func (h *hp) Push(v interface{}) { h.IntSlice = append(h.IntSlice, v.(int)) }
    func (h *hp) Pop() interface{}   { a := h.IntSlice; v := a[len(a)-1]; h.IntSlice = a[:len(a)-1]; return v }
    func (h *hp) push(v int)         { heap.Push(h, v) }
    func (h *hp) pop() int           { return heap.Pop(h).(int) }
    ```

## Json

1. 基础JSON类型：**数字**（十进制或科学记数法）、**布尔值**（true或false）、**字符串**，其中字符串是以双引号包含的Unicode字符序列。这些基础类型可以通过JSON的**数组**和**对象**类型进行递归组合。一个JSON数组可以用于编码Go语言的数组和slice。一个JSON对象是一个字符串到值的映射，写成一系列的name:value对形式，用花括号包含并以逗号分隔。JSON的对象类型可以用于编码Go语言的map类型（key类型是字符串）和结构体。

2. 将golang变量转换为JSON的过程叫编组（marshaling）。编组通过调用json.Marshal函数完成：

   ```Go
   data, err := json.Marshal(movies)
   ```

3. 为了生成便于阅读的格式，另一个json.MarshalIndent函数将产生整齐缩进的输出。该函数有两个额外的字符串参数用于表示每一行输出的前缀和每一个层级的缩进：

   ```Go
   type Movie struct {
       Title  string
       Year   int  `json:"released"`
       Color  bool `json:"color,omitempty"`
       Actors []string
   }
   
   var movies = []Movie{
       {Title: "Casablanca", Year: 1942, Color: false,
           Actors: []string{"Humphrey Bogart", "Ingrid Bergman"}},
       {Title: "Cool Hand Luke", Year: 1967, Color: true,
           Actors: []string{"Paul Newman"}},
       {Title: "Bullitt", Year: 1968, Color: true,
           Actors: []string{"Steve McQueen", "Jacqueline Bisset"}},
       // ...
   }
   
   data, err := json.MarshalIndent(movies, "", "    ")
   if err != nil {
       log.Fatalf("JSON marshaling failed: %s", err)
   }
   fmt.Printf("%s\n", data)
   
   /*
   [
       {
           "Title": "Casablanca",
           "released": 1942,
           "Actors": [
               "Humphrey Bogart",
               "Ingrid Bergman"
           ]
       },
       {
           "Title": "Cool Hand Luke",
           "released": 1967,
           "color": true,
           "Actors": [
               "Paul Newman"
           ]
       },
       {
           "Title": "Bullitt",
           "released": 1968,
           "color": true,
           "Actors": [
               "Steve McQueen",
               "Jacqueline Bisset"
           ]
       }
   ]
   */
   ```

4. 在编码时，默认使用Go语言结构体的成员名字作为JSON的对象（通过reflect反射技术）。**只有导出的结构体成员才会被编码**，这也就是我们为什么选择用大写字母开头的成员名称。

5. 编码的逆操作是解码，对应将JSON数据解码为Go语言的数据结构，Go语言中一般叫`unmarshaling`，通过`json.Unmarshal`函数完成。下面的代码将JSON格式的电影数据解码为一个结构体slice，结构体中只有Title成员。通过定义合适的Go语言数据结构，我们可以选择性地解码JSON中感兴趣的成员。Unmarshal函数调用返回，slice将被只含有Title信息的值填充，**其它JSON成员将被忽略**。

   ```Go
   var titles []struct{ Title string }
   if err := json.Unmarshal(data, &titles); err != nil {
       log.Fatalf("JSON unmarshaling failed: %s", err)
   }
   fmt.Println(titles) // "[{Casablanca} {Cool Hand Luke} {Bullitt}]"
   ```

# 五、函数

1. 在Go语言中，所有的函数参数都是**值拷贝传入**的

# 八、Goroutines和Channels

- 在Go语言中，每一个并发的执行单元叫作一个goroutine
- 当一个程序启动时，其主函数即在一个单独的goroutine中运行，我们叫它**main goroutine**。新的goroutine会用`go`语句来创建。
- 默认情况下，最新的go版本协程可以利用多核CPU，但是通过runtime.GOMAXPROCS() 我们可以设置所需的核心数（其实并不是CPU核心数）

## Channel

**chan相关操作**

``` Go
ch := make(chan int)   // 创建chan，缓存为0
ch = make(chan int, 3) // 建立带缓存的chan
ch <- x                // 向chan发送元素
x, ok = <- ch          // 从chan接收元素, ture表示成功从channels接收到值，false表示channels已经被关闭并且里面没有值可接收。
cap(ch)                // 获取chan容量
len(cap)               // 获取内部缓存队列中有效元素的个数
close(ch)              // 关闭chan

```

- 两个相同类型的channel**可以使用==运算符比较**。如果两个channel引用的是相同的对象，那么比较的结果为真。一个channel也可以和nil进行比较，channel零值为nil。
- 对关闭channel的任何发送操作都会引发异常，对一个已经被close过的channel进行接收操作依然可以接受到之前已经成功发送的数据；如果channel中已经没有数据的话将产生一个零值的数据。
- 一个基于无缓存Channels的发送操作将导致发送者goroutine阻塞，直到另一个goroutine在相同的Channels上执行接收操作，反之也是。基于无缓存Channels的发送和接收操作将导致两个goroutine做一次同步操作。因为这个原因，**无缓存Channels有时候也被称为同步Channels**。
- 试图重复关闭一个channel将导致panic异常，试图关闭一个nil值的channel也将导致panic异常。
- 可以通过关闭一个channel来进行广播
- 一个goroutine持续向一个channel发送值，而已经没有goroutine从中接收值的情况称为goroutine泄漏

channel可以通过range迭代，当channel关闭时，停止迭代

```go
for x := range channel1 {
    squares <- x * x
}
close(channel2)
```

Go语言的类型系统提供了单方向的channel类型，分别用于只发送或只接收的channel。类型`chan<- int`表示一个只发送int的channel，只能发送不能接收。相反，类型`<-chan int`表示一个只接收int的channel，只能接收不能发送。因为关闭操作只用于断言不再向channel发送新的数据，所以只有在发送者所在的goroutine才会调用close函数，因此对一个只接收的channel调用close将是一个编译错误。

```Go
// 调用counter（naturals）时，naturals的类型将隐式地从chan int转换成chan<- int。
func counter(out chan<- int) {
    for x := 0; x < 100; x++ {
        out <- x
    }
    close(out)
}

func squarer(out chan<- int, in <-chan int) {
    for v := range in {
        out <- v * v
    }
    close(out)
}

func printer(in <-chan int) {
    for v := range in {
        fmt.Println(v)
    }
}

func main() {
    naturals := make(chan int)
    squares := make(chan int)
    go counter(naturals)
    go squarer(squares, naturals)
    printer(squares)
}
```

## select

```go
select {
case <-ch1:
    // ...
case x := <-ch2:
    // ...use x...
case ch3 <- y:
    // ...
default:
    // ...
}
```

上面是select语句的一般形式。和switch语句稍微有点相似，也会有几个case和最后的default选择分支。每一个case代表一个通信操作（在某个channel上进行发送或者接收）。

select会等待case中有能够执行的case时去执行。当条件满足时，select才会去通信并执行case之后的语句；这时候其它通信是不会执行的。一个没有任何case的select语句写作select{}，会永远地等待下去。

**如果多个case同时就绪时，select会随机地选择一个执行**，这样来保证每一个channel都有平等的被select的机会。go 不会删除重复的channel，所以可以使用多次添加case来影响结果。

对一个nil的channel发送和接收操作会永远阻塞，在select语句中操作nil的channel永远都不会被select到

# 九、基于共享变量的开发

## sync.Mutex互斥锁

```go
mu := sync.Mutex
mu.Lock()
mu.Unlock()
```

## sync.RWMutex读写锁

- 写操作使用 `sync.RWMutex.Lock`和 `sync.RWMutex.Unlock` 方法；
- 读操作使用 `sync.RWMutex.RLock`和`sync.RWMutex.RUnlock` 方法；

**结构**

```go
type RWMutex struct {
	w           Mutex
	writerSem   uint32 // 写信号量
	readerSem   uint32 // 读信号量
	readerCount int32  // 现阶段希望进行读操作的总和
	readerWait  int32  // 写操作需要等待的读操作的数目
}
```

**实例**

```go
var mu sync.RWMutex
var balance int
func Balance() int {
    mu.RLock() // readers lock
    defer mu.RUnlock()
    return balance
}
```

RWMutex的设计可以避免连续的写或读。当读时，后来的读写先来先服务，当写时，后来的读写是优先服务写执行完毕前来的一批读，再服务写。

## sync.Once惰性初始化

```go
var loadIconsOnce sync.Once
var icons map[string]image.Image
// Concurrency-safe.
func Icon(name string) image.Image {
    loadIconsOnce.Do(loadIcons)
    return icons[name]
}
```

每一次对Do(loadIcons)的调用都会锁定mutex，并会检查boolean变量（译注：Go1.9中会先判断boolean变量是否为1(true)，只有不为1才锁定mutex，不再需要每次都锁定mutex）。在第一次调用时，boolean变量的值是false，Do会调用loadIcons并会将boolean变量设置为true。随后的调用什么都不会做。

## sync.WaitGroup

WaitGroup用于一个协程等待另一个协程

```go
package main

import (
	"sync"
)

var wg sync.WaitGroup
// 或写成 var wg = sync.WaitGroup{}

func main() {
	wg.Add(1)
	go say("Hello World")
	wg.Wait()
}

func say(s string) {
	println(s)
	wg.Done()
}
```

# 附录一、Golang常用函数及数据结构

## fmt——格式化IO

> 参考资料：https://books.studygolang.com/The-Golang-Standard-Library-by-Example/chapter01/01.3.html

### 输出

对于`Sprint/Fprint/Print`，当两个参数都不是字符串时，会自动添加一个空格，否则不会加。这起到了连接字符串的作用。

#### 输出转换（占位符/动词verb）

```
%d          十进制整数
%x, %o, %b  十六进制，八进制，二进制整数。
%f, %g, %e  浮点数： 3.141593 3.141592653589793(根据情况用%e或%f) 3.141593e+00
%t          布尔：true或false
%c          字符（rune） (Unicode码点)
%s          字符串
%q          带双引号的字符串"abc"或带单引号的字符'c'
%v          变量的自然形式（natural format）
%+v         同上，但是输出结构体的时候会添加字段名
%T          变量的类型
%%          字面上的百分号标志（无操作数）
```

对于小数，指定宽度是输入宽度至少需要达到指定值，指定精度是将输入数据转化为对应的精度值输出。（精度不够补0，精度多了截断）

对数值而言，宽度为该数值占用区域的最小宽度；精度为小数点之后的位数。 但对于 %g/%G 而言，精度为所有数字的总数。%e 和 %f 的默认精度为6。

对大多数的值而言，宽度为输出的最小字符数，如果必要的话会为已格式化的形式填充空格。对字符串而言，精度为输出的最大字符数，如果必要的话会直接截断。

#### 输出标记

```
占位符                        说明                                                                   
+        总打印数值的正负号；对于%q（%+q）保证只输出ASCII编码的字符。   

-        在右侧而非左侧填充空格（左对齐该区域）。打印默认是右对齐的。

#        备用格式：为八进制添加前导 0（%#o），为十六进制添加前导 0x（%#x）或 0X（%#X），为 %p（%#p）去掉前导 0x；如果可能的话，%q（%#q）会打印          原始（即反引号围绕的）字符串；如果是可打印字符，%U（%#U）会写出该字符的Unicode 编码形式（如字符 x 会被打印成 U+0078 'x'）。

' '     （空格）为数值中省略的正负号留出空白（% d）；以十六进制（% x, % X）打印字符串或切片时，在字节之间用空格隔开

0        填充整数前导的0而非空格；对于数字，这会将填充移到正负号之后
```

**实现了Stringer接口的类型，在`Print`类函数中会以对应格式打印**

```go
type Stringer interface {
    String() string
}
```

**实现了GoStringer接口的类型会用于打印格式化占位符为 %#v 的值**

```go
type GoStringer interface {
    GoString() string
}
```

**实现了Formatter 接口的类型会解析对应的占位符，做到自定义格式化输出**

```go
type Formatter interface {
    Format(f State, c rune)
}
```

### 输入

 Scan、Scanf 和 Scanln 从 os.Stdin 中读取； Fscan、Fscanf 和 Fscanln 从指定的 io.Reader 中读取； Sscan、Sscanf 和 Sscanln 从实参字符串中读取。

**对于输入的占位符有所不同：**

```
%p 没有实现
%T 没有实现
%e %E %f %F %g %G 都完全等价，且可扫描任何浮点数或复数数值
%s 和 %v 在扫描字符串时会将其中的空格作为分隔符
标记 # 和 + 没有实现
在使用 %v 占位符扫描整数时，可接受友好的进制前缀0（八进制）和0x（十六进制）
```

- 宽度被解释为输入的文本（%5s 意为最多从输入中读取5个 rune 来扫描成字符串），而扫描函数则**没有精度**的语法（没有 %5.2f，只有 %5f）。
- 当以某种格式进行扫描时，无论在格式中还是在输入中，所有非空的连续空白字符 （除换行符外）都等价于单个空格。

**扫描函数遇到空白字符都会停止扫描**

`Scan/FScan/Sscan` 这组函数将连续由空白字符分隔的值存储为连续的实参

`Scanf/FScanf/Sscanf` 这组函数将连续由空白字符分隔的值存储为连续的实参， 其格式由 `format` 决定，不是对应格式的输入无法录入

`Scanln/FScanln/Sscanln`这组函数将连续由空白字符的值存储为连续的实参，输入最后必须有个回车，遇到回车直接结束扫描，无论参数中的变量是否扫描完毕。

```go
func Scan(a ...interface{}) (n int, err error)
func Scanf（format string, a ...interface{}）(n int, err error)
func Scanln(a ...interface{}) (n int, err error)
```

## Buffer高效拼接字符串以及自定义线程安全Buffer

使用`bytes.Buffer`数据结构，和`buffer.String()`函数可以实现高效率的字符串增长。

**创建buffer的四种方式**

```go
var b bytes.Buffer         //直接定义一个 Buffer 变量，而不用初始化
b.Write([]byte("Hello "))  // 可以直接使用
b.Write([]byte("world"))

b1 := new(bytes.Buffer)    //直接使用 new 初始化，可以直接使用
// 其它两种定义方式
func NewBuffer(buf []byte) *Buffer
func NewBufferString(s string) *Buffer
```

但是Buffer是线程不安全的，需要自己加锁才可以实现线程安全。

```go
type Buffer struct {
    b bytes.Buffer
    rw sync.RWMutex // 读写锁
}
func (b *Buffer) Read(p []byte) (n int, err error) {
    b.rw.RLock()
    defer b.rw.RUnlock()
    return b.b.Read(p)
}
func (b *Buffer) Write(p []byte) (n int, err error) {
    b.rw.Lock()
    defer b.rw.Unlock()
    return b.b.Write(p)
}
```

## 二分查找lower_bound和upper_bound实现

```go
//二分查找第一个大于或等于target的数字，length为查找段长度，查找不到则返回left+length
func lower_bound(nums []int, left, length,target int) int {
    for length > 0 {
        half := length >> 1
        mid := left + half
        if nums[mid] >= target {
            length = half
        }else {
            left = mid
            left++
            length -= half + 1
        }
    }
    return left
}


//二分查找第一个大于target的数字，length为查找段长度，查找不到则返回left+length
func upper_bound(nums []int, left, length,target int) int {
    for length > 0 {
        half := length >> 1
        mid := left + half
        if nums[mid] > target {
            length = half
        }else {
            left = mid
            left++
            length -= half + 1
        }
    }
    return left
}
```

**sort包内部的二分查找Search**

该方法会使用“二分查找”算法来找出能使 f(x)(0<=x<n) 返回 ture 的最小值的下标 i。 前提条件 : f(x)(0<=x<i) 均返回 false, f(x)(i<=x<n) 均返回 ture。 如果不存在 i 可以使 f(i) 返回 ture, 则返回 n。

```go
func Search(n int, f func(int) bool) int
```

**例子**

```go
x := 11
s := []int{3, 6, 8, 11, 45} // 注意已经升序排序
pos := sort.Search(len(s), func(i int) bool { return s[i] >= x })
```





## strconv包下string和各种类型的互相转换

> 参考资料：http://c.biancheng.net/view/5112.html

**string与int的互相转换**

```go
func Itoa(i int) string                //int转string
func Atoi(s string) (i int, err error) //string转int
```

**Parse系列函数**

Parse 系列函数用于将字符串转换为指定类型的值，其中包括 ParseBool()、ParseFloat()、ParseInt()、ParseUint()。

```go
func ParseBool(str string) (value bool, err error)
```

```go
func ParseInt(s string, base int, bitSize int) (i int64, err error)
```

- base 指定进制，取值范围是 2 到 36。如果 base 为 0，则会从字符串前置判断，“0x”是 16 进制，“0”是 8 进制，否则是 10 进制。
- bitSize 指定结果必须能无溢出赋值的整数类型，0、8、16、32、64 分别代表 int、int8、int16、int32、int64。
- 返回的 err 是 *NumErr 类型的，如果语法有误，err.Error = ErrSyntax，如果结果超出类型范围 err.Error = ErrRange。

```go
func ParseUint(s string, base int, bitSize int) (n uint64, err error) //无符号整数类型
```

```go
func ParseFloat(s string, bitSize int) (f float64, err error)
```

**Format系列函数**

Format 系列函数实现了将给定类型数据格式化为字符串类型的功能，其中包括 FormatBool()、FormatInt()、FormatUint()、FormatFloat()。

```go
func FormatBool(b bool) string
```

```go
func FormatInt(i int64, base int) string
```

```go
func FormatUint(i uint64, base int) string
```

```go
func FormatFloat(f float64, fmt byte, prec, bitSize int) string
```

- bitSize 表示参数 f 的来源类型（32 表示 float32、64 表示 float64），会据此进行舍入。
- fmt 表示格式，可以设置为“f”表示 -ddd.dddd、“b”表示 -ddddp±ddd，指数为二进制、“e”表示 -d.dddde±dd 十进制指数、“E”表示 -d.ddddE±dd 十进制指数、“g”表示指数很大时用“e”格式，否则“f”格式、“G”表示指数很大时用“E”格式，否则“f”格式。
- prec 控制精度（排除指数部分）：当参数 fmt 为“f”、“e”、“E”时，它表示小数点后的数字个数；当参数 fmt 为“g”、“G”时，它控制总的数字个数。如果 prec 为 -1，则代表使用最少数量的、但又必需的数字来表示 f。

**Append系列函数**

Append 系列函数和 Format 系列函数的使用方法类似，只不过是将转换后的结果追加到一个切片中。

```go
package main
import (
    "fmt"
    "strconv"
)
func main() {
    // 声明一个slice
    b10 := []byte("int (base 10):")
  
    // 将转换为10进制的string，追加到slice中
    b10 = strconv.AppendInt(b10, -42, 10)
    fmt.Println(string(b10))
    b16 := []byte("int (base 16):")
    b16 = strconv.AppendInt(b16, -42, 16)
    fmt.Println(string(b16))
}
```

## unicode包判断字符类型

```go
func IsSpace(r rune) bool // 是否空格
func IsDigit(r rune) bool  // 是否阿拉伯数字字符，即 0-9
func IsLetter(r rune) bool // 是否字母
func IsLower(r rune) bool // 是否小写字符
func IsUpper(r rune) bool // 是否大写字符
```

## golang二路归并排序实现

```go
func merge(nums []int, l1, r1, l2, r2 int) {
	tem := make([]int, r2-l1+1)
	i, j, index := l1, l2, 0
	for i <= r1 && j <= r2 {
		if nums[i] <= nums[j] {
			tem[index] = nums[i]
			i++
			index++
		} else {
			tem[index] = nums[j]
			j++
			index++
		}
	}

	for i <= r1 {
		tem[index] = nums[i]
		i++
		index++
	}

	for j <= r2 {
		tem[index] = nums[j]
		j++
		index++
	}

	index = 0
	for ; index < len(tem); index++ {
		nums[l1+index] = tem[index]
	}
}

func mergeSort(nums []int) {
	for step := 2; step <= len(nums); step <<= 1 {
		for i := 0; i < len(nums); i += step {
			mid := i + (step >> 1)
			if mid > len(nums)-1 {
				break
			}
			merge(nums, i, mid-1, mid, i+step-1)
		}
	}
}
```

**golang随机数生成**

```go
import (
    "fmt"
    "math/rand"
    "time"
)

func main() {
    rand.Seed(time.Now().Unix())
    fmt.Println(rand.Intn(100))   // [0,100)的随机值，返回值为int
    fmt.Println(rand.Int31n(100)) // [0,100)的随机值，返回值为int32
    fmt.Println(rand.Float32())   // [0, 1)范围内32位float随机值，返回值为float32
    fmt.Println(rand.Float64())   // [0, 1)范围内64位float随机值，返回值为float64
    
    //使用rand对象
    r := rand.New(rand.NewSource(time.Now().Unix()))
    fmt.Println(r.Intn(100))   // [0,100)的随机值，返回值为int
}
```

## golang排序

该包实现了四种基本排序算法：插入排序、归并排序、堆排序和快速排序。 但是这四种排序方法是不公开的，它们只被用于 sort 包内部使用。sort 包会根据实际数据自动选择高效的排序算法。

**排序接口定义**

```go
import "sort"
type Interface interface {
        // 获取数据集合元素个数
        Len() int
        // 如果 i 索引的数据小于 j 索引的数据，返回 true，且不会调用下面的 Swap()，即数据升序排序。
        Less(i, j int) bool
        // 交换 i 和 j 索引的两个元素的位置
        Swap(i, j int)
}
```

数据集合实现了这三个方法后，即可调用该包的 Sort() 方法进行排序。 Sort() 方法定义如下：

```go
func Sort(data Interface)
```

该包还提供了一个方法可以判断数据集合是否已经排好顺序O(n):

```go
func IsSorted(data Interface) bool {
    n := data.Len()
    for i := n - 1; i > 0; i-- {
        if data.Less(i, i-1) {
            return false
        }
    }
    return true
}
```

*sort*包提供了 `Reverse()` 方法，可以允许将数据按 Less() 定义的排序方式逆序排序，而不必修改 Less() 代码。

```go
func Reverse(data Interface) Interface
sort.Sort(sort.Reverse(stus))
```

*sort*包定义了一个 IntSlice 类型，并且实现了 sort.Interface 接口：

> 同样的也有Float64Slice和StringSlice类型

```go
type IntSlice []int
func (p IntSlice) Len() int           { return len(p) }
func (p IntSlice) Less(i, j int) bool { return p[i] < p[j] }
func (p IntSlice) Swap(i, j int)      { p[i], p[j] = p[j], p[i] }
//IntSlice 类型定义了 Sort() 方法，包装了 sort.Sort() 函数
func (p IntSlice) Sort() { Sort(p) }
//IntSlice 类型定义了 SearchInts() 方法，包装了 SearchInts() 函数
func (p IntSlice) Search(x int) int { return SearchInts(p, x) }
```

并且提供的 sort.Ints() 方法使用了该 IntSlice 类型：

```go
func Ints(a []int) { Sort(IntSlice(a)) }
```

所以，对[]int 切片排序更常使用 sort.Ints()，而不是直接使用 IntSlice 类型

```go
s := []int{5, 2, 6, 3, 1, 4} // 未排序的切片数据
sort.Ints(s)
```

如果要使用降序排序，显然要用前面提到的 Reverse() 方法：

```go
sort.Sort(sort.Reverse(sort.IntSlice(s)))
```

如果要查找整数 x 在切片 a 中的位置，相对于前面提到的 Search() 方法，*sort*包提供了 SearchInts()，使用条件为：**切片 a 已经升序排序** （返回第一个>=x的数的下标）:

```go
func SearchInts(a []int, x int) int
```

### sort 包提供的对[]int 切片、[]float64 切片和[]string 切片完整支持

```go
a := []int{1,2,3}
sort.Ints(a)
sort.IntsAreSorted(a)
sort.SearchInts(a, 3)

b := []float64{1.2, 1.4, 1.6}
sort.Float64s(b)
sort.Float64sAreSorted(b)
sort.SearchFloat64s(b , 1.5)

c := []string{"alice", "bob", "candy"}
sort.Strings(c)
sort.StringsAreSorted(c)
sort.SearchStrings(c, "amy")
```

### 为结构体切片设计的通用排序

```go
func Slice(slice interface{}, less func(i, j int) bool) 
func SliceStable(slice interface{}, less func(i, j int) bool) //完成稳定排序
func SliceIsSorted(slice interface{}, less func(i, j int) bool) bool 
func Search(n int, f func(int) bool) int
```

## golang堆

**堆相关接口定义**

```go
import "container/heap"
type Interface interface {
    sort.Interface
    Push(x interface{}) // add x as element Len()
    Pop() interface{}   // remove and return element Len() - 1.
}
```

**golang小顶堆实现**

```go
type IntHeap struct {
   sort.IntSlice
}

func (h *IntHeap) Push(v interface{}) {
   h.IntSlice = append(h.IntSlice, v.(int))
}

func (h *IntHeap)Pop()interface{} {
   num := h.IntSlice[len(h.IntSlice) - 1]
   h.IntSlice = h.IntSlice[:len(h.IntSlice) - 1]
   return num
}

func (h *IntHeap) push(v int) {
   heap.Push(h, v)
}

func (h *IntHeap)pop()int {
   return heap.Pop(h).(int)
}

//需要实现大顶堆只需要修改Less方法
//func (h *IntHeap) Less(i, j int) bool {
//	return h.IntSlice[i] > h.IntSlice[j]
//}
```

## copy函数

`func copy(dst, src []Type) int`

用于将源slice的数据（第二个参数），复制到目标slice（第一个参数）。返回值为拷贝了的数据个数，是len(dst)和len(src)中的最小值。

# 附录二golang机制底层原理

Go 语言中最常见的、也是经常被人提及的设计模式就是：不要通过共享内存的方式进行通信，而是应该通过通信的方式共享内存。

## Golang协程

### Golang 线程和协程的区别

- 对于 **进程、线程（内核支持线程）**，都是有内核进行调度，有 CPU 时间片的概念，进行 **抢占式调度**（有多种调度算法）
- 对于 **协程(用户级线程)**，这是对内核透明的，也就是系统并不知道有协程的存在，是完全由用户自己的程序进行调度的，因为是由用户程序自己控制，那么就很难像抢占式调度那样做到强制的 CPU 控制权切换到其他进程/线程，通常只能进行 **协作式调度**，需要协程自己主动把控制权转让出去之后，其他协程才能被执行到。缺点是无法进行公平调度。

**本质上，goroutine 就是协程。** 不同的是，Golang 在 runtime、系统调用等多方面对 goroutine 调度进行了封装和处理。

1. 内存消耗方面

　　每个 goroutine (协程) 默认占用内存远比 Java 、C 的线程少。

　　*goroutine：*2KB 

　　线程：8MB

　2. 线程和 goroutine 切换调度开销方面

　　线程/goroutine 切换开销方面，goroutine 远比线程小

　　*线程：*涉及模式切换(从用户态切换到内核态)、16个寄存器、PC、SP...等寄存器的刷新等。

　　*goroutine：*只有三个寄存器的值修改 - PC（程序计数器） / SP （堆栈指针）/ DX（数据寄存器）

## Context

> 参考资料：https://segmentfault.com/a/1190000024441501
>
> https://draveness.me/golang/docs/part3-runtime/ch06-concurrency/golang-context/

golang预定义了`backgroud`和`todo`这两个context可分别通过`context.Backgourd()`和`context.TODO()`返回。

```go
var (
	background = new(emptyCtx)
	todo       = new(emptyCtx)
)

func Background() Context {
	return background
}

func TODO() Context {
	return todo
}
```

**context接口内容**

```go
type Context interface {
	Deadline() (deadline time.Time, ok bool)
	Done() <-chan struct{}
	Err() error
	Value(key interface{}) interface{}
}
```

1. `Deadline` — 返回 `context.Context` 被取消的时间，也就是完成工作的截止日期；

2. `Done` — 返回一个 Channel，这个 Channel 会在当前工作完成或者上下文被取消后关闭，多次调用 `Done` 方法会返回同一个 Channel；

3. `Err`— 返回`context.Context`结束的原因，它只会在`Done`方法对应的 Channel 关闭时返回非空的值；
1. 如果 `context.Context` 被取消，会返回 `Canceled` 错误；
   
2. 如果 `context.Context` 超时，会返回 `DeadlineExceeded` 错误；
4. `Value` — 从 `context.Context` 中获取键对应的值，对于同一个上下文来说，多次调用 `Value` 并传入相同的 `Key` 会返回相同的结果，该方法可以用来传递请求特定的数据；



在 Goroutine 构成的树形结构中对**信号进行同步**以减少计算资源的浪费是 `context.Context` 的最大作用。



**衍生子Context：with系列函数**

```go
func WithCancel(parent Context) (ctx Context, cancel CancelFunc)
func WithDeadline(parent Context, deadline time.Time) (Context, CancelFunc)
func WithTimeout(parent Context, timeout time.Duration) (Context, CancelFunc)
func WithValue(parent Context, key, val interface{}) Context
```

这四个`With`函数，接收的都有一个partent参数，就是父Context，我们要基于这个父Context创建出子Context的意思，这种方式可以理解为子Context对父Context的继承，也可以理解为基于父Context的衍生。

`WithCancel`函数，传递一个父Context作为参数，返回子Context，以及一个取消函数用来取消Context。 `WithDeadline`函数，和`WithCancel`差不多，它会多传递一个截止时间参数，意味着到了这个时间点，会自动取消Context，当然我们也可以不等到这个时候，可以提前通过取消函数进行取消。

`WithTimeout`和`WithDeadline`基本上一样，这个表示是超时自动取消，是多少时间后自动取消Context的意思。前者设置超时时间，后者设置超时的截止时间。

`WithValue`函数和取消Context无关，它是为了生成一个绑定了一个键值对数据的Context，这个绑定的数据可以通过`Context.Value`方法访问到

前三个函数都返回一个取消函数`CancelFunc`，这就是取消函数的类型，该函数可以取消一个Context，以及这个节点Context下所有的所有的Context，不管有多少层级。

```go
type CancelFunc func()
```



**示例**

```go
var key string="name"

func main() {
   ctx, cancel := context.WithCancel(context.Background())
   //附加值
   valueCtx:=context.WithValue(ctx,key,"【监控1】")
   go watch(valueCtx)
   time.Sleep(10 * time.Second)
   fmt.Println("可以了，通知监控停止")
   cancel()
   //为了检测监控过是否停止，如果没有监控输出，就表示停止了
   time.Sleep(5 * time.Second)
}

func watch(ctx context.Context) {
   for {
      select {
      case <-ctx.Done():
         //取出值
         fmt.Println(ctx.Value(key),"监控退出，停止了...")
         return
      default:
         //取出值
         fmt.Println(ctx.Value(key),"goroutine监控中...")
         time.Sleep(2 * time.Second)
      }
   }
}

//【监控1】 goroutine监控中...
//【监控1】 goroutine监控中...
//【监控1】 goroutine监控中...
//【监控1】 goroutine监控中...
//【监控1】 goroutine监控中...
//可以了，通知监控停止
//【监控1】 监控退出，停止了...
```

## 调度器

> 参考资料：https://draveness.me/golang/docs/part3-runtime/ch06-concurrency/golang-goroutine/

Go 语言的调度器通过使用与 **CPU 数量相等**的线程减少线程频繁切换的内存开销，同时**在每一个线程上执行**额外开销更低的 **Goroutine** 来降低操作系统和硬件的负载。

调度器的发展历程：

- 单线程调度器
- 多线程调度器
- 任务窃取调度器
- 抢占式调度器
  - 基于协作的抢占式调度器
  - 基于信号的抢占式调度器（至今使用）
- 非均匀存储访问调度器（计划）

### GMP模型

> 参考资料：https://draveness.me/golang/docs/part3-runtime/ch06-concurrency/golang-goroutine/
>
> https://zhuanlan.zhihu.com/p/261057034
>
> https://juejin.cn/post/6844904034449489933
>
> https://wudaijun.com/2018/01/go-scheduler/

处理器 P 是M线程和 Goroutine 的中间层

处理器持有一个由**可运行的 Goroutine** 组成的环形的运行队列 `runq`，还反向持有一个线程。调度器在调度时会从处理器的队列中选择队**列头**的 Goroutine 放到线程 M 上执行。

基于**工作窃取**的多线程调度器将每一个线程绑定到了**独立的 CPU** 上，这些线程会被不同**处理器**管理，不同的处理器通过**工作窃取**对任务进行**再分配**实现**任务的平衡**，也能提升调度器和 Go 语言程序的整体性能。

每个进程都有一个全局的G队列，也拥有P的本地执行队列，同时也有不在运行队列中的G。如正处于channel的阻塞状态的G，还有脱离P绑定在M的(系统调用)G，还有执行结束后进入P的gFree列表中的G等等，接下来列举一下常见的几种状态。

![golang-gmp](../../../E/typora/Pictures/2020-02-02-15805792666151-golang-gmp.png)

**M的状态**

**自旋线程**：处于运行状态但是没有可执行goroutine的线程(如下图)，数量最多为GOMAXPROC，若是数量大于GOMAXPROC就会进入休眠。

**非自旋线程**：处于运行状态有可执行goroutine的线程。



#### 调度场景

**Channel阻塞**：当goroutine读写channel发生阻塞时候，会调用gopark函数，该G会脱离当前的M与P，调度器会执行schedule函数调度新的G到当前M。

> 当Goroutine因为Channel操作而阻塞(通过gopark)时，对应的G会被放置到某个wait队列(如channel的waitq)，该G的状态由`_Gruning`变为`_Gwaitting`，而M会跳过该G尝试获取并执行下一个G。
>
> 当阻塞的G被G2唤醒(通过goready)时(比如channel可读/写)，G会尝试加入G2所在P的runnext，然后再是P Local队列和Global队列。

**系统调用**：当某个G由于系统调用陷入内核态时，该P就会脱离当前的M，此时P会更新自己的状态为Psyscall，M与G互相绑定，进行系统调用。结束以后若该P状态还是Psyscall，则直接关联该M和G，否则使用闲置的处理器处理该G，或者进入全局队列。（在一个G因为系统调用产生阻塞的时候,能做到解绑PM, 并将剩余的G+P转移到另一个M上继续执行。 在这个M返回时, 它会尝试从别的线程那里偷一个P过来, 如果没偷到, 那么这个G则会进入全局G队列）

**系统监控**：当某个G在P上运行的时间超过**10ms**时候，或者P处于Psyscall状态过长等情况就会调用retake函数，触发新的调度。

**主动让出**：由于是协作式调度，该G会主动让出当前的P，更新状态为Grunnable，该P会调度队列中的G运行。

<img src="../../../E/typora/Pictures/v2-d9d8dadcdaf2d3119b5f488d9da7bf2c_720w.jpg" alt="img" style="zoom: 67%;" />

- 如果P不再与任何M相关联了,  Golang会在合适的时间点下断开P&M, 然后另寻其主的切换M, 使得P中的G能够在合适的时间点下更好的运行，就会被直接投入调度器的空闲队列中, `runtime.shcedt.pidle`, 等待后续需要的时候再投入使用。

- 当P空闲时会首先从全局G队列拿一些任务来做，如果全局队列为空，这时它会从别的M偷一些任务出来（通常是偷一半）

**调度生命周期**

<img src="../../../E/typora/Pictures/image-20210118180626247.png" alt="image-20210118180626247" style="zoom:67%;" />

#### M0

M0 是启动程序后的编号为 0 的主线程，这个 M 对应的实例会在全局变量 runtime.m0 中，不需要在 heap 上分配，M0 负责执行初始化操作和启动第一个 G， 在之后 M0 就和其他的 M 一样了。

#### G0

G0 是每次启动一个 M 都会第一个创建的 gourtine，G0 是仅用于**负责调度的 G**，G0 不指向任何可执行的函数，**每个 M 都会有一个自己的 G0**。在调度或系统调用时会使用 G0 的栈空间，全局变量的 G0 是 M0 的 G0。

## GC垃圾回收

> 参考资料：https://segmentfault.com/a/1190000018161588

### 标记-清除算法（mark and sweep）

该方法分为两步，标记从**根变量**开始迭代得遍历所有被引用的对象，对能够通过应用遍历访问到的对象都进行标记为“被引用”；标记完成后进行清除操作，对没有标记过的内存进行回收（回收同时可能伴有碎片整理操作）。这种方法解决了引用计数的不足，但是也有比较明显的问题：每次启动垃圾回收都会暂停当前所有的正常代码执行，回收是系统响应能力大大降低！当然后续也出现了很多mark&sweep算法的变种（如三色标记法）优化了这个问题。

### 三色标记法

三色标记算法原理如下：

1. 起初所有对象都是白色。
2. 从根出发扫描所有可达对象，标记为灰色，放入**待处理队列**。
3. 从队列取出灰色对象，将其引用对象标记为灰色放入队列，自身标记为黑色。
4. 重复 3，直到灰色对象队列为空。此时白色对象即为垃圾，进行回收。

三色标记的一个明显好处是能够让用户程序和 mark 并发的进行

### GC的几个重要阶段

> 参考资料：https://wudaijun.com/2020/01/go-gc-keypoint-and-monitor/

#### Mark Prepare - STW

做标记阶段的准备工作，需要停止所有正在运行的goroutine(即**STW**)，**标记根对象**，启用**内存屏障**，内存屏障有点像内存读写钩子，它用于在后续并发标记的过程中，维护三色标记的完备性(三色不变性)，这个过程通常很快，大概在**10-30微秒**。

#### Marking - Concurrent

标记阶段会将大概**25%(gcBackgroundUtilization)的P用于标记对象**，**逐个扫描所有G的堆栈**，执行三色标记，在这个过程中，所有**新分配的对象都是黑色**，**被扫描的G会被暂停，扫描完成后恢复**，这部分工作叫后台标记gcBgMarkWorker。这会降低系统大概25%的吞吐量，比如`MAXPROCS=6`，那么GC P期望使用率为`6*0.25=1.5`，这150%P会通过专职(Dedicated)/兼职(Fractional)/懒散(Idle)三种工作模式的Worker共同来完成。

这还没完，为了保证在Marking过程中，其它G分配堆内存太快，导致Mark跟不上Allocate的速度，还需要其它G配合做一部分标记的工作，这部分工作叫**辅助标记**(mutator assists)。

#### Mark Termination - STW

标记阶段的最后工作是Mark Termination，关闭**内存屏障**，**停止后台标记**以及**辅助标记**，做一些清理工作，整个过程也需要STW，大概需要60-90微秒。在此之后，所有的P都能继续为应用程序G服务了。

#### Sweeping - Concurrent

在标记工作完成之后，剩下的就是清理过程了，**清理过程的本质是将没有被使用的内存块整理回收给上一个内存管理层级**(mcache -> mcentral -> mheap -> OS)，清理回收的开销被平摊到应用程序的每次内存分配操作中，直到所有内存都Sweeping完成。当然每个层级不会全部将待清理内存都归还给上一级，避免下次分配再申请的开销。