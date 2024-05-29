`noncopyable`类是一种设计模式，它的目的是明确地禁止对象的拷贝构造和赋值。这在C++中通常通过私有化拷贝构造函数和赋值运算符来实现。通过继承一个设计为`noncopyable`的基类，你可以避免在派生类中重复编写禁止拷贝和赋值的代码。

`noncopyable`类的作用有以下几点：

- **防止对象拷贝**：有些对象不应该被拷贝，例如因为它们代表着对资源的唯一拥有或管理，如文件句柄、线程和锁。
- **避免意外的副本**：阻止对象拷贝可以避免因隐式拷贝构造导致的性能问题或竞争条件。
- **保护资源所有权**：对于管理独占资源的类，拷贝可能导致资源泄露、双重释放或难以追踪的错误。

例如，`boost`库中有一个`noncopyable`实用类，它看起来是这样的：

```cpp
namespace boost {
    class noncopyable {
    protected:
        noncopyable() = default;
        ~noncopyable() = default;

        noncopyable(const noncopyable&) = delete;
        noncopyable& operator=(const noncopyable&) = delete;
    };
}
```

当你的类继承了`noncopyable`：

```cpp
class MyClass : private boost::noncopyable {
    // ... class definition ...
};
```

尝试拷贝`MyClass`的对象，如下：

```cpp
MyClass a;
MyClass b = a; // 错误：拷贝构造函数已被删除
MyClass c;
c = a; // 错误：赋值运算符已被删除
```

上述操作都会导致编译错误，因为拷贝构造函数和赋值运算符被声明为`delete`，即它们无法被访问。

在C++11及以后的标准中，明确删除函数是推荐的方式来创建一个`noncopyable`类或对象。在以前的C++标准中，要实现这一目的，通常把这两个函数声明为私有，并且不提供实现。这样，如果试图拷贝类的实例，会得到编译错误。C++11的`= delete`语法更清晰地表达了意图，并对于阅读代码的其他人来说更直观。
