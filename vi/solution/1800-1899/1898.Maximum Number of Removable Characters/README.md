---
comments: true
difficulty: Medium
rating: 1912
source: Weekly Contest 245 Q2
tags:
    - Array
    - Two Pointers
    - String
    - Binary Search
---

<!-- problem:start -->

# [1898. Maximum Number of Removable Characters](https://leetcode.com/problems/maximum-number-of-removable-characters)

[中文文档](/solution/1800-1899/1898.Maximum%20Number%20of%20Removable%20Characters/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>s</code> và <code>p</code>, trong đó <code>p</code> là một <strong>subsequence</strong> của <code>s</code>. Ngoài ra, cho một mảng số nguyên <strong>đánh chỉ số từ 0 và không chứa phần tử trùng nhau</strong> <code>removable</code>, chứa một tập con các chỉ số của <code>s</code> (<code>s</code> cũng <strong>đánh chỉ số từ 0</strong>).</p>

<p>Bạn muốn chọn một số nguyên <code>k</code> (<code>0 &lt;= k &lt;= removable.length</code>) sao cho sau khi xóa <code>k</code> ký tự khỏi <code>s</code> bằng <strong>các chỉ số đầu tiên</strong> <code>k</code> trong <code>removable</code>, <code>p</code> vẫn là một <strong>subsequence</strong> của <code>s</code>. Cụ thể hơn, với mỗi <code>0 &lt;= i &lt; k</code>, ta đánh dấu ký tự tại <code>s[removable[i]]</code>, sau đó xóa mọi ký tự đã đánh dấu và kiểm tra xem <code>p</code> còn là subsequence hay không.</p>

<p>Hãy trả về <em><strong>giá trị lớn nhất</strong> của </em><code>k</code><em> có thể chọn sao cho </em><code>p</code><em> vẫn là một <strong>subsequence</strong> của </em><code>s</code><em> sau khi xóa</em>.</p>

<p>Một <strong>subsequence</strong> của chuỗi là chuỗi mới được tạo từ chuỗi ban đầu bằng cách xóa một số ký tự (có thể không xóa ký tự nào) mà không thay đổi thứ tự tương đối của các ký tự còn lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abcacb&quot;, p = &quot;ab&quot;, removable = [3,1,0]
<strong>Đầu ra:</strong> 2
<strong>Giải thích</strong>: Sau khi xóa các ký tự tại chỉ số 3 và 1, &quot;a<s><strong>b</strong></s>c<s><strong>a</strong></s>cb&quot; trở thành &quot;accb&quot;.
&quot;ab&quot; là một subsequence của &quot;<strong><u>a</u></strong>cc<strong><u>b</u></strong>&quot;.
Nếu xóa các ký tự tại chỉ số 3, 1 và 0, &quot;<s><strong>ab</strong></s>c<s><strong>a</strong></s>cb&quot; trở thành &quot;ccb&quot;, và &quot;ab&quot; không còn là subsequence.
Vì vậy, k lớn nhất là 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abcbddddd&quot;, p = &quot;abcd&quot;, removable = [3,2,1,4,5,6]
<strong>Đầu ra:</strong> 1
<strong>Giải thích</strong>: Sau khi xóa ký tự tại chỉ số 3, &quot;abc<s><strong>b</strong></s>ddddd&quot; trở thành &quot;abcddddd&quot;.
&quot;abcd&quot; là một subsequence của &quot;<u><strong>abcd</strong></u>dddd&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abcab&quot;, p = &quot;abc&quot;, removable = [0,1,2,3,4]
<strong>Đầu ra:</strong> 0
<strong>Giải thích</strong>: Nếu xóa chỉ số đầu tiên trong mảng removable, &quot;abc&quot; không còn là một subsequence.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= p.length &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= removable.length &lt; s.length</code></li>
	<li><code>0 &lt;= removable[i] &lt; s.length</code></li>
	<li><code>p</code> là một <strong>subsequence</strong> của <code>s</code>.</li>
	<li><code>s</code> và <code>p</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li>Các phần tử trong <code>removable</code> là <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Ta xóa $s[\textit{removable}[0..k))$ theo thứ tự và cần giá trị $k$ lớn nhất sao cho $p$ vẫn là subsequence. Xóa càng nhiều ký tự thì điều kiện càng khó thỏa mãn.
>
> Tìm kiếm nhị phân trên $k$, đánh dấu $k$ chỉ số đầu tiên rồi duyệt $s$ để đối chiếu với $p$. Nếu $p$ vẫn khớp, thử giá trị $k$ lớn hơn.

<!-- thinking:end -->

Ta nhận thấy nếu sau khi xóa các ký tự tại $k$ chỉ số đầu tiên trong $\textit{removable}$ mà $p$ vẫn là một subsequence của $s$, thì xóa các ký tự tại $k \lt k' \leq \textit{removable.length}$ chỉ số đầu tiên cũng sẽ thỏa mãn điều kiện. Tính đơn điệu này cho phép ta dùng tìm kiếm nhị phân để tìm $k$ lớn nhất.

Ta đặt biên trái của tìm kiếm nhị phân là $l = 0$ và biên phải là $r = \textit{removable.length}$. Sau đó, ta thực hiện tìm kiếm nhị phân. Ở mỗi bước, ta lấy giá trị giữa $mid = \left\lfloor \frac{l + r + 1}{2} \right\rfloor$ và kiểm tra xem sau khi xóa các ký tự tại $mid$ chỉ số đầu tiên trong $\textit{removable}$ thì $p$ còn là một subsequence của $s$ hay không. Nếu có, ta cập nhật biên trái $l = mid$; nếu không, cập nhật biên phải $r = mid - 1$.

Sau khi kết thúc tìm kiếm nhị phân, ta trả về biên trái $l$.

Độ phức tạp thời gian là $O(k \times \log k)$, còn độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài chuỗi $s$ và $k$ là độ dài của $\textit{removable}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumRemovals(self, s: str, p: str, removable: List[int]) -> int:
        def check(k: int) -> bool:
            rem = [False] * len(s)
            for i in removable[:k]:
                rem[i] = True
            i = j = 0
            while i < len(s) and j < len(p):
                if not rem[i] and p[j] == s[i]:
                    j += 1
                i += 1
            return j == len(p)

        l, r = 0, len(removable)
        while l < r:
            mid = (l + r + 1) >> 1
            if check(mid):
                l = mid
            else:
                r = mid - 1
        return l
```

#### Java

```java
class Solution {
    private char[] s;
    private char[] p;
    private int[] removable;

    public int maximumRemovals(String s, String p, int[] removable) {
        int l = 0, r = removable.length;
        this.s = s.toCharArray();
        this.p = p.toCharArray();
        this.removable = removable;
        while (l < r) {
            int mid = (l + r + 1) >> 1;
            if (check(mid)) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        return l;
    }

    private boolean check(int k) {
        boolean[] rem = new boolean[s.length];
        for (int i = 0; i < k; ++i) {
            rem[removable[i]] = true;
        }
        int i = 0, j = 0;
        while (i < s.length && j < p.length) {
            if (!rem[i] && p[j] == s[i]) {
                ++j;
            }
            ++i;
        }
        return j == p.length;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumRemovals(string s, string p, vector<int>& removable) {
        int m = s.size(), n = p.size();
        int l = 0, r = removable.size();
        bool rem[m];

        auto check = [&](int k) {
            memset(rem, false, sizeof(rem));
            for (int i = 0; i < k; i++) {
                rem[removable[i]] = true;
            }
            int i = 0, j = 0;
            while (i < m && j < n) {
                if (!rem[i] && s[i] == p[j]) {
                    ++j;
                }
                ++i;
            }
            return j == n;
        };
        while (l < r) {
            int mid = (l + r + 1) >> 1;
            if (check(mid)) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        return l;
    }
};
```

#### Go

```go
func maximumRemovals(s string, p string, removable []int) int {
	m, n := len(s), len(p)
	l, r := 0, len(removable)
	check := func(k int) bool {
		rem := make([]bool, m)
		for i := 0; i < k; i++ {
			rem[removable[i]] = true
		}
		i, j := 0, 0
		for i < m && j < n {
			if !rem[i] && s[i] == p[j] {
				j++
			}
			i++
		}
		return j == n
	}
	for l < r {
		mid := (l + r + 1) >> 1
		if check(mid) {
			l = mid
		} else {
			r = mid - 1
		}
	}
	return l
}
```

#### TypeScript

```ts
function maximumRemovals(s: string, p: string, removable: number[]): number {
    const [m, n] = [s.length, p.length];
    let [l, r] = [0, removable.length];
    const rem: boolean[] = Array(m);

    const check = (k: number): boolean => {
        rem.fill(false);
        for (let i = 0; i < k; i++) {
            rem[removable[i]] = true;
        }

        let i = 0,
            j = 0;
        while (i < m && j < n) {
            if (!rem[i] && s[i] === p[j]) {
                j++;
            }
            i++;
        }
        return j === n;
    };

    while (l < r) {
        const mid = (l + r + 1) >> 1;
        if (check(mid)) {
            l = mid;
        } else {
            r = mid - 1;
        }
    }

    return l;
}
```

#### Rust

```rust
impl Solution {
    pub fn maximum_removals(s: String, p: String, removable: Vec<i32>) -> i32 {
        let m = s.len();
        let n = p.len();
        let s: Vec<char> = s.chars().collect();
        let p: Vec<char> = p.chars().collect();
        let mut l = 0;
        let mut r = removable.len();

        let check = |k: usize| -> bool {
            let mut rem = vec![false; m];
            for i in 0..k {
                rem[removable[i] as usize] = true;
            }
            let mut i = 0;
            let mut j = 0;
            while i < m && j < n {
                if !rem[i] && s[i] == p[j] {
                    j += 1;
                }
                i += 1;
            }
            j == n
        };

        while l < r {
            let mid = (l + r + 1) / 2;
            if check(mid) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }

        l as i32
    }
}
```

#### JavaScript

```js
/**
 * @param {string} s
 * @param {string} p
 * @param {number[]} removable
 * @return {number}
 */
var maximumRemovals = function (s, p, removable) {
    const [m, n] = [s.length, p.length];
    let [l, r] = [0, removable.length];
    const rem = Array(m);

    const check = k => {
        rem.fill(false);
        for (let i = 0; i < k; i++) {
            rem[removable[i]] = true;
        }

        let i = 0,
            j = 0;
        while (i < m && j < n) {
            if (!rem[i] && s[i] === p[j]) {
                j++;
            }
            i++;
        }
        return j === n;
    };

    while (l < r) {
        const mid = (l + r + 1) >> 1;
        if (check(mid)) {
            l = mid;
        } else {
            r = mid - 1;
        }
    }

    return l;
};
```

#### Kotlin

```kotlin
class Solution {
    fun maximumRemovals(s: String, p: String, removable: IntArray): Int {
        val m = s.length
        val n = p.length
        var l = 0
        var r = removable.size

        fun check(k: Int): Boolean {
            val rem = BooleanArray(m)
            for (i in 0 until k) {
                rem[removable[i]] = true
            }
            var i = 0
            var j = 0
            while (i < m && j < n) {
                if (!rem[i] && s[i] == p[j]) {
                    j++
                }
                i++
            }
            return j == n
        }

        while (l < r) {
            val mid = (l + r + 1) / 2
            if (check(mid)) {
                l = mid
            } else {
                r = mid - 1
            }
        }

        return l
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
