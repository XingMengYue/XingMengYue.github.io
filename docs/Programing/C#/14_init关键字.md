# init关键字

在 C# 9.0 中引入的 init 关键字用于修饰属性的 setter（初始化器），它的核心作用是允许属性在对象初始化时被赋值，但初始化后不可修改，平衡了对象的可初始化性和不可变性。

> init 修饰的属性只能在对象创建时（通过对象初始化器、构造函数）赋值，对象创建完成后无法再修改，类似 “只读但可在初始化时灵活设置” 的效果。
> 
```csharp
public class Person
{
    // 用 init 修饰 setter
    public string Name { get; init; }
    public int Age { get; init; }
}

// 使用示例
var person = new Person
{
    Name = "张三",  // 允许：初始化时赋值
    Age = 25
};

// 错误：对象创建后无法修改 init 属性
// person.Name = "李四";  // 编译时报错
```

- `readonly` 字段只能在声明时或构造函数中赋值，无法通过对象初始化器设置。
- `init` 属性可通过对象初始化器赋值，且保持了属性的封装性（字段通常是私有或内部的）。