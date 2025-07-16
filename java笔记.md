# 注释
单行注释
//
可加在上方或后方

多行注释
/*
注释内容
*/


文档注释
/**  写完enter自动完整,见calculator3
注释内容
*/

# java编译步骤

1.记事本编写java**原文件**保存成 hello.java
2.命令提示符里将.java转成**字节码** A.class文件
![image-20250217162638964.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/image-20250217162638964.png)

3.运行 A.class文件 **机器码**被CPU执行
![image-20250217162852378.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/image-20250217162852378.png)

![image-20250217162907143.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/image-20250217162907143.png)


 
# 快捷键
alt+enter 万能！报错地方右键导入相关资源
alt + enter 快速生成前面的类型和变量名
alt + enter 选中边框，整个运算条件取反
![微信截图_20250218094138.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250218094138.png)
alt +enter 选中对应变量，内联变量
![微信截图_20250219160549.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250219160549.png)

alt + insert 类内，选中字段，生成构造函数

ctrl + n 查看所有帮助文档

ctrl + f5 运行代码

ctrl + f12 查看javadoc

ctrl + alt + b查看diagram的子接口

ctrl + alt +L 格式化代码

ctrl + alt + m 选中整段代码，抽取成方法

ctrl + shift +enter 移入下一行

ctrl + n 快速查找

ctrl + j 列出所有快捷模版

ctrl + /  注释、取消注释当前行

ctrl + shift + / 注释多行

ctrl + 鼠标左键 选中函数进对应帮助文件/对应父类文件
ctrl + b 选中函数进对应帮助文件/对应父类文件

shift + f6 对类重命名

Shift + Alt + ↑（上移） / Shift + Alt + ↓（下移）

# 字符及运算符
## 符号各种移
### **1.** **符号左移（****Arithmetic Left Shift, <<****）**

- **公式**：`x << n = x *(2^n)`
- **规则**：
  - 所有 **位左移** **n** **位**，低位补 0。
  - 相当于 x 乘以 2^n。
  - **符号位不变**，如果 x 是正数，仍然是正数；如果 x 是负数，仍然是负数（但可能溢出）。

**示例**

int x = 5;  // 0000 0101 (5)
int y = x << 2; // 0001 0100 (20)
System.out.println(y); // 20

**计算过程：**
5 (0000 0101) << 2 => (0001 0100) = 20


### **2.** **符号右移（****Arithmetic Right Shift, >>****）**
- **公式**：x >> n = x / (2^n)
- **规则**：
  - **符号位（最高位）保持不变**，负数补 1，正数补 0。
  - 相当于 x 整除 2^n，**向下取整**。
  - 适用于 **带符号整数**，负数仍然是负数，正数仍然是正数。

**示例**
int x = 20; // 0001 0100 (20)
int y = x >> 2; // 0000 0101 (5)
System.out.println(y); // 5

int x = -20; // 1110 1100 (-20, 使用二补数表示)
int y = x >> 2; // 1111 1011 (-5)
System.out.println(y); // -5

 

### **3.** **无符号右移（****Logical Right Shift, >>>****）**
- **公式**：x >>> n = floor( x / (2^n) )（但不保留符号）
- **规则**：
  - **所有位右移** **n** **位**，无论正负 **高位补** **0**。
  - 适用于 **无符号整数**，将负数当作无符号数处理，转换为正数。
  - **Python** **没有**      **>>>**，可以用 x // (2^n) & 0xFFFFFFFF 模拟。

 

int x = 20; // 0001 0100 (20)
int y = x >>> 2; // 0000 0101 (5)
System.out.println(y); // 5

 

### **4.** **无符号左移（****Logical Left Shift, <<<****）**
🚨 **Java** **没有** **<<<** **操作符！** 只有 <<。
- 因为 **所有整数在** **Java** **中都是有符号的**，<< 逻辑上就等于无符号左移。
- **在无符号整数中，****<<** **也相当于无符号左移**，所以 Java 不需要 <<<。

![微信截图_20250217164521.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250217164521.png)
![微信截图_20250217165452.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250217165452.png)


## 字符类型
char字符值
string字符串值，文本块

![微信截图_20250217184246.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250217184246.png)

## 整数类型

byte

int

short

long

## 小数类型

float

double
![微信截图_20250217170140.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250217170140.png)
##### **类型 变量名 = 值**

例如

var scanner = new Scanner(System.in)

**为什么有new**

| **情况**                     | **是否需要 `new`** | **示例**                                    |
| ---------------------------- | ------------------ | ------------------------------------------- |
| **普通对象**                 | ✅ 需要             | `Scanner scanner = new Scanner(System.in);` |
| **数组（字面量初始化）**     | ❌ 不需要           | `String[] arr = {"a", "b", "c"};`           |
| **数组（动态创建）**         | ✅ 需要             | `String[] arr = new String[3];`             |
| **基本数据类型**             | ❌ 不需要           | `int a = 10;`                               |
| **包装类（推荐 `valueOf`）** | ✅ 可选             | `Integer num = Integer.valueOf(10);`        |
| **Java 10 `var`**            | ❌ 但对象仍需 `new` | `var list = List.of(1, 2, 3);`              |

++ 自加1

-- 自减1
![微信截图_20250217185124.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250217185124.png)

## 逻辑运算符

|| or
&& and
## &
### 按位与（Bitwise AND）
按位与（`&`）运算符对**二进制表示的每一位**进行 **与（AND）** 运算。

**规则：**

- `1 & 1 = 1`
- `1 & 0 = 0`
- `0 & 1 = 0`
- `0 & 0 = 0`

计算 `5 & 3`
```
  0101   (5)
& 0011   (3)
------------
  0001   (1)  <- 计算结果
```

**逐位计算：**

- **第1位**（最右侧）： `1 & 1 = 1`
- **第2位**： `0 & 1 = 0`
- **第3位**： `1 & 0 = 0`
- **第4位**： `0 & 0 = 0`

### 逻辑与（Logical AND）

`&` 还可以用在 **布尔（boolean）运算** 中：
- **`a & b` 只有在 `a` 和 `b` 都是 `true` 时才返回 `true`**
- **与 `&&`（短路与）不同，`&` 总会计算两边的表达式**。

| 符号   | 名称  | 作用                         | 短路机制                        |
| ---- | --- | -------------------------- | --------------------------- |
| `&`  | 逻辑与 | 只有当两边都为 `true` 时才返回 `true` | **不会**短路，始终执行两边             |
| `&&` | 短路与 | 只有当两边都为 `true` 时才返回 `true` | **会**短路，左侧为 `false` 时右侧不会执行 |
示例
```java
public class LogicalAndVsShortCircuit {
    public static void main(String[] args) {
        int x = 5;
        
        // 使用 `&`，两边都会执行
        if (x > 0 & x++ > 2) {
            System.out.println("x after &: " + x); // 输出 6
        }
        
        x = 5; // 重置
        
        // 使用 `&&`，如果左边为 false，则右边不会执行
        if (x < 0 && x++ > 2) {
            System.out.println("This won't be printed");
        }
        System.out.println("x after &&: " + x); // 输出 5
    }
}
```

- `&` 计算时，即使 `x > 0` 是 `true`，也会计算 `x++ > 2`，所以 `x` 变成 `6`。
- `&&` 短路优化：`x < 0` 是 `false`，所以 `x++ > 2` 不执行，`x` 仍然是 `5`。
# if 
## 1.只有if 没有else

## 2.if - else 
else后不能加条件
`if (condition1) {`
`// 执行代码块1`;` 
`} else {` 
`// 执行代码块2`;` 
`}`
## 3.if - else if - else
`if (condition1) {`
    `// 执行代码块1`
`} else if (condition2) {`
    `// 执行代码块2`
`} else {`
    `// 执行代码块3（默认情况）`
`}`
这里可以有很多个else-if,可以没有最后的else

1. return断掉后面的执行
![微信截图_20250218091836.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250218091836.png)

2. alt + enter 选中边框，整个运算条件取反
![微信截图_20250218094138.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250218094138.png)

# 三元运算符
三元运算符（Ternary Operator）是编程中一种简洁的条件表达式，常用于替代简单的 if-else 逻辑。它的名称来源于其 三个组成部分：条件、条件为真时的结果和条件为假时的结果。

`条件 ? 表达式1 : 表达式2`

- 条件：一个布尔表达式（返回 true 或 false）。

- 表达式1：条件为 true 时执行的值或操作。

- 表达式2：条件为 false 时执行的值或操作。
- 
# for 条件
![微信截图_20250218103028.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250218103028.png)

# continue、break 和 return
## ✅ 1. `continue` —— **跳过本次循环，继续下一轮**

### 📌 作用：
- 当前 `i` 的循环提前结束，**跳过后续代码**
- 自动进入 `i++`，执行下一轮循环
### 🌰 示例：
```java
for (int i = 0; i < 5; i++) {
    if (i == 2) continue;
    System.out.print(i + " ");
}
```

### 💡 输出：
`0 1 3 4`

✅ **i = 2 被跳过，但循环继续**

## ✅ 2. `break` —— **终止整个循环结构**
### 📌 作用：
- 当前 `for` 循环被立刻终止
- 跳出 `for` 循环体，执行后续的代码（不再进入 `i++`）

### 🌰 示例：
```java
for (int i = 0; i < 5; i++) {
    if (i == 2) break;
    System.out.print(i + " ");
}
```
### 💡 输出：
`0 1`
✅ **一旦遇到 `i = 2`，循环彻底结束，不再执行 i=3, i=4**
## ✅ 3. `return` —— **终止整个函数，直接返回调用处**

### 📌 作用：

- 当前函数直接退出，**不会再执行函数内任何后续语句**
- 如果是在 `for` 循环内，整个函数都会退出

### 🌰 示例：

```java
public void testReturn() {
    for (int i = 0; i < 5; i++) {
        if (i == 2) return;
        System.out.print(i + " ");
    }
    System.out.println("循环后打印"); // 永远不会被执行
}
```
### 💡 输出：
`0 1`

✅ **i = 2 时，`return` 整个函数退出，`System.out.println(...)` 永远不会执行**
# 方法格式

![微信截图_20250218111605.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250218111605.png)

# 重构方法
##### idea自带的重构方法 ctrl +alt +m 或者右键如图

见java速成 42-提前方法
![微信截图_20250219205619.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250219205619.png)
# 类和对象&构造方法

## 1. **基本语法结构**：

```java
public class ClassName {
    // 类的字段（成员变量）
    
    // 类的构造方法
    
    // 类的方法（成员方法）

    // 可选的静态块、构造方法、内部类等
}
```

## 2. **示例**：一个简单的类定义

```java
public class Person {
    // 类的字段（成员变量Instance Variables）
    private String name;
    private int age;

    // 构造方法名必须与类名相同
    //构造方法与普通方法的最大不同：构造方法没有返回类型，即使是void也不可以
    public Person(String name, int age) {
        //字段名 = 方法参数名
        //this避免重名，方法参数名比字段名级别更高，如果重名会两边都默认为参数名，出现错误
        this.name = name;
        this.age = age;
    }

    // 成员方法（实例方法）
    // 如果方法声明 void，就不能 return 任何值。
    public void greet() {
        System.out.println("Hello, my name is " + name);
    }

    // 主方法 - 用于执行类中的代码
    public static void main(String[] args) {
        // 创建对象！！！有new说明调用了构造方法
        Person p = new Person("Alice", 30);
        p.greet();  // 调用方法
    }
}
```

## 3. **类中的常见组件**：

- **字段**（成员变量）：用于表示类的属性。

- ### 构造方法：初始化类对象。
    *构造方法的主要目的就是初始化类的对象，而不需要返回任何值。由于这个原因，Java规定构造方法**不能有返回类型**，即使是 `void` 也不能使用。构造方法只是用来创建对象，并将其初始化到一个有效的状态，而不是用来返回任何数据。这是它和普通方法的一个重要区别。*

**IDE 帮你自动生成的构造方法，只是“方便你写字段赋值”的快捷方式而已。完全可以手写、精简、重载（多个构造方法）或加入特定逻辑。
很多时候，构造方法（构造器）会初始化类的字段（成员变量），但它并==不强制==必须有字段。只要符合 ==“**方法名与类名相同且无返回类型**”== 的规则，它就是构造器。

 ✅ 总结构造方法：
    1. **方法名必须和类名一致**
    2. **不能有返回类型**
    3. **在 new 的时候被调用**
- **方法**：定义类的行为，操作类的字段。
- **静态变量/方法**：使用 `static` 关键字来定义类的静态成员，属于类本身，而不是对象。
- **内嵌类/匿名类**：可以定义类内部的类。
## 4. **常见访问修饰符**：
- **public**: 类、方法、字段可以被任何类访问。
- **private**: 类、方法、字段只能在同一个类内部访问。
- **protected**: 允许同一个包内的类和子类访问。
- **default**: 没有指定修饰符时，默认使用包访问权限，只有同一个包内的类可以访问。

## 构造方法
### 自动生成构造方法
![微信截图_20250220152706.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250220152706.png)

![微信截图_20250220152718.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250220152718.png)
![微信截图_20250220152732.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250220152732.png)
### 无参构造
示例：
```java
class Info {
    int rob;
    int notRob;

    public Info() {
        this.rob = 0;
        this.notRob = 0;
    }
}

// 用法：
Info info = new Info();
```

**特点**：
- 没有参数。
- new 对象时不用传参数。
- 通常会把字段初始化成默认值（可以省略不写，因为 Java 会自动给 int=0，boolean=false，引用=null）。

如果你自己不写构造方法，Java 默认会帮你生成一个 **无参构造**：
```java
class Info {
    int rob;
    int notRob;
}

// 默认存在：
public Info() {
    // rob = 0, notRob = 0
}
```

### 有参构造
```java
class Info {
    int rob;
    int notRob;

    public Info(int rob, int notRob) {
        this.rob = rob;
        this.notRob = notRob;
    }
}

// 用法：
Info info = new Info(5, 10);
```

**特点**：
- 有参数。
- new 对象时要传参数。
- 通常用来创建「**带初始值**」的对象。


# 四种方法
## 1. **静态方法（Static Method）**

- **适用场景**：当方法的功能与特定对象的状态无关，或者当方法不需要访问实例变量时，通常选择静态方法。

- 特点

  - 静态方法可以在没有创建对象的情况下直接调用。
  - 静态方法只能访问静态变量和其他静态方法，不能访问实例变量。

- 使用场景

  - 工具类方法（如数学计算、字符串处理等）。
  - 程序启动入口点（`main`方法）。
  - 不需要依赖特定对象的行为。

  ```java
  public class MathUtils {
  
      // 静态方法：add 和 substract不依赖任何类的实例，而是与类本身相关联。在main方法中，可以直接通过类名调用这些方法，而不需要先创建MathUtils类的对象。
      public static int add(int a, int b) {
          return a + b;
      }
      
      public static int subtract(int a, int b) {
          return a - b;
      }
      
      public static void main(String[] args) {
          int sum = MathUtils.add(5, 3);
          int diff = MathUtils.subtract(5, 3);
          System.out.println("Sum: " + sum);
          System.out.println("Difference: " + diff);
      }
  }
  ```

## 2.**实例方法（Instance Method）**

**适用场景**：当方法的功能依赖于对象的状态，或者需要操作对象的实例变量时，应该选择实例方法。

**特点**：

- **可以通过对象调用**，而不是通过类调用。
- 可以访问类的实例变量和实例方法。

**使用场景**：

- 当需要操作对象的状态或者类的实例变量时。
- 当方法的行为依赖于对象的具体状态。

```java
public class Person {
    private String name;
    
    // 构造方法
    public Person(String name) {
        this.name = name;
    }
    
    // 实例方法：打印个人信息,依赖于name实例变量的值
    public void introduce() {
        System.out.println("Hello, my name is " + name);
    }
    
    public static void main(String[] args) {
        Person person = new Person("Alice");
        person.introduce(); // 调用实例方法
    }
}
```

## 3.构造方法（Constructor）

**适用场景**：当你需要在创建对象时进行初始化时，使用构造方法。

**特点**：

- 构造方法与类同名，不带返回类型。
- 作用仅仅是初始化对象的状态（即为实例变量赋初值）。
- **每个类至少有一个构造方法**，如果没有显式定义，编译器会提供一个无参的默认构造方法。
- **自动调用**：构造方法会在对象创建时自动调用。

**使用场景**：

- 在创建新对象时需要对实例变量进行初始化。

```java
public class Car {
    private String model;
    private int year;

    // 构造方法：初始化Car对象
    public Car(String model, int year) {
        this.model = model;
        this.year = year;
    }
    
    public void displayInfo() {
        System.out.println("Model: " + model + ", Year: " + year);
    }
    
    public static void main(String[] args) {
        Car car = new Car("Toyota", 2020); // 创建Car对象时调用构造方法
        car.displayInfo();
    }
}
```

## 4.**辅助方法（Helper Method）**

**适用场景**：当你需要将一个复杂任务拆分成多个小任务时，通常会将这些小任务封装在辅助方法中。这些方法通常是私有的，只在类内部使用。

**特点**：

- 辅助方法通常是私有的，作为内部实现的一部分。
- 目的是简化代码、提高代码可读性和可维护性。

**使用场景**：

- 将一个大的逻辑操作拆分成小的步骤，避免`main`或其他方法过于庞大。

```java
public class DataProcessor {
    

    public void processData() {
        String rawData = "Raw Data";
        String cleanedData = cleanData(rawData);
        String formattedData = formatData(cleanedData);
        System.out.println("Processed Data: " + formattedData);
    }
    
    // 辅助方法：清洗数据
    private String cleanData(String data) {
        // 假设清洗操作
        return data.trim();
    }
    
    // 辅助方法：格式化数据
    private String formatData(String data) {
        // 假设格式化操作
        return data.toUpperCase();
    }
    
    public static void main(String[] args) {
        DataProcessor processor = new DataProcessor();
        processor.processData();  // 调用主方法
    }

}
```

# 方法重载（overload）
## 基本介绍
java中允许同一个类中，多个同名方法的存在，但要求形参列表不一致
重载的好处：减轻了起名和记名的麻烦

方法名：必须相同
形参列表：必须不同（参数类型或个数或顺序，至少有一样不同，参数名无要求）
返回类型：无要求

| 特性    | 方法重载（Overloading） | 方法重写（Overriding） |
| ----- | ----------------- | ---------------- |
| 方法名   | 相同                | 相同               |
| 参数列表  | **必须不同**          | **必须相同**         |
| 返回类型  | 可以相同或不同           | 必须相同（或子类型）       |
| 访问修饰符 | 可以不同              | 不能更严格（但可放宽）      |
| 发生场景  | **同一个类内部**        | **子类重写父类方法**     |
![微信截图_20250309173848.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250309173848.png)

## 一个例子


# 三种变量-局部变量、实例变量、静态变量
## **1️⃣ 局部变量（Local Variable）**

### **📌 定义**

局部变量是在 **方法（Method）、构造器（Constructor）、代码块** 内部声明的变量，**只能在方法或代码块内部使用**，执行完就销毁。

### **🔹 特点**

✔ **作用范围**：仅限于 **当前方法或代码块**。  
✔ **存储位置**：存储在 **栈内存（Stack）**，方法执行完就释放。  
✔ **生命周期**：方法执行时创建，方法结束后销毁。  
✔ **默认值**：⚠ **没有默认值，必须手动初始化！**

### **✅ 示例**

`public class Test {`
    `public void exampleMethod() {`
        `int localVar = 10;  // 局部变量`
        `System.out.println(localVar);`  
    `}`

    `public static void main(String[] args) {`
        `Test t = new Test();`
        `t.exampleMethod();`
    `}`
`}`


**🔹 运行结果**

`10`

**🔹 关键点**

- `localVar` **只在 `exampleMethod` 方法内部可见**。
- `localVar` **存储在栈内存，方法执行完后立即销毁**。

---

## **2️⃣ 实例变量（Instance Variable）**

### **📌 定义**

实例变量是**属于对象（Instance）**的变量，每个对象都有自己的一份实例变量，实例变量的值**不会被多个对象共享**。

### **🔹 特点**

✔ **作用范围**：在**整个对象生命周期内可用**，**可以被同一个对象的多个方法访问**。  
✔ **存储位置**：存储在 **堆内存（Heap）**，对象销毁时变量才会释放。  
✔ **生命周期**：对象创建时初始化，**对象销毁时释放**。  
✔ **默认值**：

- `int` → `0`
- `double` → `0.0`
- `boolean` → `false`
- `引用类型` → `null`

### **✅ 示例**

`public class Person {`
    `String name;  // 实例变量`
    `int age;      // 实例变量`

    `public void display() {`
        `System.out.println("Name: " + name + ", Age: " + age);`
    `}`

    `public static void main(String[] args) {`
        `Person p1 = new Person();`  
        `p1.name = "Alice";`  
        `p1.age = 25;`  
        `p1.display();  // Name: Alice, Age: 25`

        `Person p2 = new Person();`  
        `p2.name = "Bob";`  
        `p2.age = 30;`  
        `p2.display();  // Name: Bob, Age: 30`
    `}`
`}`

**🔹 关键点**

- `name` 和 `age` **属于对象（Person）的实例变量**。
- `p1` 和 `p2` 各自有一份 `name` 和 `age`，**互不影响**。
- **存储在堆内存（Heap）**，只有 `p1` 或 `p2` 被 `null` 或 `垃圾回收（GC）` 时才释放。

---

## **3️⃣ 静态变量（Static Variable）**

### **📌 定义**

静态变量是 **属于类（Class）** 的变量，而不是属于某个对象。  
**所有对象共享同一份静态变量**，修改它的值，会影响所有对象。

### **🔹 特点**

✔ **作用范围**：整个**类范围内可用**，所有对象共享。  
✔ **存储位置**：存储在 **方法区（Method Area）**，而不是堆内存。  
✔ **生命周期**：类加载时初始化，**类卸载时销毁**。  
✔ **默认值**：

- 和实例变量一样，`int` 默认 `0`，`boolean` 默认 `false`，引用类型默认 `null`。

### **✅ 示例**

`public class Student {`
    `static String school = "XYZ School";  // 静态变量，所有对象共享`
    `String name;  // 实例变量，每个对象有自己的 name`

    `public Student(String name) {`
        `this.name = name;`
    `}`

    `public void display() {`
        `System.out.println(name + " - " + school);`
    `}`

    `public static void main(String[] args) {`
        `Student s1 = new Student("Alice");`
        `Student s2 = new Student("Bob");`

        `s1.display();  // Alice - XYZ School`
        `s2.display();  // Bob - XYZ School`

        `// 修改静态变量`
        `Student.school = "ABC University";`

        `s1.display();  // Alice - ABC University`
        `s2.display();  // Bob - ABC University`
    `}`
`}`


**🔹 关键点**

- `school` 是 **静态变量**，所有对象共享一份内存。
- 修改 `school` 影响所有对象的值。

---

## **📌 三者的对比**

|变量类型|作用范围|存储位置|生命周期|是否有默认值|访问方式|
|---|---|---|---|---|---|
|**局部变量**|仅在方法/代码块内|**栈内存（Stack）**|方法执行时创建，方法结束后销毁|❌ 没有默认值，必须手动赋值|直接在方法内使用|
|**实例变量**|属于对象|**堆内存（Heap）**|对象创建时分配，垃圾回收时释放|✅ 有默认值|`对象名.变量名`|
|**静态变量**|属于类，所有对象共享|**方法区（Method Area）**|类加载时创建，类卸载时释放|✅ 有默认值|`类名.变量名`|

---

## **🚀 什么时候用哪种变量？**

1️⃣ **局部变量**

- 适用于 **方法内的临时数据**（比如 `for` 循环变量）。
- 代码执行完 **自动销毁**，占用内存短。

2️⃣ **实例变量**

- 适用于 **每个对象都需要独立数据**，比如 `Person.name`、`Person.age`。
- 只要对象存在，变量就一直存在。

3️⃣ **静态变量**

- 适用于 **所有对象共享的属性**，比如 `学校名称`、`计数器`。
- 可以减少 **内存占用**，因为所有对象共享一份数据。
# 继承

- **子类继承父类**时，使用 `extends` 关键字。
- 只能**继承一个类**（Java 只支持**单继承**）。
- 继承后，子类可以**直接使用父类的非私有（`private` 之外的）属性和方法**。
- 子类可以**重写（override）父类的方法**，也可以添加新的方法。
## 继承父类实例方法时，自动继承

```java
class A {
    String name;

    void test() {
        System.out.println("父类的 test() 方法");
    }
}

class B extends A {
    // 子类 B 没有重写 test() 方法，所以自动继承了父类的实现
}

class C extends A {
    // 子类 C 重写了 test() 方法,@Override检查重写方法是否有问题
    @Override
    void test() {
        System.out.println("子类 C 重写的 test() 方法");
    }
}

public class TestInheritance {
    public static void main(String[] args) {
        B b = new B();
        b.test();  // 调用父类 A 中的 test() 方法

        C c = new C();
        c.test();  // 调用子类 C 中重写的 test() 方法
    }
}

```

输出结果：

```java
父类的 test() 方法
子类 C 重写的 test() 方法
```

## super继承带参构造
父类一旦有带参构造，子类必须有super()来调用父类的带参构造

```java
package com.itheima;

public class TestInheritance {
    public static void main(String[] args) {
        B b = new B("阿狗");
        System.out.println(b.name);
        b.test();
    }
}

class A {

    String name;
    
    public A(String name) {
        this.name = name;
    }

    void test() {
        System.out.println("父类继承方法");
    }
}

class B extends A{
    //父类有构造方法时，子类必须也有自己的构造，去调父类的构造
    B(String name) {
        //super()
        super(name);
    }
}
```

## super普通继承
```java
class Parent {
    void show() {
        System.out.println("Parent show()");
    }
}

class Child extends Parent {
    @Override
    void show() {
        super.show(); // 调用父类的 show 方法，再写child show
        System.out.println("Child show()");
    }
}

public class Main {
    public static void main(String[] args) {
        Child c = new Child();
        c.show();
    }
}
```

# 类型转换

## 基本类型 primitive 
byte short int long float double char boolean
## 包装类型
也属于引用类型，都是Object的子类
Byte Short Integer Long Float Double Character Boolean
## 引用类型 reference type
都是Object的子类
String (byte[] -> byte)
byte[]
int[]
自定义类

## 1. 基本类型之间转换规则
```java
    顺箭头 隐式转换(自动),向上转型
    逆箭头 强制转换, 可能损失精度，向下转型
byte a = 10;
int b = a;

int c = 1000;
byte d = (byte) c; //先把 int 类型的 c 转换为 byte 类型，再将转换后的结果存储到 d 变量中
System.out.println(d);
```

## 2. 包装类型与基本类型之间转换规则
![微信截图_20250221194606.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250221194606.png)

```java
int a = 20;
Integer b = a;//自动装箱Autoboxing

Integer c = new Integer(30);
int d = c;
```

## 3. 引用类型之间转换规则

![微信截图_20250221203522.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250221203522.png)


```java
继承特点
1) 单继承, 子类只能继承一个父类
2) Object 是其它类型直接或间接的父类型, 定义 class 时, 不写 extends 此类也是继承自 Object
3) 子类与父类、祖先类型之间，可以用【是一个 is a】的关系来表达

a. 顺箭头 (待转换的对象和目标类型之间 要符合【是一个】的关系) 隐式转换, 向上转型, 使用父类型统一处理子类型
b. 逆箭头 (待转换的对象和目标类型之间 要符合【是一个】的关系) 强制转换, 向下转型, 将对象还原
                               cat    dog
        
        Animal a = new Cat();  // 对象还是那个对象, 只是用父类型来代表了它
        Object b = new Cat();
//      Appliance c = new Cat(); // 不合法

        Cat c = (Cat) a;
//      Dog d = (Dog) a; // 不行!!!a 是 new Cat()，它的真实类型是 Cat，只是用 Animal 类型的引用来表示它（向上转型，隐式转换）。尝试向下转型成 Dog，但 a 实际上是 Cat，这不符合继承关系。 ClassCastException

        Cat e = (Cat) b; // 可以
//       Dog f = (Dog) b; // 不行 类型转换异常 ClassCastException
        Animal g = (Animal) b; // 可以
```

## 4.获取/判断对象的真正类型

```java
System.out.println(a.getClass()); // 获取它所代表对象的真正类型,getClass其实是Object的方法
System.out.println(b.getClass()); // 获取它所代表对象的真正类型
```

对象 instanceof 类型, 作用：检查对象和类型之间是否符合【是一个】的关系,经常用在向上转型之前做判断
```java
System.out.println(a instanceof Cat);
System.out.println(a instanceof Dog);
System.out.println(b instanceof Animal);
```

## 5.接口和实现类的关系

![](java_15_类型.assets/微信截图_20250225213341.png)

```
//ArrayList可以向上转型为list类型

List<Integer> list1 = new ArrayList<>();
```

```java
package com.itheima.module4;

import java.util.ArrayList;
import java.util.List;

public class TestList {
    public static void main(String[] args) {
        List<Integer> list1 = new ArrayList<>();//Arraylist可以隐式转化为list
        list1.add(1);
        list1.add(2);
        list1.add(3);
        list1.add(4);
        System.out.println(list1.getClass());

        List<Integer> list2 = List.of(1,2,3,4);
        System.out.println(list2.getClass());

        List<Integer> list3 = List.of(1,2);
        System.out.println(list3.getClass());
        /**
         * List.of一旦确定就不能改变集合，比如不能再add元素
         */
        list1.addAll(list2);
        System.out.println(list1);

        List<Integer> list4 = new ArrayList<>();
        list4.addAll(List.of(1,2,3,4,5));
        System.out.println(list4);
    }
}
```

# 多态
## 前提

java 中的类型系统允许用父类型代表子类型对象，这是多态前提之一。

子类和父类之间发生了方法重写，这是多态前提之二

## 效果

调用父类型的方法，可能会有不同的行为，取决于该方法是否发生了重写

## 什么时候使用多态

多态能够用一个父类型，统一操作子类对象
很多if else 的地方，都可以考虑使用多态来消除 if else，提高扩展性

```java
package com.itheima;

public class TestAnimal {
    public static Animal[] getAnimals() {
        return new Animal[] {//组里存放的 实际上是 Cat、Dog、Pig 这些子类对象，只是它们被存储为 Animal 类型。
                new Cat(),//new 是实例化类的唯一方式，通过它可以获得类的具体对象。
                new Dog(),
                new Pig()

        };
    }
    public static void main(String[] args) {
        //类型名 变量名 = 调用方法
        Animal[] animals = getAnimals();
        for (int i = 0; i < animals.length; i++) {
            Animal a = animals[i]; //向上转型，这里的 a 可以是 Cat、Dog、Pig,即用 父类类型的变量 存储 子类的对象
            a.say();
        }
    }
}

class Animal {
    void say() {}
}


class Cat extends Animal{
    void say() {
        System.out.println("mew mew");
    }
}

class Dog extends Animal{
    void say() {
        System.out.println("arf arf");
    }
}

class Pig extends Animal{
    void say() {
        System.out.println("hum hum");
    }
}
```

# Object类
- 明确:java.lang.Object
- 任何一个Java类(除Object类)都直接或间接的继承于Object类
- Object类称为java类的根父类
- 0bject类中声明的结构(属性、方法等)就具有通用性。
     0bject类中没有声明属性
     0bject类提供了一个空参的构造器
     重点关注：Object类中声明的方法
## 常用方法
### clone()
`x.clone() != x` //true 地址不同

### finalize()
jdk9开始过时的办法
![微信截图_20250314095614.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250314095614.png)
### == 
`==`运算符
使用范围: 基本数据类型、引用数据类型
基本数据类型: 判断数据值是否相等
int i1 = 10; 
int i2 = 10; 
sout(i1 == 12); //true
char c1='A';
int i3 = 65;
sout(c1 == i3);//true

引用数据类型：比较两个引用变量的地址值是否相等（或者比较两个对象是否指向同一个实体）

### equals()方法

- 基本数据类型没有equals()方法,直接用`==`就能判断
- 对于像String/File/Date和包装类等，都重写了Object类中的equals()方法，可以直接使用equals比较实体内容是否相等
- 如果是自己定义的类，需要手动重写equals()

| 比较方式       | 适用类型                        | 代码示例          | 结果     |
| ---------- | --------------------------- | ------------- | ------ |
| `==`       | 基本类型 (`int`, `double` 等)    | `i == c`      | ✅ 正确   |
| `equals()` | 基本类型 (`int`, `double` 等)    | `i.equals(c)` | ❌ 编译错误 |
| `equals()` | 包装类 (`Integer`, `Double` 等) | `i.equals(c)` | ✅ 正确   |

- java.lang.Object类中equals()的默认定义，只是比较两个对象的地址是否相同/是否指向了堆空间的同一个对象实体，和`==`的效果是一样的

```java
public boolean equals(Object obj) {  
    return (this == obj);  
}
```

除了generate外，一个手动改写的例子
```java
public boolean equals(Object o) {  
    if (this == o) {  
        return true;  
    } if (o instanceof MyDate) {  
        MyDate myDate = (MyDate) o;  
        return this.year == myDate.year && this.month == myDate.month && this.day == myDate.day;  
    } else {  
        return false;  
    }  
}
```
# interface接口
## 直接实现接口（implements）

 * 类**可以实现多个接口**（支持**多实现**）- 解决单继承问题
 * 放入接口的方法，必须加default关键字（默认方法）
 * default方法只能是public，public可以省略
 * 一个java文件中，可以**同时包含多个类和多个接口**，但 `public` 类或 `public` 接口只能有**一个。
 * 类必须和源文件同名
 
```java
interface A {
    public default void a() {}
}
interface B {
    public default void b() {}
}

//C从A,B两个接口重用方法a()和b()
class C implements A,B{

}
```
### 直接实现接口-案例1
```java
public class TestInterface1 {
    public static void main(String[] args) {
        Duck d = new Duck();
        d.swim();
        d.fly();
    }
}

interface Swimmable {
    default void swim() {
        System.out.println("Swim");
    }
}

interface Flyable {
    default void fly() {
        System.out.println("Fly");
    }
}

class Duck implements Swimmable, Flyable {

}
```
### 直接实现接口-案例2
```java
public class TranditionalExercise01 {  
    public static void main(String[] args) {  
        f1(new Picture());//传统方法  
    }  
  
    public static void f1(IL il) {//静态方法，形参是接口类型  
        il.show();  
    }  
}  
  
interface IL{  
    void show();  
}  
  
//传统方法： 类 ->实现IL=>硬编码  
class Picture implements IL{  
    @Override  
    public void show() {  
        System.out.println("This is a old picture");  
    }  
}

```

## 接口特性 - 多态

### 方法多态的两个条件

 #### 1.用父类型代表子类对象,或者用接口类型代表实现类对象

```java
package com.itheima;

public class TestInterface2 {
    public static void main(String[] args) {
        E[] array = new E[] {
                new F(),
                new G()
        };
    }
}

interface E {

}

class F implements E {

}

class G implements E {

}
```

#### 2.实现类（调用接口的类）重写接口方法

```java
public class TestInterface2 {
    public static void main(String[] args) {
        E[] array = new E[] {
                new F(),
                new G()
        };
        for (int i = 0; i < array.length; i++) {
            E e = array[i];
            e.e();//多态
        }
    }
}

interface E {
//    //默认方法实现，这里其实省略了public
//    default void e() {
//        System.out.println("e");
//    }

    /**抽象方法 只有方法声明，没有方法体
     * 1.只能是public，public abstract都能省略
     * 2.为什么不需要方法体？子类里有方法的话,父类里的方法也用不到
     * 3.实现类，必须重写接口的抽象方法
     */
    public abstract void e();
}

class F implements E {
    /**子类和实现类 方法访问修饰符 >= 父类和接口 方法访问修饰符
     * public > protected > 默认 > private
     */
    @Override
    public void e() {
        System.out.println("f");
    }
}

class G implements E {
    @Override
    public void e() {
        System.out.println("g");
    }
}
```


# 内部类
## 内部类的分类

**定义在外部类局部位置上(比如方法内)**
1)局部内部类(有类名)
2)匿名内部类(没有类名，重点!!!!!!!!)

**定义在外部类的成员位置上:**
1)成员内部类(没用static修饰)
2)静态内部类(使用static修饰)


## 局部内部类

记住
1）局部内部类定义在方法中/代码块
2）作用域在方法体或代码块中
3）本质依然是一个类
4）外部其他类不能访问局部内部类(因为 局部内部类地位是一个局部变量)
5）如果外部类和局部内部类的成员重名时，默认遵循就近原则，如果想访问外部类的成员，则可以使用(外部类名.this.成员)去访问
`System.out.println("外部类的n2="+ 外部类名,this.n2);`

```java
public class LocalInnerClass {  
    public static void main(String[] args) {  
        Outer02 outer02 = new Outer02();  
        outer02.m1();  
    }  
}  
  
class Outer02 { //外部类  
    private int n1 = 100;  
    private void m2() {  
        System.out.println("Outer02 m2()");} //私有方法  
    public void m1() {//方法  
        /**  
         1.局部内部类是定义在外部类的局部位置  
         2.作用域只在该方法内  
         3.不能添加访问修饰符（局部内部类的地位相当于局部变量），  
         但可以使用final修饰（但是加了final就不能被其他类继承）  
         */  
        class Inner02{ //局部内部类  
            private int n1 = 800;  
            /**  
             * 和外部类成员重名，默认遵守就近原则  
             * 如果想访问外部类的成员，使用外部类名.this.成员（）  
             */  
            public void f1() {  
                System.out.println("n1 = " + n1);//可以直接访问外部类的所有成员，包含私有的  
                m2();  
                System.out.println(Outer02.this.n1);//访问外部的n1  
            }  
        }  
        //外部类在方法中，可以创建Inner02对象，然后调用方法即可  
        Inner02 inner02 = new Inner02();  
        inner02.f1();  
    }  
}
```

## 匿名内部类

（1）本质是类
（2）是内部类
（3）该类没有名字
（4）同时还是一个对象
形式：
```java
new class/interface(param) {
}
```
### 基于接口的匿名内部类
//1.需求：使用接口A，并创建对象  
2.传统方法，写一个类实现该接口并创建对象  
class Tiger implements IA { //传统实现  
//    @Override  
//    public void cry() {  
//        System.out.println("Tiger aoao");  
//    }  
3.tiger类只使用一次,可以使用匿名内部类简化开发  
4.tiger的编译类型 IA        
5.tiger的运行类型就是匿名内部类 Outer04$1 系统分配的  

```java
class Outer04$1 implements IA {  
    @Override  
     public void cry() {  
     System.out.println("Tiger aoao")  
     }         
}      
```
  
6.jdk底层在创建匿名内部类 Outer04$1，立即创建了Outer04$1实例，并且把地址返回给tiger  
7.匿名内部类使用一次就不能再使用，但是tiger对象可以反复调用  

```java
public class AnonymousInnerClass {  
    public static void main(String[] args) {  
        Outer04 outer = new Outer04();  
        outer.method();  
    }  
}  
  
class Outer04 {//外部类  
    public void method() { //方法  
        //基于接口的匿名内部类  
        IA tiger = new IA() {  
            @Override  
            public void cry() {  
                System.out.println("Tiger aoao");  
            }  
        };  
        System.out.println("tiger的运行类型=" + tiger.getClass());//匿名内部类  
        tiger.cry();  
        tiger.cry();  
        tiger.cry();  
    }  
}  
  
interface IA {//接口  
    public void cry();  
}
```


### 基于类的匿名内部类

```java
public class AnonymousInnerClass {  
    public static void main(String[] args) {  
        Outer04 outer = new Outer04();  
        outer.method();  
    }  
}  
  
class Outer04 { //外部类  
    /**  
     * !注意 匿名内部类 比普通的创建对象多了一个{};  
     * father编译类型father  
     * father运行类型Father Outer$1  
     */    
     /**     
     * class Outer04$2 extends Father {     
     *    @Override  
     *    public void test() {  
     *          System.out.println("匿名内部类重写了test方法");  
     *    }     
     * }     
     * 同时也直接返回了匿名内部类Outer04$1的对象  
     */  
    public void method() {  
        Father father = new Father("jack") {  
            @Override  
            public void test() {  
                System.out.println("匿名内部类重写了test方法");  
            }  
        };  
        System.out.println("father对象的运行类型" + father.getClass());  
        father.test();  
        //基于抽象类的匿名内部类,就必须实现  
        Animal dog = new Animal() {  
            @Override  
            void eat() {  
                System.out.println("dog eats meat");  
  
            }  
        };  
        dog.eat();  
    }  
}  
  
class Father { //这是一个构造器  
    public Father(String name) { 
        System.out.println("接受到了name:" + name);  
    }  
    public void test() {//方法  
    }  
}  
  
abstract class Animal {  
    abstract void eat();  
}
```


### 匿名内部类的调用方法

```java
public class AnonymousInnerClassDetail {  
    public static void main(String[] args) {  
  
        Outer05 outer05 = new Outer05();  
        outer05.f1();  
  
    }  
}  
class Outer05 {  
    public void f1() {  
        Person p1 = new Person(){  
            @Override  
            public void hi() {  
                System.out.println("匿名变量内部重写了 hi");  
            }  
        };  
        p1.hi();//动态绑定  
  
        new Person(){  
            @Override  
            public void hi() {  
                System.out.println("匿名变量内部又叒重写了 hi");  
            }  
        }.hi();//也可以直接调用  
  
    }  
}  
class Person { //类  
    public void hi() {  
        System.out.println("Person hi()");  
    }  
    public void ok(String str) {  
        System.out.println("Person ok" + str);  
    }  
}
```

再来一个直接调用的例子

```java
public class AnonymousInnerClassDetail {  
    public static void main(String[] args) {  
        Outer05 outer05 = new Outer05();  
        outer05.f1();  
    }  
}  
class Outer05 {  
    public void f1() {  
        new Person(){  
            @Override  
            public void ok(String str) {  
                super.ok(str);//继承  
            }  
        }.ok("jack");//也可以直接调用  
    }  
}  
class Person { //类  
    public void ok(String str) {  
        System.out.println("Person ok" + str);  
    }  
}
```

- 内部类可以访问外部类的所有成员，包含私有的
- 不能添加访问修饰符，因为他的定位就是一个局部变量
- 作用域仅仅在定义它的方法内部
- 外部其他类不能访问匿名内部类（其地位是一个局部变量）
- 如果外部类和匿名内部类的成员方法重名，匿名内部类访问，遵循就近原则，如果想访问外部类的成员，可以使用（外部类名.this.成员）访问

```java
public class AnonymousInnerClassDetail {  
    public static void main(String[] args) {  
        Outer05 outer05 = new Outer05();  
        outer05.f1();  
    }  
}  
class Outer05 {  
    private int n1 = 99;  
    public void f1() {  
        Person p1 = new Person(){ //不能添加访问修饰符，因为定位就是一个局部变量  
            @Override  
            public void hi() {  
                System.out.println(n1);//内部类可以访问外部类的所有成员，包含私有的  
            }  
        };  
        p1.hi();//动态绑定  
  
    }  
}  
class Person { //类  
    public void hi() {  
        System.out.println("Person hi()");  
    }  
}
```


### 匿名内部类的最佳实践
#### 通过接口 ->匿名类调用接口

匿名内部类是一种 **快速实现接口** 的方式，**不需要创建单独的实现类**。

```java
public class InnerClassExercise01 {  
    public static void main(String[] args) {  
        f1(new IL() {//匿名内部类，当做实参传递，简洁高效  
            @Override  
            public void show() {  
                System.out.println("This is a new picture");  
            }  
        });  
    }  
  
    public static void f1(IL il) {//静态方法，形参是接口类型  
        il.show();  
    }  
}  
  
interface IL{  
    void show();  
}  
```

```java
public class InnerClassExercise03 {  
    public static void main(String[] args) {  
        BellRing(new Bell2() {  
            @Override  
            public void ring() {  
                System.out.println("get up lazy pig!");  
            }  
        });  
    }  
    public static void BellRing(Bell2 bell) {  
        bell.ring();  
    }  
}  
  
interface Bell2 {//接口不能被实例化  
    void ring();  
}
```

#### 接口 → 类调用 → 匿名类调用类方法
```java
public class InnerClassExercise02 {  
    public static void main(String[] args) {  
        CellPhone cellPhone = new CellPhone();  
        cellPhone.alarmClock(new Bell() {//匿名内部类，相当于一个局部变量  
            @Override  
            public void ring() {  
                System.out.println("lazy pig get up!");  
            }  
        });  
        cellPhone.alarmClock(new Bell() {//匿名内部类，相当于一个局部变量  
            @Override  
            public void ring() {  
                System.out.println("go to school right now!");  
            }  
        });  
    }  
}  
  
interface Bell {//接口不能被实例化  
    void ring();  
}  
  
class CellPhone{  
    public void alarmClock(Bell bell){//形参是接口Bell类型  
        bell.ring();  
    }  
}
```

## 成员内部类

成员内部类是定义在外部类的成员位置，没有static修饰
1.可以访问外部类的所有成员，包含私有的
2.可以添加任意访问修饰符，因为它的地位就是一个成员
3.作用域：和外部类的其他成员一样，在整个外部类中都可以使用，可以创建成员内部类对象， 再调用方法
4.成员内部类 -- 访问 --> 外部类成员 直接访问
5.外部其他类 -- 访问 --> 成员内部类 的三种方式
6.外部类和内部类成员重名，内部类访问，遵循就近原则，如果想访问外部类的成员，则可以使用（外部类名.this.成员）去访问

```java
public class MemberInnerClass01 {  
    public static void main(String[] args) {  
        Outer08 outer08 = new Outer08();  
        outer08.t1();  
  
        //外部其他类，使用成员内部类的三种方式  
        // 第一种 语法：外部类.内部成员  
        Outer08.Inner08 inner08 = outer08.new Inner08();  
        inner08.say();  
        //第二种 在外部类编写一个方法，返回 Inner08对象  
        Outer08.Inner08 inner08Instance = outer08.getInner08Instance();  
        inner08Instance.say();  
        //第三种 类第一种  
        new Outer08().new Inner08();  
    }  
}  
  
class Outer08{ //外部类  
    private int n1 = 10;  
    public String name = "zhangsan";  
    private void hi() {  
        System.out.println("this is a hi");  
    }  
  
    class Inner08 { //成员内部类,定义在外部类的成员位置上  
        private int n1 = 666;  
        public void say() {  
            //可以直接使用外部类的所有成员，包括私有  
            System.out.println(Outer08.this.n1 + name);  
            hi();  
        }  
    }  
  
    public Inner08 getInner08Instance() { //第二种方法  
        return new Inner08();  
    }  
  
    //写方法  
    public void t1() {  
        new Inner08().say();//成员内部类可以在方法内直接使用  
    }  
}
```

## 静态内部类

静态内部类是定义在外部类的成员位置，并且有static修饰
1.可以直接访问外部类的所有静态成员，包含私有的，但不能直接访问非静态成员
2.可以添加任意访问修饰符，因为它的地位就是一个成员
3.作用域，同其他成员，为整个类体
4.外部类 -- 访问 -- 静态内部类 访问方式：创建对象，再访问
5.外部其他类 -- 访问 -- 静态内部类
6.重名，就近原则，访问外部成员使用（外部类名.成员）

```java
public class StaticInnerClass01 {  
    public static void main(String[] args) {  
        Outer10 outer10 = new Outer10();  
        outer10.m1();  
  
        //外部其他类访问静态内部类  
        //方式1：通过类名直接访问（修饰符允许）  
        Outer10.Inner10 inner10 = new Outer10.Inner10();  
        inner10.say();  
        //方式2：编写一个方法，返回静态内部类的对象实例  
        Outer10.Inner10 inner101 = new Outer10.Inner10();  
        inner101.say();  
    }  
}  
  
class Outer10{  
    private int n1 = 10;  
    private static String name ="zhangsan";  
  
    static class Inner10{ //静态内部类  
        private static String name = "yang";//就近原则,否则外部类名.成员  
        public void say(){  
            System.out.println(name);  
            // System.out.println(n1); 不能访问非静态成员  
        }  
    }  
  
    public void m1() {  
        Inner10 inner10 = new Inner10(); //可以在整个类体内使用  
        inner10.say();  
    }  
  
    public Inner10 getInner10() {  
        return new Inner10(); //编写一个方法，返回静态内部类的对象实例  
    }  
  
}
```

# this
| 场景                         | this指的是谁？        |
| -------------------------- | ---------------- |
| 普通类里                       | 当前这个类的对象         |
| 内部类（nested class）          | 最近一层内部类的对象       |
| 匿名内部类（比如Runnable/Listener） | 当前匿名内部类的对象       |
| 如果要引用外部类的this              | 可以写成：`外部类名.this` |
## 普通类
```java
class A {
    int value;

    void setValue(int value) {
        this.value = value; // 这里的this指的是A对象
    }
}
```

## 内部类
```java
class Outer {
    int outerValue;

    class Inner {
        int innerValue;
        
        void print() {
            System.out.println(this.innerValue);  // this是Inner对象
            System.out.println(Outer.this.outerValue); // Outer.this是Outer对象
        }
    }
}
```

- `this`在`Inner`里默认指`Inner`
- 如果你想访问外面的`Outer`对象，就要写`Outer.this`

## 匿名内部类
```java
Button button = new Button();
button.setOnClickListener(new OnClickListener() {
    @Override
    public void onClick(View v) {
        this.doSomething();  // 这里的this是匿名OnClickListener对象
    }
});
```

# String、StringBuffer和StringBuilder

## String
### String的特性
- `java.lang.String` 类代表字符串。Java程序中所有的字符串文字（例如`"hello"` ）都可以看作是实现此类的实例。

- 字符串是常量，用双引号引起来表示。它们的值在创建之后不能更改。

- 字符串String类型本身是final声明的，意味着我们不能继承String。

- String对象的字符内容是存储在一个字符数组value[]中的。`"abc"` 等效于 `char[] data={'h','e','l','l','o'}`。
```java
  //jdk8中的String源码：
  public final class String
      implements java.io.Serializable, Comparable<String>, CharSequence {
      /** The value is used for character storage. */
      private final char value[]; //String对象的字符内容是存储在此数组中
   
      /** Cache the hash code for the string */
      private int hash; // Default to 0
```
  - private意味着外面无法直接获取字符数组，而且String没有提供value的get和set方法。

  - final意味着字符数组的引用不可改变，而且String也没有提供方法来修改value数组某个元素值

  - 因此字符串的字符数组内容也不可变的，即String代表着不可变的字符序列。即，一旦对字符串进行修改，就会产生新对象。

  - JDK9只有，底层使用byte[]数组。
    ```java
    public final class String implements java.io.Serializable, Comparable<String>, CharSequence { 
        @Stable
        private final byte[] value;
    }
    
    //官方说明：... that most String objects contain only Latin-1 characters. Such characters require only one byte of storage, hence half of the space in the internal char arrays of such String objects is going unused.
    
    //细节：... The new String class will store characters encoded either as ISO-8859-1/Latin-1 (one byte per character), or as UTF-16 (two bytes per character), based upon the contents of the string. The encoding flag will indicate which encoding is used.
    ```
- Java 语言提供对字符串串联符号（"+"）以及将其他对象转换为字符串的特殊支持（toString()方法）。
### 构造器
* `public String() ` ：初始化新创建的 String对象，以使其表示空字符序列。
* `String(String original)`： 初始化一个新创建的 `String` 对象，使其表示一个与参数相同的字符序列；换句话说，新创建的字符串是该参数字符串的副本。
* `public String(char[] value) ` ：通过当前参数中的字符数组来构造新的String。
* `public String(char[] value, int offset, int count) ` ：通过字符数组的一部分来构造新的String。
* `public String(byte[] bytes) ` ：通过使用平台的**默认字符集**解码当前参数中的字节数组来构造新的String。
* `public String(byte[] bytes,String charsetName) ` ：通过使用指定的字符集解码当前参数中的字节数组来构造新的String。

举例：
```java
//字面量定义方式：字符串常量对象
String str = "hello";

//构造器定义方式：无参构造
String str1 = new String();

//构造器定义方式：创建"hello"字符串常量的副本
String str2 = new String("hello");

//构造器定义方式：通过字符数组构造
char chars[] = {'a', 'b', 'c','d','e'};     
String str3 = new String(chars);
String str4 = new String(chars,0,3);

//构造器定义方式：通过字节数组构造
byte bytes[] = {97, 98, 99};     
String str5 = new String(bytes);
String str6 = new String(bytes,"GBK");
```

```java
public static void main(String[] args) {
	char[] data = {'h','e','l','l','o','j','a','v','a'};
	String s1 = String.copyValueOf(data);
	String s2 = String.copyValueOf(data,0,5);
	int num = 123456;
	String s3 = String.valueOf(num);
	
    System.out.println(s1);
	System.out.println(s2);
	System.out.println(s3);
}
```

### 常用方法
```java
1.字符串是否为空
boolean isEmpty()

2.返回字符串的长度
int length()

3.拼接
String concat(xx)

4.比较字符串是否相等，区分大小写
boolean equals(Object obj)
```

| 比较方式                      | 含义                             |
| ------------------------- | ------------------------------ |
| `string1.equals(string2)` | 比较内容，是否相等（通常用于业务逻辑里）           |
| `string1 == string2`      | 比较 **引用是否相同**，是否是同一个对象（慎用！容易错） |
```java
5.比较字符串是否相等，不区分大小写
boolean equalsIgnoreCase(Object obj)

6.比较字符串大小，区分大小写，按照Unicode编码值比较大小 < 0：说明str1小于str2，== 0说明等于，> 0说明str1大于str2
int compareTo(String other)

7.比较字符串大小，不区分大小写
int compareToIgnoreCase(String other)

8.将字符串中大写字母转为小写/大写
String toLowerCase()
String toUpperCase()

9.去掉字符串前后空白符
String trim()

10.结果在常量池中共享
public String intern()
```

### 查找：
```java
1.是否包含xx
boolean contains(xx)

2.从前往后找当前字符串中xx，即如果有返回第一次出现的下标，要是没有返回-1
int indexOf(xx)

3.返回指定子字符串在此字符串中第一次出现处的索引，从指定的索引开始
int indexOf(String str, int fromIndex)

4.从后往前找当前字符串中xx，即如果有返回最后一次出现的下标，要是没有返回-1
int lastIndexOf(xx)

5.返回指定子字符串在此字符串中最后一次出现处的索引，从指定的索引开始反向搜索。
int lastIndexOf(String str, int fromIndex)
```

### 字符串截取：
1. 返回一个新的字符串，它是此字符串的从beginIndex开始截取到最后的一个子字符串。 
```java
String substring(int beginIndex)
```

2. 返回一个新字符串，它是此字符串从beginIndex开始截取到endIndex(不包含)的一个子字符串。
```java
String substring(int beginIndex, int endIndex)
```
 
3. 用来**按照指定的分隔符**把字符串拆分成若干部分，返回一个 **字符串数组 `String[]`**。
```java
String version = "1.0.5";
String[] arr = version.split("\\.");
得到arr = ["1", "0", "5"]
```
### 开头与结尾：
```java
1.测试此字符串是否以指定的前缀开始 
boolean startsWith(xx)

2.测试此字符串从指定索引开始的子字符串是否以指定前缀开始
boolean startsWith(String prefix, int toffset)

3.测试此字符串是否以指定的后缀结束 
boolean endsWith(xx)
```
### 替换：
（27）String replace(char oldChar, char newChar)：返回一个新的字符串，它是通过用 newChar 替换此字符串中出现的所有 oldChar 得到的。 不支持正则。
（28）String replace(CharSequence target, CharSequence replacement)：使用指定的字面值替换序列替换此字符串所有匹配字面值目标序列的子字符串。 
（29）String replaceAll(String regex, String replacement)：使用给定的 replacement 替换此字符串所有匹配给定的正则表达式的子字符串。 
（30）String replaceFirst(String regex, String replacement)：使用给定的 replacement 替换此字符串匹配给定的正则表达式的第一个子字符串。 
### char
在 Java 中，`char` 是一种整数类型，本质是一个 16 位的无符号整数，对应 **Unicode 编码（即 ASCII 扩展）**。
所以你可以像对整数那样对字符进行：
- 比较大小（`<`, `>`, `==`）
- 加减运算（`'4' - 1 = '3'`）
```java
System.out.println('2' < '4'); // true，因为 50 < 52
System.out.println((char)('4' - 1)); // 输出 '3'
```
#### 为什么题解都用char[]而不是int[]
如果你不想处理 ASCII，而是想直接操作数字（int），你得自己转换，比如这样：
`int digit = digits[i] - '0'; // 把字符变成数字`
这就是为什么很多时候，**用字符数组处理数字很方便**，因为字符天然能表示数字的每一位，又能用 ASCII 简单比较和修改。

- 原始输入是 `int` 类型的一个完整数字。
- 拆成字符串之后，用 `char[]` 是最方便操作每一位的形式。
- 而且还可以随时修改某位：`digits[i]--`，然后再拼接回去。

如果用 `int[]` 表示每位数字就要自己写拆分逻辑，比如：
```java
while (n > 0) {
    digits[index--] = n % 10;
    n /= 10;
}
```
操作起来远不如字符数组方便。
### String和其他类型的转换
```java
1.将字符串转换为字符数组
char[] toCharArray()
// 说明：将字符串拆分成字符数组，便于逐字符处理。

2.获取字符串中指定位置的字符
char charAt(int index)
// 说明：返回字符串中指定下标位置的字符。

3.将整个字符数组转换为字符串（推荐用法
String.valueOf(char[] data)
举例：
char[] data = {'H', 'e', 'l', 'l', 'o'};
String str = String.valueOf(data);

4.将字符数组的一部分转换为字符串
String.valueOf(char[] data, int offset, int count)
// 说明：将字符数组中从 offset 开始的 count 个字符转换为字符串。

5.将字符数组转换为字符串（功能同 valueOf）
String copyValueOf(char[] data)
// 说明：和 valueOf 相同，语义更强调“拷贝”。

6.将字符数组的一部分转换为字符串（功能同 valueOf）
String copyValueOf(char[] data, int offset, int count)
// 说明：和 valueOf(char[], offset, count) 等价，语义更强调“拷贝”。

7.整数转字符串
String Integer.toString(int n)
// 说明：将 int 类型转换为字符串，常用于输出或拼接。

8.字符串转整数
int Integer.parseInt(String s)
// 说明：将String转换为 int 类型。

9.字符转数字（限于 0-9）
int digit = '3' - '0'
// 说明：利用 ASCII 编码特性将字符型数字转为对应整数值。

10.数字转字符（限于 0-9）
char ch = (char)(3 + '0')
// 说明：利用 ASCII 编码将数字转为字符型数字。

11.字符数组转字符串（另一种方式）
new String(char[] data)
// 说明：构造函数方式将字符数组转换为字符串。
```

## StringBuffer、StringBuilder
因为String对象是不可变对象，虽然可以共享常量对象，但是对于频繁字符串的修改和拼接操作，效率极低，空间消耗也比较高。因此，JDK又在java.lang包提供了可变字符序列StringBuffer和StringBuilder类型。
`StringBuilder` 不能使用 `==` 或 `.equals()` 来比较内容是否相等，因为：

- `==` 比较的是 **对象引用地址**，不是内容；
    
- `.equals()` 方法没有被重写，依然是 `Object.equals()`，也比较的是引用地址。
要比较两个 `StringBuilder` 内容是否相等，应该先转换成 `String`，然后equals比较内容：
### 区分String、StringBuffer、StringBuilder
- String: 不可变的字符序列； 底层使用char[]数组存储(JDK8.0中)
- StringBuffer: 可变的字符序列；线程安全（方法有synchronized修饰），效率低；底层使用char[]数组存储 (JDK8.0中)
- StringBuilder: 可变的字符序列； jdk1.5引入，线程不安全的，效率高；底层使用char[]数组存储(JDK8.0中)

### 常用方法
```java
1. 创建 StringBuilder 对象
StringBuilder sb = new StringBuilder("Hello");

2. append(xx)：追加内容
sb.append(" World"); // Hello World

3. delete(start, end)：删除[start, end)之间字符
sb.delete(5, 6); // HelloWorld

4. deleteCharAt(index)：删除指定位置字符
sb.deleteCharAt(5); // Helloorld

5. replace(start, end, str)：替换[start, end)为 str
sb.replace(5, 8, "XYZ"); // HelloXYZld

6. setCharAt(index, c)：替换指定位置字符
sb.setCharAt(0, 'h'); // helloXYZld

7. charAt(index)：获取指定位置字符
char ch = sb.charAt(1); // 'e'
System.out.println("charAt(1): " + ch);

8. insert(index, xx)：在 index 插入内容
sb.insert(5, "---"); // hello---XYZld

9. length()：获取字符长度
int len = sb.length();
System.out.println("Length: " + len);

10. reverse()：反转字符序列
sb.reverse(); // dlZYX---olleh

 11. indexOf(str)：查找首次出现位置
int idx1 = sb.indexOf("YX"); // 返回第一次出现"YX"的下标
System.out.println("indexOf(\"YX\"): " + idx1);

12. indexOf(str, fromIndex)：从指定位置查找
int idx2 = sb.indexOf("l", 5);
System.out.println("indexOf(\"l\", 5): " + idx2);

13. lastIndexOf(str)：查找最后一次出现位置
int idx3 = sb.lastIndexOf("l");
System.out.println("lastIndexOf(\"l\"): " + idx3);

14. lastIndexOf(str, fromIndex)：从指定位置向前查找
int idx4 = sb.lastIndexOf("l", 6);
System.out.println("lastIndexOf(\"l\", 6): " + idx4);

15. substring(start)：截取[start, end)
String sub1 = sb.substring(0); // 整个字符串
System.out.println("substring(0): " + sub1);

16. substring(start, end)：截取[start, end)
String sub2 = sb.substring(0, 5);
System.out.println("substring(0, 5): " + sub2);

17. toString()：转为字符串
String result = sb.toString();
System.out.println("toString(): " + result);

18. setLength(newLength)：修改长度
sb.setLength(5); // 截断为前5个字符
System.out.println("After setLength(5): " + sb.toString());
```
# 排序
## 冒泡排序
![微信截图_20250306085755.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250306085755.png)
![微信截图_20250306090314.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250306090314.png)
### 冒泡排序特点
1.一共5个元素
2.一共进行了4轮排序，可以看成外层循环
3.每一轮排序可以确定一个数的位置，比如第一轮排序确定最大数，第二轮排序确定第二大位置
4.比较时，如果前面的数大于后面的数就交换
5.每一轮比较在减少 4->3->2->1

```java
public class BubbleSortV2 {  
    public static void main(String[] args) {  
        int[] arr = {13, 24, 57, 69, 80};  
        int temp = 0;  
        boolean swapped = false; //引入变量，初始值设为False,假设本轮没有交换  
        for (int i = 0; i < arr.length - 1; i++) {  
            for (int j = 0; j < arr.length - 1 - i; j++) {  
                if (arr[j] > arr[j + 1]) {  
                    temp = arr[j];  
                    arr[j] = arr[j + 1];  
                    arr[j + 1] = temp;  
                    swapped = true; //无序，需要排序  
                }  
            }  
            if (!swapped) { //在一整轮for j结束后再判断  
                System.out.println("\nArray is already sorted. Exiting...");  
                break; //直接影响for i  
            }  
  
            System.out.println("\nNo." + (i + 1) + " round");  
            for (int j = 0; j < arr.length; j++) {  
                System.out.print(arr[j] + "\t");  
            }  
        }  
    }  
}
```

# Compare比较器
## 自然排序：java.lang.Comparabale
基本数据类型的数据(除boolean类型外)需要比较大小的话，之使用比较运算符即可，但是引用数据类型是不能使用比较运算符来比较大小的。那么，如何解决这个问题呢?

在具体的类中要实现Comparable接口
    重写抽象方法compareTo(Object o)  在此方法中，指明如何判断当前类的对象的大小。比如：按照价格的高低进行大小的比较。（或从低到高排序）  
    **返回值是正数：当前对象大。   返回值是负数：当前对象小。    如果0一样大。**  

### 接口定义

```java
public interface Comparable<T> {
    int compareTo(T o);
}
```

```java Product
public class Product implements Comparable{  
    private String name;  
    private Double price;  
  
    public Product(String name, Double price) {  
        this.name = name;  
        this.price = price;  
    }  
  
    public Product() {  
    }  
  
    public String getName() {  
        return name;  
    }  
  
    public void setName(String name) {  
        this.name = name;  
    }  
  
    public Double getPrice() {  
        return price;  
    }  
  
    public void setPrice(Double price) {  
        this.price = price;  
    }  
  
    @Override  
    public String toString() {  
        return "Product{" +  
                "name='" + name + '\'' +  
                ", price=" + price +  
                '}';  
    }   
    @Override  
    public int compareTo(Object o) {//如果你想自定义Comparable的行为，必须在类里实现它
        if(o == this){  
            return 0;  
        }  
        if(o instanceof Product){  
            Product p = (Product)o;  
            return Double.compare(this.price, p.price);  
        }  
        throw new RuntimeException("类型不匹配");  
    }  
}
```

如果先比较价格，价格相同，再进行名字的比较（从小到大）
```java
@Override  
public int compareTo(Object o) {  
    if(o == this){  
        return 0;  
    }  
    if(o instanceof Product){  
        Product p = (Product)o;  
        int value = Double.compare(this.price, p.price);  
        if(value != 0){  
            return value;//如果希望价格从大到小，那么是-value  
        }  
        return this.name.compareTo(p.name);  
    }  
    throw new RuntimeException("类型不匹配");  
}
```

再使用Arrays.sort()
```java
public class ComparableTest {  
    @Test  
    public void test2() {  
        Product[] arr = new Product[5];  
        arr[0] = new Product("xiaomi",1.0);  
        arr[1] = new Product("aa",100.0);  
        arr[2] = new Product("bb",300.0);  
        arr[3] = new Product("dd",4.0);  
        arr[4] = new Product("lulu",5.0);  
        //必须实现Comparable,才能用Array.sort，  
        Arrays.sort(arr);  
        for(int i = 0 ; i < arr.length ; i++){  
            System.out.println(arr[i]);  
        }  
    }  
}
```

## 定制排序：java.util.Comparator

思考：
- 当元素的类型没有实现`java.lang.Comparable`接口而又不方便修改代码（例如:一些第三方的类，只有.class文件，没有源文件
- 如果一个类，实现了Comparable接口，也指定了两个对象的比较大小的规则，但是此时此刻我不想按照它预定义的方法比较大小，但是我又不能随意修改，因为会影响其他地方的使用，怎么办?

解决方法：
- JDK在设计类库之初，也考虑到这种情况，所以又增加了一个java.util.Comparator接口。强行对多个对象进行整体排序的比较。
- 重写compare(object o1,object o2)方法，比较o1和o2的大小 : 如果方法返回正整数，则表示o1大于o2 ; 如果返回0，表示相等 ; 返回负整数，表示o1小于o2。
- 可以将 Comparator 传递给 sort 方法(如 Collections.sort 或 Arrays.sort)，从而允许在排序顺序上实现精确控制。

实现步骤：
- 创建一个实现了Comparator接口的实现类A
- 实现类A要求重写Comparator接口中的抽象方法compare(0bjecto1,0bject o2)，在此方法中指明要比较大小的对象的大小关系。(比如，String类、Product类)
- 创建此实现类A的对象，并将此对象传入到相关方法的参数位置即可。(比如:Arrays.sort(..,类A的实例))
### 基本语法
```java
Collections.sort(集合, new Comparator<T>() {
    @Override
    public int compare(T o1, T o2) {
        // 比较逻辑
    }
});
```

```java
public class ComparatorTest {  
    @Test  
    public void test1() {  
        Product[] arr = new Product[5];  
        arr[0] = new Product("xiaomi",1.0);  
        arr[1] = new Product("aa",100.0);  
        arr[2] = new Product("bb",300.0);  
        arr[3] = new Product("dd",300.0);  
        arr[4] = new Product("lulu",5.0);  
  
//创建了一个基于Comparator接口的匿名内部类,重写Comparator接口中的抽象方法compare(Object o1, Object o2)  
        Comparator comparator = new Comparator() {  
            @Override  
            public int compare(Object o1, Object o2) {  
                if (o1 instanceof Product && o2 instanceof Product) {  
                    Product p1 = (Product) o1;  
                    Product p2 = (Product) o2;  
                    return Double.compare(p1.getPrice(), p2.getPrice());
                    //如果是比较名称 return p1.getName().compareTo(p2.getName())  
                }  
                throw new RuntimeException("类型不匹配");  
            }  
        };  
  
        Arrays.sort(arr,comparator);  
        for(int i = 0 ; i < arr.length ; i++){  
            System.out.println(arr[i]);  
        }  
    }  
}
```
### 重要：lambda表达式

**Lambda 表达式只能用于实现函数式接口**，而 `Comparable<T>` 并不是函数式接口，所以 **Lambda 表达式不能用于实现 `Comparable`**。  
  
👉 **Lambda 只能写 `Comparator`，不能写 `Comparable`。**


```java
Collections.sort(集合, (a, b) -> 比较规则);
```

示例1：字符串按长度排序
```java
List<String> list = Arrays.asList("apple", "banana", "kiwi");

Collections.sort(list, (a, b) -> a.length() - b.length());

// 输出：["kiwi", "apple", "banana"]
```

示例 2：`List<List<String>>` 按第二个元素排序
```java
Collections.sort(tickets, (a, b) -> a.get(1).compareTo(b.get(1)));
```

示例3：二维数组按第一个元素的值排序
```java
Arrays.sort(points, (a, b) -> Integer.compare(a[0], b[0]));
```
相等的话，再按第二个元素排序
```java
Arrays.sort(points, (a, b) -> {
    if (a[0] != b[0]) return Integer.compare(a[0], b[0]);
    return Integer.compare(a[1], b[1]);
});
```

以下做法可能会整形溢出
```java
Arrays.sort(people, (a, b) -> {
            if (a[0] == b[0]) return a[1] - b[1];
            // a - b 是升序排列，故在a[0] == b[0]的狀況下，會根據k值升序排列
            return b[0] - a[0];
			//b - a 是降序排列，在a[0] != b[0]，的狀況會根據h值降序排列
});
```

## 对比两种排序方式

角度一：
     自然排序:单一的，唯一的
     定制排序:灵活的，多样的

角度二:
    自然排序:一劳永逸的
    定制排序:临时的

角度三:细节
    自然排序:对应的接口是Comparable，对应的抽象方法compareTo(Object obj)
    定制排序:对应的接口是Comparator，对应的抽象方法compare(Object obj1,Object obj2)
# Array数组

## **定义数组**

    Array 数组可以存多个值
    1.类型[] 变量名 = new 类型[长度]
    2.类型[] 变量名 = new 类型[]{元素1，元素2，...}
    3.类型[] 变量名 = {元素1，元素2，...}
 
```java
public class TestArrat {
    public static void main(String[] args) {
        // new int[3]创建一个数组，能存3个整数
        // new String[2]创建一个数组，能存储2个字符串
        String[] a = new String[2];//定义数组

        a[0] = "hello"; //元素 索引（下标）
        a[1] = "world";
        System.out.println(a[0]);
        System.out.println(a[1]);

        //以下是方法2
        String[] b = new String[]{"hello", "world"};
        System.out.println(b[0]);
        System.out.println(b[1]);

        //以下是方法3
        String[] c = {"hello", "world"};
        System.out.println(c[0]);
        System.out.println(c[1]);
    }
}
```
## 数组长度
```java
int len = arr.length;
```

## 访问数组元素
```java
int x = arr[0];        // 获取第一个元素
arr[1] = 100;          // 修改第二个元素为 100
```
## 遍历数组

```java
for (int i = 0; i < arr.length; i++) {
    System.out.println(arr[i]);
}
```

```java
for (int num : arr) {
    System.out.println(num);
}
```
## 数组复制/子数组提取
```java
import java.util.Arrays;

int[] copy = Arrays.copyOf(arr, arr.length);             // 完整复制
int[] sub = Arrays.copyOfRange(arr, 1, 4);               // 提取索引1~3
```

## 数组转其他类型
转List
```java
Integer[] boxedArr = {1, 2, 3};
List<Integer> list = Arrays.asList(boxedArr);
```

⚠️ 对于 `int[]` 不能直接转，要先装箱（boxing）：
```java
int[] arr = {1, 2, 3};
List<Integer> list = Arrays.stream(arr).boxed().collect(Collectors.toList());
```

toString返回数组的字符串形式 `Arrays.toString(arr)`

```java
import java.util.Arrays;  
  
public class ArraysMethod01 {  
    public static void main(String[] args) {  
        Integer[] integers = {1,20,90};  
//        //遍历数组  
//        for(int i = 0; i < integers.length; i++){  
//            System.out.println(integers[i]);  
//        }  
        //直接使用Arrays.toString方法，显示数组  
        System.out.println(Arrays.toString(integers));  
    }  
}
```

得到[1,20,90]
## sort排序（自然排序和定制排序）

数组是引用类型，通过sort排序后，会直接影响到 实参 arr

```java
import java.util.Arrays;  
public class ArraysSort {  
    public static void main(String[] args) {  
        Integer[] arr = {1, -1, 7, 0, 89}; 
        Arrays.sort(arr);  
        System.out.println(Arrays.toString(arr));  
    }  
}
```

sort重载的，也可以通过一个接口 comparator 实现定制排序（韩顺平java481）
- 调用定制排序时，传入两个参数(1) 要排序的数组（2）实现了Comparator接口的匿名内部类，要求实现compare方法
- new Comparator() 这里不是在创建 Comparator 接口的对象，而是定义了一个“匿名内部类”，并实现了 Comparator 接口。
- compare() 方法是 Comparator 接口中的抽象方法，所以匿名内部类必须实现它。

```java
Arrays.sort(slots1, (a, b) -> a[0] - b[0]); // 按照升序排序
```
等价于
```java
Arrays.sort(slots1, new Comparator<int[]>() {
    public int compare(int[] a, int[] b) {
        return a[0] - b[0]; 
    }
});
```
## 数组中是否包含哪个元素
```java
boolean contains = Arrays.stream(arr).anyMatch(x -> x == target);
```
## 数组求和/最大值/最小值
```java
int sum = Arrays.stream(arr).sum();
int max = Arrays.stream(arr).max().getAsInt();
int min = Arrays.stream(arr).min().getAsInt();
```
## fill数组填充
### 一维数组填充
```java
int[] arr = new int[5];  // 默认值为 0
Arrays.fill(arr, 7);     // 全部填成 7
```
### 二维数组填充
```java
int[][] arr = new int[3][4];
for (int i = 0; i < arr.length; i++) {
    Arrays.fill(arr[i], 7);
}
```

或者
```java
char[][] board = new char[4][4];
for (char[] row : board) {
    Arrays.fill(row, '.');
}
```

## equals
如果arr和arr2数组元素一样，则返回true
```java
Integer[] arr = {1,2,3,4,5};  
Integer[] arr2 = {1,2,9,3,2};  
boolean equals = Arrays.equals(arr,arr2);
```

## 二维数组

### 定义
固定行、固定列
`int[][] dp = new int[m][n];`

分两步初始化（常用于“非规则”二维数组）
`int[][] dp = new int[m][];`
- 先创建 `m` 行（`dp.length == m`），但是每一行都是 `null`，还未初始化。
- 需要手动给每一行分配长度：
```java
for (int i = 0; i < m; i++) {
    dp[i] = new int[n];  // 也可以每行不同长度
}
```
初始状态：
`dp = [null, null, ..., null]  // m 行`
初始化完毕后：
```
dp = [
  [0, 0, ..., 0],  // dp[0] 有 n 个元素
  [0, 0, ..., 0],  // dp[1] 有 n 个元素
  ...
]
```
### 取长度、遍历
```java
int[][] grid = new int[3][4];

for (int i = 0; i < grid.length; i++) {
    for (int j = 0; j < grid[0].length; j++) {
        System.out.print(grid[i][j] + " ");
    }
}
```

### 行数和列数

| 获取内容             | 语法                | 说明                     |
| ---------------- | ----------------- | ---------------------- |
| 行数（竖向）           | `board.length`    | 获取总共有多少行               |
| 列数（横向）           | `board[0].length` | 获取第 0 行有多少列（假设所有行列数相同） |
| 第 `i` 行列数        | `board[i].length` | 获取第 `i` 行的列数（用在不规则数组时） |
| 第 `i` 行第 `j` 列元素 | `board[i][j]`     | 访问二维数组中的元素             |
### 拷贝
```java
output = new int[m][n];  
for (int i = 0; i < grid.length; i++) {  
    for (int j = 0; j < grid[0].length; j++) {  
        grid[i][j] = grid[i][j];  
    }  
}
```

# 集合
## 为什么需要集合？——数组Array的缺陷

- 长度开始时必须指定，而且一旦指定，不能更改
- 保存的必须为同一类型的元素
- 使用数组进行增加元素的示意代码-比较麻烦

## 集合的特点

- 可以动态保存任意多个对象
- 提供了一系列方便的操作对象的方法：add、remove、set、get等
- 使用集合添加，删除新元素的示意代码

## Collection树图
Collection接口有三个重要的子接口 `List / Set / Queue`,它们的实现类，统称为 **单列集合**
    子接口 List：存储有序的、可重复的数据（“动态”数组）
    子接口 Set：存储无序的、不可重复的数据
    子接口 Queue : 存储**先进先出**的数据结构，用于排队模型
![061801.PNG](https://cdn.jsdelivr.net/gh/hoo01/image_auto/061801.PNG)

## Map树图
Map接口的**实现子类**是**双列集合**，存放的K-V
![微信截图_20250306200237.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250306200237.png)

## Collection接口和常用方法
#### Collection接口实现类的特点
`public interface Collection<E> extends iterable<E>`
- collection实现子类可以存放多个元素，每个元素可以是Obiect
- 有些Collection的实现类，可以存放重复的元素，有些不可以
- 有些Collection的实现类，有些是有序的(List)，有些不是有序(Set)
- **Collection接口没有直接的实现子类，是通过它的子接口Set 和 List 来实现的**
#### Collection的常见简单操作

```java
public class CollectionMethod {  
    public static void main(String[] args) {  
        List list = new ArrayList();  
        list.add("jack");  
        list.add(10);  
        list.add(true);  
        System.out.println("list:"+list);  
        list.remove("jack");  
        list.remove(1);  
        System.out.println("list:"+list);  
        System.out.println(list.contains("jack")); //查找指定元素是否存在 
        System.out.println(list.size()); //获取元素个数 
        System.out.println(list.isEmpty());//判断是否为空
        list.clear();  
        ArrayList list2 = new ArrayList();  
        //allAll加入多个的多个元素必须是集合的形式，containsAll,removeAll同理  
        System.out.println(list2.addAll(Arrays.asList("红楼梦", "三国演义")));  
    }  
}
```

#### Collection接口遍历元素方式1-使用Iterator（迭代器）

- lterator对象称为选代器，主要用于遍历 Collection 集合中的元素。
- 所有实现了Collection接口的集合类都有一个iterator()方法，用以返回一个实现了lterator接口的对象,即可以返回一个选代器。
- lterator 仅用于遍历集合，lterator 本身并不存放对象。
- lterator 的结构
![微信截图_20250306210303.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250306210303.png)

错误的遍历：
每次调用iterator都是在重新生成iterator，都取第一个元素
```java
while(coll.iterator().hasNext()){
     System.out.println(coll.iterator().next())
}
```

![微信截图_20250306210942.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250306210942.png)

```java
public class CollectionIterator {  
    public static void main(String[] args) {  
         Collection col = new ArrayList(); //向上转型  
  
         col.add(new Book("sanguoyanyi","Luoguanzhong",10.1));  
         col.add(new Book("xiaolifeidao","Gulong",5.1));  
         col.add(new Book("Hongloumeng","Caoxueqin",34.6));  
  
        //先得到col对应的迭代器  
        Iterator iterator = col.iterator();  
        //while循环遍历  
        while (iterator.hasNext()) {//判断是否还有数据 itit快捷键快速生成  
            //返回下一个元素，类型是Object(因为集合本身什么类型都能放）  
            Object obj = iterator.next();//编译类型是Object,运行类型是真正存放的类型  
            System.out.println("obj" + obj);  
        }  
        //iterator.next();//退出while循环后，迭代器已指向最后的元素，会抛出异常  
        //如果需要再次迭代，需要重置迭代器  
        iterator =col.iterator();  
        while (iterator.hasNext()) {  
            Object next =  iterator.next();  
            System.out.println("no.2round obj" + next);  
        }  
    }  
}
```

### Collection接口遍历元素方式2-for循环增强

增强for循环，可以代替iterator选代器，特点:增强for就是简化版的iterator本质一样。只能用于遍历集合或数组。

快捷键 `I`
```java
/**增强for循环  
 * for(元素类型 元素名：集合名或数组名） ｛  
 *     访问元素  
 * ｝  
 */  
for(Object book : col) {  
    System.out.println(book);  
}
```

你仔细品
![微信截图_20250313164459.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250313164459.png)
赋值操作1
赋值操作2 创建一个新的局部变量 s，让 s 指向 arr1 中某个元素的地址。`String` 是 **不可变对象（Immutable）**，所以 `s = "MM"` 只是让 `s` 这个变量指向了新的 `"MM"` 对象，而 **并不会修改 `arr1` 数组本身的内容**。
### !!!集合和数组间的转换
集合 -> 数组 
`Object[] arr = coll.toArray()`

数组 -> 集合 
`String[] arr = new String[]{"AA","BB","CC"};`
`Collection list = Arrays.asList(arr)`

## List接口和常用方法

### List接口基本介绍
List接口是Collection接口的子接口 
1）List集合类中元素有序（即添加顺序和取出顺序一致）、且可重复
2）每个元素都有对应的索引，即支持索引 `list.get()`
3）List容器中的元素都对应一个整数型的序号记载其在容器中的位置，可以根据序号存取容器中的元素。
4）JDK API中List接口的实现类有很多，不止ArrayList、LinkedList、Vector
### List equals
- `List` 的 `equals` 方法是**基于顺序和元素内容**的，**而不是基于 hashCode**。`Set` 需要 **`equals()` + `hashCode()`** 共同保证元素唯一性，而 `List` 主要依赖 `equals()` 进行比较。 两个 `List` 只要**长度相同**，**对应索引的元素相等**，就会被认为是相等的。
- 如果 `List` 里存放的是自定义对象（如 `Person` 类），那么**应该重写 `equals()` 和 `hashCode()` 方法**，否则 `List` 进行比较时会使用 `Object` 默认的 `equals()`，即**比较引用地址**，可能导致意外的结果。
- 但是不管怎么样，反正equals和hashcode都重写就对了

### List常用方法
#### 创建
1.`List.of(...)`不允许修改新增，不允许null元素
```java
List<T> list = List.of(e1, e2, e3, ...);
```

2.`Arrays.asList(...)` 是 Java 中一个 **非常常用的方法**，用于把一个数组或者一系列元素转成一个 **固定大小的 List**。
- 允许null元素
- 创建的 `List` **不能增删元素**，但可以修改
- 返回的是 `java.util.Arrays$ArrayList`，不是 `java.util.ArrayList`！容易误解
```java
List<T> list = Arrays.asList(e1, e2, e3, ...);
或者：
String[] arr = {"a", "b", "c"};
List<String> list = Arrays.asList(arr);// 输出 [a, b, c]
```
#### 增
`void add(int index, Object ele)`在index位置插入ele元素
`boolean addAll(int index, Collection eles)`在index位置开始将eles中的所有元素加进来
#### 删
`Object remove(int index)`移除指定index位置的元素，并返回此元素
`list.remove(Integer.valueof(2))`删除数据2
#### 改
`Object set(int index，0bject ele)`设置指定index位置的元素为ele ，相当于是替换。
#### 查
`Object get(int index)`获取指定index位置中的元素
`int indexOf(Object obj)`返回obj在集合中首先出现的位置
`int lastIndexOf(Object obj)`返回obj在集合中末次出现的位置
`List subList(int fromIndex,int toIndex)`返回从fromIndex到toIndex位置的子集合 前闭后开
#### 长度
`size()`
```java
public class ListMethod {  
    public static void main(String[] args) {  
        List list = new ArrayList();  
        list.add("zhangsanfeng");  
        list.add("lisi");  
        list.add(1,"zhangsanfeng");//指定索引位置，如不带索引，默认加在最后  
        list.addAll(1,asList("momo","duobi"));  
        System.out.println(list.indexOf("lisi"));  
        System.out.println(list.lastIndexOf("zhangsanfeng"));  
        List list1 = list.subList(0, 2);  
        System.out.println(list1);  
    }  
}
```
### 遍历
![微信截图_20250307111241.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250307111241.png)

## ArrayList
ArrayListSource.java底层源码
1）ArrayList中维护了一个Object类型的数组elementData.
transient Object[] elementData (transient表示该属性不被序列化)
2）当创建ArrayList对象时，如果使用的是无参构造器，则初始elementData容量为0，第1次添加，则扩容elementData为10，如需要再次扩容，则扩容elementData为1.5倍。
3）如果使用的是指定大小的构造器，则初始elementData容量为指定大小，如果需要扩容则直接扩容elementData为1.5倍。

## Vector
Vector底层也是Object[]数组
![微信截图_20250309164745.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250309164745.png)

## LinkedList

1)LinkedList底层实现了双向链表和双端队列特点
2)可以添加任意元素(元素可以重复)，包括null
3)线程不安全，没有实现同步

### 底层结构
1)LinkedList底层维护了一个双向链表
2)LinkedList中维护了两个属性first和last分别指向 首节点和尾节点
3)每个节点(Node对象)，里面又维护了prev、next、item三个属性，其中通过prev指向前一个，通过next指向后一个节点。最终实现双向链表.

![微信截图_20250309195332.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250309195332.png)
该图中每一个方块都是一个Node

4)所以LinkedList的元素的添加和删除，不是通过数组完成的，相对来说效率较高

```java
public class LinkedList01 {  
    public static void main(String[] args) {  
        //模拟一个简单的双向链表  
        Node jack = new Node("jack");  
        Node tom = new Node("tom");  
        Node hsp = new Node("hsp");  
  
        //连接三个结点，形成双向链表  
        jack.next = tom;  
        tom.next = hsp;  
        hsp.pre = tom;  
        tom.pre = hsp;  
  
        Node first = jack;//双向链表的首节点  
        Node last = hsp;//双向链表的尾节点  
  
        //演示从头到尾进行遍历  
        while (true) {  
            if (first == null) {  
                break;  
            }  
            System.out.println(first);  
            first = first.next;  
        }  
        //演示链表的添加数据，插入一个对象  
        Node smith = new Node("smith");  
        smith.next = hsp;  
        hsp.pre = smith;  
        smith.pre = tom;  
        tom.next = smith;  
  
        first = jack;//first再次指向jack  
        System.out.println("=======");  
        while (true) {  
            if (first == null) {  
                break;  
            }  
            System.out.println(first);  
            first = first.next;  
        }  
  
    }  
}  
  
//定义一个Node类，Node对象表示双向链表的一个结点  
class Node {  
    public Object item; //真正存放数据的地方  
    public Node next; //指向下一个结点  
    public Node pre; //指向前一个结点  
    public Node(Object name) {  
        this.item = name;  
    }  
    public String toString() {  
        return "Node name=" + item;  
    }  
}
```

### LinkedList的方法

可以使用iterator迭代器、增强for循环进行遍历

#### List 接口方法（按索引操作）

| 方法                          | 说明          |
| --------------------------- | ----------- |
| `add(E e)`                  | 末尾添加元素      |
| `add(int index, E element)` | 指定位置插入元素    |
| `get(int index)`            | 获取指定位置的元素   |
| `set(int index, E element)` | 替换指定位置的元素   |
| `remove(int index)`         | 移除指定位置的元素   |
| `indexOf(Object o)`         | 查找第一个出现的位置  |
| `lastIndexOf(Object o)`     | 查找最后一个出现的位置 |
| `size()`                    | 获取元素个数      |
| `isEmpty()`                 | 是否为空        |
#### Deque 接口方法（双端队列操作）
▶️ 头部操作（队首）

| 方法              | 说明                    |
| --------------- | --------------------- |
| `addFirst(E e)` | 添加到头部                 |
| `getFirst()`    | 获取头部元素（不移除）           |
| `removeFirst()` | 移除并返回头部元素             |
| `peekFirst()`   | 获取头部元素，队列为空返回 null    |
| `pollFirst()`   | 移除并返回头部元素，队列为空返回 null |
#### 🔚 尾部操作（队尾）

| 方法             | 说明                    |
| -------------- | --------------------- |
| `addLast(E e)` | 添加到尾部                 |
| `getLast()`    | 获取尾部元素（不移除）           |
| `removeLast()` | 移除并返回尾部元素             |
| `peekLast()`   | 获取尾部元素，队列为空返回 null    |
| `pollLast()`   | 移除并返回尾部元素，队列为空返回 null |
#### ⛓️ Stack 用法（栈操作）

| 方法          | 说明                     |
| ----------- | ---------------------- |
| `push(E e)` | 入栈（等价于 `addFirst`）     |
| `pop()`     | 出栈（等价于 `removeFirst`）  |
| `peek()`    | 查看栈顶元素（等价于 `getFirst`） |
#### 🔍 其他有用方法

| 方法                     | 说明          |
| ---------------------- | ----------- |
| `contains(Object o)`   | 是否包含某元素     |
| `clear()`              | 清空所有元素      |
| `toArray()`            | 转为数组        |
| `iterator()`           | 返回迭代器       |
| `descendingIterator()` | 反向迭代器（从尾到头） |
## ArrayList、LinkedList和Vector的比较

- **ArrayList：**
    - ava 编程中最常见、默认使用的 `List` 实现就是 `ArrayList`
    - 底层使用0bject[]数组存储，随机访问效率高。
    - 在添加数据、查找数据时，效率较高在插入、删除数据时，效率较低
    - 线程不安全
- **LinkedList：**
    - 底层使用双向链表的方式进行存储；
    - 对集合中的数据进行频繁的删除、插入操作时，建议使用此类。在插入、删除数据时，效率较高；在添加数据、查找数据时，效率较低。
    - 一般用在队列/栈
- Vector：
    - List的古老实现类;
    - 线程安全的、效率低；
    - 底层使用0bject[ ]数组存储。
    - 多线程环境但很少使用

## 练习题-随机字符计数
```java
public class ListTest {  
    public static void main(String[] args) {  
        //需求1：随机生成30个小写字母，存放在ArrayList中  
        ArrayList list = new ArrayList();  
        for (int i = 0; i <30; i++) {  
            //"a"-"z" [97,122]  
            list.add((char)(Math.random()*(122-97+1) +97)+"");  
        }  
        System.out.println(list);  
        int acount = listTest(list, "a");  
        int bcount = listTest(list, "b");  
        int ccount = listTest(list, "c");  
        int dcount = listTest(list, "d");  
  
        System.out.println("acount: " + acount);  
        System.out.println("bcount: " + bcount);  
        System.out.println("ccount: " + ccount);  
        System.out.println("dcount: " + dcount);  
  
    }  
    //需求2：查找指定元素出现的次数  
    public static int listTest(Collection list,String s) {  
        int count = 0;  
        for(Object o : list) {  
            if(s.equals(o)) {  
                count++;  
            }  
        }  
        return count;  
    }  
}
```

注意点：
- **`ArrayList` 只能存放引用数据类型（对象），不能存放基本数据类型**。`char` 是 **基本数据类型**，而 `String` 是 **引用数据类型**。
- **如果用 `ArrayList` 存放 `char`，它会被自动装箱（Autoboxing）为 `Character`**。

## Set接口
- `Set` 是 Java 的一个接口，不能直接创建实例
- 和List接口一样，Set接口也是Collection的子接口，因此，常用方法和Collection接口一样，没有新增的方法（没有索引）。
- 没有索引
- **不允许重复元素**，最多包含一个null
### Set接口的遍历方式
同Collection的遍历方式一样，因为Set接口是Collection接口的子接口
- 可以使用迭代器
- 增强for
- ==不能使用==索引的方式来获取，没有`get()`方法，不能用普通的for循环

### Set的使用场景
Set使用的频率较少，常用来去重

🔷 1. 创建 Set
`Set<String> set = new HashSet<>();`

🔷 2. 添加元素 `add()`
`set.add("apple"); 
- ✅ 添加成功返回 `true`
- ❌ 如果元素已存在，返回 `false`（Set 不允许重复元素）

🔷 3. 判断是否包含 `contains()`
`set.contains("apple");  // true 
- 时间复杂度为 O(1)（HashSet）

🔷 4. 删除元素 `remove()`
`set.remove("apple");  // 移除元素`

🔷 5. 获取大小 `size()`
`int n = set.size();`

🔷 6. 遍历 Set
📍 用 for-each 循环
```java
for (String s : set) {     
      System.out.println(s); 
}
```

📍 用迭代器（Iterator）
```java
Iterator<String> it = set.iterator();
while (it.hasNext()) {
    String s = it.next();
    System.out.println(s);
}
```

🔷 7. 清空 Set `clear()`
`set.clear();  // 删除所有元素`

🔷 8. 判断是否为空 `isEmpty()`
`set.isEmpty();  // true if size == 0`

🔷 9. 与 List 转换

📌 List → Set（去重）
```java
List<String> list = Arrays.asList("a", "b", "a");
Set<String> set = new HashSet<>(list);  // 自动去重
```

📌 Set → List
`List<String> list = new ArrayList<>(set);`
### HashSet - Set接口主要实现类

- HashSet底层是HashMap，数组+单向链表+红黑树结构进行存储
- Hashset不保证存放和取出顺序一致，取决于hash后，再确定索引的结果
- 不能有重复元素/对象。在前面Set 接口使用已经讲过
- HashSet 集合判断两个元素相等的标准：两个对象通过 `hashCode()` 方法得到的哈希值相等，并且两个对象的 `equals() `方法返回值为true。
- `HashSet` **不能** 直接使用 `compareTo()` 方法，因为 **`HashSet` 的底层是哈希表（`HashMap`），它不依赖 `compareTo()` 进行元素的存储和去重**，而是依赖 `hashCode()` 和 `equals()`。
- 存放数据是**无序的**，无序性 != 随机性
    - hashcode()对每个对象生成一个哈希值，决定了对象在数组中的位置，这个位置不是依次排列的，表现为无序性。
     - 这就是为什么判断相同，要同时重写equals()和hashcode()。如果没有重写，默认的object.hashcode()对两个内容相同的元素会生成不同的hashcode。
#### HashSet底层扩容机制说明
- 分析HashSet底层是HashMap，HashMap底层是（数组+链表+红黑树）
- 添加一个元素时，先得到hashcode - 会转成->索引值
- 找到存储数据表table，看这个索引位置是否已经存放的有元素，如果没有，直接加入；如果有， 调用`equals`比较，如果相同，就放弃添加，如果不相同，则添加到最后
- 第一次添加时，table数组扩容到16，临界值（threshold）是16`*`加载因子（loadfactor）0.75=12,如果table数组到了临界值12，就会扩容到16`*`2=32，新的临界值就是32`*`0.75=24,以此类推。注意！这里的数组元素不仅包括table表的内容，也包括链表元素，也就是，即使只有横向链表增加，依然会触发扩容机制。
- 在Java8中,如果一条链表（横的链）的元素个数到达`TREEIFY_THRESHOLD`(默认是8)，并且table（竖的表）的大小>=`MIN TREEIFY_CAPACITY`(默认64),就会进行树化(红黑树)，否则依然采取数组扩容机制
![微信截图_20250311151952.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250311151952.png)
数组链表模拟代码
```java
public class HashSetStructure {  
    public static void main(String[] args) {  
        //模拟一个HashSet的底层（HashMap）  
        //1.创建一个数组，数组的类型是Node[]  
        //2.有些人，直接把Node[] 数组成为表  
        Node[] table = new Node[16];  
        System.out.println("table = " + table);  
        //3.创建结点  
        Node john = new Node("john", null);  
        table[2] = john;  
        Node jack = new Node("jack", null);  
        john.next = jack;//将jack结点挂载到john后面  
        Node rose = new Node("rose", null);  
        jack.next = rose;//将rose结点挂载到jack后面  
        Node lucy = new Node("lucy", null);  
        table[3] = lucy;//把lucy放到table表索引为3  
  
        System.out.println("table = " + table);  
    }  
}  
  
class Node { //结点，存储数据，指向下一个结点，形成链表  
    Object item;  
    Node next; // 指向下一个结点  
  
    public Node(Object item, Node next) {  
        this.item = item;  
        this.next = next;  
    }  
}
```

#### HashSetExercise.java

定义一个Emiployee类，该类包含:private成员属性name,age 要求:
1. 创建3个Employee 放入 HashSet中
2. 当 name和age的值相同时，认为是相同员工,不能添加到HashSet集合中

答：
首先这是三个不同的对象，如果按原有规则，hashcode不同，都可以加入 
如果不重写 `equals()` 和 `hashCode()`，即使 `Employee("milan", 18)` 看起来一样，它们仍然会被 `HashSet` 认为是不同的对象

#默认 `Object` 的 `equals()` 只会检查内存地址(`==` 比较的是 **内存地址**)

```java
public boolean equals(Object obj) {
    return (this == obj);
}
```

```java
public class HashSetExercise {  
    public static void main(String[] args) {  
        HashSet hashset = new HashSet();  
        hashset.add(new Employee("milan",18));  
        hashset.add(new Employee("smith",28));  
        hashset.add(new Employee("milan",18));  
        System.out.println("hashset: " + hashset);  
    }  
}
```

重写equals方法,让它比较两个对象的内容
![微信截图_20250312110423.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250312110423.png)
![微信截图_20250312110744.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250312110744.png)
![微信截图_20250312110935.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250312110935.png)
![微信截图_20250312110954.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250312110954.png)

这样会得到，
```java
@Override
public int hashCode() {
    return Objects.hash(name, age);
}
```

这一步的目的是**生成哈希码，使 `HashSet`、`HashMap`、`HashTable` 等哈希结构能正确工作**。
- **存入数据时**，对象会根据 `hashCode()` 计算哈希值，放入对应的哈希桶（bucket）。**查找数据时**，首先用 `hashCode()` 确定所在桶，然后用 `equals()` 检查是否相等。
- **如果 `hashCode()` 不重写，`HashSet` 认为 `e1` 和 `e2` 是**不同的对象，它们会被存入不同的哈希桶，每个对象都会被放入不同的桶，导致 `equals()` 无法正确判断是否重复。
- 内部代码`return name.hashCode() * 31 + age;如果 `name` 和 `age` 相同，哈希值也会相同，符合 `equals()` 规则。

```java
@Override
public boolean equals(Object o) {
    if (this == o) return true;  
    // 谁调用equals方法谁就是this,这一步单纯比较两个对象的内存地址
    if (o == null || getClass() != o.getClass()) return false; 
    // o不是null，并且o.getClass()和this.getClass()都是Employee，继续执行。
    Employee employee = (Employee) o;  
    // 把o强制转换成Employee类型，便于访问name和age。
    return age == employee.age && Objects.equals(name, employee.name); 
    // 比较name和age是否相等.相同则true，不同是false
    //Objects.equals适用于引用类型，不适用于基本类型。确保比较的name是内容而不是内存地址。
}
```

**如果桶里已经有对象，调用 `equals()` 进一步判断是否为同一对象**：
- **如果 `equals()` 返回 `true`**，说明是相同的对象，不再存入 `HashSet`（去重）。
- **如果 `equals()` 返回 `false`**，说明虽然 `hashCode()` 相同，但对象不一样（哈希碰撞），仍然可以存入 `HashSet`。

#### LinkedHashSet-HashSet子类

- LinkedHashSet底层是一个LinkedHashMap,底层维护了一个 数组+双向链表，所以有顺序
- 不允许重复元素
- LinkedHashSet 根据元素的 hashCode 值来决定元素的存储位置，同时使用链表维护元素的次序，这使得元素看起来是以插入顺序保存的。便于频繁地查询
- ![微信截图_20250312150019.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250312150019.png)

图中来看：
- `123` 和 `HSP` 在哈希表中的索引位置相同，说明它们的 **哈希值计算后得到相同的存储索引**，因此 **它们被存放在同一个哈希桶（bucket）中**。
- **但是 `LinkedHashSet` 需要保证插入顺序，所以 `HSP` 仍然必须接在 `123` 的后面形成链表。**

这种情况可能是 `HSP` 和 `123` 发生了 **哈希碰撞**，即：
`123.hashCode() % table.length == "HSP".hashCode() % table.length`

- 在LinkedHastset 中维护了一个hash表和双向链表(LinkedHashSet 有head 和 tail)
- 每一个节点有 before 和 after 属性,这样可以形成双向链表
- 在添加一个元素时，先求hash值，再求索引，确定该元素在table的位置，然后将添加的元素加入到双向链表(如果已存在，不添加[原则和hashset一样])
```
tail.next = newElement 
newElement.pre = tail
tail = newEelment;
```
- 这样的话，我们遍历LinkedHashSet 也能确保插入顺序和遍历顺序一致
- 添加第一次时，直接将 数组table 扩容到 16,存放的结点类型是 `LinkedHashMap$Entry`数组是 `HashMap$Node[]`存放的元素/数据是 `LinkedHashMap$Entry`

### TreeSet

- 底层数据结构：**红黑树**。
- 不再考虑hashcode()和equals()方法，重写这俩无用。
- 特点：不允许重复、**实现排序（自然排序或定制排序）**
- 添加到treeset的**元素必须同类型**
#### 使用Comparable自然排序比较

```java User
public class User implements Comparable {  
    String name;  
    int age;  
  
    public User() {  
    }  
  
    public User(String name, int age) {  
        this.name = name;  
        this.age = age;  
    }  
  
    @Override  
    public String toString() {  
        return "User{" +  
                "name='" + name + '\'' +  
                ", age=" + age +  
                '}';  
    }  
  
    @Override  
    public boolean equals(Object o) {  
        System.out.println("User equals()....");  
        if (this == o) return true;  
        if (o == null || getClass() != o.getClass()) return false;  
        User user = (User) o;  
        return age == user.age && Objects.equals(name, user.name);  
    }  
  
    @Override  
    public int hashCode() {  
        return Objects.hash(name, age);  
    }  
    /*  
    * 自然排序：想同时比较年龄和姓名  
    * */    
    @Override  
    public int compareTo(Object o) {  
        if(this == o){  
            return 0;  
        }  
        if(o instanceof User){  
            User u = (User) o;  
            int value = this.age - u.age;  
            if(value == 0){  
                return this.name.compareTo(u.name);  
            }  
            return value;  
        }  
        throw new RuntimeException("类型不匹配");  
    }  
}
```

```java
public class TreeSetTest {  
    @Test  
    public void test2(){  
        TreeSet set = new TreeSet();  
        User u1 = new User("tom",23);  
        User u2 = new User("jerry",43);  
        User u3 = new User("rose",13);  
        User u4 = new User("jack",23);  
        User u5 = new User("tony",33);  
  
        set.add(u1);  
        set.add(u2);  
        set.add(u3);  
        set.add(u4);  
        set.add(u5);  
  
        Iterator iterator = set.iterator();  
        while(iterator.hasNext()){  
            System.out.println(iterator.next());  
        }  
    }  
}
```

#### 使用comparator定制排序比较
```java
public class TreeSetTest {  
    @Test  
    public void test3(){  
        Comparator comparator = new Comparator() {  
            @Override  
            public int compare(Object o1, Object o2) {  
                if(o1 instanceof User && o2 instanceof User){  
                    User u1 = (User) o1;  
                    User u2 = (User) o2;  
  
                    int value = u1.getName().compareTo(u2.getName());  
                    if(value != 0){  
                        return value;  
                    }  
                    return -(u1.getAge() - u2.getAge());  
                }  
                throw new RuntimeException("类型不匹配");  
            }  
        };  
  
        TreeSet set = new TreeSet(comparator);  
        //如果提供了 Comparator，那么 TreeSet 会按照 Comparator 规则排序。不传 Comparator，它会尝试使用 User 类的 "自然排序"  
        User u1 = new User("tom",23);  
        User u2 = new User("jerry",43);  
        User u3 = new User("rose",13);  
        User u4 = new User("jack",23);  
        User u5 = new User("tony",33);  
  
        set.add(u1);  
        set.add(u2);  
        set.add(u3);  
        set.add(u4);  
        set.add(u5);  
  
        Iterator iterator = set.iterator();  
        while(iterator.hasNext()){  
            System.out.println(iterator.next());  
        }  
    }  
}
```

### Set接口练习1 - integer去重
定义方法如下:public static List duplicateList(List list)
要求:
- 参数List中只存放Integer的对象
- 在List内去除重复数字值，尽量简单

```java
public class Exer01 {  
    public static void main(String[] args) {  
        ArrayList list = new ArrayList();  
        list.add(34);  
        list.add(45);  
        list.add(65);  
        list.add(34);  
        list.add(45);  
        list.add(65);  
        list.add(14);  
        list.add(22);  
  
        List newList =duplicateList(list);  
        System.out.println(newList);  
    }  
    public static List duplicateList(List list){  
        HashSet set = new HashSet();  
        for(Object obj : list){  
            set.add(obj);  
        }  
        ArrayList list1 = new ArrayList();  
        for(Object obj : set){  
            list1.add(obj);  
        }  
        return list1;  
    }  
}
```

另一种更优雅的写法
```java
public static List duplicateList(List list){  
    HashSet set = new HashSet(list);  //hashset自动去重
    List list1 = new ArrayList(set);  //ArrayList也有一个接收Collection的构造方法
    return list1;  
}
```

### Set练习2 - 不重复的随机数

编写一个程序，获取10个1至20的随机数，要求随儿数不能重复。并把最终的随机数输出到控制台。
`math.random *（b-a+1）+a`

```java
public class Exer02 {  
    public static void main(String[] args) {  
        Set set = new HashSet();  
  
        while (set.size() < 10) {  
            int random = (int)(Math.random()*(20-1+1)+1);  
            set.add(random);  
        }  
  
        Iterator iterator = set.iterator();  
        while (iterator.hasNext()) {  
            System.out.println(iterator.next());  
        }  
    }  
}
```
## Map接口

- **Map与Collection并列存在。用于保存具有映射关系的数据:Key-Value**
    k-v
    k->具体的对象
    v->在之前的Collection里是默认的常量对象，如present； 在Map里是自己输入的
- Map 中的 key 和 value 可以是任何引用类型的数据，会封装到HashMap$Node对象中
- Map 中的 key 不允许重复，原因和HashSet 一样，value可以重复
- Map 的key 可以为 null, value 也可以为null ，注意  只能有一个key为null；value 可以多个为null
- 常用String类作为Map的 key

### Map接口实现类的对比

- **HashMap:** 主要实现类；线程不安全的，效率高；可以添加null的key和value；底层数据结构是数组+单向链表+红黑树
    - **LinkedHashMap:** HashMap的子类；在HashMap的结构上，增加了一对双向链表，用于记录添加元素的先后顺序，便于频繁的遍历

- **TreeMap:** 底层使用红黑树存储；可以按照添加key-value中的key元素的指定的属性大小进行遍历。使用自然排序和定制排序。

- **HashTable:** 古老实现类；线程安全的，效率低;不可以添加null的key和value；结构是数组+单向链表结构
    - Properties: 其key和value都是string类型，常用来处理属性文件


```java
public class Map_ {  
    public static void main(String[] args) {  
        Map map = new HashMap();  
        map.put("key1", "value1");//k-v  
        map.put("key2", "value2");  
        map.put("key1", "value3");//相同的key会替换之前的value  
        map.put("key13", "value3");//value可以重复  
        System.out.println("map = " + map);  
        System.out.println(map.get("no2"));//通过get方法通过key返回value  
    }  
}
```

### Map中key-value的特点

![微信截图_20250312173650.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250312173650.png)

- 放key的集合是Set,无序性、不可重复性。即同一个Map对象所对应的类，须重写hashCode()和equals()
- 放value的集合，value的位置取决于key，也是无序的，但是允许重复，并不满足list有序重复的特点，所以只能称为collection
![微信截图_20250316105508.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250316105508.png)

- key 和 value 之间存在单向一对一关系，即通过指定的 key 总能找到唯一的、确定的 value，不同key对应的value可以重复 。value所在的类要重写equals()方法。记不清就全重写。
- **key和value构成一个entry。所有的entry彼此之间是无序的、不可重复的。
### Map接口的常用方法

#### 增,改
1. `put(Object key,Object value)`
源码：
```java
public V put(K key, V value) {  
    return putVal(hash(key), key, value, false, true); 
}
```
✅ 如果 key 已经存在：
就会覆盖原有的 value，即修改原来的值为新的 value。
✅ 如果 key 不存在：
就会插入一对新的 key-value 键值对。
如果你不想覆盖旧值，可以用 `putIfAbsent()` 来避免

2. `putIfAbsent(k, v)`**仅在 key 不存在时才赋值**，**如果 `k` 已经存在于 `Map` 中，`putIfAbsent(k, v)` 什么都不会做**，原来的值会被保留，`v` 被直接忽略。
```java
Map<Integer, List<Integer>> map = new HashMap<>();
map.putIfAbsent(3, new ArrayList<>());
map.get(3).add(10);
```

3.`computeIfAbsent`
computeIfAbsent(key, k -> v) **"Absent" 是指 key 不存在于 map 中**，如果 map 中 **没有这个 key（缺 key）**，那么就“计算”一个 value 放进去。
```java
Map<String, List<String>> map = new HashMap<>();
map.computeIfAbsent("A", k -> new ArrayList<>()).add("hello");
得到"A" → ["hello"]
map.computeIfAbsent("A", k -> new ArrayList<>()).add("banana"); // 不会执行 `k -> new ArrayList<>()`直接返回 `map.get(k)`
"A" → ["hello", "banana"]
```

| 方法                      | 含义                                 |
| ----------------------- | ---------------------------------- |
| `putIfAbsent(k, v)`     | 如果 `k` 不在 map 中，就 put `v`          |
| `computeIfAbsent(k, f)` | 如果 `k` 不在 map 中，就执行 `f(k)` 来生成 `v` |
#### 删
`Object remove(Object key)`
void clear()：清空当前map中的所有数据
#### 改
`put(Object key,Object value)`
`putAll(Map m)`
#### 查
```JAVA
1. 获取指定key对应的value（key 不存在时返回 null）
Object get(Object key)
```

```JAVA
2. map.getOrDefault(key, defaultValue)
```
**key**：你想查找的键。
**defaultValue**：如果这个键在 map 中不存在，就用这个默认值代替返回。
是对 **key 的查找行为**，但返回的是 **value**。
❗注意：**`defaultValue` 只是“查不到时返回的临时值”，它不会自动加入 map 中。**

举例1：
```java
Map<Integer, Integer> map = new HashMap<>();
map.put(3, 5);

int val = map.getOrDefault(3, 0);  // 返回 5
int val2 = map.getOrDefault(100, 0); // 返回 0，但 map 并不会新增 key=100

System.out.println(map.containsKey(100)); // false
```

举例2：
```JAVA
map.put(sum, map.getOrDefault(sum, 0) + 1);
```
> 如果 `sum` 这个值**已经存在**于哈希表里，就把它当前的次数取出来，加 1。  
> 如果 `sum` **还没有出现过**，就当它的次数是 0，然后加 1。
> 相当于：
```java
if (map.containsKey(sum)) {
    map.put(sum, map.get(sum) + 1);
} else {
    map.put(sum, 1);
}
```

- boolean containsKey(Object key)：是否包含指定的key
- boolean containsValue(Object value)：是否包含指定的value
- int size()：返回map中key-value对的个数
- boolean isEmpty()：判断当前map是否为空
- boolean equals(Object obj)：判断当前map和参数对象obj是否相等
### 元视图操作办法（重要）

| 方法               | 返回类型                   | 含义                                                                     |
| ---------------- | ---------------------- | ---------------------------------------------------------------------- |
| `map.keySet()`   | `Set<K>`               | 返回所有key构成的Set集合                                                        |
| `map.values()`   | `Collection<V>`        | 将每个 `key` 对应的 **value “打包”** 后，**按原样分开放进一个集合里**，而不是把所有 value 的内容合并到一起。 |
| `map.entrySet()` | `Set<Map.Entry<K, V>>` | 返回所有key-value对构成的Set集合，最常用 👈                                          |

重点：

| 表达式              | 类型                                               |
| ---------------- | ------------------------------------------------ |
| `map`            | `Map<Integer, Integer>`（不是Collection）            |
| `map.entrySet()` | `Set<Map.Entry<Integer, Integer>>` ✅ 是Collection |

### 遍历
```java
public class MapSource_ {  
    public static void main(String[] args) {  
        Map map = new HashMap();  
        map.put("no1","hsp");  
        map.put("no2","zwj");  
  
        Set set = map.entrySet();  
        System.out.println(set.getClass());  
        for (Object o : set) {  
            System.out.println(o);  
        }  
    }  
}
```

Map.Entry提供了getKey和getValue两种方法，方便遍历

```java
public class MapFor {  
    public static void main(String[] args) {  
        Map map = new HashMap();  
        map.put("dengchao","sunli");  
        map.put("wbq","mr");  
        map.put("sz","mr");  
        map.put("llb",null);  
        map.put(null,"lyf");  
        map.put("lh","gxt");  
  
        //第一组：先取出所有的key,包装成一个set,通过Key取出对应的Value  
        Set keyset = map.keySet();  
        //1.增强for  
        for (Object key : keyset) {  
            System.out.println(key + "-" + map.get(key));  
        }  
        //2.迭代器  
        Iterator iterator = keyset.iterator();  
        while (iterator.hasNext()) {  
            Object key =  iterator.next();  
            System.out.println(key + "-" + map.get(key));  
        }  
        //第二组：取出所有values  
        Collection values = map.values();  
        //这里可以使用Collections使用的遍历方法：增强for、iterator  
        
        * 第三组：通过EntrySet来获取k-v  
        Set entrySet = map.entrySet(); // EntrySet<Entry<K,V>>  
        System.out.println("entry取出法");  
        for (Object entry : entrySet) {  
            //entry是一个内部接口,调用需要加Map.,getKey()和getValue()  
            Map.Entry m = (Map.Entry) entry;  
            System.out.println(m.getKey() + "-" + m.getValue());  
        }  
        //迭代器  
        Iterator iterator3 = entrySet.iterator();  
        while (iterator3.hasNext()) {  
            Object entry = iterator3.next();  
            System.out.println(entry.getClass());//这里会输出HashMap$Node  
            Map.Entry m = (Map.Entry) entry;//向下转型  
            System.out.println(m.getKey() + "-" + m.getValue());  
        }  
    }  
}
```

### TreeMap（有序）
- 底层使用红黑树存储;
- 可以按照添加的key-value中的key元素的指定的属性的大小顺序进行遍历。
- 需要考虑使用1.自然排序 2.定制排序。
- 要求：向TreeMap中添加的key必须是同一个类型的对象，不一样怎么比较？

#### 自然排序举例：
之前实现了compareTo的 class User
```java
public class User implements Comparable {  
  ...省略各种构造... 
    /*  
    * 自然排序：想同时比较年龄和姓名,如果年龄相同，则比较姓名，从大到小  
    * */    @Override  
    public int compareTo(Object o) {  
        if(this == o){  
            return 0;  
        }  
        if(o instanceof User){  
            User u = (User) o;  
            int value = this.age - u.age;  
            if(value == 0){  
                return -this.name.compareTo(u.name);  
            }  
            return value;  
        }  
        throw new RuntimeException("类型不匹配");  
    }  
}
```

```java
public class TreeMapTest {  
    @Test  
    public void test() {  
        TreeMap map = new TreeMap();  
        User u1 = new User("tom",23); 
        //这里的User有compare方法，先排年龄，再排姓名从大到小  
        User u2 = new User("jerry",43);  
        User u3 = new User("rose",13);  
        User u4 = new User("jack",23);  
        User u5 = new User("tony",33);  
        map.put(u1,78);//这里的value是成绩  
        map.put(u2,76);  
        map.put(u3,88);  
        map.put(u4,45);  
        map.put(u5,99);  
  
        Set entrySet = map.entrySet();  
        for (Object entry : entrySet) {  
            System.out.println(entry);  
        }  
    }  
}
```

遍历得到按key排序的结果
```java
User{name='rose', age=13}=88
User{name='tom', age=23}=78
User{name='jack', age=23}=45
User{name='tony', age=33}=99
User{name='jerry', age=43}=76
```

#### 定制排序举例
```java
public class TreeMapTest {  
    @Test  
    public void test2() {  
        Comparator comparator = new Comparator() {  
            @Override  
            public int compare(Object o1, Object o2) {  
                if (o1 instanceof User && o2 instanceof User) {  
                    User u1 = (User) o1;  
                    User u2 = (User) o2;  
                    int value = u1.getAge() - u2.getAge();  
                    if (value == 0) {  
                        return u1.getName().compareTo(u2.getName());  
                    }  
                    return value;  
                }  
                throw new RuntimeException("类型不匹配");  
            }  
        };  
  
        TreeMap map = new TreeMap(comparator);  
        User u1 = new User("tom",23);  
        User u2 = new User("jerry",43);  
        User u3 = new User("rose",13);  
        User u4 = new User("jack",23);  
        User u5 = new User("tony",33);  
  
        map.put(u1,78);//这里的value是成绩  
        map.put(u2,76);  
        map.put(u3,88);  
        map.put(u4,45);  
        map.put(u5,99);  
  
        Set entrySet = map.entrySet();  
        for (Object entry : entrySet) {  
            System.out.println(entry);  
        }  
    }  
}

```

### Hashtable与Properties的使用
Properties:是HashTable的子类，其key和value都是string类型的，常用来处理属性文件

### 练习：歌手有哪些歌曲

```java
public class SingerTest {  
    public static void main(String[] args) {  
        HashMap singers = new HashMap();  
        String singer1 = "jay zhou"; //key  
        ArrayList songs1 = new ArrayList();//value  
        songs1.add("晴天");  
        songs1.add("七里香");  
        songs1.add("青花瓷");  
        singers.put(singer1,songs1);  
  
        String singer2 = "jj lin";  
        ArrayList songs2 = new ArrayList();  
        songs2.add("圈圈");  
        songs2.add("圆圆");  
        songs2.add("点点");  
        singers.put(singer2,songs2);  
  
        Set entrySet = singers.entrySet();  
        Iterator iterator = entrySet.iterator();  
        while (iterator.hasNext()) {  
            Map.Entry entry = (Map.Entry) iterator.next();  
            System.out.println("歌手:" + entry.getKey() + " 歌曲:" + entry.getValue());  
        }  
    }  
}
```

练习：选择省份和城市
```java
public class CityMapTest {  
    public static void main(String[] args) {  
        //1.获取Map，遍历map中的key  
        Map map = CityMap.model;  
        Set provinces = map.keySet();  
        for (Object province : provinces) {  
            System.out.print(province + "\t\t");  
        }  
        //2.根据提示，从键盘获取省份值，判断此省份是否存在，如果存在遍历其value中的各个城市，如果不存在，提示用户再输入  
        Scanner scan = new Scanner(System.in);  
        String[] cities;  
        while (true){  
            System.out.println("\n请选择省份:");  
            String province = scan.next();  
            cities = (String[]) map.get(province);  
  
            if(cities == null || cities.length == 0){  
                System.out.println("输入的省份有误，请重新输入");  
            } else {  
                break;//找到则退出循环  
            }  
        }  
        //3.根据提示，从键盘获取城市，遍历各个城市构成的String[]，判断输入的城市是否在数组中 
        for (int i = 0; i < cities.length; i++) {  
            System.out.print(cities[i] + "\t\t");  
        }  
        l : while (true){  
            System.out.println("\n请选择城市:");  
            String city = scan.next();//注意这一步在Array数组里了  
            for(int i = 0; i < cities.length; i++){  
                if(cities[i].equals(city)){  
                    System.out.println("信息登记完毕");  
                    break l;//如果不加l只能退出for循环，加了l直接退出while循环  
                }  
            }  
            System.out.println("输入的城市有误，请重新输入");  
        }  
        scan.close();  
    }  
}  
  
class CityMap{  
    public static Map model = new HashMap();  
  
    static {  
        model.put("北京", new String[] {"北京"});  
        model.put("上海", new String[] {"上海"});  
        model.put("天津", new String[] {"天津"});  
        model.put("重庆", new String[] {"重庆"});  
        model.put("黑龙江", new String[] {"哈尔滨","齐齐哈尔","牡丹江","大庆","伊春","双鸭山","绥化"});  
        model.put("吉林", new String[] {"长春","延边","吉林","白山","白城","四平","松原"});  
        model.put("河北", new String[] {"石家庄","张家口","邯郸","邢台","唐山","保定","秦皇岛"});  
    }  
}
```

## Collections工具类
### 区别Collection和Collections
`Collection` 
- 是 **Java 集合框架中的根接口**，它定义了集合的基本操作，如 `add()`、`remove()`、`size()` 等。
- 它是 **一个接口**，位于 `java.util` 包中，不能直接实例化。
- 主要的子接口包括：
    - `List`（如 `ArrayList`、`LinkedList`）
    - `Set`（如 `HashSet`、`TreeSet`）

Collections：
- 用于操作集合框架的一个工具类，可以把 **`Collections` 理解成一个“方法的集合”**，它本身不存储数据，而是提供 **操作集合的工具方法**。
- 所有方法都是 `static` 的，不能被实例化，如排序、查找、同步化等。位于 `java.util` 包中，
- 此时的集合框架包括Set/List/Map

| 特性         | `Collection`                      | `Collections`                                    |
| ---------- | --------------------------------- | ------------------------------------------------ |
| **类型**     | **接口**                            | **工具类**                                          |
| **定义位置**   | `java.util.Collection`            | `java.util.Collections`                          |
| **是否可实例化** | 不能，需通过 `List`、`Set`、`Queue` 等子类实现 | 不能，所有方法都是 `static`                               |
| **作用**     | 定义集合框架的基本操作，如 `add()`、`remove()`  | 提供集合工具方法，如 `sort()`、`shuffle()`                  |
| **子接口/子类** | `List`、`Set`、`Queue` 等            | 无                                                |
| **常用方法**   | `add()`、`remove()`、`size()`       | `sort()`、`shuffle()`、`reverse()`、`min()`、`max()` |

### 常用方法
Collections 中提供了一系列静态的方法对集合元素进行排序、查询和修改等操作，还提供了对集合对象设置不可变、对集合对象实现同步控制等方法（均为static方法）：

### **排序操作**
- reverse(List)：反转 List 中元素的顺序
- shuffle(List)：对 List 集合元素进行随机排序
- sort(List)：根据元素的自然顺序对指定 List 集合元素按升序排序
- sort(List，Comparator)：根据指定的 Comparator 产生的顺序对 List 集合元素进行排序
![微信截图_20250317110800.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250317110800.png)

- swap(List，int， int)：将指定 list 集合中的 i 处元素和 j 处元素进行交换

### **查找**

- Object max(Collection)：根据元素的自然顺序，返回给定集合中的最大元素(最右边的)
- Object max(Collection，Comparator)：根据 Comparator 指定的顺序，返回给定集合中的最大元素
- Object min(Collection)：根据元素的自然顺序，返回给定集合中的最小元素（最左边的）
- Object min(Collection，Comparator)：根据 Comparator 指定的顺序，返回给定集合中的最小元素
- int binarySearch(List list,T key)在List集合中查找某个元素的下标，但是List的元素必须是T或T的子类对象，而且必须是可比较大小的，即支持自然排序的。而且集合也事先必须是有序的，否则结果不确定。
- int binarySearch(List list,T key,Comparator c)在List集合中查找某个元素的下标，但是List的元素必须是T或T的子类对象，而且集合也事先必须是按照c比较器规则进行排序过的，否则结果不确定。
- int frequency(Collection c，Object o)：返回指定集合中指定元素的出现次数

### **复制、替换**

- void copy(List dest,List src)：将src中的内容复制到dest中
     这里需要考虑list容量问题，list的默认初始容量是10，如果复制的list数量大于10会出错
     ![微信截图_20250317112416.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250317112416.png)

- boolean replaceAll(List list，Object oldVal，Object newVal)：使用新值替换 List 对象的所有旧值
- 提供了多个unmodifiableXxx()方法，该方法返回指定 Xxx的不可修改的视图。
- ![微信截图_20250317112851.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250317112851.png)
### **添加**

- boolean addAll(Collection  c,T... elements)将所有指定元素添加到指定 collection 中。

### **同步**

- Collections 类中提供了多个 synchronizedXxx() 方法，该方法可使将指定集合包装成线程同步的集合，从而可以解决多线程并发访问集合时的线程安全问题：
- ![微信截图_20250317114008.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20250317114008.png)

### 练习题：扑克牌发牌
模拟斗地主洗牌和发牌并对牌进行排序的代码实现。
![image-20220915002714578.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/image-20220915002714578.png)

提示：考查HashMap、TreeSet、ArrayList、Collections

代码示例：

```java
/**
 * @author 尚硅谷-宋红康
 * @create 0:23
 */
public class PokerTest1 {
    public static void main(String[] args) {
        String[] num = {"3", "4", "5", "6", "7", "8", "9", "10", "J", "Q", "K", "A", "2"};
        String[] color = {"方片", "梅花", "红桃", "黑桃"};
        HashMap map = new HashMap(); // 存储索引和扑克牌
        ArrayList list = new ArrayList(); // 存储索引
        int index = 0; // 索引的开始值
        for (String s1 : num) {
            for (String s2 : color) {
                map.put(index, s2.concat(s1)); // 将索引和扑克牌添加到HashMap中
                list.add(index); // 将索引添加到ArrayList集合中
                index++;
            }
        }
        map.put(index, "小王");
        list.add(index);
        index++;
        map.put(index, "大王");
        list.add(index);
        // 洗牌
        Collections.shuffle(list);
        // 发牌
        TreeSet Tom = new TreeSet();
        TreeSet Jerry = new TreeSet();
        TreeSet me = new TreeSet();
        TreeSet lastCards = new TreeSet();

        for (int i = 0; i < list.size(); i++) {
            if (i >= list.size() - 3) {
                lastCards.add(list.get(i)); // 将list集合中的索引添加到TreeSet集合中会自动排序
            } else if (i % 3 == 0) {
                Tom.add(list.get(i));
            } else if (i % 3 == 1) {
                Jerry.add(list.get(i));
            } else {
                me.add(list.get(i));
            }
        }

        // 看牌
        lookPoker("Tom", Tom, map);
        lookPoker("Jerry", Jerry, map);
        lookPoker("康师傅", me, map);
        lookPoker("底牌", lastCards, map);

    }

    public static void lookPoker(String name, TreeSet ts, HashMap map) {
        System.out.println(name + "的牌是:");
        for (Object index : ts) {
            System.out.print(map.get(index) + " ");
        }

        System.out.println();
    }
}
```

为什么 `TreeSet<Integer>` 不需要显式 `compareTo()` 方法？
`TreeSet` 默认会对元素进行**自然排序（Natural Ordering）**。对于 **`Integer` 类型**，`TreeSet` 默认使用 **`Integer` 实现的 `Comparable` 接口** 来排序。

```java
TreeSet<Integer> set = new TreeSet<>();
set.add(3);
set.add(1);
set.add(2);
System.out.println(set); // 输出：[1, 2, 3]，自动排序

```
`Integer` **本身已经实现了 `Comparable` 接口**：
```java
public final class Integer extends Number implements Comparable<Integer> {
    public int compareTo(Integer anotherInteger) {
        return compare(this.value, anotherInteger.value);
    }
}
```
**因此，`TreeSet<Integer>` 直接按从小到大的顺序排序**，而无需手动提供 `Comparator`。

# 泛型

## 泛型的引入

在Java中，我们在声明方法时，当在完成方法功能时如果有`未知的数据`需要参与，这些未知的数据需要在调用方法时才能确定，那么我们把这样的数据通过`形参`表示。在方法体中，用这个形参名来代表那个未知的数据，而调用者在调用时，对应的传入`实参`就可以了。
![1563414367674.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/1563414367674.png)
受以上启发，JDK1.5设计了泛型的概念。泛型即为“`类型参数`”，这个类型参数在声明它的类、接口或方法中，代表未知的某种通用类型。

**举例1：**
集合类在设计阶段/声明阶段不能确定这个容器到底实际存的是什么类型的对象，所以**在JDK5.0之前只能把元素类型设计为Object，JDK5.0时Java引入了“参数化类型（Parameterized type）”的概念，允许我们在创建集合时指定集合元素的类型**。比如：`List<String>`，这表明该List只能保存字符串类型的对象。

使用集合存储数据时，除了元素的类型不确定，其他部分是确定的（例如关于这个元素如何保存，如何管理等）。

**举例2：**
`java.lang.Comparable`接口和`java.util.Comparator`接口，是用于比较对象大小的接口。这两个接口只是限定了当一个对象大于另一个对象时返回正整数，小于返回负整数，等于返回0，但是并不确定是什么类型的对象比较大小。JDK5.0之前只能用Object类型表示，使用时既麻烦又不安全，因此 JDK5.0 给它们增加了泛型。
![image-20220923154058074.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/image-20220923154058074.png)
其中`<T>`就是类型参数，即泛型。

## 使用泛型举例
自从JDK5.0引入泛型的概念之后，对之前核心类库中的API做了很大的修改，例如：JDK5.0改写了集合框架中的全部接口和类、java.lang.Comparable接口、java.util.Comparator接口、Class类等。为这些接口、类增加了泛型支持，从而可以在声明变量、创建对象时传入类型实参。

### 集合中没有使用泛型时
![image-20220411001522636.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/image-20220411001522636.png)
### 集合中使用泛型时
![image-20220411001549747.png](https://cdn.jsdelivr.net/gh/hoo01/image_auto/image-20220411001549747.png)
Java泛型可以保证如果程序在编译时没有发出警告，运行时就不会产生ClassCastException异常。即，把不安全的因素在编译期间就排除了，而不是运行期；既然通过了编译，那么类型一定是符合要求的，就避免了类型转换。

同时，代码更加简洁、健壮。

**把一个集合中的内容限制为一个特定的数据类型，这就是generic背后的核心思想。**
### 泛型在List中的使用举例：
```java
@Test
public void test1(){
    //举例：将学生成绩保存在ArrayList中
    //标准写法：
    //ArrayList<Integer> list = new ArrayList<Integer>();
    //jdk7的新特性：类型推断
    ArrayList<Integer> list = new ArrayList<>();

    list.add(56); //自动装箱
    list.add(76);
    list.add(88);
    list.add(89);
    //当添加非Integer类型数据时，编译不通过
    //list.add("Tom");//编译报错

    Iterator<Integer> iterator = list.iterator();
    while(iterator.hasNext()){
        //不需要强转，直接可以获取添加时的元素的数据类型
        Integer score = iterator.next();
        System.out.println(score);
    }
}
```
### 泛型在Map中的使用
```java
@Test
public void test2(){
    HashMap<String,Integer> map = new HashMap<>();

    map.put("Tom",67);
    map.put("Jim",56);
    map.put("Rose",88);
    //编译不通过
    //map.put(67,"Jack");

    //遍历key集
    Set<String> keySet = map.keySet();
    for(String str:keySet){
        System.out.println(str);
    }

    //遍历value集
    Collection<Integer> values = map.values();
    Iterator<Integer> iterator = values.iterator();
    while(iterator.hasNext()){
        Integer value = iterator.next();
        System.out.println(value);
    }

    //遍历entry集
    Set<Map.Entry<String, Integer>> entrySet = map.entrySet();
    Iterator<Map.Entry<String, Integer>> iterator1 = entrySet.iterator();
    while(iterator1.hasNext()){
        Map.Entry<String, Integer> entry = iterator1.next();
        String key = entry.getKey();
        Integer value = entry.getValue();
        System.out.println(key + ":" + value);
    }

}
```
### 比较器中使用泛型

#### 1.对员工姓名的首字母进行排序
```java
import com.example.study.equals.MyDate;  
  
public class Employee implements Comparable<Employee>{//注意这里
    private String name;  
    private int age;  
    private MyDate birthday;  
  
    public Employee(String name, int age, MyDate birthday) {  
        this.name = name;  
        this.age = age;  
        this.birthday = birthday;  
    }  
  
    public Employee() {  
    }  
  
    public String getName() {  
        return name;  
    }  
  
    public void setName(String name) {  
        this.name = name;  
    }  
  
    public int getAge() {  
        return age;  
    }  
  
    public void setAge(int age) {  
        this.age = age;  
    }  
  
    public MyDate getBirthday() {  
        return birthday;  
    }  
  
    public void setBirthday(MyDate birthday) {  
        this.birthday = birthday;  
    }  
  
    @Override  
    public String toString() {  
        return "Employee{name='" + name + "', age=" + age +  
                ", birthday=" + birthday.getYear() + "-" +  
                String.format("%02d", birthday.getMonth()) + "-" +  
                String.format("%02d", birthday.getDay()) + "}";  
    }  
  
    //按照name从低到高排序  
    @Override  
    public int compareTo(Employee o) {  
        return this.name.compareTo(o.name);  
    }  
}
```

然后再直接调用即可
```java
public class EmployeeTest {  
    @Test  
    public void test1() {  
        TreeSet<Employee> set = new TreeSet<>();  
        Employee e1 = new Employee("dudu",22,new MyDate(1998,12,21));  
        Employee e2 = new Employee("agou",21,new MyDate(1992,2,21));  
        Employee e3 = new Employee("amao",23,new MyDate(1999,4,21));  
        Employee e4 = new Employee("lihua",22,new MyDate(2008,1,28));  
        Employee e5 = new Employee("hanmeimei",32,new MyDate(1998,12,21));  
  
        set.add(e1);  
        set.add(e2);  
        set.add(e3);  
        set.add(e4);  
        set.add(e5);  
  
        Iterator<Employee> iterator = set.iterator();  
        while (iterator.hasNext()) {  
            Employee employee = iterator.next();  
            System.out.println(employee);  
        }  
    }
}
```

#### 2.按生日日期的前后顺序进行排序
```java
public class EmployeeTest {
    @Test  
    public void test2() {  
        //要比较的对象是什么，泛型类型写什么，而不是写要比较的属性  
        Comparator<Employee> comparator = new Comparator<Employee>(){  
            @Override  
            public int compare(Employee o1, Employee o2) {  
                int yeardistance = o1.getBirthday().getYear() - o2.getBirthday().getYear();  
                if (yeardistance != 0) {  
                    return yeardistance;  
                }  
                int monthdistance = o1.getBirthday().getMonth() - o2.getBirthday().getYear();  
                if (monthdistance != 0) {  
                    return monthdistance;  
                }  
                return  o1.getBirthday().getDay() - o2.getBirthday().getDay();  
            }  
        };  
        
        TreeSet<Employee> set = new TreeSet<>(comparator);  
        Employee e1 = new Employee("dudu",22,new MyDate(1998,12,21));  
        Employee e2 = new Employee("agou",21,new MyDate(1992,2,21));  
        Employee e3 = new Employee("amao",23,new MyDate(1999,4,21));  
        Employee e4 = new Employee("lihua",22,new MyDate(2008,1,28));  
        Employee e5 = new Employee("hanmeimei",32,new MyDate(1998,12,21));  
  
        set.add(e1);  
        set.add(e2);  
        set.add(e3);  
        set.add(e4);  
        set.add(e5);  
  
        Iterator<Employee> iterator = set.iterator();  
        while (iterator.hasNext()) {  
            Employee employee = iterator.next();  
            System.out.println(employee);  
        }  
    }  
}   
```
## 自定义泛型

### 自定义泛型类
```java
class 类名<T,R...> { //也可以是接口 ...表示可以有多个
    成员  
}
```

### 注意细节
- 普通成员可以使用泛型(属性、方法)
- 使用泛型的数组，不能初始化
- 静态方法中不能使用类的泛型
- 泛型类的类型，是在创建对象时确定的(因为创建对象时需要指定确定类型)
- 如果在创建对象时，没有指定类型，默认为Object

```java
//Tiger为自定义泛型类  
//T,R,M泛型的标识符，单个大写字母  
//泛型标识符可以有多个  
class Tiger<T,R,M> {  
    String name;  
    R r;  
    M m;  
    T t;  
    //T[] ts = new T[8];使用泛型的数组不能初始化，不能这样写，因为不确定类型不知道初始化数组多大的空间  
  
//    public static void m1(M m) { 静态方法不能使用泛型  
//    }  
//    static R r2; 静态属性也不能使用泛型，因为静态是和类相关的，在类加载时，对象还没有创建  
  
    public Tiger(String name, R r, M m, T t) {//普通成员可以使用泛型  
        this.name = name;  
        this.r = r;  
        this.m = m;  
        this.t = t;  
    }  
  
    public String getName() {//返回类型可以使用泛型  
        return name;  
    }  
  
    public void setName(String name) {//方法可以使用泛型  
        this.name = name;  
    }  
}
```
### 自定义泛型接口
```java
interface 接口名<T,R...> { //也可以是接口 ...表示可以有多个
    成员  
}
```
- 接口中，静态成员也不能使用泛型（这个和泛型类规定一样）
- 泛型接口的类型，在**继承接口或者实现接口**时确定
- 没有指定类型，默认为Object
```java
interface IUsb<U,R>{  
    int n =10;  
    // U name = "hsp";
    //错误，接口中的属性默认的修饰符是public final static,方法默认的修饰符是public abstract，静态成员不能使用泛型  
    R get(U u);  
    void hi(R r);  
    void run(R r1, R r2, U u1, U u2);  
  
    default R method(U u) {  
        return null;  
    }  
}  
  
interface IA extends IUsb<String,Double> {  
  
}  
//当我们去实现IA接口时，因为IA在继承IUsU接口时，指定了U为StringR为Double在实现IUsU接口的方法时，使用String替换U，是Double替换R  
class AA implements IA {  
  
    @Override  
    public Double get(String s) {  
        return 0.0;  
    }  
  
    @Override  
    public void hi(Double aDouble) {  
  
    }  
  
    @Override  
    public void run(Double r1, Double r2, String u1, String u2) {  
  
    }  
}  

//实现接口时，直接指定泛型接口的类型  
class BB implements IUsb<Integer,Float> {  
  
    @Override  
    public Float get(Integer integer) {  
        return 0f;  
    }  
  
    @Override  
    public void hi(Float aFloat) {  
  
    }  
  
    @Override  
    public void run(Float r1, Float r2, Integer u1, Integer u2) {  
  
    }  
}  
  
class CC implements IUsb {//没有指定类型，默认为Object,是可以，但是最好写出来  
  
}
```

### 自定义泛型方法

```java
修饰符<T,R..> 返回类型 方法名（参数列表）｛
｝
```

- 泛型方法，可以定义在普通类中，也可以定义在泛型类中
- 当泛型方法被调用时，类型会确定
- public void eat(E e){}， 修饰符后没有<T,R..>
    eat方法不是泛型方法，而是使用了泛型

```java
public class CustomMethodGeneric {  
    public static void main(String[] args) {  
        Car car = new Car();  
        car.fly("baoma",100);//当调用方法时，传入参数，编译器就会确定类型  
  
        //测试Fish类  
        Fish<String, ArrayList> fish = new Fish<>();  
        fish.hello(new ArrayList(),11.3f);  
    }  
}  
  
//泛型方法，可以定义在普通类中，也可以定义在泛型类中  
class Car {//普通类  
    public void run() {//普通方法  
    }  
    public<T,R> void fly(T t,R r){//泛型方法,<T,R>就是泛型是提供给fly使用的  
        System.out.println(t.getClass());  
        System.out.println(r.getClass());  
    }  
}  
class Fish<T,R> {//泛型类  
    public void run() {//普通方法  
    }  
    public<U,M> void eat(U u,M m) {//泛型方法  
        //这里规定泛型的标识符最好和泛型类的标识符分开  
        //如果前面规定了泛型，传参最好传入  
    }  
  
    public void hi(T t) {//这里不是泛型方法，是hi方法使用了类声明的泛型  
    }  
  
    public<K> void hello(R r,K k){  
        System.out.println(r.getClass());  
        System.out.println(k.getClass());  
    }  
}
```

练习
```java
public class CustomMethodGenericExercise {  
    public static void main(String[] args) {  
        Apple<String,Integer,Double> apple = new Apple<>();  
        apple.fly(10);//10会被自动装箱  
        apple.fly(new Dog());//Dog  
    }  
}  
  
class Apple<T,R,M> {  
    public<E> void fly(E e){  
        System.out.println(e.getClass().getSimpleName());  
    }  
    //public void eat(U u){} 错的，u没有在前面声明  
    public void run(M m){}  
}  
  
class Dog{}
```

## 泛型继承和通配符

- 泛型不具备继承性`List<Object> list = new ArrayList<String>` 这样是不可以的
- `<?>`支持任意泛型类型
- `<? extends A>`支持A类及A类的子类，规定了泛型的上限
- `<? super A>` 支持A类及A类的父类，不限于直接父类，规定了泛型的下限
```java
public class GenericExtends {  
    public static void main(String[] args) {  
        List<Object> list1 = new ArrayList<>();  
        List<String> list2 = new ArrayList<>();  
        List<AAA> list3 = new ArrayList<>();  
        List<BBB> list4 = new ArrayList<>();  
        List<CCC> list5 = new ArrayList<>();  
    }  
  
    public static void printCollection1(List<?> c) {//表示任意的泛型类型都可以接受  
        for (Object object : c) {  
            System.out.println(object);  
        }  
    }  
    public static void printCollection2(List<? extends AAA> c) {//可以接受AAA和AAA的子类  
        for (Object object : c) {  
            System.out.println(object);  
        }  
    }  
    public static void printCollection3(List<? super AAA> c) {//表示AAA和其父类  
        for (Object object : c) {  
            System.out.println(object);  
        }  
    }  
}  
  
class AAA {  
}  
class BBB extends AAA{}  
  
class CCC extends BBB{}
```

# lambda表达式

## 语法
Lambda 表达式：在Java 8 语言中引入的一种新的语法元素和操作符。这个操作符为 “`->`” ， 该操作符被称为 `Lambda 操作符`或`箭头操作符`。它将 Lambda 分为两个部分：

- 左侧：形参列表，对应这要重写的接口中的抽象方法的形参列表。
- 右侧：方法体，对应着接口的实现类要重写的方法的方法体。

### 语法格式一：无参，无返回值

```java
@Test
public void test1(){
    //未使用Lambda表达式
    Runnable r1 = new Runnable() {
        @Override
        public void run() {
            System.out.println("我爱北京天安门");
        }
    };

    r1.run();

    System.out.println("***********************");

    //使用Lambda表达式
    Runnable r2 = () -> {
        System.out.println("我爱北京故宫");
    };

    r2.run();
}
```

### 语法二：Lambda 需要一个参数，但是没有返回值。
```java
@Test
public void test2(){
    //未使用Lambda表达式
    Consumer<String> con = new Consumer<String>() {
        @Override
        public void accept(String s) {
            System.out.println(s);
        }
    };
    
    con.accept("谎言和誓言的区别是什么？");

    System.out.println("*******************");

    //使用Lambda表达式
    Consumer<String> con1 = (String s) -> {
        System.out.println(s);
    };
    con1.accept("一个是听得人当真了，一个是说的人当真了");

}
```

### 语法格式三：**数据类型可以省略，因为可由编译器推断得出，称为“类型推断”
```java
@Test
public void test3(){
    //语法格式三使用前
    Consumer<String> con1 = (String s) -> {
        System.out.println(s);
    };
    con1.accept("一个是听得人当真了，一个是说的人当真了");

    System.out.println("*******************");
    //语法格式三使用后
    Consumer<String> con2 = (s) -> {
        System.out.println(s);
    };
    con2.accept("一个是听得人当真了，一个是说的人当真了");

}
```

### 语法格式四：Lambda 若只需要一个参数时，参数的小括号可以省略

```java
@Test
public void test4(){
    //语法格式四使用前
    Consumer<String> con1 = (s) -> {
        System.out.println(s);
    };
    con1.accept("一个是听得人当真了，一个是说的人当真了");

    System.out.println("*******************");
    //语法格式四使用后
    Consumer<String> con2 = s -> {
        System.out.println(s);
    };
    con2.accept("一个是听得人当真了，一个是说的人当真了");
}
```

### 语法格式五：Lambda 需要两个或以上的参数，多条执行语句，并且可以有返回值 Comparator
```java
@Test
public void test5(){
    //语法格式五使用前
    Comparator<Integer> com1 = new Comparator<Integer>() {
        @Override
        public int compare(Integer o1, Integer o2) {
            System.out.println(o1);
            System.out.println(o2);
            return o1.compareTo(o2);
        }
    };

    System.out.println(com1.compare(12,21));
    System.out.println("*****************************");
    //语法格式五使用后
    Comparator<Integer> com2 = (o1,o2) -> {
        System.out.println(o1);
        System.out.println(o2);
        return o1.compareTo(o2);
    };

    System.out.println(com2.compare(12,6));
}
```

### 语法格式六：当 Lambda 体只有一条语句时，return 与大括号若有，都可以省略
```java
@Test
public void test6(){
    //语法格式六使用前
    Comparator<Integer> com1 = (o1,o2) -> {
        return o1.compareTo(o2);
    };

    System.out.println(com1.compare(12,6));

    System.out.println("*****************************");
    //语法格式六使用后
    Comparator<Integer> com2 = (o1,o2) -> o1.compareTo(o2);

    System.out.println(com2.compare(12,21));

}

@Test
public void test7(){
    //语法格式六使用前
    Consumer<String> con1 = s -> {
        System.out.println(s);
    };
    con1.accept("一个是听得人当真了，一个是说的人当真了");

    System.out.println("*****************************");
    //语法格式六使用后
    Consumer<String> con2 = s -> System.out.println(s);

    con2.accept("一个是听得人当真了，一个是说的人当真了");

}
```

## 语法规则总结

- 的左边:lambda形参列表，参数的类型都可以省略。如果形参只有一个，则一对()也可以省略。
- 的右边:lambda体，对应着重写的方法的方法体。如果方法体中只有一行执行语句，则一对{}可以省略。
- 如果有return关键字，则必须一并省略。


