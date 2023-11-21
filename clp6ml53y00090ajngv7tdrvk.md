---
title: "[Java Concurrency] Thread safety"
datePublished: Mon Nov 20 2023 08:08:59 GMT+0000 (Coordinated Universal Time)
cuid: clp6ml53y00090ajngv7tdrvk
slug: java-concurrency-thread-safety
tags: java, concurrency

---

# Thread safety

Cỗ máy chính để đồng bộ hóa trong Java là từ khóa `synchronized`. Nó cung cấp exclusive locking. Ngoài ra thuật ngữ đồng bộ hóa còn bao gồm việc sử dụng:

* biến `volatile`
    
* explicit locks
    
* biến atomic
    

Có 3 cách để đồng bộ hóa:

* không chia sẻ biến trạng thái giữa các luồng
    
* làm cho biến trạng thái immutable
    
* sử dụng đồng bộ hóa bất cứ khi nào truy cập vào biến trạng thái
    

# Race condition

Điều kiện tương tranh xảy ra khi tính đúng đắn của tính toán phụ thuộc vào thời gian tương đối hoặc sự đan xen giữa nhiều luồng trong thời gian chạy.

# Intrinsic locks (Monitor locks)

Một synchronized block bao gồm 2 phần:

* 1 tham chiếu tới đối tượng mà đóng vai trò là lock
    
* 1 block code cần được bảo vệ bởi lock đó
    

```java
synchronized (lock) {
    // Access or modify shared state guarded by lock
}
```

synchronized method là viết tắt của synchronized block mà:

* bao trùm toàn bộ phần thân phương thức.
    
* lock là
    
    * với non-static method: đối tượng mà phương thức đang được gọi
        
    * với static method: đối tượng Class làm lock
        

Intrinsic locks trong Java hành xử như mutexes: nhiều nhất một luồng có thể sở hữu lock.

# Reentrancy

Reentrancy: lock được cấp bởi mỗi luồng, không phải là mỗi lần gọi.

Reentrancy được implement bằng cách liên kết mỗi lock với

* một biến count
    
* luồng sở hữu
    

# Liveness

Tránh giữ khóa trong quá trình tính toán kéo dài hoặc các thao tác có nguy cơ không hoàn thành nhanh chóng như network I/O...

```java
@ThreadSafe
public class CachedFactorizer implements Servlet {
    @GuardedBy("this") private BigInteger lastNumber;
    @GuardedBy("this") private BigInteger[] lastFactors;
    @GuardedBy("this") private long hits;
    @GuardedBy("this") private long cacheHits;
    public synchronized long getHits() {
        return hits;
    }
    public synchronized double getCacheHitRatio() {
        return (double) cacheHits / (double) hits;
    }
    public void service(ServletRequest req, ServletResponse resp) {
        BigInteger i = extractFromRequest(req);
        BigInteger[] factors = null;
        synchronized(this) {
            ++hits;
            if (i.equals(lastNumber)) {
                ++cacheHits;
                factors = lastFactors.clone();
            }
        }
        if (factors == null) {
            factors = factor(i);
            synchronized(this) {
                lastNumber = i;
                lastFactors = factors.clone();
            }
        }
        encodeIntoResponse(resp, factors);
    }
}
```