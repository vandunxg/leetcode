---
comments: true
difficulty: Easy
rating: 1276
source: Weekly Contest 331 Q1
tags:
    - Array
    - Simulation
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2558. Take Gifts From the Richest Pile](https://leetcode.com/problems/take-gifts-from-the-richest-pile)

[中文文档](/solution/2500-2599/2558.Take%20Gifts%20From%20the%20Richest%20Pile/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>gifts</code> biểu thị số lượng quà trong các đống quà khác nhau. Mỗi giây, bạn thực hiện các bước sau:</p>

<ul>
	<li>Chọn đống có số lượng quà lớn nhất.</li>
	<li>Nếu có nhiều đống cùng có số lượng quà lớn nhất, chọn bất kỳ đống nào.</li>
	<li>Giảm số lượng quà trong đống đó xuống bằng phần nguyên của căn bậc hai của số quà ban đầu trong đống.</li>
</ul>

<p>Trả về <em>số lượng quà còn lại sau </em><code>k</code><em> giây.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> gifts = [25,64,9,4,100], k = 4
<strong>Đầu ra:</strong> 29
<strong>Giải thích:</strong>
Các món quà được lấy theo cách sau:
- Ở giây đầu tiên, đống cuối cùng được chọn và còn lại 10 món quà.
- Sau đó, đống thứ hai được chọn và còn lại 8 món quà.
- Tiếp theo, đống đầu tiên được chọn và còn lại 5 món quà.
- Cuối cùng, đống cuối cùng lại được chọn và còn lại 3 món quà.
Các món quà còn lại cuối cùng là [5,8,9,4,3], nên tổng số quà còn lại là 29.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> gifts = [1,1,1,1], k = 4
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong>
Trong trường hợp này, bất kể chọn đống nào, bạn cũng phải để lại 1 món quà trong mỗi đống.
Nói cách khác, bạn không thể lấy đi món quà nào trong các đống.
Vì vậy, tổng số quà còn lại là 4.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= gifts.length &lt;= 10<sup>3</sup></code></li>
	<li><code>1 &lt;= gifts[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>3</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Priority Queue (Max Heap)

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi bước thay đống giàu nhất bằng $\lfloor\sqrt{\,\cdot\,}\rfloor$, thực hiện $k$ lần, rồi tính tổng các giá trị còn lại. Duyệt để tìm giá trị lớn nhất ở mỗi bước vẫn đáp ứng được giới hạn, nhưng heap giúp lấy trực tiếp giá trị này.
>
> Max-heap lấy phần tử đầu tiên rồi đưa căn bậc hai nguyên của nó vào lại heap, thực hiện $k$ lần, sau đó tính tổng heap. Python lưu các giá trị âm.

<!-- thinking:end -->

Ta có thể lưu mảng $gifts$ trong một max heap, sau đó lặp lại $k$ lần; ở mỗi lần, lấy phần tử đầu heap ra, lấy căn bậc hai của phần tử đó rồi đưa kết quả trở lại heap.

Cuối cùng, cộng tất cả phần tử trong heap để nhận được đáp án.

Độ phức tạp thời gian là $O(n + k \times \log n)$, còn độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $gifts$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def pickGifts(self, gifts: List[int], k: int) -> int:
        h = [-v for v in gifts]
        heapify(h)
        for _ in range(k):
            heapreplace(h, -int(sqrt(-h[0])))
        return -sum(h)
```

#### Java

```java
class Solution {
    public long pickGifts(int[] gifts, int k) {
        PriorityQueue<Integer> pq = new PriorityQueue<>((a, b) -> b - a);
        for (int v : gifts) {
            pq.offer(v);
        }
        while (k-- > 0) {
            pq.offer((int) Math.sqrt(pq.poll()));
        }
        long ans = 0;
        for (int v : pq) {
            ans += v;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long pickGifts(vector<int>& gifts, int k) {
        make_heap(gifts.begin(), gifts.end());
        while (k--) {
            pop_heap(gifts.begin(), gifts.end());
            gifts.back() = sqrt(gifts.back());
            push_heap(gifts.begin(), gifts.end());
        }
        return accumulate(gifts.begin(), gifts.end(), 0LL);
    }
};
```

#### Go

```go
func pickGifts(gifts []int, k int) (ans int64) {
	h := &hp{gifts}
	heap.Init(h)
	for ; k > 0; k-- {
		gifts[0] = int(math.Sqrt(float64(gifts[0])))
		heap.Fix(h, 0)
	}
	for _, x := range gifts {
		ans += int64(x)
	}
	return
}

type hp struct{ sort.IntSlice }

func (h hp) Less(i, j int) bool { return h.IntSlice[i] > h.IntSlice[j] }
func (hp) Pop() (_ any)         { return }
func (hp) Push(any)             {}
```

#### TypeScript

```ts
function pickGifts(gifts: number[], k: number): number {
    const pq = new MaxPriorityQueue<number>();
    gifts.forEach(v => pq.enqueue(v));
    while (k--) {
        let v = pq.dequeue();
        v = Math.floor(Math.sqrt(v));
        pq.enqueue(v);
    }
    let ans = 0;
    while (!pq.isEmpty()) {
        ans += pq.dequeue();
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn pick_gifts(gifts: Vec<i32>, k: i32) -> i64 {
        let mut h = std::collections::BinaryHeap::from(gifts);
        let mut ans = 0;

        for _ in 0..k {
            if let Some(mut max_gift) = h.pop() {
                max_gift = (max_gift as f64).sqrt().floor() as i32;
                h.push(max_gift);
            }
        }

        for x in h {
            ans += x as i64;
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
