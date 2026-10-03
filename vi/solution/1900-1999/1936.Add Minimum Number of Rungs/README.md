---
comments: true
difficulty: Medium
rating: 1322
source: Weekly Contest 250 Q2
tags:
    - Greedy
    - Array
---

<!-- problem:start -->

# [1936. Add Minimum Number of Rungs](https://leetcode.com/problems/add-minimum-number-of-rungs)

[中文文档](/solution/1900-1999/1936.Add%20Minimum%20Number%20of%20Rungs/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>rungs</code> <strong>tăng dần nghiêm ngặt</strong>, trong đó biểu thị <strong>độ cao</strong> của các bậc trên một chiếc thang. Hiện tại bạn đang ở trên <strong>sàn</strong> tại độ cao <code>0</code> và muốn đi đến bậc cuối cùng.</p>

<p>Bạn cũng được cho một số nguyên <code>dist</code>. Bạn chỉ có thể trèo lên bậc cao hơn tiếp theo nếu khoảng cách giữa vị trí hiện tại (sàn hoặc một bậc) và bậc tiếp theo <strong>không vượt quá</strong> <code>dist</code>. Bạn có thể thêm bậc ở bất kỳ độ cao nguyên <strong>dương</strong> nào nếu tại đó chưa có bậc.</p>

<p>Trả về <em><strong>số bậc ít nhất</strong> cần thêm vào thang để bạn có thể trèo lên bậc cuối cùng.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> rungs = [1,3,5,10], dist = 2
<strong>Đầu ra:</strong> 2
<strong>Giải thích:
</strong>Hiện tại bạn không thể với tới bậc cuối cùng.
Thêm các bậc ở độ cao 7 và 8 để trèo lên chiếc thang này.
Khi đó, thang sẽ có các bậc ở [1,3,5,<u>7</u>,<u>8</u>,10].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> rungs = [3,6,8,10], dist = 3
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>
Có thể trèo lên thang này mà không cần thêm bậc.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> rungs = [3,4,6,7], dist = 2
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>
Hiện tại bạn không thể với tới bậc đầu tiên từ mặt đất.
Thêm một bậc ở độ cao 1 để trèo lên thang này.
Khi đó, thang sẽ có các bậc ở [<u>1</u>,3,4,6,7].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= rungs.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= rungs[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= dist &lt;= 10<sup>9</sup></code></li>
	<li><code>rungs</code> tăng dần <strong>nghiêm ngặt</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Khoảng cách lớn hơn $\textit{dist}$ cần có thêm bậc, và khoảng cách giữa mỗi bậc cũng không được vượt quá $\textit{dist}$. Độ cao có thể lên tới $10^9$, nên không thể thêm từng bậc một.
>
> Số bậc cần thêm ít nhất từ $a$ đến $b$ là $\lfloor(b-a-1)/\textit{dist}\rfloor$. Thêm $0$ vào đầu mảng rồi tính tổng trên các cặp phần tử kề nhau sẽ cho tổng cần tìm.
>
> Việc kéo giãn mỗi lần thêm bậc đến khoảng cách $\textit{dist}$ là tối ưu theo công thức trên.

<!-- thinking:end -->

Theo mô tả bài toán, mỗi khi muốn trèo lên một bậc mới, ta cần đảm bảo chênh lệch độ cao giữa bậc mới và vị trí hiện tại không vượt quá `dist`. Nếu không, ta tham lam thêm một bậc mới cách vị trí hiện tại một khoảng $dist$, trèo lên bậc mới, và tổng số bậc cần thêm là $\lfloor \frac{b - a - 1}{dist} \rfloor$, trong đó $a$ và $b$ lần lượt là vị trí hiện tại và độ cao của bậc mới. Đáp án là tổng số bậc được thêm.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của `rungs`. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def addRungs(self, rungs: List[int], dist: int) -> int:
        rungs = [0] + rungs
        return sum((b - a - 1) // dist for a, b in pairwise(rungs))
```

#### Java

```java
class Solution {
    public int addRungs(int[] rungs, int dist) {
        int ans = 0, prev = 0;
        for (int x : rungs) {
            ans += (x - prev - 1) / dist;
            prev = x;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int addRungs(vector<int>& rungs, int dist) {
        int ans = 0, prev = 0;
        for (int& x : rungs) {
            ans += (x - prev - 1) / dist;
            prev = x;
        }
        return ans;
    }
};
```

#### Go

```go
func addRungs(rungs []int, dist int) (ans int) {
	prev := 0
	for _, x := range rungs {
		ans += (x - prev - 1) / dist
		prev = x
	}
	return
}
```

#### TypeScript

```ts
function addRungs(rungs: number[], dist: number): number {
    let ans = 0;
    let prev = 0;
    for (const x of rungs) {
        ans += ((x - prev - 1) / dist) | 0;
        prev = x;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn add_rungs(rungs: Vec<i32>, dist: i32) -> i32 {
        let mut ans = 0;
        let mut prev = 0;

        for &x in rungs.iter() {
            ans += (x - prev - 1) / dist;
            prev = x;
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
