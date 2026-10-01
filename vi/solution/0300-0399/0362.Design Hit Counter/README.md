---
comments: true
difficulty: Medium
tags:
    - Design
    - Queue
    - Array
    - Binary Search
    - Data Stream
---

<!-- problem:start -->

# [362. Design Hit Counter 🔒](https://leetcode.com/problems/design-hit-counter)

[中文文档](/solution/0300-0399/0362.Design%20Hit%20Counter/README.md)

## Mô tả

<!-- description:start -->

<p>Hãy thiết kế bộ đếm hit để đếm số hit nhận được trong <code>5</code> phút gần nhất (tức <code>300</code> giây).</p>

<p>Hệ thống nhận tham số <code>timestamp</code> (độ chính xác đến <strong>giây</strong>). Có thể giả sử các lời gọi đến hệ thống theo thứ tự thời gian, tức <code>timestamp</code> tăng đơn điệu. Nhiều hit có thể đến gần như cùng lúc.</p>

<p>Hãy cài đặt class <code>HitCounter</code>:</p>

<ul>
	<li><code>HitCounter()</code> Khởi tạo object của hệ thống đếm hit.</li>
	<li><code>void hit(int timestamp)</code> Ghi nhận một hit xảy ra tại <code>timestamp</code> (đơn vị <strong>giây</strong>). Nhiều hit có thể xảy ra tại cùng một <code>timestamp</code>.</li>
	<li><code>int getHits(int timestamp)</code> Trả về số hit trong 5 phút trước thời điểm <code>timestamp</code> (tức <code>300</code> giây gần nhất).</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;HitCounter&quot;, &quot;hit&quot;, &quot;hit&quot;, &quot;hit&quot;, &quot;getHits&quot;, &quot;hit&quot;, &quot;getHits&quot;, &quot;getHits&quot;]
[[], [1], [2], [3], [4], [300], [300], [301]]
<strong>Đầu ra</strong>
[null, null, null, null, 3, null, 4, 3]

<strong>Giải thích</strong>
HitCounter hitCounter = new HitCounter();
hitCounter.hit(1);       // hit at timestamp 1.
hitCounter.hit(2);       // hit at timestamp 2.
hitCounter.hit(3);       // hit at timestamp 3.
hitCounter.getHits(4);   // get hits at timestamp 4, return 3.
hitCounter.hit(300);     // hit at timestamp 300.
hitCounter.getHits(300); // get hits at timestamp 300, return 4.
hitCounter.getHits(301); // get hits at timestamp 301, return 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= timestamp &lt;= 2 * 10<sup>9</sup></code></li>
	<li>Tất cả lời gọi đến hệ thống đều theo thứ tự thời gian (tức <code>timestamp</code> tăng đơn điệu).</li>
	<li>Có nhiều nhất <code>300</code> lời gọi đến <code>hit</code> và <code>getHits</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Nếu số hit mỗi giây có thể rất lớn thì sao? Thiết kế của bạn có mở rộng tốt không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Đếm số hit trong $300$ giây gần nhất. Timestamp tăng đơn điệu. Có thể dùng queue để loại bỏ các hit đã cũ, hoặc dùng một list kết hợp tìm kiếm nhị phân cho lời giải ngắn gọn hơn.
>
> `hit` thêm timestamp vào cuối; `getHits` tìm chỉ số đầu tiên thỏa $\ge timestamp-299$ bằng tìm kiếm nhị phân rồi trả về số phần tử từ đó đến cuối list.

<!-- thinking:end -->

Vì `timestamp` tăng đơn điệu, ta có thể dùng mảng `ts` để lưu tất cả `timestamp`. Trong method `getHits`, dùng tìm kiếm nhị phân để tìm vị trí đầu tiên có giá trị lớn hơn hoặc bằng `timestamp - 300 + 1`, rồi trả về độ dài của `ts` trừ đi vị trí đó.

Về độ phức tạp thời gian, method `hit` mất $O(1)$, còn method `getHits` mất $O(\log n)$, trong đó $n$ là độ dài của `ts`.

<!-- tabs:start -->

#### Python3

```python
class HitCounter:

    def __init__(self):
        self.ts = []

    def hit(self, timestamp: int) -> None:
        self.ts.append(timestamp)

    def getHits(self, timestamp: int) -> int:
        return len(self.ts) - bisect_left(self.ts, timestamp - 300 + 1)


# Your HitCounter object will be instantiated and called as such:
# obj = HitCounter()
# obj.hit(timestamp)
# param_2 = obj.getHits(timestamp)
```

#### Java

```java
class HitCounter {
    private List<Integer> ts = new ArrayList<>();

    public HitCounter() {
    }

    public void hit(int timestamp) {
        ts.add(timestamp);
    }

    public int getHits(int timestamp) {
        int l = search(timestamp - 300 + 1);
        return ts.size() - l;
    }

    private int search(int x) {
        int l = 0, r = ts.size();
        while (l < r) {
            int mid = (l + r) >> 1;
            if (ts.get(mid) >= x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
}

/**
 * Your HitCounter object will be instantiated and called as such:
 * HitCounter obj = new HitCounter();
 * obj.hit(timestamp);
 * int param_2 = obj.getHits(timestamp);
 */
```

#### C++

```cpp
class HitCounter {
public:
    HitCounter() {

    }

    void hit(int timestamp) {
        ts.push_back(timestamp);
    }

    int getHits(int timestamp) {
        return ts.end() - lower_bound(ts.begin(), ts.end(), timestamp - 300 + 1);
    }

private:
    vector<int> ts;
};

/**
 * Your HitCounter object will be instantiated and called as such:
 * HitCounter* obj = new HitCounter();
 * obj->hit(timestamp);
 * int param_2 = obj->getHits(timestamp);
 */
```

#### Go

```go
type HitCounter struct {
	ts []int
}

func Constructor() HitCounter {
	return HitCounter{}
}

func (this *HitCounter) Hit(timestamp int) {
	this.ts = append(this.ts, timestamp)
}

func (this *HitCounter) GetHits(timestamp int) int {
	return len(this.ts) - sort.SearchInts(this.ts, timestamp-300+1)
}

/**
 * Your HitCounter object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Hit(timestamp);
 * param_2 := obj.GetHits(timestamp);
 */
```

#### TypeScript

```ts
class HitCounter {
    private ts: number[] = [];

    constructor() {}

    hit(timestamp: number): void {
        this.ts.push(timestamp);
    }

    getHits(timestamp: number): number {
        const search = (x: number) => {
            let [l, r] = [0, this.ts.length];
            while (l < r) {
                const mid = (l + r) >> 1;
                if (this.ts[mid] >= x) {
                    r = mid;
                } else {
                    l = mid + 1;
                }
            }
            return l;
        };
        return this.ts.length - search(timestamp - 300 + 1);
    }
}

/**
 * Your HitCounter object will be instantiated and called as such:
 * var obj = new HitCounter()
 * obj.hit(timestamp)
 * var param_2 = obj.getHits(timestamp)
 */
```

#### Rust

```rust
struct HitCounter {
    ts: Vec<i32>,
}

/**
 * `&self` means the method takes an immutable reference.
 * If you need a mutable reference, change it to `&mut self` instead.
 */
impl HitCounter {
    fn new() -> Self {
        HitCounter { ts: Vec::new() }
    }

    fn hit(&mut self, timestamp: i32) {
        self.ts.push(timestamp);
    }

    fn get_hits(&self, timestamp: i32) -> i32 {
        let l = self.search(timestamp - 300 + 1);
        (self.ts.len() - l) as i32
    }

    fn search(&self, x: i32) -> usize {
        let (mut l, mut r) = (0, self.ts.len());
        while l < r {
            let mid = (l + r) / 2;
            if self.ts[mid] >= x {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        l
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
