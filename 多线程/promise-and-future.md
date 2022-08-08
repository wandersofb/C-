# Promise & Future

std::promise 是一种提供存储值或者异常的设施，一般在初始化时，不知道这个值是多少，之后通过 std::promise 对象所创建的 std::future 对象异步获取结果。

当子线程的返回值，主线程需要的等待时。

```cpp
void task(std::promise<int> &ret) {
    // 2. 设置值  
    ret.set_value(2);
}
// 1. 建立联系
std::promise<int> p;
std::future<int> f = p.get_future();

std::thread t(task, std::ref(p));

// 3. 获取值
std::cout << f.get();
```

当子线程的参数需要主线程获取，而子线程需要等待。

```cpp
void task(int a, std::future<int> &b) {
    // 2. get value only once
    a = a + b.get();
}

std::promise<int> p_in;
std::future<int> f_in = p_in.get_future();

// thread begin
std::thread t(task, 1, f_in);
// do something else

// 1. set value
p_in.set_value(3);
```

当多个线程的参数要从主线程中获取

```cpp
void task(int a, std::shared_future<int> b) {
    // 2. get value more times
    a = a + b.get();    
}

std::promise<int> p_in;
std::shared_future<int> s_in = p_in.get_future();

std::thread t(task, 1, s_in);
std::thread t(task, 1, s_in);
std::thread t(task, 1, s_in);
std::thread t(task, 1, s_in);

// 1. set value
p_in.set_value(3);
```
