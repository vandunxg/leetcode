---
comments: true
difficulty: Hard
tags:
    - Recursion
    - Array
    - String
---

<!-- problem:start -->

# [248. Strobogrammatic Number III 🔒](https://leetcode.com/problems/strobogrammatic-number-iii)

[中文文档](/solution/0200-0299/0248.Strobogrammatic%20Number%20III/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi low và high biểu diễn hai số nguyên <code>low</code> và <code>high</code>, với <code>low &lt;= high</code>. Hãy trả về <em>số lượng <strong>số strobogrammatic</strong> trong đoạn</em> <code>[low, high]</code>.</p>

<p><strong>Số strobogrammatic</strong> là số vẫn giống như cũ khi xoay <code>180</code> độ (nhìn lộn ngược).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> low = "50", high = "100"
<strong>Đầu ra:</strong> 3
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> low = "0", high = "0"
<strong>Đầu ra:</strong> 1
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= low.length, high.length &lt;= 15</code></li>
	<li><code>low</code> và <code>high</code> chỉ gồm các chữ số.</li>
	<li><code>low &lt;= high</code></li>
	<li><code>low</code> và <code>high</code> không có số 0 ở đầu, trừ trường hợp giá trị là chính số 0.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Kiểm tra lần lượt mọi số nguyên trong đoạn sẽ chậm. Tương tự bài trước, ta tạo các số strobogrammatic theo độ dài, rồi giữ lại những số nằm trong đoạn $[\textit{low},\textit{high}]$.
>
> Duyệt các độ dài từ $|\textit{low}|$ đến $|\textit{high}|$ và so sánh các số nguyên.

<!-- thinking:end -->

Nếu độ dài là $1$, các số strobogrammatic chỉ có $0, 1, 8$; nếu độ dài là $2$, chúng chỉ có $11, 69, 88, 96$.

Ta thiết kế hàm đệ quy $dfs(u)$ để trả về các số strobogrammatic có độ dài $u$.

Nếu $u$ bằng $0$, trả về danh sách chứa chuỗi rỗng, tức `[""]`; nếu $u$ bằng $1$, trả về danh sách `["0", "1", "8"]`.

Nếu $u$ lớn hơn $1$, ta duyệt tất cả số strobogrammatic có độ dài $u - 2$. Với mỗi số strobogrammatic $v$, ta thêm lần lượt $1, 8, 6, 9$ vào hai phía của nó để tạo ra các số strobogrammatic có độ dài $u$.

Lưu ý rằng nếu $u \neq n$, ta cũng có thể thêm $0$ vào hai phía của số strobogrammatic.

Gọi độ dài của $low$ và $high$ lần lượt là $a$ và $b$.

Tiếp theo, ta duyệt mọi độ dài trong đoạn $[a,..b]$. Với mỗi độ dài $n$, ta lấy tất cả số strobogrammatic từ $dfs(n)$, rồi kiểm tra xem chúng có nằm trong đoạn $[low, high]$ hay không. Nếu có, ta tăng đáp án lên một.

Độ phức tạp thời gian là $O(2^{n+2} \times \log n)$.

Các bài tương tự:

- [247. Strobogrammatic Number II](https://github.com/doocs/leetcode/blob/main/solution/0200-0299/0247.Strobogrammatic%20Number%20II/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def strobogrammaticInRange(self, low: str, high: str) -> int:
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

        a, b = len(low), len(high)
        low, high = int(low), int(high)
        ans = 0
        for n in range(a, b + 1):
            for s in dfs(n):
                if low <= int(s) <= high:
                    ans += 1
        return ans
```

#### Java

```java
class Solution {
    private static final int[][] PAIRS = {{1, 1}, {8, 8}, {6, 9}, {9, 6}};
    private int n;

    public int strobogrammaticInRange(String low, String high) {
        int a = low.length(), b = high.length();
        long l = Long.parseLong(low), r = Long.parseLong(high);
        int ans = 0;
        for (n = a; n <= b; ++n) {
            for (String s : dfs(n)) {
                long v = Long.parseLong(s);
                if (l <= v && v <= r) {
                    ++ans;
                }
            }
        }
        return ans;
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
using ll = long long;

class Solution {
public:
    const vector<pair<char, char>> pairs = {{'1', '1'}, {'8', '8'}, {'6', '9'}, {'9', '6'}};

    int strobogrammaticInRange(string low, string high) {
        int n;
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

        int a = low.size(), b = high.size();
        int ans = 0;
        ll l = stoll(low), r = stoll(high);
        for (n = a; n <= b; ++n) {
            for (auto& s : dfs(n)) {
                ll v = stoll(s);
                if (l <= v && v <= r) {
                    ++ans;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func strobogrammaticInRange(low string, high string) int {
	n := 0
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
	a, b := len(low), len(high)
	l, _ := strconv.Atoi(low)
	r, _ := strconv.Atoi(high)
	ans := 0
	for n = a; n <= b; n++ {
		for _, s := range dfs(n) {
			v, _ := strconv.Atoi(s)
			if l <= v && v <= r {
				ans++
			}
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
