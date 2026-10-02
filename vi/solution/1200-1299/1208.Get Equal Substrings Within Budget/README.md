---
comments: true
difficulty: Medium
rating: 1496
source: Weekly Contest 156 Q2
tags:
    - String
    - Binary Search
    - Prefix Sum
    - Sliding Window
---

<!-- problem:start -->

# [1208. Get Equal Substrings Within Budget](https://leetcode.com/problems/get-equal-substrings-within-budget)

[中文文档](/solution/1200-1299/1208.Get%20Equal%20Substrings%20Within%20Budget/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>s</code> và <code>t</code> có cùng độ dài và số nguyên <code>maxCost</code>.</p>

<p>Bạn muốn biến đổi <code>s</code> thành <code>t</code>. Đổi ký tự thứ <code>i<sup>th</sup></code> của <code>s</code> thành ký tự thứ <code>i<sup>th</sup></code> của <code>t</code> tốn <code>|s[i] - t[i]|</code> (tức trị tuyệt đối của hiệu giữa mã ASCII của hai ký tự).</p>

<p>Trả về <em>độ dài lớn nhất của chuỗi con trong </em><code>s</code><em> có thể được biến đổi thành chuỗi con tương ứng trong </em><code>t</code><em> với chi phí không vượt quá </em><code>maxCost</code>. Nếu không có chuỗi con nào trong <code>s</code> có thể biến đổi thành chuỗi con tương ứng trong <code>t</code>, trả về <code>0</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abcd&quot;, t = &quot;bcdf&quot;, maxCost = 3
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> &quot;abc&quot; trong s có thể được biến đổi thành &quot;bcd&quot;.
Chi phí là 3, nên độ dài lớn nhất là 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abcd&quot;, t = &quot;cdef&quot;, maxCost = 3
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Mỗi ký tự trong s cần chi phí 2 để đổi thành ký tự tương ứng trong t, nên độ dài lớn nhất là 1.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abcd&quot;, t = &quot;acde&quot;, maxCost = 0
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Bạn không thể thực hiện phép đổi nào, nên độ dài lớn nhất là 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>t.length == s.length</code></li>
	<li><code>0 &lt;= maxCost &lt;= 10<sup>6</sup></code></li>
	<li><code>s</code> và <code>t</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tổng tiền tố + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Chi phí của một chuỗi con là tổng trị tuyệt đối của các hiệu mã ASCII. Vì $n \le 10^5$, không thể duyệt hết $O(n^2)$ chuỗi con.
>
> Tính khả thi đơn điệu theo độ dài: nếu có chuỗi dài $x$ vừa ngân sách thì cũng có chuỗi dài $x-1$ vừa ngân sách. Có thể tính chi phí của một đoạn bất kỳ trong $O(1)$ bằng tổng tiền tố.
>
> Ta lập mảng tổng tiền tố của chi phí từng cặp ký tự, tìm kiếm nhị phân theo độ dài rồi duyệt các cửa sổ có độ dài đó để kiểm tra. Tính đơn điệu cho phép tìm kiếm nhị phân; tổng tiền tố loại bỏ vòng lặp tính tổng bên trong.

<!-- thinking:end -->

Ta tạo mảng $f$ có độ dài $n + 1$, trong đó $f[i]$ là tổng trị tuyệt đối của hiệu mã ASCII giữa $i$ ký tự đầu của chuỗi $s$ và $i$ ký tự đầu của chuỗi $t$. Vì vậy, tổng trị tuyệt đối của hiệu mã ASCII từ ký tự thứ $i$ đến ký tự thứ $j$ trong $s$ được tính bằng $f[j + 1] - f[i]$, với $0 \leq i \leq j < n$.

Tính khả thi đơn điệu theo độ dài: nếu tồn tại chuỗi con độ dài $x$ thỏa điều kiện, thì cũng tồn tại chuỗi con độ dài $x - 1$ thỏa điều kiện. Do đó, ta có thể dùng tìm kiếm nhị phân để tìm độ dài lớn nhất.

Ta định nghĩa hàm $check(x)$ để kiểm tra xem có chuỗi con độ dài $x$ thỏa điều kiện hay không. Chỉ cần duyệt tất cả chuỗi con độ dài $x$ và kiểm tra điều kiện. Nếu có chuỗi con phù hợp, hàm trả về `true`; nếu không, trả về `false`.

Đặt biên trái $l = 0$ và biên phải $r = n$. Mỗi bước, tính $mid = \lfloor \frac{l + r + 1}{2} \rfloor$. Nếu $check(mid)$ trả về `true`, cập nhật biên trái thành $mid$; nếu không, cập nhật biên phải thành $mid - 1$. Khi tìm kiếm nhị phân kết thúc, biên trái là đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def equalSubstring(self, s: str, t: str, maxCost: int) -> int:
        def check(x):
            for i in range(n):
                j = i + mid - 1
                if j < n and f[j + 1] - f[i] <= maxCost:
                    return True
            return False

        n = len(s)
        f = list(accumulate((abs(ord(a) - ord(b)) for a, b in zip(s, t)), initial=0))
        l, r = 0, n
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
    private int maxCost;
    private int[] f;
    private int n;

    public int equalSubstring(String s, String t, int maxCost) {
        n = s.length();
        f = new int[n + 1];
        this.maxCost = maxCost;
        for (int i = 0; i < n; ++i) {
            int x = Math.abs(s.charAt(i) - t.charAt(i));
            f[i + 1] = f[i] + x;
        }
        int l = 0, r = n;
        while (l < r) {
            int mid = (l + r + 1) >>> 1;
            if (check(mid)) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        return l;
    }

    private boolean check(int x) {
        for (int i = 0; i + x - 1 < n; ++i) {
            int j = i + x - 1;
            if (f[j + 1] - f[i] <= maxCost) {
                return true;
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int equalSubstring(string s, string t, int maxCost) {
        int n = s.size();
        int f[n + 1];
        f[0] = 0;
        for (int i = 0; i < n; ++i) {
            f[i + 1] = f[i] + abs(s[i] - t[i]);
        }
        auto check = [&](int x) -> bool {
            for (int i = 0; i + x - 1 < n; ++i) {
                int j = i + x - 1;
                if (f[j + 1] - f[i] <= maxCost) {
                    return true;
                }
            }
            return false;
        };
        int l = 0, r = n;
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
func equalSubstring(s string, t string, maxCost int) int {
	n := len(s)
	f := make([]int, n+1)
	for i, a := range s {
		f[i+1] = f[i] + abs(int(a)-int(t[i]))
	}
	check := func(x int) bool {
		for i := 0; i+x-1 < n; i++ {
			if f[i+x]-f[i] <= maxCost {
				return true
			}
		}
		return false
	}
	l, r := 0, n
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

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function equalSubstring(s: string, t: string, maxCost: number): number {
    const n = s.length;
    const f = Array(n + 1).fill(0);

    for (let i = 0; i < n; i++) {
        f[i + 1] = f[i] + Math.abs(s.charCodeAt(i) - t.charCodeAt(i));
    }

    const check = (x: number): boolean => {
        for (let i = 0; i + x - 1 < n; i++) {
            if (f[i + x] - f[i] <= maxCost) {
                return true;
            }
        }
        return false;
    };

    let l = 0,
        r = n;
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

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 dùng mảng tổng tiền tố tốn $O(n)$ bộ nhớ và thời gian $O(n\log n)$. Chi phí không âm, nên mở rộng đầu phải chỉ làm tăng chi phí còn dịch đầu trái chỉ làm giảm chi phí. Hai con trỏ duy trì cửa sổ dài nhất thỏa điều kiện trong một lượt duyệt, với thời gian tuyến tính và bộ nhớ phụ hằng số.

<!-- thinking:end -->

Duy trì hai con trỏ $l$ và $r$, ban đầu $l = r = 0$, cùng biến $\textit{cost}$ biểu thị tổng trị tuyệt đối của hiệu mã ASCII trong đoạn chỉ số $[l,..r]$. Mỗi bước, dịch $r$ sang phải một vị trí rồi cập nhật $\textit{cost} = \textit{cost} + |s[r] - t[r]|$. Nếu $\textit{cost} \gt \textit{maxCost}$, tiếp tục dịch $l$ sang phải và giảm $\textit{cost}$ cho đến khi $\textit{cost} \leq \textit{maxCost}$. Sau đó cập nhật đáp án bằng $\textit{ans} = \max(\textit{ans}, r - l + 1)$.

Cuối cùng, trả về đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def equalSubstring(self, s: str, t: str, maxCost: int) -> int:
        n = len(s)
        ans = cost = l = 0
        for r in range(n):
            cost += abs(ord(s[r]) - ord(t[r]))
            while cost > maxCost:
                cost -= abs(ord(s[l]) - ord(t[l]))
                l += 1
            ans = max(ans, r - l + 1)
        return ans
```

#### Java

```java
class Solution {
    public int equalSubstring(String s, String t, int maxCost) {
        int n = s.length();
        int ans = 0, cost = 0;
        for (int l = 0, r = 0; r < n; ++r) {
            cost += Math.abs(s.charAt(r) - t.charAt(r));
            while (cost > maxCost) {
                cost -= Math.abs(s.charAt(l) - t.charAt(l));
                ++l;
            }
            ans = Math.max(ans, r - l + 1);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int equalSubstring(string s, string t, int maxCost) {
        int n = s.length();
        int ans = 0, cost = 0;
        for (int l = 0, r = 0; r < n; ++r) {
            cost += abs(s[r] - t[r]);
            while (cost > maxCost) {
                cost -= abs(s[l] - t[l]);
                ++l;
            }
            ans = max(ans, r - l + 1);
        }
        return ans;
    }
};
```

#### Go

```go
func equalSubstring(s string, t string, maxCost int) (ans int) {
	var cost, l int
	for r := range s {
		cost += abs(int(s[r]) - int(t[r]))
		for ; cost > maxCost; l++ {
			cost -= abs(int(s[l]) - int(t[l]))
		}
		ans = max(ans, r-l+1)
	}
	return
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function equalSubstring(s: string, t: string, maxCost: number): number {
    const getCost = (i: number) => Math.abs(s[i].charCodeAt(0) - t[i].charCodeAt(0));
    const n = s.length;
    let ans = 0,
        cost = 0;
    for (let l = 0, r = 0; r < n; ++r) {
        cost += getCost(r);
        while (cost > maxCost) {
            cost -= getCost(l++);
        }
        ans = Math.max(ans, r - l + 1);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3: Một cách dùng hai con trỏ khác

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 2 có thể thu nhỏ rồi lại mở rộng cửa sổ, đồng thời lưu đáp án riêng. Vì chỉ cần độ dài lớn nhất, ta có thể chỉ dịch con trỏ trái về phía trước để duy trì cửa sổ đơn điệu. Cuối cùng, $n-l$ chính là độ dài lớn nhất, không cần biến $ans$ riêng.

<!-- thinking:end -->

Trong Lời giải 2, đoạn được duy trì bởi hai con trỏ có thể ngắn lại hoặc dài ra. Vì bài toán chỉ yêu cầu độ dài lớn nhất, ta có thể duy trì một đoạn có độ dài không giảm.

Cụ thể, dùng hai con trỏ $l$ và $r$ làm hai đầu đoạn, ban đầu $l = r = 0$. Mỗi bước, dịch $r$ sang phải một vị trí rồi cập nhật $\textit{cost} = \textit{cost} + |s[r] - t[r]|$. Nếu $\textit{cost} \gt \textit{maxCost}$, dịch $l$ sang phải một vị trí và giảm $\textit{cost}$.

Cuối cùng, trả về $n - l$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$, trong đó $n$ là độ dài chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def equalSubstring(self, s: str, t: str, maxCost: int) -> int:
        cost = l = 0
        for a, b in zip(s, t):
            cost += abs(ord(a) - ord(b))
            if cost > maxCost:
                cost -= abs(ord(s[l]) - ord(t[l]))
                l += 1
        return len(s) - l
```

#### Java

```java
class Solution {
    public int equalSubstring(String s, String t, int maxCost) {
        int n = s.length();
        int cost = 0, l = 0;
        for (int r = 0; r < n; ++r) {
            cost += Math.abs(s.charAt(r) - t.charAt(r));
            if (cost > maxCost) {
                cost -= Math.abs(s.charAt(l) - t.charAt(l));
                ++l;
            }
        }
        return n - l;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int equalSubstring(string s, string t, int maxCost) {
        int n = s.length();
        int cost = 0, l = 0;
        for (int r = 0; r < n; ++r) {
            cost += abs(s[r] - t[r]);
            if (cost > maxCost) {
                cost -= abs(s[l] - t[l]);
                ++l;
            }
        }
        return n - l;
    }
};
```

#### Go

```go
func equalSubstring(s string, t string, maxCost int) int {
	n := len(s)
	var cost, l int
	for r := range s {
		cost += abs(int(s[r]) - int(t[r]))
		if cost > maxCost {
			cost -= abs(int(s[l]) - int(t[l]))
			l++
		}
	}
	return n - l
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function equalSubstring(s: string, t: string, maxCost: number): number {
    const getCost = (i: number) => Math.abs(s[i].charCodeAt(0) - t[i].charCodeAt(0));
    const n = s.length;
    let cost = 0;
    let l = 0;
    for (let r = 0; r < n; ++r) {
        cost += getCost(r);
        if (cost > maxCost) {
            cost -= getCost(l++);
        }
    }
    return n - l;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
