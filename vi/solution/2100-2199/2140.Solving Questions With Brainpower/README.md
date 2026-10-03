---
comments: true
difficulty: Medium
rating: 1709
source: Weekly Contest 276 Q3
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [2140. Solving Questions With Brainpower](https://leetcode.com/problems/solving-questions-with-brainpower)

[中文文档](/solution/2100-2199/2140.Solving%20Questions%20With%20Brainpower/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên 2 chiều <strong>đánh chỉ số từ 0</strong> <code>questions</code>, trong đó <code>questions[i] = [points<sub>i</sub>, brainpower<sub>i</sub>]</code>.</p>

<p>Mảng này mô tả các câu hỏi trong một bài thi. Bạn phải xử lý các câu hỏi <strong>theo thứ tự</strong> (bắt đầu từ câu hỏi <code>0</code>) và quyết định <strong>giải</strong> hoặc <strong>bỏ qua</strong> từng câu hỏi. Giải câu hỏi <code>i</code> sẽ giúp bạn <strong>nhận được</strong> <code>points<sub>i</sub></code> điểm, nhưng bạn sẽ <strong>không thể</strong> giải <code>brainpower<sub>i</sub></code> câu hỏi tiếp theo. Nếu bỏ qua câu hỏi <code>i</code>, bạn có thể quyết định ở câu hỏi tiếp theo.</p>

<ul>
	<li>Ví dụ, với <code>questions = [[3, 2], [4, 3], [4, 4], [2, 5]]</code>:

    <ul>
    <li>Nếu giải câu hỏi <code>0</code>, bạn nhận được <code>3</code> điểm nhưng không thể giải các câu hỏi <code>1</code> và <code>2</code>.</li>
    <li>Nếu thay vào đó bỏ qua câu hỏi <code>0</code> và giải câu hỏi <code>1</code>, bạn nhận được <code>4</code> điểm nhưng không thể giải các câu hỏi <code>2</code> và <code>3</code>.</li>
    </ul>
    </li>

</ul>

<p>Hãy trả về <em><strong>số điểm tối đa</strong> bạn có thể đạt được trong bài thi</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> questions = [[3,2],[4,3],[4,4],[2,5]]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Có thể đạt số điểm tối đa bằng cách giải các câu hỏi 0 và 3.
- Giải câu hỏi 0: Nhận 3 điểm, không thể giải 2 câu hỏi tiếp theo
- Không thể giải các câu hỏi 1 và 2
- Giải câu hỏi 3: Nhận 2 điểm
Tổng số điểm nhận được: 3 + 2 = 5. Không có cách nào khác để đạt 5 điểm trở lên.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> questions = [[1,1],[2,2],[3,3],[4,4],[5,5]]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Có thể đạt số điểm tối đa bằng cách giải các câu hỏi 1 và 4.
- Bỏ qua câu hỏi 0
- Giải câu hỏi 1: Nhận 2 điểm, không thể giải 2 câu hỏi tiếp theo
- Không thể giải các câu hỏi 2 và 3
- Giải câu hỏi 4: Nhận 5 điểm
Tổng số điểm nhận được: 2 + 5 = 7. Không có cách nào khác để đạt 7 điểm trở lên.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= questions.length &lt;= 10<sup>5</sup></code></li>
	<li><code>questions[i].length == 2</code></li>
	<li><code>1 &lt;= points<sub>i</sub>, brainpower<sub>i</sub> &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi câu hỏi, ta có thể giải hoặc bỏ qua; nếu giải thì sẽ nhảy qua $\textit{brainpower}$ câu hỏi. Các nhánh bị trùng lặp, nên tìm kiếm trực tiếp có độ phức tạp lũy thừa. Với $n\le 10^5$, ta cần một công thức truy hồi tuyến tính.
>
> Đáp án tối ưu từ chỉ số $i$ là giá trị lớn hơn giữa việc giải câu hỏi $i$ rồi tiếp tục từ $i+b+1$, hoặc bỏ qua và chuyển sang $i+1$. DFS có ghi nhớ sẽ lưu lại giá trị này.
>
> $\textit{dfs}(i)$ là đáp án bắt đầu từ $i$; ta trả về $\textit{dfs}(0)$.

<!-- thinking:end -->

Ta xây dựng hàm $\textit{dfs}(i)$, biểu diễn số điểm tối đa có thể đạt được khi bắt đầu từ câu hỏi thứ $i$. Đáp án là $\textit{dfs}(0)$.

Hàm $\textit{dfs}(i)$ được tính như sau:

- Nếu $i \geq n$, nghĩa là đã xử lý hết các câu hỏi, ta trả về $0$;
- Ngược lại, gọi số điểm của câu hỏi thứ $i$ là $p$, và số câu hỏi cần bỏ qua là $b$. Khi đó, $\textit{dfs}(i) = \max(p + \textit{dfs}(i + b + 1), \textit{dfs}(i + 1))$.

Để tránh tính toán lặp lại, ta có thể dùng kỹ thuật ghi nhớ bằng cách lưu các giá trị của $\textit{dfs}(i)$ trong mảng $f$.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ là số lượng câu hỏi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def mostPoints(self, questions: List[List[int]]) -> int:
        @cache
        def dfs(i: int) -> int:
            if i >= len(questions):
                return 0
            p, b = questions[i]
            return max(p + dfs(i + b + 1), dfs(i + 1))

        return dfs(0)
```

#### Java

```java
class Solution {
    private int n;
    private Long[] f;
    private int[][] questions;

    public long mostPoints(int[][] questions) {
        n = questions.length;
        f = new Long[n];
        this.questions = questions;
        return dfs(0);
    }

    private long dfs(int i) {
        if (i >= n) {
            return 0;
        }
        if (f[i] != null) {
            return f[i];
        }
        int p = questions[i][0], b = questions[i][1];
        return f[i] = Math.max(p + dfs(i + b + 1), dfs(i + 1));
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long mostPoints(vector<vector<int>>& questions) {
        int n = questions.size();
        long long f[n];
        memset(f, 0, sizeof(f));
        auto dfs = [&](this auto&& dfs, int i) -> long long {
            if (i >= n) {
                return 0;
            }
            if (f[i]) {
                return f[i];
            }
            int p = questions[i][0], b = questions[i][1];
            return f[i] = max(p + dfs(i + b + 1), dfs(i + 1));
        };
        return dfs(0);
    }
};
```

#### Go

```go
func mostPoints(questions [][]int) int64 {
	n := len(questions)
	f := make([]int64, n)
	var dfs func(int) int64
	dfs = func(i int) int64 {
		if i >= n {
			return 0
		}
		if f[i] > 0 {
			return f[i]
		}
		p, b := questions[i][0], questions[i][1]
		f[i] = max(int64(p)+dfs(i+b+1), dfs(i+1))
		return f[i]
	}
	return dfs(0)
}
```

#### TypeScript

```ts
function mostPoints(questions: number[][]): number {
    const n = questions.length;
    const f = Array(n).fill(0);
    const dfs = (i: number): number => {
        if (i >= n) {
            return 0;
        }
        if (f[i] > 0) {
            return f[i];
        }
        const [p, b] = questions[i];
        return (f[i] = Math.max(p + dfs(i + b + 1), dfs(i + 1)));
    };
    return dfs(0);
}
```

#### Rust

```rust
impl Solution {
    pub fn most_points(questions: Vec<Vec<i32>>) -> i64 {
        let n = questions.len();
        let mut f = vec![-1; n];

        fn dfs(i: usize, questions: &Vec<Vec<i32>>, f: &mut Vec<i64>) -> i64 {
            if i >= questions.len() {
                return 0;
            }
            if f[i] != -1 {
                return f[i];
            }
            let p = questions[i][0] as i64;
            let b = questions[i][1] as usize;
            f[i] = (p + dfs(i + b + 1, questions, f)).max(dfs(i + 1, questions, f));
            f[i]
        }

        dfs(0, &questions, &mut f)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 đã có độ phức tạp tuyến tính nhưng sử dụng stack đệ quy. Ta có thể dùng cùng công thức chuyển để điền một bảng từ cuối lên đầu.
>
> Gọi $f[i]$ là số điểm tốt nhất có thể đạt được từ $i$. Khi đó $f[i]=\max(f[i+1],p+f[i+b+1])$, với các chỉ số vượt phạm vi được xem là $0$.
>
> Tính từ $n-1$ về $0$ rồi trả về $f[0]$.

<!-- thinking:end -->

Ta định nghĩa $f[i]$ là số điểm tối đa có thể đạt được khi bắt đầu từ câu hỏi thứ $i$. Do đó, đáp án là $f[0]$.

Xét $f[i]$, gọi số điểm của câu hỏi thứ $i$ là $p$, và số câu hỏi cần bỏ qua là $b$. Nếu giải câu hỏi thứ $i$, ta cần tiếp tục từ câu hỏi sau khi bỏ qua $b$ câu hỏi, nên $f[i] = p + f[i + b + 1]$. Nếu bỏ qua câu hỏi thứ $i$, ta bắt đầu từ câu hỏi thứ $(i + 1)$, nên $f[i] = f[i + 1]$. Ta lấy giá trị lớn hơn trong hai lựa chọn. Công thức chuyển trạng thái như sau:

$$
f[i] = \max(p + f[i + b + 1], f[i + 1])
$$

Ta tính các giá trị của $f$ từ cuối về đầu, cuối cùng trả về $f[0]$.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là số lượng câu hỏi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def mostPoints(self, questions: List[List[int]]) -> int:
        n = len(questions)
        f = [0] * (n + 1)
        for i in range(n - 1, -1, -1):
            p, b = questions[i]
            j = i + b + 1
            f[i] = max(f[i + 1], p + (0 if j > n else f[j]))
        return f[0]
```

#### Java

```java
class Solution {
    public long mostPoints(int[][] questions) {
        int n = questions.length;
        long[] f = new long[n + 1];
        for (int i = n - 1; i >= 0; --i) {
            int p = questions[i][0], b = questions[i][1];
            int j = i + b + 1;
            f[i] = Math.max(f[i + 1], p + (j > n ? 0 : f[j]));
        }
        return f[0];
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long mostPoints(vector<vector<int>>& questions) {
        int n = questions.size();
        long long f[n + 1];
        memset(f, 0, sizeof(f));
        for (int i = n - 1; ~i; --i) {
            int p = questions[i][0], b = questions[i][1];
            int j = i + b + 1;
            f[i] = max(f[i + 1], p + (j > n ? 0 : f[j]));
        }
        return f[0];
    }
};
```

#### Go

```go
func mostPoints(questions [][]int) int64 {
	n := len(questions)
	f := make([]int64, n+1)
	for i := n - 1; i >= 0; i-- {
		p := int64(questions[i][0])
		if j := i + questions[i][1] + 1; j <= n {
			p += f[j]
		}
		f[i] = max(f[i+1], p)
	}
	return f[0]
}
```

#### TypeScript

```ts
function mostPoints(questions: number[][]): number {
    const n = questions.length;
    const f = Array(n + 1).fill(0);
    for (let i = n - 1; i >= 0; --i) {
        const [p, b] = questions[i];
        const j = i + b + 1;
        f[i] = Math.max(f[i + 1], p + (j > n ? 0 : f[j]));
    }
    return f[0];
}
```

#### Rust

```rust
impl Solution {
    pub fn most_points(questions: Vec<Vec<i32>>) -> i64 {
        let n = questions.len();
        let mut f = vec![0; n + 1];
        for i in (0..n).rev() {
            let p = questions[i][0] as i64;
            let b = questions[i][1] as usize;
            let j = i + b + 1;
            f[i] = f[i + 1].max(p + if j > n { 0 } else { f[j] });
        }
        f[0]
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
