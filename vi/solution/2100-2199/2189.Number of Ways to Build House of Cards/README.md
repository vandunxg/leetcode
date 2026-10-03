---
comments: true
difficulty: Medium
tags:
    - Math
    - Dynamic Programming
---

<!-- problem:start -->

# [2189. Number of Ways to Build House of Cards 🔒](https://leetcode.com/problems/number-of-ways-to-build-house-of-cards)

[Tài liệu tiếng Trung](/solution/2100-2199/2189.Number%20of%20Ways%20to%20Build%20House%20of%20Cards/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code> biểu diễn số quân bài bạn có. Một <strong>ngôi nhà bằng quân bài</strong> thỏa mãn các điều kiện sau:</p>

<ul>
	<li>Một <strong>ngôi nhà bằng quân bài</strong> gồm một hoặc nhiều hàng <strong>tam giác</strong> và các quân bài nằm ngang.</li>
	<li><strong>Tam giác</strong> được tạo thành bằng cách dựa hai quân bài vào nhau.</li>
	<li>Giữa <strong>mọi</strong> cặp tam giác liền kề trong một hàng phải có một quân bài được đặt nằm ngang.</li>
	<li>Mọi tam giác ở hàng cao hơn hàng đầu tiên phải được đặt trên một quân bài nằm ngang của hàng ngay bên dưới.</li>
	<li>Mỗi tam giác được đặt vào vị trí trống <strong>ngoài cùng bên trái</strong> trong hàng.</li>
</ul>

<p>Trả về <em>số lượng <strong>ngôi nhà bằng quân bài</strong> <strong>khác nhau</strong> mà bạn có thể xây dựng bằng cách sử dụng <strong>toàn bộ</strong></em> <code>n</code><em> quân bài.</em> Hai ngôi nhà bằng quân bài được xem là khác nhau nếu tồn tại một hàng mà ở đó hai ngôi nhà có số quân bài khác nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2189.Number%20of%20Ways%20to%20Build%20House%20of%20Cards/images/image-20220227213243-1.png" style="width: 726px; height: 150px;" />
<pre>
<strong>Đầu vào:</strong> n = 16
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Hai ngôi nhà bằng quân bài hợp lệ được hiển thị trong hình.
Ngôi nhà bằng quân bài thứ ba trong hình không hợp lệ vì tam giác ngoài cùng bên phải ở hàng trên cùng không được đặt trên một quân bài nằm ngang.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2189.Number%20of%20Ways%20to%20Build%20House%20of%20Cards/images/image-20220227213306-2.png" style="width: 96px; height: 80px;" />
<pre>
<strong>Đầu vào:</strong> n = 2
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Một ngôi nhà bằng quân bài hợp lệ được hiển thị trong hình.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2189.Number%20of%20Ways%20to%20Build%20House%20of%20Cards/images/image-20220227213331-3.png" style="width: 330px; height: 85px;" />
<pre>
<strong>Đầu vào:</strong> n = 4
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Ba ngôi nhà bằng quân bài trong hình đều không hợp lệ.
Ngôi nhà đầu tiên cần có một quân bài nằm ngang được đặt giữa hai tam giác.
Ngôi nhà thứ hai sử dụng 5 quân bài.
Ngôi nhà thứ ba sử dụng 2 quân bài.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 500</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Một tầng sử dụng $3k+2$ quân bài và các tầng có kích thước khác nhau. Ta cần đếm số cách biểu diễn $n$ thành tổng của các số dạng này. Các tầng rộng hơn phải nằm bên dưới, nên thứ tự đã được cố định và ta chỉ cần chọn một tập hợp con có tổng bằng $n$.
>
> Với $k$ tăng dần, ta chọn hoặc bỏ qua từng tầng, dừng sớm khi $3k+2>n$, và đếm $1$ khi tổng vừa đúng bằng $n$. Ghi nhớ kết quả biến bài toán thành một bài toán knapsack 2 chiều.
>
> $\textit{dfs}(n,0)$ là điểm bắt đầu.

<!-- thinking:end -->

Ta nhận thấy số quân bài ở mỗi tầng là $3 \times k + 2$, và số quân bài ở mỗi tầng là khác nhau. Vì vậy, bài toán có thể được chuyển thành: có bao nhiêu cách biểu diễn số nguyên $n$ thành tổng của các số có dạng $3 \times k + 2$. Đây là một bài toán knapsack kinh điển, có thể giải bằng tìm kiếm có ghi nhớ.

Ta xây dựng hàm $\text{dfs}(n, k)$, biểu diễn số cách xây dựng các ngôi nhà bằng quân bài khác nhau khi còn lại $n$ quân bài và tầng hiện tại là $k$. Đáp án là $\text{dfs}(n, 0)$.

Logic thực thi của hàm $\text{dfs}(n, k)$ như sau:

- Nếu $3 \times k + 2 \gt n$, tầng hiện tại không thể đặt quân bài nào, trả về $0$;
- Nếu $3 \times k + 2 = n$, tầng hiện tại có thể đặt quân bài và sau khi đặt xong thì toàn bộ ngôi nhà bằng quân bài được hoàn thành, trả về $1$;
- Nếu không, ta có thể chọn không đặt quân bài hoặc đặt quân bài. Nếu chọn không đặt quân bài, số quân bài còn lại không thay đổi và số tầng tăng lên $1$, tức là $\text{dfs}(n, k + 1)$. Nếu chọn đặt quân bài, số quân bài còn lại giảm đi $3 \times k + 2$ và số tầng tăng lên $1$, tức là $\text{dfs}(n - (3 \times k + 2), k + 1)$. Tổng của hai trường hợp này là đáp án.

Trong quá trình này, ta có thể sử dụng ghi nhớ để tránh tính toán lặp lại.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n^2)$. Trong đó, $n$ là số quân bài.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def houseOfCards(self, n: int) -> int:
        @cache
        def dfs(n: int, k: int) -> int:
            x = 3 * k + 2
            if x > n:
                return 0
            if x == n:
                return 1
            return dfs(n - x, k + 1) + dfs(n, k + 1)

        return dfs(n, 0)
```

#### Java

```java
class Solution {
    private Integer[][] f;

    public int houseOfCards(int n) {
        f = new Integer[n + 1][n / 3];
        return dfs(n, 0);
    }

    private int dfs(int n, int k) {
        int x = 3 * k + 2;
        if (x > n) {
            return 0;
        }
        if (x == n) {
            return 1;
        }
        if (f[n][k] != null) {
            return f[n][k];
        }
        return f[n][k] = dfs(n - x, k + 1) + dfs(n, k + 1);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int houseOfCards(int n) {
        int f[n + 1][n / 3 + 1];
        memset(f, -1, sizeof(f));
        auto dfs = [&](this auto&& dfs, int n, int k) -> int {
            int x = 3 * k + 2;
            if (x > n) {
                return 0;
            }
            if (x == n) {
                return 1;
            }
            if (f[n][k] != -1) {
                return f[n][k];
            }
            return f[n][k] = dfs(n - x, k + 1) + dfs(n, k + 1);
        };
        return dfs(n, 0);
    }
};
```

#### Go

```go
func houseOfCards(n int) int {
	f := make([][]int, n+1)
	for i := range f {
		f[i] = make([]int, n/3+1)
		for j := range f[i] {
			f[i][j] = -1
		}
	}
	var dfs func(n, k int) int
	dfs = func(n, k int) int {
		x := 3*k + 2
		if x > n {
			return 0
		}
		if x == n {
			return 1
		}
		if f[n][k] == -1 {
			f[n][k] = dfs(n-x, k+1) + dfs(n, k+1)
		}
		return f[n][k]
	}
	return dfs(n, 0)
}
```

#### TypeScript

```ts
function houseOfCards(n: number): number {
    const f: number[][] = Array(n + 1)
        .fill(0)
        .map(() => Array(Math.floor(n / 3) + 1).fill(-1));
    const dfs = (n: number, k: number): number => {
        const x = k * 3 + 2;
        if (x > n) {
            return 0;
        }
        if (x === n) {
            return 1;
        }
        if (f[n][k] === -1) {
            f[n][k] = dfs(n - x, k + 1) + dfs(n, k + 1);
        }
        return f[n][k];
    };
    return dfs(n, 0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
