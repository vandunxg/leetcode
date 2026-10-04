---
comments: true
difficulty: Medium
tags:
    - Array
    - Backtracking
---

<!-- problem:start -->

# [3437. Permutations III 🔒](https://leetcode.com/problems/permutations-iii)

[中文文档](/solution/3400-3499/3437.Permutations%20III/README.md)

## Mô tả

<!-- description:start -->

<p>Với một số nguyên <code>n</code>, <strong>hoán vị xen kẽ</strong> là một hoán vị của <code>n</code> số nguyên dương đầu tiên sao cho không có <strong>hai</strong> phần tử kề nhau nào <strong>đồng thời</strong> là số lẻ hoặc <strong>đồng thời</strong> là số chẵn.</p>

<p>Hãy trả về <em>toàn bộ các </em><strong>hoán vị xen kẽ</strong> như vậy theo thứ tự từ điển.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[[1,2,3,4],[1,4,3,2],[2,1,4,3],[2,3,4,1],[3,2,1,4],[3,4,1,2],[4,1,2,3],[4,3,2,1]]</span></p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[[1,2],[2,1]]</span></p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[[1,2,3],[3,2,1]]</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= n &lt;= 10</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quay lui

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tạo các hoán vị của $1..n$ sao cho các giá trị kề nhau có tính chẵn lẻ trái ngược. Vì $n\le 10$, có thể tìm kiếm toàn bộ nếu cắt tỉa các tiền tố có hai số cùng tính chẵn lẻ.
>
> Quay lui điền lần lượt các vị trí, còn mảng đánh dấu giúp mỗi số chỉ được dùng một lần.
>
> Bỏ qua một ứng viên nếu nó có cùng tính chẵn lẻ với giá trị được chọn gần nhất. Vị trí đầu tiên không có phần tử đứng trước. Khi $i=n$, lưu một bản sao của hoán vị hiện tại.

<!-- thinking:end -->

Ta xây dựng hàm $\textit{dfs}(i)$, đại diện cho việc điền vị trí thứ $i$, với chỉ số vị trí bắt đầu từ $0$.

Trong $\textit{dfs}(i)$, nếu $i \geq n$, nghĩa là tất cả vị trí đã được điền, ta thêm hoán vị hiện tại vào mảng kết quả.

Nếu chưa, ta duyệt các số $j$ có thể được đặt vào vị trí hiện tại. Nếu $j$ chưa được sử dụng và $j$ có tính chẵn lẻ khác với số cuối cùng trong hoán vị hiện tại, ta có thể đặt $j$ vào vị trí hiện tại rồi tiếp tục điền đệ quy vị trí tiếp theo.

Độ phức tạp thời gian là $O(n \times n!)$, còn độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của hoán vị.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def permute(self, n: int) -> List[List[int]]:
        def dfs(i: int) -> None:
            if i >= n:
                ans.append(t[:])
                return
            for j in range(1, n + 1):
                if not vis[j] and (i == 0 or t[-1] % 2 != j % 2):
                    t.append(j)
                    vis[j] = True
                    dfs(i + 1)
                    vis[j] = False
                    t.pop()

        ans = []
        t = []
        vis = [False] * (n + 1)
        dfs(0)
        return ans
```

#### Java

```java
class Solution {
    private List<int[]> ans = new ArrayList<>();
    private boolean[] vis;
    private int[] t;
    private int n;

    public int[][] permute(int n) {
        this.n = n;
        t = new int[n];
        vis = new boolean[n + 1];
        dfs(0);
        return ans.toArray(new int[0][]);
    }

    private void dfs(int i) {
        if (i >= n) {
            ans.add(t.clone());
            return;
        }
        for (int j = 1; j <= n; ++j) {
            if (!vis[j] && (i == 0 || t[i - 1] % 2 != j % 2)) {
                vis[j] = true;
                t[i] = j;
                dfs(i + 1);
                vis[j] = false;
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> permute(int n) {
        vector<vector<int>> ans;
        vector<bool> vis(n);
        vector<int> t;
        auto dfs = [&](this auto&& dfs, int i) -> void {
            if (i >= n) {
                ans.push_back(t);
                return;
            }
            for (int j = 1; j <= n; ++j) {
                if (!vis[j] && (i == 0 || t[i - 1] % 2 != j % 2)) {
                    vis[j] = true;
                    t.push_back(j);
                    dfs(i + 1);
                    t.pop_back();
                    vis[j] = false;
                }
            }
        };
        dfs(0);
        return ans;
    }
};
```

#### Go

```go
func permute(n int) (ans [][]int) {
    vis := make([]bool, n+1)
    t := make([]int, n)
    var dfs func(i int)
    dfs = func(i int) {
        if i >= n {
            ans = append(ans, slices.Clone(t))
            return
        }
        for j := 1; j <= n; j++ {
            if !vis[j] && (i == 0 || t[i-1]%2 != j%2) {
                vis[j] = true
                t[i] = j
                dfs(i + 1)
                vis[j] = false
            }
        }
    }
    dfs(0)
    return
}
```

#### TypeScript

```ts
function permute(n: number): number[][] {
    const ans: number[][] = [];
    const vis: boolean[] = Array(n).fill(false);
    const t: number[] = Array(n).fill(0);
    const dfs = (i: number) => {
        if (i >= n) {
            ans.push([...t]);
            return;
        }
        for (let j = 1; j <= n; ++j) {
            if (!vis[j] && (i === 0 || t[i - 1] % 2 !== j % 2)) {
                vis[j] = true;
                t[i] = j;
                dfs(i + 1);
                vis[j] = false;
            }
        }
    };
    dfs(0);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
