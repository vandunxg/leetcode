---
comments: true
difficulty: Hard
rating: 2076
source: Biweekly Contest 2 Q4
tags:
    - Math
    - Backtracking
---

<!-- problem:start -->

# [1088. Confusing Number II 🔒](https://leetcode.com/problems/confusing-number-ii)

[中文文档](/solution/1000-1099/1088.Confusing%20Number%20II/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Số gây nhầm lẫn</strong> là số mà khi xoay <code>180</code> độ sẽ thành một số khác, trong đó <strong>mọi chữ số đều hợp lệ</strong>.</p>

<p>Ta có thể xoay từng chữ số của một số <code>180</code> độ để tạo thành chữ số mới.</p>

<ul>
	<li>Khi xoay <code>0</code>, <code>1</code>, <code>6</code>, <code>8</code> và <code>9</code> <code>180</code> độ, chúng lần lượt trở thành <code>0</code>, <code>1</code>, <code>9</code>, <code>8</code> và <code>6</code>.</li>
	<li>Khi xoay <code>2</code>, <code>3</code>, <code>4</code>, <code>5</code> và <code>7</code> <code>180</code> độ, chúng trở nên <strong>không hợp lệ</strong>.</li>
</ul>

<p>Lưu ý rằng sau khi xoay số, ta có thể bỏ qua các số 0 ở đầu.</p>

<ul>
	<li>Ví dụ, xoay <code>8000</code> ta được <code>0008</code>, được xem như số <code>8</code>.</li>
</ul>

<p>Cho số nguyên <code>n</code>, hãy trả về <em>số lượng <strong>số gây nhầm lẫn</strong> trong đoạn bao gồm hai đầu mút </em><code>[1, n]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 20
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Các số gây nhầm lẫn là [6,9,10,16,18,19].
6 biến thành 9.
9 biến thành 6.
10 biến thành 01, tức là 1.
16 biến thành 91.
18 biến thành 81.
19 biến thành 61.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 100
<strong>Đầu ra:</strong> 19
<strong>Giải thích:</strong> Các số gây nhầm lẫn là [6,9,10,16,18,19,60,61,66,68,80,81,86,89,90,91,98,99,100].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đếm số gây nhầm lẫn trong $[1,n]$ với $n\le 10^9$. Khi xoay, chỉ các chữ số $0,1,6,8,9$ còn hợp lệ, vì vậy ta tạo số hợp lệ từng chữ số một rồi kiểm tra kết quả sau khi xoay.
>
> Dùng DFS từ chữ số cao nhất để liệt kê các số hợp lệ không vượt quá $n$. Khi tạo xong một số, ta dùng lại hàm kiểm tra phép xoay của bài $1056$.
>
> Bảng $d$ được dùng cả khi tạo số lẫn trong hàm `check`; các số 0 ở đầu chỉ được xem là số nguyên $0$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def confusingNumberII(self, n: int) -> int:
        def check(x: int) -> bool:
            y, t = 0, x
            while t:
                t, v = divmod(t, 10)
                y = y * 10 + d[v]
            return x != y

        def dfs(pos: int, limit: bool, x: int) -> int:
            if pos >= len(s):
                return int(check(x))
            up = int(s[pos]) if limit else 9
            ans = 0
            for i in range(up + 1):
                if d[i] != -1:
                    ans += dfs(pos + 1, limit and i == up, x * 10 + i)
            return ans

        d = [0, 1, -1, -1, -1, -1, 9, -1, 8, 6]
        s = str(n)
        return dfs(0, True, 0)
```

#### Java

```java
class Solution {
    private final int[] d = {0, 1, -1, -1, -1, -1, 9, -1, 8, 6};
    private String s;

    public int confusingNumberII(int n) {
        s = String.valueOf(n);
        return dfs(0, 1, 0);
    }

    private int dfs(int pos, int limit, int x) {
        if (pos >= s.length()) {
            return check(x) ? 1 : 0;
        }
        int up = limit == 1 ? s.charAt(pos) - '0' : 9;
        int ans = 0;
        for (int i = 0; i <= up; ++i) {
            if (d[i] != -1) {
                ans += dfs(pos + 1, limit == 1 && i == up ? 1 : 0, x * 10 + i);
            }
        }
        return ans;
    }

    private boolean check(int x) {
        int y = 0;
        for (int t = x; t > 0; t /= 10) {
            int v = t % 10;
            y = y * 10 + d[v];
        }
        return x != y;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int confusingNumberII(int n) {
        string s = to_string(n);
        int d[10] = {0, 1, -1, -1, -1, -1, 9, -1, 8, 6};
        auto check = [&](int x) -> bool {
            int y = 0;
            for (int t = x; t; t /= 10) {
                int v = t % 10;
                y = y * 10 + d[v];
            }
            return x != y;
        };
        function<int(int, int, int)> dfs = [&](int pos, int limit, int x) -> int {
            if (pos >= s.size()) {
                return check(x);
            }
            int up = limit ? s[pos] - '0' : 9;
            int ans = 0;
            for (int i = 0; i <= up; ++i) {
                if (d[i] != -1) {
                    ans += dfs(pos + 1, limit && i == up, x * 10 + i);
                }
            }
            return ans;
        };
        return dfs(0, 1, 0);
    }
};
```

#### Go

```go
func confusingNumberII(n int) int {
	d := [10]int{0, 1, -1, -1, -1, -1, 9, -1, 8, 6}
	s := strconv.Itoa(n)
	check := func(x int) bool {
		y := 0
		for t := x; t > 0; t /= 10 {
			v := t % 10
			y = y*10 + d[v]
		}
		return x != y
	}
	var dfs func(pos int, limit bool, x int) int
	dfs = func(pos int, limit bool, x int) (ans int) {
		if pos >= len(s) {
			if check(x) {
				return 1
			}
			return 0
		}
		up := 9
		if limit {
			up = int(s[pos] - '0')
		}
		for i := 0; i <= up; i++ {
			if d[i] != -1 {
				ans += dfs(pos+1, limit && i == up, x*10+i)
			}
		}
		return
	}
	return dfs(0, true, 0)
}
```

#### TypeScript

```ts
function confusingNumberII(n: number): number {
    const s = n.toString();
    const d: number[] = [0, 1, -1, -1, -1, -1, 9, -1, 8, 6];
    const check = (x: number) => {
        let y = 0;
        for (let t = x; t > 0; t = Math.floor(t / 10)) {
            const v = t % 10;
            y = y * 10 + d[v];
        }
        return x !== y;
    };
    const dfs = (pos: number, limit: boolean, x: number): number => {
        if (pos >= s.length) {
            return check(x) ? 1 : 0;
        }
        const up = limit ? parseInt(s[pos]) : 9;
        let ans = 0;
        for (let i = 0; i <= up; ++i) {
            if (d[i] !== -1) {
                ans += dfs(pos + 1, limit && i === up, x * 10 + i);
            }
        }
        return ans;
    };
    return dfs(0, true, 0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
