---
comments: true
difficulty: Medium
tags:
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [2052. Minimum Cost to Separate Sentence Into Rows 🔒](https://leetcode.com/problems/minimum-cost-to-separate-sentence-into-rows)

[中文文档](/solution/2000-2099/2052.Minimum%20Cost%20to%20Separate%20Sentence%20Into%20Rows/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>sentence</code> chứa các từ được ngăn cách bằng dấu cách và một số nguyên <code>k</code>. Nhiệm vụ của bạn là tách <code>sentence</code> thành các <strong>dòng</strong> sao cho số ký tự trong mỗi dòng <strong>không vượt quá </strong><code>k</code>. Có thể giả sử rằng <code>sentence</code> không bắt đầu hoặc kết thúc bằng dấu cách, và các từ trong <code>sentence</code> được ngăn cách bằng một dấu cách.</p>

<p>Bạn có thể tách <code>sentence</code> thành các dòng bằng cách chèn ký tự xuống dòng giữa các từ trong <code>sentence</code>. Một từ <strong>không thể</strong> bị tách thành hai dòng. Mỗi từ phải được sử dụng đúng một lần và không được thay đổi thứ tự các từ. Các từ liền kề trong một dòng phải được ngăn cách bằng một dấu cách, và các dòng không được bắt đầu hoặc kết thúc bằng dấu cách.</p>

<p><strong>Chi phí</strong> của một dòng có độ dài <code>n</code> là <code>(k - n)<sup>2</sup></code>, và <strong>tổng chi phí</strong> là tổng <strong>chi phí</strong> của tất cả các dòng <strong>ngoại trừ</strong> dòng cuối cùng.</p>

<ul>
	<li>Ví dụ, nếu <code>sentence = &quot;i love leetcode&quot;</code> và <code>k = 12</code>:

    <ul>
    <li>Tách <code>sentence</code> thành <code>&quot;i&quot;</code>, <code>&quot;love&quot;</code> và <code>&quot;leetcode&quot;</code> có chi phí là <code>(12 - 1)<sup>2</sup> + (12 - 4)<sup>2</sup> = 185</code>.</li>
    <li>Tách <code>sentence</code> thành <code>&quot;i love&quot;</code> và <code>&quot;leetcode&quot;</code> có chi phí là <code>(12 - 6)<sup>2</sup> = 36</code>.</li>
    <li>Không thể tách <code>sentence</code> thành <code>&quot;i&quot;</code> và <code>&quot;love leetcode&quot;</code> vì độ dài của <code>&quot;love leetcode&quot;</code> lớn hơn <code>k</code>.</li>
    </ul>
    </li>

</ul>

<p>Trả về <em><strong>chi phí nhỏ nhất</strong> có thể đạt được khi tách</em><em> </em><code>sentence</code><em> thành các dòng.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> sentence = &quot;i love leetcode&quot;, k = 12
<strong>Đầu ra:</strong> 36
<strong>Giải thích:</strong>
Tách sentence thành &quot;i&quot;, &quot;love&quot; và &quot;leetcode&quot; có chi phí là (12 - 1)<sup>2</sup> + (12 - 4)<sup>2</sup> = 185.
Tách sentence thành &quot;i love&quot; và &quot;leetcode&quot; có chi phí là (12 - 6)<sup>2</sup> = 36.
Không thể tách sentence thành &quot;i&quot; và &quot;love leetcode&quot; vì &quot;love leetcode&quot; có độ dài 13.
36 là tổng chi phí nhỏ nhất có thể đạt được, nên trả về giá trị này.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> sentence = &quot;apples and bananas taste great&quot;, k = 7
<strong>Đầu ra:</strong> 21
<strong>Giải thích</strong>
Tách sentence thành &quot;apples&quot;, &quot;and&quot;, &quot;bananas&quot;, &quot;taste&quot; và &quot;great&quot; có chi phí là (7 - 6)<sup>2</sup> + (7 - 3)<sup>2</sup> + (7 - 7)<sup>2</sup> + (7 - 5)<sup>2 </sup>= 21.
21 là tổng chi phí nhỏ nhất có thể đạt được, nên trả về giá trị này.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> sentence = &quot;a&quot;, k = 5
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>
Chi phí của dòng cuối không được tính vào tổng chi phí. Vì chỉ có một dòng nên trả về 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= sentence.length &lt;= 5000</code></li>
	<li><code>1 &lt;= k &lt;= 5000</code></li>
	<li>Độ dài của mỗi từ trong <code>sentence</code> không vượt quá <code>k</code>.</li>
	<li><code>sentence</code> chỉ bao gồm các chữ cái tiếng Anh viết thường và dấu cách.</li>
	<li><code>sentence</code> không bắt đầu hoặc kết thúc bằng dấu cách.</li>
	<li>Các từ trong <code>sentence</code> được ngăn cách bằng một dấu cách.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mảng tổng tiền tố + Tìm kiếm có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Các từ được xếp vào các dòng; mọi dòng trừ dòng cuối có chi phí $(k-\textit{width})^2$. Số cách ngắt dòng tăng theo cấp số mũ, nhưng chi phí nhỏ nhất khi bắt đầu từ từ $i$ chỉ phụ thuộc vào hậu tố còn lại.
>
> Mảng tổng tiền tố cho phép tính độ dài một đoạn trong $O(1)$. Nếu phần còn lại vừa với dòng cuối, chi phí là $0$; nếu không, thử điểm ngắt tiếp theo $j$ và cộng $(k-m)^2+dfs(j)$.
>
> Ghi nhớ tạo ra $n$ trạng thái, mỗi trạng thái có $O(n)$ chuyển trạng thái.

<!-- thinking:end -->

Ta dùng một mảng $\textit{nums}$ để lưu độ dài của mỗi từ, với độ dài mảng là $n$. Sau đó, ta xây dựng mảng tổng tiền tố $\textit{s}$ có độ dài $n + 1$, trong đó $\textit{s}[i]$ biểu diễn tổng độ dài của $i$ từ đầu tiên.

Tiếp theo, ta thiết kế hàm $\textit{dfs}(i)$, biểu diễn chi phí nhỏ nhất khi tách chuỗi bắt đầu từ từ thứ $i$. Đáp án là $\textit{dfs}(0)$.

Quá trình thực thi hàm $\textit{dfs}(i)$ như sau:

- Nếu tổng độ dài của các từ bắt đầu từ từ thứ $i$ đến từ cuối cùng, cộng với số dấu cách giữa các từ, nhỏ hơn hoặc bằng $k$, thì các từ này có thể được đặt trên dòng cuối và chi phí là $0$.
- Nếu không, ta liệt kê vị trí $j$ của từ bắt đầu dòng tiếp theo, sao cho tổng độ dài của các từ từ thứ $i$ đến từ thứ $(j-1)$, cộng với số dấu cách giữa chúng, nhỏ hơn hoặc bằng $k$. Khi đó, $\textit{dfs}(j)$ biểu diễn chi phí nhỏ nhất khi tách chuỗi bắt đầu từ từ thứ $j$, còn $(k - m)^2$ là chi phí đặt các từ từ thứ $i$ đến từ thứ $(j-1)$ trên một dòng, trong đó $m$ là tổng độ dài của các từ từ thứ $i$ đến từ thứ $(j-1)$, cộng với số dấu cách giữa chúng. Ta liệt kê mọi $j$ và lấy giá trị nhỏ nhất.

Đáp án là $\textit{dfs}(0)$.

Để tránh tính toán lặp lại, ta có thể sử dụng tìm kiếm có ghi nhớ.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là số từ.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumCost(self, sentence: str, k: int) -> int:
        @cache
        def dfs(i: int) -> int:
            if s[n] - s[i] + n - i - 1 <= k:
                return 0
            ans = inf
            j = i + 1
            while j < n and (m := s[j] - s[i] + j - i - 1) <= k:
                ans = min(ans, dfs(j) + (k - m) ** 2)
                j += 1
            return ans

        nums = [len(s) for s in sentence.split()]
        n = len(nums)
        s = list(accumulate(nums, initial=0))
        return dfs(0)
```

#### Java

```java
class Solution {
    private Integer[] f;
    private int[] s;
    private int k;
    private int n;

    public int minimumCost(String sentence, int k) {
        this.k = k;
        String[] words = sentence.split(" ");
        n = words.length;
        f = new Integer[n];
        s = new int[n + 1];
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + words[i].length();
        }
        return dfs(0);
    }

    private int dfs(int i) {
        if (s[n] - s[i] + n - i - 1 <= k) {
            return 0;
        }
        if (f[i] != null) {
            return f[i];
        }
        int ans = Integer.MAX_VALUE;
        for (int j = i + 1; j < n && s[j] - s[i] + j - i - 1 <= k; ++j) {
            int m = s[j] - s[i] + j - i - 1;
            ans = Math.min(ans, dfs(j) + (k - m) * (k - m));
        }
        return f[i] = ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumCost(string sentence, int k) {
        istringstream iss(sentence);
        vector<int> s = {0};
        string w;
        while (iss >> w) {
            s.push_back(s.back() + w.size());
        }
        int n = s.size() - 1;
        int f[n];
        memset(f, -1, sizeof(f));
        auto dfs = [&](this auto&& dfs, int i) -> int {
            if (s[n] - s[i] + n - i - 1 <= k) {
                return 0;
            }
            if (f[i] != -1) {
                return f[i];
            }
            int ans = INT_MAX;
            for (int j = i + 1; j < n && s[j] - s[i] + j - i - 1 <= k; ++j) {
                int m = s[j] - s[i] + j - i - 1;
                ans = min(ans, dfs(j) + (k - m) * (k - m));
            }
            return f[i] = ans;
        };
        return dfs(0);
    }
};
```

#### Go

```go
func minimumCost(sentence string, k int) int {
	s := []int{0}
	for _, w := range strings.Split(sentence, " ") {
		s = append(s, s[len(s)-1]+len(w))
	}
	n := len(s) - 1
	f := make([]int, n)
	for i := range f {
		f[i] = -1
	}
	var dfs func(int) int
	dfs = func(i int) int {
		if s[n]-s[i]+n-i-1 <= k {
			return 0
		}
		if f[i] != -1 {
			return f[i]
		}
		ans := math.MaxInt32
		for j := i + 1; j < n && s[j]-s[i]+j-i-1 <= k; j++ {
			m := s[j] - s[i] + j - i - 1
			ans = min(ans, dfs(j)+(k-m)*(k-m))
		}
		f[i] = ans
		return ans
	}
	return dfs(0)
}
```

#### TypeScript

```ts
function minimumCost(sentence: string, k: number): number {
    const s: number[] = [0];
    for (const w of sentence.split(' ')) {
        s.push(s.at(-1)! + w.length);
    }
    const n = s.length - 1;
    const f: number[] = Array(n).fill(-1);
    const dfs = (i: number): number => {
        if (s[n] - s[i] + n - i - 1 <= k) {
            return 0;
        }
        if (f[i] !== -1) {
            return f[i];
        }
        let ans = Infinity;
        for (let j = i + 1; j < n && s[j] - s[i] + j - i - 1 <= k; ++j) {
            const m = s[j] - s[i] + j - i - 1;
            ans = Math.min(ans, dfs(j) + (k - m) ** 2);
        }
        return (f[i] = ans);
    };
    return dfs(0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
