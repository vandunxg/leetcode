---
comments: true
difficulty: Medium
rating: 1499
source: Biweekly Contest 31 Q3
tags:
    - Bit Manipulation
    - Hash Table
    - String
    - Dynamic Programming
    - Prefix Sum
---

<!-- problem:start -->

# [1525. Number of Good Ways to Split a String](https://leetcode.com/problems/number-of-good-ways-to-split-a-string)

[中文文档](/solution/1500-1599/1525.Number%20of%20Good%20Ways%20to%20Split%20a%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code>.</p>

<p>Một phép chia được gọi là <strong>tốt</strong> nếu có thể chia <code>s</code> thành hai chuỗi không rỗng <code>s<sub>left</sub></code> và <code>s<sub>right</sub></code>, sao cho phép nối của chúng bằng <code>s</code> (tức là <code>s<sub>left</sub> + s<sub>right</sub> = s</code>) và số chữ cái phân biệt trong <code>s<sub>left</sub></code> và <code>s<sub>right</sub></code> bằng nhau.</p>

<p>Trả về <em>số <strong>phép chia tốt</strong> có thể thực hiện trên <code>s</code></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;aacaba&quot;
<strong>Output:</strong> 2
<strong>Giải thích:</strong> Có 5 cách chia <code>&quot;aacaba&quot;</code> và 2 cách trong số đó là tốt. 
(&quot;a&quot;, &quot;acaba&quot;) Chuỗi trái và phải lần lượt chứa 1 và 3 chữ cái khác nhau.
(&quot;aa&quot;, &quot;caba&quot;) Chuỗi trái và phải lần lượt chứa 1 và 3 chữ cái khác nhau.
(&quot;aac&quot;, &quot;aba&quot;) Chuỗi trái và phải lần lượt chứa 2 và 2 chữ cái khác nhau (phép chia tốt).
(&quot;aaca&quot;, &quot;ba&quot;) Chuỗi trái và phải lần lượt chứa 2 và 2 chữ cái khác nhau (phép chia tốt).
(&quot;aacab&quot;, &quot;a&quot;) Chuỗi trái và phải lần lượt chứa 3 và 1 chữ cái khác nhau.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;abcd&quot;
<strong>Output:</strong> 1
<strong>Giải thích:</strong> Chia chuỗi như sau (&quot;ab&quot;, &quot;cd&quot;).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các vị trí chia mà hai phía có cùng số chữ cái phân biệt. Vì $n\le 10^5$, việc quét lại cả hai phía ở mỗi vị trí chia sẽ có độ phức tạp bậc hai.
>
> Phía phải ban đầu là một frequency map của toàn chuỗi, còn tập ký tự phía trái chỉ tăng dần. Khi dịch vị trí chia sang phải, ta thêm ký tự hiện tại vào trái và giảm số đếm ở phải, xóa key khi số đếm về 0. Khi hai map có cùng kích thước, phép chia là tốt. Chỉ cần một lần duyệt.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numSplits(self, s: str) -> int:
        cnt = Counter(s)
        vis = set()
        ans = 0
        for c in s:
            vis.add(c)
            cnt[c] -= 1
            if cnt[c] == 0:
                cnt.pop(c)
            ans += len(vis) == len(cnt)
        return ans
```

#### Java

```java
class Solution {
    public int numSplits(String s) {
        Map<Character, Integer> cnt = new HashMap<>();
        for (char c : s.toCharArray()) {
            cnt.merge(c, 1, Integer::sum);
        }
        Set<Character> vis = new HashSet<>();
        int ans = 0;
        for (char c : s.toCharArray()) {
            vis.add(c);
            if (cnt.merge(c, -1, Integer::sum) == 0) {
                cnt.remove(c);
            }
            if (vis.size() == cnt.size()) {
                ++ans;
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
    int numSplits(string s) {
        unordered_map<char, int> cnt;
        for (char& c : s) {
            ++cnt[c];
        }
        unordered_set<char> vis;
        int ans = 0;
        for (char& c : s) {
            vis.insert(c);
            if (--cnt[c] == 0) {
                cnt.erase(c);
            }
            ans += vis.size() == cnt.size();
        }
        return ans;
    }
};
```

#### Go

```go
func numSplits(s string) (ans int) {
	cnt := map[rune]int{}
	for _, c := range s {
		cnt[c]++
	}
	vis := map[rune]bool{}
	for _, c := range s {
		vis[c] = true
		cnt[c]--
		if cnt[c] == 0 {
			delete(cnt, c)
		}
		if len(vis) == len(cnt) {
			ans++
		}
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
