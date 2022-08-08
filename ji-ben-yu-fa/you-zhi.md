# 右值

本篇旨在把 C++ 的右值的特性搞清楚。

#### 右值

**&&**，第一次见这个符号是在 cs144 上，其实这是右值引用的符号。当函数声明时，想传入一个右值，需要用到这个符号。

```cpp
//! TCP sender payloader
//! \brief Construct by taking ownership of a string
Buffer(std::string &&str) noexcept : _storage(std::make_shared<std::string>(std::move(str))) {}
```