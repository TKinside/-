# C++

## 标准知识

### cstdint

 >cstdint：C++ 标准库中的一个头文件，它提供了固定宽度的整数类型以及相应的类型别名

这些类型保证了在所有平台上具有相同的宽度，因此非常适合跨平台编程和需要精确控制整数宽度的场景。

以下是一些常用的固定宽度整数类型和类型别名：

- `int8_t`、`int16_t`、`int32_t`、`int64_t`：分别表示有符号的 8 位、16 位、32 位和 64 位整数。
- `uint8_t`、`uint16_t`、`uint32_t`、`uint64_t`：分别表示无符号的 8 位、16 位、32 位和 64 位整数。

这些类型的定义确保了它们在不同平台上的一致性，因此在需要明确整数位宽的情况下非常有用。

**要特别注意不同长度的同类型转换，比如将int的变量赋值给short，会导致缩窄转换错误**

可使用列表初始化来解决，即使用大括号 **{}** 进行初始化，C++11引入了该机制防止**缩窄转换错误**

### 函数指针 ***

> 函数指针：指向函数的指针，允许我们动态地调用函数、将函数作为参数传递给其他函数、以及在运行时决定调用哪个函数。函数指针的类型由它指向的函数的返回类型和参数类型决定。

**语法：`return_type (*pointer_name)(parameter_types)`**

- 例如：定义一个指向返回类型为 `int`，参数类型为 `double` 的函数的指针。

```
int (*func_ptr)(double);
```

**定义一个函数**：

```
int myFunction(double d) {
    return static_cast<int>(d);
}
```

**将函数地址赋值给函数指针**：

```
func_ptr = myFunction;
```

**通过函数指针调用函数**：

```C++
int result = func_ptr(3.14); // 调用 myFunction(3.14)
```

**函数指针可以更加复杂，支持不同的返回类型和多个参数类型;函数指针可以作为函数的参数和返回值，以实现更高级的功能。**

**函数指针作为参数**

```
#include <iostream>

int add(int a, int b) {
    return a + b;
}

int operate(int a, int b, int (*operation)(int, int)) {
    return operation(a, b);
}

int main() {
    int result = operate(5, 3, add); // 调用 add(5, 3)
    std::cout << "Result: " << result << std::endl; // 输出 Result: 8

    return 0;
}
```

**函数指针作为返回值**

**语法：return_type (*function_name(parameter_types))(parameter_types)  **

```
#include <iostream>

int add(int a, int b) {
    return a + b;
}

int subtract(int a, int b) {
    return a - b;
}

int (*getOperation(char op))(int, int) {
    if (op == '+')
        return add;
    else
        return subtract;
}

int main() {
    int (*operation)(int, int);

    operation = getOperation('+');
    std::cout << "Add: " << operation(5, 3) << std::endl; // 输出 Add: 8

    operation = getOperation('-');
    std::cout << "Subtract: " << operation(5, 3) << std::endl; // 输出 Subtract: 2

    return 0;
}
```

### using

>using：最常见的用途是用于定义类型别名(此条是为了简化函数指针的使用，原语法过于地狱)

`using` 关键字可以简化函数指针的定义，使代码更具可读性。

~~~c++
#include <iostream>
// 使用 using 定义函数指针类型
using OperationFunc = int(*)(int, int);
int add(int a, int b) {
    return a + b;
}
int subtract(int a, int b) {
    return a - b;
}
//返回值为函数指针的函数
OperationFunc getOperation(char op) {
    if (op == '+')
        return add;
    else
        return subtract;
}
int main() {
    OperationFunc operation;//创建函数指针变量
    operation = getOperation('+');//调用函数
    std::cout << "Add: " << operation(5, 3) << std::endl; // 输出 Add: 8
    operation = getOperation('-');//调用函数
    std::cout << "Subtract: " << operation(5, 3) << std::endl; // 输出 Subtract: 2
    return 0;
}

~~~

## 泛型编程

### auto与decltype

>使用auto自动推断类型：允许编译器根据初始化表达式自动推断变量的类型，从而减少代码的冗长和复杂性

**`auto` 关键字非常有用，特别是在处理复杂类型时，例如迭代器、lambda 表达式或模板类型。**

1. **简化代码**： 使用 `auto` 可以简化代码，避免手动指定冗长的类型。
2. **提高可维护性**： 当类型发生变化时，只需要修改初始化表达式，而不需要修改变量声明的类型。
3. **使用复杂类型**： `auto` 特别适用于那些类型非常复杂的变量，如迭代器和函数返回类型。**(!!!)**
4. **与decltype结合使用**：`decltype` 是 C++11 标准中引入的一个关键字，用于查询表达式的类型。它可以获取变量或表达式的类型，并以此类型定义新的变量。这在编写模板代码和需要类型推断的情况下非常有用。

**使用括号括起变量可强调其为左值，以推断其类型为其的引用**

> 