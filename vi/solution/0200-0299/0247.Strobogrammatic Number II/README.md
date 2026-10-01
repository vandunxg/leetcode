---
comments: true
difficulty: Medium
tags:
    - Recursion
    - Array
    - String
---

<!-- problem:start -->

# [247. Strobogrammatic Number II 🔒](https://leetcode.com/problems/strobogrammatic-number-ii)

[中文文档](/solution/0200-0299/0247.Strobogrammatic%20Number%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên <code>n</code>, hãy trả về tất cả <strong>số strobogrammatic</strong> có độ dài <code>n</code>. Bạn có thể trả lời theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p><strong>Số strobogrammatic</strong> là số vẫn giữ nguyên khi xoay <code>180</code> độ (khi nhìn lộn ngược).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> n = 2
<strong>Đầu ra:</strong> ["11","69","88","96"]
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> n = 1
<strong>Đầu ra:</strong> ["0","1","8"]
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 14</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Số strobogrammatic độ dài $n$ được tạo bằng cách thêm một cặp chữ số đối xứng vào hai đầu của chuỗi hợp lệ ngắn hơn. Với độ dài $1$, các số hợp lệ là $0,1,8$; với độ dài $0$, ta có chuỗi rỗng.
>
> $dfs(u)$ thêm các cặp $11,88,69,96$ bao quanh kết quả của $dfs(u-2)$; chỉ thêm cặp $00$ khi $u\neq n$ để số hoàn chỉnh không bắt đầu bằng số $0$.

<!-- thinking:end -->

Nếu độ dài là $1$, các số strobogrammatic duy nhất là $0, 1, 8$; nếu độ dài là $2$, các số duy nhất là $11, 69, 88, 96$.

Ta xây dựng hàm đệ quy $dfs(u)$ để trả về các số strobogrammatic có độ dài $u$. Kết quả cần tìm là $dfs(n)$.

Nếu $u$ bằng $0$, trả về danh sách chỉ chứa chuỗi rỗng, tức `[""]`; nếu $u$ bằng $1$, trả về danh sách `["0", "1", "8"]`.

Nếu $u$ lớn hơn $1$, ta duyệt tất cả số strobogrammatic có độ dài $u - 2$. Với mỗi số strobogrammatic $v$, ta lần lượt thêm các cặp $11, 88, 69, 96$ vào hai đầu để tạo ra các số strobogrammatic có độ dài `u`.

Lưu ý rằng nếu $u \neq n$, ta cũng có thể thêm chữ số $0$ vào cả hai đầu của số strobogrammatic.

Cuối cùng, trả về tất cả số strobogrammatic có độ dài $n$.

Độ phức tạp thời gian là $O(2^{n+2})$.

Các bài tương tự:

- [248. Strobogrammatic Number III 🔒](https://github.com/doocs/leetcode/blob/main/solution/0200-0299/0248.Strobogrammatic%20Number%20III/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findStrobogrammatic(self, n: int) -> List[str]:
        def dfs(u):
            if u == 0:
                return ['']
            if u == 1:
                return ['0', '1', '8']
            ans = []
            for v in dfs(u - 2):
                for l, r in ('11', '88', '69', '96'):
                    ans.append(l + v + r)
                if u != n:
                    ans.append('0' + v + '0')
            return ans

        return dfs(n)
```

#### Java

```java
class Solution {
    private static final int[][] PAIRS = {{1, 1}, {8, 8}, {6, 9}, {9, 6}};
    private int n;

    public List<String> findStrobogrammatic(int n) {
        this.n = n;
        return dfs(n);
    }

    private List<String> dfs(int u) {
        if (u == 0) {
            return Collections.singletonList("");
        }
        if (u == 1) {
            return Arrays.asList("0", "1", "8");
        }
        List<String> ans = new ArrayList<>();
        for (String v : dfs(u - 2)) {
            for (var p : PAIRS) {
                ans.add(p[0] + v + p[1]);
            }
            if (u != n) {
                ans.add(0 + v + 0);
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    const vector<pair<char, char>> pairs = {{'1', '1'}, {'8', '8'}, {'6', '9'}, {'9', '6'}};

    vector<string> findStrobogrammatic(int n) {
        function<vector<string>(int)> dfs = [&](int u) {
            if (u == 0) return vector<string>{""};
            if (u == 1) return vector<string>{"0", "1", "8"};
            vector<string> ans;
            for (auto& v : dfs(u - 2)) {
                for (auto& [l, r] : pairs) ans.push_back(l + v + r);
                if (u != n) ans.push_back('0' + v + '0');
            }
            return ans;
        };
        return dfs(n);
    }
};
```

#### Go

```go
func findStrobogrammatic(n int) []string {
	var dfs func(int) []string
	dfs = func(u int) []string {
		if u == 0 {
			return []string{""}
		}
		if u == 1 {
			return []string{"0", "1", "8"}
		}
		var ans []string
		pairs := [][]string{{"1", "1"}, {"8", "8"}, {"6", "9"}, {"9", "6"}}
		for _, v := range dfs(u - 2) {
			for _, p := range pairs {
				ans = append(ans, p[0]+v+p[1])
			}
			if u != n {
				ans = append(ans, "0"+v+"0")
			}
		}
		return ans
	}
	return dfs(n)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
