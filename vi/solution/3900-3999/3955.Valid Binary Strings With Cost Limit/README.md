---
comments: true
difficulty: Medium
rating: 1429
source: Weekly Contest 505 Q2
tags:
    - Bit Manipulation
    - String
    - Backtracking
    - Enumeration
---

<!-- problem:start -->

# [3955. Valid Binary Strings With Cost Limit](https://leetcode.com/problems/valid-binary-strings-with-cost-limit)

[中文文档](/solution/3900-3999/3955.Valid%20Binary%20Strings%20With%20Cost%20Limit/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên <code>n</code> và <code>k</code>.</p>

<p><strong>Chi phí</strong> của một chuỗi nhị phân <code>s</code> được định nghĩa là tổng tất cả các chỉ số <code>i</code> (đánh số từ 0) sao cho <code>s[i] == &#39;1&#39;</code>.</p>

<p>Một chuỗi nhị phân được xem là <strong>hợp lệ</strong> nếu:</p>

<ul>
	<li>Không chứa hai ký tự <code>&#39;1&#39;</code> liên tiếp.</li>
	<li>Chi phí <strong>nhỏ hơn hoặc bằng</strong> <code>k</code>.</li>
</ul>

<p>Trả về danh sách tất cả các chuỗi nhị phân hợp lệ có độ dài <code>n</code>, theo bất kỳ thứ tự nào.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[&quot;000&quot;,&quot;010&quot;,&quot;100&quot;]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các chuỗi nhị phân độ dài 3 không có hai ký tự <code>&#39;1&#39;</code> liên tiếp là:</p>

<ul>
	<li><code>&quot;000&quot;</code> : <code>cost = 0</code></li>
	<li>&quot;<code>100&quot;</code> : <code>cost = 0</code></li>
	<li><code>&quot;010&quot;</code> : <code>cost = 1</code></li>
	<li><code>&quot;001&quot;</code> : <code>cost = 2</code></li>
	<li><code>&quot;101&quot;</code> : <code>cost = 0 + 2 = 2</code></li>
</ul>

<p>Trong số này, các chuỗi có chi phí nhỏ hơn hoặc bằng <code>k = 1</code> là <code>&quot;000&quot;</code>, <code>&quot;010&quot;</code> và <code>&quot;100&quot;</code>.</p>

<p>Vì vậy, các chuỗi hợp lệ là <code>[&quot;000&quot;, &quot;010&quot;, &quot;100&quot;]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 1, k = 0</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[&quot;0&quot;,&quot;1&quot;]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các chuỗi nhị phân hợp lệ độ dài 1 là <code>&quot;0&quot;</code> và <code>&quot;1&quot;</code>.</p>

<p>Vì vậy, đáp án là <code>[&quot;0&quot;, &quot;1&quot;]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 12</code></li>
	<li><code>0 &lt;= k &lt;= n * (n - 1) / 2</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Vì $n\le 12$, có $2^n$ chuỗi, nhưng ta còn cần không có hai số 1 kề nhau và tổng các chỉ số của số 1 $\le k$. DFS theo vị trí sẽ liệt kê mọi chuỗi hợp lệ.
>
> Ở mỗi vị trí, ta luôn có thể chọn $0$; chỉ chọn $1$ khi bit trước đó không phải là $1$ và $tot+i\le k$. Khi đạt độ dài $n$, ta lưu đường đi hiện tại.
>
> Ràng buộc tập độc lập giúp không gian tìm kiếm nhỏ hơn nhiều so với $2^n$.

<!-- thinking:end -->

Ta muốn tạo các chuỗi nhị phân độ dài $n$ thỏa mãn các điều kiện sau:

- Tổng các vị trí $i$ (đánh số từ 0) của mỗi `1` không vượt quá $k$, có thể biểu diễn như sau:

$$
\sum_{i \mid s_i = 1} i \le k
$$

- Không có hai `1` nào đứng cạnh nhau.

Vì vậy, ta thiết kế hàm đệ quy $\text{dfs}(i, tot)$, trong đó:

- $i$ biểu thị vị trí hiện tại đang được xử lý trong chuỗi;
- $tot$ biểu thị tổng các chỉ số của tất cả `1` đã đặt.

#### Logic đệ quy

**1. Trường hợp cơ sở (điều kiện kết thúc)**

Khi $i \ge n$, nghĩa là đã tạo xong một chuỗi độ dài $n$. Lúc này, thêm đường đi hiện tại vào danh sách đáp án.

**2. Chọn `0`**

Ta luôn có thể đặt `0` ở vị trí hiện tại. Gọi đệ quy $\text{dfs}(i + 1, tot)$. Vì đặt `0`, tổng $tot$ không thay đổi.

**3. Chọn `1`**

Chỉ có thể đặt `1` ở vị trí hiện tại nếu thỏa mãn cả hai điều kiện sau: ký tự trước đó không tồn tại (hoặc là `0`), và $tot + i \le k$. Khi đó, ta gọi đệ quy $\text{dfs}(i + 1, tot + i)$.

**4. Quay lui**

Sau khi mỗi lời gọi đệ quy kết thúc, ta hoàn tác lựa chọn hiện tại để khôi phục trạng thái trước khi đi vào đệ quy, cho phép thuật toán khám phá các tổ hợp khác.

Độ phức tạp thời gian là $O(n \times 2^n)$ và độ phức tạp không gian là $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def generateValidStrings(self, n: int, k: int) -> list[str]:
        def dfs(i: int, tot: int):
            if i >= n:
                ans.append("".join(path))
                return
            path.append("0")
            dfs(i + 1, tot)
            path.pop()
            if (not path or path[-1] == "0") and tot + i <= k:
                path.append("1")
                dfs(i + 1, tot + i)
                path.pop()

        ans = []
        path = []
        dfs(0, 0)
        return ans
```

#### Java

```java
class Solution {
    private int n;
    private int k;
    private List<String> ans;
    private StringBuilder path;

    public List<String> generateValidStrings(int n, int k) {
        this.n = n;
        this.k = k;
        ans = new ArrayList<>();
        path = new StringBuilder();

        dfs(0, 0);

        return ans;
    }

    private void dfs(int i, int tot) {
        if (i >= n) {
            ans.add(path.toString());
            return;
        }

        path.append('0');
        dfs(i + 1, tot);
        path.deleteCharAt(path.length() - 1);

        if ((path.isEmpty() || path.charAt(path.length() - 1) == '0') && tot + i <= k) {
            path.append('1');
            dfs(i + 1, tot + i);
            path.deleteCharAt(path.length() - 1);
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> generateValidStrings(int n, int k) {
        vector<string> ans;
        string path;

        auto dfs = [&](this auto&& dfs, int i, int tot) -> void {
            if (i >= n) {
                ans.push_back(path);
                return;
            }

            path.push_back('0');
            dfs(i + 1, tot);
            path.pop_back();

            if ((path.empty() || path.back() == '0') && tot + i <= k) {
                path.push_back('1');
                dfs(i + 1, tot + i);
                path.pop_back();
            }
        };

        dfs(0, 0);

        return ans;
    }
};
```

#### Go

```go
func generateValidStrings(n int, k int) []string {
	ans := []string{}
	path := make([]byte, 0, n)

	var dfs func(int, int)
	dfs = func(i, tot int) {
		if i >= n {
			ans = append(ans, string(path))
			return
		}

		path = append(path, '0')
		dfs(i+1, tot)
		path = path[:len(path)-1]

		if (len(path) == 0 || path[len(path)-1] == '0') && tot+i <= k {
			path = append(path, '1')
			dfs(i+1, tot+i)
			path = path[:len(path)-1]
		}
	}

	dfs(0, 0)

	return ans
}
```

#### TypeScript

```ts
function generateValidStrings(n: number, k: number): string[] {
    const ans: string[] = [];
    const path: string[] = [];

    const dfs = (i: number, tot: number): void => {
        if (i >= n) {
            ans.push(path.join(''));
            return;
        }

        path.push('0');
        dfs(i + 1, tot);
        path.pop();

        if ((path.length === 0 || path[path.length - 1] === '0') && tot + i <= k) {
            path.push('1');
            dfs(i + 1, tot + i);
            path.pop();
        }
    };

    dfs(0, 0);

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
