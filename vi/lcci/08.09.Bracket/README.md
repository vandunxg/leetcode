---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [08.09. Bracket](https://leetcode.cn/problems/bracket-lcci)

[中文文档](/lcci/08.09.Bracket/README.md)

## Mô tả

<!-- description:start -->

<p>Hãy triển khai một thuật toán để in ra tất cả các tổ hợp hợp lệ (ví dụ: được mở và đóng đúng cách) của n cặp dấu ngoặc.</p>

<p>Lưu ý: Tập kết quả không được chứa các tập con trùng lặp.</p>

<p>Ví dụ, với&nbsp;n = 3, kết quả là:</p>

<pre>

[

  &quot;((()))&quot;,

  &quot;(()())&quot;,

  &quot;(())()&quot;,

  &quot;()(())&quot;,

  &quot;()()()&quot;

]

</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS + Cắt tỉa

<!-- thinking:start -->

> **Tư duy**
>
> Cần tìm tất cả các chuỗi hợp lệ gồm $n$ cặp dấu ngoặc. Vì $n\le 8$, ta có thể tạo $2^{2n}$ chuỗi rồi lọc, nhưng nhiều tiền tố đã không hợp lệ ngay từ đầu.
>
> Một tiền tố hợp lệ không bao giờ có số ngoặc phải nhiều hơn số ngoặc trái, và cả hai số đếm đều không vượt quá $n$.
>
> $dfs(l,r,t)$ cắt tỉa khi $l<r$ hoặc một số đếm vượt quá $n$, đồng thời ghi nhận khi $l=r=n$. Việc thử `'('` rồi `')'` chỉ tạo ra các chuỗi hợp lệ.

<!-- thinking:end -->

Miền giá trị của $n$ trong đề bài là $[1, 8]$, vì vậy chúng ta có thể trực tiếp giải bài toán này bằng "tìm kiếm vét cạn + cắt tỉa".

Chúng ta thiết kế một hàm `dfs(l, r, t)`, trong đó $l$ và $r$ lần lượt biểu thị số lượng ngoặc trái và ngoặc phải, còn $t$ biểu thị chuỗi ngoặc hiện tại. Khi đó, ta có thể nhận được cấu trúc đệ quy như sau:

- Nếu $l > n$ hoặc $r > n$ hoặc $l < r$, thì tổ hợp ngoặc hiện tại $t$ không hợp lệ, trả về trực tiếp;
- Nếu $l = n$ và $r = n$, thì tổ hợp ngoặc hiện tại $t$ hợp lệ, thêm nó vào mảng kết quả `ans`, rồi trả về trực tiếp;
- Ta có thể chọn thêm một ngoặc trái, rồi thực hiện đệ quy `dfs(l + 1, r, t + "(")`;
- Ta cũng có thể chọn thêm một ngoặc phải, rồi thực hiện đệ quy `dfs(l, r + 1, t + ")")`.

Độ phức tạp thời gian là $O(2^{n\times 2} \times n)$, và độ phức tạp không gian là $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        def dfs(l, r, t):
            if l > n or r > n or l < r:
                return
            if l == n and r == n:
                ans.append(t)
                return
            dfs(l + 1, r, t + '(')
            dfs(l, r + 1, t + ')')

        ans = []
        dfs(0, 0, '')
        return ans
```

#### Java

```java
class Solution {
    private List<String> ans = new ArrayList<>();
    private int n;

    public List<String> generateParenthesis(int n) {
        this.n = n;
        dfs(0, 0, "");
        return ans;
    }

    private void dfs(int l, int r, String t) {
        if (l > n || r > n || l < r) {
            return;
        }
        if (l == n && r == n) {
            ans.add(t);
            return;
        }
        dfs(l + 1, r, t + "(");
        dfs(l, r + 1, t + ")");
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> generateParenthesis(int n) {
        vector<string> ans;
        auto dfs = [&](this auto&& dfs, int l, int r, string t) {
            if (l > n || r > n || l < r) return;
            if (l == n && r == n) {
                ans.push_back(t);
                return;
            }
            dfs(l + 1, r, t + "(");
            dfs(l, r + 1, t + ")");
        };
        dfs(0, 0, "");
        return ans;
    }
};
```

#### Go

```go
func generateParenthesis(n int) []string {
	ans := []string{}
	var dfs func(int, int, string)
	dfs = func(l, r int, t string) {
		if l > n || r > n || l < r {
			return
		}
		if l == n && r == n {
			ans = append(ans, t)
			return
		}
		dfs(l+1, r, t+"(")
		dfs(l, r+1, t+")")
	}
	dfs(0, 0, "")
	return ans
}
```

#### TypeScript

```ts
function generateParenthesis(n: number): string[] {
    function dfs(l, r, t) {
        if (l > n || r > n || l < r) {
            return;
        }
        if (l == n && r == n) {
            ans.push(t);
            return;
        }
        dfs(l + 1, r, t + '(');
        dfs(l, r + 1, t + ')');
    }
    let ans = [];
    dfs(0, 0, '');
    return ans;
}
```

#### Rust

```rust
impl Solution {
    fn dfs(left: i32, right: i32, s: &mut String, res: &mut Vec<String>) {
        if left == 0 && right == 0 {
            res.push(s.clone());
            return;
        }
        if left > 0 {
            s.push('(');
            Self::dfs(left - 1, right, s, res);
            s.pop();
        }
        if right > left {
            s.push(')');
            Self::dfs(left, right - 1, s, res);
            s.pop();
        }
    }

    pub fn generate_parenthesis(n: i32) -> Vec<String> {
        let mut res = Vec::new();
        Self::dfs(n, n, &mut String::new(), &mut res);
        res
    }
}
```

#### JavaScript

```js
/**
 * @param {number} n
 * @return {string[]}
 */
var generateParenthesis = function (n) {
    function dfs(l, r, t) {
        if (l > n || r > n || l < r) {
            return;
        }
        if (l == n && r == n) {
            ans.push(t);
            return;
        }
        dfs(l + 1, r, t + '(');
        dfs(l, r + 1, t + ')');
    }
    let ans = [];
    dfs(0, 0, '');
    return ans;
};
```

#### Swift

```swift
class Solution {
    private var ans: [String] = []
    private var n: Int = 0

    func generateParenthesis(_ n: Int) -> [String] {
        self.n = n
        dfs(l: 0, r: 0, t: "")
        return ans
    }

    private func dfs(l: Int, r: Int, t: String) {
        if l > n || r > n || l < r {
            return
        }
        if l == n && r == n {
            ans.append(t)
            return
        }
        dfs(l: l + 1, r: r, t: t + "(")
        dfs(l: l, r: r + 1, t: t + ")")
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
