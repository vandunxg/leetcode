---
comments: true
difficulty: Medium
rating: 1505
source: Weekly Contest 378 Q2
tags:
    - Hash Table
    - String
    - Binary Search
    - Counting
    - Sliding Window
---

<!-- problem:start -->

# [2981. Find Longest Special Substring That Occurs Thrice I](https://leetcode.com/problems/find-longest-special-substring-that-occurs-thrice-i)

[中文文档](/solution/2900-2999/2981.Find%20Longest%20Special%20Substring%20That%20Occurs%20Thrice%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</p>

<p>Một chuỗi được gọi là <strong>đặc biệt</strong> nếu nó chỉ gồm một ký tự duy nhất. Ví dụ, chuỗi <code>&quot;abc&quot;</code> không đặc biệt, trong khi các chuỗi <code>&quot;ddd&quot;</code>, <code>&quot;zz&quot;</code> và <code>&quot;f&quot;</code> là các chuỗi đặc biệt.</p>

<p>Trả về <em>độ dài của <strong>chuỗi con đặc biệt dài nhất</strong> trong </em><code>s</code> <em>xuất hiện <strong>ít nhất ba lần</strong></em>, <em>hoặc </em><code>-1</code><em> nếu không có chuỗi con đặc biệt nào xuất hiện ít nhất ba lần</em>.</p>

<p><strong>Chuỗi con</strong> là một dãy ký tự <strong>liên tiếp, không rỗng</strong> trong một chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aaaa&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Chuỗi con đặc biệt dài nhất xuất hiện ba lần là &quot;aa&quot;: các chuỗi con &quot;<u><strong>aa</strong></u>aa&quot;, &quot;a<u><strong>aa</strong></u>a&quot; và &quot;aa<u><strong>aa</strong></u>&quot;.
Có thể chứng minh rằng độ dài lớn nhất có thể đạt được là 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abcdef&quot;
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không có chuỗi con đặc biệt nào xuất hiện ít nhất ba lần. Do đó, trả về -1.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abcaba&quot;
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Chuỗi con đặc biệt dài nhất xuất hiện ba lần là &quot;a&quot;: các chuỗi con &quot;<u><strong>a</strong></u>bcaba&quot;, &quot;abc<u><strong>a</strong></u>ba&quot; và &quot;abcab<u><strong>a</strong></u>&quot;.
Có thể chứng minh rằng độ dài lớn nhất có thể đạt được là 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= s.length &lt;= 50</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân + Đếm bằng cửa sổ trượt

<!-- thinking:start -->

> **Tư duy**
>
> Chuỗi con đặc biệt là một đoạn liên tiếp chỉ gồm một chữ cái. Nếu độ dài $x$ xuất hiện ba lần thì độ dài $x-1$ cũng xuất hiện, nên ta có thể tìm kiếm nhị phân $x$. Một đoạn liên tiếp có độ dài $L$ đóng góp $\max(0, L-x+1)$ chuỗi con độ dài $x$; ta cộng theo từng chữ cái và kiểm tra xem tổng có $\ge 3$ hay không.
>
> Vì $n \le 50$, tìm kiếm nhị phân kết hợp với phép đếm tuyến tính là đủ.

<!-- thinking:end -->

Ta nhận thấy rằng nếu tồn tại một chuỗi con đặc biệt có độ dài $x$ xuất hiện ít nhất ba lần, thì chuỗi con đặc biệt có độ dài $x-1$ cũng chắc chắn tồn tại. Điều này thể hiện tính đơn điệu, nên ta có thể dùng tìm kiếm nhị phân để tìm chuỗi con đặc biệt dài nhất.

Ta đặt biên trái của tìm kiếm nhị phân là $l = 0$ và biên phải là $r = n$, trong đó $n$ là độ dài chuỗi. Ở mỗi bước tìm kiếm nhị phân, ta lấy $mid = \lfloor \frac{l + r + 1}{2} \rfloor$. Nếu tồn tại chuỗi con đặc biệt có độ dài $mid$, ta cập nhật biên trái thành $mid$. Ngược lại, ta cập nhật biên phải thành $mid - 1$. Trong quá trình tìm kiếm nhị phân, ta dùng cửa sổ trượt để đếm số chuỗi con đặc biệt.

Cụ thể, ta thiết kế hàm $check(x)$ để kiểm tra xem có tồn tại chuỗi con đặc biệt độ dài $x$ xuất hiện ít nhất ba lần hay không.

Trong hàm $check(x)$, ta định nghĩa một bảng băm hoặc một mảng độ dài $26$ có tên $cnt$, trong đó $cnt[i]$ là số chuỗi con đặc biệt độ dài $x$ được tạo bởi chữ cái tiếng Anh viết thường thứ $i$. Ta duyệt chuỗi $s$. Nếu ký tự hiện tại là $s[i]$, ta di chuyển con trỏ $j$ sang phải cho đến khi $s[j] \neq s[i]$. Khi đó, $s[i \cdots j-1]$ là một chuỗi con đặc biệt có độ dài $x$. Ta tăng $cnt[s[i]]$ thêm $\max(0, j - i - x + 1)$, sau đó cập nhật con trỏ $i$ thành $j$.

Sau khi duyệt xong, ta duyệt qua mảng $cnt$. Nếu tồn tại $cnt[i] \geq 3$, điều đó có nghĩa là tồn tại một chuỗi con đặc biệt độ dài $x$ xuất hiện ít nhất ba lần, nên ta trả về $true$. Nếu không, ta trả về $false$.

Độ phức tạp thời gian là $O((n + |\Sigma|) \times \log n)$, và độ phức tạp không gian là $O(|\Sigma|)$, trong đó $n$ là độ dài chuỗi $s$, còn $|\Sigma|$ là kích thước của tập ký tự. Trong bài toán này, tập ký tự là các chữ cái tiếng Anh viết thường, nên $|\Sigma| = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumLength(self, s: str) -> int:
        def check(x: int) -> bool:
            cnt = defaultdict(int)
            i = 0
            while i < n:
                j = i + 1
                while j < n and s[j] == s[i]:
                    j += 1
                cnt[s[i]] += max(0, j - i - x + 1)
                i = j
            return max(cnt.values()) >= 3

        n = len(s)
        l, r = 0, n
        while l < r:
            mid = (l + r + 1) >> 1
            if check(mid):
                l = mid
            else:
                r = mid - 1
        return -1 if l == 0 else l
```

#### Java

```java
class Solution {
    private String s;
    private int n;

    public int maximumLength(String s) {
        this.s = s;
        n = s.length();
        int l = 0, r = n;
        while (l < r) {
            int mid = (l + r + 1) >> 1;
            if (check(mid)) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        return l == 0 ? -1 : l;
    }

    private boolean check(int x) {
        int[] cnt = new int[26];
        for (int i = 0; i < n;) {
            int j = i + 1;
            while (j < n && s.charAt(j) == s.charAt(i)) {
                j++;
            }
            int k = s.charAt(i) - 'a';
            cnt[k] += Math.max(0, j - i - x + 1);
            if (cnt[k] >= 3) {
                return true;
            }
            i = j;
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumLength(string s) {
        int n = s.size();
        int l = 0, r = n;
        auto check = [&](int x) {
            int cnt[26]{};
            for (int i = 0; i < n;) {
                int j = i + 1;
                while (j < n && s[j] == s[i]) {
                    ++j;
                }
                int k = s[i] - 'a';
                cnt[k] += max(0, j - i - x + 1);
                if (cnt[k] >= 3) {
                    return true;
                }
                i = j;
            }
            return false;
        };
        while (l < r) {
            int mid = (l + r + 1) >> 1;
            if (check(mid)) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        return l == 0 ? -1 : l;
    }
};
```

#### Go

```go
func maximumLength(s string) int {
	n := len(s)
	l, r := 0, n
	check := func(x int) bool {
		cnt := [26]int{}
		for i := 0; i < n; {
			j := i + 1
			for j < n && s[j] == s[i] {
				j++
			}
			k := s[i] - 'a'
			cnt[k] += max(0, j-i-x+1)
			if cnt[k] >= 3 {
				return true
			}
			i = j
		}
		return false
	}
	for l < r {
		mid := (l + r + 1) >> 1
		if check(mid) {
			l = mid
		} else {
			r = mid - 1
		}
	}
	if l == 0 {
		return -1
	}
	return l
}
```

#### TypeScript

```ts
function maximumLength(s: string): number {
    const n = s.length;
    let [l, r] = [0, n];
    const check = (x: number): boolean => {
        const cnt: number[] = Array(26).fill(0);
        for (let i = 0; i < n;) {
            let j = i + 1;
            while (j < n && s[j] === s[i]) {
                j++;
            }
            const k = s[i].charCodeAt(0) - 'a'.charCodeAt(0);
            cnt[k] += Math.max(0, j - i - x + 1);
            if (cnt[k] >= 3) {
                return true;
            }
            i = j;
        }
        return false;
    };
    while (l < r) {
        const mid = (l + r + 1) >> 1;
        if (check(mid)) {
            l = mid;
        } else {
            r = mid - 1;
        }
    }
    return l === 0 ? -1 : l;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 duyệt lại chuỗi ở mỗi giá trị mid. Vì $n$ nhỏ, với mỗi đoạn liên tiếp, ta có thể thêm mọi độ dài $j \in [1,L]$ vào một map $L-j+1$ lần, sau đó giữ lại key dài nhất có số lần xuất hiện $\ge 3$.
>
> Cách này loại bỏ các vòng lặp logarit mà vẫn chỉ xử lý các đoạn liên tiếp, phù hợp với chuỗi có độ dài rất nhỏ.

<!-- thinking:end -->

Độ phức tạp thời gian là $O(n)$

<!-- tabs:start -->

#### TypeScript

```ts
function maximumLength(s: string): number {
    const cnt = new Map<string, number>();
    const n = s.length;
    let [c, ch] = [0, ''];

    for (let i = 0; i < n + 1; i++) {
        if (ch === s[i]) {
            c++;
        } else {
            let j = 1;
            while (c) {
                const char = ch.repeat(j++);
                cnt.set(char, (cnt.get(char) ?? 0) + c);
                c--;
            }

            ch = s[i];
            c = 1;
        }
    }

    let res = -1;
    for (const [x, c] of cnt) {
        if (c >= 3) {
            res = Math.max(res, x.length);
        }
    }

    return res;
}
```

### JavaScript

```js
function maximumLength(s) {
    const cnt = new Map();
    const n = s.length;
    let [c, ch] = [0, ''];

    for (let i = 0; i < n + 1; i++) {
        if (ch === s[i]) {
            c++;
        } else {
            let j = 1;
            while (c) {
                const char = ch.repeat(j++);
                cnt.set(char, (cnt.get(char) ?? 0) + c);
                c--;
            }

            ch = s[i];
            c = 1;
        }
    }

    let res = -1;
    for (const [x, c] of cnt) {
        if (c >= 3) {
            res = Math.max(res, x.length);
        }
    }

    return res;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
