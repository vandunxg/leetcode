---
comments: true
difficulty: Easy
rating: 1257
source: Weekly Contest 281 Q1
tags:
    - Math
    - Simulation
---

<!-- problem:start -->

# [2180. Count Integers With Even Digit Sum](https://leetcode.com/problems/count-integers-with-even-digit-sum)

[中文文档](/solution/2100-2199/2180.Count%20Integers%20With%20Even%20Digit%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên dương <code>num</code>, hãy trả về <em>số lượng số nguyên dương <strong>nhỏ hơn hoặc bằng</strong></em> <code>num</code> <em>có tổng các chữ số là <strong>số chẵn</strong></em>.</p>

<p><strong>Tổng các chữ số</strong> của một số nguyên dương là tổng của tất cả các chữ số của số đó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 4
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Các số nguyên duy nhất nhỏ hơn hoặc bằng 4 có tổng các chữ số là số chẵn là 2 và 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 30
<strong>Đầu ra:</strong> 14
<strong>Giải thích:</strong>
14 số nguyên nhỏ hơn hoặc bằng 30 có tổng các chữ số là số chẵn là
2, 4, 6, 8, 11, 13, 15, 17, 19, 20, 22, 24, 26 và 28.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= num &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các số nguyên trong $[1,\textit{num}]$ có tổng các chữ số là số chẵn. Vì $\textit{num}\le 1000$, ta có thể tính tổng chữ số của từng giá trị.
>
> Liên tục cộng $x\bmod 10$ và tăng biến đếm khi tổng là số chẵn.
>
> Chi phí bổ sung là số chữ số.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countEven(self, num: int) -> int:
        ans = 0
        for x in range(1, num + 1):
            s = 0
            while x:
                s += x % 10
                x //= 10
            ans += s % 2 == 0
        return ans
```

#### Java

```java
class Solution {
    public int countEven(int num) {
        int ans = 0;
        for (int i = 1; i <= num; ++i) {
            int s = 0;
            for (int x = i; x > 0; x /= 10) {
                s += x % 10;
            }
            if (s % 2 == 0) {
                ++ans;
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
    int countEven(int num) {
        int ans = 0;
        for (int i = 1; i <= num; ++i) {
            int s = 0;
            for (int x = i; x; x /= 10) {
                s += x % 10;
            }
            ans += s % 2 == 0;
        }
        return ans;
    }
};
```

#### Go

```go
func countEven(num int) (ans int) {
	for i := 1; i <= num; i++ {
		s := 0
		for x := i; x > 0; x /= 10 {
			s += x % 10
		}
		if s%2 == 0 {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function countEven(num: number): number {
    let ans = 0;
    for (let i = 1; i <= num; ++i) {
        let s = 0;
        for (let x = i; x; x = Math.floor(x / 10)) {
            s += x % 10;
        }
        if (s % 2 == 0) {
            ++ans;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 có độ phức tạp tuyến tính theo $\textit{num}$. Trong mỗi mười số nguyên liên tiếp, chính xác năm số có tổng các chữ số là số chẵn, nên có thể tính gộp các nhóm đủ mười số trong $O(1)$.
>
> Các nhóm đủ mười số đóng góp $\lfloor\textit{num}/10\rfloor\times 5$, sau đó trừ một để loại bỏ $0$. Số lượng các số còn lại phụ thuộc vào tính chẵn lẻ của tổng các chữ số ở phần cao $s$, làm thay đổi công thức tính trực tiếp.
>
> Chỉ cần duyệt các chữ số của $\textit{num}/10$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countEven(self, num: int) -> int:
        ans = num // 10 * 5 - 1
        x, s = num // 10, 0
        while x:
            s += x % 10
            x //= 10
        ans += (num % 10 + 2 - (s & 1)) >> 1
        return ans
```

#### Java

```java
class Solution {
    public int countEven(int num) {
        int ans = num / 10 * 5 - 1;
        int s = 0;
        for (int x = num / 10; x > 0; x /= 10) {
            s += x % 10;
        }
        ans += (num % 10 + 2 - (s & 1)) >> 1;
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countEven(int num) {
        int ans = num / 10 * 5 - 1;
        int s = 0;
        for (int x = num / 10; x > 0; x /= 10) {
            s += x % 10;
        }
        ans += (num % 10 + 2 - (s & 1)) >> 1;
        return ans;
    }
};
```

#### Go

```go
func countEven(num int) (ans int) {
	ans = num/10*5 - 1
	s := 0
	for x := num / 10; x > 0; x /= 10 {
		s += x % 10
	}
	ans += (num%10 + 2 - (s & 1)) >> 1
	return
}
```

#### TypeScript

```ts
function countEven(num: number): number {
    let ans = Math.floor(num / 10) * 5 - 1;
    let s = 0;
    for (let x = Math.floor(num / 10); x; x = Math.floor(x / 10)) {
        s += x % 10;
    }
    ans += ((num % 10) + 2 - (s & 1)) >> 1;
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
