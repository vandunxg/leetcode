---
comments: true
difficulty: Medium
tags:
    - Hash Table
    - String
    - Counting
    - Sliding Window
---

<!-- problem:start -->

# [2067. Number of Equal Count Substrings 🔒](https://leetcode.com/problems/number-of-equal-count-substrings)

[中文文档](/solution/2000-2099/2067.Number%20of%20Equal%20Count%20Substrings/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <strong>0-indexed</strong> <code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường và một số nguyên <code>count</code>. Một <strong>substring</strong> của <code>s</code> được gọi là <strong>substring có số lần xuất hiện bằng nhau</strong> nếu với mỗi chữ cái <strong>khác nhau</strong> trong substring, chữ cái đó xuất hiện đúng <code>count</code> lần trong substring.</p>

<p>Hãy trả về <em>số lượng <strong>substring có số lần xuất hiện bằng nhau</strong> trong </em><code>s</code>.</p>

<p><strong>Substring</strong> là một dãy ký tự liên tiếp, không rỗng trong một chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aaabcbbcc&quot;, count = 3
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
Substring bắt đầu tại chỉ số 0 và kết thúc tại chỉ số 2 là &quot;aaa&quot;.
Chữ cái &#39;a&#39; trong substring xuất hiện đúng 3 lần.
Substring bắt đầu tại chỉ số 3 và kết thúc tại chỉ số 8 là &quot;bcbbcc&quot;.
Các chữ cái &#39;b&#39; và &#39;c&#39; trong substring xuất hiện đúng 3 lần.
Substring bắt đầu tại chỉ số 0 và kết thúc tại chỉ số 8 là &quot;aaabcbbcc&quot;.
Các chữ cái &#39;a&#39;, &#39;b&#39; và &#39;c&#39; trong substring xuất hiện đúng 3 lần.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abcd&quot;, count = 2
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>
Số lần xuất hiện của mỗi chữ cái trong s đều nhỏ hơn count.
Vì vậy, không có substring nào trong s là substring có số lần xuất hiện bằng nhau, nên trả về 0.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;a&quot;, count = 5
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>
Số lần xuất hiện của mỗi chữ cái trong s đều nhỏ hơn count.
Vì vậy, không có substring nào trong s là substring có số lần xuất hiện bằng nhau, nên trả về 0</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 3 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= count &lt;= 3 * 10<sup>4</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê + Sliding Window

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi chữ cái xuất hiện trong substring đều xuất hiện đúng $count$ lần. Với $i \in [1,26]$ loại chữ cái, độ dài cửa sổ là $i \cdot count$, nên ta có thể trượt cửa sổ này.
>
> Theo dõi số chữ cái hiện có tần suất bằng $count$; cập nhật giá trị này khi thêm hoặc xóa một ký tự. Cửa sổ hợp lệ khi số đó bằng $i$.

<!-- thinking:end -->

Ta có thể liệt kê số loại chữ cái trong substring trong khoảng $[1..26]$, khi đó độ dài substring là $i \times count$.

Tiếp theo, ta lấy độ dài substring hiện tại làm kích thước cửa sổ, đếm số loại chữ cái trong cửa sổ có số lần xuất hiện bằng $count$ và lưu vào $t$. Nếu lúc này $i = t$, nghĩa là tất cả các chữ cái trong cửa sổ hiện tại đều xuất hiện $count$ lần, khi đó tăng đáp án lên một.

Độ phức tạp thời gian là $O(n \times C)$, độ phức tạp không gian là $O(C)$. Trong đó $n$ là độ dài chuỗi $s$, còn $C$ là số loại chữ cái; trong bài này $C = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def equalCountSubstrings(self, s: str, count: int) -> int:
        ans = 0
        for i in range(1, 27):
            k = i * count
            if k > len(s):
                break
            cnt = Counter()
            t = 0
            for j, c in enumerate(s):
                cnt[c] += 1
                t += cnt[c] == count
                t -= cnt[c] == count + 1
                if j >= k:
                    cnt[s[j - k]] -= 1
                    t += cnt[s[j - k]] == count
                    t -= cnt[s[j - k]] == count - 1
                ans += i == t
        return ans
```

#### Java

```java
class Solution {
    public int equalCountSubstrings(String s, int count) {
        int ans = 0;
        int[] cnt = new int[26];
        int n = s.length();
        for (int i = 1; i < 27 && i * count <= n; ++i) {
            int k = i * count;
            Arrays.fill(cnt, 0);
            int t = 0;
            for (int j = 0; j < n; ++j) {
                int a = s.charAt(j) - 'a';
                ++cnt[a];
                t += cnt[a] == count ? 1 : 0;
                t -= cnt[a] == count + 1 ? 1 : 0;
                if (j - k >= 0) {
                    int b = s.charAt(j - k) - 'a';
                    --cnt[b];
                    t += cnt[b] == count ? 1 : 0;
                    t -= cnt[b] == count - 1 ? 1 : 0;
                }
                ans += i == t ? 1 : 0;
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
    int equalCountSubstrings(string s, int count) {
        int ans = 0;
        int n = s.size();
        int cnt[26];
        for (int i = 1; i < 27 && i * count <= n; ++i) {
            int k = i * count;
            memset(cnt, 0, sizeof(cnt));
            int t = 0;
            for (int j = 0; j < n; ++j) {
                int a = s[j] - 'a';
                t += ++cnt[a] == count;
                t -= cnt[a] == count + 1;
                if (j >= k) {
                    int b = s[j - k] - 'a';
                    t += --cnt[b] == count;
                    t -= cnt[b] == count - 1;
                }
                ans += i == t;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func equalCountSubstrings(s string, count int) (ans int) {
	n := len(s)
	for i := 1; i < 27 && i*count <= n; i++ {
		k := i * count
		cnt := [26]int{}
		t := 0
		for j, c := range s {
			a := c - 'a'
			cnt[a]++
			if cnt[a] == count {
				t++
			} else if cnt[a] == count+1 {
				t--
			}
			if j >= k {
				b := s[j-k] - 'a'
				cnt[b]--
				if cnt[b] == count {
					t++
				} else if cnt[b] == count-1 {
					t--
				}
			}
			if i == t {
				ans++
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function equalCountSubstrings(s: string, count: number): number {
    const n = s.length;
    let ans = 0;
    for (let i = 1; i < 27 && i * count <= n; ++i) {
        const k = i * count;
        const cnt: number[] = Array(26).fill(0);
        let t = 0;
        for (let j = 0; j < n; ++j) {
            const a = s.charCodeAt(j) - 97;
            t += ++cnt[a] === count ? 1 : 0;
            t -= cnt[a] === count + 1 ? 1 : 0;
            if (j >= k) {
                const b = s.charCodeAt(j - k) - 97;
                t += --cnt[b] === count ? 1 : 0;
                t -= cnt[b] === count - 1 ? 1 : 0;
            }
            ans += i === t ? 1 : 0;
        }
    }
    return ans;
}
```

#### JavaScript

```js
/**
 * @param {string} s
 * @param {number} count
 * @return {number}
 */
var equalCountSubstrings = function (s, count) {
    const n = s.length;
    let ans = 0;
    for (let i = 1; i < 27 && i * count <= n; ++i) {
        const k = i * count;
        const cnt = Array(26).fill(0);
        let t = 0;
        for (let j = 0; j < n; ++j) {
            const a = s.charCodeAt(j) - 97;
            t += ++cnt[a] === count ? 1 : 0;
            t -= cnt[a] === count + 1 ? 1 : 0;
            if (j >= k) {
                const b = s.charCodeAt(j - k) - 97;
                t += --cnt[b] === count ? 1 : 0;
                t -= cnt[b] === count - 1 ? 1 : 0;
            }
            ans += i === t ? 1 : 0;
        }
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
