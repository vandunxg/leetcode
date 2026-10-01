---
comments: true
difficulty: Medium
tags:
    - Hash Table
    - String
    - Backtracking
---

<!-- problem:start -->

# [267. Palindrome Permutation II 🔒](https://leetcode.com/problems/palindrome-permutation-ii)

[中文文档](/solution/0200-0299/0267.Palindrome%20Permutation%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi s, hãy trả về <em>tất cả các hoán vị palindrome của chuỗi (không trùng lặp)</em>.</p>

<p>Bạn có thể trả lời theo <strong>bất kỳ thứ tự nào</strong>. Nếu <code>s</code> không có hoán vị palindrome nào, hãy trả về danh sách rỗng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> s = "aabb"
<strong>Đầu ra:</strong> ["abba","baab"]
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> s = "abc"
<strong>Đầu ra:</strong> []
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 16</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Trước tiên, kiểm tra xem có nhiều nhất một ký tự xuất hiện số lần lẻ hay không. Giữ ký tự đó (nếu có) làm tâm rồi tạo từng cặp ký tự ở hai phía.
>
> $dfs(t)$ chọn một ký tự còn ít nhất hai lần xuất hiện, đặt ký tự đó ở hai đầu của $t$, rồi dừng khi độ dài chuỗi tạo được bằng độ dài của $s$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def generatePalindromes(self, s: str) -> List[str]:
        def dfs(t):
            if len(t) == len(s):
                ans.append(t)
                return
            for c, v in cnt.items():
                if v > 1:
                    cnt[c] -= 2
                    dfs(c + t + c)
                    cnt[c] += 2

        cnt = Counter(s)
        mid = ''
        for c, v in cnt.items():
            if v & 1:
                if mid:
                    return []
                mid = c
                cnt[c] -= 1
        ans = []
        dfs(mid)
        return ans
```

#### Java

```java
class Solution {
    private List<String> ans = new ArrayList<>();
    private int[] cnt = new int[26];
    private int n;

    public List<String> generatePalindromes(String s) {
        n = s.length();
        for (char c : s.toCharArray()) {
            ++cnt[c - 'a'];
        }
        String mid = "";
        for (int i = 0; i < 26; ++i) {
            if (cnt[i] % 2 == 1) {
                if (!"".equals(mid)) {
                    return ans;
                }
                mid = String.valueOf((char) (i + 'a'));
            }
        }
        dfs(mid);
        return ans;
    }

    private void dfs(String t) {
        if (t.length() == n) {
            ans.add(t);
            return;
        }
        for (int i = 0; i < 26; ++i) {
            if (cnt[i] > 1) {
                String c = String.valueOf((char) (i + 'a'));
                cnt[i] -= 2;
                dfs(c + t + c);
                cnt[i] += 2;
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int n;
    vector<string> ans;
    unordered_map<char, int> cnt;

    vector<string> generatePalindromes(string s) {
        n = s.size();
        for (char c : s) ++cnt[c];
        string mid = "";
        for (auto& [k, v] : cnt) {
            if (v & 1) {
                if (mid != "") {
                    return ans;
                }
                mid += k;
            }
        }
        dfs(mid);
        return ans;
    }

    void dfs(string t) {
        if (t.size() == n) {
            ans.push_back(t);
            return;
        }
        for (auto& [k, v] : cnt) {
            if (v > 1) {
                v -= 2;
                dfs(k + t + k);
                v += 2;
            }
        }
    }
};
```

#### Go

```go
func generatePalindromes(s string) []string {
	cnt := map[byte]int{}
	for i := range s {
		cnt[s[i]]++
	}
	mid := ""
	ans := []string{}
	for k, v := range cnt {
		if v%2 == 1 {
			if mid != "" {
				return ans
			}
			mid = string(k)
		}
	}
	var dfs func(t string)
	dfs = func(t string) {
		if len(t) == len(s) {
			ans = append(ans, t)
			return
		}
		for k, v := range cnt {
			if v > 1 {
				cnt[k] -= 2
				c := string(k)
				dfs(c + t + c)
				cnt[k] += 2
			}
		}
	}
	dfs(mid)
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
