# Condition\_variable & Semaphore

### condition\_variable

condition\_variable 能够阻塞一个线程，直至另一个线程修改共享变量并通知 condition\_variable。

```cpp
#include <iostream>
#include <thread>
#include <mutex>
#include <condition_variable>
 
std::mutex mutex;
std::condition_variable cv;
bool flag = true;

void f() {
    std::unique_lock<std::mutex> ul(mutex);
    while (flag) {
        std::cout << "I wait falg" << std::endl;
        // 线程休眠，释放锁。当它醒过来之后会获取锁。
        cv.wait(ul);
    }
    std::cout << "I wake up" << std::endl;
}

int main() {
    std::thread t(f);

    std::this_thread::sleep_for(std::chrono::milliseconds(30));

    int n = 10;
    while (n --) {
        std::cout << "---" << std::endl;
    }
    flag = false;
    // 唤醒随机一个沉睡线程
    cv.notify_one();
    t.join();
}
```

### semaphore
