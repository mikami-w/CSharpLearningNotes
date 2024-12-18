# 第二章 核心C#

## 程序框架

```c#
using System;
namespace Mikami
{
	class Program
	{
		static void Main(string[] args)
		{
			string obj="world"
			Console.Write($"Hello,{obj}!");
		}
	}
}
```

### Main()方法

* 注意程序入口Main**首字母大写**
* Main函数参数可选, 返回值类型void或int, public修饰符可选
* 因为程序运行不会创建Program实例,所以Main声明为static
* 可在项目属性中决定可选参数
* 使用命令行运行时, 只需在`dotnet run`命令后提供参数: `dotnet run arg1 arg2 arg3 ...`;
  * 若参数与命令冲突, 可在参数前添加`--`: `dotnet run -- arg1 arg2 ...`

## 初始化变量

* 类或结构中的字段若显式初始化, 则默认值为0

* 方法局部变量必须显式初始化

* 方法局部变量可使用var自动类型推断

## 作用域

* 局部变量作用域位于从被声明开始到所在最小块结束
* for, while或类似语句中声明的局部变量作用域为循环体内
* 若**方法内嵌套块与外层块同名局部变量作用域冲突, 则会产生编译错误**(不同于C++的覆盖)
* 若**同名字段与方法局部变量作用域冲突, 则方法中的局部变量会覆盖字段, 不产生编译错误**, 访问字段时使用 object.fieldname访问字段

## 常量

* 常量必须在编译时能够计算值, 否则应使用只读字段
* 常量总是隐式静态的, 不需(也不允许)在声明中使用const限定符

## 预定义数据类型

### 值类型和引用类型

​	考虑代码 `typename valuename = initialvalue;`

* 若typename为值类型,则变量valuename将在栈上分配内存存储**值本身**;
* 若typename为引用类型,则变量在栈上分配内存存储**引用**, 使用new运算符在托管堆上创建实例对象, initialvalue可以为new运算符创建的实例或null

​	一般地, **赋值总是复制栈上的数据**(值类型的值或引用类型的引用).

### 预定义整形

| 名称   | .NET类型      | 说明       |
| ------ | :------------ | ---------- |
| sbyte  | System.SByte  | 8位有符号  |
| short  | System.Int16  | 16位有符号 |
| int    | System.Int32  | 32位有符号 |
| long   | System.Int64  | 64位有符号 |
| byte   | System.Byte   | 8位无符号  |
| ushort | System.UInt16 | 16位无符号 |
| uint   | System.UInt32 | 32位无符号 |
| ulong  | System.UInt64 | 64位无符号 |

* byte与char不会隐式转换
* 只有byte默认无符号, 其他类型默认有符号
* 可在数字间任意不连续插入下划线帮助阅读而不影响其他功能, 如`long l=0x123_456_789_abc_def;`
* 十六进制字面值使用前缀0x, 二进制字面值使用前缀0b

### 预定义浮点型

| 名称    | .NET类型       | 说明                                   | 位数  |
| ------- | -------------- | -------------------------------------- | ----- |
| float   | System.Single  | 32位单精度浮点数                       | 7     |
| double  | System.Double  | 64位双精度浮点数                       | 15/16 |
| decimal | System.Demical | 128位高精度十进制数表示法, 字面值后缀M | 28    |

### bool类型

* .NET类型: System.Boolean
* 表示true或false
* 不能和整数值等相互隐式转换, 如果试图用0表示false, 用非0表示true, 就会出错

### 字符类型 char

* .NET类型: System.Char
* 表示一个**16位Unicode字符**
* 可以使用单引号字符字面量, 4位Unicode值(如'\u0041'), 强制转换的整数值((char)65)或十六进制数('\x0041'), 或转义序列来表示它们

## 预定义引用类型

### object类型

* .NET类型: System.Object
* 根类型,其他类型(包括值类型)都由它派生而来
* 可以使用object引用绑定任意对象

| 成员方法                                                     | 说明                                                         |
| :----------------------------------------------------------- | ------------------------------------------------------------ |
| Object()                                                     | 构造函数                                                     |
| [Equals(Object)](https://learn.microsoft.com/zh-cn/dotnet/api/system.object.equals?view=net-8.0#system-object-equals(system-object)) | 确定指定对象是否等于当前对象。                               |
| [Equals(Object, Object)](https://learn.microsoft.com/zh-cn/dotnet/api/system.object.equals?view=net-8.0#system-object-equals(system-object-system-object)) | 确定指定的对象实例是否被视为相等。                           |
| [Finalize()](https://learn.microsoft.com/zh-cn/dotnet/api/system.object.finalize?view=net-8.0#system-object-finalize) | 在垃圾回收将某一对象回收前允许该对象尝试释放资源并执行其他清理操作。 |
| [GetHashCode()](https://learn.microsoft.com/zh-cn/dotnet/api/system.object.gethashcode?view=net-8.0#system-object-gethashcode) | 作为默认哈希函数。                                           |
| [GetType()](https://learn.microsoft.com/zh-cn/dotnet/api/system.object.gettype?view=net-8.0#system-object-gettype) | 获取当前实例的 [Type](https://learn.microsoft.com/zh-cn/dotnet/api/system.type?view=net-8.0)。 |
| [MemberwiseClone()](https://learn.microsoft.com/zh-cn/dotnet/api/system.object.memberwiseclone?view=net-8.0#system-object-memberwiseclone) | 创建当前 [Object](https://learn.microsoft.com/zh-cn/dotnet/api/system.object?view=net-8.0) 的浅表副本。 |
| [ReferenceEquals(Object, Object)](https://learn.microsoft.com/zh-cn/dotnet/api/system.object.referenceequals?view=net-8.0#system-object-referenceequals(system-object-system-object)) | 确定指定的 [Object](https://learn.microsoft.com/zh-cn/dotnet/api/system.object?view=net-8.0) 实例是否是相同的实例。 |
| [ToString()](https://learn.microsoft.com/zh-cn/dotnet/api/system.object.tostring?view=net-8.0#system-object-tostring) | 返回表示当前对象的字符串。                                   |

### string类型

* string 对象是**不可变的**, 修改任意一处就会创造一个全新的string对象, 而原来的对象不发生改变
* 在字符串字面量前加上@, 字符串内不会有字符被解释为转义字符
  * 甚至允许在其中包含换行符
* 用前缀$标记字符串插值格式, 允许在花括号内包含变量或表达式, 花括号内的值会被计算后成为字符串的一部分
* string 类型的`==`运算符与`Equals()`方法均为比较字符串内容, 而不是比较引用

## 程序流控制

​	`if()...else...` `while()` `do...while()` `for(;;)` `break` `continue`语句同C++.

​	局部变量可以使用`var`声明类型确定的变量, 把类型推断交给编译器(参考C++的自动类型推断`auto`). var用于局部变量的自动类型推断, 而不能用于类或结构的成员.

* foreach循环用于迭代集合中的每一项
  * **需支持IEnumerable接口**
  * 不能改变被迭代集合元素的值

```c#
foreach(var elem in arrayOfInts)
{
    //do something......
}
```

* switch语句, 不同于C++, 如果激活了块中靠前的一条case, 后面的case就不会被激活, 除非使用goto
  * 编译器把没有break语句的case字句标记为错误
  * 但有一种例外, 如果一条case字句为空, 就可以直接跳到下一条case, 这样可以用相同方式处理多条case
  * 不同于C++, **可以把字符串常量作为case的测试量**

## 命名空间

* 最外层命名空间可以使用.分隔的多级名称, 但不允许声明嵌套在另一个命名空间中的多部分名称命名空间
* 命名空间与程序集无关. 同一程序集可以有不同命名空间, 也可以在不同程序集定义同一命名空间的类型
  * 程序集: 单个或多个源代码文件编译生成的exe或dll文件, 一个程序集最多有一个Main函数
* 允许使用using语句将某一命名空间内的类型导入到当前程序集, 若出现名称冲突只需使用相应命名空间限定
* 允许使用using语句为命名空间指定别名: `using alias = NamespaceName;` 
  * 命名空间别名修饰符是`::`, 使用`::`访问相应别名命名空间的类型, 非别名命名空间使用`.`访问

## 预处理指令

​	预处理指令不用分号结束, 一般一行上只有一条命令. 如果编译器遇到一条预处理指令, 就会假定下一条命令在下一行. 不同于C++, **预处理指令实际是由编译器处理的**.

* `#define #undef`

​	不同于C++, `#define`指令只是告诉编译器存在特定符号, 这个符号没有任何意义. 如果符号已经存在, 则#define不起任何作用. `#undef`用于删除符号定义, 如果符号不存在则#undef无意义.
​	必须把#define和#undef命令放在C#源文件开头位置, 在声明要编译的任何对象的代码之前. 
​	#define本身没有任何作用, 与其他预处理指令(特别是`#if`)结合使用时才能发挥作用.

* `#if #elif #else #endif`

​	用于条件编译. #if, #elif还支持一组逻辑运算符:`! == != && ||`. 如果符号存在, 符号就被解释为true, 否则为false.

```c#
#define ENTERPRISE
#define W10
//do something
#if ENTERPRISE
//do something
#endif
#if PROFESSIONAL
//do something professional
#elif W10 && (PROFESSIONAL==false)
//do something unprofessional
#else    
//do something
#endif
```

* `#warning #error`

​	当编译器遇到这两个命令时,会分别产生警告和错误, 并显示后面的文本或输出后面的文本作为编译错误信息. 

```c#
#if DEBUG && RELEASE
#error "You've defined DEBUG and RELEASE simultaneously!"
#endif
#warning "Don't forget to remove these lines before the boss tests the code!"
```

* `#region #endregion`

​	用于把一段代码视为有给定名称的一个块, 如下所示:

```c#
#region Member Field Declarations
int x;
double d;
Currency balance;
#endregion
```

​	这看起来似乎没什么用, 因为它根本不影响编译过程. 这些指令真正的优点是可以被某些编辑器识别, 包括Visual Studio编辑器. 这些编辑器可以使用这些指令使代码在屏幕上更好地布局.

* `#line`

​	用于改变编译器在warning和error信息中显示的文件名和行号信息. 使用`#line default`把行号还原为默认行号.

* `#pragma`

​	用于抑制或还原指定的编译警告. 与命令行选项不同, #pragma指令可以在类或方法级别实现, 对抑制警告的内容和抑制的时间进行更精细的控制. 下面的例子禁止"字段未使用"警告, 然后在编译 MyClass 类后还原该警告.

```c#
#pragma warning disable 169
public class MyClass
{
    int neverUsedField;
}
#pragma warning restore 169
```

## C#编程准则

### 标识符

* 标识符区分大小写, 可以包含数字, 字母和下划线, 但必须以字母或下划线开头.
* 不能把C#保留的关键字用作标识符, 除非在标识符前加上符号@, 告知编译器其后内容是一个标识符,而不是C#关键字(如`event`不是有效的标识符, 但`@event`是).
* 标识符可以包含Unicode字符, 用`\uXXXX`指定.

### 用法约定

1.命名约定

​	C#的约定是命名变量时一般不使用任何前缀.

> Hungarian notation(匈牙利命名法)使用带有表示类型的前缀字母来命名变量,如`LAccountNum`表示变量是一个长整型(long). 在C#中只有少量数据类型使用了这种命名法,如接口(interface).

​	名称大小写通常使用 PascalCase 形式, 最好不要使用带有下划线的变量名.
​	一些特殊情况推荐使用 lowerCamelCase 形式:

* 类型中所有私有成员字段的名称

  * 但要注意, 成员字段名称常常以一条下划线开头

* 传递给方法的所有参数名称

* 用于区分同名两个对象: 比较常见的是属性封装字段:

  ```c#
  class MyClass
  {
      private string employeeIdentity
      private string _employeeName;
      public string EmployeeName
      {
          get{ return employeeName; }
      }
  }
  ```
  
  * 如果这样做, 则局部变量使用 lowerCamelCase, 私有成员总是使用 _lowerCamelCase, 而共有的或受保护的成员总是使用 PascalCase 
    * 在被封装为属性的私有字段前加上下划线可以为区分字段与局部变量提供极大的便利

2.属性和方法的使用

​	一般情况下, 如果某成员外观像变量, 用法像变量, 那么应该使用属性来表示, 而不是方法.
​	一般情况下, 读取某属性不应该有任何明显的和不希望的负面效果.
​	可以按照任何顺序设置属性, 最好不要因为还没有设置另一个相关属性而抛出异常. 例如, 当用户输入账号密码时, 不应因为先输入密码而抛出找不到账号的异常.
​	顺序读取属性应有相同效果.如果属性的值可能出现预料不到的改变, 就应该将其编写为一个方法. 

* 在监控汽车运动的类中, 把speed设置为属性就不合适, 而应当使用GetSpeed()方法; 另一方面,应把Weight和EngineSize设置为属性, 因为对于给定的对象, 它们是不变的.

​	如果相关项满足上述效果, 则应该使用属性, 否则应使用方法.

3.字段的使用

​	字段总是私有的, 但在某些情况下也可以将常量或只读字段设置为公有.

​	应当注意, 这些准则与语言规范不同, 用户应当尽可能遵守这些准则, 但如果有很好的理由不遵守它们, 完全不会有什么问题. 准则应当是正确的决策, 而不是一种束缚.

> ValueTuple 类型包含公有字段, 而旧的 Tuple 类型使用属性. 微软打破了自己为字段定义的准则, 这是因为元组的变量可以想int一样简单, 而使用属性在这种情况下会相对严重地影响性能. 这只是说明没有无例外的规则.
