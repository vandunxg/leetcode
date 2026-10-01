---
comments: true
difficulty: Medium
tags:
    - Backtracking
    - Prime Factorization
---

<!-- problem:start -->

# [254. Factor Combinations 🔒](https://leetcode.com/problems/factor-combinations)

[中文文档](/solution/0200-0299/0254.Factor%20Combinations/README.md)

## Mô tả

<!-- description:start -->

<p>Một số có thể được biểu diễn thành tích các thừa số của nó.</p>

<ul>
	<li>Ví dụ, <code>8 = 2 x 2 x 2 = 2 x 4</code>.</li>
</ul>

<p>Cho số nguyên <code>n</code>, hãy trả về <em>tất cả các tổ hợp thừa số có thể có của nó</em>. Bạn có thể trả về đáp án theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p><strong>Lưu ý</strong> rằng các thừa số phải nằm trong khoảng <code>[2, n - 1]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1
<strong>Đầu ra:</strong> []
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 12
<strong>Đầu ra:</strong> [[2,6],[3,4],[2,2,3]]
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 37
<strong>Đầu ra:</strong> []
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>7</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Để tránh các hoán vị trùng nhau, ta tạo các cách phân tích $n$ thành ít nhất hai thừa số theo thứ tự không giảm. Duyệt các thừa số từ giá trị nhỏ nhất hiện tại $i$ đến $\sqrt{n}$.
>
> $dfs(n,i)$ ghi nhận các thừa số đã chọn cùng với phần $n$ còn lại, sau đó thử từng $j\ge i$ sao cho $j$ là ước của $n$ và gọi đệ quy với $n/j$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getFactors(self, n: int) -> List[List[int]]:
        def dfs(n, i):
            if t:
                ans.append(t + [n])
            j = i
            while j * j <= n:
                if n % j == 0:
                    t.append(j)
                    dfs(n // j, j)
                    t.pop()
                j += 1

        t = []
        ans = []
        dfs(n, 2)
        return ans
```

#### Java

```java
class Solution {
    private List<Integer> t = new ArrayList<>();
    private List<List<Integer>> ans = new ArrayList<>();

    public List<List<Integer>> getFactors(int n) {
        dfs(n, 2);
        return ans;
    }

    private void dfs(int n, int i) {
        if (!t.isEmpty()) {
            List<Integer> cp = new ArrayList<>(t);
            cp.add(n);
            ans.add(cp);
        }
        for (int j = i; j <= n / j; ++j) {
            if (n % j == 0) {
                t.add(j);
                dfs(n / j, j);
                t.remove(t.size() - 1);
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> getFactors(int n) {
        vector<int> t;
        vector<vector<int>> ans;
        function<void(int, int)> dfs = [&](int n, int i) {
            if (t.size()) {
                vector<int> cp = t;
                cp.emplace_back(n);
                ans.emplace_back(cp);
            }
            for (int j = i; j <= n / j; ++j) {
                if (n % j == 0) {
                    t.emplace_back(j);
                    dfs(n / j, j);
                    t.pop_back();
                }
            }
        };
        dfs(n, 2);
        return ans;
    }
};
```

#### Go

```go
func getFactors(n int) [][]int {
	t := []int{}
	ans := [][]int{}
	var dfs func(n, i int)
	dfs = func(n, i int) {
		if len(t) > 0 {
			ans = append(ans, append(slices.Clone(t), n))
		}
		for j := i; j <= n/j; j++ {
			if n%j == 0 {
				t = append(t, j)
				dfs(n/j, j)
				t = t[:len(t)-1]
			}
		}
	}
	dfs(n, 2)
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
