# Thread

线程在 C++11 之后被引入到 C++ 语言中，包含在 <thread> 中。

线程创建

```cpp
std::thread t1; // t1 不是线程
std::thread t2(f1, n + 1); // 按值传递
std::thread t3(f2, std::ref(n)); // 按引用传递
std::thread t4(std::move(t3)); // t4 现在运行 f2() 。 t3 不再是线程
```

在多线程时，如果主线程先结束，子线程还未结束，这是就会报错，所以一般主线程会等待子线程的结束。

```cpp
std::thread t;
t.join(); // 在当前线程阻塞，直到 this 线程回收
```

如果主线程不想等待子线程，可以将子线程分离，各做各的，但是当主线程结束时不会去回收子线程。

```cpp
std::thread t;
t.detach(); // 允许线程从线程句柄中独立开
```

线程库还提供了很多方便的函数。

```cpp
std::this_thread::get_id(); // 得到此线程的 ID
std::this_thread::sleep_for(std::chrono::milliseconds(30)); // 使此线程睡眠
```

