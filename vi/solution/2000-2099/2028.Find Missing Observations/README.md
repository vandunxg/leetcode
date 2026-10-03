---
comments: true
difficulty: Medium
rating: 1444
source: Weekly Contest 261 Q2
tags:
    - Array
    - Math
    - Simulation
---

<!-- problem:start -->

# [2028. Find Missing Observations](https://leetcode.com/problems/find-missing-observations)

[中文文档](/solution/2000-2099/2028.Find%20Missing%20Observations/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có kết quả của <code>n + m</code> lần gieo một con xúc xắc <strong>6 mặt</strong>, trong đó mỗi mặt được đánh số từ <code>1</code> đến <code>6</code>. Kết quả của <code>n</code> lần gieo đã bị mất, và bạn chỉ còn kết quả của <code>m</code> lần gieo. May mắn là bạn cũng đã tính được <strong>giá trị trung bình</strong> của <code>n + m</code> lần gieo.</p>

<p>Bạn được cho một mảng số nguyên <code>rolls</code> có độ dài <code>m</code>, trong đó <code>rolls[i]</code> là giá trị của <code>i<sup>th</sup></code> observation. Bạn cũng được cho hai số nguyên <code>mean</code> và <code>n</code>.</p>

<p>Hãy trả về <em>một mảng có độ dài </em><code>n</code><em> chứa các kết quả bị thiếu sao cho <strong>giá trị trung bình </strong>của </em><code>n + m</code><em> lần gieo bằng <strong>chính xác</strong> </em><code>mean</code>. Nếu có nhiều đáp án hợp lệ, hãy trả về <em>bất kỳ đáp án nào</em>. Nếu không tồn tại mảng như vậy, hãy trả về <em>mảng rỗng</em>.</p>

<p><strong>Giá trị trung bình</strong> của một tập hợp gồm <code>k</code> số là tổng các số chia cho <code>k</code>.</p>

<p>Lưu ý rằng <code>mean</code> là một số nguyên, nên tổng của <code>n + m</code> lần gieo phải chia hết cho <code>n + m</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> rolls = [3,2,4,3], mean = 4, n = 2
<strong>Đầu ra:</strong> [6,6]
<strong>Giải thích:</strong> Giá trị trung bình của tất cả n + m lần gieo là (3 + 2 + 4 + 3 + 6 + 6) / 6 = 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> rolls = [1,5,6], mean = 3, n = 4
<strong>Đầu ra:</strong> [2,3,2,2]
<strong>Giải thích:</strong> Giá trị trung bình của tất cả n + m lần gieo là (1 + 5 + 6 + 2 + 3 + 2 + 2) / 7 = 3.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> rolls = [1,2,3,4], mean = 6, n = 4
<strong>Đầu ra:</strong> []
<strong>Giải thích:</strong> Không thể có giá trị trung bình bằng 6, bất kể bốn kết quả bị thiếu là gì.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == rolls.length</code></li>
	<li><code>1 &lt;= n, m &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= rolls[i], mean &lt;= 6</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Xây dựng

<!-- thinking:start -->

> **Tư duy**
>
> Ta thiếu $n$ lần gieo xúc xắc có giá trị trong $[1,6]$, với giá trị trung bình của tất cả các lần gieo là $mean$. Tổng cần tìm $s$ là cố định, nên ta chỉ cần kiểm tra $s \in [n,6n]$. Với $n,m \le 10^5$, xây dựng trực tiếp tốt hơn tìm kiếm.
>
> Nếu có đáp án, hãy phân phối đều $s$: mỗi phần tử có giá trị $s//n$, sau đó tăng một cho $s \bmod n$ phần tử đầu tiên; khi đó tất cả các phần tử vẫn thỏa mãn $\le 6$.

<!-- thinking:end -->

Theo đề bài, tổng của tất cả các số là $(n + m) \times \textit{mean}$, còn tổng các số đã biết là $\sum_{i=0}^{m-1} \textit{rolls}[i]$. Vì vậy, tổng của các số bị thiếu là $s = (n + m) \times \textit{mean} - \sum_{i=0}^{m-1} \textit{rolls}[i]$.

Nếu $s \gt n \times 6$ hoặc $s \lt n$, nghĩa là không có đáp án nào thỏa mãn các điều kiện, nên ta trả về một mảng rỗng.

Ngược lại, ta có thể phân phối đều $s$ cho $n$ số, tức là giá trị của mỗi số là $s / n$, sau đó tăng giá trị của $s \bmod n$ số lên $1$.

Độ phức tạp thời gian là $O(n + m)$, trong đó $n$ và $m$ lần lượt là số lượng kết quả bị thiếu và kết quả đã biết. Không tính phần không gian dành cho đáp án, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def missingRolls(self, rolls: List[int], mean: int, n: int) -> List[int]:
        m = len(rolls)
        s = (n + m) * mean - sum(rolls)
        if s > n * 6 or s < n:
            return []
        ans = [s // n] * n
        for i in range(s % n):
            ans[i] += 1
        return ans
```

#### Java

```java
class Solution {
    public int[] missingRolls(int[] rolls, int mean, int n) {
        int m = rolls.length;
        int s = (n + m) * mean;
        for (int v : rolls) {
            s -= v;
        }
        if (s > n * 6 || s < n) {
            return new int[0];
        }
        int[] ans = new int[n];
        Arrays.fill(ans, s / n);
        for (int i = 0; i < s % n; ++i) {
            ++ans[i];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> missingRolls(vector<int>& rolls, int mean, int n) {
        int m = rolls.size();
        int s = (n + m) * mean - accumulate(rolls.begin(), rolls.end(), 0);
        if (s > n * 6 || s < n) {
            return {};
        }
        vector<int> ans(n, s / n);
        for (int i = 0; i < s % n; ++i) {
            ++ans[i];
        }
        return ans;
    }
};
```

#### Go

```go
func missingRolls(rolls []int, mean int, n int) []int {
	m := len(rolls)
	s := (n + m) * mean
	for _, v := range rolls {
		s -= v
	}
	if s > n*6 || s < n {
		return []int{}
	}
	ans := make([]int, n)
	for i, j := 0, 0; i < n; i, j = i+1, j+1 {
		ans[i] = s / n
		if j < s%n {
			ans[i]++
		}
	}
	return ans
}
```

#### TypeScript

```ts
function missingRolls(rolls: number[], mean: number, n: number): number[] {
    const m = rolls.length;
    const s = (n + m) * mean - rolls.reduce((a, b) => a + b, 0);
    if (s > n * 6 || s < n) {
        return [];
    }
    const ans: number[] = Array(n).fill((s / n) | 0);
    for (let i = 0; i < s % n; ++i) {
        ans[i]++;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn missing_rolls(rolls: Vec<i32>, mean: i32, n: i32) -> Vec<i32> {
        let m = rolls.len() as i32;
        let s = (n + m) * mean - rolls.iter().sum::<i32>();

        if s > n * 6 || s < n {
            return vec![];
        }

        let mut ans = vec![s / n; n as usize];
        for i in 0..(s % n) as usize {
            ans[i] += 1;
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
