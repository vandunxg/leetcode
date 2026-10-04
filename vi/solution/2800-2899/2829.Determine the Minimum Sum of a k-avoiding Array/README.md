---
comments: true
difficulty: Medium
rating: 1347
source: Weekly Contest 359 Q2
tags:
    - Greedy
    - Math
---

<!-- problem:start -->

# [2829. Determine the Minimum Sum of a k-avoiding Array](https://leetcode.com/problems/determine-the-minimum-sum-of-a-k-avoiding-array)

[中文文档](/solution/2800-2899/2829.Determine%20the%20Minimum%20Sum%20of%20a%20k-avoiding%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên <code>n</code> và <code>k</code>.</p>

<p>Một mảng gồm các số nguyên dương <strong>phân biệt</strong> được gọi là mảng <b>k-avoiding</b> nếu không tồn tại cặp phần tử phân biệt nào có tổng bằng <code>k</code>.</p>

<p>Trả về <em>tổng <strong>nhỏ nhất</strong> có thể có của một mảng k-avoiding có độ dài </em><code>n</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 5, k = 4
<strong>Đầu ra:</strong> 18
<strong>Giải thích:</strong> Xét mảng k-avoiding [1,2,4,5,6], có tổng bằng 18.
Có thể chứng minh rằng không tồn tại mảng k-avoiding nào có tổng nhỏ hơn 18.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2, k = 6
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ta có thể tạo mảng [1,2], có tổng bằng 3.
Có thể chứng minh rằng không tồn tại mảng k-avoiding nào có tổng nhỏ hơn 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n, k &lt;= 50</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần $n$ số nguyên dương phân biệt, không có cặp nào có tổng bằng $k$, đồng thời tổng phải nhỏ nhất. Bắt đầu từ $1$, ta chọn giá trị tiếp theo chưa được sử dụng và ngay lập tức cấm phần tử đối ứng $k-i$, vì vậy mỗi lựa chọn luôn là giá trị nhỏ nhất còn được phép.

<!-- thinking:end -->

Bắt đầu từ số nguyên dương $i = 1$, ta lần lượt xác định xem $i$ có thể được thêm vào mảng hay không. Nếu có, ta thêm $i$ vào mảng, cộng giá trị đó vào đáp án, sau đó đánh dấu $k - i$ là đã được thăm, cho biết $k-i$ không thể được thêm vào mảng. Ta tiếp tục quá trình này cho đến khi độ dài mảng đạt $n$.

Độ phức tạp thời gian là $O(n + k)$, độ phức tạp không gian là $O(n + k)$, trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumSum(self, n: int, k: int) -> int:
        s, i = 0, 1
        vis = set()
        for _ in range(n):
            while i in vis:
                i += 1
            vis.add(k - i)
            s += i
            i += 1
        return s
```

#### Java

```java
class Solution {
    public int minimumSum(int n, int k) {
        int s = 0, i = 1;
        boolean[] vis = new boolean[n + k + 1];
        while (n-- > 0) {
            while (vis[i]) {
                ++i;
            }
            if (k >= i) {
                vis[k - i] = true;
            }
            s += i++;
        }
        return s;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumSum(int n, int k) {
        int s = 0, i = 1;
        bool vis[n + k + 1];
        memset(vis, false, sizeof(vis));
        while (n--) {
            while (vis[i]) {
                ++i;
            }
            if (k >= i) {
                vis[k - i] = true;
            }
            s += i++;
        }
        return s;
    }
};
```

#### Go

```go
func minimumSum(n int, k int) int {
	s, i := 0, 1
	vis := make([]bool, n+k+1)
	for ; n > 0; n-- {
		for vis[i] {
			i++
		}
		if k >= i {
			vis[k-i] = true
		}
		s += i
		i++
	}
	return s
}
```

#### TypeScript

```ts
function minimumSum(n: number, k: number): number {
    let s = 0;
    let i = 1;
    const vis: boolean[] = Array(n + k + 1).fill(false);
    while (n--) {
        while (vis[i]) {
            ++i;
        }
        if (k >= i) {
            vis[k - i] = true;
        }
        s += i++;
    }
    return s;
}
```

#### Rust

```rust
impl Solution {
    pub fn minimum_sum(n: i32, k: i32) -> i32 {
        let (mut s, mut i) = (0, 1);
        let mut vis = std::collections::HashSet::new();

        for _ in 0..n {
            while vis.contains(&i) {
                i += 1;
            }
            vis.insert(k - i);
            s += i;
            i += 1;
        }

        s
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
