---
comments: true
difficulty: Easy
---

<!-- problem:start -->

# [16.05. Factorial Zeros](https://leetcode.cn/problems/factorial-zeros-lcci)

[中文文档](/lcci/16.05.Factorial%20Zeros/README.md)

## Mô tả

<!-- description:start -->

<p>Hãy viết một thuật toán tính số lượng số 0 ở cuối của n giai thừa.</p>
<p><strong>Ví dụ 1:</strong></p>
<pre>

<strong>Đầu vào:</strong> 3

<strong>Đầu ra:</strong> 0

<strong>Giải thích:</strong>&nbsp;3! = 6, không có số 0 ở cuối.</pre>

<p><strong>Ví dụ&nbsp;2:</strong></p>
<pre>

<strong>Đầu vào:</strong> 5

<strong>Đầu ra:</strong> 1

<strong>Giải thích:</strong>&nbsp;5! = 120, có một số 0 ở cuối.</pre>

<p><b>Lưu ý:&nbsp;</b>Lời giải của bạn phải có độ phức tạp thời gian logarit.</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Các số 0 ở cuối của $n!$ xuất hiện từ các thừa số $10=2\times 5$. Các thừa số 2 có rất nhiều, nên số lượng số 0 bằng số lượng số 5 trong $[1,n]$, bao gồm cả các lũy thừa cao hơn.
>
> Nếu tính $n!$ rồi đếm số 0 thì sẽ xảy ra tràn số khi $n$ lớn.
>
> Lặp lại phép chia $n//=5$ sẽ cộng các đóng góp của $5$, $5^2$, $5^3,\ldots$ trong thời gian $O(\log n)$.

<!-- thinking:end -->

Thực ra, bài toán yêu cầu tính số lượng thừa số của $5$ trong $[1,n]$.

Hãy lấy $130$ làm ví dụ:

1. Chia $130$ cho $5$ lần thứ nhất, được $26$, nghĩa là có $26$ số chứa một thừa số của $5$.
2. Chia $26$ cho $5$ lần thứ hai, được $5$, nghĩa là có $5$ số chứa một thừa số của $5^2$.
3. Chia $5$ cho $5$ lần thứ ba, được $1$, nghĩa là có $1$ số chứa một thừa số của $5^3$.
4. Cộng tất cả các số đếm được để có tổng số thừa số của $5$ trong $[1,n]$.

Độ phức tạp thời gian là $O(\log n)$, còn độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def trailingZeroes(self, n: int) -> int:
        ans = 0
        while n:
            n //= 5
            ans += n
        return ans
```

#### Java

```java
class Solution {
    public int trailingZeroes(int n) {
        int ans = 0;
        while (n > 0) {
            n /= 5;
            ans += n;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int trailingZeroes(int n) {
        int ans = 0;
        while (n) {
            n /= 5;
            ans += n;
        }
        return ans;
    }
};
```

#### Go

```go
func trailingZeroes(n int) int {
	ans := 0
	for n > 0 {
		n /= 5
		ans += n
	}
	return ans
}
```

#### TypeScript

```ts
function trailingZeroes(n: number): number {
    let ans = 0;
    while (n) {
        n = Math.floor(n / 5);
        ans += n;
    }
    return ans;
}
```

#### Swift

```swift
class Solution {
    func trailingZeroes(_ n: Int) -> Int {
        var count = 0
        var number = n
        while number > 0 {
            number /= 5
            count += number
        }
        return count
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
