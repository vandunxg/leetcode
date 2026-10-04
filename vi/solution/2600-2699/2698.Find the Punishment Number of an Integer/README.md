---
comments: true
difficulty: Medium
rating: 1678
source: Weekly Contest 346 Q3
tags:
    - Math
    - Backtracking
---

<!-- problem:start -->

# [2698. Find the Punishment Number of an Integer](https://leetcode.com/problems/find-the-punishment-number-of-an-integer)

[中文文档](/solution/2600-2699/2698.Find%20the%20Punishment%20Number%20of%20an%20Integer/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên dương <code>n</code>, hãy trả về <em><strong>số phạt</strong></em> của <code>n</code>.</p>

<p><strong>Số phạt</strong> của <code>n</code> được định nghĩa là tổng bình phương của tất cả các số nguyên <code>i</code> thỏa mãn:</p>

<ul>
	<li><code>1 &lt;= i &lt;= n</code></li>
	<li>Biểu diễn thập phân của <code>i * i</code> có thể được chia thành các chuỗi con liên tiếp sao cho tổng các giá trị nguyên của những chuỗi con này bằng <code>i</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 10
<strong>Đầu ra:</strong> 182
<strong>Giải thích:</strong> Có đúng 3 số nguyên i trong đoạn [1, 10] thỏa mãn các điều kiện trong đề bài:
- 1 vì 1 * 1 = 1
- 9 vì 9 * 9 = 81 và 81 có thể được chia thành 8 và 1, với tổng bằng 8 + 1 == 9.
- 10 vì 10 * 10 = 100 và 100 có thể được chia thành 10 và 0, với tổng bằng 10 + 0 == 10.
Do đó, số phạt của 10 là 1 + 81 + 100 = 182
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 37
<strong>Đầu ra:</strong> 1478
<strong>Giải thích:</strong> Có đúng 4 số nguyên i trong đoạn [1, 37] thỏa mãn các điều kiện trong đề bài:
- 1 vì 1 * 1 = 1.
- 9 vì 9 * 9 = 81 và 81 có thể được chia thành 8 + 1.
- 10 vì 10 * 10 = 100 và 100 có thể được chia thành 10 + 0.
- 36 vì 36 * 36 = 1296 và 1296 có thể được chia thành 1 + 29 + 6.
Do đó, số phạt của 37 là 1 + 81 + 100 + 1296 = 1478
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê + DFS

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần kiểm tra liệu các chữ số thập phân của $i^2$ có thể được chia thành các phần có tổng bằng $i$ hay không. Vì $n \le 1000$ nên số chính phương có nhiều nhất bảy chữ số; do đó chỉ cần liệt kê $i$ và dùng DFS để phân tách.
>
> Mở rộng phần hiện tại từ trái sang phải và dừng nếu nó vượt quá mục tiêu còn lại; một cách phân tách hoàn tất với phần dư $0$ là hợp lệ và ta cộng bình phương đó.

<!-- thinking:end -->

Ta liệt kê $i$, trong đó $1 \leq i \leq n$. Với mỗi $i$, ta chia chuỗi biểu diễn thập phân của $x = i^2$, rồi kiểm tra xem nó có thỏa mãn yêu cầu của đề bài hay không. Nếu có, ta cộng $x$ vào kết quả.

Sau khi kết thúc việc liệt kê, trả về kết quả.

Độ phức tạp thời gian là $O(n^{1 + 2 \log_{10}^2})$, độ phức tạp không gian là $O(\log n)$, trong đó $n$ là số nguyên dương đã cho.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def punishmentNumber(self, n: int) -> int:
        def check(s: str, i: int, x: int) -> bool:
            m = len(s)
            if i >= m:
                return x == 0
            y = 0
            for j in range(i, m):
                y = y * 10 + int(s[j])
                if y > x:
                    break
                if check(s, j + 1, x - y):
                    return True
            return False

        ans = 0
        for i in range(1, n + 1):
            x = i * i
            if check(str(x), 0, i):
                ans += x
        return ans
```

#### Java

```java
class Solution {
    public int punishmentNumber(int n) {
        int ans = 0;
        for (int i = 1; i <= n; ++i) {
            int x = i * i;
            if (check(x + "", 0, i)) {
                ans += x;
            }
        }
        return ans;
    }

    private boolean check(String s, int i, int x) {
        int m = s.length();
        if (i >= m) {
            return x == 0;
        }
        int y = 0;
        for (int j = i; j < m; ++j) {
            y = y * 10 + (s.charAt(j) - '0');
            if (y > x) {
                break;
            }
            if (check(s, j + 1, x - y)) {
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
    int punishmentNumber(int n) {
        int ans = 0;
        for (int i = 1; i <= n; ++i) {
            int x = i * i;
            string s = to_string(x);
            if (check(s, 0, i)) {
                ans += x;
            }
        }
        return ans;
    }

    bool check(const string& s, int i, int x) {
        int m = s.size();
        if (i >= m) {
            return x == 0;
        }
        int y = 0;
        for (int j = i; j < m; ++j) {
            y = y * 10 + s[j] - '0';
            if (y > x) {
                break;
            }
            if (check(s, j + 1, x - y)) {
                return true;
            }
        }
        return false;
    }
};
```

#### Go

```go
func punishmentNumber(n int) (ans int) {
	var check func(string, int, int) bool
	check = func(s string, i, x int) bool {
		m := len(s)
		if i >= m {
			return x == 0
		}
		y := 0
		for j := i; j < m; j++ {
			y = y*10 + int(s[j]-'0')
			if y > x {
				break
			}
			if check(s, j+1, x-y) {
				return true
			}
		}
		return false
	}
	for i := 1; i <= n; i++ {
		x := i * i
		s := strconv.Itoa(x)
		if check(s, 0, i) {
			ans += x
		}
	}
	return
}
```

#### TypeScript

```ts
function punishmentNumber(n: number): number {
    const check = (s: string, i: number, x: number): boolean => {
        const m = s.length;
        if (i >= m) {
            return x === 0;
        }
        let y = 0;
        for (let j = i; j < m; ++j) {
            y = y * 10 + Number(s[j]);
            if (y > x) {
                break;
            }
            if (check(s, j + 1, x - y)) {
                return true;
            }
        }
        return false;
    };
    let ans = 0;
    for (let i = 1; i <= n; ++i) {
        const x = i * i;
        const s = x.toString();
        if (check(s, 0, i)) {
            ans += x;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
