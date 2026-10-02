---
comments: true
difficulty: Medium
rating: 1418
source: Weekly Contest 138 Q2
tags:
    - Array
    - Sliding Window
---

<!-- problem:start -->

# [1052. Grumpy Bookstore Owner](https://leetcode.com/problems/grumpy-bookstore-owner)

[中文文档](/solution/1000-1099/1052.Grumpy%20Bookstore%20Owner/README.md)

## Mô tả

<!-- description:start -->

<p>Một chủ hiệu sách mở cửa hàng trong <code>n</code> phút. Cho mảng số nguyên <code>customers</code> có độ dài <code>n</code>, trong đó <code>customers[i]</code> là số khách vào cửa hàng ở đầu phút thứ <code>i</code>; tất cả khách này rời đi sau khi phút đó kết thúc.</p>

<p>Trong một số phút, chủ hiệu sách cáu kỉnh. Cho mảng nhị phân grumpy, trong đó <code>grumpy[i]</code> bằng <code>1</code> nếu chủ hiệu sách cáu kỉnh trong phút thứ <code>i</code>, và bằng <code>0</code> nếu không.</p>

<p>Khi chủ hiệu sách cáu kỉnh, khách đến trong phút đó sẽ không <strong>hài lòng</strong>. Nếu không, họ sẽ hài lòng.</p>

<p>Chủ hiệu sách biết một kỹ thuật bí mật giúp mình <strong>không cáu kỉnh</strong> trong <code>minutes</code> phút liên tiếp, nhưng chỉ được dùng kỹ thuật này <strong>một lần</strong>.</p>

<p>Hãy trả về số khách <strong>tối đa</strong> có thể <em>hài lòng</em> trong suốt cả ngày.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">customers = [1,0,1,2,1,1,7,5], grumpy = [0,1,0,1,0,1,0,1], minutes = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">16</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chủ hiệu sách dùng kỹ thuật để không cáu kỉnh trong 3 phút cuối.</p>

<p>Số khách tối đa có thể hài lòng = 1 + 1 + 1 + 1 + 7 + 5 = 16.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">customers = [1], grumpy = [0], minutes = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == customers.length == grumpy.length</code></li>
	<li><code>1 &lt;= minutes &lt;= n &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>0 &lt;= customers[i] &lt;= 1000</code></li>
	<li><code>grumpy[i]</code> chỉ có thể là <code>0</code> hoặc <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sliding Window

<!-- thinking:start -->

> **Tư duy**
>
> Khách đến trong những phút chủ hiệu sách bình tĩnh luôn được tính; kỹ thuật chỉ giúp xử lý một cửa sổ gồm $\textit{minutes}$ phút cáu kỉnh. Với $n\le 2\times 10^4$, chỉ cần duyệt một lượt.
>
> Cộng số khách đến trong các phút bình tĩnh, rồi tìm tổng khách đến trong các phút cáu kỉnh lớn nhất trên mọi cửa sổ dài $\textit{minutes}$.
>
> Khi trượt cửa sổ, cộng $customers[i]\cdot grumpy[i]$ và trừ giá trị vừa rời khỏi cửa sổ. Đáp án bằng tổng khách trong các phút bình tĩnh cộng với giá trị lớn nhất đó.

<!-- thinking:end -->

Theo mô tả bài toán, ta chỉ cần đếm số khách trong những phút chủ hiệu sách không cáu kỉnh, gọi là $tot$, rồi cộng số khách cáu kỉnh nhiều nhất trong một sliding window có kích thước `minutes`, gọi là $mx$.

Ta định nghĩa biến $cnt$ để lưu số khách đến trong những phút chủ hiệu sách cáu kỉnh nằm trong cửa sổ trượt. Ban đầu, $cnt$ bằng số khách cáu kỉnh trong `minutes` phút đầu tiên. Sau đó, ta duyệt mảng; mỗi lần trượt cửa sổ, cập nhật $cnt$ và đồng thời cập nhật $mx$.

Cuối cùng, trả về $tot + mx$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng `customers`. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSatisfied(
        self, customers: List[int], grumpy: List[int], minutes: int
    ) -> int:
        mx = cnt = sum(c * g for c, g in zip(customers[:minutes], grumpy))
        for i in range(minutes, len(customers)):
            cnt += customers[i] * grumpy[i]
            cnt -= customers[i - minutes] * grumpy[i - minutes]
            mx = max(mx, cnt)
        return sum(c * (g ^ 1) for c, g in zip(customers, grumpy)) + mx
```

#### Java

```java
class Solution {
    public int maxSatisfied(int[] customers, int[] grumpy, int minutes) {
        int cnt = 0;
        int tot = 0;
        for (int i = 0; i < minutes; ++i) {
            cnt += customers[i] * grumpy[i];
            tot += customers[i] * (grumpy[i] ^ 1);
        }
        int mx = cnt;
        int n = customers.length;
        for (int i = minutes; i < n; ++i) {
            cnt += customers[i] * grumpy[i];
            cnt -= customers[i - minutes] * grumpy[i - minutes];
            mx = Math.max(mx, cnt);
            tot += customers[i] * (grumpy[i] ^ 1);
        }
        return tot + mx;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxSatisfied(vector<int>& customers, vector<int>& grumpy, int minutes) {
        int cnt = 0;
        int tot = 0;
        for (int i = 0; i < minutes; ++i) {
            cnt += customers[i] * grumpy[i];
            tot += customers[i] * (grumpy[i] ^ 1);
        }
        int mx = cnt;
        int n = customers.size();
        for (int i = minutes; i < n; ++i) {
            cnt += customers[i] * grumpy[i];
            cnt -= customers[i - minutes] * grumpy[i - minutes];
            mx = max(mx, cnt);
            tot += customers[i] * (grumpy[i] ^ 1);
        }
        return tot + mx;
    }
};
```

#### Go

```go
func maxSatisfied(customers []int, grumpy []int, minutes int) int {
	var cnt, tot int
	for i, c := range customers[:minutes] {
		cnt += c * grumpy[i]
		tot += c * (grumpy[i] ^ 1)
	}
	mx := cnt
	for i := minutes; i < len(customers); i++ {
		cnt += customers[i] * grumpy[i]
		cnt -= customers[i-minutes] * grumpy[i-minutes]
		mx = max(mx, cnt)
		tot += customers[i] * (grumpy[i] ^ 1)
	}
	return tot + mx
}
```

#### TypeScript

```ts
function maxSatisfied(customers: number[], grumpy: number[], minutes: number): number {
    let [cnt, tot] = [0, 0];
    for (let i = 0; i < minutes; ++i) {
        cnt += customers[i] * grumpy[i];
        tot += customers[i] * (grumpy[i] ^ 1);
    }
    let mx = cnt;
    for (let i = minutes; i < customers.length; ++i) {
        cnt += customers[i] * grumpy[i];
        cnt -= customers[i - minutes] * grumpy[i - minutes];
        mx = Math.max(mx, cnt);
        tot += customers[i] * (grumpy[i] ^ 1);
    }
    return tot + mx;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_satisfied(customers: Vec<i32>, grumpy: Vec<i32>, minutes: i32) -> i32 {
        let mut cnt = 0;
        let mut tot = 0;
        let minutes = minutes as usize;
        for i in 0..minutes {
            cnt += customers[i] * grumpy[i];
            tot += customers[i] * (1 - grumpy[i]);
        }
        let mut mx = cnt;
        let n = customers.len();
        for i in minutes..n {
            cnt += customers[i] * grumpy[i];
            cnt -= customers[i - minutes] * grumpy[i - minutes];
            mx = mx.max(cnt);
            tot += customers[i] * (1 - grumpy[i]);
        }
        tot + mx
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
