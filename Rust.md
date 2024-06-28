# 入门篇

## 安装

### RustUp

工具链版本：v1.79.0

官方：[Rust Programming Language](https://www.rust-lang.org/)

使用国内的镜像：

[rustup | 镜像站使用帮助 | 清华大学开源软件镜像站 | Tsinghua Open Source Mirror](https://mirrors.tuna.tsinghua.edu.cn/help/rustup/)

添加环境变量：（Windows）

```powershell
setx RUSTUP_UPDATE_ROOT "https://mirrors.tuna.tsinghua.edu.cn/rustup/rustup"  
setx RUSTUP_DIST_SERVER "https://mirrors.tuna.tsinghua.edu.cn/rustup"
```

运行：rustup-init.exe

```powershell
# 默认安装。
# 安装稳定版本的 Rust 工具链，以及默认的工具和数据集。
1) Proceed with standard installation (default - just press enter)
# 自定义安装
2) Customize installation
# 取消安装
3) Cancel installation

# 这是指定 Rust 代码应该被编译成的平台和架构
Default host triple? [x86_64-pc-windows-msvc]
# 这是指定 Rust 编译器的发布通道（稳定版、测试版、最新版、不安装）
Default toolchain? (stable/beta/nightly/none) [stable]
# 这是指定 Rust 的工具和数据集（最少的工具和库，常用的，完整的）
Profile (which tools and data to install)? (minimal/default/complete) [default]
# 这是将 Rust 的工具添加到系统环境，这样就可以在终端中使用 rustc 和 cargo 等命令
Modify PATH variable? (Y/n)
```

检查安装是否成功。

```powershell
rustc --version
```

### 生成工具

Windows 系统的 C++ 编译器有两种，一个是微软的 MSVC，另一个是 GNU 的 MinGW。

#### MSVC

如果 Rust 编译器选择了`x86_64-pc-windows-msvc`，还需要安装 MSVC 的 C++ 生成工具。

下载：[Microsoft C++ 生成工具 - Visual Studio](https://visualstudio.microsoft.com/zh-hans/visual-cpp-build-tools/)

工作负载选择“使用 C++ 的桌面开发”。

#### GUN

[MinGW-w64](https://www.mingw-w64.org/)

推荐使用 Cygwin ，添加 mingw64-x86_64 的 `gcc-g++`、`binutils`、`gdb`、`gcc-core`、`make`、`cmake`。

### 开发工具

#### Visual Studio Code

[Download Visual Studio Code - Mac, Linux, Windows](https://code.visualstudio.com/Download)

**插件**：

rust-analyzer：[rust-analyzer - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=rust-lang.rust-analyzer)

调试：

Windows - [C/C++ - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)

macOS/Linux - [CodeLLDB - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=vadimcn.vscode-lldb)

#### RustRover

[Download RustRover - JetBrains Rust IDE](https://www.jetbrains.com/rust/download/)

## 第一个程序

```powershell
cargo new hello_world
```

文件名： Cargo.toml

详细配置：[The Manifest Format - The Cargo Book (rust-lang.org)](https://doc.rust-lang.org/cargo/reference/manifest.html)

```toml
[package]
name = "hello_world"
version = "0.0.1"
edition = "2024"

[dependencies]
```

文件名： src/main.rs

```rust
fn main() {
    println!("Hello, world!");
}
```

运行程序

```powershell
cargo run
```

## mdBook

mdBook 是一个由 Rust 语言编写的开源工具，旨在帮助开发者使用 Markdown 语法编写书籍或文档，并将其编译为美观、交互式的 HTML 页面。它的核心理念是将注意力集中在内容上，而不是复杂的排版细节，让作者专注于创作，而 mdBook 则负责处理剩下的工作。

安装：运行`cargo install mdbook`，从 https://crates.io/ 安装 mdbook 工具

然后在`src`目录中编写`md`文件。初始化：`mdbook init`。

本地运行：`mdbook serve`访问 http://localhost:3000 。

## 基本语法

### 输入和输出

```rust
// 引入标准库，这里有封装好的函数，开箱即用
use std::io;

// 程序的入口main函数
fn main() {
    // 输出信息到控制台的函数
    println!("Guess the number!");

    println!("Please input your guess.");

    // 使用let变量存储值
    // 默认是不可改变的变量，如果需要改变，使用mut标记
    let mut guess = String::new();

    // 使用标准库的输入输出功能
    io::stdin()
        // 获取用户的输入，以换行为结束
        .read_line(&mut guess)
        // 处理潜在的错误
        .expect("Failed to read line");
    // 支持占位符
    println!("You guessed: {guess}");
}
```

### 引入外部依赖

文件名： Cargo.toml

```toml
[dependencies]
# 添加随机数的依赖
rand = "0.8.5"
```

**构建程序或更新程序**

```powershell
# 更新
cargo update
# 编译
cargo build
# 运行
cargo run
```

**更换国内源**

[crates.io-index.git | 镜像站使用帮助 | 清华大学开源软件镜像站 | Tsinghua Open Source Mirror](https://mirrors.tuna.tsinghua.edu.cn/help/crates.io-index.git/)

全局配置文件：

Windows: `%USERPROFILE%\.cargo\config.toml`
Unix: `$HOME/.cargo/config.toml`

```toml
[source.crates-io]
registry = "https://github.com/rust-lang/crates.io-index"
replace-with = "tuna"

[source.tuna]
registry = "https://mirrors.tuna.tsinghua.edu.cn/git/crates.io-index.git"
```

文件名： src/main.rs

```rust
// 输出信息到控制台的函数
println!("Guess the number!");

// 使用随机数生成库，产生了一个[1-100]
let secret_number = rand::thread_rng()
    .gen_range(1..=100);

println!("The secret number is:{secret_number}");
```

### 变量的类型

```rust
// 对变量重新定义，更换类型，变量名直接复用
let guess: u32 = guess.trim().parse().expect("Please type a number!");

// guess是String类型
// secret_number因为guess明确指出了是u32的类型而是u32类型
match guess.cmp(&secret_number) {
    Ordering::Less => { println!("小了") }
    Ordering::Equal => { println!("赢了") }
    Ordering::Greater => { println!("大了") }
}
```

### 控制流语句

```rust
//...
// 循环执行
loop {
    println!("Please input your guess.");
    let mut guess: String = String::new();
    io::stdin()
        .read_line(&mut guess)
        .expect("Failed to read line");
    println!("You guessed: {guess}");
    let guess: u32 = guess.trim().parse().expect("Please type a number!");
    match guess.cmp(&secret_number) {
        Ordering::Less => { println!("小了") }
        Ordering::Equal => {
            println!("赢了");
            break;// 退出loop循环
        }
        Ordering::Greater => { println!("大了") }
    }
}
```

### 处理错误输入

```rust
//...
// 使用match表达式
let guess: u32 = match guess.trim().parse() {
    // 没有错误时
    Ok(num) => num,
    // 有错误时
    // _可以忽略的变量名
    // continue是继续执行loop循环
    Err(_) => {
        println!("别闹");
        continue;
    }
};
//...
```

### 猜数字

```rust
// 引入标准库，这里有封装好的函数，开箱即用
use std::io;
use std::cmp::Ordering;
use rand::Rng;

// 程序的入口main函数
fn main() {
    // 输出信息到控制台的函数
    println!("Guess the number!");

    // 使用随机数生成库，产生了一个[1-100]
    let secret_number = rand::thread_rng()
        .gen_range(1..=100);

    println!("The secret number is:{secret_number}");

    loop {
        println!("Please input your guess.");

        // 使用let变量存储值
        // 默认是不可改变的变量，如果需要改变，使用mut标记
        let mut guess: String = String::new();

        // 使用标准库的输入输出功能
        io::stdin()
            // 获取用户的输入，以换行为结束
            .read_line(&mut guess)
            // 处理潜在的错误
            .expect("Failed to read line");
        // 支持占位符
        println!("You guessed: {guess}");

        // 使用match表达式
        let guess: u32 = match guess.trim().parse() {
            // 没有错误时
            Ok(num) => num,
            // 有错误时
            // _可以忽略的变量名
            // continue是继续执行loop循环
            Err(_) => {
                println!("别闹");
                continue;
            }
        };

        // guess是String类型
        // secret_number是i32类型
        match guess.cmp(&secret_number) {
            Ordering::Less => { println!("小了") }
            Ordering::Equal => {
                println!("赢了");
                break;// 退出loop循环
            }
            Ordering::Greater => { println!("大了") }
        }
    }
}
```

# 基础篇

## 变量绑定

```rust
let x = 5;
```

使用 `let` 关键字来声明变量。变量默认是不可变的，这意味着一旦绑定了一个值，就不能再改变它。

### 可变性

如果需要改变一个变量的值，可以使用 `mut` 关键字来声明一个可变的变量。

```rust
let mut x = 5;
x = 6;
```

### 类型注解

Rust 是一种静态类型语言，但通常不需要显式地声明变量的类型，因为 Rust 的编译器可以通过变量的初始值来推断其类型。

然而，在某些情况下，你可能需要提供一个类型注解。

```rust
let x: i32 = 5;// 32位的整数类型
```

### 常量

```rust
const PI: f64 = 3.141592653589793;// 常量
```

### 遮蔽

```rust
// 在 main 函数的作用域内声明一个变量 x 并赋值为 5
let x = 5;
// 输出 "5"，因为此时只有这一个 x 在作用域内
println!("{}", x);

{
    // 进入一个新的作用域（由花括号 {} 定义）
    // 在这个新的作用域内，声明一个与外层 x 同名的变量 x 并赋值为 10
    // 这会遮蔽外层的 x
    let x = 10;
    // 输出 "10"，因为此时这个作用域内的 x 遮蔽了外层的 x
    println!("{}", x);
} // 离开这个新的作用域，里面的 x 不再可见

// 输出 "5"，因为我们又回到了 main 函数的作用域，只有外层的 x 可见
println!("{}", x);
```

### 作用域

变量的作用域：从声明它的位置开始，一直延伸到包含该声明的块的末尾。这个“块”可以是一个函数体、一个花括号 `{}` 包围的代码段，或者任何其他定义了作用域的构造。

在作用域内，变量可以被访问和使用。一旦离开该作用域（即代码执行到块的末尾之后），变量就不再可用，除非它在另一个嵌套的作用域中被重新声明（这将导致变量遮蔽）。

## 数据类型

Rust 是一种静态类型语言，意味着在编译时，每个变量和表达式都必须有已知的类型。

- 基本类型：整数，浮点数，布尔，字符。

- 复合类型：元组，数组。

### 基本类型

#### 整数

| 关键字   | 最小值                        | 最大值                        |
|:-----:|:--------------------------:|:--------------------------:|
| `i8`  | -128                       | 127                        |
| `i16` | -32,768                    | 32,767                     |
| `i32` | -2,147,483,648             | 2,147,483,647              |
| `i64` | -9,223,372,036,854,775,808 | 9,223,372,036,854,775,807  |
| `u8`  | 0                          | 255                        |
| `u16` | 0                          | 65,535                     |
| `u32` | 0                          | 4,294,967,295              |
| `u64` | 0                          | 18,446,744,073,709,551,615 |

`isize`和`usize`是Rust中两种特殊的整数类型，它们的位宽取决于目标平台。

> [!NOTE]
> 
> 128位的整数类型从 1.26 版本开始支持。
> 
> - `u128`: `0 - 340,282,366,920,938,463,463,374,607,431,768,211,455`
> - `i128`: `−170,141,183,460,469,231,731,687,303,715,884,105,728 - 170,141,183,460,469,231,731,687,303,715,884,105,727`

**整数的字面值**：

默认使用`i32`数据类型。

```rust
// 默认是i32类型，可以在字面值后标记特定的类型
let v = 12345u32;
// 建议使用下划线分隔
let _v = 12345_u32;
// 输出：12345
println!("{}", v);
```

可以使用下划线`_`分隔。

```rust
// 可以使用下划线分隔
let dec = 1_23_456_789;
// 十六进制使用0x作为前缀
let hex = 0x7F_FF;
// 八进制使用0o作为前缀
let oct = 0o123_456;
// 二进制使用0o作为前缀
let byte = 0b0111_1111;
// 单字节字符，使用b作为前缀
let c = b'A';// 单引号
```

**整数溢出**：

调试模式下，包含了对整数溢出的检查。如果发生溢出，程序会在运行时触发 panic，这会导致程序终止并打印出一个错误消息。

发布模式下，大于类型可以持有的最大值的值，会“环绕”回到类型可以持有的最小值。

对于 `u8` 类型，值 256 变为 0，值 257 变为 1，依此类推。

程序不会 panic，但变量将具有您可能不期望的值。

#### 浮点数

根据IEEE 754-2019标准 ，Rust中的浮点类型`f32`和`f64`分别对应于单精度（32位）和双精度（64位）浮点数。

| 关键字 |     名称     | 存储字节 | 10进制有效位数 | 2进制有效位数 |
| :----: | :----------: | :------: | :------------: | :-----------: |
| `f32`  | 单精度浮点数 |    4     |       6        |      24       |
| `f64`  | 双精度浮点数 |    8     |       15       |      53       |

数值运算：

```rust
// 加
let sum = 5 + 10;

// 减
let difference = 95.5 - 4.3;

// 乘
let product = 4 * 30;

// 除
let quotient = 56.7 / 32.2; // 浮点数除法，默认是f64类型
let truncated = -5 / 3; // 整除 -1

// 余
let remainder = 43 % 5;
```

#### 布尔

```rust
let t = true;
let f: bool = false;
```

#### 字符

长度：4byte，使用单引号标记，是Unicode的标量值，范围从`U+0000`到`U+D7FF`和从`U+E000`到`U+10FFFF`。

```rust
let c = 'z';
// 字面值只用\u和大括号转义
let z: char = '\u{30EDD}';
let e = '😻';
```

[Index of /Public (unicode.org)](https://www.unicode.org/Public/)

### 复合类型

#### 元组

```rust
// 不同基本类型组合为元组
let tuple: (i32, f64, u8) = (500, 6.4, 1);
// 使用3个变量对元组解构（模式匹配）
let (x, y, z) = tuple;
println!("The value of y is: {y}");
// 使用下标进行访问，从0开始
let v = tuple.2;
println!("The value of z is: {v}");
```

#### 数组

```rust
// 相同基本类型组合为数组
// 创建数组时长度和类型必须是确定的。
let a = [1, 2, 3, 4, 5];

// 创建5个元素，每个元素的值是3的数组
let mut b = [3; 5];

// 使用数组的索引方式读取a的第一个元素
// 并将其赋值给b的第一个元素
b[0] = a[0];

// [1, 2, 3, 4, 5]
println!("{:?}", a);
// [1, 3, 3, 3, 3]
println!("{:?}", b);

// 使用 .. 范围模式匹配数组中间的任意元素
// 从而解构到首尾的元素first和last，并将它们赋值给相应的变量
let [first, .., last] = a;
println!("{}..{}", first, last);

// 使用 _ 作为占位符来忽略某个元素的值，没有发生赋值操作
// 但是 _1st 和 _3th 变量发生了赋值操作，这两个变量可以使用
// 变量名前加下划线，可以忽略编译器的警告，因为后续没有用到这个变量
let [_1st, _, _3th, _, _5th] = b;
```

**数组不能越界访问**：

```rust
use std::io; // 导入 Rust 标准库中的 io 模块，用于处理输入/输出

fn main() {
    // 定义一个包含五个整数的数组 a
    let a = [1, 2, 3, 4, 5];

    // 打印提示信息，要求用户输入一个数组索引
    println!("Please enter an array index.");

    // 创建一个新的空字符串，用于存储用户输入的索引
    let mut index = String::new();

    // 从标准输入读取一行文本，并存储在 index 变量中
    // expect 方法用于处理可能出现的错误，如果读取失败，则程序会输出 "Failed to read line" 并退出
    io::stdin()
        .read_line(&mut index)
        .expect("Failed to read line");

    // 移除字符串两端的空白字符（如空格、制表符、换行符等）
    // 然后尝试将处理后的字符串解析为一个无符号整数（usize）
    // 如果解析失败，则程序会输出 "Index entered was not a number" 并退出
    let index: usize = index
        .trim()
        .parse::<usize>() // 显式指定要解析的类型为 usize
        .expect("Index entered was not a number");

    // 使用解析得到的索引从数组 a 中获取对应的元素
    let element = a[index];

    // 打印获取到的元素的值和索引
    // 注意：在 println! 宏中，不需要在 {} 中指定变量名，{} 会自动插入变量的值
    println!("The value of the element at index {} is: {}", index, element);
}
```

## 函数

```rust
// 函数名为main是程序的入口
fn main() {
    let char_array: [char; 5] = ['a', 'b', 'c', 'd', 'e'];
    // 调用get_char_from_array函数，并传入参数，返回值传入println!宏
    println!("{}", get_char_from_array(&char_array, 2));
}

// fn是函数的关键字
// 函数名使用snake case风格，下划线连接单词
// 参数格式 形参名:类型
// ->指定返回值类型
// 可以使用return和指定值提前返回，也可以隐式的返回最后的表达式
fn get_char_from_array(arr: &[char], index: usize) -> char {
    // 函数体
    return arr[index];
}
```

### 语句和表达式

函数中的代码由语句和（可选）表达式构成。

```rust
// x = 1是表达式，使用let告诉程序定义一个变量是语句
let x = 1;
// 语句不能作为返回值，没法交给变量y
// let y = (let x = 1);
// 块作用域
let z = {
    println!("{x}");
    1 //返回1给变量z
};
// 这是个语句，只有一个分号
;
```

以下都是表达式：

- 函数的调用
- 宏调用
- 块作用域

**语句使用unit类型`()`，表示没有返回值。**

### 注释

#### 非文档注释

```rust
// 行注释：用于在代码行中插入注释，编译器会忽略该行注释及其后面的内容，直到行尾。

/*
块注释：用于跨越多行的注释，编译器会忽略这些注释块及其包含的内容。
*/
```

#### 文档注释

```rust
//! (Crate)内部行文档注释<p>

/*!
(Crate)内部块文档注释
*/

/// 外部行文档注释<p>
pub mod line_module {
    //! 内部行文档注释
}

/**
外部块文档注释
*/
pub mod block_module {
    /*!
    内部块文档注释
    */
}
```

文档注释使用markdown的语法：

```rust
// src/lib.rs
/// 模块文档
///
/// 此模块提供了使用整数和向量的功能。
///
/// 以下是演示此功能的示例函数：
/// 这是一个使用`std::Vec<i32>`的函数示例。
///
/// 它接受一个整数向量，并返回向量中所有偶数的和。
///
/// # 参数
///
/// - `numbers`: 包含整数的向量。
///
/// # 返回值
///
/// 返回一个`i32`类型的值，表示向量中所有偶数的和。
///
/// # 示例
///
/// ```rust
/// // 假设这个函数在：stu::sum_even_numbers
/// use stu::sum_even_numbers;
///
/// let numbers = vec![1, 2, 3, 4, 5, 6];
/// let sum_of_evens = sum_even_numbers(numbers);
/// assert_eq!(sum_of_evens, 12);
/// ```
pub fn sum_even_numbers(numbers: Vec<i32>) -> i32 {
    let mut sum = 0;
    for &num in &numbers {
        if num % 2 == 0 {
            sum += num;
        }
    }
    sum
}
```

#### 块注释的嵌套

```rust
/* 在Rust中 /* 可以 /* 嵌套注释 */ */ */
// 这三种类型的块注释都可以包含或嵌套在任何其他类型中：
/*   /* */  /** */  /*! */  */
/*!  /* */  /** */  /*! */  */
/**  /* */  /** */  /*! */  */
pub mod module {}
```

> [!NOTE]
> 
> - 注释中的转义符会失效。
> - Rust 的文档注释中不允许直接使用回车符（CR, `\r`, U+000D），这是因为 Rust 的源代码文件通常使用 LF（line feed, `\n`, U+000A）作为行结束符。
> - 在一些旧的文本文件或跨平台转换中，可能会遇到`\r\n`（CR后跟LF）这样的序列。但在 Rust 源代码文件中，这样的序列通常会被自动转换为单个 LF 字符。
> 
> 使用`cargo doc --open`命令生成并打开注释文档`target\doc\`。
> 
> 使用`cargo test`命令可以测试文档注释中的示例。

### 控制流

#### if 表达式

```rust
fn main() {
    let num1 = 5;
    let num2 = 10;
    println!(
        "The result of comparing {} and {} is {}",
        num1, num2, compare_numbers(num1, num2)
    );
}

fn compare_numbers(a: i32, b: i32) -> i32 {
    // if的条件必须是布尔值，分支返回值类型必须一致
    if a < b { -1 } else if a > b { 1 } else { 0 }
}
```

#### 循环

```rust
fn main() {
    let x = 'outer: loop {
        // while：条件满足进入循环
        // loop：条件满足，在内部退出循环
        'inner: while true {
            println!("Entered the inner loop");

            // 这只是中断内部的循环
            //break;

            // 借助标签，这会中断外层循环
            // 循环是表达式，可以返回值
            break 'outer 5;
        }
        // 标记为不可达的代码，以确保逻辑正确
        unreachable!();
    };
    println!("{}", x);
}
```

```rust
fn fibonacii(n: i32) {
    let mut a = 0;
    let mut b = 1;

    // .. 是中缀表达式，表示从左边到右边的区间
    // 0..20 => [0,n)
    // 0..=20 => [0,n]
    // (0..20).rev() => (n,0]
    for i in 1..=n {
        println!("{i}=>{a}");
        let next = a + b;
        a = b;
        b = next;
    }
}
```

## 所有权

Rust 通过控制变量的所有权来实现垃圾回收，变量失去所有权，内存就会被回收清理。

> [!TIP]
> 
> **堆和栈**：
> 
> 堆和栈都是代码运行时可以使用的内存，用来对数据进行存取，但是结构不同。
> 
> - 栈：先进后出，就像叠起来的盘子，放盘子时放最上面一层（压栈），拿盘子也是从最上面一层取（弹栈），存取速度很快。
> - 堆：仓库，随时可以存放物品（分配内存），也可以取出物品（释放内存），但是这个过程要从仓库中寻找能放得下的位置，会花一些时间。

规则：

> [!IMPORTANT]
> 
> - Rust 的每一个值都有一个所有者。
> - 这个值在任何时刻有且只有一个所有者。
> - 当所有者离开作用域，这个值就会被删除

这就像是一群孩子围着一个玩具，但只有一个孩子可以真正拥有它，其他孩子只能看着或者经过允许后玩一下。真正拥有这个玩具的孩子可以把玩具送给别的孩子，但是一旦这样做了，这个孩子就不能再玩这个玩具了。如果所有的孩子都遗忘了这个玩具，没有人再需要它，这个玩具就被丢弃回收了。

这样就很好的解决了没有及时释放内存而引用到了奇怪的位置，获取到了奇怪的值。

### 变量作用域

```rust
fn main() {
    let s = get();
    println!("{s}");
}

fn get() -> String {
    // 两种方式都可以，只是代码风格不同
    // let s = String::from("Hello");
    let s = "Hello".to_string();
    return s;
}
```

1. 字符串字面量`"Hello"`是硬编码到代码中的，编译时会被存放到程序的只读数据段（通常称为文本段或常量段）。这部分内存在程序运行时是只读的，无法被修改。
2. 在`get()`函数中，调用了`"Hello".to_string()`。这个调用创建了一个新的`String`实例，这个实例会在堆上分配内存来存储`"Hello"`的副本。堆是程序运行时用于动态内存分配的区域，这意味着可以在运行时根据需要分配和释放内存。
3. 当`to_string()`方法执行完毕后，它返回了新创建的`String`实例的所有权给`get()`函数中的局部变量`s`。在 Rust 中，所有权是一种机制，用于确保资源（如内存）在不再需要时被适当地释放。
4. 当`get()`函数返回时，它返回了局部变量`s`（即`String`实例）的所有权给调用者。在这个例子中，调用者是`main()`函数。`main()`函数中的变量`s`接收了这个`String`实例的所有权，因此现在可以安全地使用它。
5. 在`main()`函数中，调用了`println!("{s}")`来打印字符串。这里，`println!`宏接收了`s`的所有权，并在打印完成后释放了相关的资源。注意，`println!`宏内部可能会立即或稍后释放`String`实例占用的堆内存，这取决于具体的实现和运行时系统。
6. 由于`main()`函数是程序的入口点，并且没有其他后续的代码引用`s`（它已经被`println!`消耗了），因此不会有内存泄漏。Rust的所有权系统确保了这一点。

### 内存与分配

字符串字面值编译时就确定了，为了快速和高效的访问，就硬编码进可执行文件中，这种字符串不可以被修改。

为了支持一个可变的字符串，需要在堆上分配一块在编译时不可知的内存存放，运行时向内存分配器请求内存，获取一块内存的所有权，当这个内存使用完成后，把所有权归还给内存分配器。

内存分配器会在实例作用域结束时，自动调用实例编写的`drop`函数，释放内存。

这种生命周期结束时释放资源和内存的模式为**资源获取即初始化（Resource Acquisitions Is Initialization, RAII）**。

### 变量与数据交互的方式

#### 移动

```rust
// 将数值5交给变量x
let x = 5;
// 将x的拷贝交给变量y
let y = x;
// 整数是已知固定大小的简单值
// 所以这两个5都放到了栈中

// 字符串由于长度不固定，放在了堆上
// 将访问堆的数据的所有权交给了s1
let s1 = String::from("hello");
// s1将自己的数据地址拷贝交给变量s2
let s2 = s1;
// s1和s2是两个不同的数据地址拷贝
// 但是指向堆的同一个内存位置
```

当变量 x 和 y 都离开自己的作用域后，各自的数值 5 都会被释放，但是变量 s1 和 s2 是地址，如果 s1 离开作用域后释放了堆内存，s2 会造成对堆内存的二次释放错误，这是很致命的，如果 s1 释放后，在 s1 和 s2 之间有别的变量使用了这段内存，会导致内存污染。

为了确保堆内存的安全访问，s1 把堆内存的地址交给 s2 后，s1 不能再使用这块堆内存，这就是**所有权转移**。

```rust
let s1 = "Hello".to_string();
let s2 = s1;// 没有发生拷贝
// 变量s1的值的所有权已经交出，无法使用
println!("{s1}");
println!("{s2}");
```

#### 克隆

如果需要深度复制堆上的数据，而不是在栈上做引用的转移，可以使用克隆函数。

```rust
let s1 = "Hello".to_string();
let s2 = s1.clone();// 发生拷贝
// 变量s1的值没有交给s2，可以使用，两个变量互不干扰
println!("{s1}");
println!("{s2}");
```

只在栈上的数据拷贝：

```rust
let x = 5;
let y = x;// 没有涉及堆的拷贝，只在栈上发生
println!("{x}");
println!("{y}");
```

基本类型和只含有基本类型的元组，这些变量类型只需要在栈上分配内存，都可以做到不需要克隆函数的拷贝。

所有权在函数之间的传递：

```rust
fn main() {
    // 将String对象交给变量s
    let s = String::from("hello");
    // 变量s将String的所有权交给函数的形参
    takes_ownership(s);
    //println!("{s}");// s的作用域已经结束

    // 将整数值5对象交给变量x
    let x = 5;
    // 变量x将数值5的拷贝的所有权交给函数的形参
    makes_copy(x);
} // x的作用域结束，释放了自己原有的数值5

fn takes_ownership(some_string: String) {
    // some_string接收了调用函数中变量s传入的String的所有权
    println!("{}", some_string);
}//some_string的作用域结束，String实例被释放


fn makes_copy(some_integer: i32) {
    // some_integer接收了外部变量x传入的数值5的拷贝的所有权
    println!("{}", some_integer);
}// some_integer作用域结束，拷贝的数值5释放
```

所有权从函数中返还调用者：

```rust
fn main() {
    // 变量s1接收到函数gives_ownership传递出的对String实例的索引的所有权
    let s1 = gives_ownership();
    // 变量s2得到了String的所有权
    let s2 = String::from("hello");
    // 变量s2将所有权交给函数的形参，自己失去了对String的控制，作用域结束
    // 变量s3接收到了函数返回的String的所有权
    let s3 = takes_and_gives_back(s2);
}// 变量s1和s3作用域结束，释放了各自的String实例的所有权，两个String被回收

fn gives_ownership() -> String {
    // 在这个函数中创建String的实例，将所有权交给变量some_string
    let some_string = String::from("yours");
    // 变量some_string作为返回变量，将所有权交给调用的位置，然后传递给s1
    some_string
}// 变量some_string作用域结束，因为已经把所有权交出去了

fn takes_and_gives_back(a_string: String) -> String {
    // 所有权在这个方法中经过a_string的转手，短暂的持有后又还了回去
    a_string
}
```

#### 引用

为了不将变量的所有权交给函数的形参，然后又把所有权传回来，来回踢皮球，只是让函数借用一下就行，可以使用引用。

```rust
fn main() {
    let s = "Hello World".to_string();
    // 变量s的值的所有权交了出去，可是为了后面使用，还得再从函数里传回来
    let ss = calc_length(s);
    println!("{}.len={}", ss.0, ss.1);
}
// 为了调用的函数后续能继续使用字符串的所有权，用完还得把所有权交还回去
fn calc_length(s: String) -> (String, usize) {
    let len = s.len();
    (s, len)
}
```

使用引用后：

```rust
fn main() {
    let s = "Hello World".to_string();
    // 创建一个对变量s的字符串内容的引用，而不是直接将String对象的引用交出去
    let l = calc_length(&s);
    println!("{}.len={}", s, l);
}

fn calc_length(s: &String) -> usize {
    // 引用的作用域与变量一致，只是在失效时不会释放引用的值，因为没有所有权
    // 只是用简单类型而不是构造元组，这是效率最高的办法
    s.len()
}
```

可变引用：

```rust
fn main() {
    let mut s = "Hello World".to_string();
    // 没有发生所有权的转移
    let l = calc_length(&mut s);
    println!("{}.len={}", s, l);
}

// 引用默认是只读，使用mut后可以修改和替换
fn calc_length(s: &mut String) -> usize {
    // 修改
    s.push_str("!");
    // 替换，使用*解引用
    *s = "123".to_string();
    s.len()
}
```

尝试创建多个可变引用：

```rust
fn main() {
    let mut s = String::from("hello");

    let mut r1 = &mut s;
    // 可变引用的拷贝传递给了r1，不能再次拷贝
    // let r2 = &mut s;

    // s的引用拷贝也不能再使用，已经传递给r1
    // println!("{}, {}", s, r1);
}
```

但是可以限制作用域，只要不满足同时拥有多个可变引用拷贝：

```rust
fn main() {
    let mut s = String::from("hello");
    {
        // 可变引用r1与r2不能同时存在
        let mut r1 = &mut s;
        println!("{}", r1);
    }

    let mut r2 = &mut s;

    println!("{}", r2);
}
```

避免数据竞争：

```rust
fn main() {
    let mut s = String::from("hello");
    // 不可变引用可以有多个，因为只能读
    let r1 = &s;
    let r2 = &s;
    let r3 = &mut s;
    // 在r3的作用域内不能同时存在可变引用和不可变引用
    println!("{},{},{}", r1, r2, r3);
}
```

非词法作用域生命周期（Non-Lexical Lifetimes, NLL）：

解决作用域重叠后，可以编译通过，r1 和 r2 依然在 `}` 位置被释放，这时候和 r3 作用域重叠，但是借用编译器会确保在 r3 的生命周期内，没有其他的可变引用和不可变引用存在而产生冲突。

```rust
fn main() {
    let mut s = String::from("hello");

    let r1 = &s;
    let r2 = &s;

    let r3 = &mut s;
    // r3作用域内，没有别的引用来打扰
    println!("{}", r3);
}
```

悬垂引用：

引用的堆上的数据已经被释放，但是引用还存在。Rust的编译器会确保引用永远与指向的数据同在。

```rust
fn main() {
    let s = dangle();
    println!("{}", s);
}

fn dangle() -> &String {
    let s = "hello".to_string();
    &s
}
```

**规则**：

> [!IMPORTANT]
> 
> 1. 在任何时间，只能有一个可变引用，或者只能有多个不可变引用，指向同一个堆的地址。
> 2. 引用必须总是有效的。

#### 切片

切片（`slice`）可以引用集合中的一段连续的元素序列，而不持有这个集合的所有权。

```rust
fn main() {
    let mut s = "1234567890".to_string();
    // 对s进行切片，[2,5)，Range
    print_slice(&s[2..5]);// 345
    // [2,5]，Range
    print_slice(&s[2..=5]);// 3456
    // RangeTo
    print_slice(&s[..5]);// 12345
    // 切片引用是不可变引用，这里将使用了可变引用，冲突
    // s.clear();
    // RangeFrom
    print_slice(&s[2..]);// 34567890
    // RangeFull
    print_slice(&s[..]);// 1234567890
}

fn print_slice(slice: &str) {
    println!("{slice}");
}
```

字符串字面值`&str`是一个不可变的切片引用。

```rust
let arr: [i32; 5] = [1, 2, 3, 4, 5];
// 对数组进行切片
let slice = &arr[0..2];
// 对数组类型的引用，不关心原始数组的长度
let a: &[i32] = &arr;
// 对切片的引用
let b: &[i32] = &slice;
```

## 结构体

### 定义和实例化结构

结构体类似于元组类型，只是结构体需要清楚的知道这些类型的含义，需要命名，元组只需要知道数据类型即可。

```rust
// 结构体关键字
struct User {
    // 结构体字段
    name: String,
    age: u8,
    id: u32,
    active: bool,
}

fn main() {
    // 实例可变，则所有字段都是可变的
    let mut user1 = User {
        id: 10000001,
        name: "张三".to_string(),
        age: 10,
        active: false,
    };

    // 将user1实例使用解构赋值分解为4个变量
    let User {
        name: n,
        age,
        id,
        active,
    } = user1;
    println!("{}", n);
    println!("{}", user1.age);
}
```

使用函数创建结构体的实例：

```rust
fn main() {
    let mut user1 = build_user("张三".to_string(), 10);
}

fn build_user(name: String, age: u8) -> User {
    User {
        // 函数的参数名和字段名相同时可以简写
        name,
        age,
        id: 1,
        active: true,
    }
}
// ...
```

从其他实例创建新的实例：

```rust
fn main() {
    let mut user1 = User {
        id: 10000001,
        name: "张三".to_string(),
        age: 10,
        active: false,
    };
    // 更新实例
    let mut user2 = User {
        id: 2,
        ..user1 //不加逗号，放在最后
    };
    // 旧的实例部分字段失去所有权，后续会使用生命周期解决
    println!("{}", user1.age);
    // println!("{}", user1.name);
}
```

元组结构体：

```rust
// 虽然字段类型和位置相同，但是类型不同
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

fn main() {
    let black = Color(0, 0, 0);
    let origin = Point(0, 0, 0);
    // 使用下标访问
    println!("{}", black.2);
}
```

没有任何字段的类单元结构体：

```rust
struct Null;

impl Clone for Null {
    fn clone(&self) -> Self {
        Null {}
    }
}

fn main() {
    // 虽然没有任何字段，但是确实创建了一个实例
    let obj1 = Null;
    let obj2 = obj1.clone();
}
```

### 使用结构体的示例

结构体的目的是整合相关联的变量，这些变量可以是任何数据类型。

计算长方形面积：

```rust
fn main() {
    let width = 30;
    let height = 50;

    println!(
        "这个区域的面积是{}", area(width, height)
    );
}

fn area(w: u32, h: u32) -> u32 {
    w * h
}
```

宽和高两个参数是分离的，用结构体封装后表面这两个参数是相互联系的，而且有具体的意义。

```rust
fn main() {
    let rect = Rectangle {
        width: 50,
        height: 30,
    };

    println!(
        "这个区域的面积是{}", area(&rect)
    );
}

struct Rectangle {
    width: u32,
    height: u32,
}

fn area(r: &Rectangle) -> u32 {
    r.width * r.height
}
```

输出结构体：

```rust
use std::fmt::{Display, Formatter};

fn main() {
    let rect = Rectangle {
        width: 50,
        height: 30,
    };

    // :? 
    /*
    Rectangle { width: 50, height: 30 }
    */

    // :#? 
    /*
    Rectangle {
        width: 50,
        height: 30,
    }
    */
    println!("{rect:#?}");

    /*
    [src\main.rs:10:5] rect = Rectangle {
        width: 50,
        height: 30,
    }
    */
    dbg!(&rect);
}

// 调试这个结构体
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

fn area(r: &Rectangle) -> u32 {
    r.width * r.height
}
```

### 方法

方法与函数类似，使用`fn`关键字和名称声明，可以拥有参数和返回值。不过方法与函数不同，方法在结构体的上下文中被定义，并且第一个参数总是`self`，代表调用该方法的结构体实例。

```rust
fn main() {
    let rect1 = Rectangle { width: 50, height: 30 };
    // 调试输出
    dbg!(&rect1);// 使用引用避免失去所有权
    // 直接使用.来调用方法或者字段
    println!("{}", rect1.area2());

    // 调用关联函数，使用::命名空间限定符
    println!("{}", Rectangle::area(&rect1));

    let mut rect2 = Rectangle::new(30, 20);
    rect2.double_width();

    // 一行输出
    println!("{rect1:?}");
    // 格式化输出
    println!("{rect2:#?}");

    println!("{}", rect1.can_hold(&rect2));
    println!("{}", rect2.can_hold(&rect1));
}

#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}
impl Rectangle{
    // 第一个参数是&self，指代调用的结构体实例
    fn area2(self: &Self) -> u32 {
        self.width * self.height
    }
}

// impl 可以把Rectangle的方法分开写，最后编译时还是会合并
impl Rectangle {
    // 构造方法
    pub fn new(width: u32, height: u32) -> Self {
        Self {
            width,
            height,
        }
    }

    // 关联函数，第一个参数不是&self，不作用于构造体的实例
    fn area(r: &Rectangle) -> u32 {
        r.width * r.height
    }

    // 修改调用的实例的字段
    fn double_width(&mut self) {
        self.width *= 2
    }

    // 第一个参数是&self
    // 位于结构体的实现中
    // 这个是方法
    fn can_hold(&self, other: &Rectangle) -> bool {
        self.width > other.width && self.height > other.height
    }
}
```

## 枚举

枚举是一种将一组命名常量组合成为一个类型的语法结构。这些常量通常用于表示一组有限的、固定的值，比如一周的七天、月份的名称、颜色的集合等。

```rust
enum Weekday {
    Monday,
    Tuesday,
    Wednesday,
    Thursday,
    Friday,
    Saturday,
    Sunday,
}

fn main() {
    let today = Weekday::Wednesday;

    // 匹配枚举值以执行不同的操作  
    match today {
        Weekday::Monday => println!("Today is Monday!"),
        Weekday::Tuesday => println!("Today is Tuesday!"),
        Weekday::Wednesday => println!("Today is Wednesday!"),
        Weekday::Thursday => println!("Today is Thursday!"),
        Weekday::Friday => println!("Today is Friday!"),
        Weekday::Saturday => println!("Today is Saturday!"),
        Weekday::Sunday => println!("Today is Sunday!"),
    }
    // 输出: Today is Wednesday!  
}
```

使用枚举封装不同数据类型的成员：

```rust
enum MyEnum {
    Integer(i32),
    Double(f64),
    Object(Option<String>),
}

impl MyEnum {
    fn print_value(&self) {
        match self {
            MyEnum::Integer(i) => println!("Integer: {}", i),
            MyEnum::Double(d) => println!("Double: {}", d),
            MyEnum::Object(s) => match s {
                Some(ref str) => println!("Object: {}", str),
                None => println!("Object is None"),
            },
        }
    }
}

fn main() {
    let int_value = MyEnum::Integer(42);
    let double_value = MyEnum::Double(3.14159);
    let object_value = MyEnum::Object(Some("Hello, Rust!".to_string()));
    let none_value = MyEnum::Object(None);

    int_value.print_value();
    double_value.print_value();
    object_value.print_value();
    none_value.print_value();
}
```

### Option

Option 是标准库的一个枚举，表示一个变量的值可能存在也可能无效。Rust没有Null。

```rust
fn divide(p: u32, q: u32) -> Option<u32> {
    if q == 0 {
        // 当分母为0时，返回None
        None
    } else {
        // 否则，把结果打包入Some返回
        Some(p / q)
    }
}
fn f_some() {
    let result = divide(10, 2);
    match result {
        Some(value) => println!("Result: {}", value),
        None => println!("Error: division by zero"),
    }
}

fn f_none() {
    let result_zero = divide(10, 0);
    match result_zero {
        Some(value) => println!("Result: {}", value),
        None => println!("Error: division by zero"),
    }
}

fn main() {
    f_some();
    f_none();
}
```

### match

控制流运算符

```rust
fn value(number: i32) -> String {
    let b = true;

    // match匹配不用担心会穿层
    // 使用match表达式来匹配传入的number
    match number {
        // 如果number是4
        4 => "The number is 4".to_string(),

        // 如果number是1, 2, 3, 且b为true
        // 注意这里使用竖线（|）来分隔多个值，如何还需要追加条件，使用（if）
        1 | 2 | 3 if b => "The number is 1, 2, or 3".to_string(),

        // 如果number在5到7之间（包括5和7）
        // 注意这里使用范围（..=）来表示闭区间，(...)的方式已经废弃
        // @ 将具体的结果绑定到变量n，
        n @ 5..=7 => format!("The number is {}", n),

        // 匹配必须穷尽
        // 下划线（_）是一个通配符，匹配所有其他未明确列出的模式
        // 这个分支一定要放在最后，因为依次从上往下比配，这个分支是最后匹配穷尽的
        _ => "The number is something else".to_string(),
    }
}

fn main() {
    println!("{}", value(4));       // 输出: The number is 4
    println!("{}", value(2));       // 输出: The number is 1, 2, or 3
    println!("{}", value(6));       // 输出: The number is 6
    println!("{}", value(10));      // 输出: The number is something else
    println!("{}", value(-1));      // 输出: The number is something else
}
```

match 不仅可以匹配值和枚举，还可以匹配结构体：

```rust
fn match_point(p: Point) {
    match p {
        // 只匹配y为0的情况，使用..忽略p的剩余值（不关注x的值）
        Point { y: 0, .. }
        => println!("该点位于y轴上，y坐标为{}", p.y), // 修正为p.y

        // 匹配x为0的情况，y可以是任何值
        Point { x: 0, y }
        => println!("该点位于x轴上，y坐标为{}", y),

        // 匹配x在0到5（包括5）范围内的点
        // ..= 替代了已废弃的 ... 的写法
        Point { x: x_value @ ..=5, y }
        => println!("该点位于({}, {})处，且x在0到5之间", x_value, y),

        // 其他所有情况
        Point { x, y }
        => println!("该点位于({}, {})处", x, y),
    }
}

struct Point {
    x: i32,
    y: i32,
}

fn main() {
    match_point(Point { x: 2, y: 5 });
    match_point(Point { x: 0, y: 3 });
    match_point(Point { x: 3, y: 0 });
    match_point(Point { x: 2, y: 0 });
    match_point(Point { x: 6, y: 7 });
}
```

### if let

使用`match`匹配不需要处理的分支时，可以使用`if let`只匹配需要的分支，减少代码的冗长。

```rust
fn main() {
    let config_max = Some(3u8);
    match config_max {
        Some(max) => println!("The maximum is configured to be {}", max),
        _ => (),// 这里由于使用 _ 忽略了这个分支，这是冗余的代码
    }

    // 使用 if let 只匹配 Some 分支，并解构其值到变量 max
    if let Some(max) = config_max {
        println!("The maximum is configured to be {}", max);
    }
}
```

这个语法结构其实和`if`是一样的，也可以配合`else`使用。

### let和if let的区别

不可反驳模式（irrefutable patterns）：不可能匹配失败，也不允许匹配失败。

```rust
// 不可反驳模式，x肯定能与5匹配成功
let x = 5;
// 同样，y总能与一个i32类型的值匹配成功
// 否则编译器不会放过这里的问题
let y: i32 = 10;
```

可反驳模式（refutable patterns）：可能匹配失败。

```rust
fn process_value(option: Option<i32>) {
    // 尝试将变量 option 与 Some 匹配，并解构其值到变量 v
    // 由于这里是可反驳模式，有匹配失败的可能
    // 于是借助if的语法给出两个分支（涵盖了Option枚举的所有可能）进行处理
    if let Some(v) = option {
        println!("The value is: {}", v);
    } else {
        println!("No value present");
    }
}
```

> [!NOTE]
>
> `if let`也可以用于匹配不可反驳模式，只是没有意义，因为不会匹配失败，无法执行到`else`分支。

# 进阶篇

## 项目结构管理

当编写大型程序时，对代码的管理能力尤其重要，因为能把整个项目所有的代码都熟悉是很难的事情，需要根据功能的划分进行管理，也可以将一个项目分给不同的人来完成，提高开发进度。

`Cargo.toml`配置清单：[The Manifest Format - The Cargo Book (rust-lang.org)](https://doc.rust-lang.org/cargo/reference/manifest.html)

### 工作空间**（Workspace）**

假设有一个大型项目，这个项目文件夹内有一个`Cargo.toml`用于管理这个项目。

```toml
[workspace]
members = [
    "ChatServer",
    "Common",
    "Database",
    "GameServer",
    "LoginServer"
]

# 也可以这样写
# workspace = { members = ["ChatServer", "Common", "Database", "GameServer", "LoginServer"] }
```

### 包（Package）

包是工作空间内用于划分代码功能的单元（文件夹）。

每个包内有一个`Cargo.toml`文件，用于管理包内的箱（Crate）。

这些箱存放在包内的`src`文件夹内，且必须至少有一个箱。

包内还可以有`tests`用于存放测试用例，`benches`用于存放基准测试，`examples`用于存放示例代码，`target`是编译后的产物。

> [!NOTE]
> 
> 包的发布配置：[Publishing a Crate to Crates.io - The Rust Programming Language (rust-lang.org)](https://doc.rust-lang.org/book/ch14-02-publishing-to-crates-io.html)
> 
> 发布到远程仓库后，包是永久存在的，无法覆盖和删除，谨慎。

### 箱（Crate）

Crate 是 Rust 编译时的最小单元。根据功能的不同，分为两种：

- 库箱（Library Crate）：编译后生成库文件，可以被其他 Crate 使用，或者其他语言使用。
- 二进制箱（Binary Crate）：编译后生成一个可执行文件。

根箱（Crate Root）是一个源文件，编译器编译时，默认情况下，是两个位于包的`src`文件夹内的特定名称的源文件：

- `src/main.rs`：二进制箱的根箱，必须有 main 函数作为程序的入口。
- `src/lib.rs`：库箱的根箱。

包内可以有多个二进制箱，通过`Cargo.toml`的`[[bin]]`配置。但是库箱只能有一个，通过`Cargo.toml`的`[lib]`配置。

项目结构示例：

```powershell
project # 游戏项目
│  .gitignore
│  Cargo.lock
│  Cargo.toml # 项目的包配置文件
│
├─ChatServer # 游戏的聊天服务
│  │  Cargo.toml
│  │
│  └─src
│          main.rs # 聊天服务的CrateRoot，是可执行的
│
├─Common # 游戏的公共资源
│  │  Cargo.toml
│  │
│  └─src
│          lib.rs # 公共资源的CrateRoot，是库
│
├─Database
│  │  Cargo.toml
│  │
│  └─src
│          lib.rs
│
├─GameServer
│  │  Cargo.toml
│  │
│  └─src
│          main.rs
│
└─LoginServer
    │  Cargo.toml
    │
    └─src
            main.rs
```

### 模块（Module）

在一个 Crate 内，将相关联的代码进行统一管理的作用域，使用`mod`关键字标记。

模块中所有成员都是默认私有，需要使用关键字`pub`才能对外公有，父模块不能使用子模块的私有成员，但是子模块中可以访问父模块中的成员。

```rust
// Stu/src/lib.rs
pub mod father {
    pub fn f_pub() {
        println!("father::f_pub")
    }


    fn f_pri() {
        println!("father::f_pri")
    }

    mod child_pri {
        use crate::father::child_pub;

        pub fn f_pub() {
            self::f_pri();// child_pri::f_pri
            child_pub::f_pub();// child_pub::f_pub
            super::f_pub();// father::f_pub
            super::f_pri();// father::f_pri
            crate::f_pri();// crate::f_pri

            println!("child_pri::f_pub")
        }

        fn f_pri() {
            println!("child_pri::f_pri")
        }
    }

    // 外部无法访问child_pri
    // 但是可以使用这个方法将内部可以公开的成员对外重新导出
    // as 是重命名，避免命名冲突
    pub use child_pri::f_pub as pub_use;

    pub mod child_pub {
        use crate::father::child_pri;

        pub fn f_pub() {
            self::f_pri();// child_pub::f_pri
            child_pri::f_pub();// child_pri::f_pub
            super::f_pub();// father::f_pub
            super::f_pri();// father::f_pri
            crate::f_pri();// crate::f_pri

            println!("child_pub::f_pub")
        }

        fn f_pri() {
            println!("child_pub::f_pri")
        }
    }
}

fn f_pri() {
    println!("crate::f_pri")
}
```

```rust
// Stu/src/main.rs
use Stu::father;

fn main() {
    // 可以访问father模块（包括子模块）中的一切公开成员
    father::f_pub();
    father::child_pub::f_pub();

    // 通过重导出访问到的内部私有成员的公有方法
    father::pub_use();
}
```

#### 路径

绝对路径：从Crate Root开始，以`crate`名或者字面值`crate`开始。

相对路径：从当前模块开始，以`self`、`super`或当前模块的名称标识符开始。

#### use、as

```rust
// 引入模块中的可见成员，还可以使用as重命名，避免命名冲突
use std::collections::HashMap as Map;
// 使用大括号组织use
use std::{
    fs::*, // 使用 * 是引入全部公共成员
    io::{Read, Write},
};

fn main() {
    // 局部引入也是可以的，但是不常见，不便于维护
    // 使用Glob运算符全部引入
    use std::collections::*;
    let mut map = Map::new();
    map.insert("key", "value");
}
```

### 模块系统

将模块的嵌套层次作为文件路径，从 Crate Root 开始创建目录结构：

```bash
Stu
│  Cargo.toml
│  
└─src
    │  father.rs # 注意：在现代 Rust 中，不再使用 mod.rs 的方式
    │  lib.rs # Crate Root，编译器从这里开始编译模块
    │  main.rs
    │  
    └─father
            mod.rs # 这种方式废弃了
            child_pri.rs
            child_pub.rs

```

```rust
// lib.rs
pub mod father;

fn f_pri() {
    println!("crate::f_pri")
}
```

```rust
// father.rs 替代旧的方式 father/mod.rs，两种方式不能共存
pub fn f_pub() {
    println!("father::f_pub")
}


fn f_pri() {
    println!("father::f_pri")
}

mod child_pri;

pub mod child_pub;

pub use child_pri::f_pub as pub_use;
```

```rust
// father/child_pri.rs
use crate::father::child_pub;

pub fn f_pub() {
    self::f_pri();// child_pri::f_pri
    child_pub::f_pub();// child_pub::f_pub
    super::f_pub();// father::f_pub
    super::f_pri();// father::f_pri
    crate::f_pri();// crate::f_pri

    println!("child_pri::f_pub")
}

fn f_pri() {
    println!("child_pri::f_pri")
}
```

```rust
// father/child_pub.rs
use crate::father::child_pri;

pub fn f_pub() {
    self::f_pri();// child_pub::f_pri
    child_pri::f_pub();// child_pri::f_pub
    super::f_pub();// father::f_pub
    super::f_pri();// father::f_pri
    crate::f_pri();// crate::f_pri

    println!("child_pub::f_pub")
}

fn f_pri() {
    println!("child_pub::f_pri")
}
```

```rust
// main.rs
use Stu::father;

fn main() {
    father::f_pub();
    father::child_pub::f_pub();

    father::pub_use();
}
```

## 集合

### Vector

向量（在 Rust 中称为动态数组或数组列表）是一种数据结构，它允许我们在内存中连续存储相同类型的多个值。

这些值被存储在连续的地址空间中，这使得访问和操作它们非常高效。

```rust
fn main() {
    // 创建向量集合需要指定存放元素的类型
    let mut v1: Vec<i32> = Vec::new();
    // 使用宏创建
    let mut v2 = vec![1, 2, 3, 4, 5, 6];
    let mut v3 = vec![1; 5]; // ()[]{}都可以

    v1.push(5); // 增加
    v1.append(&mut v2); // 将v2的全部元素从头到尾全部转移到v1
    v1.pop(); // 删除最后一个元素

    // [5, 1, 2, 3, 4, 5]
    println!("{:?}", &v1);
    // []
    println!("{:?}", &v2);

    // 使用get获取，获取了一个对Vec中元素的引用（Option<&T>）
    let opt: Option<&i32> = v3.get(0);
    // [1, 1, 1, 1, 1]
    println!("{:?}", &v3);

    // get到的opt变量下面会使用，此处禁止修改v3的操作
    //v3.push(6);

    // 1
    println!("{}", opt.unwrap());

    // 使用下标，得到一个对Vec中元素的不可变引用（&T）
    let a = v3[0];
    // 1
    println!("{}", a);

    // [1, 1, 1, 1, 1]
    println!("{:?}", &v3);
}
```

使用枚举，让 Vec 能存放不同的数据类型：

```rust
fn main() {
    let vec = vec![
        Data::Double(10.2),
        Data::Long(12),
        Data::String("str".to_string()),
    ];
    // [Double(10.2), Long(12), String("str")]
    println!("{:?}", vec);
}

#[derive(Debug)]
enum Data {
    Long(i64),
    Double(f64),
    String(String),
}
```

### 字符串

#### str

字符串切片是对存储在堆上或其他地方的 UTF-8 编码字符数据的引用。

- 字符串切片是不可变的，因此你不能修改它指向的字符串内容。
- 字符串切片没有所有权，这意味着它们不会在其生命周期结束时自动释放内存。相反，它们依赖于其引用的数据（通常是一个`String`或静态字符串字面量）来保持有效性。
- 字符串切片通常以`&str`的形式出现，其中`&`表示引用。
- 字符串切片通常用于函数参数和返回值，因为它们可以以零成本的方式传递，并且不需要担心所有权转移。

#### String

`String` 是Rust标准库提供的一个类型，用于动态分配和存储UTF-8编码的字符串。

- 它是可变的。
- 拥有其存储的字符串数据的所有权，并在其生命周期结束时自动释放内存。
- 底层使用堆内存来存储其数据，这使得它可以动态地增长和缩小。
- 当需要创建一个新的字符串或修改一个现有字符串时，通常会使用`String` 类型。

```rust
// 三种创建String实例的方式
let mut str = String::new();
// str切片转String，写法不同效果一样
let mut str = "".to_string();
let mut str = String::from("");
```

```rust
// 实现
pub struct String {
    // 8bit的字符
    vec: Vec<u8>,
}
```

```rust
let mut str = String::new();
let s = "Hello";

// 使用 + 拼接时，左边为String（可变），右边为str（不可变）
// str的所有权转移，后面不能使用
let mut s1 = str + s;
// 与 + 等价
s1.push_str(" World");
// 添加字符时，由于字符串是UTF-8，需要确定增加多少个byte
s1.push('!');

let s = format!("s1={s1}");
println!("{s}");

// + 对应字符串实例的add方法
// &String 强制转换为 &str，as_str方法
let s = s + &s1;
println!("{s}");
```

字符串取切片

必须正好切到字符的边界byte，否则会出现错误而中断执行，慎用。

```rust
let hello = String::from("नमस्ते");
let c = &hello[0..3];
// न
println!("{}", c);
```

遍历字符串的字符

[![unicode-segmentation-badge](https://badge-cache.kominick.com/crates/v/unicode-segmentation.svg?label=unicode-segmentation)](https://docs.rs/unicode-segmentation/1.2.1/unicode_segmentation/)

```rust
use unicode_segmentation::UnicodeSegmentation;

// 操作字符串的每一部分，最好明确是需要操作字符串的字节、标量值、字形簇
fn main() {
    println!("标量值");// 字符如何书写，包含音调符号
    let hello = String::from("नमस्ते");
    // न म स ् त े
    for a in hello.chars() {
        print!("{} ", a);
    }
    println!("字形簇");// 将音调符号整合后的一个可视化字符
    // न म स् ते
    for a in hello.graphemes(true) {
        print!("{} ", a);
    }
    println!("字节");// 存储的字节值向量
    // e0 a4 a8 e0 a4 ae e0 a4 b8 e0 a5 8d e0 a4 a4 e0 a5 87
    for a in hello.as_bytes() {
        print!("{:08b} ", a);
    }
}
```

### HashMap

```rust
use std::collections::HashMap;

fn main() {
    // 键值对，键不能重复，无序
    let mut age: HashMap<String, usize> = HashMap::new();

    let user1 = "张三".to_string();
    let user2 = "李四".to_string();
    let user3 = "王五".to_string();

    // 注意所有权会发生转移
    age.insert(user1.clone(), 10);
    age.insert(user2.clone(), 10);
    // 覆盖，修改
    age.insert(user2.clone(), 15);
    // 获取引用进行修改
    if let Some(v) = age.get_mut(&user1.clone()) { *v = 20 };
    // 如果不存在就添加值12，否则不添加
    age.entry(user3.clone()).or_insert(12);

    // 遍历HashMap
    // {"张三": 10, "李四": 10}
    println!("{:?}", &age);
    // 自动调用 map 的迭代器，遍历每一个键值对
    for (k, v) in &age {
        println!("{k}={v}");
    }

    // 根据键获取值
    let m1: usize = age[&user1];
    // copied：获取一个 Option<i32> 而不是 Option<&i32>
    let m2: usize = age.get(&user2).copied().unwrap();
}
```

将键值对合并：

```rust
use std::collections::HashMap;

fn main() {
    let names = vec!["张三", "李四", "王五"];
    let ages = vec![12, 11, 11];
    // map类型自动推导
    // zip将两个Vec整合
    let map = names.iter().zip(ages.iter()).collect::<HashMap<&str, i32>>();
    println!("{:?}", map);
}
```

## 错误

### panic

恐慌是一个用于表示程序遇到了不可恢复的错误的机制。当`panic!`被调用时，它会终止当前线程的执行，并开始解栈（unwinding）过程，这个过程会释放该线程所拥有的所有资源，并打印出一条错误消息。

```rust
// main.rs
fn main() {
    div(5, 0);
}

fn div(a: i32, b: i32) -> i32 {
    if b == 0 { panic!("除数为零"); }
    a / b
}
```

配置文件：

[Profiles - The Cargo Book (rust-lang.org)](https://doc.rust-lang.org/cargo/reference/profiles.html#panic)

```toml
# Cargo.toml

[profile.dev]
# 开发时为了可以逆向定位源码，编译优化需要关闭，加快编译时间
opt-level = 0

[profile.release]
# 默认方式：沿着函数调用栈逐一释放内存
# panic = "unwind"
# 直接终止进程，由系统回收进程的内存
panic = 'abort'

# 发布时为了程序体积小，性能高，编译器优化需要开启，编译速度会变慢
opt-level = 3
```

命令行后不加`--release`，并且在环境变量中设置`RUST_BACKTRACE=full`。

```rust
fn main() {
    let v = vec![1, 2, 3];
    // 缓冲区溢出
    // index out of bounds: the len is 3 but the index is 100
    println!("{}", v[100]);
}
```

> [!TIP]
>
> ````rust
> todo!();
> ````
>
> 使用 `todo!()` 的主要目的是作为一个占位符，告诉未来的开发者或自己这个地方还需要完成一些工作，避免在实现函数时忘记编写返回语句或处理返回值。
>
> 因为 `todo!()` 本身会返回一个类型（`!`），这个类型表示函数永远不会返回任何值（Never Type），也就是触发了`panic`。
>
> 将这个放到函数的最后，可以减少在这个函数内编写实现时，返回值类型推断造成的干扰。

### result

`Result` 类型是一个枚举，用于表示操作可能成功（`Ok(T)`）或失败（`Err(E)`），其中 `T` 是成功时返回的类型，`E` 是失败时返回的错误类型。

使用 `Result` 而不是 `panic` 可以使程序更加健壮，因为它允许显式地处理错误情况，而不是让程序在出现错误时立即崩溃。

这样，调用者就可以决定如何处理错误：是记录一个错误并继续执行、尝试不同的方法、还是将错误传递给上层调用者。

```rust
use std::fs::File;
use std::io::{Error, Read};

fn read_file(filename: &str) -> Result<String, Error> {
    // 使用 ? 操作符来处理错误
    // 如果失败，自动将 Err 信息传播给调用者
    // 如果成功，变量 file 自动获取到 Ok 内的变量
    let mut file = std::fs::File::open(filename)?;
    let mut contents = String::new();
    // 再次使用 ? 操作符
    file.read_to_string(&mut contents)?;
    Ok(contents)
}

fn create_file(filename: &str) -> Result<File, Error> {
    // 再次使用 ? 操作符
    Ok(File::create(filename)?)
}

fn main() {
    let filename = "example.txt";
    if let Err(e) = read_file(filename) {
        eprintln!("Error reading file: {}", e);
        // 发现文件不存在，尝试创建
        if let Err(e) = create_file(filename) {
            eprintln!("Error creating file: {}", e);
            // 实在是创建不了文件了
            panic!("Unable to create file {}", filename);
        }
    }
    // 假设文件创建成功，再次尝试读取
    if let Ok(file_contents) = read_file(filename) {
        println!("File contents: {}", file_contents);
    }
}
```

> [!NOTE]
>
> ```rust
> let mut file = r#try!(std::fs::File::open(filename));
> // 从1.39版本开始，废除了上面的写法，使用下面的写法
> let mut file = std::fs::File::open(filename)?;
> ```

panic 失败时不做处理的简写：

```rust
// unwrap 只需要Ok的部分，如果不是则会触发恐慌
// expect 使用错误信息替换 Err 的错误信息
let file = File::open(&"hello.txt").unwrap();
```

错误的传播：返回 Result 枚举类型的函数或方法。

```rust
use std::fs::File;
use std::io::{Read, Result};

fn main() {
    let result = read_file();
    match result {
        Ok(str) => {
            println!("{}", str)
        }
        Err(error) => {
            panic!("{:?}", error)
        }
    }
}

// 使用result作为返回值，交给调用位置处理
fn read_file() -> Result<String> {
    let file_path = "a.txt";
    let mut contents = String::new();
    // 使用 `?` 表达式来处理可能出现的错误
    // 不同的错误类型实现转换 std::convert::From
    File::open(file_path)?.read_to_string(&mut contents)?;
    Ok(contents)
}
```

> [!NOTE]
> 
> 不建议在 main 函数中返回错误。
> 
> ```rust
> use std::error::Error;
> use std::fs::File;
> use std::io::Read;
> 
> // dyn是动态类型标识符，只要实现了这个Error的都可以返回
> fn main() -> Result<(), Box<dyn Error>> {
>        let file_path = "hello.txt".to_string();
>        let mut file = File::open(&file_path)?;
>        let mut str = String::new();
>        file.read_to_string(&mut str)?;
>        println!("{}", str);
>        Ok(())
> }
> ```

### 何时处理错误

如果能使用`Result`就尽量使用，比如发生了可以预期的状况下，将问题交给调用者处理。除非万不得已，必须立即终止程序，以避免对数据安全或者程序环境造成不可恢复的破坏时使用`panic!`，并且在使用`panic!`之前最好在日志中详细记录。

## 泛型

泛型（Generics）是编程语言中一种强大的特征，它允许编写可以处理多种数据类型的代码，而无需为每种类型单独编写实现。

### 简化重复代码

```rust
fn main() {
    let num = vec![34, 54, 56, 81, 15, 65, 3, 2];
    println!("{:?}中最大的是：{}", &num, largest_i32(&num));//81

    let char = vec!['w', 'o', 'r', 'l', 'd'];
    println!("{:?}中最大的是：{}", &char, largest_char(&char));//w
}

fn largest_i32(list: &[i32]) -> &i32 {
    let mut result = &list[0];

    for item in list {
        if item > result {
            result = item;
        }
    }
    result
}

fn largest_char(list: &[char]) -> &char {
    let mut result = &list[0];

    for item in list {
        if item > result {
            result = item;
        }
    }
    result
}
```

这两个函数的代码完全相同，为了适应`i32`和`char`类型，写了两遍，为了统一，使用泛型。

```rust
fn main() {
    let num = vec![34, 54, 56, 81, 15, 65, 3, 2];
    println!("{:?}中最大的是：{}", &num, largest(&num));

    let char = vec!['w', 'o', 'r', 'l', 'd'];
    println!("{:?}中最大的是：{}", &char, largest(&char));
}

// T是泛型的占位符，编译时会被编译器推导而替换
fn largest<T: PartialOrd>(list: &[T]) -> &T {
    let mut result = &list[0];

    for item in list {
        // 使用泛型后，需要实现这个比较函数
        // 使用PartialOrd实现，让泛型T具备比较功能
        if item > result {
            result = item;
        }
    }
    result
}
```

### 结构体中使用泛型

```rust
fn main() {
    let p1 = Point { x: 1, y: 5 };
    println!("{:?}", &p1);
    let p2 = Point { x: 'a', y: 'b' };
    println!("{:?}", &p2);
    let p3 = p1.mixup(p2);
    println!("{:?}", &p3);
}

#[derive(Debug)]
struct Point<T, U> {
    x: T,
    y: U,
}

impl<T, U> Point<T, U> {
    // 将self的T与方法传入的other的W组合成新的Point
    fn mixup<V, W>(self, other: Point<V, W>) -> Point<T, W> {
        Point {
            x: self.x,
            y: other.y,
        }
    }
}
```

`Option`和`Result`使用了泛型封装运行后的结果和返回值。

## Trait

Trait 是一种为数据类型定义共有的行为的特征。

### 为结构体实现特征

```rust
use std::fmt;
// ...
struct User {
    name: String,
    age: u8,
}

// 定义特征使用trait
pub trait Json {
    // 特征的方法不需要实现，交给实现的实例去做
    fn to_json(&self) -> String;
}

// 为结构体实现特征
impl Json for User {
    fn to_json(&self) -> String {
        format!(
            // 原始字符串，让特殊含义字符失效，成为普通字符
            // #的数量前后必须一致
            // { }会从中插入字符，为了失效，成为字符，使用{{ }}
            r#"{{"name":"{}", "age":{}}}"#,
            self.name.replace("\"", "\\\""),
            self.age
        )
    }
}

// 为结构体实现特征
impl fmt::Display for User {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        write!(f, r#"User{{name: "{}", age: {}}}"#, self.name, self.age)
    }
}
```

### 特征作为函数的参数

```rust
use std::fmt::{Display, Formatter, Result};

fn main() {
    let user = User {
        name: "张三".to_string(),
        age: 10,
    };
    send_json4(&user);
}

// 使用特征作为参数的函数
pub fn send_json1(j: &impl Json) {
    println!("{}", j.to_json());
}

// 使用泛型限定特征作为参数的函数
pub fn send_json2<T: Json>(j: &T) {
    println!("{}", j.to_json());
}

// 使用+添加多个特征限定
pub fn send_json3<T: Json + Display>(t: &T) {
    send_json1(t);
    send_json2(t);
}

// 使用where替代+的形式，简化参数列表信息
pub fn send_json4<T>(t: &T)
where
    T: Json + Display,
{
    send_json3(t);
}

struct User {
    name: String,
    age: u8,
}

pub trait Json {
    fn to_json(&self) -> String;
}

impl Json for User {
    fn to_json(&self) -> String {
        format!(
            r#"{{"name":"{}", "age":{}}}"#,
            self.name.replace("\"", "\\\""),
            self.age
        )
    }
}

impl Display for User {
    fn fmt(&self, f: &mut Formatter<'_>) -> Result {
        write!(f, r#"User{{name: "{}", age: {}}}"#, self.name, self.age)
    }
}
```

### 特征的默认实现

特征的函数默认是不用写实现，如果实现了函数的过程，实现了特征的结构体可以直接使用这个函数过程，或者重写这个函数过程。

```rust
fn main() {
    let user = User {
        name: "张三".to_string(),
        age: 10,
    };
    user.print();
}

struct User {
    name: String,
    age: u8,
}

pub trait Json {
    // 默认方法，实例可以重写这个方法，或者直接用
    fn print(&self) {
        todo!("未实现方法");
    }
}

impl Json for User {
    fn print(&self) {
        println!(
            "{}",
            format!(r#"User{{name: "{}", age: {}}}"#, self.name, self.age)
        );
    }
}
```

### 通过特征改变结构体的实现

```rust
trait Animal {
    fn baby_name() -> String;
}

struct Dog;

impl Dog {
    fn baby_name() -> String {
        String::from("Spot")
    }
}

impl Animal for Dog {
    fn baby_name() -> String {
        String::from("puppy")
    }
}

fn main() {
    println!("A baby dog is called a {}", Dog::baby_name());
    // 使用实现Animal特征的baby_name函数
    println!("A baby dog is called a {}", <Dog as Animal>::baby_name());
}
```

### 特征的返回

```rust
fn main() {
    let a = new(true, "张三".to_string(), 10);
    println!("{}", a.to_json())
}

// 实现了Json特征的实例可以返回，且必须唯一确定
pub fn new_user(name: String, age: u8) -> impl Json {
    User { name, age }
}

// 如果返回的Json特征的实例有多种可能，使用动态封装
pub fn new(is_user: bool, name: String, age: u8) -> Box<dyn Json> {
    if is_user {
        Box::new(new_user(name, age))
    } else {
        Box::new(Person { name, age })
    }
}

struct User {
    name: String,
    age: u8,
}

pub trait Json {
    fn to_json(&self) -> String;
}

impl Json for User {
    fn to_json(&self) -> String {
        format!(
            r#"{{"name":"{}", "age":{}}}"#,
            self.name.replace("\"", "\\\""),
            self.age
        )
    }
}

struct Person {
    name: String,
    age: u8,
}

impl Json for Person {
    fn to_json(&self) -> String {
        format!(
            r#"{{"name":"{}", "age":{}}}"#,
            self.name.replace("\"", "\\\""),
            self.age
        )
    }
}
```

## type

`type` 关键字用于在当前作用域中引入一个新的名称，该名称是现有类型的别名。这不会创建新的类型，只是给现有类型一个更短或更具描述性的名称。

```rust
// 从现在开始，这些类型就换了个名称
type Int = i32;
type Double = f64;
type LongLongLong = i128;

fn main() {
    // 使用新的名称
    println!("{}", Int::MAX);
    println!("{}", Double::EPSILON);
    println!("{}", LongLongLong::MAX);
}
```

## 关联类型

关联类型是 Trait 中的一部分，它允许 Trait 定义一个类型名称，但不立即指定具体的类型。这个类型的具体值由实现该 Trait 的类型来指定。

```rust
trait MyTrait {
    // 定义一个占位符类型
    type Item;

    // 返回一个与关联类型 T 相关的值
    fn get_item(&self) -> &Self::Item;
}
```

> [!NOTE]
>
> - self：表示调用方法的实例，&self作为方法的第一个参数，这个方法可以使用实例调用。
> - Self：表示调用实例的类型。

使用泛型实现`Point`的加法：

```rust
use std::ops::Add;

struct Point<T>
    where T: Add<Output=T> + Copy
{
    x: T,
    y: T,
}

trait Plus<T> {
    fn add(&self, r: T) -> T;
}


impl<T> Plus<Self> for Point<T>
    where T: Add<Output=T> + Copy {
    fn add(&self, r: Self) -> Self {
        Self {
            x: self.x + r.x,
            y: self.y + r.y,
        }
    }
}

fn main() {
    let p1: Point<i32> = Point { x: 1, y: 2 };
    let p2: Point<i32> = Point { x: 3, y: 4 };

    let p3 = p1.add(p2);

    println!("({},{})", p3.x, p3.y);
}
```

使用关联类型实现`Point`的加法：

```rust
use std::ops::Add;

struct Point<T>
    where T: Add<Output=T> + Copy
{
    x: T,
    y: T,
}

impl<T> Plus for Point<T>
    where T: Add<Output=T> + Copy
{
    type Item = Point<T>;

    fn add(&self, r: Self::Item) -> Self::Item {
        Self::Item {
            x: self.x + r.x,
            y: self.y + r.y,
        }
    }
}

trait Plus {
    type Item;
    fn add(&self, r: Self::Item) -> Self::Item;
}

fn main() {
    let p1: Point<i32> = Point { x: 1, y: 2 };
    let p2: Point<i32> = Point { x: 3, y: 4 };

    let p3 = p1.add(p2);

    println!("({},{})", p3.x, p3.y);
}
```

使用泛型，编译时就确定了实现的数据类型，多用于解决代码复用的问题，允许你编写不依赖于任何特定类型的代码。使用关联类型，允许实现选择最合适的数据类型，更专注的为这个数据类型进行实现。

### 运算符重载

重载运算符就是关联类型针对具体的某个类型实现特定功能的例子。

为 Point 结构体实现 + 运算功能：

```rust
use std::ops::Add;

struct Point<T>
    where T: Add<Output=T> + Copy
{
    x: T,
    y: T,
}

impl<T> Add for Point<T> where T: Add<Output=T> + Copy {
    type Output = Self;

    fn add(self, rhs: Self) -> Self::Output {
        Self {
            x: self.x + rhs.x,
            y: self.y + rhs.y,
        }
    }
}

fn main() {
    let p1: Point<i32> = Point { x: 1, y: 2 };
    let p2: Point<i32> = Point { x: 3, y: 4 };

    // 使用add方法相加，这里重载了 + 运算符
    let p3 = p1 + p2;

    println!("({},{})", p3.x, p3.y);
}
```

## 生命周期

Rust 的**引用类型**存在生命周期，表示引用所指向的值的存活时间。

### 生命周期注释

生命周期注释用于帮助编译器，理解数据在程序中的存活时间，从而确保引用在它们引用的数据存在时有效，避免悬垂引用和多重释放。

> [!NOTE]
>
> - 悬垂引用：使用引用的变量还存活，引用地址的数据已经不存在或被重新分配给其他变量使用。
>
> ```rust
> use std::error::Error;
> use std::ffi::{c_char, CStr};
> 
> fn main() -> Result<(), Box<dyn Error>> {
>     let r: *const u8;
>     {
>         let x: String = "hello world".to_string();
>         // 悬垂引用：引用r还在，值被x释放了
>         r = x.as_ptr();
>     }
>     for _ in 1..=10 {
>         let _str = "123456789".to_string();
>         // 输出的时候，会发现引用的数据已经被释放，并且被重新分配，覆盖了原有数据
>         println!("{}", unsafe {
>             CStr::from_ptr(r as *const c_char).to_str()?
>         });
>     }
>     Ok(())
> }
> ```
>
> - 多重释放：同一个地址有多个引用，一个引用释放了内存后，另一个引用再次释放时发生，这时这个地址的数据可能不存在或者被重新分配。

```rust
// 'a 是一个生命周期注释，返回值的生命周期是 x 或 y 的生命周期
// 可以和泛型在同一个<>内定义
fn max<'a>(x: &'a i32, y: &'a i32) -> &'a i32 {
    if x > y {
        x
    } else {
        y
    }
}

fn main() {
    let a = 5;
    let b = 10;
    let result = max(&a, &b);
    println!("The maximum is: {}", result);
}
```

函数或方法的参数的生命周期为：输入生命周期。

函数的返回值的生命周期为：输出生命周期。

### 默认省略生命周期的规则

1. 编译器为每个引用类型的参数分配一个生命周期注释。
   
   ```rust
   // 有一个参数的函数获得一个生命周期注释
   fn foo<'a>(x: &'a i32) { /* ... */ }
   // 有两个参数的函数获得两个独立的生命周期注释
   fn foo<'a, 'b>(x: &'a i32, y: &'b i32) { /* ... */ }
   // 以此类推
   ```
   
2. 如果只有一个输入生命周期注释，那么这个生命周期会被分配给所有的输出生命周期注释。
   
   ```rust
   // 第1条的第一种情况
   fn foo<'a>(x: &'a i32) -> &'a i32 { x }
   ```
   
3. 如果有多个输入生命周期注释，但其中一个是 `&self` 或 `&mut self`（这是一个方法），那么 `self` 的生命周期会被分配给所有的输出生命周期注释。
   
   ```rust
   struct Example {
       value: String,
   }
   
   impl Example {
       // 输出生命周期注释与 self 默认保持一致
       fn set<'b, 'c>(&mut self, x: &'b String, y: &'c String) -> &String {
           self.value.push_str(x);
           self.value.push_str(y);
           &self.value
           // 如果返回 x 或 y 的引用时，返回值的生命周期注释与 self 不一致，需要显示指定
       }
   }
   
   fn main() {
       let mut e = Example { value: "1".to_string() };
       {
           let a = "2".to_string();
           let _ = e.set(&a, &"3".to_string());
       }
       println!("{}", e.value);
   }
   ```
   
   不满足上述三条规则，就需要显示指定生命周期。

### 静态生命周期

`'static` 生命周期表示整个程序执行期间都存在的生命周期。这通常用于描述那些在程序整个生命周期中都有效的引用，例如全局变量、静态变量、字符串字面量，以及在编译时分配的内存的成员。

```rust
static STATIC_ARRAY: [i32; 3] = [1, 2, 3];

fn get_static_array_slice() -> &'static [i32] {
    &STATIC_ARRAY
}

fn main() {
    let slice = get_static_array_slice();
    println!("{:?}", slice);
}
```

## 自动化调试

Rust的标准测试框架`libtest`提供了编写和运行这些测试的基础设施。

### 单元测试

```rust
// src/lib.rs
pub fn add(a: i32, b: i32) -> i32 {
    a + b
}

// tests/lib_test.rs
// 标注
// 单元测试，只有test命令时会编译运行
#[cfg(test)]
mod tests {
    use crate::*; // lib.rs中的所有代码

    // 使用标注，将函数变成测试函数
    #[test]
    fn test1() {
        // 期待值和测试值相等时通过
        assert_eq!(add(2, 2), 5, "左边是4，右边是5，不相等");
    }

    #[test]
    fn test2() {
        // 期待值和测试值不相等时通过
        assert_ne!(4, add(2, 2), "左边是4，右边是4，相等");
    }

    #[test]
    fn test3() {
        // 为true时通过
        assert!((2 + 2) == 4, "2+2==5，错误");
    }

    // 发生恐慌则通过测试
    #[should_panic(expected = "捕获指定的异常信息")]
    #[test]
    fn test_panic() {
        panic!("自定义的错误信息");
    }

    // 忽略这个测试
    #[ignore]
    #[test]
    fn test_ignore() {}

    // 使用Result封装结果
    #[test]
    fn it_works() -> Result<(), String> {
        if 2 + 2 == 5 {
            Ok(())
        } else {
            // 输出错误信息
            Err(String::from("自定义的错误信息"))
        }
    }
}
```

测试命令行：

```cmd
# 运行测试，默认并行运行所有测试捕获所有输出
cargo test
# 运行包含add的所有测试测试，如果是模块名则是全部模块
cargo test add
# 不同的测试命令帮助
cargo test --help
cargo test -- --help

# 解除并行测试
-- --test-threads=1
# 显示println!的输出
-- --show-output
# 测试被忽略的测试
-- --ignored
```

对于测试私有函数和方法，建议单独用公开的函数或方法封装后用于测试调用的入口。

对于并行运行测试，所有测试用例之间不能相互依赖，共享状态。

### 测试文档示例

```rust
// src/lib.rs

/// 这是一个使用`std::Vec<i32>`的函数示例。  
///  
/// 它接受一个整数向量，并返回向量中所有偶数的和。  
///  
/// # 参数  
///  
/// - `numbers`: 包含整数的向量。  
///  
/// # 返回值  
///  
/// 返回一个`i32`类型的值，表示向量中所有偶数的和。  
///  
/// # 示例  
///  
/// ```rust  
/// let numbers = vec![1, 2, 3, 4, 5, 6];  
/// let sum_of_evens = sum_even_numbers(numbers);  
/// assert_eq!(sum_of_evens, 12);  
/// ```  
pub fn sum_even_numbers(numbers: Vec<i32>) -> i32 {
    let mut sum = 0;
    for &num in &numbers {
        if num % 2 == 0 {
            sum += num;
        }
    }
    sum
}
```

### 集成测试

在`lib\lib.rs`的模块中创建`lib\tests`目录，将所有测试用的源文件放入其中。

只能在Library Crate中使用集成测试。

可以在`tests`目录中使用模块系统。

```rust
use my_project::add;

#[test]
fn test() {
    assert_eq!(add(2, 2), 5, "左边是4，右边是5，不相等");
}
```

## 函数式编程

### 闭包

可以捕获所在环境成员的匿名函数。

```rust
fn main() {
    // 从函数到闭包
    fn add_1_v1(n: i32) -> i32 {
        n + 1
    }
    let add_1_v2 = |n: i32| -> i32 { n + 1 };
    let add_1_v3 = |n| { n + 1 };
    // 闭包不要求标注类型，编译器可以推导
    let add_1_v4 = |n| n + 1;
    println!("{}", add_1_v4(5));
    // 一旦确定了闭包的类型，不能改变
    // println!("{}", add_1_v4(5.0));
}
```

闭包和函数的区别：

```rust
fn main() {
    let a = 1;
    // 函数不能捕获环境中的成员，闭包可以捕获环境中的变量a
    let add_1_v4 = |n| n + a;

    println!("{}", add_1_v4(5));
}
```

#### Fn

闭包可以捕获其环境变量的不可变引用。

```rust
fn main() {
    let s = "hello".to_string();
    let mut _s = s.clone();
    let f = || {
        println!("{} world", s);
        // 因为闭包的特征，_s无法进行修改
        // _s.push_str(" world");
        // 因为闭包的特征，_s被当作不可变引用而捕获
        println!("{} world", _s);
    };

    _fn(f);
    // s和_s的所有权没有被闭包转移
    println!("{}", s);
    println!("{}", _s);
}

fn _fn<F: Fn()>(f: F) {
    f();
}
```

#### FnMut

闭包可以捕获其环境变量的可变引用。

```rust
fn main() {
    let s = "hello".to_string();
    let mut _s = s.clone();
    let f = || {
        println!("{} world", s);
        // 因为闭包的特征，_s被捕获，并且可以修改
        _s.push_str(" world");
        println!("{} world", _s);
    };

    _fn_mut(f);
    // s和_s的所有权没有被闭包转移
    println!("{}", s);
    println!("{}", _s);// _s被闭包修改
}

fn _fn_mut<F: FnMut()>(mut f: F) {
    f();
}
```

#### FnOnce

闭包只能调用一次。

```rust
fn main() {
    let s = "hello".to_string();
    let mut _s = s.clone();
    let f = || {
        println!("{} world", s);
        _s.push_str(" world");
        println!("{} world", _s);
    };

    _fn_once(f);
    // 因为闭包的特征，只能执行一次
    // _fn_once(f);
    // s和_s的所有权没有被闭包转移
    println!("{}", s);
    println!("{}", _s); // _s被闭包修改
}

fn _fn_once<F: FnOnce()>(f: F) {
    f();
}
```

> [!NOTE]
> 
> 特征继承关系：`FnOnce -> FnMut -> Fn`

### 迭代器

```rust
fn main() {
    let v = vec![1, 2, 3, 4, 5];
    // 用于遍历v的迭代器
    let mut it = v.iter();
    for &num in it {
        println!("{}", num);
    }

    for num in v.iter() {
        println!("{}", num);
    }

    for num in v {
        println!("{}", num);
    }
}
```

#### 只读可重入迭代器

`iter`：获取元素的不可变引用，不会消耗集合本身。

```rust
fn main() {
    let v = vec![1, 2, 3, 4, 5];
    // 在不可变的集合上创建迭代器，获取集合中元素的引用
    let mut it = v.iter();

    loop {
        let num: &i32 = match it.next() {
            Some(num) => num,
            None => break,
        };
        println!("{}", num);
    }
    // [1, 2, 3, 4, 5]
    println!("{:?}", v);
}
```

#### 可写可重入迭代器

`iter_mut`：获取元素的可变引用，不会消耗集合本身。

```rust
fn main() {
    let mut v = vec![1, 2, 3, 4, 5];
    //  在可变的集合上创建迭代器，获取集合中元素的可变引用
    let mut it = v.iter_mut();

    loop {
        // 修改了原集合的值，但是没有消耗原集合的元素
        match it.next() {
            Some(num) => *num += 1,
            None => break,
        };
    }
    // [2, 3, 4, 5, 6]
    println!("{:?}", v);
}
```

#### 只读不可重入迭代器

`into_iter`：集合中的元素的所有权转移到迭代器。

```rust
fn main() {
    let v = vec![1, 2, 3, 4, 5];
    // 创建获取集合中元素的所有权的迭代器，消耗集合中的元素
    let mut it = v.into_iter();

    loop {
        let num = match it.next() {
            Some(num) => num,
            None => break,
        };
        println!("{}", num);
    }
    // v已经不再可用，已经被into_iter()方法消耗了
    // println!("{:?}", v);
}
```

#### 自定义迭代器

实现`Iterator`特征的`next`方法，创建了一个整数的有限迭代器。

```rust
use std::ops::AddAssign;

fn main() {
    // 生成(1..100)，步长为2的整数迭代器
    let v = IntRange::new(1, 100, 2);
    for num in v {
        // 调用next方法，耗尽这个迭代器
        print!("{} ", num);
    }
}

// 整数迭代器
struct IntRange<T> {
    star: T, // 起始值
    end: T,  // 结束值
    step: T, // 步长
}

impl<T> IntRange<T> {
    pub fn new(star: T, end: T, step: T) -> Self {
        Self { star, end, step }
    }
}

impl<T> Iterator for IntRange<T>
where
    T: AddAssign + Copy + PartialOrd,
{
    type Item = T;

    fn next(&mut self) -> Option<Self::Item> {
        // PartialOrd 特征
        if self.star < self.end {
            let star = self.star; // Copy 特征
            self.star += self.step; // AddAssign 特征
            return Some(star);
        }
        return None;
    }
}
```

#### 使用迭代器处理集合中的元素

有限元素集合

```rust
// 使用 Range 的字面量，创建一个从1到100，步长为1的有限元素迭代器
// collect：将这个迭代器执行，生成一个集合
let v: Vec<i32> = (1..100).collect();
print!("{:?} ", v);
```

无限元素集合转有限元素集合

```rust
// 使用 RangeFrom 的字面量，创建一个从1开始，步长为1的无限元素迭代器
// take：限制迭代器只收集前100个元素
// collect必须是有限的集合
let v: Vec<i32> = (1..).take(100).collect();
print!("{:?} ", v);
```

有限元素集合无限循环

```rust
// cycle：将有限的集合首尾相连，循环
// [..., 97, 98, 99, 100, 1, 2, 3, ...]
let v: i32 = (1..=100).cycle().take(500).sum();
println!("{:?}", v); //25250
```

终端操作：消耗迭代器，并在执行时处理迭代器中的所有元素，这时迭代器就不能再次使用了。

- `collect`: 将迭代器中的元素收集到一个集合中。

- `count`: 计算迭代器中元素的数量。

- `sum`: 对迭代器中的元素进行求和。对于整数和浮点数迭代器，`sum`会返回总和；对于自定义类型，需要实现`Add` trait中的`+`运算符。

- `product`: 对迭代器中的元素进行求积。

- `min`/`max`: 返回迭代器中的最小值或最大值。这些操作需要元素类型实现`Ord` trait。

- `min_by`/`max_by`: 使用给定的比较函数返回迭代器中的最小或最大元素。

- `min_by_key`/`max_by_key`: 根据键函数返回的最小或最大键对应的元素。

- `all`: 检查迭代器中的所有元素是否都满足某个条件。

- `any`: 检查迭代器中是否至少有一个元素满足某个条件。

- `find`: 返回迭代器中第一个满足条件的元素的可选值（`Option<T>`）。

- `find_map`: 对迭代器中的每个元素应用一个函数，并返回第一个非`None`的结果。

- `last`: 返回迭代器中最后一个元素的可选值。

- `nth`: 返回迭代器中索引为`n`的元素的可选值。

- `for_each`: 对迭代器中的每个元素执行某个操作，但不返回任何值（类似于`for`循环）。

- `fold`: 使用提供的初始值和二元操作符将迭代器中的所有元素累积成一个值。`fold`非常通用，可以实现很多其他迭代器方法的功能。

- `reduce`: 类似于`fold`，但不需要初始值，它使用迭代器中的第一个元素作为初始值（如果迭代器为空，则返回`None`）。

- `join`: 将迭代器中的字符串元素连接成一个单独的字符串。需要元素类型实现`Display` trait。

- `take`: 返回一个包含迭代器前`n`个元素的新迭代器。

- `take_while`: 返回一个迭代器，该迭代器在条件为真时产生元素，然后立即停止。

#### 常见的惰性迭代器

`skip`：

```rust
fn main() {
    // 跳过前10个元素
    let v: Vec<i32> = (1..=100).skip(10).collect();
    print!("{:?} ", v);
}
```

`rev`：

迭代器都是惰性执行，只在需要时才会执行，从而保证只遍历一次即可消费了所有元素，得到最终的结果。

即使使用 take 方法限制了无限迭代器的元素个数，但是 rev 方法也是惰性的，无法得知无限迭代器具体的元素个数。

```rust
fn main() {
    // (1..=100)是Range类型，表示从1到100，步长为1
    let v: Vec<i32> = (1..=100).rev().collect();
    print!("{:?} ", v);
}
```

`map`：

使用 map ，将迭代器的元素映射到闭包中，处理后将元素放入新的迭代器。

```rust
fn main() {
    let v: Vec<_> = (0..100).map(|x| x + 1).collect();
    println!("{:?}", v);
}
```

`filter`：

使用 filter ，将迭代器的元素传入闭包中，通过闭包的返回值判断元素是否保留。

```rust
fn main() {
    let v: Vec<_> = (1..=100)
        .filter(|n: &i32| {
            if *n < 2 {
                return false;
            }
            for i in 2..=(*n as f32).sqrt() as i32 {
                // 输出的是1到100的质数，只能被1和自己整除的数
                if n % i == 0 {
                    return false;
                }
            }
            return true;
        })
        .collect();
    println!("{:?}", v);
}
```

`enumerate`：

```rust
fn main() {
    let v: Vec<(usize, i32)> = (1..=100)
        .filter(|n: &i32| {
            if *n < 2 {
                return false;
            }
            for i in 2..=(*n as f32).sqrt() as i32 {
                if n % i == 0 {
                    return false;
                }
            }
            return true;
        })
        .enumerate()
        .collect();
    // [(0, 2), (1, 3), (2, 5), (3, 7),... (23, 89), (24, 97)]
    println!("{:?}", v);
}
```

enumerate 可以为每一个元素与从0开始的索引组合成元组序列，这样可以得知元素的当前索引，避免外部额外创建一个计数器。同时也方便根据索引进行切片。

`zip`：

使用 zip ，将两个迭代器的元素依次组合成一个元组的迭代器，迭代器的元组个数以最短的迭代器为准。

```rust
use std::iter;

fn main() {
    let vec1 = vec![1, 2, 3];
    let vec2 = vec!['a', 'b', 'c'];

    // 使用 zip 将两个向量的元素组合成元组
    let zipped: Vec<_> = iter::zip(vec1.iter(), vec2.iter()).collect();

    // 打印结果
    for (i, (a, b)) in zipped.iter().enumerate() {
        println!("Index {}: ({}, {})", i, a, b);
    }

    // 如果需要可变访问，可以直接迭代可变的引用
    let mut vec1_mut = vec![1, 2, 3];
    let mut vec2_mut = vec!['a', 'b', 'c'];

    for (a, b) in iter::zip(vec1_mut.iter_mut(), vec2_mut.iter_mut()) {
        *a *= 2; // 乘以2
        *b = (*b as u8).to_ascii_uppercase() as char; // 转换为大写
    }

    println!("vec1_mut: {:?}", vec1_mut); // 输出: [2, 4, 6]
    println!("vec2_mut: {:?}", vec2_mut); // 输出: ['A', 'B', 'C']
}
```

`fold`：

使用 fold ，将迭代器中的元素折叠，存放到指定的容器中。

```rust
use std::collections::HashMap;

fn main() {
    let v = (1..=100)
        .filter(|n: &i32| {
            if *n < 2 {
                return false;
            }
            for i in 2..=(*n as f32).sqrt() as i32 {
                if n % i == 0 {
                    return false;
                }
            }
            return true;
        })
        .enumerate()
        .fold(HashMap::new(), |mut m, (k, v)| {
            m.insert(k + 1, v);
            m
        });
    // {14: 43, 6: 13, 7: 17, 8: 19,... 3: 5, 19: 67}
    println!("{:?}", v);
}
```

## 智能指针

智能指针是一类特殊的数据结构，它们的行为类似于传统指针，但提供了额外的功能和安全性。 Rust 中的智能指针通常用于解决如生命周期管理、内存安全、所有权等问题。

### Box

`Box` 是一个智能指针，允许在堆上动态地分配内存，并管理这块内存的生命周期。

由于 Rust 的所有值都有固定的生命周期，并且它们通常存储在栈上（对于较小的值）或静态存储区（对于常量）。

`Box` 提供了一种在堆上存储值的方法，这样就可以拥有更大的灵活性，特别是当需要存储的数据大小在编译时未知或可能很大的时候。

> [!NOTE]
>
> 使用场景：
>
> 1. **当需要动态大小的数据时**：例如，有一个可变长度的数组（在Rust中称为切片或`Vec`）并且想把它作为一个字段存储在一个结构体中时，可以使用`Box<[T]>`来避免在栈上分配可能的大量内存。
> 2. **当需要递归数据结构时**：例如，链表或树节点通常包含指向其他节点的指针。使用`Box`可以确保这些节点在堆上分配，从而避免栈溢出。
> 3. **当想要抽象出值的所有权时**：`Box` 允许隐藏值的实际存储位置（栈或堆），从而只关注其接口或行为。
>
> Box 只能在单线程中使用。

链式结构：

```rust
use std::fmt::{Display, Formatter, Result};

// Node节点
struct Node<T>
    where T: Display
{
    value: T,
    next: Option<Box<Node<T>>>,
}


// 使用堆来模拟栈，这个是栈顶的元素
struct Stack<T>
    where T: Display
{
    top: Option<Box<Node<T>>>,
}

impl<T> Display for Stack<T>
    where T: Display
{
    fn fmt(&self, f: &mut Formatter<'_>) -> Result {
        let mut nodes = self.top.as_ref();
        let mut output = String::from("Stack(");

        while let Some(node) = nodes {
            output.push_str(&format!("{} ", node.value));
            nodes = node.next.as_ref();

            if nodes.is_some() {
                output.push_str(", ");
            }
        }

        output.push_str(")");
        write!(f, "{}", output)
    }
}

impl<T> Stack<T>
    where T: Display
{
    pub fn new() -> Self {
        Self { top: None }
    }

    pub fn push(&mut self, value: T) {
        let new_node = Box::new(Node {
            value,
            next: self.top.take(), // 将当前栈顶设置为新节点的下一个节点
        });
        self.top = Some(new_node); // 设置新节点为栈顶
    }

    pub fn pop(&mut self) -> Option<T> {
        self.top.take().map(|node| {
            let value = node.value;
            self.top = node.next; // 将下一个节点设置为新的栈顶
            value
        })
    }
}

fn main() {
    let mut stack = Stack::new();
    // 在堆上模拟栈，可以避免栈溢出
    // (exit code: 0xc00000fd, STATUS_STACK_OVERFLOW)
    for n in 1..=1_0000 {
        stack.push(n);
    }
    println!("{}", stack);
}
```

### Deref 特征

解引用运算符的实现 Trait。

```rust
use std::ops::Deref;

struct Node<T> {
    value: T,
}

impl<T> Deref for Node<T> {
    type Target = T;

    fn deref(&self) -> &Self::Target {
        &self.value
    }
}

fn main() {
    let n = Node { value: "n1".to_string() };

    // 实现Deref特征后，获取Node节点的值
    println!("n={}", *n);
}
```

### 隐式解引用转换

```rust
use std::ops::Deref;

struct Node<T> {
    value: T,
}

impl<T> Deref for Node<T> {
    type Target = T;

    fn deref(&self) -> &Self::Target {
        &self.value
    }
}

fn main() {
    let n = Node { value: "hello".to_string() };

    // 自动解引用链，编译时完成
    // &r -> &Node<String> -> &String -> &str
    hello(&n);

    // 如果Node没有实现Deref特征，只能这么转换后才能传入
    hello(&(&n.value)[..])
}

fn hello(s: &str) {
    println!("{}", s);
}
```

> [!IMPORTANT]
>
> 解引用和可变性：
>
> - 当`T::Deref<Target=U>`，允许从`&T`转换为`&U`。（不可变转不可变）
> - 当`T::DerefMut<Target=U>`，允许从`&mut T`转换为`&mut U`。（可变转可变）
> - 当`T::Deref<Target=U>`，允许从`&mut T`转换为`&U`。（可变转不可变）
>
> 特征继承关系：`Deref -> DerefMut`：
>
> ```rust
> use std::ops::{Deref, DerefMut};
> 
> struct Node<T> {
>     value: T,
> }
> 
> // 可变引用，必须先实现 Deref 的特征
> impl<T> DerefMut for Node<T> {
>     fn deref_mut(&mut self) -> &mut Self::Target {
>         &mut self.value
>     }
> }
> 
> // 不可变引用
> impl<T> Deref for Node<T> {
>     type Target = T;
> 
>     fn deref(&self) -> &Self::Target {
>         &self.value
>     }
> }
> ```

### Drop 特征

用于定义类型在离开作用域时需要执行的清理操作，以确保在类型的实例不再需要时能够正确释放或清理与之关联的资源。

```rust
use std::fmt::Display;

struct Node<T>
    where T: Display
{
    value: T,
}

impl<T> Drop for Node<T> where T: Display {
    fn drop(&mut self) {
        println!("触发了drop方法，释放了={}", &self.value);
    }
}

fn main() {
    for _ in 1..=100 {
        println!();
        let n1 = Node { value: "n1".to_string() };
        let n2 = Node { value: "n2".to_string() };
        {
            // 手动调用drop方法，将所有权转移，后续就不能再使用这个变量
            drop(n1);
            let n3 = Node { value: "n3".to_string() };
        }// 当n3离开作用域时，Drop trait的drop方法会被调用
    }// 当n2离开作用域时，Drop trait的drop方法会被调用
    // n1>n3>n2
}
```

### Rc

引用计数指针，记录引用的次数。Box 智能指针无法共享数据的所有权，一旦将堆上的数据存入 Box 实例，并将所有权交给另一个 Box 实例时，原来的 Box 实例就不能使用堆上的数据了。

> [!NOTE]
>
> 使用场景：
>
> - **当需要在多个所有者之间共享一个值时**：Rc 会自动跟踪有多少所有者指向堆上的一个特定的值，并在最后一个所有者被销毁时自动释放该值。
>
> Rc 只能在单线程中使用。

```rust
use std::rc::Rc;

fn main() {
    let rc: Rc<String> = Rc::new("hello".to_string());
    println!("{}", Rc::strong_count(&rc)); //1

    // 引用克隆的两种写法，效果一样
    let a: Rc<String> = rc.clone();
    println!("{}", Rc::strong_count(&rc)); //2

    drop(a);// drop会让引用计数-1

    let _b: Rc<String> = Rc::clone(&rc);
    println!("{}", Rc::strong_count(&rc)); //2
}
```

### RefCell

当需要从 Box 中解引用，以获取对堆上数据的引用时，需要遵守 Rust 的借用规则，不能在拥有可变引用的时刻同时拥有另一个可变引用或多个不可变引用指向堆上的同一个数据。

> [!NOTE]
>
> 使用场景：
>
> - **当需要将借用规则从编译阶段延后到运行时**：这时就需要开发者通过代码逻辑维护借用规则。
>
> RefCell 只能在单线程中使用。

```rust
use std::cell::{Ref, RefCell, RefMut};
use std::rc::Rc;

fn main() {
    let rc = Rc::new(RefCell::new(5));

    // 不可变引用
    let n: Ref<i32> = rc.borrow();
    println!("{}", n); // 5
    drop(n); // 释放不可变引用

    // 可变引用
    let mut _n: RefMut<i32> = rc.borrow_mut();
    *_n = 10;
    drop(_n); // 释放可变引用

    // 再来一个不可变引用，查看值被修改
    let n: Ref<i32> = rc.borrow();
    println!("{}", n); // 10
}
```

获取 Ref 不可变引用时，不可变引用计数+1，释放后不可变引用计数-1；

获取 RefMut 可变引用时，可变引用计数+1，释放后可变引用计数-1；

**可变引用计数或不可变引用计数必须时刻有一个为0，且可变引用计数不能超过1。**

否则违反规则触发 panic （`already borrowed: BorrowMutError`）。

### Cell

> [!NOTE]
>
> 使用场景：
>
> - **当需要在不可变上下文中存储和修改基本数据类型时**：例如，在结构体中存储一个需要在多个方法之间共享的计数器时，可以使用 Cell 来避免将整个结构体标记为可变。
>
> Cell 只能在单线程中使用。

由于 Cell 用于基本数据类型，不涉及借用规则（拷贝），比 RefCell 性能更好，但是功能有限。

```rust
use std::cell::Cell;
use std::rc::Rc;

fn main() {
    let rc = Rc::new(Cell::new(5));

    // 不可变引用
    let n: &Cell<i32> = rc.as_ref();
    println!("{}", n.get()); // 5

    // 因为 Cell 提供了内部可变性，可以直接通过 n 引用修改其值
    n.set(10);

    // 修改值后，打印新的值
    println!("{}", n.get()); // 10

    // 尝试通过 rc 再次获取并打印值
    println!("{}", rc.as_ref().get()); // 10
}
```

### 循环引用

> [!WARNING]
> 
> 这是个内存泄漏的代码，程序进程终止后泄漏的内存才会被系统清理。

```rust
use std::cell::RefCell;
use std::rc::Rc;

fn main() {
    let rc1 = Rc::new(RefCell::new(Node::new(1)));
    let rc2 = Rc::new(RefCell::new(Node::new(2)));

    // 创建循环引用，clone会将强引用计数+1
    rc1.borrow_mut().next = Some(rc2.clone());
    rc2.borrow_mut().next = Some(rc1.clone());

    // 2
    println!("Node1 strong count: {}", Rc::strong_count(&rc1));
    println!("Node2 strong count: {}", Rc::strong_count(&rc2));
} // 1，n1和n2没有执行drop

pub struct Node {
    value: i32,
    next: Option<Rc<RefCell<Node>>>,
}

impl Drop for Node {
    // 强引用计数不为0，不会被调用
    fn drop(&mut self) {
        println!("调用Drop：{}", self.value);
    }
}

impl Node {
    pub fn new(value: i32) -> Self {
        Self { value, next: None }
    }
}
```

### Weak

Weak 为了防止循环引用造成引用计数额外增加，实际没有新的变量接收这个增加的引用，而是由已经有这个引用的变量接收（循环引用），最后这个变量作用域结束后，引用计数没有归零，导致内存泄漏。

Weak 的出现，将引用分为了强引用和弱引用，强引用负责引用计数，实现内存回收；弱引用仅仅是为了给变量来访问堆上的数据，不会影响内存回收的计数（打破循环引用）。

```rust
use std::cell::RefCell;
use std::rc::{Rc, Weak};

fn main() {
    let rc1 = Rc::new(RefCell::new(Node::new(1)));
    let rc2 = Rc::new(RefCell::new(Node::new(2)));

    // 使用downgrade方法转换成Weak
    rc1.borrow_mut().next = Some(Rc::downgrade(&rc2));
    rc2.borrow_mut().next = Some(Rc::downgrade(&rc1));

    // 1
    println!("Node1 strong count: {}", Rc::strong_count(&rc1));
    println!("Node2 strong count: {}", Rc::strong_count(&rc2));
    println!("Node1 weak count: {}", Rc::weak_count(&rc1));
    println!("Node2 weak count: {}", Rc::weak_count(&rc2));

    // Rc和Weak的互转
    let weak_rc2 = Rc::downgrade(&rc2);
    // 尝试从Weak升级回Rc
    if let Some(rc2_upgraded) = weak_rc2.upgrade() {
        // 如果upgrade成功，rc2_upgraded现在是一个Rc<RefCell<Node>>
        // 1
        println!(
            "Upgrade successful, new Rc2 strong count: {}",
            Rc::strong_count(&rc2_upgraded)
        );
    } else {
        println!("Upgrade failed, Rc2 has been dropped");
    }
}

pub struct Node {
    value: i32,
    next: Option<Weak<RefCell<Node>>>, // 改为Weak
}

impl Drop for Node {
    fn drop(&mut self) {
        println!("调用Drop：{}", self.value);
    }
}

impl Node {
    pub fn new(value: i32) -> Self {
        Self { value, next: None }
    }
}
```

## 提高程序的执行效率

### 概念

#### 并发（Concurrency）

多个任务在同一时间段内交替执行，但不一定在同一时刻同时执行。它允许任务通过时间片轮转等方式“看起来”同时执行，从而提高了资源利用率和系统的整体响应性。

就像是多个厨师在同一时间段内交替烹饪不同的菜肴，共享工具和设备。

#### 并行（Parallelism）

并行是**真正的多个任务在同时运行**，这通常发生在多核心或多处理器系统中，每个任务都在自己的CPU核心上独立运行，从而显著提高了系统的整体性能。

就像是多个独立的厨房的场景下（每个代表一个CPU核心），每个厨房都有自己的厨师和设备，同时烹饪不同的菜肴。

#### 进程（Process）

操作系统分配资源的基本单位，它拥有独立的内存空间和系统资源。多个进程之间通过操作系统提供的机制进行通信和同步。

每个进程就像一个独立的餐厅或厨房，有自己的厨师、设备和食材。

#### 线程（Thread）

操作系统调度的基本单位，它共享进程的资源，但拥有独立的执行路径。多个线程可以在同一进程中并发执行，从而提高了程序的响应性和吞吐量。

就像是同一个餐厅（进程）内的不同厨师，他们共享同一个厨房（内存空间）的设备和食材，但每个厨师都在做不同的菜肴（执行不同的代码路径）。

#### 协程（Coroutine）

用户态的轻量级线程，由用户程序或运行库控制。协程的切换不需要操作系统的介入，因此切换成本非常低。它们特别适用于IO密集型任务，可以在等待IO操作完成时切换到其他协程，从而充分利用CPU资源。

就像是灶台上的锅，一个厨师可以同时用多个锅做多道菜（比如网红“过油哥”），每道菜都是同一个厨师的厨艺（代码逻辑）和厨房（内存空间）。当某个菜正在等待时（读取文件、网络请求），师可以切换到另一个锅继续做另一个菜，而不是空等。这种切换是由厨师自己控制，而不是由餐厅经理（系统）来调度。

#### 线程模型

描述操作系统如何管理和调度线程的方式。

- **1:1 模型**：每个用户线程直接对应一个操作系统线程。这种模型简单直接，但可能浪费资源。
- **M:N 模型**：M个用户线程映射到N个操作系统线程。这种模型更加灵活和高效，允许用户线程在系统线程之间迁移，以优化资源利用和性能。然而，这种模型也增加了复杂性。

| 并发机制          | 描述                                               | 内存 | 通信机制                                           | 切换成本 | 适用场景                     |
| :---------------- | :------------------------------------------------- | :--- | :------------------------------------------------- | :------- | :--------------------------- |
| 进程              | 操作系统资源分配的基本单位，独立内存空间           | 独立 | IPC (进程间通信，比如管道、消息队列、信号、套接字) | 高       | 需要高隔离性的任务           |
| 线程              | 操作系统调度的基本单位，共享进程资源               | 共享 | 共享内存                                           | 中       | 需要并发执行且共享内存的任务 |
| 协程              | 用户态轻量级线程，由程序或运行库控制，支持异步执行 | 共享 | 共享内存                                           | 低       | IO密集型任务，如网络编程     |
| 纤程/微线程       | 比协程更轻量级的用户态线程，适用于大量并发场景     | 共享 | 共享内存                                           | 极低     | 需要极高并发的任务           |
| 虚拟线程/绿色线程 | 用户态线程，可映射到多个系统线程，实现跨核并行     | 共享 | 共享内存                                           | 低       | 需要跨多核并发利用的任务     |

### spawn

```rust
use std::thread;
use std::time::Duration;

fn main() {
    let word = |id: usize, count: usize, v_ref: &Vec<i32>| {
        for i in 0..count {
            println!("Thread {}: Work iteration {}: {:?}", id, i + 1, v_ref);
            thread::sleep(Duration::from_secs(1));
        }
        println!("Thread {} finished their work.", id);
    };

    let v = vec![1, 2, 3];
    // 创建子线程
    let _v = v.clone(); // 使用move，将v的克隆实例传入闭包，避免数据竞争
    let thread_handle = thread::spawn(move || {
        word(1, 10, &_v);
    });

    // 主线程也执行工作
    word(0, 5, &v);

    // 等待子线程完成
    thread_handle.join().unwrap();
}
```

move 关键字将闭包捕获的变量的所有权转移到闭包中，避免闭包外对变量的操作影响生命周期或者造成数据竞争。

thread::spawn 会直接创建新的线程，执行传入的闭包，闭包没有参数。

### 消息传递

#### ~~mpsc~~

使用多个生产者和一个消费者的通道传递消息。

```rust
use std::sync::mpsc::{self, Receiver, Sender};
use std::thread;
use std::time::Duration;

fn main() {
    // 创建一个通道
    let (tx, rx): (Sender<String>, Receiver<String>) = mpsc::channel();
    // 复制发送者，管道可以有多个发送者，只能有一个接收者
    let tx_1 = tx.clone();

    // 消费者线程
    let consumer_handle = thread::spawn(move || consumer_1(rx));
    // 观察接收者的等待
    thread::sleep(Duration::from_secs(3));
    // 生产者线程
    let producer_handle = thread::spawn(move || producer(tx_1));

    producer_handle.join().unwrap();
    consumer_handle.join().unwrap();
}

fn producer(tx: Sender<String>) {
    for i in 0..5 {
        // 发送数据到通道
        println!("{:?} 生产者 => {}", thread::current().id(), i);
        // i的所有权已经转移发送出去
        tx.send(i.to_string()).unwrap();
        // 模拟一些工作
        thread::sleep(Duration::from_millis(100));
    }
    // 发送一个特殊值来通知消费者线程停止
    tx.send("生产者：我好了".to_string()).unwrap();
    println!("{:?} 生产者 => 生产者：我好了", thread::current().id());
    println!("{:?} 生产者完成任务", thread::current().id());
}

// 阻塞的方式等待消息
fn consumer_1(rx: Receiver<String>) {
    loop {
        println!("{:?} 接收者 等待...", thread::current().id());
        // 从管道中获取消息
        if let Ok(value) = rx.recv() {
            println!("{:?} 接收者 <= {}", thread::current().id(), value);
            // 接收到特殊值，正常关闭接收，避免无限等待下去
            if "生产者：我好了".eq(&value) {
                break;
            }
        }
        // 等待一下再重试
        thread::sleep(Duration::from_millis(100));
    }
    println!("{:?} 消费者完成任务", thread::current().id());
}

// 不阻塞的方式查看有没有消息
fn consumer_2(rx: Receiver<String>) {
    loop {
        println!("{:?} 接收者 等待...", thread::current().id());
        if let Ok(value) = rx.try_recv() {
            println!("{:?} 接收者 <= {}", thread::current().id(), value);
            // 接收到特殊值，正常关闭接收，避免无限等待下去
            if "生产者：我好了".eq(&value) {
                break;
            }
        } else {
            println!("{:?} 接收者没有收到消息", thread::current().id());
            // 等待一下再重试
            thread::sleep(Duration::from_millis(100));
        }
    }
    println!("{:?} 消费者完成任务", thread::current().id());
}
```

> [!TIP]
> 
> Rust 并没有内置的 channel 类型。
> 
> `std::sync::mpsc`模块已被弃用，推荐使用`crossbeam-channel`或`tokio`。

### 共享内存

#### Mutex

共享内存提供了线程间通信的一种有效方式，但也可能导致数据竞争。

为了解决这个问题，可以使用互斥锁（Mutex）来保护共享内存。

当一个线程想要访问共享内存时，它必须先获得互斥锁。

如果锁已经被其他线程持有，则当前线程将被阻塞在阻塞队列中，直到锁被释放。

当锁被释放时，等待队列中的一个线程将被唤醒并尝试获取锁。

线程也可以选择放弃等待锁，以继续执行各自的后续代码。

```rust
use std::ops::Not;
use std::sync::{Arc, Mutex};
use std::thread;
use std::time::Duration;

fn main() {
    let mut user = Vec::new();
    // 创建了一个网吧，只有一台电脑
    let computers = Arc::new(Mutex::new(false));
    for _ in 1..=10 {
        // 确保Mutex引用的数据的生命周期能走完所有线程的生命周期
        // 需要借助Arc将Mutex引用克隆一份传递给子线程，Rc不支持多线程
        let c = computers.clone();
        let t = thread::spawn(move || {
            using_computers(c);
        });
        user.push(t);
    }
    // 等待所有线程完成
    user.into_iter().for_each(|u| { u.join().unwrap() })
}

fn using_computers(computers: Arc<Mutex<bool>>) {
    // 尝试获取互斥锁，对这台电脑的控制权
    let mut mutex = computers.lock().unwrap();

    if (*mutex).not() {
        *mutex = true;
        println!("用户 {:?} 开始使用电脑", thread::current().id());

        // 模拟用户使用电脑时间
        thread::sleep(Duration::from_secs(1));

        *mutex = false;
        println!("用户 {:?} 从电脑下机", thread::current().id());
    } else {
        println!("电脑已经被占用，请等候");
    }
}
```

#### Arc

Arc 是 Rc 智能指针的原子（Atomic）版本，可以用在多线程的环境中。

Arc 基于引用计数来实现自动内存管理，当引用计数降至零时，它会自动删除所指向的对象。

Arc 通常与 Mutex 或 RwLock 等互斥锁一起使用，以便在多个线程之间安全地访问和修改共享数据。

这是因为 Rust 的借用检查器（borrow checker）不允许在多个线程之间共享可变引用，以防止数据竞争。

通过使用 Arc 和 Mutex，可以在多个线程之间安全地共享和修改数据。

```rust
use std::collections::VecDeque;
use std::sync::{Arc, Mutex};
use std::thread;
use std::time::Duration;

fn main() {
    // 创建一个包含互斥锁的消息队列
    let data = Arc::new(Mutex::new(VecDeque::new()));
    // 复制Arc指针，以便多个线程可以访问相同的Mutex
    // Arc实现了Send特征，支持多线程之间传递数据
    let data_1 = data.clone();

    // 消费者线程
    let consumer_handle = thread::spawn(move || consumer(data_1));
    // 保证消费者准备就绪
    thread::sleep(Duration::from_secs(1));
    // 生产者线程
    let producer_handle = thread::spawn(move || producer(data));

    producer_handle.join().unwrap();
    consumer_handle.join().unwrap();
}

fn producer(data: Arc<Mutex<VecDeque<String>>>) {
    for i in 0..5 {
        // 获取互斥锁，用来修改数据，修改期间其他共享者不能操作这个数据
        let mut queue = data.lock().unwrap();
        println!("发送: 发送的消息={}", i);
        // 发送数据到消息队列中，向后追加
        queue.push_back(i.to_string());
        // 模拟一些工作
        thread::sleep(Duration::from_millis(100));
    }
    // 发送一个特殊值来通知消费者
    data.lock().unwrap().push_back("生产者：我好了".to_string());
    println!("生产者完成任务");
}

fn consumer(data: Arc<Mutex<VecDeque<String>>>) {
    'l: loop {
        println!("接收：等待");
        // 尝试获取互斥锁以查看是否有新消息
        let mut queue = data.lock().unwrap();
        // 消息队列中没有消息，等待一下再重试
        if queue.is_empty() {
            println!("接收: 没有收到消息");
            thread::sleep(Duration::from_millis(100));
            continue;
        }
        // 消息队列中有消息，从开始位置依次取出
        while let Some(value) = queue.pop_front() {
            println!("接收: 接收到消息={}", value);
            // 接收到特殊值，正常关闭接收，避免无限等待下去
            if "生产者：我好了".eq(&value) {
                break 'l;
            }
        }
    }
    println!("消费者完成任务");
}
```

#### 死锁

当两个或更多的线程相互等待对方释放资源时，就会发生死锁。

```rust
use std::sync::{Arc, Mutex};
use std::thread;
use std::time::Duration;

fn main() {
    // 创建了两个Arc<Mutex<i32>>对象，分别代表两个被互斥锁保护的数据资源data_1和data_2
    let data_1 = Arc::new(Mutex::new(1));
    let data_2 = Arc::new(Mutex::new(2));
    // 为这两个资源创建了克隆，以便在子线程中使用它们
    let _data_1 = data_1.clone();
    let _data_2 = data_2.clone();

    // 在thread_1中，首先锁定了data_1，然后尝试在稍后锁定data_2
    let thread_1 = thread::spawn(move || {
        let guard_1 = data_1.lock().unwrap();
        println!("线程1: 持有data_1={}，尝试获取data_2...", guard_1);
        // 模拟耗时操作
        thread::sleep(Duration::from_secs(1));
        // 这里会阻塞，因为data_2被线程2持有
        let guard_2 = data_2.lock().unwrap();
        println!("线程1: 获得了data_2={}", guard_2);

        // 释放锁
        drop(guard_1);
        drop(guard_2);
    });

    // 同时，在thread_2中，首先锁定了data_2，然后尝试在稍后锁定data_1
    let thread_2 = thread::spawn(move || {
        let mut guard_2 = _data_2.lock().unwrap();
        println!("线程2: 持有data_2={}，尝试获取data_1...", guard_2);
        *guard_2 = 3;
        println!("线程2: 修改data_2={}", guard_2);
        // 模拟耗时操作
        thread::sleep(Duration::from_secs(1));
        // 获取guard_2并操作后，及时释放guard_2的锁，避免死锁
        drop(guard_2);
        // 这里会阻塞，因为data_1被线程1持有
        let guard_1 = _data_1.lock().unwrap();
        println!("线程2: 获得了data_1={}", guard_1);
        drop(guard_1);
    });

    // 由于死锁，程序会永远挂起，在这里等待
    thread_1.join().unwrap();
    thread_2.join().unwrap();
}
```

### 线程安全特征

Send：表示一个类型的值可以在线程之间安全地发送。这个类型的值可以被安全地移动到另一个线程中。Arc 在 Rc 智能指针的基础上实现了这个特征。

Sync：表示一个类型的值可以在多线程环境中安全地被多个线程并发访问，即使这些线程同时拥有该值的引用。但是并不意味着类型的实例可以被多个线程同时修改，它仅仅保证并发读取是安全的。Mutex 实现了这个特征，可以被多个线程同时持有，只是内部的数据只能同时有一个线程进行访问。

> [!WARNING]
> 
> 这两个标记特征是不安全的（unsafe），不能保证安全的情况下，不建议手动实现这两个特征，标准库中的大部分基本类型实现了这两个特征，只要保证结构体中的所有字段都是实现了这两个特征，这个结构体会被编译器判定为实现了这两个特征，从而可以在多线程环境中使用。

## Unsafe

Rust 语言的核心设计目标之一是确保内存安全。它通过引入所有权（ownership）、借用检查（borrowing）、生命周期（lifetimes）等概念，在编译时自动检查并防止许多常见的内存错误，如空指针解引用、双重释放、悬挂指针等。

然而，在某些特殊情况下，如进行底层系统编程、与C语言库交互、实现某些高级功能时，Rust 的常规安全机制可能与这些需求产生矛盾。在这些情况下，Rust 提供了一个“后门”，允许程序员编写`unsafe`代码块，从而绕过 Rust 的内存安全检查。

`unsafe`关键字在Rust中是一个强大的工具，但它也伴随着巨大的责任。当使用`unsafe`代码时，实际上是在告诉 Rust 编译器：“我知道这段代码可能违反了你的内存安全规则，但我保证我会小心行事，手动确保它的安全性。”

因此，在编写`unsafe`代码时，必须格外小心。需要确保代码不会引入任何可能导致程序崩溃、数据损坏、其他意外情况的内存错误。这通常需要对 Rust 的内存模型和底层系统编程有深入的理解。

### 原始指针（Raw Pointers）

```rust
fn main() {
    let mut b: Box<Vec<i32>> = Box::new(vec![1, 2, 3]);

    // 创建一个可变的原始指针
    let ptr_m: *mut i32 = b.as_mut_ptr();
    unsafe {
        *ptr_m.offset(1) = 10;
    }
    println!("The value in Box is: {:?}", *b.as_ref());

    // 创建一个不可变的原始指针
    let ptr: *const i32 = b.as_ptr();
    for i in -10..=10 {
        // 尝试越界访问
        print!("{:?} ", unsafe { *ptr.offset(i) });
    }
    println!("\nThe value through the const pointer address is: {:?}", ptr);
}
```

> [!IMPORTANT]
> 
> 原始指针的类型：
> 
> - `*mut T`：可变的原始指针，可以在 unsafe 中解引用后修改值。
> - `*const T`：不可变的原始指针，只能在 unsafe 中读取值。
> 
> 原始指针的特性：
> 
> - 不受借用检查，可以有多个可变和不可变的指针指向同一个内存地址的数据。
> 
> - 可能会指向非法的内存地址。
> 
> - 可以为null（`std::ptr::{null, null_mut}`）。
> 
> - 无法自动清理。

### 联合体

在相同的内存位置存储不同的数据类型，用于不同类型的二进制转换。

常用于将二进制数据反序列化后，方便的解释为不同的数据类型。

```rust
fn main() {
    // 只能使用一个字段初始化
    let u = Convert_64 { f: 1.1 };
    unsafe {
        // 将f64，基于位模式，转换为i64
        let v = u.i;
        // 4607632778762754458
        println!("{}", v);
    }
}

union Convert_64 {
    i: i64,
    f: f64,
}

/*
IEEE-754:
      --- Sign:符号位，1bit
      |      ---Exponent:指数位，11bit
      |      |                           --- Mantissa:有效数，52bit
      ↓      ↓                           ↓
     | |           |                                                    |
1.1=> 0 01111111111 0001100110011001100110011001100110011001100110011010

转换过程：
  bin 0011111111110001100110011001100110011001100110011001100110011010
  hex 3FF1 9999 9999 999A
  dec 4607632778762754458
 */
```

### extern

用于创建 Rust 与其他语言交互的外部函数接口（FFI）。[External blocks](https://doc.rust-lang.org/reference/items/external-blocks.html)

#### 调用系统库

```rust
use std::ffi::{c_char, c_int, CString};

#[link(name = "User32")]
extern "stdcall" {
    fn MessageBoxA(hwnd: c_int, text: *const c_char, title: *const c_char, flags: c_int) -> c_int;
}

fn main() {
    let text = CString::new("Hello").unwrap();
    unsafe {
        // 调用win32 函数
        MessageBoxA(0, text.as_ptr(), text.as_ptr(), 0);
    }
}
```

#### C++ To Rust

```c++
#include <iostream>

// extern "C"：该函数应该使用C语言的链接规范，即不进行名称修饰
// __declspec(dllexport)：MSVC编译器的特有属性，这个函数可以被导出，被其他程序使用
extern "C" __declspec(dllexport) int squ(int num) {
    int result = num * num;
    std::cout << "Result=" << result << std::endl;
    return result;
}
```

将编译后的 dll 和 lib 复制到 Rust 的项目根目录

```rust
use std::ffi::c_int;

// 链接到square模块，需要有square.lib
#[link(name = "square")]
extern {
    fn squ(num: c_int) -> c_int;
}

fn main() {
    unsafe {
        let num = 5;
        let result = squ(num);
        println!("{} ^ 2 = {}", num, result);
    }
}
```

#### Rust To C++

[Linkage - The Rust Reference (rust-lang.org)](https://doc.rust-lang.org/reference/linkage.html)

crate-type 用于指定 crate（包）应该被编译为哪种类型。

1. **`lib`**：默认类型，表示这是一个库 crate。
2. **`bin`**：表示这是一个可执行文件 crate。然而，当你使用 `[[bin]]` 部分时，你通常不需要在 `crate-type` 中指定 `bin`，因为 `[[bin]]` 部分本身就指示 Cargo 编译一个可执行文件。
3. **`rlib`**：Rust 的动态链接库（Rust 动态库）。这实际上与 `lib` 类似，但更明确地表示你正在创建一个 Rust 动态库。
4. **`dylib`**：C 动态链接库。这种类型允许你的 Rust 代码作为一个动态库被 C 或其他语言使用。
5. **`staticlib`**：静态链接库。这种类型的库会包含所有必要的代码和数据，以便其他程序可以静态地链接到它。
6. **`cdylib`**：C 兼容的动态链接库。与 `dylib` 类似，但它使用 C 风格的 ABI，允许 Rust 代码作为动态库被 C 或其他语言使用。
7. **`proc-macro`**：过程宏库。这种类型允许你编写在编译时执行的宏。

```toml
# Cargo.toml
[lib]
crate-type = ["cdylib"]
```

```rust
// no_mangle：确保 Rust 编译器在编译代码时不改变某个符号（通常是函数或变量）的名称
// extern "C"：来指定该函数应该使用 C 语言的调用约定（ABI）
#[no_mangle]
pub extern "C" fn squ(num: i32) -> i32 {
    let result = num * num;
    println!("Result={}", result);
    result
}
```

将生成的 dll 和 lib 复制到 C 语言的项目中，并添加为依赖项。

```cmake
# ...
add_executable(${PROJECT_NAME} main.cpp)
# 添加环境变量，设置路径
set(RUST_DLL_DIR ${CMAKE_SOURCE_DIR}/lib/)
# 将dll文件复制到编译后的可执行文件旁
file(COPY ${RUST_DLL_DIR}/rust_lib.dll DESTINATION ${CMAKE_BINARY_DIR})
# 链接lib文件
target_link_libraries(${PROJECT_NAME} ${RUST_DLL_DIR}/rust_lib.dll.lib)
```

```c++
#include <iostream>
#include <cstdint>

extern "C" {
int32_t squ(int32_t num);
}

int main() {
    int num = 5;
    int result = squ(num);
    std::cout << num << " ^ 2 = " << result << std::endl;
    return 0;
}
```

## 面向对象

Rust 本身不是面向对象的（Object-Oriented, OO）编程语言。

但是在 Rust 中，可以使用结构体（structs）和枚举（enums）来封装数据和相关的函数（方法）。虽然 Rust 没有类（classes），但是可以使用结构体和 impl 块来模拟面向对象编程中的类和对象。

Rust 的 trait 系统提供了一种类似接口（interfaces）的机制，允许定义一组方法，并在多个类型上实现这些方法。这可以实现类似多态的行为，即在不关心具体类型的情况下调用方法。

通过以上方式，可以实现面向对象的风格，但是 Rust 的目标不是成为面向对象语言，而是在内存安全性和性能之间进行平衡。

### 封装

在 Rust 中，封装是通过模块（modules）和结构体（structs）的私有性（privateness）来实现的。

默认情况下，结构体中的字段是私有的，这意味着它们只能在定义它们的模块或结构体内部被访问。

要使字段或方法公开，可以使用`pub`关键字。

```rust
use crate::obj::User;

mod obj {
    pub struct User {
        // 私有字段
        name: String,
        pub age: u8,
    }

    impl User {
        pub fn new(name: String, age: u8) -> Self {
            let user = Self { name, age };
            user.println();
            user
        }
        // 私有方法，只能在这个impl块内调用
        fn println(&self) {
            println!("name:{},age:{}", self.name, self.age);
        }
    }

    impl User {
        pub fn set_name(&mut self, name: String) {
            // 借助self访问私有方法
            self.println();
            self.name = name;
        }
    }
}

fn main() {
    let user = User::new("张三".to_string(), 10);
    // 能直接访问公有字段
    println!("age={}", user.age);
}
```

### 继承

Rust 并没有直接的“继承”机制，而是使用了组合（composition）和特征（traits）来实现类似的功能。

结构体可以包含其他结构体的实例，而特征则定义了一组可以在多种类型上重用的方法。

虽然不能直接从一个结构体“继承”另一个结构体的字段或方法，但是可以通过实现相同的特征来复用代码。

```rust
trait Animal {
    fn speak(&self);
}

struct Dog {
    name: String,
}

impl Dog {
    fn new(name: &str) -> Dog {
        Dog { name: name.to_string() }
    }
}

impl Animal for Dog {
    fn speak(&self) {
        println!("{} says woof!", self.name);
    }
}

// 使用 trait 来模拟继承
fn animal_speak(animal: &dyn Animal) {
    animal.speak();
}

fn main() {
    let my_dog = Dog::new("Rex");
    // 使用动态类型，将对象转为父类的类型
    animal_speak(&my_dog as &dyn Animal);
}
```

### 多态

Rust 通过泛型（generics）和特征（traits）来实现多态。

泛型允许编写可以处理多种类型的代码，而特征则允许定义一组可以在多种类型上重用的方法。

```rust
use std::f64::consts::PI;

// 定义Shape特征，它有一个area方法
// 所有的图形都可以计算面积
trait Shape {
    fn area(&self) -> f64;
}

// 定义Circle结构体
// 圆形
struct Circle {
    // 半径
    radius: f64,
}

// 为Circle实现Shape特征中的area方法
impl Shape for Circle {
    fn area(&self) -> f64 {
        PI * (self.radius * self.radius)
    }
}

// 定义Rectangle结构体
// 四边形
struct Rectangle {
    width: f64,
    height: f64,
}

// 为Rectangle实现Shape特征中的area方法
impl Shape for Rectangle {
    fn area(&self) -> f64 {
        self.width * self.height
    }
}

// 多态
// 定义一个函数，它接受任何实现了Shape特征的对象
fn print_area<T: Shape>(shape: &T) {
    println!("The area is: {}", shape.area());
}

fn main() {
    // 创建一个Circle对象
    let circle = Circle { radius: 5.0 };
    // 调用print_area函数，并传入Circle对象的引用
    print_area(&circle); // 输出: The area is: 78.53981633974483

    // 创建一个Rectangle对象
    let rectangle = Rectangle {
        width: 4.0,
        height: 6.0,
    };
    // 调用print_area函数，并传入Rectangle对象的引用
    print_area(&rectangle); // 输出: The area is: 24
}
```

### 静态分配和动态分配

```rust
// ...

// 静态分配
// 定义一个函数，它接受任何实现了Shape特征的对象
// 这里使用了泛型T，并在函数签名中指定了T必须实现Shape特征
fn print_area<T: Shape>(shape: &T) {
    // 调用shape的area方法时，Rust编译器在编译时就能确定要调用哪个具体实现
    // 这就是静态分配的核心特点
    println!("The area is: {}", shape.area());
}

// 动态分配
// 使用特征对象（trait object）的print_area函数
// 这里使用了&dyn Shape，它是一个特征对象，允许在运行时确定要调用的具体实现
fn print_area_dynamic(shape: &dyn Shape) {
    // 调用shape的area方法时，Rust在运行时才确定要调用哪个具体实现
    // 这就是动态分配的核心特点
    println!("The area is: {}", shape.area());
}

fn main() {
    // 使用动态分配，需要在堆上分配对象
    // 因为特征对象不能直接引用栈上的对象
    // 使用Box::new在堆上分配Circle对象，并转换为Box<dyn Shape>
    let circle = Box::new(Circle { radius: 5.0 }) as Box<dyn Shape>;
    let rectangle = Box::new(Rectangle {
        width: 4.0,
        height: 6.0,
    }) as Box<dyn Shape>;

    // 动态分配的使用
    // 调用print_area_dynamic函数，并传入特征对象的引用
    // 这里Rust会在运行时确定要调用哪个area方法
    print_area_dynamic(&*circle); // 输出: The area is: 78.53981633974483

    // 上面的 &* 是一种解引用再取引用的方式，因为首先有一个Box引用，
    // 需要解引用它以获取到Box中的值（也就是Circle或Rectangle），
    // 然后再次取这个值的引用，因为print_area_dynamic需要一个&dyn Shape类型的引用。
    // 在实际代码中，通常会直接传递Box的引用，因为Box实现了Deref trait，
    // 允许直接将它用作它所包含值的引用。
    // 使用Box的as_ref方法直接获取内部值的引用
    print_area_dynamic(rectangle.as_ref()); // 输出: The area is: 24
}
```

> [!NOTE]
> 
> 静态分配（static dispatch）和动态分配（dynamic dispatch）是两种常见的运行时函数或方法调用的方式，它们各自有优势和取舍。
> 
> 如果需要高性能、类型安全和直接的控制，使用静态分配。
> 
> 如果你需要多态性、灵活性和可扩展性，使用动态分配。
> 
> 在 Rust 中，这通常意味着在不需要 trait 对象或 trait 约束时使用静态分配，而在需要这些功能时使用动态分配。

## 动态类型

### dyn

用于明确的指定特征在运行时是动态分配的。特征对象（trait objects）是一种特殊的指针类型，它们允许存储指向任何实现了特定特征的对象的引用。

Rust 对哪些特征可以转换为特征对象有所限制，这些限制确保了对象的安全性（object safety）。

> [!IMPORTANT]
> 
> 特征对象安全性规则：
> 
> 1. 方法的返回类型与 Self 无关
> 2. 方法中不包含任何泛型类型的参数
> 3. 没有关联类型
> 
> 满足以上规则，这个特征就可以使用关键字`dyn`修饰为特征对象，使用动态分配的特征。

```rust
trait Shape {
    // 返回f64，不是Self，满足规则1
    // 没有泛型参数，满足规则2
    // 没有关联类型，满足规则3
    fn area(&self) -> f64;
}
// Shape 可以被转换为特征对象
fn print_area_dynamic(shape: &dyn Shape) {}
//----------------------------------------------
// 标准库中的 Clone 特征，它返回 Self 类型
/*pub trait Clone: Sized {
    fn clone(&self) -> Self;
}*/
// 违反对象安全性规则1：返回 Self
fn use_clone_trait_object(obj: &dyn Clone) {}
//----------------------------------------------
trait GenericTrait {
    // 违反对象安全性规则2：有泛型参数
    fn process<T>(&self, value: T);
}
fn use_generic_trait_object(obj: &dyn GenericTrait) {}
//----------------------------------------------
trait AssociatedTypeTrait {
    // 违反对象安全性规则3：返回关联类型
    type Item;
    fn get_item(&self) -> &Self::Item;
}
// AssociatedTypeTrait 不能被转换为特征对象，因为它有一个关联类型
fn use_trait_object_2(obj: &dyn AssociatedTypeTrait) {}
```

### Sized

Rust 中常见的动态类型类型（DST，dynamically sized types）有: `str`、`[T]`、`dyn Trait`

它们都无法单独被使用，必须要通过引用或者 `Box` 来间接使用。

```rust
use std::any::type_name;
use std::mem::size_of_val;

fn print_size_of<T: Sized>(i: &T) {
    println!("Size of {} is {}", type_name::<T>(), size_of_val(i));
}

fn main() {
    let x = 5;
    // i32 的大小是固定的
    print_size_of(&x);

    // str是动态类型，只有运行时才能知道内存大小
    // let slice: str = "123";
    // print_size_of(&slice);
    // 但 Box<str> 的大小是固定的（等于指针大小）
    let b: Box<str> = "1".into();
    // 打印 Box<str> 的大小，不是 str 的内容大小
    print_size_of(&b);
}
```

## 宏

宏不产生函数的调用，而是展开代码，然后和程序的其余代码一起编译。

```rust
// 使用 #[macro_export] 属性来允许这个宏在其他模块中被使用
// 当宏被定义在一个库中时，这个属性是必要的
#[macro_export]
macro_rules! arr {
    // 宏的匹配模式：接受任意数量的表达式，每个表达式由 $x 标识
    // $( ... ),* 表示可以匹配零个或多个由逗号分隔的表达式
    ( $( $x:expr ),* ) => {
        {
            // 创建一个新的可变向量，用于存储表达式的结果
            let mut temp_vec = Vec::new();

            // 对每个传入的表达式执行以下操作
            $(
                // 将当前表达式 $x 的值推入 temp_vec 向量中
                temp_vec.push($x);
            )*

            // 返回填充了所有表达式的值的向量
            temp_vec
        }
    };
}
```

将宏展开后：

```rust
fn main() {
    let a = {
        let mut temp_vec = Vec::new();
        temp_vec.push(1);
        temp_vec.push(2);
        temp_vec.push(3);
        temp_vec
    };
    println!("{:?}", a);
}
```

[Introduction - The Little Book of Rust Macros (veykril.github.io)](https://veykril.github.io/tlborm/)

## 网络

### TCP（传输控制协议）

```rust
use std::io::{BufRead, BufReader, Write};
use std::net::{TcpListener, TcpStream};

fn main() {
    // 创建一个TcpListener实例，绑定到本地地址127.0.0.1的7878端口  
    let listener = TcpListener::bind("127.0.0.1:7878").unwrap();

    // 循环等待新的TCP连接  
    for stream in listener.incoming() {
        // unwrap用于从Result类型中提取值，如果发生错误则会panic  
        let stream = stream.unwrap();
        // 对每个连接启动一个线程处理（这里并未显式使用线程，但为了并发可以这么做）  
        handle_connection(stream);
    }
}

fn handle_connection(mut stream: TcpStream) {
    // 创建一个BufReader实例，用于从TcpStream中读取数据，并提供缓冲功能  
    let buf_reader = BufReader::new(&mut stream);

    // 读取HTTP请求，直到遇到空行（HTTP请求头的结束）  
    let http_request: Vec<_> = buf_reader
        // 从BufReader中按行读取数据  
        .lines()
        // unwrap用于从Result中提取字符串，如果读取失败则会panic  
        .map(|result| result.unwrap())
        // take_while用于只取非空行，即HTTP请求头  
        .take_while(|line| !line.is_empty())
        // 收集所有行到一个Vec<String>中  
        .collect();

    // 打印读取到的HTTP请求  
    println!("Request: {:#?}", http_request);

    let page = r#"
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>Hello!</title>
  </head>
  <body>
    <h1>Hello!</h1>
    <p>Hi from Rust</p>
  </body>
</html>
"#;

    // 构造一个简单的HTTP响应
    let response = format!("HTTP/1.1 200 OK\r\n\
    Content-Length: {}\r\n\r\n\
    {}", page.len(), page);
    // 将响应写入TcpStream  
    stream.write_all(response.as_bytes()).unwrap();
}
```

# 专业篇

## 算法

### 随机数

[![rand-badge](https://badge-cache.kominick.com/crates/v/rand.svg?label=rand)](https://docs.rs/rand/)[![rand_distr-badge](https://badge-cache.kominick.com/crates/v/rand.svg?label=rand_distr)](https://docs.rs/rand_distr/)

#### 生成随机数

```rust
use rand::Rng;
use rand::distributions::{Distribution, Standard};

fn main() {
    let mut rng = rand::thread_rng();

    // 整数：最小值到最大值的随机数（全闭区间）
    let n_i8: i8 = rng.gen();
    let n_u8: u8 = rng.gen();
    let n_i16: i16 = rng.gen();
    let n_u16: u16 = rng.gen();
    let n_i32: i32 = rng.gen();
    let n_u32: u32 = rng.gen();
    let n_i64: i64 = rng.gen();
    let n_u64: u64 = rng.gen();
    let n_i128: i128 = rng.gen();
    let n_u128: u128 = rng.gen();
    let n_isize: isize = rng.gen();
    let n_usize: usize = rng.gen();

    // 浮点数：0到1的随机数（左闭右开区间）
    let n_f32: f32 = rng.gen();
    let n_f64: f64 = rng.gen();
    
    // 自定义类型的随机数（本质还是多个基本数据类型的组合）
    let n_p: Point = rng.gen();
    let n_t: (i32, bool, f64) = rng.gen();
}

struct Point {
    x: f64,
    y: f64,
}

// 为Point类型实现Distribution特征
impl Distribution<Point> for Standard {
    fn sample<R: Rng + ?Sized>(&self, rng: &mut R) -> Point {
        // 通过宏，生成元组内的所有参数的随机数
        let (rand_x, rand_y) = rng.gen();
        Point {
            x: rand_x,
            y: rand_y,
        }
    }
}
```

#### 生成特定范围内的随机数

```rust
use rand::distributions::Uniform;
use rand::Rng;

fn main() {
    let mut rng = rand::thread_rng();

    // 根据特定区间，生成随机数
    let n_i = rng.gen_range(0..10);//[0,10)
    println!("{}", n_i);
    let n_f = rng.gen_range(-1.0..=0.1);//[-1.0,0.1]
    println!("{}", &n_f);

    // 推荐使用Uniform，性能更好
    let n_i: u32 = rng.sample(Uniform::new(0, 10));//[0,10)
    println!("{}", n_i);

    let n_f: f64 = rng.sample(Uniform::new_inclusive(-1.0, 0.1));//[-1.0,0.1]
    println!("{}", n_f);
}
```

#### 生成正态分布的随机数

$$
f(x|\mu,\sigma^2) = \frac{1}{\sqrt{2\pi\sigma^2}} e^{-\frac{(x-\mu)^2}{2\sigma^2}}
$$


其中，  
- \( x \) 是随机变量
- \( μ \) 是分布的均值（或期望值），正态分布的对称轴，也是概率最大的值
- \( σ² \) 是分布的方差，离散程度，方差越大，数据越分散
- \( \σ \) 是分布的标准差

```rust
use rand::distributions::Distribution;
use rand::thread_rng;
use rand_distr::Normal;

fn main() {
    let mut rng = thread_rng();
    // 正态分布：均值2.0，标准差3.0
    let normal = Normal::new(2.0, 3.0).unwrap();
    // 根据正态分布的概率，生成随机数
    let v = normal.sample(&mut rng);
    println!("{} is from a N(2, 9) distribution", v);
}
```

示例中的正态分布图像，横轴为随机生成的数，纵轴为这个数出现的概率。

<div style="text-align: center;"><svg width="800" height="600" viewBox="0 0 800 600" xmlns="http://www.w3.org/2000/svg">
<rect x="0" y="0" width="800" height="600" opacity="1" fill="#FFFFFF" stroke="none"/>
<text x="400" y="20" dy="0.76em" text-anchor="start" font-family="Arial" font-size="16.129032258064516" opacity="1" fill="#000000">
Top=(2,0.1329807601338109)
</text>
<polyline fill="none" opacity="1" stroke="#000000" stroke-width="1" points="59,30 59,539 "/>
<text x="50" y="539" dy="0.5ex" text-anchor="end" font-family="Arial" font-size="16.129032258064516" opacity="1" fill="#000000">
0.0
</text>
<polyline fill="none" opacity="1" stroke="#000000" stroke-width="1" points="54,539 59,539 "/>
<text x="50" y="501" dy="0.5ex" text-anchor="end" font-family="Arial" font-size="16.129032258064516" opacity="1" fill="#000000">
0.01
</text>
<polyline fill="none" opacity="1" stroke="#000000" stroke-width="1" points="54,501 59,501 "/>
<text x="50" y="463" dy="0.5ex" text-anchor="end" font-family="Arial" font-size="16.129032258064516" opacity="1" fill="#000000">
0.02
</text>
<polyline fill="none" opacity="1" stroke="#000000" stroke-width="1" points="54,463 59,463 "/>
<text x="50" y="425" dy="0.5ex" text-anchor="end" font-family="Arial" font-size="16.129032258064516" opacity="1" fill="#000000">
0.03
</text>
<polyline fill="none" opacity="1" stroke="#000000" stroke-width="1" points="54,425 59,425 "/>
<text x="50" y="386" dy="0.5ex" text-anchor="end" font-family="Arial" font-size="16.129032258064516" opacity="1" fill="#000000">
0.04
</text>
<polyline fill="none" opacity="1" stroke="#000000" stroke-width="1" points="54,386 59,386 "/>
<text x="50" y="348" dy="0.5ex" text-anchor="end" font-family="Arial" font-size="16.129032258064516" opacity="1" fill="#000000">
0.05
</text>
<polyline fill="none" opacity="1" stroke="#000000" stroke-width="1" points="54,348 59,348 "/>
<text x="50" y="310" dy="0.5ex" text-anchor="end" font-family="Arial" font-size="16.129032258064516" opacity="1" fill="#000000">
0.06
</text>
<polyline fill="none" opacity="1" stroke="#000000" stroke-width="1" points="54,310 59,310 "/>
<text x="50" y="272" dy="0.5ex" text-anchor="end" font-family="Arial" font-size="16.129032258064516" opacity="1" fill="#000000">
0.07
</text>
<polyline fill="none" opacity="1" stroke="#000000" stroke-width="1" points="54,272 59,272 "/>
<text x="50" y="233" dy="0.5ex" text-anchor="end" font-family="Arial" font-size="16.129032258064516" opacity="1" fill="#000000">
0.08
</text>
<polyline fill="none" opacity="1" stroke="#000000" stroke-width="1" points="54,233 59,233 "/>
<text x="50" y="195" dy="0.5ex" text-anchor="end" font-family="Arial" font-size="16.129032258064516" opacity="1" fill="#000000">
0.09
</text>
<polyline fill="none" opacity="1" stroke="#000000" stroke-width="1" points="54,195 59,195 "/>
<text x="50" y="157" dy="0.5ex" text-anchor="end" font-family="Arial" font-size="16.129032258064516" opacity="1" fill="#000000">
0.1
</text>
<polyline fill="none" opacity="1" stroke="#000000" stroke-width="1" points="54,157 59,157 "/>
<text x="50" y="118" dy="0.5ex" text-anchor="end" font-family="Arial" font-size="16.129032258064516" opacity="1" fill="#000000">
0.11
</text>
<polyline fill="none" opacity="1" stroke="#000000" stroke-width="1" points="54,118 59,118 "/>
<text x="50" y="80" dy="0.5ex" text-anchor="end" font-family="Arial" font-size="16.129032258064516" opacity="1" fill="#000000">
0.12
</text>
<polyline fill="none" opacity="1" stroke="#000000" stroke-width="1" points="54,80 59,80 "/>
<text x="50" y="42" dy="0.5ex" text-anchor="end" font-family="Arial" font-size="16.129032258064516" opacity="1" fill="#000000">
0.13
</text>
<polyline fill="none" opacity="1" stroke="#000000" stroke-width="1" points="54,42 59,42 "/>
<polyline fill="none" opacity="1" stroke="#000000" stroke-width="1" points="60,540 769,540 "/>
<text x="60" y="550" dy="0.76em" text-anchor="middle" font-family="Arial" font-size="16.129032258064516" opacity="1" fill="#000000">
-10.0
</text>
<polyline fill="none" opacity="1" stroke="#000000" stroke-width="1" points="60,540 60,545 "/>
<text x="116" y="550" dy="0.76em" text-anchor="middle" font-family="Arial" font-size="16.129032258064516" opacity="1" fill="#000000">
-8.0
</text>
<polyline fill="none" opacity="1" stroke="#000000" stroke-width="1" points="116,540 116,545 "/>
<text x="173" y="550" dy="0.76em" text-anchor="middle" font-family="Arial" font-size="16.129032258064516" opacity="1" fill="#000000">
-6.0
</text>
<polyline fill="none" opacity="1" stroke="#000000" stroke-width="1" points="173,540 173,545 "/>
<text x="230" y="550" dy="0.76em" text-anchor="middle" font-family="Arial" font-size="16.129032258064516" opacity="1" fill="#000000">
-4.0
</text>
<polyline fill="none" opacity="1" stroke="#000000" stroke-width="1" points="230,540 230,545 "/>
<text x="286" y="550" dy="0.76em" text-anchor="middle" font-family="Arial" font-size="16.129032258064516" opacity="1" fill="#000000">
-2.0
</text>
<polyline fill="none" opacity="1" stroke="#000000" stroke-width="1" points="286,540 286,545 "/>
<text x="343" y="550" dy="0.76em" text-anchor="middle" font-family="Arial" font-size="16.129032258064516" opacity="1" fill="#000000">
0.0
</text>
<polyline fill="none" opacity="1" stroke="#000000" stroke-width="1" points="343,540 343,545 "/>
<text x="400" y="550" dy="0.76em" text-anchor="middle" font-family="Arial" font-size="16.129032258064516" opacity="1" fill="#000000">
2.0
</text>
<polyline fill="none" opacity="1" stroke="#000000" stroke-width="1" points="400,540 400,545 "/>
<text x="457" y="550" dy="0.76em" text-anchor="middle" font-family="Arial" font-size="16.129032258064516" opacity="1" fill="#000000">
4.0
</text>
<polyline fill="none" opacity="1" stroke="#000000" stroke-width="1" points="457,540 457,545 "/>
<text x="513" y="550" dy="0.76em" text-anchor="middle" font-family="Arial" font-size="16.129032258064516" opacity="1" fill="#000000">
6.0
</text>
<polyline fill="none" opacity="1" stroke="#000000" stroke-width="1" points="513,540 513,545 "/>
<text x="570" y="550" dy="0.76em" text-anchor="middle" font-family="Arial" font-size="16.129032258064516" opacity="1" fill="#000000">
8.0
</text>
<polyline fill="none" opacity="1" stroke="#000000" stroke-width="1" points="570,540 570,545 "/>
<text x="627" y="550" dy="0.76em" text-anchor="middle" font-family="Arial" font-size="16.129032258064516" opacity="1" fill="#000000">
10.0
</text>
<polyline fill="none" opacity="1" stroke="#000000" stroke-width="1" points="627,540 627,545 "/>
<text x="683" y="550" dy="0.76em" text-anchor="middle" font-family="Arial" font-size="16.129032258064516" opacity="1" fill="#000000">
12.0
</text>
<polyline fill="none" opacity="1" stroke="#000000" stroke-width="1" points="683,540 683,545 "/>
<text x="740" y="550" dy="0.76em" text-anchor="middle" font-family="Arial" font-size="16.129032258064516" opacity="1" fill="#000000">
14.0
</text>
<polyline fill="none" opacity="1" stroke="#000000" stroke-width="1" points="740,540 740,545 "/>
<polyline fill="none" opacity="1" stroke="#000000" stroke-width="1" points="60,539 62,539 65,539 68,539 71,539 74,539 77,539 79,539 82,539 85,539 88,539 91,539 94,539 96,539 99,539 102,538 105,538 108,538 111,538 113,538 116,538 119,537 122,537 125,537 128,536 130,536 133,536 136,535 139,535 142,534 145,534 147,533 150,533 153,532 156,531 159,530 162,529 164,528 167,527 170,526 173,525 176,524 179,522 181,521 184,519 187,517 190,515 193,513 196,511 198,509 201,506 204,503 207,500 210,497 213,494 215,491 218,487 221,483 224,479 227,475 230,471 232,466 235,461 238,456 241,450 244,445 247,439 250,433 252,426 255,420 258,413 261,405 264,398 267,390 269,382 272,374 275,366 278,357 281,348 284,339 286,330 289,321 292,311 295,302 298,292 301,282 303,272 306,262 309,251 312,241 315,231 318,220 320,210 323,200 326,190 329,180 332,170 335,160 337,151 340,141 343,132 346,123 349,114 352,106 354,98 357,90 360,83 363,76 366,70 369,64 371,58 374,53 377,48 380,44 383,41 386,38 388,35 391,33 394,32 397,31 400,30 403,31 405,32 408,33 411,35 414,38 417,41 420,44 423,48 425,53 428,58 431,64 434,70 437,76 440,83 442,90 445,98 448,106 451,114 454,123 457,132 459,141 462,151 465,160 468,170 471,180 474,190 476,200 479,210 482,220 485,231 488,241 491,251 493,262 496,272 499,282 502,292 505,302 508,311 510,321 513,330 516,339 519,348 522,357 525,366 527,374 530,382 533,390 536,398 539,405 542,413 544,420 547,426 550,433 553,439 556,445 559,450 561,456 564,461 567,466 570,471 573,475 576,479 578,483 581,487 584,491 587,494 590,497 593,500 596,503 598,506 601,509 604,511 607,513 610,515 613,517 615,519 618,521 621,522 624,524 627,525 630,526 632,527 635,528 638,529 641,530 644,531 647,532 649,533 652,533 655,534 658,534 661,535 664,535 666,536 669,536 672,536 675,537 678,537 681,537 683,538 686,538 689,538 692,538 695,538 698,538 700,539 703,539 706,539 709,539 712,539 715,539 717,539 720,539 723,539 726,539 729,539 732,539 734,539 737,539 740,539 743,539 746,539 749,539 751,539 754,539 757,539 760,539 763,539 766,539 769,539 "/>
</svg>
</div>

使用 plotters 绘制这个正态分布曲线：

```toml
[dependencies]
plotters = "0.3.6"
```

```rust
use std::error::Error;
use std::f64::consts::PI;
use std::iter::from_fn;

use plotters::backend::SVGBackend;
use plotters::element::Text;
use plotters::prelude::{ChartBuilder, IntoDrawingArea, IntoFont, LineSeries, WHITE};
use plotters::style::BLACK;

// 正态分布公式
fn normal_pdf(x: f64, mean: f64, std_dev: f64) -> f64 {
    (-0.5 * ((x - mean) / std_dev).powi(2)).exp() / (std_dev * (2.0 * PI).sqrt())
}

// 正态分布坐标点
fn normal_pdf_points(start: f64, step: f64, end: f64, mean: f64, std_dev: f64) -> Vec<(f64, f64)> {
    let mut start = start;
    from_fn(move || {
        if start >= end {
            None
        } else {
            let x = start;
            start += step;
            Some((x, normal_pdf(x, mean, std_dev)))
        }
    }).collect::<Vec<(f64, f64)>>()
}

fn draw_normal_pdf(x_min: f64, x_max: f64, y_max: f64, mean: f64, points: Vec<(f64, f64)>) -> Result<(), Box<dyn Error>> {
    // 图像
    let width = 800;
    let high = 600;
    let root_area = SVGBackend::new("output.svg", (width, high))
        .into_drawing_area();
    // 设置背景
    root_area.fill(&WHITE)?;

    // 顶点标记
    root_area.draw(
        &Text::new(
            format!("Top=({},{})", mean, y_max),
            (width as i32 / 2, 20),
            ("Arial", 20).into_font(),
        )
    )?;

    // 创建一个图表
    let mut chart = ChartBuilder::on(&root_area)
        .margin(30)// 图表边距
        .set_left_and_bottom_label_area_size(30)// 坐标轴大小
        .build_cartesian_2d(x_min..x_max, 0.0..y_max)?;

    // 配置网格
    chart.configure_mesh()
        // 不显示网格
        .disable_x_mesh()
        .disable_y_mesh()
        // 坐标轴的标记个数
        .x_labels(20)
        .y_labels(20)
        // 坐标轴的字体格式
        .label_style(("Arial", 20).into_font())
        .draw()?;

    // 创建一个包含正态分布PDF值的曲线
    let series = LineSeries::new(
        // 坐标点
        points, BLACK,
    ).point_size(0);

    // 绘制
    chart.draw_series(series)?;

    Ok(())
}

fn main() {
    // 正态分布的均值
    let mean = 2.0;
    // 正态分布的标准差
    let std_dev = 3.0;

    let x_min = -10.0;
    let x_max = 15.0;
    let step = 0.1;

    // 期望值是对称轴的x值，y是最大值
    let y_max = normal_pdf(mean, mean, std_dev);
    let p = normal_pdf_points(x_min, step, x_max, mean, std_dev);

    draw_normal_pdf(x_min, x_max, y_max, mean, p).unwrap();
}
```

#### 生成字母和数字的随机字符串

```rust
use rand::distributions::Alphanumeric;
use rand::Rng;

fn main() {
    let rand_string: String = rand::thread_rng()
        // a-z, A-Z, 0-9
        .sample_iter(&Alphanumeric)
        // 限制30个字符
        .take(30)
        // 将ASCII码u8转为char
        .map(char::from)
        // 将字符迭代器转换为字符串
        .collect();

    println!("{}", rand_string);
}
```

#### 生成指定字符表的随机字符串

```rust
use rand::Rng;

fn main() {
    // 定义一个常量 CHARSET，它是一个包含大小写字母、数字和特殊字符的字节切片
    // Rust 的多行字符串字面量，通过反斜杠 \ 来进行续行
    const CHARSET: &[u8] = "\
                ABCDEFGHIJKLMNOPQRSTUVWXYZ\
                abcdefghijklmnopqrstuvwxyz\
                0123456789\
                !#$%&'()*+,-./:;<=>?@[]^_`{|}~\
                \" \\"//特殊字符，双引号，空格，反斜线
        .as_bytes();
    // 定义一个常量 PASSWORD_LEN，表示要生成的密码的长度
    const PASSWORD_LEN: usize = 30;

    let mut rng = rand::thread_rng();

    let password: String = (0..PASSWORD_LEN)
        .map(|_| {
            // 使用迭代器 (0..PASSWORD_LEN) 来生成一个随机数
            let idx = rng.gen_range(0..CHARSET.len());
            // 根据这个随机数，映射密码表的字符
            CHARSET[idx] as char
        })
        // 将字符迭代器转换为字符串
        .collect();

    println!("{}", password);
}
```

密码的字符不重复：

```rust
use rand::seq::SliceRandom;

fn main() {
    // 定义一个包含大小写字母、数字和特殊字符的字节 Vec
    let mut charset: Vec<u8> = "\
                ABCDEFGHIJKLMNOPQRSTUVWXYZ\
                abcdefghijklmnopqrstuvwxyz\
                0123456789\
                !#$%&'()*+,-./:;<=>?@[]^_`{|}~\
                \" \\"
        .as_bytes().to_vec();
    const PASSWORD_LEN: usize = 30;

    // 确保密码长度不会超过字符集长度，这样才能让每一个字符不重复
    if PASSWORD_LEN > charset.len() {
        panic!("Password length exceeds the character set length.");
    }

    let mut rng = rand::thread_rng();
    // 使用 Vec 的 shuffle 方法来随机排列字符集
    charset.shuffle(&mut rng);

    // 取 charset 的切片，转换为字符串即可获得每个字符不重复的密码
    let password: String = charset[..PASSWORD_LEN]
        .iter()
        .map(|&b| b as char)
        .collect();

    println!("{}", password);
}
```

#### 生成随机CJK字符串

不连续区间的随机。

```rust
use rand::Rng;

fn main() {
    let charset_cjk: Vec<(u32, u32)> = vec![
        (0x4E00, 0x9FFF), /* Base */
        (0x3400, 0x4DBF), /* A */
        (0x20000, 0x2A6DF), /* B */
        (0x2A700, 0x2B73F), /* C */
        (0x2B740, 0x2B81F), /* D */
        (0x2B820, 0x2CEAF), /* E */
        (0x2CEB0, 0x2EBEF), /* F */
        (0x30000, 0x3134F), /* G */
        (0x31350, 0x323AF), /* H */
        (0x2EBF0, 0x2EE5F), /* I */
        (0x2EBF0, 0x2EE5F), /* I */
    ];

    const LEN: usize = 30;

    let mut rng = rand::thread_rng();
    loop {
        let str: String = (0..LEN).map(|_| {
            let i = rng.gen_range(0..charset_cjk.len());
            let start = charset_cjk[i].0;
            let end = charset_cjk[i].1;
            char::from_u32(rng.gen_range(start..=end)).unwrap()
        }).collect();

        println!("{}", str);
    }
}
```

[Unicode Character Code Charts](https://www.unicode.org/charts/)

### 排序

#### 整数向量

```rust
fn main() {
    let mut vec = vec![1, 5, 10, 5, 15];
    // 稳定的排序，不会重新排列相等的元素
    vec.sort();
    // 不稳定的排序，可能会重新排列相等的元素，不需要分配额外的空间
    vec.sort_unstable();
    // [1, 5, 5, 10, 15]
    println!("{:?}", vec);
}
```

#### 浮点数向量

```rust
fn main() {
    let mut vec = vec![1.1, 1.15, 5.5, 1.123, 2.0];
    // 浮点数排序，自定义排序规则，返回Ordering枚举
    // NaN无法比较
    vec.sort_by(|a, b| a.partial_cmp(b).unwrap());
    // [1.1, 1.123, 1.15, 2.0, 5.5]
    println!("{:?}", vec);
}
```

## 命令行

### 解析命令行参数

[![clap-badge](https://badge-cache.kominick.com/crates/v/clap.svg?label=clap)](https://docs.rs/clap/)

```toml
clap = { version = "4.5.4", features = ["derive"] }
```

#### 使用结构体封装

```rust
use clap::Parser;

#[derive(Parser)]
#[command(name = "MyApp")]
#[command(version)]// 从Cargo.toml获取
// 完整的帮助文档, 帮助文档另起一行显示
#[command(long_about = "这是段很长的说明文档", next_line_help = true)]
struct Cli {}

fn main() {
    // 解析命令行实例
    let _cli = Cli::parse();
}
```

编译后，使用`--help`可以查看：

```powershell
这是段很长的说明文档

Usage: Stu.exe

Options:
  -h, --help
          Print help (see a summary with '-h')

  -V, --version
          Print version
```

默认有`-h`和`--help`两个命令，如果指定`version`信息，还会出现`-v`和`--version`两个命令。

#### 必选和非必填

```rust
use clap::Parser;

#[derive(Parser)]
#[command(name = "MyApp")]
struct Cli {
    /// 这个字段是必填的
    s1: String,// 必填

    /// 这个字段是非必填的,默认是空字符
    #[arg(default_value = "")]
    s2: Option<String>,// 设置默认值或者使用Option的是非必填的
}

fn main() {
    let cli = Cli::parse();

    // 获取命令行参数的值
    println!("s1={},s2={}", cli.s1, cli.s2.unwrap());
}
```

```powershell
Usage: Stu.exe <S1> [S2]

Arguments:
  <S1>  这个字段是必填的
  [S2]  这个字段是非必填的,默认是空字符 [default: ]

Options:
  -h, --help  Print help
```

#### 开关标识符的指定

```rust
use clap::Parser;

#[derive(Parser)]
#[command(name = "MyApp")]
struct Cli {
    /// 指定缩写和全写的开关标识符
    #[arg(short = '1', long = "str")]
    s1: String,

    /// 使用字段的首字母作为缩写开关,字段名作为全写开关的标识符
    /// 开关标识符分配时不能冲突
    #[arg(short, long, default_value = "0")]
    s2: i32,// 支持对非法字符的拦截
}

fn main() {
    let cli = Cli::parse();
    println!("s1={},s2={}", cli.s1, cli.s2);
}
```

```powershell
Usage: Stu.exe [OPTIONS] --str <S1>

Options:
  -1, --str <S1>  指定缩写和全写的开关标识符
  -s, --s2 <S2>   使用字段的首字母作为缩写开关,字段名作为全写开关的标识符 开关标识符分配时不能冲突 [default: 0]
  -h, --help      Print help
```

> [!NOTE]
>
> 可以使用的开关格式：
>
> - `stu.exe --str 字符串`
> - `stu.exe --str字符串`
> - `stu.exe --str=字符串`
>
> `-h`是简略的帮助文档，`--help`是详细的帮助文档，显示文档注释

#### 开关标识符的标记检测

```rust
use clap::Parser;

#[derive(Parser)]
#[command(name = "MyApp")]
struct Cli {
    #[arg(short)]
    boolean: bool,// 只要有这个标记,就是true

    #[arg(short, action = clap::ArgAction::Count)]
    count: u8,// 统计这个标记出现的次数
}

fn main() {
    //use clap::ArgAction::Count;
    let cli = Cli::parse();
    println!("a={},count={}", cli.boolean, cli.count);
}
```

```powershell
Usage: Stu.exe [OPTIONS]

Options:
  -b
  -c...
  -h, --help  Print help
```

#### 子命令

```rust
use clap::{Args, Parser, Subcommand};

#[derive(Parser)]
#[command(name = "MyApp", version = "0.1")]
#[command(propagate_version = true)]// 子命令也可以显示版本开关
struct Cli {
    #[command(subcommand)]
    command: Commands,
}

#[derive(Subcommand)]
enum Commands {
    SUB1(Sub1),
    SUB2(Sub2),
}

#[derive(Args)]
struct Sub1 {
    #[arg(short)]
    name: Option<String>,
}

#[derive(Args)]
struct Sub2 {
    #[arg(long)]
    name: Option<String>,
}

fn main() {
    let cli = Cli::parse();
    match cli.command {
        Commands::SUB1(name) => {
            println!("sub1={}", name.name.unwrap());
        }
        Commands::SUB2(name) => {
            println!("sub2={}", name.name.unwrap());
        }
    }
}
```

```powershell
Usage: Stu.exe <COMMAND>

Commands:
  sub1
  sub2
  help  Print this message or the help of the given subcommand(s)

Options:
  -h, --help     Print help
  -V, --version  Print version
------------------------------------------------------------------
Usage: Stu.exe sub1 [OPTIONS]

Options:
  -n <NAME>
  -h, --help     Print help
  -V, --version  Print version
------------------------------------------------------------------
Usage: Stu.exe sub2 [OPTIONS]

Options:
      --name <NAME>
  -h, --help         Print help
  -V, --version      Print version
```

#### 枚举值

```rust
use std::thread;
use std::time::Duration;
use clap::{Parser, ValueEnum};

#[derive(Parser)]
#[command(name = "MyApp")]
struct Cli {
    /// 运行程序的模式
    #[arg(value_enum)]
    mode: Mode,
}

#[derive(Copy, Clone, PartialEq, Eq, PartialOrd, Ord, ValueEnum)]
enum Mode {
    /// 全速运行
    #[value(name = "1")]// 自定义枚举值
    Fast,
    /// 缓慢运行
    #[value(name = "2")]
    Slow,
}

fn main() {
    let cli = Cli::parse();
    match cli.mode {
        Mode::Fast => {
            println!("请稍后...");
            thread::sleep(Duration::from_secs(1));
            println!("运行完毕");
        }
        Mode::Slow => {
            println!("请稍后...");
            thread::sleep(Duration::from_secs(5));
            println!("运行完毕");
        }
    }
}
```

```powershell
Usage: Stu.exe <MODE>

Arguments:
  <MODE>
          运行程序的模式

          Possible values:
          - 1: 全速运行
          - 2: 缓慢运行

Options:
  -h, --help
          Print help (see a summary with '-h')

  -V, --version
          Print version
```

#### 有效性验证

```rust
use clap::Parser;

#[derive(Parser)]
#[command(name = "MyApp")]
struct Cli {
    #[arg(value_parser = name_in_len)]
    name: String,
}

fn name_in_len(name: &str) -> Result<String, String> {
    if name.len() > 4 {
        Err(format!("姓名:{},不能超过4个字", name).to_string())
    } else {
        Ok(name.to_string())
    }
}


fn main() {
    let cli = Cli::parse();
    println!("{}", cli.name);
}
```

#### 开关关联

```rust
use clap::{Args, Parser};

#[derive(Parser)]
#[command(name = "MyApp")]
struct Cli {
    #[command(flatten)]
    g1: Group1,
    #[command(flatten)]
    g2: Group2,
}

#[derive(Args)]
// 参数a,b,c为一组,必填,只能填1个
#[group(required = true, multiple = false)]
struct Group1 {
    #[arg(short = 'a')]
    a1: Option<String>,
    #[arg(short = 'b')]
    a2: Option<String>,
    #[arg(short = 'c')]
    a3: Option<String>,
}

#[derive(Args)]
// 参数1,2,3为一组,必填,可以多选
#[group(required = true, multiple = true)]
struct Group2 {
    // 参数1,2默认只能选一个
    #[arg(short = '1', group = "Group3")]
    b1: bool,
    #[arg(short = '2', group = "Group3")]
    b2: bool,
    #[arg(short = '3')]
    b3: bool,
}

fn main() {
    let cli = Cli::parse();
    println!("a={:?},b={:?},c={:?}", cli.g1.a1, cli.g1.a2, cli.g1.a3);
    println!("a={:?},b={:?},c={:?}", cli.g2.b1, cli.g2.b2, cli.g2.b3);
}
```

```powershell
Usage: Stu.exe <-a <A1>|-b <A2>|-c <A3>> <-1|-2|-3>

Options:
  -a <A1>
  -b <A2>
  -c <A3>
  -1
  -2
  -3
  -h, --help     Print help
  -V, --version  Print version
```

#### 测试

```rust
// ...
#[test]
fn verify() {
    use clap::CommandFactory;
    Cli::command().debug_assert();
}

#[test]
fn test1() {
    // 正常测试
    Cli::try_parse_from(["Stu", "-a", "asdf", "-1"]).unwrap();
}

#[test]
#[should_panic]
fn test2() {
    // a和b参数只能存在一个
    Cli::try_parse_from(["Stu", "-a", "asdf", "-b", "12165"]).unwrap();
}
```

### ANSI终端

[![ansi_term-badge](https://badge-cache.kominick.com/crates/v/base64.svg?label=ansi_term)](https://docs.rs/ansi_term/)

#### 彩色

```rust
use ansi_term::Colour;

fn main() {
    println!("{}", Colour::Black.paint("hello world"));
    println!("{}", Colour::Red.paint("hello world"));
    println!("{}", Colour::Green.paint("hello world"));
    println!("{}", Colour::Yellow.paint("hello world"));
    println!("{}", Colour::Blue.paint("hello world"));
    println!("{}", Colour::Purple.paint("hello world"));
    println!("{}", Colour::Cyan.paint("hello world"));
    println!("{}", Colour::White.paint("hello world"));

    // https://upload.wikimedia.org/wikipedia/commons/1/15/Xterm_256color_chart.svg
    for i in 0..=255 {
        println!("{:03}={}", i, Colour::Fixed(i).paint("hello world"));
    }

    for r in 0..=15 {
        for g in 0..=15 {
            for b in 0..=15 {
                let _r = r * 17;
                let _g = g * 17;
                let _b = b * 17;
                print!("{}", Colour::RGB(_r, _g, _b)
                    .paint(format!("[{:03},{:03},{:03}]", _r, _g, _b))
                );
            }
            println!();
        }
        println!();
    }
    
    // 绿底黑字
    println!("{}", Style::new().on(Green).fg(Black).paint("hello world"));
}
```

#### 加粗

```rust
use ansi_term::Style;

fn main() {
    println!("{} hello world", Style::new().bold().paint("hello world"));
}
```

#### 样式混合

```rust
use ansi_term::{Colour, Style};

fn main() {
    println!("{}", Colour::Blue.paint("hello world"));
    // 保留字体颜色不变,其余恢复初始状态
    println!("normal:{}", Colour::Blue.normal().paint("hello world"));
    // 加粗
    println!("bold:{}", Colour::Blue.bold().paint("hello world"));
    // 暗淡
    println!("dimmed:{}", Colour::Blue.dimmed().paint("hello world"));
    // 倾斜
    println!("italic:{}", Colour::Blue.italic().paint("hello world"));
    // 下划线
    println!("underline:{}", Colour::Blue.underline().paint("hello world"));
    // 闪烁
    println!("blink:{}", Colour::Blue.blink().paint("hello world"));
    // 字体颜色和背景色交换
    println!("reverse:{}", Colour::Blue.reverse().paint("hello world"));
    // 透明隐藏
    println!("hidden:{}", Colour::Blue.hidden().paint("hello world"));
    // 删除线
    println!("strikethrough:{}", Colour::Blue.strikethrough().paint("hello world"));

    // 绿底黑字
    println!("{}", Style::new().fg(Colour::Black).on(Colour::Green).paint("hello world"));
}
```

## 压缩

### tar+gzip

[![flate2-badge](https://badge-cache.kominick.com/crates/v/flate2.svg?label=flate2)](https://docs.rs/flate2/) [![tar-badge](https://badge-cache.kominick.com/crates/v/tar.svg?label=tar)](https://docs.rs/tar/)

#### GzEncoder

```rust
use std::fs::File;
use flate2::Compression;
use flate2::write::GzEncoder;

fn main() -> Result<(), std::io::Error> {
    // GZIP
    let tar_gz = File::create("archive.tar.gz")?;
    let enc = GzEncoder::new(tar_gz, Compression::best());
    let mut tar = tar::Builder::new(enc);
    // 压缩包内的目录结构,待压缩目录结构
    tar.append_dir_all("src", "src")?;
    Ok(())
}
```

#### GzDecoder

```rust
use std::fs::File;
use flate2::read::GzDecoder;
use tar::Archive;

fn main() -> Result<(), std::io::Error> {
    let path = "archive.tar.gz";

    let tar_gz = File::open(path)?;
    let tar = GzDecoder::new(tar_gz);
    let mut archive = Archive::new(tar);
    // 解压到当前位置
    archive.unpack(".")?;

    Ok(())
}
```

## 并发、并行

### 显式线程

#### ~~线程作用域（Scope）~~

[![crossbeam-badge](https://badge-cache.kominick.com/crates/v/crossbeam.svg?label=crossbeam)](https://docs.rs/crossbeam/)

> [!TIP]
>
> 从 Rust v1.63 开始，被`std::thread::scoped`取代。

```rust
extern crate crossbeam;

fn main() {
    let arr = &[1, 25, -4, 10];
    let max = find_max(arr);
    assert_eq!(max, Some(25));
}

fn find_max(arr: &[i32]) -> Option<i32> {
    const THRESHOLD: usize = 2;

    if arr.len() <= THRESHOLD {
        return arr.iter().cloned().max();
    }

    let mid = arr.len() / 2;
    let (left, right) = arr.split_at(mid);

    // 二分法寻找最大值
    crossbeam::scope(|s| {
        let thread_l = s.spawn(|_| find_max(left));
        let thread_r = s.spawn(|_| find_max(right));

        let max_l = thread_l.join().unwrap()?;
        let max_r = thread_r.join().unwrap()?;
        
        Some(max_l.max(max_r))
    }).unwrap()
}
```

```rust
use std::thread;
use std::time::Duration;

fn main() {
    let word = |id: usize, count: usize, v_ref: &Vec<i32>| {
        for i in 0..count {
            println!("Thread {}: Work iteration {}: {:?}", id, i + 1, v_ref);
            thread::sleep(Duration::from_secs(1));
            if id == 2 && i == 5 {
                panic!("This thread panicked!");
            }
        }
        println!("Thread {} finished their work.", id);
    };

    let v = vec![1, 2, 3];

    thread::scope(|s| {
        let _v = v.clone();
        s.spawn(move || {
            word(1, 10, &_v);
        });
        let _v = v.clone();
        s.spawn(move || {
            word(2, 10, &_v);
        });
        // 当scope闭包结束，所有线程自动join
    })

    // 当有线程触发panic，其他线程继续执行，线程作用域最后会触发panic
}
```

#### 管道（Channel ）

```rust
use std::thread;
use std::time::Duration;

use crossbeam_channel::{bounded, Receiver, Sender};

fn main() {
    // 创建一个可以一次容纳1个消息的通道
    // 无限量通道使用 unbounded
    let (tx, rx) = bounded(1);

    thread::scope(|s| {
        s.spawn(move || { producer(tx) });
        s.spawn(move || { consumer(rx) });
    })
}

fn producer(tx: Sender<String>) {
    for i in 0..10 {
        // 发送数据到通道
        println!("{:?} 生产者 => {}", thread::current().id(), i);
        // i的所有权已经转移发送出去
        tx.send(i.to_string()).unwrap();
        // 模拟一些工作
        thread::sleep(Duration::from_millis(100));
    }
    // 发送一个特殊值来通知消费者线程停止
    tx.send("生产者：我好了".to_string()).unwrap();
    println!("生产者完成任务");
}

fn consumer(rx: Receiver<String>) {
    loop {
        println!("{:?} 接收者 等待...", thread::current().id());
        // 从管道中获取消息
        if let Ok(value) = rx.recv() {
            println!("{:?} 接收者 <= {}", thread::current().id(), value);
            // 接收到特殊值，正常关闭接收，避免无限等待下去
            if "生产者：我好了".eq(&value) {
                break;
            }
        }
        // 等待一下再重试
        thread::sleep(Duration::from_millis(100));
    }
    println!("消费者完成任务");
}
```

#### 线程池（ThreadPool）

[![threadpool-badge](https://badge-cache.kominick.com/crates/v/threadpool.svg?label=threadpool)](https://docs.rs/threadpool/) [![num_cpus-badge](https://badge-cache.kominick.com/crates/v/num_cpus.svg?label=num_cpus)](https://docs.rs/num_cpus/)

```rust
use std::thread;
use std::time::Duration;
use threadpool::ThreadPool;

fn main() {
    let n_workers = 4;
    let n_jobs = 8;
    let cpus = num_cpus::get();// 使用系统的内核数
    let pool = ThreadPool::new(cpus);

    for _ in 0..n_workers {
        pool.execute(move || {
            for i in 0..n_jobs {
                println!("Thread {:?}: Work iteration {}", thread::current().id(), i);
                thread::sleep(Duration::from_secs(1));
            }
            println!("Thread {:?} finished their work.", thread::current().id());
        });
    }
    pool.join();
}
```

### 数据并行

[![rayon-badge](https://badge-cache.kominick.com/crates/v/rayon.svg?label=rayon)](https://docs.rs/rayon/)

#### 并行改变数组中的元素

```rust
use rayon::prelude::{IntoParallelRefMutIterator, ParallelIterator};

fn main() {
    let mut arr = [0, 7, 9, 11];
    // 获取并行迭代器
    arr.par_iter_mut()
        // 对数组的每个元素并行更新值
        .for_each(|p| *p += 1);
    // [1, 8, 10, 12]
    println!("{:?}", arr);
}
```

#### 并行测试数组中的元素

```rust
use rayon::prelude::{IntoParallelRefIterator, ParallelIterator};

fn main() {
    let mut vec = vec![2, 4, 6, 8];
    println!("{:?}", vec);

    println!("存在奇数：{}", vec.par_iter().any(|n| (*n % 2) != 0));
    println!("都是偶数：{}", vec.par_iter().all(|n| (*n % 2) == 0));
    println!("有一个数大于8：{}", vec.par_iter().any(|n| *n > 8));
    println!("所有数都小于8：{}", vec.par_iter().all(|n| *n <= 8));

    vec.push(9);
    println!("{:?}", vec);

    println!("存在奇数：{}", vec.par_iter().any(|n| (*n % 2) != 0));
    println!("都是偶数：{}", vec.par_iter().all(|n| (*n % 2) == 0));
    println!("有一个数大于8：{}", vec.par_iter().any(|n| *n > 8));
    println!("所有数都小于8：{}", vec.par_iter().all(|n| *n <= 8));
}
```

#### 并行检索

```rust
use rayon::prelude::{IntoParallelRefIterator, ParallelIterator};

fn main() {
    let v = vec![6, 2, 1, 9, 3, 8, 11];
    println!("{:?}", v);
    println!("有一个数9：{:?}", v.par_iter().find_any(|&&x| x == 9));
    println!("有一个大于6的偶数：{:?}", v.par_iter().find_any(|&&x| x % 2 == 0 && x > 6));
    println!("有一个大于8的数：{:?}", v.par_iter().find_any(|&&x| x > 8));
}
```

#### 并行排序

```rust
use rand::prelude::ThreadRng;
use rayon::prelude::ParallelSliceMut;
use rand::Rng;

fn main() {
    let mut rng: ThreadRng = rand::thread_rng();
    let mut vec: [u8; 100] = [0; 100];
    for v in vec.iter_mut() {
        *v = rng.gen::<u8>();
    }
    println!("{:?}", &vec);
    // 并行排序
    vec.par_sort_unstable();
    println!("{:?}", &vec);
}
```

## 密码学

[![ring-badge](https://badge-cache.kominick.com/crates/v/ring.svg?label=ring)](https://briansmith.org/rustdoc/ring/) [![data-encoding-badge](https://badge-cache.kominick.com/crates/v/data-encoding.svg?label=data-encoding)](https://docs.rs/data-encoding/) 

### 哈希

```rust
// 引入标准库中的IO模块，用于处理缓冲读取  
use std::io::{BufReader, Read};
// 引入data_encoding库中的HEXUPPER编码，用于将字节转换为大写十六进制字符串  
use data_encoding::HEXUPPER;
// 引入ring库的digest模块，用于SHA256哈希计算  
use ring::digest::{Context, SHA256};

fn main() {
    // 定义一个字符串"hello"  
    let str = "hello";
    // 创建一个BufReader来读取字符串的字节表示  
    let mut reader = BufReader::new(str.as_bytes());

    // 创建一个新的SHA256哈希上下文  
    let mut context = Context::new(&SHA256);
    // 创建一个缓冲区，用于存储从reader中读取的数据  
    let mut buffer = [0; 1024];

    // 循环读取数据，直到reader中没有更多数据  
    loop {
        // 尝试从reader中读取数据到buffer中，并返回读取的字节数  
        let count = reader.read(&mut buffer).unwrap();
        // 如果没有读取到任何数据（即已经到达字符串末尾），则退出循环  
        if count == 0 { break; }
        // 使用读取到的数据（buffer中的前count个字节）更新哈希上下文  
        // 分段计算摘要值  
        context.update(&buffer[..count]);
    }

    // 完成哈希计算，并获取最终的摘要值  
    let digest = context.finish();

    // 将摘要值的字节表示转换为大写十六进制字符串  
    // 将bytes转为HEX编码  
    let s = HEXUPPER.encode(digest.as_ref());

    // 打印出大写十六进制表示的摘要值  
    // 应该输出：2CF24DBA5FB0A30E26E83B2AC5B9E29E1B161E5C1FA7425E73043362938B9824  
    println!("{}", s);
}
```

### 签名

HMAC（Hash-based Message Authentication Code）是一种基于哈希算法和密钥的消息认证码算法。

```rust
// 引入ring库中的hmac和rand模块
use ring::{hmac, rand};
// 引入ring库中rand模块的SecureRandom trait
use ring::rand::SecureRandom;

fn main() {
    // 定义一个待签名的字符串
    let str = "hello";

    // 创建一个用于HMAC密钥的字节数组，大小为48字节
    let mut key_value = [0u8; 48];

    // 创建一个SystemRandom对象，它是SecureRandom trait的一个实现，用于生成随机数
    let rng = rand::SystemRandom::new();

    // 使用rng填充key_value数组，生成一个随机的密钥
    rng.fill(&mut key_value).unwrap();

    // 打印生成的密钥
    println!("{:?}", &key_value);

    // 使用HMAC_SHA256算法和生成的密钥值创建一个HMAC密钥对象
    let key = hmac::Key::new(hmac::HMAC_SHA256, &key_value);

    // 使用HMAC密钥对字符串进行签名
    let sign = hmac::sign(&key, str.as_bytes());

    // 打印签名结果
    println!("{:?}", sign);

    // 验证签名，检查原始数据是否被篡改
    let result = hmac::verify(&key, str.as_bytes(), sign.as_ref());

    // 根据验证结果打印相应的消息
    match result {
        Ok(_) => {
            println!("签名验证通过，信息没有被篡改")
        },
        Err(e) => {
            println!("签名异常:{:?}", e)
        }
    }
}
```

### 加密

PBKDF2是一种基于密码的密钥派生函数，它可以将密码和盐值作为输入，通过哈希函数和迭代次数生成一个固定的哈希值，从而对密码进行哈希处理。

```rust
// 引入std库中的NonZeroU32类型，它表示一个非零的U32整数
use std::num::NonZeroU32;

// 引入data_encoding库中的HEXUPPER编码器，用于将字节切片编码为十六进制大写字符串
use data_encoding::HEXUPPER;

// 引入ring库中的digest、pbkdf2和rand模块
use ring::{digest, pbkdf2, rand};
// 引入ring库中rand模块的SecureRandom trait
use ring::rand::SecureRandom;

fn main() {
    // 定义一个常量CREDENTIAL_LEN，表示PBKDF2生成的哈希值的长度，这里使用SHA512的输出长度
    const CREDENTIAL_LEN: usize = digest::SHA512_OUTPUT_LEN;

    // 创建一个NonZeroU32对象，表示PBKDF2的迭代次数，这里设置为100,000次
    let n_iter = NonZeroU32::new(100_000).unwrap();

    // 创建一个SystemRandom对象，用于生成随机数
    let rng = rand::SystemRandom::new();

    // 创建一个字节切片salt，用于PBKDF2的盐值，初始化为全0
    let mut salt = [0u8; CREDENTIAL_LEN];
    // 使用rng填充salt，生成一个随机的盐值
    rng.fill(&mut salt).unwrap();

    // 定义一个待加密的密码字符串
    let password = "这是个很强的密码";
    // 创建一个字节切片pbkdf2_hash，用于存储PBKDF2生成的哈希值
    let mut pbkdf2_hash = [0u8; CREDENTIAL_LEN];

    // 使用PBKDF2算法对密码进行哈希处理，并将结果存储在pbkdf2_hash中
    pbkdf2::derive(
        pbkdf2::PBKDF2_HMAC_SHA512, // 使用HMAC-SHA512作为PBKDF2的哈希函数
        n_iter,                     // 迭代次数
        &salt,                      // 盐值
        password.as_bytes(),        // 待加密的密码
        &mut pbkdf2_hash,           // 存储哈希值的字节切片
    );

    // 将salt编码为十六进制大写字符串并打印
    println!("Salt: {}", HEXUPPER.encode(&salt));
    // 将pbkdf2_hash编码为十六进制大写字符串并打印
    println!("PBKDF2 hash: {}", HEXUPPER.encode(&pbkdf2_hash));

    // 验证哈希值是否与原始密码匹配
    let result = pbkdf2::verify(
        pbkdf2::PBKDF2_HMAC_SHA512, // 使用HMAC-SHA512作为PBKDF2的哈希函数
        n_iter,                     // 迭代次数
        &salt,                      // 盐值
        password.as_bytes(),        // 待验证的密码
        &pbkdf2_hash,               // 存储哈希值的字节切片
    );
    // 打印验证结果，is_ok()方法检查Result是否为Ok
    println!("密码:[{}]解密结果={}", password, result.is_ok());

    // 尝试使用一个不同的密码进行验证
    let password = "這是個很强的密碼";
    // 验证哈希值是否与新的密码匹配
    let result = pbkdf2::verify(
        pbkdf2::PBKDF2_HMAC_SHA512, // 使用HMAC-SHA512作为PBKDF2的哈希函数
        n_iter,                     // 迭代次数
        &salt,                      // 盐值
        password.as_bytes(),        // 待验证的密码
        &pbkdf2_hash,               // 存储哈希值的字节切片
    );
    // 打印验证结果，is_ok()方法检查Result是否为Ok
    println!("密码:[{}]解密结果={}", password, result.is_ok());
}
```

## 数据结构

### 位域

```rust
use bitflags::bitflags;

pub struct Auth(u8);

bitflags! {
    // Auth 结构体扩展了位标志功能，底层使用 u8 存储
    impl Auth:u8{
        // READ 权限，对应二进制第1位
        const READ    = 1 << 0;
        // WRITE 权限，对应二进制第2位
        const WRITE   = 1 << 1;
        // EXECUTE 权限，对应二进制第3位
        const EXECUTE = 1 << 2;

        // 定义一个占位符，用于表示所有位都被设置的掩码
        // 目的为了与外部源兼容
        const _ = !0;
    }
}

fn main() {
    // 所有位都开启
    // all:11111111
    println!("all:{:8b}", Auth::all());
    // 没有任何位都关闭
    // empty:       0
    println!("empty:{:8b}", Auth::empty());

    let mut p = Auth::all();
    // 移除 READ 权限 [a & !b]
    p.remove(Auth::READ);
    // remove:11111110
    println!("remove:{:8b}", p.bits());
    // 添加 READ 权限 [a | b]
    p.insert(Auth::READ);
    // insert:11111111
    println!("insert:{:8b}", p.bits());
    // 切换 EXECUTE 权限 [a ^ b]
    p.toggle(Auth::EXECUTE);
    // toggle:11111011
    println!("toggle:{:8b}", p.bits());

    // 设置 EXECUTE 权限为开启状态
    p.set(Auth::EXECUTE, true);
    // set:11111111
    println!("set:{:8b}", p.bits());

    p = Auth::all();
    // 获取 READ 和 ALL 的交集 [a & (b != 0)]
    p = p.intersection(Auth::READ);
    // intersection:       1
    println!("intersection:{:8b}", p.bits());
    // 获取 READ 和 WRITE 的并集 [a | b]
    p = p.union(Auth::WRITE);
    // union:      11
    println!("union:{:8b}", p.bits());
    // 获取 WRITE 和 READ 的差集 [a & !b]
    p = p.difference(Auth::READ);
    // difference:      10
    println!("difference:{:8b}", p.bits());
    // 获取 READ 和 WRITE 的对称差集 [a ^ b]
    p = p.symmetric_difference(Auth::READ);
    // symmetric_difference:      11
    println!("symmetric_difference:{:8b}", p.bits());
    // 获取 ALL 的补集 [!a]
    p = p.complement();
    // complement:11111100
    println!("complement:{:8b}", p.bits());

    // from_bits:       1
    println!("from_bits:{:8b}", Auth::from_bits(0b001).unwrap());
    // from_name:       1
    println!("from_name:{:8b}", Auth::from_name("READ").unwrap());
    // contains:true
    println!("contains:{}", Auth::all().contains(Auth::READ));
}
```

## 数据库

### SQLite

[![rusqlite-badge](https://badge-cache.kominick.com/crates/v/rusqlite.svg?label=rusqlite)](https://crates.io/crates/rusqlite/)

[SQLite Download Page](https://www.sqlite.org/download.html)，下载 dll 和 tools，然后将 sqlite3.def 编译为 sqlite3.lib，将 lib 和 dll 放入 Rust 项目根目录。

```powershell
lib /DEF:sqlite3.def /OUT:sqlite3.lib /MACHINE:x64
```

#### 基本SQL交互

```rust
use mysql::params;
use rusqlite::{Connection, params};

fn main() {
    // 创建打开一个数据库文件
    let conn = Connection::open("data.db").unwrap();
    // 创建数据库
    let _ = conn.execute("DROP TABLE person", []);
    // 创建数据表
    let _ = conn.execute(
        r"CREATE TABLE person (
            id int primary key,
            name text not null
        )", []);

    // 插入数据
    conn.execute(
        r"INSERT INTO person (id, name)
          VALUES (?1, ?2), (?3, ?4);",
        params![1, "张三", 2, "李四"]).unwrap();

    // 查询数据
    let mut s = conn.prepare("SELECT id, name FROM person").unwrap();
    // 执行查询，没有参数，返回结果的处理
    let ps = s.query_map([], |row| {
        Ok(Person {
            id: row.get(0).unwrap(),
            name: row.get(1).unwrap(),
        })
    }).unwrap();

    for p in ps {
        let _p = p.unwrap();
        println!("[{},{}]", _p.id, _p.name);
    }
}

struct Person {
    id: i32,
    name: String,
}
```

#### 事务

```rust
use rusqlite::{Connection, TransactionBehavior};

fn main() {
    // 创建打开一个数据库文件
    let mut conn = Connection::open("db/data.db").unwrap();

    // 开启事务
    // Deferred 不会立即执行，直到需要访问时
    // Immediate 立即开始写入操作
    // Exclusive 事务存在期间阻止其他数据库链接访问
    let tx = conn.transaction_with_behavior(TransactionBehavior::Deferred).unwrap();
    let name = {
        // 第一步获取id=2的姓名
        let mut s = tx.prepare("SELECT name FROM person WHERE id = 2").unwrap();
        let mut rows = s.query([]).unwrap();
        let row = rows.next().unwrap().unwrap();
        row.get(0).unwrap()
    };

    // 第二步删除id=2的记录
    tx.execute("DELETE FROM person WHERE id = 2", []).unwrap();
    // 第三步修改id=1的姓名
    tx.execute("UPDATE person SET name = ?1 WHERE id = ?2", [&name, &"1".to_string()]).unwrap();

    // 提交事务
    tx.commit().unwrap();
}
```

### MySQL

[![mysql](https://badge-cache.kominick.com/crates/v/mysql.svg?label=mysql)](https://crates.io/crates/mysql/)

#### 简单示例

```rust
use mysql::{Conn, OptsBuilder, params};
use mysql::prelude::Queryable;

fn main() {
    let opts = OptsBuilder::new()
        .user(Some("user"))
        .pass(Some("password"))
        // .ip_or_hostname(Some("192.168.0.111"))
        .ip_or_hostname(Some("[2409:8a0c:2e71:b14::1003]"))
        .tcp_port(3306)
        .db_name(Some("person"));

    // 获取链接
    let mut conn = Conn::new(opts).unwrap();

    // 创建数据表
    conn.query_drop(
        r"CREATE TABLE person (
            id int primary key,
            name text not null
        )").unwrap();

    let persons = vec![
        Person { id: 1, name: "张三".to_string() },
        Person { id: 2, name: "李四".to_string() },
    ];
    // 插入数据
    conn.exec_batch(
        r"INSERT INTO person (id, name)
        VALUES (:id, :name)",
        persons.iter().map(|p| params! {
            "id"=>p.id,
            "name"=>p.name.as_str(),
    })).unwrap();

    // 查询数据
    let ps = conn.query_map(
        "SELECT id, name from person",
        |(id, name)| {
            Person { id, name }
        },
    ).unwrap();
    println!("{:?}", ps);
}

#[derive(Debug)]
struct Person {
    id: i32,
    name: String,
}
```

#### 事务

```rust
use mysql::{Conn, OptsBuilder, TxOpts};
use mysql::prelude::Queryable;

fn main() {
    let opts = OptsBuilder::new()
        .user(Some("user"))
        .pass(Some("password"))
        // .ip_or_hostname(Some("192.168.0.111"))
        .ip_or_hostname(Some("[2409:8a0c:2e71:b14::1003]"))
        .tcp_port(3306)
        .db_name(Some("person"));

    // 获取链接
    let mut conn = Conn::new(opts).unwrap();

    // 开启事务
    let mut tx = conn.start_transaction(TxOpts::default()).unwrap();
    // 第一步获取id=2的姓名
    let name: String = tx.query_first("SELECT name FROM person WHERE id = 2").unwrap().unwrap();
    // 第二步删除id=2的记录
    tx.exec_drop("DELETE FROM person WHERE id = (?)", ("2", )).unwrap();
    // 第三步修改id=1的姓名
    tx.exec_drop("UPDATE person SET name = (?) WHERE id = (?)", (name.as_str(), "1", )).unwrap();

    // 提交事务
    tx.commit().unwrap();
}

#[derive(Debug)]
struct Person {
    id: i32,
    name: String,
}
```

### sea-orm

[![sea-orm](https://badge-cache.kominick.com/crates/v/sea-orm.svg?label=sea-orm)](https://crates.io/crates/sea-orm/)

[Introduction - SeaORM Tutorials (sea-ql.org)](https://www.sea-ql.org/sea-orm-tutorial/)

#### 链接和创建数据库

```rust
use sea_orm::{ConnectionTrait, Database, DbBackend, DbErr, Statement};

const DATABASE_URL: &str = "mysql://user:password@192.168.0.111:3306";
const DB_NAME: &str = "person";

#[tokio::main]
async fn main() -> Result<(), DbErr> {
    // 链接数据库主机
    let db = Database::connect(DATABASE_URL).await.unwrap();

    // 创建数据库
    let db = &match db.get_database_backend() {
        DbBackend::MySql => {
            db.execute(Statement::from_string(
                db.get_database_backend(),
                format!("CREATE DATABASE IF NOT EXISTS `{}`;", DB_NAME),
            )).await.unwrap();
            let url = format!("{}/{}", DATABASE_URL, DB_NAME);
            Database::connect(&url).await.unwrap();
        }
        DbBackend::Postgres => {}
        DbBackend::Sqlite => {}
    };

    Ok(())
}
```

#### 生成数据库实体结构体

```
sea-orm-cli generate entity -u mysql://user:password@192.168.0.111:3306/person -o src/entity
```

```rust
// src/entity/person.rs
//! `SeaORM` Entity. Generated by sea-orm-codegen 0.12.15

use sea_orm::entity::prelude::*;

#[derive(Clone, Debug, PartialEq, DeriveEntityModel, Eq)]
#[sea_orm(table_name = "person")]
pub struct Model {
    #[sea_orm(primary_key, auto_increment = false)]
    pub id: i32,
    #[sea_orm(column_type = "Text")]
    pub name: String,
}

#[derive(Copy, Clone, Debug, EnumIter, DeriveRelation)]
pub enum Relation {}

impl ActiveModelBehavior for ActiveModel {}
```

```rust
// src/entity/prelude.rs
//! `SeaORM` Entity. Generated by sea-orm-codegen 0.12.15

pub use super::person::Entity as Person;
```

```rust
// src/entity/mod.rs
//! `SeaORM` Entity. Generated by sea-orm-codegen 0.12.15

pub mod prelude;

pub mod person;
```

#### 增删改查

```rust
use sea_orm::*;
use Stu::entity::person::{ActiveModel, Column};
use Stu::entity::prelude::Person;

const DATABASE_URL: &str = "mysql://user:password@192.168.0.111:3306";
const DB_NAME: &str = "person";

#[tokio::main]
async fn main() -> Result<(), DbErr> {
    // 链接数据库主机
    let db = Database::connect(DATABASE_URL).await.unwrap();

    // 创建数据库
    let db = &match db.get_database_backend() {
        DbBackend::MySql => {
            db.execute(Statement::from_string(
                db.get_database_backend(),
                format!("CREATE DATABASE IF NOT EXISTS `{}`;", DB_NAME),
            )).await.unwrap();
            let url = format!("{}/{}", DATABASE_URL, DB_NAME);
            Database::connect(&url).await.unwrap()
        }
        DbBackend::Postgres => { db }
        DbBackend::Sqlite => { db }
    };

    // 插入数据
    let p1 = Person::insert(ActiveModel {
        id: ActiveValue::Set(1),
        name: ActiveValue::Set("张三".to_string()),
    }).exec(db).await.unwrap();
    let p2 = Person::insert(ActiveModel {
        id: ActiveValue::Set(2),
        name: ActiveValue::Set("李四".to_string()),
    }).exec(db).await.unwrap();

    // 更新数据
    ActiveModel {
        id: ActiveValue::Set(p2.last_insert_id),
        name: ActiveValue::Set("王五".to_string()),
    }.update(db).await.unwrap();

    // 查找数据
    let ps = Person::find().all(db).await.unwrap();
    println!("{:?}", ps);
    let p = Person::find_by_id(2).one(db).await.unwrap();
    println!("{:?}", p);
    let p = Person::find()
        .filter(Column::Name.eq("张三"))
        .one(db).await.unwrap();
    println!("{:?}", p);

    // 删除数据
    let p = ActiveModel {
        id: ActiveValue::Set(p1.last_insert_id),
        ..Default::default()
    };
    p.delete(db).await.unwrap();
    Ok(())
}
```

## 时间和日期

[![chrono-badge](https://badge-cache.kominick.com/crates/v/chrono.svg?label=chrono)](https://docs.rs/chrono/)

### 创建计时器

```rust
use std::thread;
use std::time::{Duration, Instant};

fn main() {
    let start = Instant::now();
    println!("比赛开始...");
    thread::sleep(Duration::from_secs(3));
    let time = start.elapsed();
    println!("第一圈，用时={:?}", time);

    thread::sleep(Duration::from_secs(4));
    let time = start.elapsed();
    println!("第二圈，用时={:?}", time);

    thread::sleep(Duration::from_secs(5));
    let time = start.elapsed();
    println!("第三圈，用时={:?}", time);
    println!("比赛结束...");
}
```

### 时间

```rust
use std::ops::Add;
use chrono::{DateTime, Local, NaiveDate, TimeDelta, TimeZone, Utc, Weekday};

fn main() {
    // 获取当前时间
    let time = Utc::now();
    println!("{}", time);
    let time = Local::now();
    println!("{}", time);

    // 2024-01-01 01:01:01 UTC
    // MappedLocalTime:是不确定的时间，可能存在歧义，类似于Option注解
    let time = Utc.with_ymd_and_hms(2024, 1, 1, 1, 1, 1).unwrap();
    println!("{}", time);
    let time = NaiveDate::from_ymd_opt(2024, 1, 1).unwrap()
        .and_hms_opt(1, 1, 1).unwrap()
        .and_utc();
    println!("{}", time);

    // 2024年1月1日向后推280天，当天默认为第1天
    let time = NaiveDate::from_yo_opt(2024, 280).unwrap()
        // and_hms_milli_opt 毫秒
        // and_hms_micro_opt 微秒
        // 纳秒
        .and_hms_nano_opt(1, 1, 1, 999_999_999).unwrap();
    // 2024-10-06 01:01:01.999999999
    println!("{}", time);

    // 2024年1月1日向后推40周的星期六，当周为第1周
    let time = NaiveDate::from_isoywd_opt(2024, 40, Weekday::Sat).unwrap();
    // 2024-10-05
    println!("{}", time);

    // 时间差
    let dt1 = Utc.with_ymd_and_hms(2014, 1, 1, 1, 1, 1).unwrap();
    let dt2 = Utc.with_ymd_and_hms(2024, 10, 10, 10, 10, 10).unwrap();
    let mut td = dt2.signed_duration_since(dt1);
    println!("{}", td.num_days());// 3935
    td += TimeDelta::hours(200);// 加200小时
    println!("{}", td);// PT340736949S
    td -= TimeDelta::days(2000);// 减2000天
    println!("{}", td);// PT167936949S
    println!("{}", dt1.add(td).to_rfc3339());// 2019-04-28T18:10:10+00:00
}
```

### 时区转换

[![chrono-tz](https://badge-cache.kominick.com/crates/v/chrono-tz.svg?label=chrono-tz)](https://docs.rs/chrono-tz/)

```rust
use chrono::{FixedOffset, TimeZone, Utc};

fn main() {
    let east_eight_time = FixedOffset::east_opt(8 * 3600).unwrap()
        .with_ymd_and_hms(2024, 1, 1, 1, 1, 1).unwrap();

    let utc_time = east_eight_time.with_timezone(&Utc);

    let west_eight_time = east_eight_time.with_timezone(
        &FixedOffset::west_opt(8 * 3600).unwrap()
    );

    // 东八区时间: 2024-01-01 01:01:01 +08:00
    println!("东八区时间: {}", east_eight_time);
    // UTC时间: 2023-12-31 17:01:01 UTC
    println!("UTC时间: {}", utc_time);
    // 西八区时间: 2023-12-31 09:01:01 -08:00
    println!("西八区时间: {}", west_eight_time);
}
```

```rust
use std::str::FromStr;
use chrono::{TimeZone, Utc};
use chrono_tz::Tz;
use chrono_tz::Tz::America__Los_Angeles;

fn main() {
    let east_eight_time = Tz::from_str("Asia/Shanghai").unwrap()
        .with_ymd_and_hms(2024, 1, 1, 1, 1, 1).unwrap();

    let utc_time = east_eight_time.with_timezone(&Utc);

    let west_eight_time = utc_time.with_timezone(&America__Los_Angeles);

    // 东八区时间: 2024-01-01 01:01:01 CST
    println!("东八区时间: {}", east_eight_time);
    // UTC时间: 2023-12-31 17:01:01 UTC
    println!("UTC时间: {}", utc_time);
    // 西八区时间: 2023-12-31 09:01:01 PST
    println!("西八区时间: {}", west_eight_time);
}
```

### 格式化

[chrono::format::strftime - Rust (docs.rs)](https://docs.rs/chrono/latest/chrono/format/strftime/index.html#specifiers)

```rust
use chrono::{DateTime, FixedOffset, Locale, Timelike, TimeZone, Utc};

fn main() {
    let time = FixedOffset::east_opt(8 * 3600).unwrap()
        .with_ymd_and_hms(2024, 1, 1, 1, 1, 1).unwrap()
        .with_nanosecond(123456789).unwrap();

    // 2024-01-01 01:01:01.123456789 +08:00
    println!("{}", time.to_string());
    // Mon, 1 Jan 2024 01:01:01 +0800
    println!("{}", time.to_rfc2822());
    // 2024-01-01T01:01:01.123456789+08:00
    println!("{}", time.to_rfc3339());
    println!("{:?}", time);
    
    // 2024-01-01 01:01:01.123456789
    println!("{}", time.format("%Y-%m-%d %H:%M:%S%.9f").to_string());
    // Mon Jan  1 01:01:01 2024
    println!("{}", time.format("%a %b %e %T %Y").to_string());
    // 2024년 01월 01일 (월) 오전 01시 01분 01초
    println!("{}", time.format_localized("%c", Locale::ko_KR).to_string());

    let str = "2024-01-02T03:04:05.123456789+08:00";
    // 2024-01-02 03:04:05.123456789 +08:00
    println!("{}", str.parse::<DateTime<FixedOffset>>().unwrap().to_string());
    // Tue, 2 Jan 2024 03:04:05 +0800
    println!("{}", DateTime::parse_from_rfc3339(str).unwrap().to_rfc2822());
    // 2024-01-02 03:04:05.123456789 +08:00
    println!("{}", DateTime::parse_from_str(str, "%+").unwrap());
}
```

### 时间戳

```rust
use chrono::{DateTime, FixedOffset, Timelike, TimeZone, Utc};

fn main() {
    let now = Utc::now();
    // 返回从1970年元旦0点整到现在的非闰秒数
    println!("{}", now.timestamp());
    // 返回从1970年元旦0点整到现在的非闰纳秒数
    println!("{}", now.timestamp_nanos_opt().unwrap());
    let dt = DateTime::from_timestamp(now.timestamp(), now.timestamp_subsec_nanos()).unwrap();
    println!("{}", dt);

    let time = FixedOffset::east_opt(8 * 3600).unwrap()
        .with_ymd_and_hms(2024, 1, 1, 1, 1, 1).unwrap()
        .with_nanosecond(123456789).unwrap();

    // 1704042061
    println!("{}", time.timestamp());
    // 1704042061123456789
    println!("{}", time.timestamp_nanos_opt().unwrap());

    let time = DateTime::from_timestamp(1704042061, 123456789).unwrap();
    // 2023-12-31T17:01:01.123456789+00:00
    println!("{}", time.to_rfc3339());
}
```

## 开发工具

### 日志

#### log4rs

[![log-badge](https://badge-cache.kominick.com/crates/v/log.svg?label=log)](https://docs.rs/log/)[![log4rs-badge](https://badge-cache.kominick.com/crates/v/log4rs.svg?label=log4rs)](https://docs.rs/log4rs/)

```yaml
# 每隔30秒扫描此文件以查看更改
refresh_rate: 30 seconds

# loggers 部分定义了如何为特定的模块或路径配置日志
loggers:
  # 必须与模块的路径名相同
  a::b:
    # 设置日志级别为trace，这意味着将记录所有trace级别及以上的日志
    level: trace
    # appenders 定义了哪些appender将用于此logger
    appenders:
      - console
    # additive 为true表示日志消息不仅发送到此logger的appender，还发送到父logger的appender
    additive: true

# 对全局 log 进行配置
root:
  # 如果取消注释，这将设置全局日志级别为info
  # level: info
  # appenders 定义了哪些appender将用于全局日志
  appenders:
    - console
    - file
    - rolling_file

# appender 部分定义了如何将日志消息收集到控制台或文件等
appenders:
  # 定义一个console appender，将日志输出到stdout
  console:
    kind: console
    target: stdout
    # 编码器定义了日志消息的格式
    encoder:
      pattern: "[{d(%Y-%m-%dT%H:%M:%S%.9f)} {h({l}):<5.5} {M}] {m}{n}"
    # 过滤器定义了哪些级别的日志应该被处理
    filters:
      - kind: threshold
        level: trace

  # 定义一个file appender，将日志写入到指定的文件
  file:
    kind: file
    path: "log/file.log"
    # 如果文件已存在，append为true表示追加日志到文件末尾，而不是覆盖它
    append: true
    encoder:
      pattern: "[{d(%Y-%m-%dT%H:%M:%S%.9f)} {h({l}):<5.5} {M}] {m}{n}"

  # 定义一个rolling_file appender，当文件达到一定条件时，会滚动（即创建新文件）
  rolling_file:
    kind: rolling_file
    path: "log/rolling_file.log"
    encoder:
      pattern: "[{d(%Y-%m-%dT%H:%M:%S%.9f)} {h({l}):<5.5} {M}] {m}{n}"
    policy:
      # 触发滚动文件的条件
      trigger:
        # 这里使用了时间触发，而不是大小触发
        # kind: size
        # interval: 10mb
        kind: time
        # 每1分钟滚动一次文件
        interval: 1 minute
      # 滚动时如何命名和创建新文件的策略
      roller:
        kind: fixed_window
        # 滚动文件的命名模式，{}将被替换为序列号
        pattern: "log/compressed-log-{}-.log"
        # 序列号的起始值
        base: 0
        # 保留的最大文件数
        count: 2
```

```rust
use log::Level;
use std::thread;
use log::{debug, error, info, log, trace, warn};
use log4rs::init_file;

fn main() {
    // 加载配置
    init_file("config.yaml", Default::default()).unwrap();

    thread::scope(|s| {
        for _ in 1..=10 {
            s.spawn(|| {
                let id = thread::current().id();
                // RUST_LOG=trace
                trace!("{:02?}:追踪日志: {}",id,"trace");
                debug!("{:02?}:调试日志: {}",id, "debug");
                info!("{:02?}:信息日志: {}",id, "info");
                warn!("{:02?}:警告日志: {}",id,"warn");
                error!("{:02?}:错误日志: {}",id,"error");

                log!(Level::Info,"{:02?}:信息日志: {}",id,"log");
            });
        }
    });
}
```

### 版本控制

[![semver-badge](https://badge-cache.kominick.com/crates/v/semver.svg?label=semver)](https://docs.rs/semver/)

#### 语义化版本

[语义化版本 2.0.0 | Semantic Versioning (semver.org)](https://semver.org/lang/zh-CN/)

[Specifying Dependencies - The Cargo Book (rust-lang.org)](https://doc.rust-lang.org/cargo/reference/specifying-dependencies.html)

```rust
use std::str::FromStr;
use semver::{BuildMetadata, Prerelease, Version, VersionReq};

fn main() {
    // 版本要求
    let req = VersionReq::parse(">=1.2.3, <1.8.0").unwrap();

    // 1.2.3-alpha.1
    let version = Version {
        major: 1,
        minor: 2,
        patch: 3,
        pre: Prerelease::new("alpha.1").unwrap(),
        build: BuildMetadata::EMPTY,
    };
    println!("{}:{}", version, req.matches(&version));

    // 1.3.0
    let version = Version::parse("1.3.0").unwrap();
    println!("{}:{}", version, req.matches(&version));

    let req = Version::parse("1.0.49-125+g72ee7853").unwrap();
    let version = Version {
        major: 1,
        minor: 0,
        patch: 49,
        pre: Prerelease::from_str("125").unwrap(),
        build: BuildMetadata::from_str("g72ee7853").unwrap(),
    };
    // 1.0.49-125+g72ee7853
    println!("{}", version);
    println!("{}", version.to_string().eq(&req.to_string()));
}
```

## 编码

### 字符集

#### URL编码

[![percent-encoding-badge](https://badge-cache.kominick.com/crates/v/percent-encoding.svg?label=percent-encoding)](https://docs.rs/percent-encoding/latest/percent_encoding)

```rust
use percent_encoding::{AsciiSet, CONTROLS, percent_decode, utf8_percent_encode};

// 非ASCII和控制字符需要转码，还可以添加需要转码的字符
const FRAGMENT: &AsciiSet = &CONTROLS.add(b' ').add(b'"').add(b'<').add(b'>').add(b'`');

fn main() {
    let str = utf8_percent_encode(
        r#"fs://example.com/search?key1=value1&key2=<"你 好">&key3=123,456"#, FRAGMENT,
    ).to_string();
    // fs://example.com/search?key1=value1&key2=%3C%22%E4%BD%A0%20%E5%A5%BD%22%3E&key3=123,456
    println!("{}", str);

    let url = percent_decode(str.as_bytes());
    // fs://example.com/search?key1=value1&key2=<"你 好">&key3=123,456
    println!("{}", url.decode_utf8().unwrap());
}
```

#### 表单数据格式

application/x-www-form-urlencoded

[![url-badge](https://badge-cache.kominick.com/crates/v/url.svg?label=url)](https://docs.rs/url/)

```rust
use std::collections::HashMap;
use url::form_urlencoded;

fn main() {
    let mut form = HashMap::new();
    form.insert("username".to_string(), "张 三".to_string());
    form.insert("password".to_string(), "3CfeD9Vxra".to_string());
    let params = form_urlencoded::Serializer::new(String::new())
        .extend_pairs(form.iter())
        .finish();
    // password=3CfeD9Vxra&username=%E5%BC%A0+%E4%B8%89
    println!("{}", params);

    let vec: Vec<_> = form_urlencoded::parse(params.as_bytes()).collect();
    // [("password", "3CfeD9Vxra"), ("username", "张 三")]
    println!("{:?}", vec);
}
```

#### Base64编码

[![data-encoding-badge](https://badge-cache.kominick.com/crates/v/data-encoding.svg?label=data-encoding)](https://docs.rs/data-encoding/)

```rust
use data_encoding::{BASE64, BASE64_MIME, BASE64_NOPAD, BASE64URL};

fn main() {
    let str = "月下微风起，静夜思悠长。星辰点点亮，流水轻轻淌。梦回古道长，思绪随风扬。人生如梦幻，珍惜每一光.";
    // 最后两个字符 + /
    let b64 = BASE64.encode(str.as_bytes());
    println!("{}", b64);
    let str = String::from_utf8(BASE64.decode(b64.as_bytes()).unwrap()).unwrap();
    println!("{}", str);

    // 每隔76个字符换行
    let b64 = BASE64_MIME.encode(str.as_bytes());
    println!("{}", b64);
    let str = String::from_utf8(BASE64_MIME.decode(b64.as_bytes()).unwrap()).unwrap();
    println!("{}", str);

    // 最后两个字符 - _
    let b64 = BASE64URL.encode(str.as_bytes());
    println!("{}", b64);
    let str = String::from_utf8(BASE64URL.decode(b64.as_bytes()).unwrap()).unwrap();
    println!("{}", str);

    // 没有填充符 =
    let b64 = BASE64_NOPAD.encode(str.as_bytes());
    println!("{}", b64);
    let str = String::from_utf8(BASE64_NOPAD.decode(b64.as_bytes()).unwrap()).unwrap();
    println!("{}", str);
}
```

### 结构化数据

[![serde](https://badge-cache.kominick.com/crates/v/serde.svg?label=serde)](https://docs.rs/serde/)

[Attributes · Serde](https://serde.rs/attributes.html)

```rust
use serde::{Deserialize, Serialize};

#[derive(Serialize, Deserialize)]
struct Person {
    name: String,
    age: u8,
    phones: Vec<String>,
}
```

#### JSON

[![serde-json-badge](https://badge-cache.kominick.com/crates/v/serde_json.svg?label=serde_json)](https://docs.rs/serde_json/*/serde_json/)

```rust
fn main() {
    let p = Person {
        name: "张三".to_string(),
        age: 10,
        phones: vec![
            "0081-1234567890".to_string(),
            "0086-13912345678".to_string(),
            "00886-1234567890".to_string(),
        ],
    };
	// to_string 没有换行
    let json = serde_json::to_string_pretty(&p).unwrap();
    println!("{}", json);
    let p: Person = serde_json::from_str(json.as_str()).unwrap();
    println!("{:?}", p);
}
```

```json
{
  "name": "张三",
  "age": 10,
  "phones": [
    "0081-1234567890",
    "0086-13912345678",
    "00886-1234567890"
  ]
}
```

#### CBOR

[![ciborium](https://badge-cache.kominick.com/crates/v/ciborium.svg?label=ciborium)](https://docs.rs/ciborium)

Concise Binary Object Representation 是一种基于二进制的数据序列化格式，设计上类似于 JSON，但是更简洁。

```rust
let mut buffer = Vec::new();
ciborium::into_writer(&p, &mut buffer).unwrap();
buffer.iter().for_each(|b| {
    print!("{:02X}", b)
});
println!();

let p: Person = ciborium::from_reader(buffer.as_slice()).unwrap();
println!("{:?}", p);
```

```
A3646E616D6566E5BCA0E4B889636167650A6670686F6E6573836F303038312D3132333435363738393070303038362D31333931323334353637387030303838362D31323334353637383930
```

#### YAML

[![serde_yaml](https://badge-cache.kominick.com/crates/v/serde_yaml.svg?label=serde_yaml)](https://docs.rs/serde_yaml/)

```rust
let yaml = serde_yaml::to_string(&p).unwrap();
println!("{}", yaml);
let p: Person = serde_yaml::from_str(&yaml).unwrap();
println!("{:?}", p);
```

```yaml
name: 张三
age: 10
phones:
- 0081-1234567890
- 0086-13912345678
- 00886-1234567890
```

#### TOML

```rust
let toml = toml::to_string_pretty(&p).unwrap();
println!("{}", toml);
let p: Person = toml::from_str(&toml).unwrap();
println!("{:?}", p);
```

```toml
name = "张三"
age = 10
phones = [
    "0081-1234567890",
    "0086-13912345678",
    "00886-1234567890",
```

#### URL

[![serde_qs](https://badge-cache.kominick.com/crates/v/serde_qs.svg?label=serde_qs)](https://docs.rs/serde_qs/)

```rust
let form = serde_qs::to_string(&p).unwrap();
println!("{}", form);
let p: Person = serde_qs::from_str(&form).unwrap();
println!("{:?}", p);
```

```
name=%E5%BC%A0%E4%B8%89&age=10&phones[0]=0081-1234567890&phones[1]=0086-13912345678&phones[2]=00886-1234567890
```

### 字节序

[![byteorder](https://badge-cache.kominick.com/crates/v/byteorder.svg?label=byteorder)](https://docs.rs/byteorder/)

```rust
use byteorder::{BigEndian, LittleEndian, WriteBytesExt};

fn main() {
    let num = 0x0000_1234_u32;

    let mut bytes = vec![];

    // 小端模式：数据的低位存储在低地址
    bytes.write_u32::<LittleEndian>(num).unwrap();
    // [52, 18, 0, 0]
    println!("{:?}", bytes);

    bytes.clear();

    // 大端模式：数据的高位存储在低地址
    bytes.write_u32::<BigEndian>(num).unwrap();
    // [0, 0, 18, 52]
    println!("{:?}", bytes);
}
```

## ~~错误链~~

[![error-chain-badge](https://badge-cache.kominick.com/crates/v/error-chain.svg?label=error-chain)](https://docs.rs/error-chain/)

停止维护公告：[Error-chain is no longer maintained - announcements - The Rust Programming Language Forum (rust-lang.org)](https://users.rust-lang.org/t/error-chain-is-no-longer-maintained/27561)

## 文件系统

### 文件

#### 顺序读取

```rust
use std::fs::File;
use std::io::{BufRead, BufReader, Write};

fn main() {
    let path = "file.txt";

    // 创建文件
    let mut output = File::create(path).unwrap();

    // 一次性写入
    output.write_all("Rust\n💖\nFun".as_bytes()).unwrap();

    // 打开文件
    let input = File::open(path).unwrap();

    // 使用缓冲
    let buffer = BufReader::new(input);
    // 每次读取一行
    for line in buffer.lines() {
        println!("{}", line.unwrap());
    }
}
```

#### 随机读取

```rust
use std::fs::OpenOptions;
use std::io::{Read, Seek, SeekFrom, Write};

fn main() {
    let path = "file.txt";

    let mut file = OpenOptions::new()
        .read(true)
        .write(true)
        .create(true)
        
        .append(false)
        .open(path)
        .unwrap();

    // 写入文本
    file.write_all("月下轻风，拂过古道，叶舞翩翩。".as_bytes()).unwrap();

    // 文件指针从开始向后偏移5个字节
    file.seek(SeekFrom::Start(5)).unwrap();

    // 读取buffer长度的字节
    let mut buffer = [0; 5];
    file.read_exact(&mut buffer).unwrap();

    // 由于是utf-8的编码方式，文本可能无法取完整
    println!("{}", String::from_utf8_lossy(&buffer));

    // 从当前位置再向后和偏移3个字节
    file.seek(SeekFrom::Current(3)).unwrap();
    // 随机写入
    file.write("插入文本".as_bytes()).unwrap();
}
```

#### 内存映射

[![memmap-badge](https://badge-cache.kominick.com/crates/v/memmap.svg?label=memmap)](https://docs.rs/memmap/)

```rust
use std::fs::File;
use std::io::Read;
use memmap2::Mmap;

fn main() {
    // https://www.apache.org/licenses/LICENSE-2.0.txt
    let mut file = File::open("LICENSE-2.0.txt").unwrap();

    let mut contents = Vec::new();
    file.read_to_end(&mut contents).unwrap();

    // 将整个文件映射到内存中
    let mmap = unsafe { Mmap::map(&file).unwrap() };

    println!("{}", &contents[..] == &mmap[..]);
}
```

### 目录

#### 文件元数据

```rust
use std::{env, fs};

fn main() {
    let dir = env::current_dir().unwrap();

    for entry in fs::read_dir(dir).unwrap() {
        let entry = entry.unwrap();
        let path = entry.path();

        let metadata = fs::metadata(&path).unwrap();
        println!("{:?}=>{:#?}", path, metadata);
    }
}
```

#### 文件和目录的操作

```rust
use std::fs;

fn main() {
    let mut path = String::new();
    for i in 1..=100 {
        path.push_str(format!("{}/", i).as_str());
    }

    // create_dir：如果路径的上一级不存在（7不存在），创建8时会出现panic
    // 创建100层文件夹
    fs::create_dir_all(&path).unwrap();

    // 创建文件
    path.push_str("a.txt");
    fs::File::create(&path).unwrap();

    // 改名和移动文件和目录
    fs::rename(&path, "1/0.txt").expect("无法重命名");
    fs::rename("1/2/3", "1/a").expect("无法重命名");

    fs::remove_file("1/0.txt").expect("无法删除");

    // remove_dir：如果文件夹不为空时无法删除
    // remove_dir_all：删除当前目录和子目录的所有成员
    fs::remove_dir_all("1").expect("无法删除");
}
```

#### 递归遍历路径

```rust
use std::fs::read_dir;
use std::io::Error;
use std::io::ErrorKind::PermissionDenied;
use std::path::Path;

// 主函数
fn main() {
    // 定义要遍历的根目录
    let root = Path::new("C:\\");
    // 尝试遍历目录，如果出错则打印错误信息
    if let Err(e) = traverse_directory(root) {
        eprintln!("Error traversing directory: {}", e);
    }
}

// 遍历目录的函数，接收一个路径作为参数
fn traverse_directory(path: &Path) -> Result<(), Error> {
    // 如果路径是一个文件，打印文件路径并返回
    if path.is_file() {
        println!("{}", path.display());
        return Ok(());
    }

    // 如果路径是一个目录，遍历目录中的每一个条目
    if path.is_dir() {
        for entry in read_dir(path)? {
            let entry = entry?;
            let entry_path = entry.path();
            // 对每一个条目递归调用遍历函数
            if let Err(e) = traverse_directory(&entry_path) {
                // 如果遇到权限错误，打印错误信息，但不停止遍历
                if e.kind() == PermissionDenied {
                    eprintln!("Permission denied accessing {}: {}", entry_path.display(), e);
                } else {
                    // 如果遇到其他错误，返回错误并停止遍历
                    return Err(e);
                }
            }
        }
    } else {
        // 如果路径既不是文件也不是目录，打印错误信息
        eprintln!("Error reading directory {}: {}", path.display(), Error::last_os_error());
    }

    // 遍历完成后返回成功
    Ok(())
}
```

## 硬件访问

### CPU

[![num_cpus-badge](https://badge-cache.kominick.com/crates/v/num_cpus.svg?label=num_cpus)](https://docs.rs/num_cpus/)

多线程时可以合理分配物理资源。

```
fn main() {
    println!("可用核心数{}", num_cpus::get());
    println!("物理核心数{}", num_cpus::get_physical());
}
```

## 内存管理

### 全局静态（堆栈）

#### lazy_static

[![lazy_static-badge](https://badge-cache.kominick.com/crates/v/lazy_static.svg?label=lazy_static)](https://docs.rs/lazy_static/)

> [!NOTE]
>
> `lazy_static!`的作用：
>
> 1. 在程序启动时，需要初始化全局静态变量，这个宏可以将初始化延迟到首次访问变量时进行
> 2. 避免初始化顺序或失败的问题

```rust
use error_chain::error_chain;
use lazy_static::lazy_static;
use std::sync::Mutex;

error_chain! { }

lazy_static! {
    // ref 表示 FRUIT 是一个静态引用
    static ref FRUIT: Mutex<Vec<String>> = Mutex::new(Vec::new());
}

fn insert(fruit: &str) -> Result<()> {
    let mut db = FRUIT.lock().map_err(|_| "Failed to acquire MutexGuard")?;
    db.push(fruit.to_string());
    Ok(())
}

fn main() -> Result<()> {
    insert("apple")?;
    insert("orange")?;
    insert("peach")?;
    {
        let db = FRUIT.lock().map_err(|_| "Failed to acquire MutexGuard")?;
        db.iter().enumerate().for_each(|(i, item)| println!("{}: {}", i, item));
    }
    insert("banana")?;
    println!("{:?}", FRUIT.lock().unwrap());
    Ok(())
}
```

## 网络

### TCP

```rust
// 引入标准库中的网络和线程相关组件
use std::net::{TcpListener, TcpStream};
use std::thread;
use std::io::{Read, Write};

// 处理客户端连接的函数
fn handle_client(mut stream: TcpStream) {
    // 创建一个50字节的缓冲区
    let mut data = [0_u8; 50];
    // 循环读取客户端发送的数据
    while match stream.read(&mut data) {
        Ok(size) => {
            // 将接收到的数据发送回客户端
            stream.write(&data[0..size]).unwrap();
            true
        }
        Err(_) => {
            // 发生错误时，打印错误信息并关闭连接
            println!("An error occurred, terminating connection with {}", stream.peer_addr().unwrap());
            stream.shutdown(std::net::Shutdown::Both).unwrap();
            false
        }
    } {}
}

// 主函数
fn main() {
    // 绑定监听器到指定的地址和端口
    let listener = TcpListener::bind("127.0.0.1:3333").unwrap();
    println!("Server listening on port 3333");

    // 循环接受传入的连接请求
    for stream in listener.incoming() {
        match stream {
            Ok(stream) => {
                // 打印新连接的信息
                println!("New connection: {}", stream.peer_addr().unwrap());
                // 为每个连接创建一个新的线程来处理
                thread::spawn(move || {
                    handle_client(stream)
                });
            }
            Err(e) => {
                // 打印连接错误的信息
                println!("Error: {}", e);
            }
        }
    }
    // 关闭监听器
    drop(listener);
}
```

[localhost](http://localhost:3333/)

### UDP

```rust
use std::net::UdpSocket;

pub fn main() -> std::io::Result<()> {
    println!("启动服务端，127.0.0.1:7878");
    // 在本地地址和端口上绑定UDP套接字
    let socket = UdpSocket::bind("127.0.0.1:7878")?;

    // 创建一个用于接收数据的缓冲区
    let mut buf = [0; 20];

    loop {
        // 接收数据，返回数据量和发送者的地址
        let (amt, src) = socket.recv_from(&mut buf)?;
        println!("服务端 <= {:?} from {}", buf, src);
        // 将接收到的数据（buf的一部分）发送回发送者
        socket.send_to(&buf[..amt], &src)?;
        println!("服务端 => {:?} to {}", buf, src);
    }
}
```

```rust
use std::net::UdpSocket;

pub fn main() -> std::io::Result<()> {
    // 创建一个UDP套接字
    let socket = UdpSocket::bind("0.0.0.0:0")?;

    println!("启动客户端，127.0.0.1:7878");
    // 连接到服务器的地址和端口
    socket.connect("127.0.0.1:7878")?;

    println!("客户端 => hello world");
    // 发送数据到服务器
    socket.send(b"hello world")?;

    // 创建一个用于接收服务器响应的缓冲区
    let mut buf = [0; 20];
    // 接收服务器的响应
    socket.recv(&mut buf)?;

    println!("客户端 <= {:?}", buf);

    Ok(())
}
```

```toml
# Cargo.toml
# ...
[[bin]]
name = "client"
path = "src/client.rs"

[[bin]]
name = "server"
path = "src/server.rs"
```

## 操作系统

### 外部命令

#### 调用系统终端

```rust
use std::process::Command;

fn main() {
    // 使用`if cfg!`宏来确定目标操作系统是否为Windows
    let output = if cfg!(target_os = "windows") {
        // 如果是Windows，使用`Command::new`创建一个指向`cmd`的新命令
        Command::new("cmd")
            // 使用`args`方法添加参数，`/C`告诉cmd执行后面的字符串命令
            .args(&["/C", "echo Hello, World!"])
            // 使用`output`方法执行命令并捕获输出
            .output()
            // 如果命令执行失败，使用`expect`方法提供错误消息
            .expect("failed to execute process")
    } else {
        // 如果不是Windows，假设是类Unix系统，使用`sh`来执行命令
        Command::new("sh")
            // 添加参数`-c`，允许后面跟一个字符串命令
            .arg("-c")
            // 提供要执行的命令
            .arg("echo Hello, World!")
            // 同样使用`output`方法执行命令并捕获输出
            .output()
            // 如果命令执行失败，提供错误消息
            .expect("failed to execute process")
    };

    // 从命令输出中获取标准输出（stdout）
    let hello = output.stdout;
    // 将标准输出转换为字符串，并使用`from_utf8_lossy`处理任何非UTF-8序列
    println!("Output: {:?}", String::from_utf8_lossy(&hello));
}
```

#### 运行PowerShell脚本

```rust
use std::process::Command;

// 主函数
fn main() {
    // 定义要查找的软件名
    let software_name = "Rust";

    // 使用 PowerShell 执行脚本并获取输出
    let output = Command::new("powershell")
        .arg("-Command")
        .arg(r#"
            // 定义要查找的注册表路径
            $paths = 'HKLM:\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Uninstall', 'HKLM:\\SOFTWARE\\Wow6432Node\\Microsoft\\Windows\\CurrentVersion\\Uninstall'
            
            // 遍历所有路径
            foreach ($path in $paths) {
                // 获取路径下的所有子项
                Get-ChildItem -Path $path | ForEach-Object {
                    // 获取每个子项的 DisplayName 和 InstallLocation 属性
                    $properties = Get-ItemProperty -Path $_.PSPath -Name DisplayName, InstallLocation -ErrorAction SilentlyContinue
                    
                    // 如果 DisplayName 和 InstallLocation 属性存在，并且 DisplayName 匹配软件名，则显示
                    if ($properties.DisplayName -and $properties.InstallLocation -and $properties.DisplayName -like '*$software_name*') {
                        '{0} => {1}' -f $properties.DisplayName, $properties.InstallLocation
                    }
                }
            }
        "#.replace("$software_name", software_name)) // 将脚本中的 $software_name 替换为实际的软件名
        .output() // 执行命令并获取输出
        .expect("failed to execute process"); // 如果执行失败，则抛出异常

    // 将输出从字节转换为字符串
    let output = String::from_utf8_lossy(&output.stdout);

    // 按行遍历输出
    for line in output.lines() {
        // 尝试将每一行分割成两部分：程序名和安装路径
        let parts: Vec<&str> = line.split(" => ").collect();

        // 如果成功，则打印出程序名和安装路径
        if parts.len() == 2 {
            let program_name = parts[0];
            let install_path = parts[1];
            println!("程序名：{}，安装路径：{}", program_name, install_path);
        }
    }
}
```

#### 环境变量

```rust
use std::env;

fn main() {
    // 尝试读取名为 "HOME" 的环境变量
    if let Ok(home_dir) = env::var("OS") {
        println!("HOME environment variable is set to: {}", home_dir);
    } else {
        println!("HOME environment variable is not set");
    }

    // 读取可能不存在的环境变量，并设置默认值
    let user_name = env::var("PATH").unwrap();
    println!("USER environment variable is set to: {}", user_name);
}
```

#### 当前执行文件的路径

```rust
use std::env;

fn main() {
    // 获取当前执行文件的路径（可能是一个链接或快捷方式）
    if let Ok(exe_path) = env::current_exe() {
        println!("Current executable path: {}", exe_path.display());

        // 如果你想获得可执行文件的规范路径（解决任何链接或快捷方式）
        if let Ok(canonical_path) = exe_path.canonicalize() {
            println!("Canonical executable path: {}", canonical_path.display());
        }
    }
}
```

#### 程序运行的附加参数

```rust
use std::env;

fn main() {
    // 第一个元素是程序名称，之后的元素是命令行参数  
    for (i, arg) in env::args().enumerate() {
        println!("Argument {}: {}", i, arg);
    }
}
```

#### 卸载程序的收尾工作

```rust
use std::env;
use std::fs::File;
use std::io::Write;
use std::process::Command;
use std::path::PathBuf;

fn main() -> std::io::Result<()> {
    // 获取当前可执行文件的路径
    let current_exe = env::current_exe()?;
    // 获取当前可执行文件所在目录的路径
    let current_dir = current_exe.parent().unwrap();

    // 创建批处理脚本的路径
    let bat_path = PathBuf::from(current_dir).join("delete_folder.bat");

    // 创建并写入批处理脚本
    let mut file = File::create(&bat_path)?;
    // 写入批处理脚本的第一行，关闭命令回显
    writeln!(file, "@echo off")?;
    // 写入切换到临时文件夹的命令
    writeln!(file, "cd /d \"%temp%\"")?;
    // 写入等待5秒的命令
    writeln!(file, "timeout /t 5")?;
    // 写入删除指定目录（通过参数传入）的命令
    writeln!(file, "rd /s /q \"%~1\"")?;
    // 脚本执行完成后自我删除
    writeln!(file, "del %~0")?;

    // 运行批处理脚本
    Command::new(bat_path.to_str().unwrap())
        // 这里传入的路径可能含有空格，\"%~1\"必须使用引号
        .arg(current_dir)
        .spawn()?;

    Ok(())
}
```

> [!WARNING]
>
> 这段 Rust 代码会删除程序所在文件夹内的所有内容，包括这个文件夹。这是一个**非常危险**的操作，因为一旦删除，文件将**无法恢复**。
>
> 在运行这段代码之前，请你确保已经**备份了所有重要的文件**，或者在可恢复的虚拟机中运行。
>
> 如果你不确定这段代码的作用，或者你不确定是否已经备份了所有重要的文件，**请不要运行这段代码**。

## 唯一值

### UUID

[![uuid](https://badge-cache.kominick.com/crates/v/uuid.svg?label=uuid)](https://docs.rs/uuid/)

通用唯一识别码（[RFC-4122](https://datatracker.ietf.org/doc/html/rfc4122), Universally Unique Identifier）是一种软件构建的标准，旨在确保在分布式计算环境中所有元素，在所有空间和时间上都能拥有唯一的辨识信息（第一个版本的设计）。

> [!TIP] 
>
> 版本选择：
>
> - v1 - 使用当前时间和本地机器上的网络接口的MAC地址（或节点）生成。
> - ~~v2~~ - 在v1的基础上，将时间字段低位替换为本地账户的ID，使得在时间地点创作者三个参数上尽可能让其唯一。
> - v3 - 基于命名空间的MD5哈希生成。
> - v4 - 使用随机数据生成。
> - v5 - 基于命名空间的SHA1哈希生成。（注意：这通常是版本3的替代方案，SHA1比MD5更安全）
> - v6 - 在v1的基础上，通过时间实现单调递增的生成（注意：这通常是版本1的替代方案，解决了v1的值分散和无法顺序插入的缺点）。
> - v7 - 使用Unix Epoch毫秒时间戳并结合版本4的填充随机数据的方式生成（注意：这通常是版本6的替代版本，并且兼容ULID）。
> - v8 - 使用用户定义的数据生成（version=48-51bit，variant=64-65bit，`UUID: ffeeddcc-bbaa-8988-b766-554433221100`）。

UUID的不足之处：

- UUID v1/v2 在许多环境中是不切实际的，因为它需要访问唯一、稳定的 MAC 地址。
- UUID v3/v5 需要唯一的种子并生成随机分布的 ID，这可能会导致许多数据结构中的碎片化。
- UUID v4 除了随机性之外不提供任何其他信息，随机性可能会导致许多数据结构中的碎片化。

```rust
use uuid::{NoContext, Timestamp, Uuid};

fn main() {
    // 使用现在时的时间戳
    let uuid = Uuid::now_v7();
    let uuid = Uuid::new_v7(Timestamp::now(NoContext));
    // 使用指定的时间戳
    // let uuid = Uuid::new_v7(Timestamp::from_rfc4122(sec as u64, nano as u16));
    // 01900557-61a5-7c28-a6d9-24c5cb0a516e
    println!("{}", uuid.to_string());
    // 0190055761a57c28a6d924c5cb0a516e
    println!("{}", uuid.as_simple().to_string());

    let uuid = Uuid::parse_str("01900557-61a5-7c28-a6d9-24c5cb0a516e").unwrap();
    println!("{}", uuid);
    let uuid = Uuid::parse_str("0190055761a57c28a6d924c5cb0a516e").unwrap();
    println!("{}", uuid)
}
```

#### GUID

全局唯一标识符（GUID，Globally Unique Identifier）是微软对通用唯一识别码（UUID）的一个实现，专属用于 Windows 平台。

GUID 是微软对 UUID 的一个实现，通常用在 Windows 平台上，由于算法没有公开，在 Windows 平台时可以直接从系统的 API 中获取。

```toml
[dependencies]
winapi = { version = "0.3.9", features = ["wtypesbase", "winerror"] }
```

```rust
use std::mem::zeroed;
use std::process::exit;
use std::slice::from_raw_parts;
use winapi::ctypes::{c_int, wchar_t};
use winapi::shared::guiddef::{GUID, REFGUID};
use winapi::shared::winerror::{HRESULT, S_OK};
use winapi::shared::wtypesbase::LPOLESTR;

// 链接到Ole32.lib
#[link(name = "Ole32")]
extern "system" {
    // CoCreateGuid是一个用于创建全局唯一标识符(GUID)的函数
    fn CoCreateGuid(pguid: *mut GUID) -> HRESULT;
    // StringFromGUID2是一个将GUID转换为字符串的函数
    fn StringFromGUID2(pguid: REFGUID, lpsz: LPOLESTR, cchMax: c_int) -> c_int;
}

// 创建GUID
fn create_guid() -> Result<GUID, String> {
    let mut guid: GUID = unsafe { zeroed() };
    let hresult = unsafe { CoCreateGuid(&mut guid as *mut GUID) };
    if hresult != S_OK {
        Err("创建GUID失败".to_string())
    } else {
        Ok(guid)
    }
}

// GUID转字符串
fn guid_to_string(g: &GUID) -> Result<String, String> {
    let mut buffer: [wchar_t; 39] = [0; 39];
    let result = unsafe { StringFromGUID2(g as REFGUID, buffer.as_mut_ptr(), buffer.len() as c_int) };
    if result == 0 {
        return Err("缓冲区大小不足以容纳GUID字符串".to_string());
    } else if result < 0 {
        return Err("将GUID转换为字符串失败".to_string());
    }
    let guid_string = unsafe {
        let slice = from_raw_parts(buffer.as_ptr(), result as usize);
        String::from_utf16_lossy(slice)
    };
    Ok(guid_string)
}

// GUID转字节
fn guid_to_bytes(g: &GUID) -> [u8; 16] {
    let mut bytes = [0u8; 16];
    bytes[0..4].copy_from_slice(&g.Data1.to_be_bytes());
    bytes[4..6].copy_from_slice(&g.Data2.to_be_bytes());
    bytes[6..8].copy_from_slice(&g.Data3.to_be_bytes());
    bytes[8..].copy_from_slice(&g.Data4);
    bytes
}

// 格式化GUID
fn guid_to_format_string(g: &GUID, format: &str) -> String {
    match format {
        "N" | "n" => {
            guid_to_bytes(g).iter()
                .map(|&byte| format!("{:02x}", byte))
                .collect::<Vec<_>>()
                .join("")
        }
        "D" | "d" => {
            let s = guid_to_format_string(g, "N");
            format!("{}-{}-{}-{}-{}", &s[..8], &s[8..12], &s[12..16], &s[16..20], &s[20..])
        }
        "B" | "b" => format!("{{{}}}", guid_to_format_string(g, "D")),
        "P" | "p" => format!("({})", guid_to_format_string(g, "D")),
        "X" | "x" => {
            let s = guid_to_format_string(g, "N");
            let v = (0..s.len()).step_by(2).map(|i| &s[i..(i + 2)]).collect::<Vec<&str>>();
            format!("{{{},{},{},{{{}}}}}",
                    format!("0x{}", v[0..4].join("")),
                    format!("0x{}", v[4..6].join("")),
                    format!("0x{}", v[6..8].join("")),
                    v[8..].iter()
                        .map(|b| format!("0x{}", b))
                        .collect::<Vec<_>>()
                        .join(",")
            )
        }
        _ => guid_to_format_string(g, "D"),
    }
}

fn main() {
    let guid = create_guid().unwrap_or_else(|e| {
        eprintln!("获取GUID失败：{}", e);
        exit(1);
    });

    let str = guid_to_string(&guid).unwrap_or_else(|e| {
        eprintln!("转换GUID失败：{}", e);
        exit(2);
    });
    println!("生成的GUID是: {}", str);

    println!("GUID (N): {}", guid_to_format_string(&guid, "N"));
    println!("GUID (D): {}", guid_to_format_string(&guid, "D"));
    println!("GUID (B): {}", guid_to_format_string(&guid, "B"));
    println!("GUID (P): {}", guid_to_format_string(&guid, "P"));
    println!("GUID (X): {}", guid_to_format_string(&guid, "X"));
}
```

### ULID

ULID（Universally Unique Lexicographically Sortable Identifier）是一个全局唯一标识符（GUID）的替代方案，它提供了几个优势，如字典序排序、简洁性、安全性和效率。ULID 是一个 26 个字符的字符串，由 10 个字符的时间戳（毫秒级）和 16 个字符的随机数组成。

```rust
use ulid::Ulid;

fn main() {
    let ulid = Ulid::new();
    println!("{}", ulid.to_string());
    println!("{}", ulid.timestamp_ms());
    println!("{}", ulid.random());

    let ulid = Ulid::from_string("01j034wbwpgpnqn9d2a2h1mf7t").unwrap();
    println!("{}", ulid.to_string());
    println!("{}", ulid.timestamp_ms());
    println!("{}", ulid.random());
}
```

#### UUIDv7转ULID

[![ulid](https://badge-cache.kominick.com/crates/v/ulid.svg?label=ulid)](https://docs.rs/ulid)[![base32](https://badge-cache.kominick.com/crates/v/base32.svg?label=base32)](https://docs.rs/base32)

两者都是使用的Unix Epoch的毫秒级时间戳，其余位使用随机数据填充。

```rust
use ulid::Ulid;
use base32::Alphabet;
use uuid::{NoContext, Timestamp, Uuid};

fn main() {
    let v7 = Uuid::new_v7(Timestamp::now(NoContext));
    // 第一步：获取UUIDv7的二进制表示
    let bytes = v7.as_bytes();
    // 第二步：创建一个长度为20字节的字节数组v，并全部初始化为0
    let mut v = Vec::from([0u8; 20]);
    // 第三步：将UUIDv7的16字节二进制数据填充到v的末尾（从第5个字节开始，因为ULID通常包含一个4字节的时间戳前缀）
    v[4..].clone_from_slice(bytes);
    // 第四步：将填充后的20字节数据v转换为Crockford版本的base32编码（不含 ILOU 四个字符）
    let base32 = base32::encode(Alphabet::Crockford, v.as_slice());
    // 第五步：由于ULID通常包含一个时间戳前缀的编码，我们截取base32编码字符串的剩余部分（跳过前6个字符）
    let ulid = &base32[6..];
    println!("{}", ulid);

    let u = v7.as_u128();
    let ulid = Ulid::from(u);
    println!("{}", ulid.to_string());
}
```

### NanoID

[![nanoid](https://badge-cache.kominick.com/crates/v/nanoid.svg?label=nanoid)](https://docs.rs/nanoid)

NanoID是一个小巧、安全、URL友好且唯一的字符串ID生成器。】】】【。

- **短**：NanoID生成的ID通常只有21个字符，比标准的UUID（36个字符）短得多。
- **唯一**：NanoID生成的ID具有非常低的冲突概率，与UUID v4相当。
- **URL安全**：NanoID只使用URL友好的字符，因此可以安全地用作URL的一部分。
- **随机**：NanoID生成的ID是随机的，没有任何可预测性。
- **快速**：NanoID在性能上表现出色，比UUID快两倍。
- **安全**：NanoID使用硬件随机生成器，确保ID难以预测和伪造。

```rust
use nanoid::alphabet::SAFE;
use nanoid::nanoid;

fn main() {
    // nanoid默认使用的字符表
    let alphabet = String::from_iter(SAFE.to_vec());
    // _-0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ
    println!("{}", alphabet);

    // 创建一个nanoid
    let id = nanoid!();
    println!("{}", id);

    // 默认长度是21，减少会增加碰撞概率
    let id = nanoid!(32);
    println!("{}", id);

    // 使用自定义的字符表（Crockford's Base32）生成特定长度的nanoid
    let alphabet: [char; 32] = "0123456789abcdefghjkmnpqrstuwxyz".chars().collect::<Vec<char>>().try_into().unwrap();
    let id = nanoid!(18, &alphabet);
    println!("{}", id);

    // 指定随机的序列
    let id = nanoid!(18, &alphabet, |len| {
        (0..len).into_iter().map(|i| (i * 2) as u8).collect()
    });
    // 02468acegjmprtwy02
    println!("{}", id);
}
```

## 数学

### 矩阵

[![nalgebra](https://badge-cache.kominick.com/crates/v/nalgebra.svg?label=nalgebra)](https://docs.rs/nalgebra)

[Getting started | nalgebra](https://www.nalgebra.org/docs/user_guide/getting_started)

矩阵是一个用来组织数据的工具，矩阵可以进行运算来处理这些数据。

假如有一个一周不同时间段的销售额统计：

| 销售额（元） |   上午   |   中午   |   下午   |
| :----------: | :------: | :------: | :------: |
|    星期一    | 3162.64  | 6931.92  | 8105.56  |
|    星期二    | 3239.78  | 5251.93  | 6710.55  |
|    星期三    | 8163.82  | 4107.39  | 7610.62  |
|    星期四    | 6014.62  | 6025.03  | 4561.76  |
|    星期五    | 5233.84  | 5407.28  | 10564.35 |
|    星期六    | 16935.22 | 19536.49 | 17512.47 |
|    星期日    | 168230.7 | 15644.15 | 8516.95  |

#### 矩阵的意义

将这些数据使用矩阵表示，然后就可以提取出一些有用的数据：

```rust
use nalgebra::matrix;

fn main() {
    // matrix：固定大小的矩阵
    let sales_data = matrix![
        // 上午      中午       下午
         3162.64,  6931.92,  8105.56; // 星期一
         3239.78,  5251.93,  6710.55; // 星期二
         8163.82,  4107.39,  7610.62; // 星期三
         6014.62,  6025.03,  4561.76; // 星期四
         5233.84,  5407.28, 10564.35; // 星期五
        16935.22, 19536.49, 17512.47; // 星期六
        168230.7, 15644.15,  8516.95; // 星期日
    ];
    println!("这周各个时间段的销售情况：{}", sales_data);

    // 计算每日总销售量
    let daily_totals = sales_data.column_sum();
    // [[18200.12, 15202.26, 19881.83, 16601.41, 21205.47, 53984.18, 192391.8]]
    println!("每日总销售量: {:?}", daily_totals);

    // 计算每日平均销售额
    let daily_averages = sales_data.mean();
    // 16069.860476190477
    println!("每日平均销售额: {:?}", daily_averages);

    // 计算每周总销售额
    let weekly_total = daily_totals.sum();
    // 337467.07
    println!("这周总销售额: {}", weekly_total);

    // 找出销售额最高的一天
    let max_sales_day = daily_totals.imax();
    // 第7天
    println!("销售额最高的一天: 第{}天", max_sales_day + 1);

    // 找出销售额最低的一天
    let min_sales_day = daily_totals.imin();
    // 第2天
    println!("销售额最低的一天: 第{}天", min_sales_day + 1);
}
```

#### 加法

```rust
use nalgebra::matrix;

fn main() {
    // 前2天每个时间段的销售
    let a = matrix![
        // 上午      中午       下午
         3162.64,  6931.92,  8105.56; // 星期一
         3239.78,  5251.93,  6710.55; // 星期二
    ];
    // 后2天每个时间段的销售
    let b = matrix![
        // 上午      中午       下午
        16935.22, 19536.49, 17512.47; // 星期六
        168230.7, 15644.15,  8516.95; // 星期日
    ];
    // 加法和减法就是矩阵中位置对应的每个数据进行加减法
    let c = a + b;

    //          上午                    中午                  下午
    //   星期一二     星期六日     星期一二    星期六日     星期一二   星期六日
    // [[20097.86, 171470.48], [26468.41, 20896.08], [25618.03, 15227.5]]
    println!("这周前2天和后2天的每个时段的销售统计：{:?}", c);
    let c = a - b;

    //          上午                    中午                  下午
    //   星期一二     星期六日     星期一二    星期六日     星期一二   星期六日
    // [-13772.58, -164990.92], [-12604.57, -10392.22], [-9406.91, -1806.4]
    println!("这周前2天和后2天的每个时段的销售差值：{:?}", c);
}
```

#### 数乘

```rust
use nalgebra::matrix;

fn main() {
    let a = matrix![
        // 上午      中午       下午
        16935.22, 19536.49, 17512.47; // 星期六
        168230.7, 15644.15,  8516.95; // 星期日
    ];
    // 数乘就是将矩阵中的每个数据进行乘法运算
    let b = a * 5.0 / 100.0;
    // 周末的销售额的5%用来给员工发提成

    // [846.761, 976.8245, 875.6235]
    // [8411.535, 782.2075, 425.8475]
    println!("这周末每个时间段员工的提成明细：{:?}", b);
}
```

#### 乘法

```rust
use nalgebra::matrix;

fn main() {
    // 这周各个时间段的销售额
    let a = matrix![
        // 上午     中午       下午
         3162.64,  6931.92,  8105.56; // 星期一
         3239.78,  5251.93,  6710.55; // 星期二
         8163.00,  4107.00,  7610.00; // 星期三
         6014.00,  6025.00,  4561.00; // 星期四
         5233.00,  5407.00, 10564.00; // 星期五
        16935.22, 19536.49, 17512.47; // 星期六
        168230.7, 15644.15,  8516.95; // 星期日
    ];
    // 不同部门的员工在每天不同时间段的提成
    let b = matrix![
        //上午  中午  下午
        0.13, 0.15, 0.18; // 前厅
        0.20, 0.25, 0.28; // 后厨
        0.10, 0.13, 0.15; // 收银
    ];

    // transpose是转置，将矩阵的行和列交换位置，数据跟着一起交换
    // 就像把矩阵表格b沿着"\"对称线进行翻转
    // 行从左往右依次是：前厅，后厨，收银
    // 列从上往下依次是：上午，中午，下午
    // 这样就和矩阵a的维度匹配（a的行和b的列意义匹配），可以后续计算矩阵的乘法
    let b_t = b.transpose();

    // 矩阵乘法可以计算出不同部门的员工按不同的时间段提成比例，计算出每天的总提成
    let c = a * b_t;

    //                前厅         后厨        收银
    // 星期一     [   2909.932,  4635.0648, 2433.2476]
    // 星期二     [  2416.8599,  3839.8925, 2013.3114]
    // 星期三     [    3047.04,    4790.15,   2491.71]
    // 星期四     [    2506.55,    3986.13,    2068.8]
    // 星期五     [    3392.86,    5356.27,   2810.81]
    // 星期六     [  8284.2967, 13174.6581, 6860.1362]
    // 星期日     [ 25749.6645, 39941.9235, 20134.352]
    println!("这周每个部门每天的总提成：{}", c);
}
```

#### 逆

逆矩阵运算是矩阵乘法的逆运算，就像是`a * b = c`，`c * (1/b) = a`。

两个矩阵相乘得到的矩阵，如果再与其中一个矩阵的逆矩阵相乘，就会得到另一个矩阵。

```rust
use nalgebra::matrix;

fn main() {
    // 定义一个矩阵a，代表鸡和兔子的头和脚的数量
    let a = matrix![
        //鸡  兔
        1.0, 1.0; // 头
        2.0, 4.0; // 脚
    ];
    // 定义一个矩阵b，代表鸡和兔子的数量
    let b = matrix![
        135.0; // 135只鸡
        274.0; // 274只兔
    ];
    // 计算c = a * b，得到头和脚的总数量
    let c = a * b;
    //    头     脚
    // [409.0, 1366.0]
    println!("头和脚的总数量: {:?}", c);

    // 计算矩阵a的逆矩阵a_i，不是所有矩阵都可以计算逆矩阵
    let a_i = a.try_inverse().unwrap();
    //   鸡    兔
    // [2.0, -1.0] // 头
    // [-0.5, 0.5] // 脚
    println!("矩阵a的逆矩阵: {:?}", a_i);
    // 计算d = a_i * c，得到鸡和兔子的数量
    let d = a_i * c;
    //   鸡      兔
    // [135.0, 274.0]
    println!("鸡和兔子的数量: {:?}", d);
}
```

> [!NOTE]
>
> ##### 一个矩阵存在逆矩阵的前提：
>
> 1. 行数和列数相等
> 2. 行列式值 ≠ 0

#### 行列式

这是个计算量很大的过程，参考行列式的**拉普拉斯展开定理**，对于二维矩阵，计算过程如下：

```rust
let a = matrix![
    //鸡  兔
    1.0, 1.0; // 头
    2.0, 4.0; // 脚
];
//  (+ 鸡头 * 兔脚) + (- 鸡脚 * 兔头) = 2
println!("a的行列式值={}", a.determinant());
```

### 三角函数

```rust
use std::f64::consts::PI;

fn main() {
    // 三角函数示例
    let radians = PI / 4.0;
    println!("sin(π/4) = {}", radians.sin());
    println!("cos(π/4) = {}", radians.cos());
    println!("tan(π/4) = {}", radians.tan());

    // 反三角函数
    println!("asin(0.5) = {}", 0.5_f64.asin());
    println!("asin(0.5) = {}", f64::asin(0.5));
    println!("acos(0.5) = {}", f64::acos(0.5));
    println!("atan(1.0) = {}", f64::atan(1.0));

    // 双曲三角函数
    println!("sinh(π/4) = {}", radians.sinh());
    println!("cosh(π/4) = {}", radians.cosh());
    println!("tanh(π/4) = {}", radians.tanh());

    // 反双曲三角函数
    println!("asinh(0.5) = {}", f64::asinh(0.5));
    println!("acosh(1.0) = {}", 1.0_f64.acosh());
    println!("atanh(0.5) = {}", 0.5_f64.atanh());
}
```

> [!NOTE]
>
> 标准库的三角函数是：弧度制
>
> 余切（cot）：`1 / tan`
>
> 正割（sec）：`1 / cos`
>
> 余割（csc）：`1 / sin`

### 复数

[![num-badge](https://badge-cache.kominick.com/crates/v/num.svg?label=num)](https://docs.rs/num/)

```rust
use std::f32::consts::PI;
use num::Complex;
use num::complex::{c32, c64, ComplexFloat};

fn main() {
    let a = Complex::new(3.0, 4.0);
    // 3 + 4i
    println!("{} + {}i", a.re, a.im);

    let b = c64(2.0, 1.0);

    // 加法
    println!("{a} + {b} = {}", a + b);

    // 减法
    println!("{a} - {b} = {}", a - b);

    // 乘法
    println!("{a} * {b}b = {}", a * b);

    // 除法
    println!("{a} / {b} = {}", a / b);

    // 复数的共轭（虚部相反）
    println!("{a} 的共轭 = {}", a.conj());

    // 复数的模（距离0点的长度）
    println!("{a} 的模 = {}", a.abs());

    // 复数的相位（复平面上与实数轴的弧度夹角）
    println!("{a} 的相位 = {}", a.arg());

    // 数学运算
    println!("e^(2i * Π) = {}", c32(0.0, 2.0 * PI).exp());// ~1
}
```

### 大数

[![num-badge](https://badge-cache.kominick.com/crates/v/num.svg?label=num)](https://docs.rs/num/)

#### 整数

```rust
use num::{BigInt, Num, ToPrimitive};
use num::bigint::ToBigInt;

fn main() {
    // 基本数据类型转换
    let num = u128::MAX.to_bigint().unwrap();
    println!("{}", &num + 1);
    // 10次方根后再平方，然后转i32
    println!("{}", num.nth_root(10).pow(2).to_i32().unwrap());

    // 进制转换 [2,36]
    let big = BigInt::from_str_radix("-123456789012345678901234567890", 10).unwrap();
    let big = &big * &big;
    println!("{}", &big.to_str_radix(36));

    // 有无符号的转换
    let b = big.to_bigint().unwrap();
    println!("{}", b);
    let b = b.to_biguint().unwrap();
    println!("{}", b);
}
```

#### 分数

```rust
use num::{BigInt, BigRational, Rational64, ToPrimitive};

fn main() {
    // 使用传统的浮点数进行计算
    let a = 0.1;
    let b = 0.2;
    // 0.1+0.2=0.30000000000000004
    println!("{}+{}={}", a, b, &a + &b);

    // 借助分数来解决精度的问题
    // 1/10
    let a = BigRational::new(BigInt::from(1), BigInt::from(10));
    // 200/1000 会自动化简为 1/5
    let b = BigRational::new(BigInt::from(200), BigInt::from(1000));
    // 计算出3/10后，输出为浮点数，这个数只能用于显示，不能用于后续计算
    println!("{}+{}={}", a, b, (&a + &b).to_f64().unwrap());

    // 虽然 BigRational 提供了高精度的计算，但它也使用了更多的内存和可能更慢的计算速度
    let a = Rational64::new(1, 10);
    let b = Rational64::new(2, 10);
    println!("{}+{}={}", a, b, (&a + &b).to_f64().unwrap());
}
```

#### 随机数

```rust
use std::iter::repeat_with;
use std::str::FromStr;
use num::{BigInt, BigUint};
use rand::{Rng, RngCore, thread_rng};

fn main() {
    // 创建线程安全的随机数生成器
    let rng = &mut thread_rng();

    // 填充大整数的byte数组
    let mut bytes: Vec<u8> = vec![0; 2];
    rng.fill_bytes(&mut bytes);

    // 按照大端的方式转换，低字节位是高位
    println!("{:?}", &bytes);
    let big = BigUint::from_bytes_be(&bytes);
    println!("{}", big);

    // 生成指定位数的大整数
    let rand_num: String = repeat_with(|| thread_rng().gen_range(b'0'..=b'9'))
        .take(50)
        .map(char::from)
        .collect();
    println!("{}", rand_num);
    let b = BigInt::from_str(&rand_num).unwrap();
    println!("{}", b)
}
```

#### 迭代

```rust
use std::str::FromStr;
use num::BigInt;

fn main() {
    let a = BigInt::from_str("1234567890123456789012345678901234567890").unwrap();
    let b = BigInt::from_str("1234567890123456789012345678901234567892").unwrap();

    // [a, b)
    let c: Vec<_> = num::range(a.clone(), b.clone()).take(4).collect();
    println!("{:?}", c);
    // [a, b]
    let c: Vec<_> = num::range_inclusive(a.clone(), b.clone()).take(4).collect();
    println!("{:?}", c);

    let s = BigInt::from_str("100000000000000000000000000000000000000").unwrap();
    let c: Vec<_> = num::range_step(a, b, s).collect();
    println!("{:?}", c);
}
```

### 统计

[![statrs](https://badge-cache.kominick.com/crates/v/statrs.svg?label=statrs)](https://docs.rs/statrs/)

#### 特征值

```rust
use statrs::distribution::{Continuous, ContinuousCDF, Exp};
use statrs::statistics::Distribution;

fn main() {
    // 创建一个 lambda=0.5 的指数分布
    // 由于这里指定了是 Exp 指数分布，这些分布的特征值使用概率公式即可算出
    let n = Exp::new(0.5).unwrap();

    // 输出指数分布的 rate 参数
    // rate 就是创建时传入的 lambda 参数值
    // mean = 1/rate。
    println!("rate={}", n.rate());

    // 输出指数分布的均值（期望）
    // mean = 1/rate。
    println!("mean={}", n.mean().unwrap());

    // 输出指数分布的方差
    // variance = 1/(rate^2)
    println!("variance={}", n.variance().unwrap());

    // 输出指数分布的熵（entropy）
    // 熵是描述一个随机变量不确定性的度量
    // 1 - ln(rate) 
    println!("entropy={}", n.entropy().unwrap());

    // 输出指数分布的偏度（skewness）
    // 偏度描述了分布的偏斜方向和程度
    // 对于 Exp 分布，偏度是正数，表示分布向右偏斜（长尾在右侧）
    // 2.0
    println!("skewness={}", n.skewness().unwrap());

    // 输出随机变量 x=0.5 的累积分布函数（CDF）值
    // CDF 描述了随机变量 X 小于或等于 x 的概率，即 P(X <= x)
    // 1 - e^(-rate * x)
    println!("cdf(0.5)={}", n.cdf(0.5));

    // 输出随机变量 x=0.5 的概率密度函数（PDF）值
    // PDF 描述了随机变量 X 取某一特定值的概率密度，即 f(x)
    // 在连续分布中，PDF 的值不是概率，而是概率密度，其积分才表示概率
    // rate * e^(-rate * x)
    println!("pdf(0.5)={}", n.pdf(0.5));
}
```

#### 指数分布

```rust
use rand::distributions::Distribution;
use rand::rngs::OsRng;
use statrs::distribution::Exp;

fn main() {
    // 创建一个基于操作系统提供的随机数生成器
    let mut rng = OsRng::default();

    // 创建一个具有 lambda=0.5 的指数分布特征实例
    let exp_dist = Exp::new(0.5).unwrap();

    // 生成 100 个指数分布的随机样本
    let mut samples = (0..100).into_iter()
        .map(|_| exp_dist.sample(&mut rng))
        .collect::<Vec<_>>();

    // 对样本进行排序，得到按值降序排列的指数分布数据
    samples.sort_by(|a, b| b.partial_cmp(a).unwrap());

    // 输出指数分布的数据样本
    samples.iter().for_each(|&value| println!("{}", value));
}
```

#### 离散分布

```rust
use rand::Rng;
use rand::rngs::OsRng;
use statrs::distribution::ChiSquared;
use statrs::statistics::{Max, Min, Statistics};
use statrs::statistics::{Data, Distribution, Median};

fn main() {
    let mut rng = OsRng::default();
    let mut numbers = [0.0; 1000];

    // 生成特定特征的随机数据，并且填充到切片
    for num in numbers.iter_mut() {
        // 卡方分布特征实例
        let n = ChiSquared::new(3.0).unwrap();
        *num = rng.sample(n);
    }
    numbers.iter().for_each(|n| println!("{}", n));

    let data = Data::new(numbers);
    println!("均值={}", data.mean().unwrap());
    println!("最大值={}", data.max());
    println!("中位数={}", data.median());
    println!("最小值={}", data.min());
    println!("最小值绝对值={}", data.iter().abs_min());
    println!("最大值绝对值={}", data.iter().abs_max());
    println!("几何平均值={}", data.iter().geometric_mean());
    println!("谐波平均值={}", data.iter().harmonic_mean());
    println!("方差={}", data.variance().unwrap());
    println!("标准差={}", data.std_dev().unwrap());
    println!("总体方差={}", data.iter().population_variance());
    println!("总体标准差={}", data.iter().population_std_dev());
    println!("协方差={}", data.iter().covariance(data.clone().iter()));
    println!("总体协方差={}", data.iter().population_covariance(data.clone().iter()));
    println!("平方根平均值={}", data.iter().quadratic_mean());
}
```

## 文本处理

### 正则表达式

[![regex-badge](https://badge-cache.kominick.com/crates/v/regex.svg?label=regex)](https://docs.rs/regex/)

#### 搜索

```rust
use regex::{Match, Regex};

fn main() {
    let re = Regex::new(r"456").unwrap();
    let str = "abc123def456ghi789jkl0xxyyzz";
    let m = re.find(str).unwrap();
    // 匹配到的文本：456，位置在：[9, 12)，长度：3
    println!("匹配到的文本：{}，位置在：[{}, {})，长度：{}",
             m.as_str(), m.start(), m.end(), m.len());

    let re = Regex::new(r"\d").unwrap();
    let m = re.find_at(str, 12).unwrap();// 从第12个字符位置开始
    // 匹配到的文本：7，位置在：[15, 16)，长度：1
    println!("匹配到的文本：{}，位置在：[{}, {})，长度：{}",
             m.as_str(), m.start(), m.end(), m.len());

    let re = Regex::new("x").unwrap();
    let ms: Vec<Match> = re.find_iter(str).collect();
    for m in ms {
        // 匹配到的文本：x，位置在：[22, 23)，长度：1
        // 匹配到的文本：x，位置在：[23, 24)，长度：1
        println!("匹配到的文本：{}，位置在：[{}, {})，长度：{}",
                 m.as_str(), m.start(), m.end(), m.len());
    }
}
```

| 表达式     | 说明                                                         | 结果  |
| ---------- | ------------------------------------------------------------ | ----- |
| `..123`    | `.`：匹配除换行符以外的任何单个字符                          | bc123 |
| `[abcxyz]` | `[]`：定义字符类，匹配方括号中的任一字符                     | a     |
| `[^abc]`   | `[^]`：定义非字符类，匹配不在方括号中的任何字符。            | 1     |
| `0x*`      | `*`：匹配前面的子表达式零次或多次。                          | 0xx   |
| `\d+`      | `+`：匹配前面的子表达式一次或多次。`\`：用于转义字符，以便它们具有特殊意义或匹配文字字符。 | 123   |
| `x?y`      | `?`：匹配前面的子表达式零次或一次。                          | xy    |
| `\d{2,4}`  | `{n,m}`：匹配前面的子表达式至少 `n` 次，但不超过 `m` 次。    | 123   |
| `(ghi)`    | `(xyz)`：定义捕获组，并匹配括号中的模式。                    | ghi   |
| `abd\|xyz` | `|`：表示逻辑“或”，匹配括号中的任何模式。                    | abc   |
| `^abc`     | `^`：匹配输入字符串的开始位置。                              | abc   |
| `zz$`      | `$`：匹配输入字符串的结束位置。                              | zz    |

| 转义字符    | 说明                                                         | 等效                                   |
| ----------- | ------------------------------------------------------------ | -------------------------------------- |
| `\w`        | 匹配所有的数字和大小写字母和下划线                           | `[a-zA-Z0-9_]`                         |
| `\W`        | 匹配`\w`以外的字符                                           | `[^\w]`                                |
| `\d`        | 匹配所有的数字                                               | `[0-9]`                                |
| `\D`        | 匹配数字以外的字符                                           | `[^\d]`                                |
| `\s`        | 匹配所有空白字符                                             | `[\f\n\r\t\v\u00A0\u2028\u2029\uFEFF]` |
| `\S`        | 匹配所有空白字符以外的字符                                   | `[^\s]`                                |
| `\f`        | 匹配一个分页符                                               |                                        |
| `\n`        | 匹配一个换行符                                               |                                        |
| `\r`        | 匹配一个回车符                                               |                                        |
| `\t`        | 匹配一个制表符                                               |                                        |
| `\v`        | 匹配一个垂直制表符                                           |                                        |
| `\b`        | 匹配单词字符的边界                                           |                                        |
| `\p{Lower}` | 匹配Unicode字符属性 [Unicode Character Categories](https://www.compart.com/en/unicode/category) |                                        |
| `\u0061`    | 匹配Unicode字符                                              |                                        |

#### 统计单词频率

```rust
use std::collections::HashMap;
use regex::Regex;

fn main() {
    // 匹配单词边界内的单词
    // \b 是一个特殊的字符，表示单词的边界
    // \w+ 是匹配单词字符，至少出现一次
    let re = Regex::new(r"(\b\w+\b)").unwrap();

    let str = "In the quietude of a summer dawn, \
    the world awakens to a symphony of life. \
    The sun peeks through the horizon, \
    painting the sky with hues of orange and pink, \
    as if a master painter had dipped his brush in the dawn's palette.";

    // 统计单词出现的次数
    let word_counts: HashMap<String, i32> = HashMap::from_iter(
        // 迭代所有匹配项
        re.find_iter(str)
            // 获取每个匹配的字符串
            .map(|mat| mat.as_str())
            // 将每个单词转换为String，并初始化计数为1
            .map(|word| (word.to_string(), 1))
            // 使用scan来累积计数
            .scan(HashMap::new(), |counts, (word, _)| {
                // 如果单词不存在则插入，否则增加计数
                *counts.entry(word).or_insert(0) += 1;
                // 返回新的HashMap状态以继续迭代
                Some(counts.clone())
            })
            // flat_map 的替代，因为我们只关心最终的HashMap
            .flatten()
    );
    println!("{:#?}", word_counts);
}
```

#### 捕获

```rust
use regex::Regex;

fn main() {
    let re = Regex::new(r"(?xmi)
        # x 自由间隔，可以使用注释，逻辑不受影响  
        # m 多行修饰符，允许 ^ 和 $ 分别匹配每行的开始和结尾  
        # i 忽略大小写
  
        # 匹配每行的开始（由于 m 修饰符，这将适用于字符串中的每一行）  
        ^  
          
        # 匹配单引号括起来的城市名（命名为 'local' 的捕获组）  
        '(?<local>[^']+)'  
          
        # 匹配一个或多个空白字符  
        \s+  
  
        # 匹配左括号  
        \(  
          
        # 匹配3到4个数字（命名为 'phone' 的捕获组）  
        (?<phone>[0-9]{3,4})  
          
        # 匹配右括号  
        \)  
          
        # 匹配每行的结束（由于 m 修饰符，这将适用于字符串中的每一行）  
        $  
    ").unwrap();

    let str = r#"
'Bei Jing' (010)
'Yin Chuan' (0951)
'Nan Jing' (025)
'Guang Zhou' (020)
    "#;

    // 使用捕获组的名称获取
    if let Some(c) = re.captures(str) {
        let local = c.name("local").unwrap();
        let phone = &c["phone"];
        println!("{}的区号是{}", local.as_str(), phone);

        // 捕获组的数量，0是整体的捕获
        for i in 0..re.captures_len() {// 3
            if let Some(capture) = c.get(i) {
                println!("{}={}", i, capture.as_str());
            }
        }
    }
    
    // 迭代所有匹配项并打印城市和区号
    for (_, [local, phone]) in re.captures_iter(str).map(|c| c.extract()) {
        println!("{}的区号是{}", local, phone);
    }

    // 如果捕获的内容太大，需要借助缓冲读取时，可以匹配目标位置的坐标
    let mut l = re.capture_locations();
    let _m = re.captures_read(&mut l, str);
    // 匹配到的结果是从第0个字符开始的偏移量：(1, 17)
    println!("{:?}", l.get(0).unwrap());
}
```

#### 匹配

```rust
use regex::Regex;

fn main() {
    let re = Regex::new(r".{8,}").unwrap();
    let str = "abc123def";
    println!("{}", re.is_match(str));// true

    let re = Regex::new(r"\d").unwrap();
    // 匹配成狗后会返回偏移位置
    println!("{:?}", re.shortest_match(str));// 4
}
```

#### 分隔

```rust
use regex::Regex;

fn main() {
    let arr = "123456789";
    let re = Regex::new("").unwrap();
    let v: Vec<&str> = re.split(&arr).collect();
    // ["", "1", "2", "3", "4", "5", "6", "7", "8", "9", ""]
    println!("{:?}", v);

    let v: Vec<&str> = re.splitn(&arr, 4).collect();
    // ["", "1", "2", "3456789"]
    println!("{:?}", v);

    let arr = "1,2,3,4,5,6,7,8,9";
    let re = Regex::new(",").unwrap();
    let v: Vec<&str> = re.split(&arr).collect();
    // ["1", "2", "3", "4", "5", "6", "7", "8", "9"]
    println!("{:?}", v);

    let v: Vec<&str> = re.splitn(&arr, 3).collect();
    // ["1", "2", "3,4,5,6,7,8,9"]
    println!("{:?}", v);
}
```

#### 替换

```rust
use regex::{NoExpand, Regex};

fn main() {
    let re = Regex::new(r"[^01]+").unwrap();
    // 10310
    println!("{}", re.replace("1078910", "3"));

    let re = Regex::new(r"(?<last>[^,\s]+),\s+(?<first>\S+)").unwrap();
    // $1和$2是捕获组的引用，${last}是命名捕获组的引用
    // World Hello World Hello
    println!("{}", re.replace("Hello, World", "$first ${last} $2 $1"));
    // 使用NoExpand来防止替换字符串中的'$'被解释为捕获组的引用
    // $first $1
    println!("{}", re.replace("Hello, World", NoExpand("$first $1")));

    let re = Regex::new(r#"(?m)^(\S+)[\s\r\n]+(\S+)$"#).unwrap();
    let hay = "
Greetings  1973
Wild\t1973
BornToRun\t\t\t\t1975
Darkness                    1978
TheRiver 1980
";
    // 全部替换
    println!("{}", re.replace_all(hay, "$2 $1"));
    // 只替换前3个
    println!("{}", re.replacen(hay, 3, "$2 $1"));
}
```

#### RegexSetBuilder

```rust
use regex::RegexSetBuilder;

fn main() {
    // 定义匹配密码的不同部分的正则表达式模式
    let patterns = vec![
        r".{8,}",      // 长度至少8个字符
        r"\d",         // 至少包含1个数字
        r"[a-z]",      // 至少包含1个小写字母
        r"[A-Z]",      // 至少包含1个大写字母
        r"[!@#$%^&*]", // 至少包含1个特殊字符
    ];

    // 使用RegexSetBuilder来构建RegexSet
    let regex_set = RegexSetBuilder::new(&patterns)
        .case_insensitive(false) // 区分大小写
        .build()
        .unwrap();

    let passwords = vec![
        "pwd",
        "LongPassword123",
        "pssw123",
        "ABCDE!",
        "Abcdefg123!",
        "Abcdefg_123!@#",
    ];

    println!("编码\t8个字符\t有数字\t有小写\t有大写\t有特殊\t密码");
    // 遍历并检查每个密码
    for (index, password) in passwords.iter().enumerate() {
        // 使用RegexSet来匹配文本
        let m = regex_set.matches(password);
        println!("{}\t{}\t{}\t{}\t{}\t{}\t{}",
                 index + 1,
                 m.matched(0),
                 m.matched(1),
                 m.matched(2),
                 m.matched(3),
                 m.matched(4),
                 password,
        );
    }
}
```

#### RegexBuilder

```rust
use regex::bytes::RegexBuilder;
use regex::Regex;

fn main() {
    let mut str = String::new();
    // 多行模式，^和$匹配每行的开始和结果，而不是整个文本的开始和结尾
    str.push_str(r"(?m)");
    // 命名捕获组timestamp，匹配形如YYYY-MM-DD HH:MM:SS的时间戳
    str.push_str(r"^(?<timestamp>\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2})");
    // \s 任意空白字符，包括空格，制表符，换行，换页

    // \s+ 匹配一个或多个空白字符
    str.push_str(r"\s+");

    // (?<level>...) 命名捕获组level，匹配一个或多个大写字母作为日志等级
    str.push_str(r"(?<level>[A-Z]+)");

    // \s* 匹配零个或多个空白字符
    str.push_str(r"\s*");

    // (?<bool>TRUE|FALSE) 命名捕获组bool，匹配TRUE或FALSE
    str.push_str(r"(?<bool>TRUE|FALSE)");

    // \s{1} 匹配一个空白字符
    str.push_str(r"\s{1}");

    // (?<message>...) 命名捕获组message，匹配一个或多个单词，单词间用空白字符分隔，并以点号结尾
    // (?:...) 是非捕获组，允许我们组合多个单词
    str.push_str(r"(?<message>[A-Za-z]+(?:\s+[A-Za-z]+)*[.])$");

    // (?m)^(?<timestamp>\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2})\s+(?<level>[A-Z]+)\s*(?<bool>TRUE|FALSE)\s{1}(?<message>[A-Za-z]+(?:\s+[A-Za-z]+)*[.])$
    println!("正则表达式：{}", str);
    
    let re = RegexBuilder::new(str.as_str())
        .case_insensitive(false) // 区分大小写
        .ignore_whitespace(false) // 不忽略空白符
        .multi_line(true) // 多行匹配
        .build().unwrap();

    let log_text = r#"
2023-03-15 10:00:00 INFO TRUE This is a log entry on a single line.
2023-03-15 10:00:05 ERROR TRUE This is a multiline
log entry that spans multiple lines.
It continues here.
2023-03-15 10:00:10 WARNING FALSE Another single line entry.
"#;

    for cap in re.captures_iter(log_text.as_bytes()) {
        println!("时间：{:?}\n等级：{:?}\n标记：{:?}\n消息：{:?}\n",
                 &cap.name("timestamp"),
                 &cap.name("level"),
                 &cap["bool"],
                 &cap.name("message"),
        );
    }
}
```

### 字符串解析

```rust
use std::str::FromStr;

#[derive(Debug, PartialEq)]
struct RGB {
    r: u8,
    g: u8,
    b: u8,
}

impl FromStr for RGB {
    type Err = std::num::ParseIntError;

    fn from_str(s: &str) -> Result<Self, Self::Err> {
        Ok(RGB {
            r: u8::from_str_radix(&s[1..3], 16)?,
            g: u8::from_str_radix(&s[3..5], 16)?,
            b: u8::from_str_radix(&s[5..7], 16)?,
        })
    }
}

fn main() {
    let color = "#3366CC";
    if let Ok(c) = RGB::from_str(color) {
        println!("{:?}", c);

        let rgb = RGB { r: 51, g: 102, b: 204 };
        println!("{}", rgb == c);
    }
}
```

## Web

[![actix-web](https://badge-cache.kominick.com/crates/v/actix-web.svg?label=actix-web)](https://docs.rs/actix-web/)[![log](https://badge-cache.kominick.com/crates/v/log.svg?label=log)](https://docs.rs/log/)[![env_logger](https://badge-cache.kominick.com/crates/v/env_logger.svg?label=env_logger)](https://docs.rs/env_logger/)

[Welcome | Actix Web](https://actix.rs/docs/)

### 基本

```rust
use std::io::Error;
use actix_web::{App, get, HttpResponse, HttpServer, main, post, Responder};
use actix_web::web::get;
use log::{info, LevelFilter};

#[main]
async fn main() -> Result<(), Error> {
    env_logger::builder().filter_level(LevelFilter::Trace).is_test(true).init();

    HttpServer::new(|| {
        App::new()
            // 添加处理链接的函数到服务
            .service(echo)
            .service(no_params)
            // 使用路由的方式，接收/hey的GET请求
            .route("/hey", get().to(manual_hello))
    }).bind(("localhost", 8080))?.run().await
}

// POST http://127.0.0.1:8080/echo
#[post("/echo")]
async fn echo(req_body: String) -> impl Responder {
    info!("接受到 Post /echo 请求，{}",req_body);
    HttpResponse::Ok().body(req_body)
}

// GET http://127.0.0.1:8080/
#[get("/")]
async fn no_params() -> impl Responder {
    info!("接受到 Get / 请求");
    HttpResponse::Ok().body("Hello world")
}

// GET http://127.0.0.1:8080/hey
async fn manual_hello() -> impl Responder {
    info!("接受到 路由 Get /hey 请求");
    HttpResponse::Ok().body("Hey there!")
}
```

### App

#### scope

```rust
use std::io::Error;
use actix_web::{App, get, HttpServer, main, Responder, web};

#[main]
async fn main() -> Result<(), Error> {
    HttpServer::new(|| {
        App::new()
            .service(
                // 指定应用的名称，类似于作用域的功能
                web::scope("/app")
                    // GET /app/index
                    .service(index)
            )
    }).bind(("localhost", 8080))?.run().await
}

#[get("/index")]
// http://127.0.0.1:8080/app/index
async fn index() -> impl Responder {
    "Hello World" // 返回静态文本
}
```

#### app_data

```rust
use std::io::Error;
use std::sync::Mutex;
use actix_web::{App, get, HttpServer, main};
use actix_web::web::Data;

#[main]
async fn main() -> Result<(), Error> {
    // web::Data 在 HttpServer::new 闭包外部创建
    let data = Data::new(Counter {
        counter: Mutex::new(0),
    });

    HttpServer::new(move || {
        App::new()
            .service(index)
            // app_data 是一种用于在应用程序的多个路由和中间件之间共享数据的机制
            .app_data(data.clone()) // 为了实现全局共享，必须在new闭包外创建，然后使用克隆实例
    }).bind(("localhost", 8080))?.run().await
}

struct Counter {
    counter: Mutex<u32>, // 使用互斥锁保护好这个计数器，避免多个访问请求产生数据竞争
}

// GET http://127.0.0.1:8080/
#[get("/")]
// 这个参数允许你访问在应用程序级别注册的状态数据，这里使用了计数器，记录访问次数
async fn index(data: Data<Counter>) -> String {
    let mut c = data.counter.lock().unwrap();
    *c += 1;
    format!("Hello {c}")
}
```

#### Host

```rust
use std::io::Error;
use actix_web::{App, HttpResponse, HttpServer, main, Responder};
use actix_web::guard::Host;
use actix_web::web::{scope, to};

#[main]
async fn main() -> Result<(), Error> {
    HttpServer::new(|| {
        App::new()
            // 在请求头中，Host不同，返回的结果也不同
            .service(scope("/")
                         .guard(Host("www.rust-lang.org"))
                         .route("", to(index)),
            )
            .service(scope("/")
                         .guard(Host("users.rust-lang.org"))
                         .route("", to(user)),
            )
            .route("/", to(none))
    })
        .bind(("localhost", 8080))?
        .run()
        .await
}

async fn none() -> impl Responder {
    HttpResponse::Ok().body("")
}

async fn index() -> impl Responder {
    HttpResponse::Ok().body("www")
}

async fn user() -> impl Responder {
    HttpResponse::Ok().body("user")
}
```

#### config

```rust
use std::io::Error;
use actix_web::{App, HttpResponse, HttpServer};
use actix_web::web::{get, post, resource, scope, ServiceConfig};

fn config(cfg: &mut ServiceConfig) {
    cfg.service(
        resource("/1")
            // GET 请求
            .route(get().to(|| async { HttpResponse::Ok().body("Hello World") }))
            // POST 请求不被允许
            .route(post().to(HttpResponse::MethodNotAllowed))
    );
}

#[actix_web::main]
async fn main() -> Result<(), Error> {
    HttpServer::new(|| {
        App::new()
            // 使用 configure 配置路由
            .configure(config)
            .service(scope("/2").configure(config))
    })
        .bind(("127.0.0.1", 8080))?
        .run()
        .await
}
```

### HttpServer

#### 多线程

```rust
use std::io::Error;
use std::thread::current;
use std::time::Duration;
use actix_web::{App, HttpServer, main, Responder};
use actix_web::web::get;
use log::{info, LevelFilter};

#[main]
async fn main() -> Result<(), Error> {
    env_logger::builder().filter_level(LevelFilter::Info).is_test(true).init();
    
    HttpServer::new(|| App::new()
        .route("/", get().to(sleep)))
        .workers(4)// 使用4个线程用于Http的服务
        .bind(("localhost", 8080))?.run().await
}

async fn sleep() -> impl Responder {
    info!("{:?}：sleep 开始执行", current().id());
    // 工作线程将在这里处理其他请求，不会被 sleep 挂起
    actix_web::rt::time::sleep(Duration::from_secs(5)).await;
    // std::thread::sleep(Duration::from_secs(5));
    info!("{:?}：sleep 执行完毕", current().id());
    "sleep ok"
}
```

#### TLS/HTTPS

[![openssl](https://badge-cache.kominick.com/crates/v/openssl.svg?label=openssl)](https://docs.rs/openssl/)[![openssl-sys](https://badge-cache.kominick.com/crates/v/openssl-sys.svg?label=openssl-sys)](https://docs.rs/openssl-sys/)

[Releases · openssl/openssl](https://github.com/openssl/openssl/releases)

[Win32/Win64 OpenSSL Installer for Windows](https://slproweb.com/products/Win32OpenSSL.html)

1. 使用OpenSSL生成私钥：

```bash
openssl genrsa -out key.pem 4096
```

2. 生成签名数字证书的请求

```bash
openssl req -new -key key.pem -out csr.pem -subj "/C=CN/ST=Beijing/L=Haidian/O=Rust/OU=Student/CN=Test"
```

3. 使用私钥和请求，生成自签名的数字证书

```bash
openssl x509 -req -in csr.pem -days 10 -sha256 -signkey key.pem -out cert.pem
```

```toml
[dependencies]
openssl = "0.10.64"
openssl-sys = "0.9.102"
actix-web = { version = "4.5.1", features = ["openssl"] }
```

```rust
use std::io::Error;
use actix_web::{App, get, HttpResponse, HttpServer, main, Responder};
use openssl::ssl::{SslAcceptor, SslFiletype, SslMethod};

#[main]
async fn main() -> Result<(), Error> {
    // 创建一个SslAcceptor对象，使用Mozilla推荐的中间配置和TLS方法
    let mut builder = SslAcceptor::mozilla_intermediate(SslMethod::tls()).unwrap();
    // 设置私钥文件
    builder.set_private_key_file("key.pem", SslFiletype::PEM).unwrap();
    // 设置证书链文件
    builder.set_certificate_chain_file("cert.pem").unwrap();

    // 创建一个新的HttpServer实例，并为其配置路由和应用
    HttpServer::new(|| App::new().service(index))
        // 绑定到本地主机的8080端口，并使用之前配置的SSL/TLS
        .bind_openssl("localhost:8080", builder)?
        .run().await
}

// https://127.0.0.1:8080/
#[get("/")]
async fn index() -> impl Responder {
    HttpResponse::Ok().body("Hello World")
}
```

#### keep_alive

```rust
use std::io::Error;
use std::time::Duration;
use actix_web::{App, get, HttpResponse, HttpServer, main, Responder};
use actix_web::http::{ConnectionType, KeepAlive};
use log::{info, LevelFilter};

#[main]
async fn main() -> Result<(), Error> {
    env_logger::builder().filter_level(LevelFilter::Trace).is_test(true).init();

    HttpServer::new(|| App::new().service(index))
        // 默认情况下，5秒后自动关闭连接
        .keep_alive(Duration::from_secs(10)) // 10秒后如果没有请求就自动关闭连接
        .keep_alive(KeepAlive::Os) // 依靠操作系统保持活动
        .keep_alive(None) // 禁用保持活动，所有请求都是一个连接，用完直接关闭
        .bind(("127.0.0.1", 8080)).unwrap().run().await
}

// http://127.0.0.1:8080/
#[get("/")]
async fn index() -> impl Responder {
    info!("接收到请求");
    let mut resp = HttpResponse::Ok()
        // 响应后强制关闭连接，即使请求中要求客户端保持连接
        .force_close()
        .body("Hello World");

    // 在响应的Headers中添加 Connection: Close
    resp.head_mut().set_connection_type(ConnectionType::Close);
    resp
}
```

> [!TIP]
>
> 1. HTTP/1.0
>    - 每个请求需要创建新的HTTP连接，然后才能传送有效的数据，因为服务端在每次请求结束后，默认关闭连接，减轻服务端的压力。
>    - 客户端发起请求时在`Headers`中加入`Connection: Keep-Alive`，服务端收到后，会在这次请求后保持连接，等待下次请求，避免重新握手。
>    - 服务端向客户端响应的`Headers`中也加入`Connection: Keep-Alive`,`Keep-Alive: TimeOut=10, Max=5`，告诉客户都连接已经保持和可用时间和次数。
> 2. HTTP/1.1
>    - 服务端默认启用了保持连接。如果需要短链接，客户端的请求中要添加属性`Connection: Close`，客户端收到后，处理完请求会自动关闭连接，并且在响应的`Headers`中也加入`Connection: Close`，告诉客户端请求的连接已经关闭。

#### 正常关闭

```rust
use std::io::Error;
use actix_web::{App, HttpResponse, HttpServer, main};
use actix_web::web::get;
use log::LevelFilter;

#[main]
async fn main() -> Result<(), Error> {
    env_logger::builder().filter_level(LevelFilter::Trace).is_test(true).init();

    HttpServer::new(|| App::new().route("/", get().to(|| async { HttpResponse::Ok().body("Ok") })))
        // 如果这个服务收到系统给出的SIGTERM信号，10秒内优雅的关闭
        // SIGINT：使用Ctrl+c，强制结束服务
        // SIGQUIT：使用Ctrl+\，强制结束的同时产生dump文件
        // SIGTERM： 使用 kill -15 <pid>，程序收到后会正常退出，执行清理和释放工作
        .shutdown_timeout(10)
        // 不处理系统给出的信号，会被系统强制终止
        .disable_signals()
        .bind(("127.0.0.1", 8080)).unwrap().run().await
}
```

### 数据提取

[![serde](https://badge-cache.kominick.com/crates/v/serde.svg?label=serde)](https://docs.rs/serde/)

#### 路径

```rust
use std::io::Error;
use actix_web::{App, get, HttpRequest, HttpResponse, HttpServer, main, Responder};
use actix_web::web::Path;
use log::{info, LevelFilter};
use serde::Deserialize;

#[main]
async fn main() -> Result<(), Error> {
    env_logger::builder().filter_level(LevelFilter::Trace).is_test(true).init();

    HttpServer::new(|| {
        App::new().service(index_1).service(index_2).service(index_3)
    }).bind(("localhost", 8080))?.run().await
}

// http://127.0.0.1:8080/1/1/a
// 方式一：使用结构体封装，需要支持反序列化
#[derive(Deserialize)]
struct Info {
    id: u32,
    name: String,
}

#[get("/1/{id}/{name}")]
async fn index_1(path: Path<Info>) -> impl Responder {
    let p = path.into_inner();
    info!("接受到 Get / 请求");
    HttpResponse::Ok().body(format!("Welcome {}, user_id {}", p.name, p.id))
}

// http://127.0.0.1:8080/2/1/a
// 方式二：使用元组封装
#[get("/2/{id}/{name}")]
async fn index_2(path: Path<(u32, String)>) -> impl Responder {
    let (id, name) = path.into_inner();
    info!("接受到 Get / 请求");
    HttpResponse::Ok().body(format!("Welcome {}, user_id {}", name, id))
}

// http://127.0.0.1:8080/3/1/a
// 方式三： 从请求实例中获取
#[get("/3/{id}/{name}")]
async fn index_3(req: HttpRequest) -> impl Responder {
    let id: u32 = req.match_info().get("id").unwrap().parse().unwrap();
    let name: String = req.match_info().get("name").unwrap().parse().unwrap();
    info!("接受到 Get / 请求");
    HttpResponse::Ok().body(format!("Welcome {}, user_id {}", name, id))
}
```

#### 查询

```rust
use std::io::Error;
use actix_web::{App, get, HttpResponse, HttpServer, main, Responder};
use actix_web::web::Query;
use log::{info, LevelFilter};
use serde::Deserialize;

#[main]
async fn main() -> Result<(), Error> {
    env_logger::builder().filter_level(LevelFilter::Trace).is_test(true).init();

    HttpServer::new(|| {
        App::new().service(index)
    }).bind(("localhost", 8080))?.run().await
}

#[derive(Deserialize)]
struct Info {
    id: u32,
    name: String,
}

// http://127.0.0.1:8080/?id=1&name=a
#[get("/")]
async fn index(path: Query<Info>) -> impl Responder {
    let p = path.into_inner();
    info!("接受到 Get / 请求");
    HttpResponse::Ok().body(format!("Welcome {}, user_id {}", p.name, p.id))
}
```

#### Mime

[![mime](https://badge-cache.kominick.com/crates/v/mime.svg?label=mime)](https://docs.rs/mime/)

用于描述发送到服务器的数据或从服务器接收的数据的媒体类型。

```rust
use std::io::Error;
use actix_web::{App, HttpResponse, HttpServer, main, post, Responder};
use actix_web::error::InternalError;
use actix_web::web::{Json, JsonConfig};
use log::{info, LevelFilter};
use serde::Deserialize;

#[main]
async fn main() -> Result<(), Error> {
    env_logger::builder().filter_level(LevelFilter::Trace).is_test(true).init();

    // 自定义 Json 提取器的配置
    let json = JsonConfig::default()
        // 限制请求数据的大小
        .limit(4096)
        // 只接受内容为JSON的数据类型
        .content_type(|mime| mime == mime::APPLICATION_JSON)
        // 错误处理
        .error_handler(|err, _req| {
            InternalError::from_response(err, HttpResponse::Conflict().into()).into()
        });

    HttpServer::new(move || {
        App::new().service(index).app_data(json.clone())
    }).bind(("localhost", 8080))?.run().await
}

#[derive(Deserialize)]
struct Info {
    id: u32,
    name: String,
}

// http://127.0.0.1:8080/submit
// Content-Type: application/json
// {"id": 1, "name": "a"}
#[post("/submit")]
async fn index(info: Json<Info>) -> impl Responder {
    info!("接受到 POST / 请求");
    HttpResponse::Ok().body(format!("Welcome {}, user_id {}", info.name, info.id))
}
```

#### 表单

```rust
use std::io::Error;
use actix_web::{App, HttpResponse, HttpServer, main, post, Responder};
use actix_web::web::Form;
use log::{info, LevelFilter};
use serde::Deserialize;

#[main]
async fn main() -> Result<(), Error> {
    env_logger::builder().filter_level(LevelFilter::Trace).is_test(true).init();

    HttpServer::new(|| {
        App::new().service(index)
    }).bind(("localhost", 8080))?.run().await
}

#[derive(Deserialize)]
struct Info {
    id: u32,
    name: String,
}

// http://127.0.0.1:8080/submit
// Content-Type: application/x-www-form-urlencoded
// id=1&name=a
#[post("/submit")]
async fn index(form: Form<Info>) -> impl Responder {
    info!("接受到 POST / 请求");
    HttpResponse::Ok().body(format!("Welcome {}, user_id {}", form.name, form.id))
}
```

### 响应

#### 自定义响应的数据

```rust
use actix_web::{App, get, HttpRequest, HttpResponse, HttpServer, main, Responder};
use actix_web::body::BoxBody;
use actix_web::http::header::ContentType;
use serde::Serialize;

#[derive(Serialize)]
struct Data {
    data: String,
}

impl Responder for Data {
    type Body = BoxBody;

    fn respond_to(self, _req: &HttpRequest) -> HttpResponse<Self::Body> {
        HttpResponse::Ok().content_type(ContentType::json()).body(self.data)
    }
}

#[get("/")]
async fn index() -> impl Responder {
    Data { data: "非常大的数据".to_string() }
}

#[main]
async fn main() -> std::io::Result<()> {
    HttpServer::new(|| { App::new().service(index) }).bind(("localhost", 8080))?.run().await
}
```

#### 流式处理响应的数据









#### 上传文件

```rust
use std::fs::create_dir_all;
use std::io::Write;
use actix_multipart::form::MultipartForm;
use actix_multipart::form::tempfile::{TempFile, TempFileConfig};
use actix_multipart::Multipart;
use actix_web::{App, Error, HttpResponse, HttpServer, main, middleware, post, Responder, web};
use futures_util::TryStreamExt;
use log::{info, LevelFilter};
use web::block;

#[derive(Debug, MultipartForm)]
struct UploadForm {
    // 使用 multipart 属性，指定该字段在表单中的名称为 "file"，限制10kb的大小
    #[multipart(rename = "file", limit = "10KB")]
    // 支持多文件上传
    files: Vec<TempFile>,
}

#[post("/1")]
// 使用 MultipartForm 来解析表单，并自动将表单数据映射到 UploadForm 结构体上
async fn save_files(MultipartForm(form): MultipartForm<UploadForm>) -> Result<impl Responder, std::io::Error> {
    // 遍历上传的文件列表
    for f in form.files {
        // 构造文件保存路径，使用文件名作为保存的文件名
        let path = format!("./tmp/{}", f.file_name.unwrap());
        info!("saving to {path}");
        // 调用 persist 方法将临时文件保存到指定路径
        f.file.persist(path)?;
    }
    Ok(HttpResponse::Ok())
}

// 手动处理 multipart/form-data 类型的表单上传
#[post("/2")]
async fn save_file_manual(mut payload: Multipart) -> Result<HttpResponse, Error> {
    while let Some(mut field) = payload.try_next().await? {
        // 表单的 Content-Type 必须是 multipart/form-data
        // 获取字段的 Content-Disposition 头信息
        let content_disposition = field.content_disposition();
        // 获取文件名，并忽略可能出现的错误
        let filename = content_disposition.get_filename().unwrap();
        // 构造文件保存路径
        let filepath = format!("./tmp/{}", filename);

        // 在单独的线程中执行阻塞操作（文件创建），这里可以使用线程池
        let mut f = block(|| std::fs::File::create(filepath)).await??;

        // 从 field 流中逐个获取数据块
        while let Some(chunk) = field.try_next().await? {
            // write_all 尝试将 chunk 的所有数据写入到某个文件或输出流 f 中
            f = block(move || {
                f.write_all(&chunk).map(|_| f)
            }).await??;
        }
    }

    Ok(HttpResponse::Ok().into())
}

#[main]
async fn main() -> Result<(), std::io::Error> {
    env_logger::builder().filter_level(LevelFilter::Info).is_test(true).init();
    // 创建临时上传目录
    create_dir_all("./tmp")?;

    HttpServer::new(|| {
        App::new()
            .wrap(middleware::Logger::default())
            // 设置临时文件的存储目录
            .app_data(TempFileConfig::default().directory("./tmp"))
            .service(save_file_manual)
            .service(save_files)
    })
        // 绑定服务器到指定的 IP 地址和端口
        .bind(("127.0.0.1", 8080))?
        // 设置工作线程数量
        .workers(2)
        .run()
        .await
}
```

# 附录

## 关键字

关键字在 Rust 语言中具有特定的语法和语义作用，因此不能用作变量名、函数名或其他标识符。

原文：[A - Keywords - The Rust Programming Language (rust-lang.org)](https://doc.rust-lang.org/book/appendix-01-keywords.html)

- `as`: 用于原始类型转换，消除类型歧义，或在语句中重命名项目。
- `async`: 用于声明异步函数，这些函数不会阻塞当前线程，而是返回一个 `Future`。
- `await`: 用于暂停执行直到异步操作的结果准备就绪。
- `break`: 立即退出循环。
- `const`: 用于定义常量项或常量原始指针。
- `continue`: 跳过当前循环迭代并开始下一个迭代。
- `crate`: 在模块路径中，指向 crate 的根目录。
- `dyn`: 用于动态调度到 trait 对象。
- `else`: 用于 `if` 控制流构造的备选分支。
- `enum`: 用于定义枚举类型。
- `extern`: 用于链接外部函数或变量。
- `false`: 布尔值 `false` 的字面量。
- `fn`: 用于定义函数或函数指针类型。
- `for`: 用于迭代迭代器中的项目，实现 trait，或指定高排名生命周期。
- `if`: 根据条件表达式的结果进行分支。
- `impl`: 实现固有的或 trait 的功能。
- `in`: `for` 循环语法的一部分。
- `let`: 绑定一个变量。
- `loop`: 无条件循环。
- `match`: 将值与模式进行匹配。
- `mod`: 定义模块。
- `move`: 使闭包接管其捕获的所有权。
- `mut`: 表示引用、原始指针或模式绑定中的可变性。
- `pub`: 在结构字段、块或模块中表示公共可见性。
- `ref`: 通过引用绑定。
- `return`: 从函数中返回。
- `Self`: 我们正在定义或实现的类型的别名。
- `self`: 方法主体或当前模块。
- `static`: 全局变量或程序执行期间持久的生命周期。
- `struct`: 用于定义结构体。
- `super`: 当前模块的父模块。
- `trait`: 用于定义 trait。
- `true`: 布尔值 `true` 的字面量。
- `type`: 定义类型别名或关联类型。
- `union`: 用于定义联合体；仅在联合体声明中作为关键字使用。
- `unsafe`: 表示不安全的代码、函数、trait 或实现。
- `use`: 将符号带入作用域。
- `where`: 表示约束类型的子句。
- `while`: 根据表达式的结果进行循环。

保留关键字：`abstract`, `become`, `box`, `do`, `final`, `macro`, `override`, `priv`, `try`, `typeof`, `unsized`, `virtual`, `yield`

## 符号

原文：[B - Operators and Symbols - The Rust Programming Language (rust-lang.org)](https://doc.rust-lang.org/book/appendix-02-operators.html)

| 运算符 | 示例                                             | 描述                  |    重载特征    |
| :----: | :----------------------------------------------- | :-------------------- | :------------: |
|  `!`   | `ident!(...)`, , `ident!{...}``ident![...]`      | 宏扩展                |                |
|  `!`   | `!expr`                                          | 按位或逻辑补码        |     `Not`      |
|  `!=`  | `expr != expr`                                   | 不相等比较            |  `PartialEq`   |
|  `%`   | `expr % expr`                                    | 算术余数              |     `Rem`      |
|  `%=`  | `var %= expr`                                    | 算数余数赋值          |  `RemAssign`   |
|  `&`   | `&expr`, `&mut expr`                             | 借用                  |                |
|  `&`   | `&type`, , , `&mut type``&'a type``&'a mut type` | 借用指针类型          |                |
|  `&`   | `expr & expr`                                    | 按位与                |    `BitAnd`    |
|  `&=`  | `var &= expr`                                    | 按位与赋值            | `BitAndAssign` |
|  `&&`  | `expr && expr`                                   | 短路按位与            |                |
|  `*`   | `expr * expr`                                    | 算术乘法              |     `Mul`      |
|  `*=`  | `var *= expr`                                    | 算术乘法赋值          |  `MulAssign`   |
|  `*`   | `*expr`                                          | 解引用                |    `Deref`     |
|  `*`   | `*const type`, `*mut type`                       | 原始指针              |                |
|  `+`   | `trait + trait`, `'a + trait`                    | 复合类型约束          |                |
|  `+`   | `expr + expr`                                    | 算术加法              |     `Add`      |
|  `+=`  | `var += expr`                                    | 算术加法赋值          |  `AddAssign`   |
|  `,`   | `expr, expr`                                     | 参数和元素分隔符      |                |
|  `-`   | `- expr`                                         | 算术负                |     `Neg`      |
|  `-`   | `expr - expr`                                    | 算术减法              |     `Sub`      |
|  `-=`  | `var -= expr`                                    | 算术减法赋值          |  `SubAssign`   |
|  `->`  | `fn(...) -> type`, `|...| -> type`               | 函数或闭包的返回类型  |                |
|  `.`   | `expr.ident`                                     | 成员访问              |                |
|  `..`  | `..`, , , `expr..``..expr``expr..expr`           | 右开区间字面量        |  `PartialOrd`  |
| `..=`  | `..=expr`, `expr..=expr`                         | 右闭区间字面量        |  `PartialOrd`  |
|  `..`  | `..expr`                                         | 更新结构体的剩余字段  |                |
|  `..`  | `variant(x, ..)`, `struct_type { x, .. }`        | 模式匹配的忽略绑定    |                |
| `...`  | `expr...expr`                                    | 已弃用，改用（`..=`） |                |
|  `/`   | `expr / expr`                                    | 算术除法              |     `Div`      |
|  `/=`  | `var /= expr`                                    | 算术除法赋值          |  `DivAssign`   |
|  `:`   | `pat: type`, `ident: type`                       | 类型约束              |                |
|  `:`   | `ident: expr`                                    | 结构体字段初始设定    |                |
|  `:`   | `'a: loop {...}`                                 | 循环的标签            |                |
|  `;`   | `expr;`                                          | 语句和项的终止符      |                |
|  `;`   | `[...; len]`                                     | 固定大小的数组的语法  |                |
|  `<<`  | `expr << expr`                                   | 左移                  |     `Shl`      |
| `<<=`  | `var <<= expr`                                   | 左移赋值              |  `ShlAssign`   |
|  `<`   | `expr < expr`                                    | 小于                  |  `PartialOrd`  |
|  `<=`  | `expr <= expr`                                   | 小于等于              |  `PartialOrd`  |
|  `=`   | `var = expr`, `ident = type`                     | 分配和赋值            |                |
|  `==`  | `expr == expr`                                   | 相等比较              |  `PartialEq`   |
|  `=>`  | `pat => expr`                                    | 模式匹配的分支指定    |                |
|  `>`   | `expr > expr`                                    | 大于                  |  `PartialOrd`  |
|  `>=`  | `expr >= expr`                                   | 大于等于              |  `PartialOrd`  |
|  `>>`  | `expr >> expr`                                   | 右移                  |     `Shr`      |
| `>>=`  | `var >>= expr`                                   | 右移赋值              |  `ShrAssign`   |
|  `@`   | `ident @ pat`                                    | 匹配绑定              |                |
|  `^`   | `expr ^ expr`                                    | 按位异或              |    `BitXor`    |
|  `^=`  | `var ^= expr`                                    | 按位异或赋值          | `BitXorAssign` |
|  `|`   | `pat | pat`                                      | 模式匹配的备选可能    |                |
|  `|`   | `expr | expr`                                    | 按位或                |    `BitOr`     |
|  `|=`  | `var |= expr`                                    | 按位或赋值            | `BitOrAssign`  |
|  `||`  | `expr || expr`                                   | 短路按位或            |                |
|  `?`   | `expr?`                                          | 错误传播              |                |


> [!CAUTION]
>
> 优先级：
>
> - 按位运算符的优先级低于比较运算符和算术运算符，但高于逻辑运算符。
>
> - 如果不确定运算符的优先级，可以使用括号来明确运算顺序。

Rust语言中没有三元运算符（如C/C++中的`?:`）。如果需要类似的功能，可以使用`if`表达式或`match`表达式来实现。


| 类型限定符号                                  | 描述                                                 |
| :-------------------------------------------- | :--------------------------------------------------- |
| `'ident`                                      | 生命周期或循环的标签                                 |
| `...u8`, , , , etc.`...i32``...f64``...usize` | 特定类型的数字字面量                                 |
| `"..."`                                       | 字符串字面量                                         |
| `r"..."`, , , etc.`r#"..."#``r##"..."##`      | 原始字符串字面量                                     |
| `b"..."`                                      | 字节字符串字面量                                     |
| `br"..."`, , , etc.`br#"..."#``br##"..."##`   | 原始字节字符串字面量                                 |
| `'...'`                                       | 字符字面量                                           |
| `b'...'`                                      | ASCII字节字面量                                      |
| `|...| expr`                                  | 闭包                                                 |
| `!`                                           | 修饰函数的返回值，函数没有返回值，定义无限循环和恐慌 |
| `_`                                           | 模式匹配的忽略值占位，数值字面量分隔                 |

| 泛型符号                       | 描述                                                         |
| ------------------------------ | ------------------------------------------------------------ |
| `path<...>`                    | 将参数指定为类型中的泛型类型                                 |
| `path::<...>`, `method::<...>` | 为表达式中的泛型类型、函数或方法指定参数类型                 |
| `fn ident<...> ...`            | 泛型函数                                                     |
| `struct ident<...> ...`        | 泛型结构体                                                   |
| `enum ident&lt;...&gt; ...`    | 泛型枚举                                                     |
| `impl<...> ...`                | 泛型实现                                                     |
| `for<...> type`                | 生命周期界限，`fn example<F>(f: F) -> &i32 where F: for<'a> Fn() -> &'a i32 {}` |
| `type<ident=type>`             | 为关联类型赋值                                               |

| 泛型约束和生命周期符号        | 描述                                                       |
| ----------------------------- | ---------------------------------------------------------- |
| `T: U`                        | 泛型参数 `T` 被限制为实现了 trait `U` 的类型               |
| `T: 'a`                       | 泛型类型 `T` 必须至少和生命周期 `'a` 一样长                |
| `T: 'static`                  | 泛型类型 `T` 不能包含任何非 `'static` 生命周期的借用引用。 |
| `'b: 'a`                      | 泛型生命周期 `'b` 必须至少和生命周期 `'a` 一样长           |
| `T: ?Sized`                   | 允许泛型类型参数 `T` 是动态大小的类型                      |
| `'a + trait`, `trait + trait` | 复合类型的约束                                             |

| 宏和属性符号                                | 描述       |
| ------------------------------------------- | ---------- |
| `#[meta]`                                   | 外部属性   |
| `#![meta]`                                  | 内部属性   |
| `$ident`                                    | 宏替换     |
| `$ident:kind`                               | 宏捕获     |
| `$(…)…`                                     | 宏重复语法 |
| `ident!(...)`, , `ident!{...}``ident![...]` | 宏调用     |

| 注释符号   | 描述           |
| ---------- | -------------- |
| `//`       | 行注释         |
| `//!`      | 内部行文档注释 |
| `///`      | 外部行文档注释 |
| `/*...*/`  | 块注释         |
| `/*!...*/` | 内部块文档注释 |
| `/**...*/` | 外部块文档注释 |

| 元组符号                 | 描述                                                         |
| ------------------------ | ------------------------------------------------------------ |
| `()`                     | 空元组（也称为单元），既是字面量也是类型，表示没有任何元素的元组 |
| `(expr)`                 | 括号表达式，用于改变表达式的优先级或用于函数调用             |
| `(expr,)`                | 单元素元组表达式，注意末尾的逗号，它区分了括号表达式和单元素元组 |
| `(type,)`                | 单元素元组类型，同样需要注意末尾的逗号                       |
| `(expr, ...)`            | 元组表达式，包含两个或更多个由逗号分隔的表达式               |
| `(type, ...)`            | 元组类型，包含两个或更多个由逗号分隔的类型                   |
| `expr(expr, ...)`        | 函数调用表达式；也用于初始化元组结构体和枚举                 |
| `expr.0`, , etc.`expr.1` | 元组索引，用于访问元组中的元素，索引从 0 开始                |

| 大括号符号   | 描述                           |
| :----------- | :----------------------------- |
| `{...}`      | 块表达式（Block expression）   |
| `Type {...}` | 结构体字面量（struct literal） |

| 中括号符号    | 描述                                       |
| :------------ | :----------------------------------------- |
| `[...]`       | 数组字面量                                 |
| `[expr; len]` | 数组字面量，包含 `len` 个 `expr` 的副本    |
| `[type; len]` | 数组类型，包含 `len` 个 `type` 的实例      |
| `expr[expr]`  | 集合索引，可以重载（`Index`、`IndexMut` ） |
| `expr[..]`    | 集合切片，表示整个集合                     |
| `expr[a..]`   | 集合切片，从索引 `a` 开始到集合末尾        |
| `expr[..b]`   | 集合切片，从集合开始到索引 `b`             |
| `expr[a..b]`  | 集合切片，从索引 `a` 到索引 `b`            |
