---
comments: true
difficulty: Medium
rating: 1352
source: Weekly Contest 405 Q2
tags:
    - Bit Manipulation
    - String
    - Backtracking
---

<!-- problem:start -->

# [3211. Generate Binary Strings Without Adjacent Zeros](https://leetcode.com/problems/generate-binary-strings-without-adjacent-zeros)

[Tài liệu tiếng Trung](/solution/3200-3299/3211.Generate%20Binary%20Strings%20Without%20Adjacent%20Zeros/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên dương <code>n</code>.</p>

<p>Một chuỗi nhị phân <code>x</code> là <strong>hợp lệ</strong> nếu mọi <span data-keyword="substring-nonempty">chuỗi con</span> có độ dài 2 của <code>x</code> đều chứa <strong>ít nhất</strong> một <code>&quot;1&quot;</code>.</p>

<p>Trả về tất cả các chuỗi <strong>hợp lệ</strong> có độ dài <code>n</code><strong>, </strong>theo <em>bất kỳ</em> thứ tự nào.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[&quot;010&quot;,&quot;011&quot;,&quot;101&quot;,&quot;110&quot;,&quot;111&quot;]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các chuỗi hợp lệ có độ dài 3 là: <code>&quot;010&quot;</code>, <code>&quot;011&quot;</code>, <code>&quot;101&quot;</code>, <code>&quot;110&quot;</code> và <code>&quot;111&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[&quot;0&quot;,&quot;1&quot;]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các chuỗi hợp lệ có độ dài 1 là: <code>&quot;0&quot;</code> và <code>&quot;1&quot;</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 18</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Vì $n\le 18$, có $2^n$ chuỗi nhị phân nên cách sinh tất cả rồi lọc vẫn phù hợp, nhưng không cần mở rộng các tiền tố đã vi phạm.
>
> Dùng DFS tại vị trí $i$ để thử $0$ hoặc $1$, và chỉ cho phép $0$ khi $i=0$ hoặc bit trước đó là $1$. Khi chuỗi có độ dài $n$ thì lưu lại. Sau khi cắt tỉa như vậy, cây tìm kiếm chính xác là tập các chuỗi hợp lệ.

<!-- thinking:end -->

Ta có thể duyệt từng vị trí $i$ của một chuỗi nhị phân có độ dài $n$, và với mỗi vị trí $i$, duyệt các giá trị khả dĩ $j$ mà vị trí đó có thể nhận. Nếu $j$ là $0$, ta cần kiểm tra xem vị trí trước đó có phải là $1$ hay không. Nếu nó là $1$, ta tiếp tục đệ quy; nếu không, chuỗi đó không hợp lệ. Nếu $j$ là $1$, ta trực tiếp tiếp tục đệ quy.

Độ phức tạp thời gian là $O(n \times 2^n)$, trong đó $n$ là độ dài chuỗi. Không tính phần bộ nhớ dùng cho mảng kết quả, độ phức tạp không gian là $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def validStrings(self, n: int) -> List[str]:
        def dfs(i: int):
            if i >= n:
                ans.append("".join(t))
                return
            for j in range(2):
                if (j == 0 and (i == 0 or t[i - 1] == "1")) or j == 1:
                    t.append(str(j))
                    dfs(i + 1)
                    t.pop()

        ans = []
        t = []
        dfs(0)
        return ans
```

#### Java

```java
class Solution {
    private List<String> ans = new ArrayList<>();
    private StringBuilder t = new StringBuilder();
    private int n;

    public List<String> validStrings(int n) {
        this.n = n;
        dfs(0);
        return ans;
    }

    private void dfs(int i) {
        if (i >= n) {
            ans.add(t.toString());
            return;
        }
        for (int j = 0; j < 2; ++j) {
            if ((j == 0 && (i == 0 || t.charAt(i - 1) == '1')) || j == 1) {
                t.append(j);
                dfs(i + 1);
                t.deleteCharAt(t.length() - 1);
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> validStrings(int n) {
        vector<string> ans;
        string t;
        auto dfs = [&](this auto&& dfs, int i) {
            if (i >= n) {
                ans.emplace_back(t);
                return;
            }
            for (int j = 0; j < 2; ++j) {
                if ((j == 0 && (i == 0 || t[i - 1] == '1')) || j == 1) {
                    t.push_back('0' + j);
                    dfs(i + 1);
                    t.pop_back();
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
func validStrings(n int) (ans []string) {
	t := []byte{}
	var dfs func(int)
	dfs = func(i int) {
		if i >= n {
			ans = append(ans, string(t))
			return
		}
		for j := 0; j < 2; j++ {
			if (j == 0 && (i == 0 || t[i-1] == '1')) || j == 1 {
				t = append(t, byte('0'+j))
				dfs(i + 1)
				t = t[:len(t)-1]
			}
		}
	}
	dfs(0)
	return
}
```

#### TypeScript

```ts
function validStrings(n: number): string[] {
    const ans: string[] = [];
    const t: string[] = [];
    const dfs = (i: number) => {
        if (i >= n) {
            ans.push(t.join(''));
            return;
        }
        for (let j = 0; j < 2; ++j) {
            if ((j == 0 && (i == 0 || t[i - 1] == '1')) || j == 1) {
                t.push(j.toString());
                dfs(i + 1);
                t.pop();
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
