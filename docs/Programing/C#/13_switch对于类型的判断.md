# Switch对于类型的判断

判断一个对象是否是某个类型，可以使用`is`关键字，会考虑到对象的继承，当`子类  is  父类` 时，返回的也是`true`，使用`==`时，则会严格判断类型是否一致，即使继承关系也会返回`false`

### switch关于类型判断的用法:

1、 只关心类型（使用了弃元，应该也是考虑到了继承来着）

```csharp
public class Animal { }
public class Dog : Animal { }
public class Cat : Animal { }

switch (animal)
        {
            case null:
                Console.WriteLine("动物为 null");
                break;
            case Dog _:  // 匹配 Dog 类型（忽略实例本身，只关心类型）
                Console.WriteLine("这是一只狗");
                break;
            case Cat _:  // 匹配 Cat 类型
                Console.WriteLine("这是一只猫");
                break;
            default:
                Console.WriteLine("未知动物类型");
                break;
        }

```

> _：是一个弃元（discard），表示 “我只关心类型是否匹配，不需要使用这个实例的值”。

2、 不考虑继承

```csharp
 switch (obj.GetType())
    {
        case Type t when t == typeof(int):
            Console.WriteLine("这是 int 类型");
            break;
        case Type t when t == typeof(string):
            Console.WriteLine("这是 string 类型");
            break;
        case Type t when t == typeof(Dog):
            Console.WriteLine("这是 Dog 类型");
            break;
        default:
            Console.WriteLine("未知类型");
            break;
    }
```

> `when`后面是判断条件，可以用`==`要求是严格的类型，即不考虑继承；也可以使用`is`考虑到具体的类型


3、 结合变量使用（需要访问实例成员时）

```csharp
switch (obj)
{
    case int num:
        Console.WriteLine($"int 值为：{num}"); // 直接使用 num 变量
        break;
    case string str:
        Console.WriteLine($"string 长度：{str.Length}"); // 访问字符串长度
        break;
    case Dog dog:
        Console.WriteLine($"狗的名字：{dog.Name}"); // 访问 Dog 类的属性
        break;
    default:
        Console.WriteLine("未知类型");
        break;
}
```

4、 C# 8.0+ 开关表达式（Switch Expression，更简洁）

```csharp
string result = obj switch
{
    int _ => "这是 int 类型",
    string _ => "这是 string 类型",
    Dog _ => "这是 Dog 类型",
    _ => "未知类型" // _ 表示默认情况
};
Console.WriteLine(result);
```

> `switch`表达式返回一个值，因此需要使用变量来接收返回值。

**特殊用法**

*匹配类型时可直接声明变量，用于访问实例的属性或方法：*

```csharp
public class Person { public int Age; }

// 根据年龄范围返回描述
string GetAgeGroup(Person person) => person switch
{
    { age: < 18 } => "未成年人",  // 属性模式：年龄小于18
    { age: >= 18 and <= 60 } => "成年人",  // 范围和逻辑运算
    { age: > 60 } => "老年人",
    null => "无此人",
    _ => "年龄未知"
};
```

*多条件组合（使用 and/or 逻辑）*

```csharp
// 判断数字是否在指定范围
string CheckNumber(int num) => num switch
{
    < 0 => "负数",
    0 => "零",
    > 0 and <= 100 => "正数（1-100）",
    > 100 => "正数（>100）",
    _ => "无效数字"
};
```

*元组模式（多值匹配）*

```csharp
// 根据坐标判断象限
string GetQuadrant((int x, int y) point) => point switch
{
    ( > 0, > 0 ) => "第一象限",
    ( < 0, > 0 ) => "第二象限",
    ( < 0, < 0 ) => "第三象限",
    ( > 0, < 0 ) => "第四象限",
    ( 0, 0 ) => "原点",
    ( 0, _ ) => "Y轴",
    ( _, 0 ) => "X轴",
    _ => "未知"
};
```

**不想要返回值**

```csharp
object obj = "test";

// 开关表达式返回 object 类型，用 _ 接收（忽略结果）
_ = obj switch
{
    int num => Console.WriteLine($"整数：{num}"),
    string str => Console.WriteLine($"字符串：{str}"),
    _ => Console.WriteLine("未知类型")
};
```

5、  结合 nameof 动态获取类型名（通用写法）

```csharp
switch (obj)
{
    case int _:
        Console.WriteLine($"这是 {nameof(Int32)} 类型"); // int 实际是 Int32 的别名
        break;
    case string _:
        Console.WriteLine($"这是 {nameof(String)} 类型");
        break;
    case Dog _:
        Console.WriteLine($"这是 {nameof(Dog)} 类型");
        break;
    default:
        Console.WriteLine($"未知类型：{obj?.GetType().Name ?? "null"}");
        break;
}
```