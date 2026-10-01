---
comments: true
difficulty: Medium
tags:
    - Hash Table
    - String
    - Divide and Conquer
    - Sliding Window
---

<!-- problem:start -->

# [395. Longest Substring with At Least K Repeating Characters](https://leetcode.com/problems/longest-substring-with-at-least-k-repeating-characters)

[中文文档](/solution/0300-0399/0395.Longest%20Substring%20with%20At%20Least%20K%20Repeating%20Characters/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code> và số nguyên <code>k</code>, hãy trả về độ dài của chuỗi con dài nhất trong <code>s</code> sao cho mỗi ký tự trong chuỗi con xuất hiện ít nhất <code>k</code> lần.</p>

<p data-pm-slice="1 1 []">Nếu không có chuỗi con nào thỏa mãn, trả về 0.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aaabb&quot;, k = 3
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Chuỗi con dài nhất là &quot;aaa&quot; vì &#39;a&#39; xuất hiện 3 lần.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;ababbc&quot;, k = 2
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Chuỗi con dài nhất là &quot;ababb&quot; vì &#39;a&#39; xuất hiện 2 lần và &#39;b&#39; xuất hiện 3 lần.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>4</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>1 &lt;= k &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

<!-- thinking:start -->

> **Tư duy**
>
> Cần tìm chuỗi con dài nhất mà mỗi ký tự xuất hiện ít nhất $k$ lần. Cửa sổ theo số ký tự phân biệt không thể biểu diễn điều kiện “tất cả đều $\ge k$”. Nếu một ký tự xuất hiện ít hơn $k$ lần trong cả đoạn, nó không thể nằm trong bất kỳ chuỗi con hợp lệ nào; vì vậy, ta dùng ký tự đó làm điểm chia.
>
> Đếm tần suất ký tự trong đoạn, chia tại một ký tự xuất hiện quá ít rồi đệ quy. Nếu không có ký tự như vậy thì cả đoạn hợp lệ. Độ sâu đệ quy tối đa là $26$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestSubstring(self, s: str, k: int) -> int:
        def dfs(l, r):
            cnt = Counter(s[l : r + 1])
            split = next((c for c, v in cnt.items() if v < k), '')
            if not split:
                return r - l + 1
            i = l
            ans = 0
            while i <= r:
                while i <= r and s[i] == split:
                    i += 1
                if i >= r:
                    break
                j = i
                while j <= r and s[j] != split:
                    j += 1
                t = dfs(i, j - 1)
                ans = max(ans, t)
                i = j
            return ans

        return dfs(0, len(s) - 1)
```

#### Java

```java
class Solution {
    private String s;
    private int k;

    public int longestSubstring(String s, int k) {
        this.s = s;
        this.k = k;
        return dfs(0, s.length() - 1);
    }

    private int dfs(int l, int r) {
        int[] cnt = new int[26];
        for (int i = l; i <= r; ++i) {
            ++cnt[s.charAt(i) - 'a'];
        }
        char split = 0;
        for (int i = 0; i < 26; ++i) {
            if (cnt[i] > 0 && cnt[i] < k) {
                split = (char) (i + 'a');
                break;
            }
        }
        if (split == 0) {
            return r - l + 1;
        }
        int i = l;
        int ans = 0;
        while (i <= r) {
            while (i <= r && s.charAt(i) == split) {
                ++i;
            }
            if (i > r) {
                break;
            }
            int j = i;
            while (j <= r && s.charAt(j) != split) {
                ++j;
            }
            int t = dfs(i, j - 1);
            ans = Math.max(ans, t);
            i = j;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int longestSubstring(string s, int k) {
        function<int(int, int)> dfs = [&](int l, int r) -> int {
            int cnt[26] = {0};
            for (int i = l; i <= r; ++i) {
                cnt[s[i] - 'a']++;
            }
            char split = 0;
            for (int i = 0; i < 26; ++i) {
                if (cnt[i] > 0 && cnt[i] < k) {
                    split = 'a' + i;
                    break;
                }
            }
            if (split == 0) {
                return r - l + 1;
            }
            int i = l;
            int ans = 0;
            while (i <= r) {
                while (i <= r && s[i] == split) {
                    ++i;
                }
                if (i >= r) {
                    break;
                }
                int j = i;
                while (j <= r && s[j] != split) {
                    ++j;
                }
                int t = dfs(i, j - 1);
                ans = max(ans, t);
                i = j;
            }
            return ans;
        };
        return dfs(0, s.size() - 1);
    }
};
```

#### Go

```go
func longestSubstring(s string, k int) int {
	var dfs func(l, r int) int
	dfs = func(l, r int) int {
		cnt := [26]int{}
		for i := l; i <= r; i++ {
			cnt[s[i]-'a']++
		}
		var split byte
		for i, v := range cnt {
			if v > 0 && v < k {
				split = byte(i + 'a')
				break
			}
		}
		if split == 0 {
			return r - l + 1
		}
		i := l
		ans := 0
		for i <= r {
			for i <= r && s[i] == split {
				i++
			}
			if i > r {
				break
			}
			j := i
			for j <= r && s[j] != split {
				j++
			}
			t := dfs(i, j-1)
			ans = max(ans, t)
			i = j
		}
		return ans
	}
	return dfs(0, len(s)-1)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
