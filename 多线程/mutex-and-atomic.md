# Mutex & Atomic

### mutex

mutex 是用于保护共享数据免受多个线程同时访问的同步原语。

```cpp
std::mutex mutex;

mutex.lock();

// do something else
// but race condition

mutex.unlock();
```

但是我们一般不用 mutex 对 race condition 上锁，而是用 std::unique\_lock，std::lock\_guard 这些更安全的方式。

#### lock\_guard

std::lock\_guard 是接受一个 mutex 的类（RAII），在获取资源时既初始化。 在作用域中创建 lock\_guard 并获取 mutex，在离开作用域时销毁 lock\_guard，释放锁。

```cpp
std::mutex mutex;
{
    std::lock_guard<std::mutex> lg(mutex);
}
```

#### unique\_lock

unique\_lock 和 lock\_guard 十分相似，只不过前者可以作为函数返回值，扩展其作用域。

```cpp
std::mutex mutex;
{
    std:unique_lock<std::mutex> ul(mutex);
    ul.unlock();
    ul.lock();
}

std::unique_lock<std::mutex> f() {
    std::unique_lock<std::mutex> ul(mutex);
    return ul;
}
```

### atomic

std::atomic 是一个模板类，创建一个原子对象。可以免受多线程访问时的 race condition。

```cpp
std::atomic<int> gv = 0;
```
