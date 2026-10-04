---
comments: true
difficulty: Medium
rating: 1325
source: Weekly Contest 388 Q2
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [3075. Maximize Happiness of Selected Children](https://leetcode.com/problems/maximize-happiness-of-selected-children)

[中文文档](/solution/3000-3099/3075.Maximize%20Happiness%20of%20Selected%20Children/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <code>happiness</code> có độ dài <code>n</code> và một số nguyên <strong>dương</strong> <code>k</code>.</p>

<p>Có <code>n</code> đứa trẻ đang xếp hàng, trong đó đứa trẻ có chỉ số <code>i<sup>th</sup></code> có <strong>giá trị hạnh phúc</strong> là <code>happiness[i]</code>. Bạn muốn chọn <code>k</code> đứa trẻ trong số <code>n</code> đứa trẻ đó qua <code>k</code> lượt.</p>

<p>Trong mỗi lượt, khi bạn chọn một đứa trẻ, <strong>giá trị hạnh phúc</strong> của tất cả những đứa trẻ <strong>chưa được chọn cho đến thời điểm hiện tại</strong> sẽ giảm đi <code>1</code>. Lưu ý rằng giá trị hạnh phúc <strong>không thể</strong> trở thành số âm và <strong>chỉ giảm</strong> nếu nó dương.</p>

<p>Trả về <em><strong>tổng lớn nhất</strong> các giá trị hạnh phúc của những đứa trẻ được chọn mà bạn có thể đạt được khi chọn </em><code>k</code> <em>đứa trẻ</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> happiness = [1,2,3], k = 2
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Ta có thể chọn 2 đứa trẻ theo cách sau:
- Chọn đứa trẻ có giá trị hạnh phúc == 3. Giá trị hạnh phúc của những đứa trẻ còn lại trở thành [0,1].
- Chọn đứa trẻ có giá trị hạnh phúc == 1. Giá trị hạnh phúc của đứa trẻ còn lại trở thành [0]. Lưu ý rằng giá trị hạnh phúc không thể nhỏ hơn 0.
Tổng giá trị hạnh phúc của những đứa trẻ được chọn là 3 + 1 = 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> happiness = [1,1,1,1], k = 2
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Ta có thể chọn 2 đứa trẻ theo cách sau:
- Chọn bất kỳ đứa trẻ nào có giá trị hạnh phúc == 1. Giá trị hạnh phúc của những đứa trẻ còn lại trở thành [0,0,0].
- Chọn đứa trẻ có giá trị hạnh phúc == 0. Giá trị hạnh phúc của những đứa trẻ còn lại trở thành [0,0].
Tổng giá trị hạnh phúc của những đứa trẻ được chọn là 1 + 0 = 1.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> happiness = [2,3,4,5], k = 1
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Ta có thể chọn 1 đứa trẻ theo cách sau:
- Chọn đứa trẻ có giá trị hạnh phúc == 5. Giá trị hạnh phúc của những đứa trẻ còn lại trở thành [1,2,3].
Tổng giá trị hạnh phúc của đứa trẻ được chọn là 5.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == happiness.length &lt;= 2 * 10<sup>5</sup></code></li>
	<li><code>1 &lt;= happiness[i] &lt;= 10<sup>8</sup></code></li>
	<li><code>1 &lt;= k &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Sorting

<!-- thinking:start -->

> **Tư duy**
>
> Việc chọn một đứa trẻ làm giảm giá trị hạnh phúc của mọi đứa trẻ còn lại đi $1$ (không nhỏ hơn $0$). $n \le 2 \times 10^5$ và ta chọn $k$ đứa trẻ.
>
> Các lượt chọn sau bị giảm nhiều lần hơn, nên ta cần chọn các giá trị hiện tại lớn hơn trước. Lượt chọn thứ $i$ đóng góp $\max(h-i,0)$.
>
> Sắp xếp theo thứ tự giảm dần rồi cộng công thức đó cho $k$ đứa trẻ đầu tiên.

<!-- thinking:end -->

Để tối đa hóa tổng các giá trị hạnh phúc, ta nên ưu tiên chọn những đứa trẻ có giá trị hạnh phúc cao hơn. Do đó, ta có thể sắp xếp những đứa trẻ theo thứ tự giảm dần của giá trị hạnh phúc, sau đó lần lượt chọn $k$ đứa trẻ. Với đứa trẻ được chọn ở lượt thứ $i$, giá trị hạnh phúc nhận được là $\max(\textit{happiness}[i] - i, 0)$. Cuối cùng, trả về tổng giá trị hạnh phúc của $k$ đứa trẻ này.

Độ phức tạp thời gian là $O(n \times \log n + k)$, và độ phức tạp không gian là $O(\log n)$, trong đó $n$ là độ dài của mảng $\textit{happiness}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumHappinessSum(self, happiness: List[int], k: int) -> int:
        happiness.sort(reverse=True)
        ans = 0
        for i, x in enumerate(happiness[:k]):
            x -= i
            ans += max(x, 0)
        return ans
```

#### Java

```java
class Solution {
    public long maximumHappinessSum(int[] happiness, int k) {
        Arrays.sort(happiness);
        long ans = 0;
        for (int i = 0, n = happiness.length; i < k; ++i) {
            int x = happiness[n - i - 1] - i;
            ans += Math.max(x, 0);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maximumHappinessSum(vector<int>& happiness, int k) {
        sort(happiness.rbegin(), happiness.rend());
        long long ans = 0;
        for (int i = 0, n = happiness.size(); i < k; ++i) {
            int x = happiness[i] - i;
            ans += max(x, 0);
        }
        return ans;
    }
};
```

#### Go

```go
func maximumHappinessSum(happiness []int, k int) (ans int64) {
	sort.Ints(happiness)
	for i := 0; i < k; i++ {
		x := happiness[len(happiness)-i-1] - i
		ans += int64(max(x, 0))
	}
	return
}
```

#### TypeScript

```ts
function maximumHappinessSum(happiness: number[], k: number): number {
    happiness.sort((a, b) => b - a);
    let ans = 0;
    for (let i = 0; i < k; ++i) {
        const x = happiness[i] - i;
        ans += Math.max(x, 0);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn maximum_happiness_sum(mut happiness: Vec<i32>, k: i32) -> i64 {
        happiness.sort_unstable_by(|a, b| b.cmp(a));

        let mut ans: i64 = 0;
        for i in 0..(k as usize) {
            let x = happiness[i] as i64 - i as i64;
            if x > 0 {
                ans += x;
            }
        }
        ans
    }
}
```

#### C#

```cs
public class Solution {
    public long MaximumHappinessSum(int[] happiness, int k) {
        Array.Sort(happiness, (a, b) => b.CompareTo(a));
        long ans = 0;
        for (int i = 0; i < k; i++) {
            int x = happiness[i] - i;
            if (x <= 0) {
                break;
            }
            ans += x;
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
