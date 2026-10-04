---
comments: true
difficulty: Hard
tags:
    - Math
    - Combinatorics
---

<!-- problem:start -->

# [2927. Distribute Candies Among Children III 🔒](https://leetcode.com/problems/distribute-candies-among-children-iii)

[中文文档](/solution/2900-2999/2927.Distribute%20Candies%20Among%20Children%20III/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên dương <code>n</code> và <code>limit</code>.</p>

<p>Hãy trả về <em><strong>tổng số cách</strong> phân phối </em><code>n</code> <em>viên kẹo cho </em><code>3</code><em> đứa trẻ sao cho không đứa trẻ nào nhận nhiều hơn </em><code>limit</code><em> viên kẹo.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 5, limit = 2
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Có 3 cách phân phối 5 viên kẹo sao cho không đứa trẻ nào nhận nhiều hơn 2 viên: (1, 2, 2), (2, 1, 2) và (2, 2, 1).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3, limit = 3
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong> Có 10 cách phân phối 3 viên kẹo sao cho không đứa trẻ nào nhận nhiều hơn 3 viên: (0, 0, 3), (0, 1, 2), (0, 2, 1), (0, 3, 0), (1, 0, 2), (1, 1, 1), (1, 2, 0), (2, 0, 1), (2, 1, 0) và (3, 0, 0).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>8</sup></code></li>
	<li><code>1 &lt;= limit &lt;= 10<sup>8</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học tổ hợp + Nguyên lý bao hàm - loại trừ

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm các số không âm $x+y+z=n$, trong đó mỗi biến không vượt quá $limit$. Vì $n$ có thể rất lớn nên không thể dùng ba vòng lặp. Phương pháp stars and bars cho $C_{n+2}^{2}$ nếu không có giới hạn; nguyên lý bao hàm - loại trừ sẽ loại các trường hợp có một biến ít nhất là $limit+1$, rồi cộng lại các trường hợp đồng thời vi phạm ở hai biến.
>
> Nếu $n>3\cdot limit$ thì kết quả là $0$. Các đối số của nhị thức được kiểm tra để luôn không âm. Công thức có độ phức tạp $O(1)$.

<!-- thinking:end -->

Theo mô tả bài toán, ta cần phân phối $n$ viên kẹo cho $3$ đứa trẻ, trong đó mỗi đứa trẻ nhận từ $[0, limit]$ viên kẹo.

Điều này tương đương với việc đặt $n$ quả bóng vào $3$ chiếc hộp. Vì các hộp có thể rỗng, ta thêm $3$ quả bóng ảo, khi đó có tổng cộng $n + 3$ quả bóng, rồi đặt $2$ vách ngăn vào $n + 3 - 1$ vị trí, qua đó chia $n$ quả bóng thực thành $3$ nhóm và cho phép các hộp rỗng. Vì vậy, số cách ban đầu là $C_{n + 2}^2$.

Ta cần loại các cách mà số bóng trong một hộp vượt quá $limit$. Giả sử có một hộp chứa nhiều hơn $limit$ quả bóng, khi đó số bóng còn lại (bao gồm cả bóng ảo) nhiều nhất là $n + 3 - (limit + 1) = n - limit + 2$, và số vị trí là $n - limit + 1$, nên số cách là $C_{n - limit + 1}^2$. Vì có $3$ hộp, số cách như vậy là $3 \times C_{n - limit + 1}^2$. Khi đó, ta đã loại cả những cách mà số bóng trong hai hộp đồng thời vượt quá $limit$, nên cần cộng lại số cách đó, tức là $3 \times C_{n - 2 \times limit}^2$.

Độ phức tạp thời gian là $O(1)$, và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def distributeCandies(self, n: int, limit: int) -> int:
        if n > 3 * limit:
            return 0
        ans = comb(n + 2, 2)
        if n > limit:
            ans -= 3 * comb(n - limit + 1, 2)
        if n - 2 >= 2 * limit:
            ans += 3 * comb(n - 2 * limit, 2)
        return ans
```

#### Java

```java
class Solution {
    public long distributeCandies(int n, int limit) {
        if (n > 3 * limit) {
            return 0;
        }
        long ans = comb2(n + 2);
        if (n > limit) {
            ans -= 3 * comb2(n - limit + 1);
        }
        if (n - 2 >= 2 * limit) {
            ans += 3 * comb2(n - 2 * limit);
        }
        return ans;
    }

    private long comb2(int n) {
        return 1L * n * (n - 1) / 2;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long distributeCandies(int n, int limit) {
        auto comb2 = [](int n) {
            return 1LL * n * (n - 1) / 2;
        };
        if (n > 3 * limit) {
            return 0;
        }
        long long ans = comb2(n + 2);
        if (n > limit) {
            ans -= 3 * comb2(n - limit + 1);
        }
        if (n - 2 >= 2 * limit) {
            ans += 3 * comb2(n - 2 * limit);
        }
        return ans;
    }
};
```

#### Go

```go
func distributeCandies(n int, limit int) int64 {
	comb2 := func(n int) int {
		return n * (n - 1) / 2
	}
	if n > 3*limit {
		return 0
	}
	ans := comb2(n + 2)
	if n > limit {
		ans -= 3 * comb2(n-limit+1)
	}
	if n-2 >= 2*limit {
		ans += 3 * comb2(n-2*limit)
	}
	return int64(ans)
}
```

#### TypeScript

```ts
function distributeCandies(n: number, limit: number): number {
    const comb2 = (n: number) => (n * (n - 1)) / 2;
    if (n > 3 * limit) {
        return 0;
    }
    let ans = comb2(n + 2);
    if (n > limit) {
        ans -= 3 * comb2(n - limit + 1);
    }
    if (n - 2 >= 2 * limit) {
        ans += 3 * comb2(n - 2 * limit);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
