# Type

在C#中，如果你有一个基类对象引用指向一个子类对象，当你调用这个基类对象的`GetType()`方法时，它将返回子类的类型。这是因为`GetType()`方法返回的是实际对象的**运行时类型**，而不是引用的类型。

```csharp
public class BaseClass
{
    // 基类代码
}

public class SubClass : BaseClass
{
    // 子类代码
}

public class Program
{
    public static void Main()
    {
        BaseClass baseObj = new SubClass();
        Type type = baseObj.GetType();
        Console.WriteLine(type); // 输出: SubClass
    }
}
```

运行这段代码，输出将是SubClass