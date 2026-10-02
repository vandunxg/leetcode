---
comments: true
difficulty: Medium
rating: 1689
source: Weekly Contest 185 Q3
tags:
    - String
    - Counting
---

<!-- problem:start -->

# [1419. Minimum Number of Frogs Croaking](https://leetcode.com/problems/minimum-number-of-frogs-croaking)

[中文文档](/solution/1400-1499/1419.Minimum%20Number%20of%20Frogs%20Croaking/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho chuỗi <code>croakOfFrogs</code>, biểu diễn sự kết hợp của chuỗi <code>&quot;croak&quot;</code> do các con ếch khác nhau phát ra. Nói cách khác, nhiều con ếch có thể kêu cùng lúc nên nhiều chuỗi <code>&quot;croak&quot;</code> bị trộn lẫn.</p>

<p><em>Hãy trả về số lượng ếch khác nhau nhỏ nhất </em>cần thiết để hoàn thành tất cả tiếng kêu trong chuỗi đã cho.</p>

<p>Một tiếng <code>&quot;croak&quot;</code> hợp lệ nghĩa là một con ếch phát ra năm chữ cái <code>&#39;c&#39;</code>, <code>&#39;r&#39;</code>, <code>&#39;o&#39;</code>, <code>&#39;a&#39;</code> và <code>&#39;k&#39;</code> theo <strong>thứ tự</strong>. Ếch phải phát ra đủ cả năm chữ cái để hoàn thành một tiếng kêu. Nếu chuỗi đã cho không phải là sự kết hợp của các tiếng <code>&quot;croak&quot;</code> hợp lệ, hãy trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> croakOfFrogs = &quot;croakcroak&quot;
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Một con ếch kêu &quot;croak<strong>&quot;</strong> hai lần.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> croakOfFrogs = &quot;crcoakroak&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Số lượng ếch nhỏ nhất là hai.
Con ếch thứ nhất có thể kêu &quot;<strong>cr</strong>c<strong>oak</strong>roak&quot;.
Con ếch thứ hai có thể kêu sau đó &quot;cr<strong>c</strong>oak<strong>roak</strong>&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> croakOfFrogs = &quot;croakcrook&quot;
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Chuỗi đã cho là sự kết hợp không hợp lệ của &quot;croak<strong>&quot;</strong> từ các con ếch khác nhau.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= croakOfFrogs.length &lt;= 10<sup>5</sup></code></li>
	<li><code>croakOfFrogs</code> chỉ gồm một trong các ký tự <code>&#39;c&#39;</code>, <code>&#39;r&#39;</code>, <code>&#39;o&#39;</code>, <code>&#39;a&#39;</code> hoặc <code>&#39;k&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm + mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Nhiều tiếng `croak` có thể đan xen và chuỗi có thể dài $10^5$, nên không thể lần lượt ghép từng con ếch. Các chữ cái phải tiến triển theo thứ tự $c\to r\to o\to a\to k$.
>
> Một bộ đếm gồm 5 ô theo dõi số tiếng kêu đang ở mỗi giai đoạn. Chữ `c` bắt đầu một con ếch mới; mỗi chữ cái tiếp theo phải sử dụng giai đoạn trước đó. Chữ `k` hoàn thành một con ếch.
>
> Đáp án là số lượng lớn nhất các con ếch chưa hoàn thành. Nếu còn tiếng kêu chưa hoàn thành hoặc độ dài không chia hết cho $5$, kết quả là $-1$.

<!-- thinking:end -->

Ta nhận thấy nếu chuỗi `croakOfFrogs` được tạo thành từ nhiều tiếng `"croak"` hợp lệ trộn lẫn, độ dài của chuỗi phải là bội số của $5$. Vì vậy, nếu độ dài chuỗi không là bội số của $5$, ta có thể trực tiếp trả về $-1$.

Tiếp theo, ta ánh xạ các chữ cái `'c'`, `'r'`, `'o'`, `'a'`, `'k'` lần lượt đến các chỉ số từ $0$ đến $4$, đồng thời sử dụng mảng $cnt$ có độ dài $5$ để ghi lại số lần xuất hiện của mỗi chữ cái trong chuỗi `croakOfFrogs`, trong đó $cnt[i]$ biểu thị số lần xuất hiện của chữ cái tại chỉ số $i$. Ngoài ra, ta định nghĩa biến số nguyên $x$ biểu thị số con ếch chưa hoàn thành tiếng kêu, và số lượng ếch nhỏ nhất cần thiết $ans$ là giá trị lớn nhất của $x$.

Ta duyệt qua từng chữ cái $c$ trong chuỗi `croakOfFrogs`, tìm chỉ số $i$ tương ứng với $c$, sau đó tăng $cnt[i]$ lên $1$. Tiếp theo, tùy thuộc vào giá trị của $i$, ta thực hiện các thao tác sau:

- Nếu $i=0$, một con ếch mới bắt đầu kêu, nên ta tăng $x$ lên $1$, sau đó cập nhật $ans = \max(ans, x)$;
- Ngược lại, nếu $cnt[i-1]=0$, điều đó có nghĩa là không có con ếch nào có thể phát ra âm $c$, nên không thể hoàn thành tiếng kêu và ta trả về $-1$. Nếu không, ta giảm $cnt[i-1]$ đi $1$. Nếu $i=4$, điều đó có nghĩa là một con ếch đã hoàn thành tiếng kêu, nên ta giảm $x$ đi $1$.

Sau khi duyệt xong, nếu $x=0$, điều đó có nghĩa là tất cả các con ếch đã hoàn thành tiếng kêu, và ta trả về $ans$. Ngược lại, ta trả về $-1$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(C)$. Trong đó, $n$ là độ dài của chuỗi `croakOfFrogs`, còn $C$ là kích thước của tập ký tự; trong bài toán này $C=26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minNumberOfFrogs(self, croakOfFrogs: str) -> int:
        if len(croakOfFrogs) % 5 != 0:
            return -1
        idx = {c: i for i, c in enumerate('croak')}
        cnt = [0] * 5
        ans = x = 0
        for i in map(idx.get, croakOfFrogs):
            cnt[i] += 1
            if i == 0:
                x += 1
                ans = max(ans, x)
            else:
                if cnt[i - 1] == 0:
                    return -1
                cnt[i - 1] -= 1
                if i == 4:
                    x -= 1
        return -1 if x else ans
```

#### Java

```java
class Solution {
    public int minNumberOfFrogs(String croakOfFrogs) {
        int n = croakOfFrogs.length();
        if (n % 5 != 0) {
            return -1;
        }
        int[] idx = new int[26];
        String s = "croak";
        for (int i = 0; i < 5; ++i) {
            idx[s.charAt(i) - 'a'] = i;
        }
        int[] cnt = new int[5];
        int ans = 0, x = 0;
        for (int k = 0; k < n; ++k) {
            int i = idx[croakOfFrogs.charAt(k) - 'a'];
            ++cnt[i];
            if (i == 0) {
                ans = Math.max(ans, ++x);
            } else {
                if (--cnt[i - 1] < 0) {
                    return -1;
                }
                if (i == 4) {
                    --x;
                }
            }
        }
        return x > 0 ? -1 : ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minNumberOfFrogs(string croakOfFrogs) {
        int n = croakOfFrogs.size();
        if (n % 5 != 0) {
            return -1;
        }
        int idx[26]{};
        string s = "croak";
        for (int i = 0; i < 5; ++i) {
            idx[s[i] - 'a'] = i;
        }
        int cnt[5]{};
        int ans = 0, x = 0;
        for (char& c : croakOfFrogs) {
            int i = idx[c - 'a'];
            ++cnt[i];
            if (i == 0) {
                ans = max(ans, ++x);
            } else {
                if (--cnt[i - 1] < 0) {
                    return -1;
                }
                if (i == 4) {
                    --x;
                }
            }
        }
        return x > 0 ? -1 : ans;
    }
};
```

#### Go

```go
func minNumberOfFrogs(croakOfFrogs string) int {
	n := len(croakOfFrogs)
	if n%5 != 0 {
		return -1
	}
	idx := [26]int{}
	for i, c := range "croak" {
		idx[c-'a'] = i
	}
	cnt := [5]int{}
	ans, x := 0, 0
	for _, c := range croakOfFrogs {
		i := idx[c-'a']
		cnt[i]++
		if i == 0 {
			x++
			ans = max(ans, x)
		} else {
			cnt[i-1]--
			if cnt[i-1] < 0 {
				return -1
			}
			if i == 4 {
				x--
			}
		}
	}
	if x > 0 {
		return -1
	}
	return ans
}
```

#### TypeScript

```ts
function minNumberOfFrogs(croakOfFrogs: string): number {
    const n = croakOfFrogs.length;
    if (n % 5 !== 0) {
        return -1;
    }
    const idx = (c: string): number => 'croak'.indexOf(c);
    const cnt: number[] = [0, 0, 0, 0, 0];
    let ans = 0;
    let x = 0;
    for (const c of croakOfFrogs) {
        const i = idx(c);
        ++cnt[i];
        if (i === 0) {
            ans = Math.max(ans, ++x);
        } else {
            if (--cnt[i - 1] < 0) {
                return -1;
            }
            if (i === 4) {
                --x;
            }
        }
    }
    return x > 0 ? -1 : ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
