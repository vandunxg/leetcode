---
comments: true
difficulty: Hard
tags:
    - Hash Table
    - String
    - Sorting
---

<!-- problem:start -->

# [3104. Find Longest Self-Contained Substring 🔒](https://leetcode.com/problems/find-longest-self-contained-substring)

[中文文档](/solution/3100-3199/3104.Find%20Longest%20Self-Contained%20Substring/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code>, hãy tìm độ dài của <span data-keyword="substring-nonempty">chuỗi con</span> <strong>tự chứa dài nhất</strong> của <code>s</code>.</p>

<p>Một chuỗi con <code>t</code> của chuỗi <code>s</code> được gọi là <strong>tự chứa</strong> nếu <code>t != s</code> và với mọi ký tự trong <code>t</code>, ký tự đó không tồn tại trong <em>phần còn lại</em> của <code>s</code>.</p>

<p>Trả về độ dài của chuỗi con <em>tự chứa<strong> </strong>dài nhất</em> của <code>s</code> nếu tồn tại, nếu không thì trả về -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abba&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong><br />
Xét chuỗi con <code>&quot;bb&quot;</code>. Có thể thấy không có ký tự <code>&quot;b&quot;</code> nào khác nằm ngoài chuỗi con này. Vì vậy, đáp án là 2.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abab&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong><br />
Mọi chuỗi con được chọn đều không thỏa mãn tính chất đã mô tả (có một ký tự vừa nằm trong vừa nằm ngoài chuỗi con). Vì vậy, đáp án là -1.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abacd&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong><br />
Xét chuỗi con <code>&quot;<span class="example-io">abac</span>&quot;</code>. Chỉ có một ký tự nằm ngoài chuỗi con này, đó là <code>&quot;d&quot;</code>. Không có ký tự <code>&quot;d&quot;</code> nào bên trong chuỗi con được chọn, nên chuỗi con này thỏa mãn điều kiện và đáp án là 4.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= s.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Một chuỗi con tự chứa phải chứa mọi lần xuất hiện của mỗi ký tự có trong nó, đồng thời không được là toàn bộ chuỗi. Nếu kiểm tra mọi đoạn bằng vị trí đầu và cuối của $26$ ký tự thì độ phức tạp là $O(n^2|\Sigma|)$, quá lớn.
>
> Điểm bắt đầu hợp lệ phải là lần xuất hiện đầu tiên của một ký tự: mở rộng sang trái chỉ có thể thêm một ký tự mới hoặc một bản sao xuất hiện sớm hơn. Vì chỉ có tối đa $26$ chữ cái, số điểm bắt đầu cần xét là rất ít.
>
> Ghi lại chỉ số xuất hiện đầu tiên và cuối cùng của mỗi ký tự, liệt kê chỉ số bắt đầu $i$, rồi duyệt $j$ sang phải và theo dõi biên phải xa nhất $mx$. Dừng nếu một ký tự xuất hiện lần đầu trước $i$; khi $mx=j$ và đoạn con không phải toàn bộ chuỗi, cập nhật độ dài lớn nhất.

<!-- thinking:end -->

Ta nhận thấy điểm bắt đầu của một chuỗi con thỏa mãn điều kiện phải là vị trí xuất hiện đầu tiên của một ký tự.

Do đó, ta có thể dùng hai mảng hoặc hash table `first` và `last` để ghi lại vị trí xuất hiện đầu tiên và cuối cùng của mỗi ký tự.

Tiếp theo, ta liệt kê từng ký tự `c`. Giả sử vị trí xuất hiện đầu tiên của `c` là $i$, còn vị trí xuất hiện cuối cùng là $mx$. Khi đó, ta có thể bắt đầu duyệt từ $i$. Với mỗi vị trí $j$, ta tìm vị trí $a$ nơi $s[j]$ xuất hiện lần đầu và vị trí $b$ nơi nó xuất hiện lần cuối. Nếu $a < i$, điều đó có nghĩa là $s[j]$ nằm bên trái $c$, không thỏa mãn điều kiện liệt kê, nên ta có thể thoát vòng lặp ngay. Nếu không, ta cập nhật $mx = \max(mx, b)$. Nếu $mx = j$ và $j - i + 1 < n$, ta cập nhật đáp án thành $ans = \max(ans, j - i + 1)$.

Cuối cùng, trả về đáp án.

Độ phức tạp thời gian là $O(n \times |\Sigma|)$, và độ phức tạp không gian là $O(|\Sigma|)$. Trong đó, $n$ là độ dài của chuỗi $s$; còn $|\Sigma|$ là kích thước của tập ký tự. Trong bài toán này, tập ký tự là các chữ cái viết thường, nên $|\Sigma| = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSubstringLength(self, s: str) -> int:
        first, last = {}, {}
        for i, c in enumerate(s):
            if c not in first:
                first[c] = i
            last[c] = i
        ans, n = -1, len(s)
        for c, i in first.items():
            mx = last[c]
            for j in range(i, n):
                a, b = first[s[j]], last[s[j]]
                if a < i:
                    break
                mx = max(mx, b)
                if mx == j and j - i + 1 < n:
                    ans = max(ans, j - i + 1)
        return ans
```

#### Java

```java
class Solution {
    public int maxSubstringLength(String s) {
        int[] first = new int[26];
        int[] last = new int[26];
        Arrays.fill(first, -1);
        int n = s.length();
        for (int i = 0; i < n; ++i) {
            int j = s.charAt(i) - 'a';
            if (first[j] == -1) {
                first[j] = i;
            }
            last[j] = i;
        }
        int ans = -1;
        for (int k = 0; k < 26; ++k) {
            int i = first[k];
            if (i == -1) {
                continue;
            }
            int mx = last[k];
            for (int j = i; j < n; ++j) {
                int a = first[s.charAt(j) - 'a'];
                int b = last[s.charAt(j) - 'a'];
                if (a < i) {
                    break;
                }
                mx = Math.max(mx, b);
                if (mx == j && j - i + 1 < n) {
                    ans = Math.max(ans, j - i + 1);
                }
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
    int maxSubstringLength(string s) {
        vector<int> first(26, -1);
        vector<int> last(26);
        int n = s.length();
        for (int i = 0; i < n; ++i) {
            int j = s[i] - 'a';
            if (first[j] == -1) {
                first[j] = i;
            }
            last[j] = i;
        }
        int ans = -1;
        for (int k = 0; k < 26; ++k) {
            int i = first[k];
            if (i == -1) {
                continue;
            }
            int mx = last[k];
            for (int j = i; j < n; ++j) {
                int a = first[s[j] - 'a'];
                int b = last[s[j] - 'a'];
                if (a < i) {
                    break;
                }
                mx = max(mx, b);
                if (mx == j && j - i + 1 < n) {
                    ans = max(ans, j - i + 1);
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxSubstringLength(s string) int {
	first := [26]int{}
	last := [26]int{}
	for i := range first {
		first[i] = -1
	}
	n := len(s)
	for i, c := range s {
		j := int(c - 'a')
		if first[j] == -1 {
			first[j] = i
		}
		last[j] = i
	}
	ans := -1
	for k := 0; k < 26; k++ {
		i := first[k]
		if i == -1 {
			continue
		}
		mx := last[k]
		for j := i; j < n; j++ {
			a, b := first[s[j]-'a'], last[s[j]-'a']
			if a < i {
				break
			}
			mx = max(mx, b)
			if mx == j && j-i+1 < n {
				ans = max(ans, j-i+1)
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function maxSubstringLength(s: string): number {
    const first: number[] = Array(26).fill(-1);
    const last: number[] = Array(26).fill(0);
    const n = s.length;
    for (let i = 0; i < n; ++i) {
        const j = s.charCodeAt(i) - 97;
        if (first[j] === -1) {
            first[j] = i;
        }
        last[j] = i;
    }
    let ans = -1;
    for (let k = 0; k < 26; ++k) {
        const i = first[k];
        if (i === -1) {
            continue;
        }
        let mx = last[k];
        for (let j = i; j < n; ++j) {
            const a = first[s.charCodeAt(j) - 97];
            if (a < i) {
                break;
            }
            const b = last[s.charCodeAt(j) - 97];
            mx = Math.max(mx, b);
            if (mx === j && j - i + 1 < n) {
                ans = Math.max(ans, j - i + 1);
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
