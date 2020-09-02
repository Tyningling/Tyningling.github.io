---
title: Swift 学习小记
date: 2020-09-03
description: 争取11月份之前造个东西出来玩。
draft: false
hideToc: false
enableToc: true
enableTocContent: true
author: lin
authorEmoji: 💪
type: BookNote
categories:
- BookNote
#系列
image: http://p2.music.126.net/dXtpz-a0_oNI4_7UWRVsRg==/18959978509471445.jpg
---

 **Swift 学习小记**

## 基础知识

#### let	常量声明 //?奇葩

#### var   变量声明

```swift
let constnumber : Int = 10;
var number : Int = 6000
print (number*10)
```

#### 注释  

```swift
//注释一
/*
注释二
注释二
注释二
*/
```

#### 字面量

```swift
let decInt = 17;     //十进制
let binInt = 0b10001;//二进制
let octInt = 0o21;   //八进制
let hexInt = 0x11;   //十六进制

/*
十进制浮点数 a：	若有一个十进制数的指数为exp 则该数相当于基数与10的exp次方的乘积
十六进制浮点数 b:	若有一个十六进制数的指数为exp 则该数相当于基数与2的exo次方的乘积
*/

// a:
let ExpDouble = 1.23456e1;
// print: ExpDouble = 1.23456 * 10^1 = 12.3456

// b:
let HexDouble = 0xFp2;
// print: HexDouble = 15.0 x 2^2 = 60.0
```

#### 类别名

```swift
typealias InkHin = UInt16;
var abc : InkHin = 3_000;
```

#### 元组

```swift
//大概就是只读版的特殊LIST？
	let Error60 = (60,"服务器连接失败");
//大致有两种应用方式
//a:
	print("状态码: \(Error60.0) 描述：\(Error60.1)");
//b:
	let (状态码, 描述) = Error60;
	print("code: \(状态码) message：\(描述)");
```

#### 可选类型与可选绑定

> 引用：https://www.cnblogs.com/junhuawang/p/6244944.html

#### 错误处理

```swift
func canThrowAnError() throws {
    // ...
}

do {
    try canThrowAnError()
}
catch {
    //错误抛出： .....
}
```

#### 断言

```swift
/*
	可以为程序增加规范性，当断言被触发时，程序代码将立即终止运行。
	大致有两种表示方式：
a:判断断言 assert(条件,<optional>描述) 当条件为false触发
b:直接断言。assertionFailure(描述) 立即触发
*/
var age = 17;
//a:
assert(age >= 18,"本店暂不接待未成人，感谢光临。")
//b:
if age >= 18 {
  	print("欢迎大人观顾，请跟我来。");
}else{
  	assertionFailure("本店拒接未成年人哦。");
}
```

#### 先决条件

```swift
/*
	与断言不同的是，断言需要被触发语句，而先决条件并不需要，它允许你在某块代码运行开始前就先检查代码的规范性。
	precondition(条件，<optional>描述)
*/
precondition(index > 0, "Index must be greater than zero.");
```

#### 关于分号

> 与其他大部分编程语言不同，Swift 并不强制要求你在每条语句的结尾处使用分号（`;`），当然，你也可以按照你自己的习惯添加分号。有一种情况下必须要用分号，即你打算在同一行内写多条独立的语句：
>
> ```
> let cat = "🐱"; print(cat)
> // 输出“🐱”
> ```



---2020.8.27记




## 基本运算符

> Swift支持大部分标准C语言的运算符，且为了减少常见编码错误做了部分改进。如：赋值符（`=`）不再有返回值，这样就消除了手误将判等运算符（`==`）写成赋值符导致代码错误的缺陷。算术运算符（`+`，`-`，`*`，`/`，`%` 等）的结果会被检测并禁止值溢出，以此来避免保存变量时由于变量大于或小于其类型所能承载的范围时导致的异常结果。
>

#### 赋值运算符

```swift
// 运算符 =
//大致有以下的情况
//a:
		var a = 1; 
		var b = 2;
		a = b;
//b:
		let (x,y) = (-1,1);
		print(x,y);
PS： Swift对=做出了改进，赋值运算不会返回任何值。
```

#### 算术运算符

```swift
/*
算术运算符	+ - * /
//这个大概不需要怎么介绍了,就是四则运算加减乘除，其中加号还可以用于字符串的连接。
*/
var a = 1
var b = -1
var r = a + b
var r2 = a - b
var r3 = a / b
var r4 = a * b
var r5 = "Result： " + String(r + r2 * r3 / r4 - a) + "\n"
print(r5);
```

#### 求余运算符

```swift
/*
	取余就是Mod() ,在类C系中以 % 表示。
	如 4 / 3 = 1 ······ 1
*/
	var a = 4 % 3; 
	print (a); // 此时 a的结果是余数1
```

#### 比较运算符

```swift
/*
	等于				==
	不等于			 !=
	大于				>
	小于				<
	大于等于 		>=
	小于等于		<=
	
比较运算符会返回布尔值，常用于条件语句。

*/
```

#### 三元运算符

```swift
/*
	三元运算符  问 ? 正: 否:
	三元实际是if的缩写形式
	if 问 {正} else {否} 
	例如：
*/
let age = 17
print(age>=18 ? "欢迎大人观顾，请跟我来" : "本店拒接未成年人哦。");
```

空盒<合>运算符

```swift
/*
解释：	
空合可以理解为空盒，在前面引用的对可选类型的解析中，可将可选类型看做一个盒子，那么因为盒子是密闭的，你无法知道里面是不是有东西，而你想往里面拿东西时，盒子里却什么都没有，那么就会得不到合理的返回值，此时就会程序报错。
	所以此时我们需要空合运算，它可以为返回值提供一个备用选项，这样就算盒子是空的，也能返回合理的值，让程序继续跑下去。
空合运算符： 可选类型 ?? 备选返回值
*/

var age : Int?; // 默认age为空盒子
//age = 16
var r = age ?? 18; // 当age是空盒子时返回18:Int
print(r);
```

#### 区间运算符

​	这个运算符挺有意思的，而且很方便。

##### 闭区间运算符

```swift
/*
用来表示	从 a 到 b 之间
闭区间运算符： a...b （记住这里是仨个点!
*/
for i in 1...10{
  print("\(i)")
}
```

##### 半开区间运算符

```swift
/*
其实可以称作左开右闭	用来表示从 a 到 b 但不包括b的区间 [a,b)
半开区间运算符 a..<b (这里是两个点！
*/
for i in 0..<10{
  print("\(i)")
}
```

##### 单侧区间

```swift
/*
	 单侧其实是前两个运算的另一种表达形式，表示往一侧无限延伸的区间 [-∞，a],[a,+∞]
	 一般应用在一些有序结构的表现，例如数组，列表。
		大致有以下几种方法：
*/
    let names = ["Yorkin","Cmjang","Error404","Eric","XXS","GHL"]
    let count = names.count
print("----a----")
// a:
    for name in names [2...] {
    print ("\(name)");
  }
print("----b----")
// b:
    for name in names [...3] {
    print ("\(name)");
  }
print("----c----")
// c:
    for name in names [..<4] {
    print ("\(name)");
  }
```

#### 逻辑运算符

```swift
/*
	逻辑非  !
	逻辑与  &&
	逻辑或  ||
这些就不记了，有学习过其他语言基础的应该都懂。
*/
```


---2020.8.28记



## 集合

> Swift 提供了三种主要的*集合类型*，所谓的数组、合集还有字典，用来储存值的集合。数组是有序的值的集合。合集是唯一值的无序集合。字典是无序的键值对集合。
>
> PS:Swift 的数组、合集和字典是以*泛型集合*实现的。要了解更多关于泛型类型和集合。

#### 数组

```swift
/**
1. 可变性 ： 当一个集合类型被赋值给一个常量，则集合就成了不可变的，即大小和内容都不能被改变。
2. 简写语法： Swift的完整写法是 Array<Element>，一般可以简写为[Element].
**/

//创建数组：
var someInts = [Int](); //创建空数组时必须显示类型
var fsomeInts = [1,2,3]; //非空时，填入值即可
var wsomeInts : Array<Int> = [1,2,3]; //完整写法
var esomeInts = Array<Int>();
var dsomeInts = Array(repeating: 1,count:3); // 创建数组[1,1,1]
var eqsomeInts = dsomeInts + fsomeInts; //创建数组[1,1,1,1,2,3]
//访问与修改数组
eqsomeInts.append(7); //向数组尾部增加新元素 7
eqsomeInts += [7]; //用赋值运算符的方式增加新元素7

eqsomeInts.insert(192,at:4); //在指定数组位置4前插入一个元素 值为192
eqsomeInts.remove(at:4); //删除指定数组位置4的元素
eqsomeInts.removeLast(); //删除数组最末尾的一个元素

eqsomeInts[6] = 8; //用赋值运算符修改位于7的元素值l
var a = eqsomeInts[6]; //获取数组位于7的元素值
eqsomeInts[4...6] = [99,88]; //使用下标脚本语法改变4-6范围的值

//遍历数组
for item in eqsomeInts{
    print( item );
}

//如果你需要每个元素以及值的整数索引，使用 enumerated()方法来遍历数组。

```
#### 合集

```swift
/*
	Swift的合集类型写作 Set<Element> 
	这里的 Element是合集要储存的类型。不同与数组，合集没有等价的简写。
*/

//创建并初始化一个空合集
	var l = Set<Character>();
	// print("\(l.count)");
/*
集合操作：
intersection(_:)：根据两个集合中都包含的值创建的一个新的集合。
symmetricDifference(_:)：根据在一个集合中但不在两个集合中的值创建一个新的集合。
union(_:)：根据两个集合的值创建一个新的集合。
subtracting(_:)：根据不在该集合中的值创建一个新的集合。

判断集合关系：
==：判断两个集合是否包含全部相同的值。
isSubset(of:)：判断一个集合中的值是否也被包含在另外一个集合中。
isSuperset(of:)：判断一个集合中包含另一个集合中所有的值。
isStrictSubset(of:)：判断一个集合是否是另外一个集合的子集合，并且两个集合并不相等。
isStrictSuperset(of:)：判断一个集合是否是另外一个集合的父集合，并且两个集合并不相等。
isDisjoint(with:)：判断两个集合是否不含有相同的值（是否没有交集）。
*/
```

#### 字典

```swift
/*
字典储存无序的互相关联的同一类型的键和同一类型的值的集合。
每一个值都与唯一的键相关联，它就好像这个值的身份标记一样。

Swift 的字典类型写全了是这样的： Dictionary<Key, Value>
同样可以用简写的形式来写字典的类型为 [Key: Value]
*/

//创建空字典：
var namesOfIntegers = [Int: String]();
namesOfIntegers[16] = "9"; 
namesOfIntegers = [:];//用字典字面量创建空字典

```

---2020.9.1记

## 控制流

####  For - in 循环

```swift
/*
	for-in循环实际上是从C-type变革来的，从swift3以后清除了C-type的for循环，所以这里只记for-in
	使用 for-in 循环来遍历序列，比如一个范围的数字，数组中的元素或者字符串中的字符。
*/

// 1.  遍历数组
let names = ["A", "B", "C", "D"];
for name in names { // for var name; i < names.count; i+=1 
  print("\(name)"); 
}

// 2.遍历字典
let Dnum = ["a" : 8, "b" : 2, "c" : 3];
for (棍,冰) in Dnum {
  print("\(棍)   \(冰)");
}

// 3.数字区间
for index in 0...6{
  print("\(index)");
  //print [0,6]
}

// 4.下划线取代初始值
var r = 0;
for _ in 0...6{
  r += 1
}
print(r);

// 5.操作符区间
for index in 0..<10
{
  print("\(index)")
}
//递减
for index in (0..<10).reversed()
{
	print(i)
}

// 6. 当for-in的步长不为1的时候，可以使用stride()函数来控制循环。

//递增
for i in stride(from: 0, to: 10 ,by: 2) { // i=0; i<10; i+=2
    print(i)
}
//递减
for i in stride(from: 10, through: 0, by: -2) { // i=10; i>=0; i-=2
    print(i)
}
```

#### While 循环

```swift
/*
	While 循环执行一个合集的语句直到条件变成 false.
	Swift 提供了两种 while 循环：
		while:在每次循环开始的时候计算它自己的条件
		repeat-while:在每次循环结束的时候计算它自己的条件
其实就是while和do while 啦，也就这两种情况。
*/
var a = 1;
var b = true;

//do while:
repeat{ //先执行一遍这里再开始循环的判断
    a += 1;
      if a == 3 {b = false; a = 1;};
}while b //当b为false时结束循环

print(a);
b = true;

// while:
while b //当b为false时开始循环
{
  a += 1;
  if a == 3 {b = false};
}
print(a);

```

## 条件语句

#### If 语句 

```swift
/*
	这个没啥好记的。
*/
```

#### Switch 语句

```swift
/*
	switch 是可以进行VB中select case一样的匹配，一般类C都有这么一个东西，貌似使用的场景不多
*/
let AChar : Character = "A";
switch AChar {
  case "A" : print("1");
  case "B" : print("2");
  case "C" : print("3");
  case "D" : print("4");
  default: print("0"); //默认
}

/*
	PS:
整个 switch 语句会在匹配到第一个 switch 情况执行完毕之后退出，不再需要显式的 break 语句。这使得 switch 语句比 C 的更安全和易用，并且避免了意外地执行多个 switch 情况。
*/
```

#### Guard 语句

```swift
/*
	这个好像是Swift专属的关键字？ 以前没见过。
	有几个注意的点： guard 必须用在函数内部，guard必须带有else语句,
	格式是如下这样：
*/
	func xxx (age:Int) {
		guard age >= 18 else{ 
			print("本店暂不接待未成人，感谢光临。");
			return;
		}
	}

```

#### 控制转移语句

```swift
/*
	Swift中的控制转移有五种： continue break fallthrough return throw
*/
// continue : 跳出一次循环 下面是一个为字符串去除指定字符的代码
  let puzzleInput = "great minds think alike"
  var puzzleOutput = ""
  let charactersToRemove: [Character] = ["a", "e", "i", "o", "u", " "]
  for character in puzzleInput {
      if charactersToRemove.contains(character) {
          continue
      } else {
          puzzleOutput.append(character)
      }
  }
  print(puzzleOutput)

//Break : 立即跳出所在控制流 下面是一个匹配Char值的代码
  let numberSymbol: Character = "三"  // Simplified Chinese for the number 3
  var possibleIntegerValue: Int?
  switch numberSymbol {
  case "1", "١", "一", "๑":
      possibleIntegerValue = 1
  case "2", "٢", "二", "๒":
      possibleIntegerValue = 2
  case "3", "٣", "三", "๓":
      possibleIntegerValue = 3
  case "4", "٤", "四", "๔":
      possibleIntegerValue = 4
  default:
      break
  }
  if let integerValue = possibleIntegerValue {
      print("The integer value of \(numberSymbol) is \(integerValue).")
  } else {
      print("An integer value could not be found for \(numberSymbol).")
  }
 
//Fallthrough : 前面提到swift的 Switch语句在匹配到对应值后就会立即退出，这个语句可以让它像其他类C一样继续往下走一段，但这一段不会检查swicth条件。
 let names = "小学生"
  var charm = 10
  switch names {
  case "加冰": charm += 20;
  case "教主": charm += 25;
  case "阿贤": charm += 40;
  case "小学生": charm += 35;fallthrough;
  case "Error404": charm -= 20;
  case "Eric": charm += 20;
  case "wey" : charm += 30;
  case "icelolly" : charm += 50;
  default: charm += 5;
}
// print charm = 25
```

#### 语句标签

```swift
/*
	swift  中虽然没有GOTO 但是标签仍然还是有存在的必要性的，例如你可以用XXX标签来标识一段控制流，此时你可以轻松的利用标签对它施展Break 等魔法。
*/
var i = 0;
InkHin:
while true {
    i += 1;
    guard i < 5000 else {break InkHin};
}
```

---2020.9.2记

