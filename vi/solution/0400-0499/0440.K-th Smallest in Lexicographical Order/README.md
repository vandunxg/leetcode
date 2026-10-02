---
comments: true
difficulty: Hard
tags:
    - Trie
---

<!-- problem:start -->

# [440. K-th Smallest in Lexicographical Order](https://leetcode.com/problems/k-th-smallest-in-lexicographical-order)

[中文文档](/solution/0400-0499/0440.K-th%20Smallest%20in%20Lexicographical%20Order/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên <code>n</code> và <code>k</code>, hãy trả về <em>số nguyên nhỏ thứ </em><code>k<sup>th</sup></code> <em>theo thứ tự từ điển trong khoảng</em> <code>[1, n]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 13, k = 2
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong> Thứ tự từ điển là [1, 10, 11, 12, 13, 2, 3, 4, 5, 6, 7, 8, 9], nên số nhỏ thứ hai là 10.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1, k = 1
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= n &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm trên Trie + Xây dựng tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Vì $n$ có thể lên tới $10^9$, ta không thể liệt kê rồi sắp xếp các số. Thứ tự từ điển tương ứng với một trie 10 nhánh: các node con của $x$ là $10x,\ldots,10x+9$ nếu chúng không vượt quá $n$.
>
> Đếm số node trong cây con của prefix $curr$ bằng cách giao các khoảng $[curr,curr+1)$ với $[1,n]$ ở từng độ dài chữ số. Nếu số lượng đó $\le k$, bỏ qua toàn bộ cây con và tăng $curr$; nếu không, đi xuống $curr\times 10$ và giảm $k$ đi một vì đã tính prefix hiện tại.
>
> Giảm $k$ đi một ngay từ đầu vì ta đang đứng tại $1$. Cách đếm theo từng tầng không cần duyệt qua tất cả số nguyên trong cây con.

<!-- thinking:end -->

Bài toán yêu cầu tìm số nhỏ thứ \$k\$ trong khoảng $[1, n]$ khi sắp xếp các số theo **thứ tự từ điển**. Vì $n$ có thể lớn tới $10^9$, ta không thể tạo rồi sắp xếp tường minh tất cả các số. Thay vào đó, ta **duyệt tham lam trên một Trie khái niệm**.

Ta xem khoảng $[1, n]$ như một **cây tiền tố 10 nhánh (Trie)**:

- Mỗi node biểu diễn một prefix dạng số, bắt đầu từ root rỗng;
- Mỗi node có 10 node con, tương ứng với việc nối thêm một chữ số $0 \sim 9$;
- Ví dụ, prefix $1$ có các node con $10, 11, \ldots, 19$, còn node $10$ có các node con $100, 101, \ldots, 109$;
- Duyệt cây này tự nhiên tạo ra thứ tự từ điển.

```
root
├── 1
│   ├── 10
│   ├── 11
│   ├── ...
├── 2
├── ...
```

Ta dùng biến $\textit{curr}$ để biểu diễn prefix hiện tại, khởi tạo bằng $1$. Ở mỗi bước, ta mở rộng hoặc bỏ qua các prefix cho đến khi tìm được số nhỏ thứ \$k\$.

Ở mỗi bước, ta đếm số hợp lệ (tức là các số $\le n$ có prefix $\textit{curr}$) trong cây con của prefix này. Gọi số lượng đó là $\textit{count}(\text{curr})$:

- Nếu $k \ge \text{count}(\text{curr})$: số cần tìm không nằm trong cây con này. Ta bỏ qua toàn bộ cây con bằng cách chuyển sang node anh em kế tiếp:

    $$
    \textit{curr} \leftarrow \textit{curr} + 1,\quad k \leftarrow k - \text{count}(\text{curr})
    $$

- Ngược lại: số cần tìm nằm trong cây con này. Ta đi sâu xuống thêm một mức:

    $$
    \textit{curr} \leftarrow \textit{curr} \times 10,\quad k \leftarrow k - 1
    $$

Ở mỗi mức, ta mở rộng khoảng hiện tại bằng cách nhân với 10 rồi tiếp tục đi xuống cho đến khi vượt quá $n$.

Độ phức tạp thời gian là $O(\log^2 n)$ vì thao tác đếm và duyệt cấu trúc Trie đều cần số bước logarithmic. Độ phức tạp không gian là $O(1)$ vì ta chỉ dùng một vài biến để theo dõi prefix hiện tại và số lượng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findKthNumber(self, n: int, k: int) -> int:
        def count(curr):
            next, cnt = curr + 1, 0
            while curr <= n:
                cnt += min(n - curr + 1, next - curr)
                next, curr = next * 10, curr * 10
            return cnt

        curr = 1
        k -= 1
        while k:
            cnt = count(curr)
            if k >= cnt:
                k -= cnt
                curr += 1
            else:
                k -= 1
                curr *= 10
        return curr
```

#### Java

```java
class Solution {
    private int n;

    public int findKthNumber(int n, int k) {
        this.n = n;
        long curr = 1;
        --k;
        while (k > 0) {
            int cnt = count(curr);
            if (k >= cnt) {
                k -= cnt;
                ++curr;
            } else {
                --k;
                curr *= 10;
            }
        }
        return (int) curr;
    }

    public int count(long curr) {
        long next = curr + 1;
        long cnt = 0;
        while (curr <= n) {
            cnt += Math.min(n - curr + 1, next - curr);
            next *= 10;
            curr *= 10;
        }
        return (int) cnt;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int n;

    int findKthNumber(int n, int k) {
        this->n = n;
        --k;
        long long curr = 1;
        while (k) {
            int cnt = count(curr);
            if (k >= cnt) {
                k -= cnt;
                ++curr;
            } else {
                --k;
                curr *= 10;
            }
        }
        return (int) curr;
    }

    int count(long long curr) {
        long long next = curr + 1;
        int cnt = 0;
        while (curr <= n) {
            cnt += min(n - curr + 1, next - curr);
            next *= 10;
            curr *= 10;
        }
        return cnt;
    }
};
```

#### Go

```go
func findKthNumber(n int, k int) int {
	count := func(curr int) int {
		next := curr + 1
		cnt := 0
		for curr <= n {
			cnt += min(n-curr+1, next-curr)
			next *= 10
			curr *= 10
		}
		return cnt
	}
	curr := 1
	k--
	for k > 0 {
		cnt := count(curr)
		if k >= cnt {
			k -= cnt
			curr++
		} else {
			k--
			curr *= 10
		}
	}
	return curr
}
```

#### TypeScript

```ts
function findKthNumber(n: number, k: number): number {
    function count(curr: number): number {
        let next = curr + 1;
        let cnt = 0;
        while (curr <= n) {
            cnt += Math.min(n - curr + 1, next - curr);
            curr *= 10;
            next *= 10;
        }
        return cnt;
    }

    let curr = 1;
    k--;

    while (k > 0) {
        const cnt = count(curr);
        if (k >= cnt) {
            k -= cnt;
            curr += 1;
        } else {
            k -= 1;
            curr *= 10;
        }
    }

    return curr;
}
```

#### Rust

```rust
impl Solution {
    pub fn find_kth_number(n: i32, k: i32) -> i32 {
        fn count(mut curr: i64, n: i32) -> i32 {
            let mut next = curr + 1;
            let mut total = 0;
            let n = n as i64;
            while curr <= n {
                total += std::cmp::min(n - curr + 1, next - curr);
                curr *= 10;
                next *= 10;
            }
            total as i32
        }

        let mut curr = 1;
        let mut k = k - 1;

        while k > 0 {
            let cnt = count(curr as i64, n);
            if k >= cnt {
                k -= cnt;
                curr += 1;
            } else {
                k -= 1;
                curr *= 10;
            }
        }

        curr
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
