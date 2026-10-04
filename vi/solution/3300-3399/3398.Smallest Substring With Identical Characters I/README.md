---
comments: true
difficulty: Hard
rating: 2301
source: Weekly Contest 429 Q3
tags:
    - Array
    - Binary Search
    - Enumeration
---

<!-- problem:start -->

# [3398. Smallest Substring With Identical Characters I](https://leetcode.com/problems/smallest-substring-with-identical-characters-i)

[中文文档](/solution/3300-3399/3398.Smallest%20Substring%20With%20Identical%20Characters%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi nhị phân <code>s</code> có độ dài <code>n</code> và một số nguyên <code>numOps</code>.</p>

<p>Bạn được phép thực hiện thao tác sau trên <code>s</code> <strong>nhiều nhất</strong> <code>numOps</code> lần:</p>

<ul>
	<li>Chọn một chỉ số bất kỳ <code>i</code> (với <code>0 &lt;= i &lt; n</code>) và <strong>lật</strong> <code>s[i]</code>. Nếu <code>s[i] == &#39;1&#39;</code>, đổi <code>s[i]</code> thành <code>&#39;0&#39;</code> và ngược lại.</li>
</ul>

<p>Bạn cần <strong>tối thiểu hóa</strong> độ dài của <strong>dài nhất</strong> <span data-keyword="substring-nonempty">substring</span> của <code>s</code> sao cho tất cả ký tự trong substring đều <strong>giống nhau</strong>.</p>

<p>Trả về độ dài <strong>nhỏ nhất</strong> sau khi thực hiện các thao tác.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;000001&quot;, numOps = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong>&nbsp;</p>

<p>Bằng cách đổi <code>s[2]</code> thành <code>&#39;1&#39;</code>, <code>s</code> trở thành <code>&quot;001001&quot;</code>. Các substring dài nhất có các ký tự giống nhau là <code>s[0..1]</code> và <code>s[3..4]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;0000&quot;, numOps = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong>&nbsp;</p>

<p>Bằng cách đổi <code>s[0]</code> và <code>s[2]</code> thành <code>&#39;1&#39;</code>, <code>s</code> trở thành <code>&quot;1010&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;0101&quot;, numOps = 0</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == s.length &lt;= 1000</code></li>
	<li><code>s</code> chỉ gồm <code>&#39;0&#39;</code> và <code>&#39;1&#39;</code>.</li>
	<li><code>0 &lt;= numOps &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta có thể lật nhiều nhất $\textit{numOps}$ bit để tối thiểu hóa đoạn liên tiếp dài nhất gồm các ký tự giống nhau. Với $n \le 1000$, ta dùng tìm kiếm nhị phân trên độ dài mục tiêu $m$.
>
> Với $m=1$, chuỗi phải trở thành `0101...` hoặc `1010...`; ta chọn mẫu gần với chuỗi ban đầu hơn. Với $m>1$, một đoạn dài $k$ cần $\lfloor k/(m+1) \rfloor$ lần lật.
>
> $m$ khả thi khi tổng số lần lật không vượt quá $\textit{numOps}$. Giá trị nhỏ nhất của $m$ thỏa mãn điều kiện là đáp án.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minLength(self, s: str, numOps: int) -> int:
        def check(m: int) -> bool:
            cnt = 0
            if m == 1:
                t = "01"
                cnt = sum(c == t[i & 1] for i, c in enumerate(s))
                cnt = min(cnt, n - cnt)
            else:
                k = 0
                for i, c in enumerate(s):
                    k += 1
                    if i == len(s) - 1 or c != s[i + 1]:
                        cnt += k // (m + 1)
                        k = 0
            return cnt <= numOps

        n = len(s)
        return bisect_left(range(n), True, lo=1, key=check)
```

#### Java

```java
class Solution {
    private char[] s;
    private int numOps;

    public int minLength(String s, int numOps) {
        this.numOps = numOps;
        this.s = s.toCharArray();
        int l = 1, r = s.length();
        while (l < r) {
            int mid = (l + r) >> 1;
            if (check(mid)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }

    private boolean check(int m) {
        int cnt = 0;
        if (m == 1) {
            char[] t = {'0', '1'};
            for (int i = 0; i < s.length; ++i) {
                if (s[i] == t[i & 1]) {
                    ++cnt;
                }
            }
            cnt = Math.min(cnt, s.length - cnt);
        } else {
            int k = 0;
            for (int i = 0; i < s.length; ++i) {
                ++k;
                if (i == s.length - 1 || s[i] != s[i + 1]) {
                    cnt += k / (m + 1);
                    k = 0;
                }
            }
        }
        return cnt <= numOps;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minLength(string s, int numOps) {
        int n = s.size();
        auto check = [&](int m) {
            int cnt = 0;
            if (m == 1) {
                string t = "01";
                for (int i = 0; i < n; ++i) {
                    if (s[i] == t[i & 1]) {
                        ++cnt;
                    }
                }
                cnt = min(cnt, n - cnt);
            } else {
                int k = 0;
                for (int i = 0; i < n; ++i) {
                    ++k;
                    if (i == n - 1 || s[i] != s[i + 1]) {
                        cnt += k / (m + 1);
                        k = 0;
                    }
                }
            }
            return cnt <= numOps;
        };
        int l = 1, r = n;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (check(mid)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
};
```

#### Go

```go
func minLength(s string, numOps int) int {
	check := func(m int) bool {
		m++
		cnt := 0
		if m == 1 {
			t := "01"
			for i := range s {
				if s[i] == t[i&1] {
					cnt++
				}
			}
			cnt = min(cnt, len(s)-cnt)
		} else {
			k := 0
			for i := range s {
				k++
				if i == len(s)-1 || s[i] != s[i+1] {
					cnt += k / (m + 1)
					k = 0
				}
			}
		}
		return cnt <= numOps
	}
	return 1 + sort.Search(len(s), func(m int) bool { return check(m) })
}
```

#### TypeScript

```ts
function minLength(s: string, numOps: number): number {
    const n = s.length;
    const check = (m: number): boolean => {
        let cnt = 0;
        if (m === 1) {
            const t = '01';
            for (let i = 0; i < n; ++i) {
                if (s[i] === t[i & 1]) {
                    ++cnt;
                }
            }
            cnt = Math.min(cnt, n - cnt);
        } else {
            let k = 0;
            for (let i = 0; i < n; ++i) {
                ++k;
                if (i === n - 1 || s[i] !== s[i + 1]) {
                    cnt += Math.floor(k / (m + 1));
                    k = 0;
                }
            }
        }
        return cnt <= numOps;
    };
    let [l, r] = [1, n];
    while (l < r) {
        const mid = (l + r) >> 1;
        if (check(mid)) {
            r = mid;
        } else {
            l = mid + 1;
        }
    }
    return l;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
