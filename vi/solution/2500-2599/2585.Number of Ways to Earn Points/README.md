---
comments: true
difficulty: Hard
rating: 1909
source: Weekly Contest 335 Q4
tags:
    - Array
    - Dynamic Programming
    - Knapsack
    - Bounded Knapsack
---

<!-- problem:start -->

# [2585. Number of Ways to Earn Points](https://leetcode.com/problems/number-of-ways-to-earn-points)

[中文文档](/solution/2500-2599/2585.Number%20of%20Ways%20to%20Earn%20Points/README.md)

## Mô tả

<!-- description:start -->

<p>Có một bài kiểm tra gồm <code>n</code> loại câu hỏi. Cho một số nguyên <code>target</code> và một mảng số nguyên 2 chiều <strong>0-indexed</strong> <code>types</code>, trong đó <code>types[i] = [count<sub>i</sub>, marks<sub>i</sub>]</code> cho biết có <code>count<sub>i</sub></code> câu hỏi thuộc loại <code>i<sup>th</sup></code>, và mỗi câu có giá trị <code>marks<sub>i</sub></code> điểm.</p>

<ul>
</ul>

<p>Trả về <em>số cách để bạn đạt được <strong>chính xác</strong> </em><code>target</code><em> điểm trong bài kiểm tra</em>. Vì đáp án có thể rất lớn, hãy trả về kết quả <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p><strong>Lưu ý</strong> rằng các câu hỏi cùng loại không thể phân biệt.</p>

<ul>
	<li>Ví dụ, nếu có <code>3</code> câu hỏi cùng loại, thì việc làm câu hỏi thứ <code>1<sup>st</sup></code> và <code>2<sup>nd</sup></code> cũng giống như làm câu hỏi thứ <code>1<sup>st</sup></code> và <code>3<sup>rd</sup></code>, hoặc câu hỏi thứ <code>2<sup>nd</sup></code> và <code>3<sup>rd</sup></code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> target = 6, types = [[6,1],[3,2],[2,3]]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Bạn có thể đạt 6 điểm theo một trong bảy cách:
- Làm 6 câu hỏi thuộc loại thứ 0<sup>th</sup>: 1 + 1 + 1 + 1 + 1 + 1 = 6
- Làm 4 câu hỏi thuộc loại thứ 0<sup>th</sup> và 1 câu hỏi thuộc loại thứ 1<sup>st</sup>: 1 + 1 + 1 + 1 + 2 = 6
- Làm 2 câu hỏi thuộc loại thứ 0<sup>th</sup> và 2 câu hỏi thuộc loại thứ 1<sup>st</sup>: 1 + 1 + 2 + 2 = 6
- Làm 3 câu hỏi thuộc loại thứ 0<sup>th</sup> và 1 câu hỏi thuộc loại thứ 2<sup>nd</sup>: 1 + 1 + 1 + 3 = 6
- Làm 1 câu hỏi thuộc loại thứ 0<sup>th</sup>, 1 câu hỏi thuộc loại thứ 1<sup>st</sup> và 1 câu hỏi thuộc loại thứ 2<sup>nd</sup>: 1 + 2 + 3 = 6
- Làm 3 câu hỏi thuộc loại thứ 1<sup>st</sup>: 2 + 2 + 2 = 6
- Làm 2 câu hỏi thuộc loại thứ 2<sup>nd</sup>: 3 + 3 = 6
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> target = 5, types = [[50,1],[50,2],[50,5]]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Bạn có thể đạt 5 điểm theo một trong bốn cách:
- Làm 5 câu hỏi thuộc loại thứ 0<sup>th</sup>: 1 + 1 + 1 + 1 + 1 = 5
- Làm 3 câu hỏi thuộc loại thứ 0<sup>th</sup> và 1 câu hỏi thuộc loại thứ 1<sup>st</sup>: 1 + 1 + 1 + 2 = 5
- Làm 1 câu hỏi thuộc loại thứ 0<sup>th</sup> và 2 câu hỏi thuộc loại thứ 1<sup>st</sup>: 1 + 2 + 2 = 5
- Làm 1 câu hỏi thuộc loại thứ 2<sup>nd</sup>: 5
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> target = 18, types = [[6,1],[3,2],[2,3]]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Bạn chỉ có thể đạt 18 điểm bằng cách trả lời tất cả câu hỏi.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= target &lt;= 1000</code></li>
	<li><code>n == types.length</code></li>
	<li><code>1 &lt;= n &lt;= 50</code></li>
	<li><code>types[i].length == 2</code></li>
	<li><code>1 &lt;= count<sub>i</sub>, marks<sub>i</sub> &lt;= 50</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi loại câu hỏi có giới hạn số lượng và số điểm cố định; ta cần đếm số cách đạt đúng $\textit{target}$. Knapsack không giới hạn sẽ đếm thừa.
>
> Đây là bài toán bounded knapsack: $f[i][j]$ là số cách đạt $j$ điểm bằng $i$ loại câu hỏi đầu tiên. Với loại $i$, ta có thể chọn từ $0..\textit{count}$ câu, cộng thêm $f[i-1][j-k\cdot\textit{marks}]$. Khởi tạo $f[0][0]=1$; đáp án là $f[n][\textit{target}]$.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là số cách đạt chính xác $j$ điểm từ $i$ loại câu hỏi đầu tiên. Ban đầu, $f[0][0] = 1$, các giá trị còn lại $f[i][j] = 0$. Đáp án là $f[n][target]$.

Ta có thể duyệt qua loại câu hỏi thứ $i$, giả sử số câu hỏi thuộc loại này là $count$ và số điểm là $marks$. Khi đó, ta có công thức chuyển trạng thái sau:

$$
f[i][j] = \sum_{k=0}^{count} f[i-1][j-k \times marks]
$$

trong đó $k$ là số câu hỏi thuộc loại thứ $i$ được chọn.

Đáp án cuối cùng là $f[n][target]$. Lưu ý rằng đáp án có thể rất lớn và cần được lấy modulo $10^9 + 7$.

Độ phức tạp thời gian là $O(n \times target \times count)$ và độ phức tạp không gian là $O(n \times target)$. $n$ là số loại câu hỏi, còn $target$ và $count$ lần lượt là số điểm mục tiêu và số câu hỏi của mỗi loại.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def waysToReachTarget(self, target: int, types: List[List[int]]) -> int:
        n = len(types)
        mod = 10**9 + 7
        f = [[0] * (target + 1) for _ in range(n + 1)]
        f[0][0] = 1
        for i in range(1, n + 1):
            count, marks = types[i - 1]
            for j in range(target + 1):
                for k in range(count + 1):
                    if j >= k * marks:
                        f[i][j] = (f[i][j] + f[i - 1][j - k * marks]) % mod
        return f[n][target]
```

#### Java

```java
class Solution {
    public int waysToReachTarget(int target, int[][] types) {
        int n = types.length;
        final int mod = (int) 1e9 + 7;
        int[][] f = new int[n + 1][target + 1];
        f[0][0] = 1;
        for (int i = 1; i <= n; ++i) {
            int count = types[i - 1][0], marks = types[i - 1][1];
            for (int j = 0; j <= target; ++j) {
                for (int k = 0; k <= count; ++k) {
                    if (j >= k * marks) {
                        f[i][j] = (f[i][j] + f[i - 1][j - k * marks]) % mod;
                    }
                }
            }
        }
        return f[n][target];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int waysToReachTarget(int target, vector<vector<int>>& types) {
        int n = types.size();
        const int mod = 1e9 + 7;
        int f[n + 1][target + 1];
        memset(f, 0, sizeof(f));
        f[0][0] = 1;
        for (int i = 1; i <= n; ++i) {
            int count = types[i - 1][0], marks = types[i - 1][1];
            for (int j = 0; j <= target; ++j) {
                for (int k = 0; k <= count; ++k) {
                    if (j >= k * marks) {
                        f[i][j] = (f[i][j] + f[i - 1][j - k * marks]) % mod;
                    }
                }
            }
        }
        return f[n][target];
    }
};
```

#### Go

```go
func waysToReachTarget(target int, types [][]int) int {
	n := len(types)
	f := make([][]int, n+1)
	for i := range f {
		f[i] = make([]int, target+1)
	}
	f[0][0] = 1
	const mod = 1e9 + 7
	for i := 1; i <= n; i++ {
		count, marks := types[i-1][0], types[i-1][1]
		for j := 0; j <= target; j++ {
			for k := 0; k <= count; k++ {
				if j >= k*marks {
					f[i][j] = (f[i][j] + f[i-1][j-k*marks]) % mod
				}
			}
		}
	}
	return f[n][target]
}
```

#### TypeScript

```ts
function waysToReachTarget(target: number, types: number[][]): number {
    const n = types.length;
    const mod = 10 ** 9 + 7;
    const f: number[][] = Array.from({ length: n + 1 }, () => Array(target + 1).fill(0));
    f[0][0] = 1;
    for (let i = 1; i <= n; ++i) {
        const [count, marks] = types[i - 1];
        for (let j = 0; j <= target; ++j) {
            for (let k = 0; k <= count; ++k) {
                if (j >= k * marks) {
                    f[i][j] = (f[i][j] + f[i - 1][j - k * marks]) % mod;
                }
            }
        }
    }
    return f[n][target];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
