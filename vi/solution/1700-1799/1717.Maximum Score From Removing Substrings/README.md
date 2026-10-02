---
comments: true
difficulty: Medium
rating: 1867
source: Biweekly Contest 43 Q2
tags:
    - Stack
    - Greedy
    - String
---

<!-- problem:start -->

# [1717. Maximum Score From Removing Substrings](https://leetcode.com/problems/maximum-score-from-removing-substrings)

[中文文档](/solution/1700-1799/1717.Maximum%20Score%20From%20Removing%20Substrings/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code> và hai số nguyên <code>x</code>, <code>y</code>. Bạn có thể thực hiện hai loại thao tác không giới hạn số lần.</p>

<ul>
	<li>Xóa chuỗi con <code>&quot;ab&quot;</code> và nhận <code>x</code> điểm.

    <ul>
      <li>Ví dụ, xóa <code>&quot;ab&quot;</code> khỏi <code>&quot;c<u>ab</u>xbae&quot;</code> sẽ được <code>&quot;cxbae&quot;</code>.</li>
    </ul>
    </li>
    <li>Xóa chuỗi con <code>&quot;ba&quot;</code> và nhận <code>y</code> điểm.
    <ul>
      <li>Ví dụ, xóa <code>&quot;ba&quot;</code> khỏi <code>&quot;cabx<u>ba</u>e&quot;</code> sẽ được <code>&quot;cabxe&quot;</code>.</li>
    </ul>
    </li>

</ul>

<p>Trả về <em>số điểm lớn nhất có thể nhận được sau khi thực hiện các thao tác trên với</em> <code>s</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;cdbcbbaaabab&quot;, x = 4, y = 5
<strong>Output:</strong> 19
<strong>Explanation:</strong>
- Xóa &quot;ba&quot; được gạch chân trong &quot;cdbcbbaaa<u>ba</u>b&quot;. Khi đó s = &quot;cdbcbbaaab&quot; và điểm tăng 5.
- Xóa &quot;ab&quot; được gạch chân trong &quot;cdbcbbaa<u>ab</u>&quot;. Khi đó s = &quot;cdbcbbaa&quot; và điểm tăng 4.
- Xóa &quot;ba&quot; được gạch chân trong &quot;cdbcb<u>ba</u>a&quot;. Khi đó s = &quot;cdbcba&quot; và điểm tăng 5.
- Xóa &quot;ba&quot; được gạch chân trong &quot;cdbc<u>ba</u>&quot;. Khi đó s = &quot;cdbc&quot; và điểm tăng 5.
Tổng điểm = 5 + 4 + 5 + 5 = 19.</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;aabbaaxybbaabb&quot;, x = 5, y = 4
<strong>Output:</strong> 20
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= x, y &lt;= 10<sup>4</sup></code></li>
	<li><code>s</code> consists of lowercase English letters.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy

<!-- thinking:start -->

> **Tư duy**
>
> Xóa $\textit{ab}$ được $x$ điểm và xóa $\textit{ba}$ được $y$ điểm; mỗi lần xóa lại thay đổi các cặp kề nhau. Tìm mọi thứ tự xóa là không khả thi với chuỗi dài.
>
> Một đoạn chỉ gồm $a$ và $b$ cuối cùng luôn còn lại một loại ký tự; số thao tác được cố định bởi số lượng hai ký tự. Cần ưu tiên các cặp có điểm cao hơn.
>
> Đổi hai ký tự và điểm khi $x<y$. Khi duyệt, ghép cặp có điểm cao với một bộ đếm; tại ký tự phân cách hoặc cuối chuỗi, xử lý cặp điểm thấp bằng $\min(cnt_a,cnt_b)$.

<!-- thinking:end -->

Ta có thể giả sử điểm của chuỗi con "ab" luôn không nhỏ hơn điểm của "ba". Nếu không, ta đổi "a" và "b", đồng thời đổi $x$ và $y$.

Tiếp theo, ta chỉ cần xét trường hợp chuỗi chỉ chứa "a" và "b". Nếu chuỗi có ký tự khác, ta xem chúng là điểm phân cách, chia chuỗi thành các chuỗi con chỉ chứa "a" và "b", rồi tính điểm riêng cho từng chuỗi.

Ta nhận thấy với chuỗi con chỉ chứa "a" và "b", dù thực hiện thao tác nào thì cuối cùng cũng chỉ còn một loại ký tự hoặc chuỗi rỗng. Vì mỗi thao tác đồng thời xóa một "a" và một "b", tổng số thao tác là cố định. Ta greedy xóa "ab" trước rồi xóa "ba" để đạt điểm tối đa.

Do đó, ta dùng hai biến $\textit{cnt1}$ và $\textit{cnt2}$ lần lượt đếm "a" và "b", rồi duyệt chuỗi, cập nhật $\textit{cnt1}$ và $\textit{cnt2}$ theo ký tự hiện tại và đồng thời tính điểm.

Với ký tự hiện tại $c$:

- Nếu $c$ là "a", vì muốn xóa "ab" trước nên chưa xóa ký tự này, chỉ tăng $\textit{cnt1}$;
- Nếu $c$ là "b" và $\textit{cnt1} > 0$, ta xóa được một "ab" và cộng $x$ điểm; ngược lại, chỉ tăng $\textit{cnt2}$;
- Nếu $c$ là ký tự khác, chuỗi con hiện tại còn $\textit{cnt2}$ ký tự "b" và $\textit{cnt1}$ ký tự "a". Ta xóa được $\min(\textit{cnt1}, \textit{cnt2})$ cặp "ba" và cộng số điểm $y$ tương ứng.

Sau khi duyệt, ta xử lý các cặp "ba" còn lại và cộng số điểm $y$ tương ứng.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumGain(self, s: str, x: int, y: int) -> int:
        a, b = "a", "b"
        if x < y:
            x, y = y, x
            a, b = b, a
        ans = cnt1 = cnt2 = 0
        for c in s:
            if c == a:
                cnt1 += 1
            elif c == b:
                if cnt1:
                    ans += x
                    cnt1 -= 1
                else:
                    cnt2 += 1
            else:
                ans += min(cnt1, cnt2) * y
                cnt1 = cnt2 = 0
        ans += min(cnt1, cnt2) * y
        return ans
```

#### Java

```java
class Solution {
    public int maximumGain(String s, int x, int y) {
        char a = 'a', b = 'b';
        if (x < y) {
            int t = x;
            x = y;
            y = t;
            char c = a;
            a = b;
            b = c;
        }
        int ans = 0, cnt1 = 0, cnt2 = 0;
        int n = s.length();
        for (int i = 0; i < n; ++i) {
            char c = s.charAt(i);
            if (c == a) {
                cnt1++;
            } else if (c == b) {
                if (cnt1 > 0) {
                    ans += x;
                    cnt1--;
                } else {
                    cnt2++;
                }
            } else {
                ans += Math.min(cnt1, cnt2) * y;
                cnt1 = 0;
                cnt2 = 0;
            }
        }
        ans += Math.min(cnt1, cnt2) * y;
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumGain(string s, int x, int y) {
        char a = 'a', b = 'b';
        if (x < y) {
            swap(x, y);
            swap(a, b);
        }

        int ans = 0, cnt1 = 0, cnt2 = 0;
        for (char c : s) {
            if (c == a) {
                cnt1++;
            } else if (c == b) {
                if (cnt1) {
                    ans += x;
                    cnt1--;
                } else {
                    cnt2++;
                }
            } else {
                ans += min(cnt1, cnt2) * y;
                cnt1 = 0;
                cnt2 = 0;
            }
        }
        ans += min(cnt1, cnt2) * y;
        return ans;
    }
};
```

#### Go

```go
func maximumGain(s string, x int, y int) (ans int) {
	a, b := 'a', 'b'
	if x < y {
		x, y = y, x
		a, b = b, a
	}

	var cnt1, cnt2 int
	for _, c := range s {
		if c == a {
			cnt1++
		} else if c == b {
			if cnt1 > 0 {
				ans += x
				cnt1--
			} else {
				cnt2++
			}
		} else {
			ans += min(cnt1, cnt2) * y
			cnt1, cnt2 = 0, 0
		}
	}
	ans += min(cnt1, cnt2) * y
	return
}
```

#### TypeScript

```ts
function maximumGain(s: string, x: number, y: number): number {
    let [a, b] = ['a', 'b'];
    if (x < y) {
        [x, y] = [y, x];
        [a, b] = [b, a];
    }

    let [ans, cnt1, cnt2] = [0, 0, 0];
    for (let c of s) {
        if (c === a) {
            cnt1++;
        } else if (c === b) {
            if (cnt1) {
                ans += x;
                cnt1--;
            } else {
                cnt2++;
            }
        } else {
            ans += Math.min(cnt1, cnt2) * y;
            cnt1 = 0;
            cnt2 = 0;
        }
    }
    ans += Math.min(cnt1, cnt2) * y;
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn maximum_gain(s: String, mut x: i32, mut y: i32) -> i32 {
        let (mut a, mut b) = ('a', 'b');
        if x < y {
            std::mem::swap(&mut x, &mut y);
            std::mem::swap(&mut a, &mut b);
        }

        let mut ans = 0;
        let mut cnt1 = 0;
        let mut cnt2 = 0;

        for c in s.chars() {
            if c == a {
                cnt1 += 1;
            } else if c == b {
                if cnt1 > 0 {
                    ans += x;
                    cnt1 -= 1;
                } else {
                    cnt2 += 1;
                }
            } else {
                ans += cnt1.min(cnt2) * y;
                cnt1 = 0;
                cnt2 = 0;
            }
        }

        ans += cnt1.min(cnt2) * y;
        ans
    }
}
```

#### JavaScript

```js
function maximumGain(s, x, y) {
    let [a, b] = ['a', 'b'];
    if (x < y) {
        [x, y] = [y, x];
        [a, b] = [b, a];
    }

    let [ans, cnt1, cnt2] = [0, 0, 0];
    for (let c of s) {
        if (c === a) {
            cnt1++;
        } else if (c === b) {
            if (cnt1) {
                ans += x;
                cnt1--;
            } else {
                cnt2++;
            }
        } else {
            ans += Math.min(cnt1, cnt2) * y;
            cnt1 = 0;
            cnt2 = 0;
        }
    }
    ans += Math.min(cnt1, cnt2) * y;
    return ans;
}
```

#### C#

```cs
public class Solution {
    public int MaximumGain(string s, int x, int y) {
        char a = 'a', b = 'b';
        if (x < y) {
            (x, y) = (y, x);
            (a, b) = (b, a);
        }

        int ans = 0, cnt1 = 0, cnt2 = 0;
        foreach (char c in s) {
            if (c == a) {
                cnt1++;
            } else if (c == b) {
                if (cnt1 > 0) {
                    ans += x;
                    cnt1--;
                } else {
                    cnt2++;
                }
            } else {
                ans += Math.Min(cnt1, cnt2) * y;
                cnt1 = 0;
                cnt2 = 0;
            }
        }

        ans += Math.Min(cnt1, cnt2) * y;
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
