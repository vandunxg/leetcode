---
comments: true
difficulty: Medium
rating: 1995
source: Weekly Contest 256 Q3
tags:
    - Bit Manipulation
    - Array
    - Dynamic Programming
    - Backtracking
    - Bitmask
---

<!-- problem:start -->

# [1986. Minimum Number of Work Sessions to Finish the Tasks](https://leetcode.com/problems/minimum-number-of-work-sessions-to-finish-the-tasks)

[中文文档](/solution/1900-1999/1986.Minimum%20Number%20of%20Work%20Sessions%20to%20Finish%20the%20Tasks/README.md)

## Mô tả

<!-- description:start -->
<p>Có <code>n</code> nhiệm vụ được giao cho bạn. Thời gian thực hiện các nhiệm vụ được biểu diễn bằng mảng số nguyên <code>tasks</code> có độ dài <code>n</code>, trong đó nhiệm vụ thứ <code>i<sup>th</sup></code> cần <code>tasks[i]</code> giờ để hoàn thành. Một <strong>phiên làm việc</strong> là khoảng thời gian bạn làm việc liên tục <strong>không quá</strong> <code>sessionTime</code> giờ rồi nghỉ.</p>

<p>Bạn cần hoàn thành các nhiệm vụ đã cho sao cho thỏa mãn các điều kiện sau:</p>

<ul>
	<li>Nếu bắt đầu một nhiệm vụ trong một phiên làm việc, bạn phải hoàn thành nhiệm vụ đó trong <strong>cùng</strong> phiên làm việc.</li>
	<li>Bạn có thể bắt đầu một nhiệm vụ mới <strong>ngay lập tức</strong> sau khi hoàn thành nhiệm vụ trước đó.</li>
	<li>Bạn có thể hoàn thành các nhiệm vụ theo <strong>bất kỳ thứ tự nào</strong>.</li>
</ul>

<p>Cho <code>tasks</code> và <code>sessionTime</code>, hãy trả về <em><strong>số phiên làm việc</strong> <strong>ít nhất</strong> cần thiết để hoàn thành tất cả nhiệm vụ theo các điều kiện trên.</em></p>

<p>Các test được tạo sao cho <code>sessionTime</code> <strong>lớn hơn</strong> hoặc <strong>bằng</strong> <strong>phần tử lớn nhất</strong> trong <code>tasks[i]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> tasks = [1,2,3], sessionTime = 3
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Bạn có thể hoàn thành các nhiệm vụ trong hai phiên làm việc.
- Phiên làm việc đầu tiên: hoàn thành nhiệm vụ thứ nhất và thứ hai trong 1 + 2 = 3 giờ.
- Phiên làm việc thứ hai: hoàn thành nhiệm vụ thứ ba trong 3 giờ.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> tasks = [3,1,3,1,1], sessionTime = 8
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Bạn có thể hoàn thành các nhiệm vụ trong hai phiên làm việc.
- Phiên làm việc đầu tiên: hoàn thành tất cả nhiệm vụ ngoại trừ nhiệm vụ cuối cùng trong 3 + 1 + 3 + 1 = 8 giờ.
- Phiên làm việc thứ hai: hoàn thành nhiệm vụ cuối cùng trong 1 giờ.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> tasks = [1,2,3,4,5], sessionTime = 15
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Bạn có thể hoàn thành tất cả nhiệm vụ trong một phiên làm việc.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == tasks.length</code></li>
	<li><code>1 &lt;= n &lt;= 14</code></li>
	<li><code>1 &lt;= tasks[i] &lt;= 10</code></li>
	<li><code>max(tasks[i]) &lt;= sessionTime &lt;= 15</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động nén trạng thái + Liệt kê tập con

<!-- thinking:start -->

> **Tư duy**
>
> Các task phải được xếp vào các phiên có sức chứa $\textit{sessionTime}$, với $n\le 14$ cho phép dùng subset DP.
>
> Đánh dấu các tập con có thể vừa trong một phiên, sau đó với mỗi mask $i$ thử mọi submask $j$: nếu $j$ khả thi, $f[i]=\min(f[i\oplus j]+1)$.
>
> $f[2^n-1]$ là số phiên làm việc.

<!-- thinking:end -->

Ta nhận thấy rằng $n$ không vượt quá $14$, vì vậy có thể sử dụng quy hoạch động nén trạng thái để giải bài toán này.

Ta dùng số nhị phân $i$ có độ dài $n$ để biểu diễn trạng thái hiện tại của các nhiệm vụ, trong đó bit thứ $j$ của $i$ bằng $1$ khi và chỉ khi nhiệm vụ thứ $j$ đã hoàn thành. Ta dùng $f[i]$ để biểu diễn số phiên làm việc ít nhất cần thiết để hoàn thành các nhiệm vụ tương ứng với trạng thái $i$.

Ta có thể liệt kê mọi tập con $j$ của $i$, trong đó mỗi bit trong biểu diễn nhị phân của $j$ là tập con của bit tương ứng trong biểu diễn nhị phân của $i$, tức là $j \subseteq i$. Nếu các nhiệm vụ tương ứng với $j$ có thể được hoàn thành trong một phiên làm việc, ta cập nhật $f[i]$ bằng $f[i \oplus j] + 1$, trong đó $i \oplus j$ là phép XOR theo bit của $i$ và $j$.

Đáp án cuối cùng là $f[2^n - 1]$.

Độ phức tạp thời gian là $O(n \times 3^n)$, độ phức tạp không gian là $O(2^n)$. Trong đó, $n$ là số nhiệm vụ.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minSessions(self, tasks: List[int], sessionTime: int) -> int:
        n = len(tasks)
        ok = [False] * (1 << n)
        for i in range(1, 1 << n):
            t = sum(tasks[j] for j in range(n) if i >> j & 1)
            ok[i] = t <= sessionTime
        f = [inf] * (1 << n)
        f[0] = 0
        for i in range(1, 1 << n):
            j = i
            while j:
                if ok[j]:
                    f[i] = min(f[i], f[i ^ j] + 1)
                j = (j - 1) & i
        return f[-1]
```

#### Java

```java
class Solution {
    public int minSessions(int[] tasks, int sessionTime) {
        int n = tasks.length;
        boolean[] ok = new boolean[1 << n];
        for (int i = 1; i < 1 << n; ++i) {
            int t = 0;
            for (int j = 0; j < n; ++j) {
                if ((i >> j & 1) == 1) {
                    t += tasks[j];
                }
            }
            ok[i] = t <= sessionTime;
        }
        int[] f = new int[1 << n];
        Arrays.fill(f, 1 << 30);
        f[0] = 0;
        for (int i = 1; i < 1 << n; ++i) {
            for (int j = i; j > 0; j = (j - 1) & i) {
                if (ok[j]) {
                    f[i] = Math.min(f[i], f[i ^ j] + 1);
                }
            }
        }
        return f[(1 << n) - 1];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minSessions(vector<int>& tasks, int sessionTime) {
        int n = tasks.size();
        bool ok[1 << n];
        memset(ok, false, sizeof(ok));
        for (int i = 1; i < 1 << n; ++i) {
            int t = 0;
            for (int j = 0; j < n; ++j) {
                if (i >> j & 1) {
                    t += tasks[j];
                }
            }
            ok[i] = t <= sessionTime;
        }
        int f[1 << n];
        memset(f, 0x3f, sizeof(f));
        f[0] = 0;
        for (int i = 1; i < 1 << n; ++i) {
            for (int j = i; j; j = (j - 1) & i) {
                if (ok[j]) {
                    f[i] = min(f[i], f[i ^ j] + 1);
                }
            }
        }
        return f[(1 << n) - 1];
    }
};
```

#### Go

```go
func minSessions(tasks []int, sessionTime int) int {
	n := len(tasks)
	ok := make([]bool, 1<<n)
	f := make([]int, 1<<n)
	for i := 1; i < 1<<n; i++ {
		t := 0
		f[i] = 1 << 30
		for j, x := range tasks {
			if i>>j&1 == 1 {
				t += x
			}
		}
		ok[i] = t <= sessionTime
	}
	for i := 1; i < 1<<n; i++ {
		for j := i; j > 0; j = (j - 1) & i {
			if ok[j] {
				f[i] = min(f[i], f[i^j]+1)
			}
		}
	}
	return f[1<<n-1]
}
```

#### TypeScript

```ts
function minSessions(tasks: number[], sessionTime: number): number {
    const n = tasks.length;
    const ok: boolean[] = new Array(1 << n).fill(false);
    for (let i = 1; i < 1 << n; ++i) {
        let t = 0;
        for (let j = 0; j < n; ++j) {
            if (((i >> j) & 1) === 1) {
                t += tasks[j];
            }
        }
        ok[i] = t <= sessionTime;
    }

    const f: number[] = new Array(1 << n).fill(1 << 30);
    f[0] = 0;
    for (let i = 1; i < 1 << n; ++i) {
        for (let j = i; j > 0; j = (j - 1) & i) {
            if (ok[j]) {
                f[i] = Math.min(f[i], f[i ^ j] + 1);
            }
        }
    }
    return f[(1 << n) - 1];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
