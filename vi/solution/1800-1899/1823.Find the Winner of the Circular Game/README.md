---
comments: true
difficulty: Medium
rating: 1412
source: Weekly Contest 236 Q2
tags:
    - Recursion
    - Queue
    - Array
    - Math
    - Simulation
---

<!-- problem:start -->

# [1823. Find the Winner of the Circular Game](https://leetcode.com/problems/find-the-winner-of-the-circular-game)

[中文文档](/solution/1800-1899/1823.Find%20the%20Winner%20of%20the%20Circular%20Game/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> người bạn đang chơi một trò chơi. Họ ngồi thành vòng tròn và được đánh số từ <code>1</code> đến <code>n</code> theo <strong>chiều kim đồng hồ</strong>. Cụ thể hơn, đi theo chiều kim đồng hồ từ người bạn thứ <code>i<sup>th</sup></code> sẽ đến người thứ <code>(i+1)<sup>th</sup></code> với <code>1 &lt;= i &lt; n</code>, còn đi từ người thứ <code>n<sup>th</sup></code> sẽ đến người thứ <code>1<sup>st</sup></code>.</p>

<p>Luật chơi như sau:</p>

<ol>
<li><strong>Bắt đầu</strong> từ người bạn thứ <code>1<sup>st</sup></code>.</li>
<li>Đếm <code>k</code> người tiếp theo theo chiều kim đồng hồ, <strong>bao gồm</strong> cả người bạn mà bạn bắt đầu từ đó. Việc đếm quay vòng quanh vòng tròn và có thể đếm một số người nhiều hơn một lần.</li>
<li>Người cuối cùng được đếm rời khỏi vòng tròn và thua cuộc.</li>
<li>Nếu vòng tròn vẫn còn hơn một người, quay lại bước <code>2</code>, <strong>bắt đầu</strong> từ người bạn <strong>ngay theo chiều kim đồng hồ</strong> sau người vừa thua, rồi lặp lại.</li>
<li>Nếu không, người cuối cùng còn lại trong vòng tròn thắng.</li>
</ol>

<p>Cho số người bạn <code>n</code> và số nguyên <code>k</code>, hãy trả về <em>người thắng trò chơi</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1800-1899/1823.Find%20the%20Winner%20of%20the%20Circular%20Game/images/ic234-q2-ex11.png" style="width: 500px; height: 345px;" />
<pre>
<strong>Đầu vào:</strong> n = 5, k = 2
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Các bước của trò chơi:
1) Bắt đầu từ người bạn 1.
2) Đếm 2 người theo chiều kim đồng hồ, đó là người 1 và 2.
3) Người 2 rời vòng tròn. Người tiếp theo bắt đầu là người 3.
4) Đếm 2 người theo chiều kim đồng hồ, đó là người 3 và 4.
5) Người 4 rời vòng tròn. Người tiếp theo bắt đầu là người 5.
6) Đếm 2 người theo chiều kim đồng hồ, đó là người 5 và 1.
7) Người 1 rời vòng tròn. Người tiếp theo bắt đầu là người 3.
8) Đếm 2 người theo chiều kim đồng hồ, đó là người 3 và 5.
9) Người 5 rời vòng tròn. Chỉ còn người 3, nên người đó thắng.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 6, k = 5
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Những người bạn rời đi theo thứ tự: 5, 4, 6, 2, 3. Người thắng là người bạn 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= n &lt;= 500</code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong></p>

<p>Bạn có thể giải bài toán này trong thời gian tuyến tính và không gian hằng số không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Có $n$ người đứng thành vòng tròn và cứ người thứ $k$ lại bị loại. Vì $n\le 500$, mô phỏng bằng danh sách vẫn đủ nhanh, nhưng đếm tuần tự $k$ bước không tận dụng được công thức truy hồi.
>
> Người thắng trong $n$ người là người thắng trong $n-1$ người sau khi dịch đi $k$ vị trí theo modulo $n$ (coi $0$ là $n$). Trường hợp cơ sở $n=1$ là người số $1$. Công thức truy hồi giải bài toán trong $O(n)$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findTheWinner(self, n: int, k: int) -> int:
        if n == 1:
            return 1
        ans = (k + self.findTheWinner(n - 1, k)) % n
        return n if ans == 0 else ans
```

#### Java

```java
class Solution {
    public int findTheWinner(int n, int k) {
        if (n == 1) {
            return 1;
        }
        int ans = (findTheWinner(n - 1, k) + k) % n;
        return ans == 0 ? n : ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findTheWinner(int n, int k) {
        if (n == 1) return 1;
        int ans = (findTheWinner(n - 1, k) + k) % n;
        return ans == 0 ? n : ans;
    }
};
```

#### Go

```go
func findTheWinner(n int, k int) int {
	if n == 1 {
		return 1
	}
	ans := (findTheWinner(n-1, k) + k) % n
	if ans == 0 {
		return n
	}
	return ans
}
```

#### TypeScript

```ts
function findTheWinner(n: number, k: number): number {
    if (n === 1) {
        return 1;
    }
    const ans = (k + findTheWinner(n - 1, k)) % n;
    return ans ? ans : n;
}
```

#### Rust

```rust
impl Solution {
    pub fn find_the_winner(n: i32, k: i32) -> i32 {
        if n == 1 {
            return 1;
        }
        let mut ans = (k + Solution::find_the_winner(n - 1, k)) % n;
        return if ans == 0 { n } else { ans };
    }
}
```

#### JavaScript

```js
/**
 * @param {number} n
 * @param {number} k
 * @return {number}
 */
var findTheWinner = function (n, k) {
    if (n === 1) {
        return 1;
    }
    const ans = (k + findTheWinner(n - 1, k)) % n;
    return ans ? ans : n;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 dùng một công thức truy hồi chỉ số ngắn gọn. Ta cũng có thể lưu những người còn lại trong một list hoặc deque, liên tục đếm đến $k$ rồi xóa cho đến khi chỉ còn một người. Với $n$ nhỏ, cách mô phỏng này bám sát đề bài và các nhãn bắt đầu từ $1$.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function findTheWinner(n: number, k: number): number {
    const arr = Array.from({ length: n }, (_, i) => i + 1);
    let i = 0;

    while (arr.length > 1) {
        i = (i + k - 1) % arr.length;
        arr.splice(i, 1);
    }

    return arr[0];
}
```

#### JavaScript

```js
function findTheWinner(n, k) {
    const arr = Array.from({ length: n }, (_, i) => i + 1);
    let i = 0;

    while (arr.length > 1) {
        i = (i + k - 1) % arr.length;
        arr.splice(i, 1);
    }

    return arr[0];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
