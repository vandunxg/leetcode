---
comments: true
difficulty: Medium
rating: 1519
source: Weekly Contest 364 Q2
tags:
    - Stack
    - Array
    - Monotonic Stack
---

<!-- problem:start -->

# [2865. Beautiful Towers I](https://leetcode.com/problems/beautiful-towers-i)

[中文文档](/solution/2800-2899/2865.Beautiful%20Towers%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <code>heights</code> gồm <code>n</code> số nguyên, biểu diễn số viên gạch trong <code>n</code> tòa tháp liên tiếp. Nhiệm vụ của bạn là xóa một số viên gạch để tạo thành một cách sắp xếp các tòa tháp có <strong>hình dạng ngọn núi</strong>. Trong cách sắp xếp này, chiều cao các tòa tháp không giảm, đạt đến giá trị đỉnh lớn nhất tại một hoặc nhiều tòa tháp liên tiếp, sau đó không tăng.</p>

<p>Trả về <strong>tổng chiều cao lớn nhất có thể</strong> của một cách sắp xếp các tòa tháp có hình dạng ngọn núi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">heights = [5,3,4,1,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">13</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta xóa một số viên gạch để được <code>heights =&nbsp;[5,3,3,1,1]</code>, đỉnh nằm ở chỉ số 0.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">heights = [6,5,3,9,2,7]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">22</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta xóa một số viên gạch để được <code>heights =&nbsp;[3,3,3,9,2,2]</code>, đỉnh nằm ở chỉ số 3.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">heights = [3,2,5,5,2,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">18</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta xóa một số viên gạch để được <code>heights = [2,2,5,5,2,2]</code>, đỉnh nằm ở chỉ số 2 hoặc 3.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == heights.length &lt;= 10<sup>3</sup></code></li>
	<li><code>1 &lt;= heights[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Với $n\le 10^3$, ta có thể thử mọi vị trí đỉnh và mở rộng sang hai phía, lấy giá trị nhỏ hơn giữa chiều cao hiện tại và giới hạn chiều cao. Mỗi lần mở rộng đều duy trì điều kiện đơn điệu dạng núi.

<!-- thinking:end -->

Ta có thể lần lượt chọn mỗi tòa tháp làm tòa tháp cao nhất, mỗi lần mở rộng sang trái và phải, tính chiều cao của các vị trí còn lại rồi cộng lại thành tổng chiều cao $t$. Giá trị lớn nhất trong tất cả các tổng chiều cao là đáp án.

Độ phức tạp thời gian là $O(n^2)$, còn độ phức tạp không gian là $O(1)$. Trong đó, $n$ là độ dài của mảng $maxHeights$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumSumOfHeights(self, maxHeights: List[int]) -> int:
        ans, n = 0, len(maxHeights)
        for i, x in enumerate(maxHeights):
            y = t = x
            for j in range(i - 1, -1, -1):
                y = min(y, maxHeights[j])
                t += y
            y = x
            for j in range(i + 1, n):
                y = min(y, maxHeights[j])
                t += y
            ans = max(ans, t)
        return ans
```

#### Java

```java
class Solution {
    public long maximumSumOfHeights(List<Integer> maxHeights) {
        long ans = 0;
        int n = maxHeights.size();
        for (int i = 0; i < n; ++i) {
            int y = maxHeights.get(i);
            long t = y;
            for (int j = i - 1; j >= 0; --j) {
                y = Math.min(y, maxHeights.get(j));
                t += y;
            }
            y = maxHeights.get(i);
            for (int j = i + 1; j < n; ++j) {
                y = Math.min(y, maxHeights.get(j));
                t += y;
            }
            ans = Math.max(ans, t);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maximumSumOfHeights(vector<int>& maxHeights) {
        long long ans = 0;
        int n = maxHeights.size();
        for (int i = 0; i < n; ++i) {
            long long t = maxHeights[i];
            int y = t;
            for (int j = i - 1; ~j; --j) {
                y = min(y, maxHeights[j]);
                t += y;
            }
            y = maxHeights[i];
            for (int j = i + 1; j < n; ++j) {
                y = min(y, maxHeights[j]);
                t += y;
            }
            ans = max(ans, t);
        }
        return ans;
    }
};
```

#### Go

```go
func maximumSumOfHeights(maxHeights []int) (ans int64) {
	n := len(maxHeights)
	for i, x := range maxHeights {
		y, t := x, x
		for j := i - 1; j >= 0; j-- {
			y = min(y, maxHeights[j])
			t += y
		}
		y = x
		for j := i + 1; j < n; j++ {
			y = min(y, maxHeights[j])
			t += y
		}
		ans = max(ans, int64(t))
	}
	return
}
```

#### TypeScript

```ts
function maximumSumOfHeights(maxHeights: number[]): number {
    let ans = 0;
    const n = maxHeights.length;
    for (let i = 0; i < n; ++i) {
        const x = maxHeights[i];
        let [y, t] = [x, x];
        for (let j = i - 1; ~j; --j) {
            y = Math.min(y, maxHeights[j]);
            t += y;
        }
        y = x;
        for (let j = i + 1; j < n; ++j) {
            y = Math.min(y, maxHeights[j]);
            t += y;
        }
        ans = Math.max(ans, t);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động + Monotonic Stack

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 có độ phức tạp $O(n^2)$ nên không mở rộng tốt. Monotonic stack tìm tòa tháp gần nhất thấp hơn ở mỗi phía, nhờ đó tổng chiều cao $f[i]$ của phần tiền tố có thể được tính từ $f[j]$ trong thời gian hằng số; kết hợp mảng bên trái và bên phải tại mỗi đỉnh sẽ cho cùng đáp án nhưng nhanh hơn.

<!-- thinking:end -->

Lời giải 1 đủ để vượt qua bài này, nhưng độ phức tạp thời gian tương đối cao. Ta có thể dùng "Quy hoạch động + Monotonic Stack" để tối ưu quá trình liệt kê.

Ta định nghĩa $f[i]$ là tổng chiều cao của phương án tòa tháp đẹp trong đó tòa tháp cuối cùng là tòa tháp cao nhất trong $i+1$ tòa tháp đầu tiên. Ta có thể nhận được công thức chuyển trạng thái sau:

$$
f[i]=
\begin{cases}
f[i-1]+heights[i],&\textit{if } heights[i]\geq heights[i-1]\\
heights[i]\times(i-j)+f[j],&\textit{if } heights[i]<heights[i-1]
\end{cases}
$$

Trong đó, $j$ là chỉ số của tòa tháp đầu tiên bên trái tòa tháp cuối cùng có chiều cao nhỏ hơn hoặc bằng $heights[i]$. Ta có thể dùng monotonic stack để duy trì chỉ số này.

Ta có thể dùng phương pháp tương tự để tìm $g[i]$, là tổng chiều cao của phương án tòa tháp đẹp khi duyệt từ phải sang trái và tòa tháp thứ $i$ là tòa tháp cao nhất. Đáp án cuối cùng là giá trị lớn nhất của $f[i]+g[i]-heights[i]$.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $maxHeights$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumSumOfHeights(self, maxHeights: List[int]) -> int:
        n = len(maxHeights)
        stk = []
        left = [-1] * n
        for i, x in enumerate(maxHeights):
            while stk and maxHeights[stk[-1]] > x:
                stk.pop()
            if stk:
                left[i] = stk[-1]
            stk.append(i)
        stk = []
        right = [n] * n
        for i in range(n - 1, -1, -1):
            x = maxHeights[i]
            while stk and maxHeights[stk[-1]] >= x:
                stk.pop()
            if stk:
                right[i] = stk[-1]
            stk.append(i)
        f = [0] * n
        for i, x in enumerate(maxHeights):
            if i and x >= maxHeights[i - 1]:
                f[i] = f[i - 1] + x
            else:
                j = left[i]
                f[i] = x * (i - j) + (f[j] if j != -1 else 0)
        g = [0] * n
        for i in range(n - 1, -1, -1):
            if i < n - 1 and maxHeights[i] >= maxHeights[i + 1]:
                g[i] = g[i + 1] + maxHeights[i]
            else:
                j = right[i]
                g[i] = maxHeights[i] * (j - i) + (g[j] if j != n else 0)
        return max(a + b - c for a, b, c in zip(f, g, maxHeights))
```

#### Java

```java
class Solution {
    public long maximumSumOfHeights(List<Integer> maxHeights) {
        int n = maxHeights.size();
        Deque<Integer> stk = new ArrayDeque<>();
        int[] left = new int[n];
        int[] right = new int[n];
        Arrays.fill(left, -1);
        Arrays.fill(right, n);
        for (int i = 0; i < n; ++i) {
            int x = maxHeights.get(i);
            while (!stk.isEmpty() && maxHeights.get(stk.peek()) > x) {
                stk.pop();
            }
            if (!stk.isEmpty()) {
                left[i] = stk.peek();
            }
            stk.push(i);
        }
        stk.clear();
        for (int i = n - 1; i >= 0; --i) {
            int x = maxHeights.get(i);
            while (!stk.isEmpty() && maxHeights.get(stk.peek()) >= x) {
                stk.pop();
            }
            if (!stk.isEmpty()) {
                right[i] = stk.peek();
            }
            stk.push(i);
        }
        long[] f = new long[n];
        long[] g = new long[n];
        for (int i = 0; i < n; ++i) {
            int x = maxHeights.get(i);
            if (i > 0 && x >= maxHeights.get(i - 1)) {
                f[i] = f[i - 1] + x;
            } else {
                int j = left[i];
                f[i] = 1L * x * (i - j) + (j >= 0 ? f[j] : 0);
            }
        }
        for (int i = n - 1; i >= 0; --i) {
            int x = maxHeights.get(i);
            if (i < n - 1 && x >= maxHeights.get(i + 1)) {
                g[i] = g[i + 1] + x;
            } else {
                int j = right[i];
                g[i] = 1L * x * (j - i) + (j < n ? g[j] : 0);
            }
        }
        long ans = 0;
        for (int i = 0; i < n; ++i) {
            ans = Math.max(ans, f[i] + g[i] - maxHeights.get(i));
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maximumSumOfHeights(vector<int>& maxHeights) {
        int n = maxHeights.size();
        stack<int> stk;
        vector<int> left(n, -1);
        vector<int> right(n, n);
        for (int i = 0; i < n; ++i) {
            int x = maxHeights[i];
            while (!stk.empty() && maxHeights[stk.top()] > x) {
                stk.pop();
            }
            if (!stk.empty()) {
                left[i] = stk.top();
            }
            stk.push(i);
        }
        stk = stack<int>();
        for (int i = n - 1; ~i; --i) {
            int x = maxHeights[i];
            while (!stk.empty() && maxHeights[stk.top()] >= x) {
                stk.pop();
            }
            if (!stk.empty()) {
                right[i] = stk.top();
            }
            stk.push(i);
        }
        long long f[n], g[n];
        for (int i = 0; i < n; ++i) {
            int x = maxHeights[i];
            if (i && x >= maxHeights[i - 1]) {
                f[i] = f[i - 1] + x;
            } else {
                int j = left[i];
                f[i] = 1LL * x * (i - j) + (j != -1 ? f[j] : 0);
            }
        }
        for (int i = n - 1; ~i; --i) {
            int x = maxHeights[i];
            if (i < n - 1 && x >= maxHeights[i + 1]) {
                g[i] = g[i + 1] + x;
            } else {
                int j = right[i];
                g[i] = 1LL * x * (j - i) + (j != n ? g[j] : 0);
            }
        }
        long long ans = 0;
        for (int i = 0; i < n; ++i) {
            ans = max(ans, f[i] + g[i] - maxHeights[i]);
        }
        return ans;
    }
};
```

#### Go

```go
func maximumSumOfHeights(maxHeights []int) (ans int64) {
	n := len(maxHeights)
	stk := []int{}
	left := make([]int, n)
	right := make([]int, n)
	for i := range left {
		left[i] = -1
		right[i] = n
	}
	for i, x := range maxHeights {
		for len(stk) > 0 && maxHeights[stk[len(stk)-1]] > x {
			stk = stk[:len(stk)-1]
		}
		if len(stk) > 0 {
			left[i] = stk[len(stk)-1]
		}
		stk = append(stk, i)
	}
	stk = []int{}
	for i := n - 1; i >= 0; i-- {
		x := maxHeights[i]
		for len(stk) > 0 && maxHeights[stk[len(stk)-1]] >= x {
			stk = stk[:len(stk)-1]
		}
		if len(stk) > 0 {
			right[i] = stk[len(stk)-1]
		}
		stk = append(stk, i)
	}
	f := make([]int64, n)
	g := make([]int64, n)
	for i, x := range maxHeights {
		if i > 0 && x >= maxHeights[i-1] {
			f[i] = f[i-1] + int64(x)
		} else {
			j := left[i]
			f[i] = int64(x) * int64(i-j)
			if j != -1 {
				f[i] += f[j]
			}
		}
	}
	for i := n - 1; i >= 0; i-- {
		x := maxHeights[i]
		if i < n-1 && x >= maxHeights[i+1] {
			g[i] = g[i+1] + int64(x)
		} else {
			j := right[i]
			g[i] = int64(x) * int64(j-i)
			if j != n {
				g[i] += g[j]
			}
		}
	}
	for i, x := range maxHeights {
		ans = max(ans, f[i]+g[i]-int64(x))
	}
	return
}
```

#### TypeScript

```ts
function maximumSumOfHeights(maxHeights: number[]): number {
    const n = maxHeights.length;
    const stk: number[] = [];
    const left: number[] = Array(n).fill(-1);
    const right: number[] = Array(n).fill(n);
    for (let i = 0; i < n; ++i) {
        const x = maxHeights[i];
        while (stk.length && maxHeights[stk.at(-1)] > x) {
            stk.pop();
        }
        if (stk.length) {
            left[i] = stk.at(-1);
        }
        stk.push(i);
    }
    stk.length = 0;
    for (let i = n - 1; ~i; --i) {
        const x = maxHeights[i];
        while (stk.length && maxHeights[stk.at(-1)] >= x) {
            stk.pop();
        }
        if (stk.length) {
            right[i] = stk.at(-1);
        }
        stk.push(i);
    }
    const f: number[] = Array(n).fill(0);
    const g: number[] = Array(n).fill(0);
    for (let i = 0; i < n; ++i) {
        const x = maxHeights[i];
        if (i && x >= maxHeights[i - 1]) {
            f[i] = f[i - 1] + x;
        } else {
            const j = left[i];
            f[i] = x * (i - j) + (j >= 0 ? f[j] : 0);
        }
    }
    for (let i = n - 1; ~i; --i) {
        const x = maxHeights[i];
        if (i + 1 < n && x >= maxHeights[i + 1]) {
            g[i] = g[i + 1] + x;
        } else {
            const j = right[i];
            g[i] = x * (j - i) + (j < n ? g[j] : 0);
        }
    }
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        ans = Math.max(ans, f[i] + g[i] - maxHeights[i]);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
