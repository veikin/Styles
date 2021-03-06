#### 头文件

##### 多重包含

使用 `#define` 进行保护，宏命名参考 `PROJECT_PATH_FILE_H_`

例如文件：foo/src/bar/baz.h

```cpp
#ifdef FOO_BAR_BAZ_H_
#define FOO_BAR_BAZ_H_

// 声明部分

#endif // FOO_BAR_BAZ_H_
```

> 笔者推荐命名方式为 `_PROJECT_MODLUE_FILE_H_`，注释方式也略有不同

```cpp
#ifdef _FOO_BAR_BAZ_H_
#define _FOO_BAR_BAZ_H_

// 声明部分

#endif /* _FOO_BAR_BAZ_H_ */
```

##### 头文件依赖

前置声明：降低编译依赖，防止修改导致多米诺效应，能依赖声明的就不要依赖定义。

+ 参数、返回值类型使用 `Foo` 的方法只声明不定义实现
+ 将成员变量声明成 `Foo*` 或 `Foo&` 类型，静态成员在类定义之外，不需要这样做
+ 不降低代码可读性和效率有选择性使用指针成员代替对象成员

举个栗子：

```cpp
class Foo;

class Bar {
 private:
    Foo* foo_;
};
```

##### 内联函数

合理使用可提高代码执行效率，比如函数只有 10 行甚至更少的时候。

##### *-inl.h

将复杂的内联函数的定义放在这个文件中，提高代码可读性。

##### 函数参数顺序

输入参数在前，输出参数在后

> 对函数参数的堆栈空间有轻微影响，笔者习惯按函数内部使用顺序排序

##### 包含文件的名称及次序

+ 使用完整的项目路径，看上去会很清晰，很条理
+ 次序：源文件的头文件，C 系统头文件，C++ 系统头文件，其他库头文件，内部头文件

减少隐藏依赖，使每个头文件在 “最需要编译” 的地方编译

