---
comments: true
difficulty: Easy
rating: 1197
source: Weekly Contest 388 Q1
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [3074. Apple Redistribution into Boxes](https://leetcode.com/problems/apple-redistribution-into-boxes)

[中文文档](/solution/3000-3099/3074.Apple%20Redistribution%20into%20Boxes/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <code>apple</code> có độ dài <code>n</code> và một mảng <code>capacity</code> có độ dài <code>m</code>.</p>

<p>Có <code>n</code> gói, trong đó gói <code>i<sup>th</sup></code> chứa <code>apple[i]</code> quả táo. Ngoài ra có <code>m</code> hộp, trong đó hộp <code>i<sup>th</sup></code> có sức chứa <code>capacity[i]</code> quả táo.</p>

<p>Trả về <em><strong>số hộp nhỏ nhất</strong> cần chọn để phân phối lại </em><code>n</code><em> gói táo vào các hộp</em>.</p>

<p><strong>Lưu ý</strong> rằng táo trong cùng một gói có thể được phân phối vào các hộp khác nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> apple = [1,3,2], capacity = [4,3,1,5,2]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Chúng ta sẽ dùng các hộp có sức chứa 4 và 5.
Có thể phân phối táo vì tổng sức chứa lớn hơn hoặc bằng tổng số táo.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> apple = [5,5,5], capacity = [2,4,2,7]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Chúng ta cần dùng tất cả các hộp.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == apple.length &lt;= 50</code></li>
	<li><code>1 &lt;= m == capacity.length &lt;= 50</code></li>
	<li><code>1 &lt;= apple[i], capacity[i] &lt;= 50</code></li>
	<li>Dữ liệu đầu vào được tạo sao cho có thể phân phối lại các gói táo vào các hộp.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thuật toán tham lam + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Táo có thể được phân phối lại tùy ý; mục tiêu là dùng ít hộp nhất. $n,m \le 50$.
>
> Chỉ tổng sức chứa so với tổng số táo mới quan trọng. Các hộp lớn hơn đạt tổng đó sớm hơn.
>
> Sắp xếp các sức chứa theo thứ tự giảm dần, trừ dần tổng số táo và trả về số hộp đã dùng.

<!-- thinking:end -->

Để giảm thiểu số hộp cần dùng, chúng ta nên ưu tiên các hộp có sức chứa lớn hơn. Vì vậy, ta sắp xếp các hộp theo thứ tự giảm dần của sức chứa, rồi lần lượt sử dụng từng hộp cho đến khi phân phối hết táo. Khi đó, ta trả về số hộp đã dùng.

Độ phức tạp thời gian là $O(m \times \log m + n)$ và độ phức tạp không gian là $O(\log m)$, trong đó $m$ và $n$ lần lượt là độ dài của các mảng $\textit{capacity}$ và $\textit{apple}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumBoxes(self, apple: List[int], capacity: List[int]) -> int:
        capacity.sort(reverse=True)
        s = sum(apple)
        for i, c in enumerate(capacity, 1):
            s -= c
            if s <= 0:
                return i
```

#### Java

```java
class Solution {
    public int minimumBoxes(int[] apple, int[] capacity) {
        Arrays.sort(capacity);
        int s = 0;
        for (int x : apple) {
            s += x;
        }
        for (int i = 1, n = capacity.length;; ++i) {
            s -= capacity[n - i];
            if (s <= 0) {
                return i;
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumBoxes(vector<int>& apple, vector<int>& capacity) {
        sort(capacity.rbegin(), capacity.rend());
        int s = accumulate(apple.begin(), apple.end(), 0);
        for (int i = 1;; ++i) {
            s -= capacity[i - 1];
            if (s <= 0) {
                return i;
            }
        }
    }
};
```

#### Go

```go
func minimumBoxes(apple []int, capacity []int) int {
	sort.Ints(capacity)
	s := 0
	for _, x := range apple {
		s += x
	}
	for i := 1; ; i++ {
		s -= capacity[len(capacity)-i]
		if s <= 0 {
			return i
		}
	}
}
```

#### TypeScript

```ts
function minimumBoxes(apple: number[], capacity: number[]): number {
    capacity.sort((a, b) => b - a);
    let s = apple.reduce((acc, cur) => acc + cur, 0);
    for (let i = 1; ; ++i) {
        s -= capacity[i - 1];
        if (s <= 0) {
            return i;
        }
    }
}
```

#### Rust

```rust
impl Solution {
    pub fn minimum_boxes(apple: Vec<i32>, mut capacity: Vec<i32>) -> i32 {
        capacity.sort();

        let mut s: i32 = apple.iter().sum();

        let n = capacity.len();
        let mut i = 1;
        loop {
            s -= capacity[n - i];
            if s <= 0 {
                return i as i32;
            }
            i += 1;
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
