---
comments: true
difficulty: Hard
rating: 2265
source: Weekly Contest 276 Q4
tags:
    - Greedy
    - Array
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [2141. Maximum Running Time of N Computers](https://leetcode.com/problems/maximum-running-time-of-n-computers)

[中文文档](/solution/2100-2199/2141.Maximum%20Running%20Time%20of%20N%20Computers/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có <code>n</code> máy tính. Cho số nguyên <code>n</code> và một mảng số nguyên <code>batteries</code> <strong>được đánh chỉ số từ 0</strong>, trong đó pin thứ <code>i<sup>th</sup></code> có thể <strong>vận hành</strong> một máy tính trong <code>batteries[i]</code> phút. Bạn muốn vận hành <strong>tất cả</strong> <code>n</code> máy tính <strong>đồng thời</strong> bằng những viên pin đã cho.</p>

<p>Ban đầu, bạn có thể lắp <strong>nhiều nhất một viên pin</strong> vào mỗi máy tính. Sau đó, tại bất kỳ thời điểm nguyên nào, bạn có thể tháo pin khỏi một máy tính và lắp một viên pin khác vào <strong>bao nhiêu lần tùy ý</strong>. Viên pin được lắp vào có thể là pin hoàn toàn mới hoặc pin đang được lắp ở một máy tính khác. Có thể giả sử quá trình tháo và lắp pin không mất thời gian.</p>

<p>Lưu ý rằng pin không thể được sạc lại.</p>

<p>Hãy trả về <em><strong>số phút lớn nhất</strong> mà bạn có thể vận hành đồng thời tất cả </em><code>n</code><em> máy tính.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2141.Maximum%20Running%20Time%20of%20N%20Computers/images/example1-fit.png" style="width: 762px; height: 150px;" />
<pre>
<strong>Đầu vào:</strong> n = 2, batteries = [3,3,3]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong>
Ban đầu, lắp pin 0 vào máy tính thứ nhất và pin 1 vào máy tính thứ hai.
Sau hai phút, tháo pin 1 khỏi máy tính thứ hai và thay bằng pin 2. Lưu ý rằng pin 1 vẫn có thể vận hành trong một phút.
Kết thúc phút thứ ba, pin 0 cạn kiệt, nên bạn cần tháo pin này khỏi máy tính thứ nhất và lắp pin 1 vào.
Kết thúc phút thứ tư, pin 1 cũng cạn kiệt và máy tính thứ nhất không còn hoạt động.
Ta có thể vận hành đồng thời hai máy tính trong nhiều nhất 4 phút, nên kết quả là 4.

</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2141.Maximum%20Running%20Time%20of%20N%20Computers/images/example2.png" style="width: 629px; height: 150px;" />
<pre>
<strong>Đầu vào:</strong> n = 2, batteries = [1,1,1,1]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Ban đầu, lắp pin 0 vào máy tính thứ nhất và pin 2 vào máy tính thứ hai.
Sau một phút, pin 0 và pin 2 cạn kiệt, nên bạn cần tháo chúng ra và lắp pin 1 vào máy tính thứ nhất, pin 3 vào máy tính thứ hai.
Sau thêm một phút, pin 1 và pin 3 cũng cạn kiệt, nên máy tính thứ nhất và thứ hai không còn hoạt động.
Ta có thể vận hành đồng thời hai máy tính trong nhiều nhất 2 phút, nên kết quả là 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= batteries.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= batteries[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Nếu có thể vận hành $n$ máy tính trong $t$ phút, ta cũng có thể vận hành chúng trong mọi khoảng thời gian ngắn hơn. $t$ bị giới hạn bởi tổng dung lượng pin, nhưng không thể duyệt hết các giá trị.
>
> Một viên pin đóng góp nhiều nhất $t$ phút cho một lần vận hành dài $t$ phút, nên điều kiện khả thi là $\sum_i \min(b_i,t)\ge n\cdot t$. Ta tìm kiếm nhị phân $t$ trong đoạn $[0,\sum b_i]$.
>
> Mỗi giá trị giữa được kiểm tra bằng một lần duyệt tuyến tính; giá trị $t$ lớn nhất thỏa điều kiện chính là đáp án.

<!-- thinking:end -->

Ta nhận thấy rằng nếu có thể vận hành đồng thời $n$ máy tính trong $t$ phút, thì cũng có thể vận hành đồng thời $n$ máy tính trong $t' \le t$ phút, thể hiện tính đơn điệu. Do đó, ta có thể dùng phương pháp tìm kiếm nhị phân để tìm $t$ lớn nhất.

Ta đặt biên trái của tìm kiếm nhị phân là $l=0$ và biên phải là $r=\sum_{i=0}^{n-1} batteries[i]$. Trong mỗi lần lặp của tìm kiếm nhị phân, ta dùng biến $mid$ để biểu diễn giá trị ở giữa, tức là $mid = (l + r + 1) >> 1$. Ta kiểm tra xem có tồn tại một cách phân phối pin để $n$ máy tính chạy đồng thời trong $mid$ phút hay không. Nếu có, ta cập nhật $l$ thành $mid$; ngược lại, ta cập nhật $r$ thành $mid - 1$. Cuối cùng, ta trả về $l$ làm đáp án.

Bài toán được chuyển thành việc xác định xem có tồn tại một cách phân phối pin để $n$ máy tính chạy đồng thời trong $mid$ phút hay không. Nếu một viên pin có thể vận hành lâu hơn $mid$ phút, vì các máy tính chạy đồng thời trong $mid$ phút và mỗi viên pin chỉ có thể cấp điện cho một máy tính tại một thời điểm, ta chỉ có thể dùng viên pin này trong $mid$ phút. Nếu một viên pin có thể vận hành trong thời gian nhỏ hơn hoặc bằng $mid$, ta có thể sử dụng toàn bộ dung lượng của viên pin đó. Vì vậy, ta tính tổng số phút $s$ mà tất cả các viên pin có thể cấp điện; nếu $s \ge n \times mid$, ta có thể vận hành đồng thời $n$ máy tính trong $mid$ phút.

Độ phức tạp thời gian là $O(n \times \log M)$, trong đó $M$ là tổng dung lượng của tất cả các viên pin; độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxRunTime(self, n: int, batteries: List[int]) -> int:
        l, r = 0, sum(batteries)
        while l < r:
            mid = (l + r + 1) >> 1
            if sum(min(x, mid) for x in batteries) >= n * mid:
                l = mid
            else:
                r = mid - 1
        return l
```

#### Java

```java
class Solution {
    public long maxRunTime(int n, int[] batteries) {
        long l = 0, r = 0;
        for (int x : batteries) {
            r += x;
        }
        while (l < r) {
            long mid = (l + r + 1) >> 1;
            long s = 0;
            for (int x : batteries) {
                s += Math.min(mid, x);
            }
            if (s >= n * mid) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        return l;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxRunTime(int n, vector<int>& batteries) {
        long long l = 0, r = 0;
        for (int x : batteries) {
            r += x;
        }
        while (l < r) {
            long long mid = (l + r + 1) >> 1;
            long long s = 0;
            for (int x : batteries) {
                s += min(1LL * x, mid);
            }
            if (s >= n * mid) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        return l;
    }
};
```

#### Go

```go
func maxRunTime(n int, batteries []int) int64 {
	l, r := 0, 0
	for _, x := range batteries {
		r += x
	}
	for l < r {
		mid := (l + r + 1) >> 1
		s := 0
		for _, x := range batteries {
			s += min(x, mid)
		}
		if s >= n*mid {
			l = mid
		} else {
			r = mid - 1
		}
	}
	return int64(l)
}
```

#### TypeScript

```ts
function maxRunTime(n: number, batteries: number[]): number {
    let l = 0n;
    let r = 0n;
    for (const x of batteries) {
        r += BigInt(x);
    }
    while (l < r) {
        const mid = (l + r + 1n) >> 1n;
        let s = 0n;
        for (const x of batteries) {
            s += BigInt(Math.min(x, Number(mid)));
        }
        if (s >= mid * BigInt(n)) {
            l = mid;
        } else {
            r = mid - 1n;
        }
    }
    return Number(l);
}
```

#### Rust

```rust
impl Solution {
    pub fn max_run_time(n: i32, batteries: Vec<i32>) -> i64 {
        let n = n as i64;
        let mut l: i64 = 0;
        let mut r: i64 = batteries.iter().map(|&x| x as i64).sum();

        while l < r {
            let mid = (l + r + 1) >> 1;
            let mut s: i64 = 0;

            for &x in &batteries {
                let v = x as i64;
                s += if v < mid { v } else { mid };
            }

            if s >= n * mid {
                l = mid;
            } else {
                r = mid - 1;
            }
        }

        l
    }
}
```

#### C#

```cs
public class Solution {
    public long MaxRunTime(int n, int[] batteries) {
        long l = 0, r = 0;
        foreach (int x in batteries) {
            r += x;
        }

        while (l < r) {
            long mid = (l + r + 1) >> 1;
            long s = 0;

            foreach (int x in batteries) {
                s += Math.Min(mid, (long)x);
            }

            if (s >= (long)n * mid) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }

        return l;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
