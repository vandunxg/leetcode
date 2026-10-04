---
comments: true
difficulty: Hard
rating: 2303
source: Biweekly Contest 150 Q4
tags:
    - Two Pointers
    - String
    - Binary Search
    - String Matching
---

<!-- problem:start -->

# [3455. Shortest Matching Substring](https://leetcode.com/problems/shortest-matching-substring)

[中文文档](/solution/3400-3499/3455.Shortest%20Matching%20Substring/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> và một chuỗi mẫu <code>p</code>, trong đó <code>p</code> chứa <strong>đúng hai</strong> ký tự <code>&#39;*&#39;</code>.</p>

<p>Ký tự <code>&#39;*&#39;</code> trong <code>p</code> khớp với một chuỗi gồm không hoặc nhiều ký tự bất kỳ.</p>

<p>Trả về độ dài của <span data-keyword="substring">chuỗi con</span> <strong>ngắn nhất</strong> trong <code>s</code> khớp với <code>p</code>. Nếu không tồn tại chuỗi con như vậy, trả về -1.</p>
<strong>Lưu ý:</strong> Chuỗi rỗng được xem là hợp lệ.
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abaacbaecebce&quot;, p = &quot;ba*c*ce&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">8</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi con ngắn nhất trong <code>s</code> khớp với <code>p</code> là <code>&quot;<u><strong>ba</strong></u>e<u><strong>c</strong></u>eb<u><strong>ce</strong></u>&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;baccbaadbc&quot;, p = &quot;cc*baa*adb&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có chuỗi con nào trong <code>s</code> khớp với mẫu.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;a&quot;, p = &quot;**&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi rỗng là chuỗi con ngắn nhất khớp với mẫu.</p>
</div>

<p><strong class="example">Ví dụ 4:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;madlogic&quot;, p = &quot;*adlogi*&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi con ngắn nhất trong <code>s</code> khớp với <code>p</code> là <code>&quot;<strong><u>adlogi</u></strong>&quot;</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>2 &lt;= p.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ chứa các chữ cái tiếng Anh viết thường.</li>
	<li><code>p</code> chỉ chứa các chữ cái tiếng Anh viết thường và đúng hai ký tự <code>&#39;*&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> $p$ chứa đúng hai dấu sao nên được tách thành ba chuỗi cố định $a$, $b$, $c$. Với $|s|,|p|\le 10^5$, việc tìm kiếm ngây thơ từ mọi vị trí bắt đầu là không phù hợp.
>
> Vị trí xuất hiện của các chuỗi quyết định kết quả khớp ngắn nhất: sau một $a$, chọn $b$ đầu tiên ở bên phải, rồi chọn $c$ đầu tiên ở bên phải nữa.
>
> KMP hoặc thuật toán Z liệt kê mọi vị trí xuất hiện của ba phần. Duyệt hai con trỏ qua các vị trí bắt đầu của $a$, đồng thời tiến các con trỏ của $b$ và $c$. Độ dài là vị trí kết thúc của $c$ trừ vị trí bắt đầu của $a$, hoặc $-1$ nếu không tồn tại kết quả.

<!-- thinking:end -->

Tách mẫu tại hai dấu sao thành ba chuỗi cố định $a$, $b$ và $c$. Bất kỳ chuỗi nào trong số chúng cũng có thể rỗng. KMP liệt kê mọi chỉ số bắt đầu của từng chuỗi trong $s$. Một chuỗi cố định rỗng khớp tại mọi chỉ số $0,1,\ldots,n$.

Với mỗi vị trí bắt đầu $i$ của $a$, tiến một con trỏ đến vị trí bắt đầu $j$ sớm nhất của $b$ sao cho $j\ge i+|a|$, sau đó đến vị trí bắt đầu $k$ sớm nhất của $c$ sao cho $k\ge j+|b|$. Kết quả khớp đó có độ dài $k+|c|-i$. Lấy giá trị nhỏ nhất trên mọi vị trí bắt đầu; nếu không có kết quả khớp nào thì trả về $-1$.

Một $b$ xuất hiện muộn hơn chỉ có thể đẩy $c$ sang phải xa hơn, vì vậy với một $i$ cố định, chọn $b$ đầu tiên và $c$ đầu tiên là tối ưu.

Độ phức tạp thời gian là $O(n+m)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ và $m$ lần lượt là độ dài của $s$ và $p$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def shortestMatchingSubstring(self, s: str, p: str) -> int:
        def starts(pat: str):
            if not pat:
                return list(range(len(s) + 1))
            m = len(pat)
            lps = [0] * m
            length = 0
            i = 1
            while i < m:
                if pat[i] == pat[length]:
                    length += 1
                    lps[i] = length
                    i += 1
                elif length:
                    length = lps[length - 1]
                else:
                    i += 1
            res = []
            i = j = 0
            n = len(s)
            while i < n:
                if s[i] == pat[j]:
                    i += 1
                    j += 1
                    if j == m:
                        res.append(i - m)
                        j = lps[j - 1]
                elif j:
                    j = lps[j - 1]
                else:
                    i += 1
            return res

        a, b, c = p.split('*')
        A, B, C = starts(a), starts(b), starts(c)
        la, lb, lc = len(a), len(b), len(c)
        ans = len(s) + 1
        j = k = 0
        for i in A:
            while j < len(B) and B[j] < i + la:
                j += 1
            if j == len(B):
                break
            while k < len(C) and C[k] < B[j] + lb:
                k += 1
            if k == len(C):
                break
            ans = min(ans, C[k] + lc - i)
        return -1 if ans > len(s) else ans
```

#### Java

```java
class Solution {
    public int shortestMatchingSubstring(String s, String p) {
        int star = p.indexOf('*');
        int star2 = p.indexOf('*', star + 1);
        String a = p.substring(0, star);
        String b = p.substring(star + 1, star2);
        String c = p.substring(star2 + 1);
        int[] A = starts(s, a);
        int[] B = starts(s, b);
        int[] C = starts(s, c);
        int ans = s.length() + 1;
        int j = 0, k = 0;
        for (int i : A) {
            while (j < B.length && B[j] < i + a.length()) {
                ++j;
            }
            if (j == B.length) {
                break;
            }
            while (k < C.length && C[k] < B[j] + b.length()) {
                ++k;
            }
            if (k == C.length) {
                break;
            }
            ans = Math.min(ans, C[k] + c.length() - i);
        }
        return ans > s.length() ? -1 : ans;
    }

    private int[] starts(String s, String pat) {
        int n = s.length();
        if (pat.isEmpty()) {
            int[] res = new int[n + 1];
            for (int i = 0; i <= n; ++i) {
                res[i] = i;
            }
            return res;
        }
        int m = pat.length();
        int[] lps = new int[m];
        for (int i = 1, len = 0; i < m;) {
            if (pat.charAt(i) == pat.charAt(len)) {
                lps[i++] = ++len;
            } else if (len > 0) {
                len = lps[len - 1];
            } else {
                ++i;
            }
        }
        int[] tmp = new int[n];
        int cnt = 0;
        for (int i = 0, j = 0; i < n;) {
            if (s.charAt(i) == pat.charAt(j)) {
                ++i;
                ++j;
                if (j == m) {
                    tmp[cnt++] = i - m;
                    j = lps[j - 1];
                }
            } else if (j > 0) {
                j = lps[j - 1];
            } else {
                ++i;
            }
        }
        int[] res = new int[cnt];
        System.arraycopy(tmp, 0, res, 0, cnt);
        return res;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int shortestMatchingSubstring(string s, string p) {
        int star = p.find('*');
        int star2 = p.find('*', star + 1);
        string a = p.substr(0, star);
        string b = p.substr(star + 1, star2 - star - 1);
        string c = p.substr(star2 + 1);
        vector<int> A = starts(s, a), B = starts(s, b), C = starts(s, c);
        int ans = s.size() + 1;
        int j = 0, k = 0;
        for (int i : A) {
            while (j < (int) B.size() && B[j] < i + (int) a.size()) {
                ++j;
            }
            if (j == (int) B.size()) {
                break;
            }
            while (k < (int) C.size() && C[k] < B[j] + (int) b.size()) {
                ++k;
            }
            if (k == (int) C.size()) {
                break;
            }
            ans = min(ans, C[k] + (int) c.size() - i);
        }
        return ans > (int) s.size() ? -1 : ans;
    }

private:
    vector<int> starts(const string& s, const string& pat) {
        int n = s.size();
        if (pat.empty()) {
            vector<int> res(n + 1);
            iota(res.begin(), res.end(), 0);
            return res;
        }
        int m = pat.size();
        vector<int> lps(m);
        for (int i = 1, len = 0; i < m;) {
            if (pat[i] == pat[len]) {
                lps[i++] = ++len;
            } else if (len) {
                len = lps[len - 1];
            } else {
                ++i;
            }
        }
        vector<int> res;
        for (int i = 0, j = 0; i < n;) {
            if (s[i] == pat[j]) {
                ++i;
                ++j;
                if (j == m) {
                    res.push_back(i - m);
                    j = lps[j - 1];
                }
            } else if (j) {
                j = lps[j - 1];
            } else {
                ++i;
            }
        }
        return res;
    }
};
```

#### Go

```go
func shortestMatchingSubstring(s string, p string) int {
	star := 0
	for p[star] != '*' {
		star++
	}
	star2 := star + 1
	for p[star2] != '*' {
		star2++
	}
	a, b, c := p[:star], p[star+1:star2], p[star2+1:]
	A, B, C := matchStarts(s, a), matchStarts(s, b), matchStarts(s, c)
	ans := len(s) + 1
	j, k := 0, 0
	for _, i := range A {
		for j < len(B) && B[j] < i+len(a) {
			j++
		}
		if j == len(B) {
			break
		}
		for k < len(C) && C[k] < B[j]+len(b) {
			k++
		}
		if k == len(C) {
			break
		}
		ans = min(ans, C[k]+len(c)-i)
	}
	if ans > len(s) {
		return -1
	}
	return ans
}

func matchStarts(s, pat string) []int {
	n := len(s)
	if pat == "" {
		res := make([]int, n+1)
		for i := 0; i <= n; i++ {
			res[i] = i
		}
		return res
	}
	m := len(pat)
	lps := make([]int, m)
	for i, length := 1, 0; i < m; {
		if pat[i] == pat[length] {
			length++
			lps[i] = length
			i++
		} else if length > 0 {
			length = lps[length-1]
		} else {
			i++
		}
	}
	res := make([]int, 0)
	for i, j := 0, 0; i < n; {
		if s[i] == pat[j] {
			i++
			j++
			if j == m {
				res = append(res, i-m)
				j = lps[j-1]
			}
		} else if j > 0 {
			j = lps[j-1]
		} else {
			i++
		}
	}
	return res
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
