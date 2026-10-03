---
comments: true
difficulty: Medium
rating: 1834
source: Weekly Contest 305 Q4
tags:
    - Hash Table
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [2370. Longest Ideal Subsequence](https://leetcode.com/problems/longest-ideal-subsequence)

[中文文档](/solution/2300-2399/2370.Longest%20Ideal%20Subsequence/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một chuỗi <code>s</code> gồm các chữ cái viết thường và một số nguyên <code>k</code>. Một chuỗi <code>t</code> được gọi là <strong>lý tưởng</strong> nếu thỏa mãn các điều kiện sau:</p>

<ul>
	<li><code>t</code> là một <strong>dãy con</strong> của chuỗi <code>s</code>.</li>
	<li>Hiệu tuyệt đối theo thứ tự alphabet của mọi cặp ký tự <strong>liền kề</strong> trong <code>t</code> không vượt quá <code>k</code>.</li>
</ul>

<p>Hãy trả về <em>độ dài của chuỗi lý tưởng <strong>dài nhất</strong></em>.</p>

<p><strong>Dãy con</strong> là một chuỗi có thể được tạo ra từ một chuỗi khác bằng cách xóa một số ký tự hoặc không xóa ký tự nào, mà không thay đổi thứ tự của các ký tự còn lại.</p>

<p><strong>Lưu ý</strong> rằng thứ tự alphabet không có tính tuần hoàn. Ví dụ, hiệu tuyệt đối theo thứ tự alphabet của <code>&#39;a&#39;</code> và <code>&#39;z&#39;</code> là <code>25</code>, không phải <code>1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;acfgbd&quot;, k = 2
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Chuỗi lý tưởng dài nhất là &quot;acbd&quot;. Độ dài của chuỗi này là 4, nên kết quả trả về là 4.
Lưu ý rằng &quot;acfgbd&quot; không phải là chuỗi lý tưởng vì &#39;c&#39; và &#39;f&#39; có hiệu là 3 theo thứ tự alphabet.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abcd&quot;, k = 3
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Chuỗi lý tưởng dài nhất là &quot;abcd&quot;. Độ dài của chuỗi này là 4, nên kết quả trả về là 4.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= k &lt;= 25</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các ký tự liền kề trong dãy con có thể chênh lệch nhiều nhất là $k$. Vì $n \le 10^5$, ta không thể liệt kê các dãy con. Kết quả tốt nhất kết thúc tại $i$ phụ thuộc vào các chữ cái trước đó nằm trong khoảng $[s[i]-k,s[i]+k]$.
>
> $dp[i]$ là kết quả tốt nhất kết thúc tại $i$; một map lưu chỉ số gần nhất của mỗi chữ cái. Ta lấy giá trị $dp$ lớn nhất trong các phần tử đứng trước hợp lệ rồi cộng thêm một. Alphabet có kích thước là $26$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestIdealString(self, s: str, k: int) -> int:
        n = len(s)
        ans = 1
        dp = [1] * n
        d = {s[0]: 0}
        for i in range(1, n):
            a = ord(s[i])
            for b in ascii_lowercase:
                if abs(a - ord(b)) > k:
                    continue
                if b in d:
                    dp[i] = max(dp[i], dp[d[b]] + 1)
            d[s[i]] = i
        return max(dp)
```

#### Java

```java
class Solution {
    public int longestIdealString(String s, int k) {
        int n = s.length();
        int ans = 1;
        int[] dp = new int[n];
        Arrays.fill(dp, 1);
        Map<Character, Integer> d = new HashMap<>(26);
        d.put(s.charAt(0), 0);
        for (int i = 1; i < n; ++i) {
            char a = s.charAt(i);
            for (char b = 'a'; b <= 'z'; ++b) {
                if (Math.abs(a - b) > k) {
                    continue;
                }
                if (d.containsKey(b)) {
                    dp[i] = Math.max(dp[i], dp[d.get(b)] + 1);
                }
            }
            d.put(a, i);
            ans = Math.max(ans, dp[i]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int longestIdealString(string s, int k) {
        int n = s.size();
        int ans = 1;
        vector<int> dp(n, 1);
        unordered_map<char, int> d;
        d[s[0]] = 0;
        for (int i = 1; i < n; ++i) {
            char a = s[i];
            for (char b = 'a'; b <= 'z'; ++b) {
                if (abs(a - b) > k) continue;
                if (d.count(b)) dp[i] = max(dp[i], dp[d[b]] + 1);
            }
            d[a] = i;
            ans = max(ans, dp[i]);
        }
        return ans;
    }
};
```

#### Go

```go
func longestIdealString(s string, k int) int {
	n := len(s)
	ans := 1
	dp := make([]int, n)
	for i := range dp {
		dp[i] = 1
	}
	d := map[byte]int{s[0]: 0}
	for i := 1; i < n; i++ {
		a := s[i]
		for b := byte('a'); b <= byte('z'); b++ {
			if int(a)-int(b) > k || int(b)-int(a) > k {
				continue
			}
			if v, ok := d[b]; ok {
				dp[i] = max(dp[i], dp[v]+1)
			}
		}
		d[a] = i
		ans = max(ans, dp[i])
	}
	return ans
}
```

#### TypeScript

```ts
function longestIdealString(s: string, k: number): number {
    const dp = new Array(26).fill(0);
    for (const c of s) {
        const x = c.charCodeAt(0) - 'a'.charCodeAt(0);
        let t = 0;
        for (let i = 0; i < 26; i++) {
            if (Math.abs(x - i) <= k) {
                t = Math.max(t, dp[i] + 1);
            }
        }
        dp[x] = Math.max(dp[x], t);
    }

    return dp.reduce((r, c) => Math.max(r, c), 0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
