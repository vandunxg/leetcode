---
comments: true
difficulty: Medium
rating: 1382
source: Weekly Contest 171 Q2
tags:
    - Bit Manipulation
---

<!-- problem:start -->

# [1318. Minimum Flips to Make a OR b Equal to c](https://leetcode.com/problems/minimum-flips-to-make-a-or-b-equal-to-c)

[中文文档](/solution/1300-1399/1318.Minimum%20Flips%20to%20Make%20a%20OR%20b%20Equal%20to%20c/README.md)

## Mô tả

<!-- description:start -->

<p>Cho 3 số nguyên dương <code>a</code>, <code>b</code> và <code>c</code>. Hãy trả về số lần lật bit ít nhất cần thực hiện trên các bit của <code>a</code> và <code>b</code> để có (&nbsp;<code>a</code> OR <code>b</code> == <code>c</code>&nbsp;) (phép OR theo bit).<br />
Mỗi lần lật bit là thay đổi <strong>bất kỳ</strong> một bit từ 1 thành 0 hoặc từ 0 thành 1 trong biểu diễn nhị phân của số.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1318.Minimum%20Flips%20to%20Make%20a%20OR%20b%20Equal%20to%20c/images/sample_3_1676.png" style="width: 260px; height: 87px;" /></p>

<pre>
<strong>Đầu vào:</strong> a = 2, b = 6, c = 5
<strong>Đầu ra:</strong> 3
<strong>Giải thích: </strong>Sau khi lật bit, a = 1 , b = 4 , c = 5, khi đó (<code>a</code> OR <code>b</code> == <code>c</code>)</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> a = 4, b = 2, c = 7
<strong>Đầu ra:</strong> 1
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> a = 1, b = 2, c = 3
<strong>Đầu ra:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= a &lt;= 10^9</code></li>
	<li><code>1 &lt;= b&nbsp;&lt;= 10^9</code></li>
	<li><code>1 &lt;= c&nbsp;&lt;= 10^9</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần lật ít bit nhất để $a\lor b=c$. Các bit độc lập với nhau, nên không cần tìm kiếm trên toàn bộ giá trị số nguyên. Nếu bit tương ứng của $c$ là $0$, mọi bit $1$ ở $a$ hoặc $b$ đều phải lật; nếu bit đó là $1$, chỉ cần lật một bit khi cả hai bit tương ứng đều bằng $0$. Chỉ cần xét tổng cộng $32$ bit.

<!-- thinking:end -->

Ta có thể duyệt từng bit trong biểu diễn nhị phân của $a$, $b$ và $c$, lần lượt ký hiệu là $x$, $y$ và $z$. Nếu kết quả phép OR theo bit của $x$ và $y$ khác $z$, ta kiểm tra xem cả $x$ và $y$ có đều bằng $1$ hay không. Nếu đúng, cần lật hai lần; nếu không, chỉ cần lật một lần. Ta cộng dồn số lần lật cần thiết.

Độ phức tạp thời gian là $O(\log M)$, trong đó $M$ là giá trị lớn nhất trong các số của đề bài. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minFlips(self, a: int, b: int, c: int) -> int:
        ans = 0
        for i in range(32):
            x, y, z = a >> i & 1, b >> i & 1, c >> i & 1
            ans += x + y if z == 0 else int(x == 0 and y == 0)
        return ans
```

#### Java

```java
class Solution {
    public int minFlips(int a, int b, int c) {
        int ans = 0;
        for (int i = 0; i < 32; ++i) {
            int x = a >> i & 1, y = b >> i & 1, z = c >> i & 1;
            ans += z == 0 ? x + y : (x == 0 && y == 0 ? 1 : 0);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minFlips(int a, int b, int c) {
        int ans = 0;
        for (int i = 0; i < 32; ++i) {
            int x = a >> i & 1, y = b >> i & 1, z = c >> i & 1;
            ans += z == 0 ? x + y : (x == 0 && y == 0 ? 1 : 0);
        }
        return ans;
    }
};
```

#### Go

```go
func minFlips(a int, b int, c int) (ans int) {
	for i := 0; i < 32; i++ {
		x, y, z := a>>i&1, b>>i&1, c>>i&1
		if z == 0 {
			ans += x + y
		} else if x == 0 && y == 0 {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function minFlips(a: number, b: number, c: number): number {
    let ans = 0;
    for (let i = 0; i < 32; ++i) {
        const [x, y, z] = [(a >> i) & 1, (b >> i) & 1, (c >> i) & 1];
        ans += z === 0 ? x + y : x + y === 0 ? 1 : 0;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
