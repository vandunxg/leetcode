---
comments: true
difficulty: Medium
tags:
    - Array
    - Binary Search
---

<!-- problem:start -->

# [1891. Cutting Ribbons 🔒](https://leetcode.com/problems/cutting-ribbons)

[中文文档](/solution/1800-1899/1891.Cutting%20Ribbons/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>ribbons</code>, trong đó <code>ribbons[i]</code> biểu diễn độ dài của dải ruy-băng thứ <code>i<sup>th</sup></code>, và một số nguyên <code>k</code>. Bạn có thể cắt bất kỳ dải ruy-băng nào thành bất kỳ số lượng đoạn nào có <strong>độ dài nguyên dương</strong>, hoặc không cắt gì cả.</p>

<ul>
	<li>Ví dụ, nếu có một dải ruy-băng độ dài <code>4</code>, bạn có thể:

    <ul>
    <li>Giữ nguyên dải ruy-băng độ dài <code>4</code>,</li>
    <li>Cắt thành một dải độ dài <code>3</code> và một dải độ dài <code>1</code>,</li>
    <li>Cắt thành hai dải độ dài <code>2</code>,</li>
    <li>Cắt thành một dải độ dài <code>2</code> và hai dải độ dài <code>1</code>, hoặc</li>
    <li>Cắt thành bốn dải độ dài <code>1</code>.</li>
    </ul>
    </li>

</ul>

<p>Nhiệm vụ của bạn là xác định độ dài <strong>lớn nhất</strong> <code>x</code> sao cho có thể cắt được <em>ít nhất</em> <code>k</code> dải, mỗi dải có độ dài <code>x</code>. Bạn có thể bỏ đi phần ruy-băng thừa sau khi cắt. Nếu <strong>không thể</strong> cắt được <code>k</code> dải có cùng độ dài, hãy trả về 0.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> ribbons = [9,7,5], k = 3
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong>
- Cắt dải đầu tiên thành hai dải, một dải độ dài 5 và một dải độ dài 4.
- Cắt dải thứ hai thành hai dải, một dải độ dài 5 và một dải độ dài 2.
- Giữ nguyên dải thứ ba.
Khi đó ta có 3 dải độ dài 5.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> ribbons = [7,5,9], k = 4
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong>
- Cắt dải đầu tiên thành hai dải, một dải độ dài 4 và một dải độ dài 3.
- Cắt dải thứ hai thành hai dải, một dải độ dài 4 và một dải độ dài 1.
- Cắt dải thứ ba thành ba dải, hai dải độ dài 4 và một dải độ dài 1.
Khi đó ta có 4 dải độ dài 4.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> ribbons = [5,7,9], k = 22
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không thể thu được k dải có cùng độ dài nguyên dương.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= ribbons.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= ribbons[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Dải ruy-băng chỉ có thể được cắt ngắn hơn; ta cần độ dài bằng nhau lớn nhất tạo ra ít nhất $k$ đoạn. Đoạn càng dài thì số đoạn tạo được càng ít.
>
> Tìm kiếm nhị phân độ dài trong $[0,\max ribbons]$ và kiểm tra xem $\sum \lfloor x/mid\rfloor \ge k$ hay không. Nếu độ dài hiện tại khả thi thì tăng lên, ngược lại thì giảm xuống.

<!-- thinking:end -->

Ta nhận thấy nếu có thể thu được $k$ dải độ dài $x$, thì cũng có thể thu được $k$ dải độ dài $x-1$. Điều này cho thấy tính đơn điệu, vì vậy ta có thể dùng tìm kiếm nhị phân để tìm độ dài lớn nhất $x$ sao cho thu được $k$ dải độ dài $x$.

Ta đặt biên trái của tìm kiếm nhị phân là $left=0$, biên phải là $right=\max(ribbons)$, và giá trị giữa là $mid=(left+right+1)/2$. Sau đó, ta tính số dải có thể thu được với độ dài $mid$, ký hiệu là $cnt$. Nếu $cnt \geq k$, nghĩa là có thể thu được $k$ dải độ dài $mid$, nên cập nhật $left$ thành $mid$. Ngược lại, cập nhật $right$ thành $mid-1$.

Cuối cùng, ta trả về $left$ là độ dài lớn nhất của các dải có thể thu được.

Độ phức tạp thời gian là $O(n \times \log M)$, trong đó $n$ và $M$ lần lượt là số lượng dải và độ dài lớn nhất của các dải. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxLength(self, ribbons: List[int], k: int) -> int:
        left, right = 0, max(ribbons)
        while left < right:
            mid = (left + right + 1) >> 1
            cnt = sum(x // mid for x in ribbons)
            if cnt >= k:
                left = mid
            else:
                right = mid - 1
        return left
```

#### Java

```java
class Solution {
    public int maxLength(int[] ribbons, int k) {
        int left = 0, right = 0;
        for (int x : ribbons) {
            right = Math.max(right, x);
        }
        while (left < right) {
            int mid = (left + right + 1) >>> 1;
            int cnt = 0;
            for (int x : ribbons) {
                cnt += x / mid;
            }
            if (cnt >= k) {
                left = mid;
            } else {
                right = mid - 1;
            }
        }
        return left;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxLength(vector<int>& ribbons, int k) {
        int left = 0, right = *max_element(ribbons.begin(), ribbons.end());
        while (left < right) {
            int mid = (left + right + 1) >> 1;
            int cnt = 0;
            for (int ribbon : ribbons) {
                cnt += ribbon / mid;
            }
            if (cnt >= k) {
                left = mid;
            } else {
                right = mid - 1;
            }
        }
        return left;
    }
};
```

#### Go

```go
func maxLength(ribbons []int, k int) int {
	left, right := 0, slices.Max(ribbons)
	for left < right {
		mid := (left + right + 1) >> 1
		cnt := 0
		for _, x := range ribbons {
			cnt += x / mid
		}
		if cnt >= k {
			left = mid
		} else {
			right = mid - 1
		}
	}
	return left
}
```

#### TypeScript

```ts
function maxLength(ribbons: number[], k: number): number {
    let left = 0;
    let right = Math.max(...ribbons);
    while (left < right) {
        const mid = (left + right + 1) >> 1;
        let cnt = 0;
        for (const x of ribbons) {
            cnt += Math.floor(x / mid);
        }
        if (cnt >= k) {
            left = mid;
        } else {
            right = mid - 1;
        }
    }
    return left;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_length(ribbons: Vec<i32>, k: i32) -> i32 {
        let mut left = 0i32;
        let mut right = *ribbons.iter().max().unwrap();
        while left < right {
            let mid = (left + right + 1) / 2;
            let mut cnt = 0i32;
            for &entry in ribbons.iter() {
                cnt += entry / mid;
                if cnt >= k {
                    break;
                }
            }
            if cnt >= k {
                left = mid;
            } else {
                right = mid - 1;
            }
        }
        return left;
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} ribbons
 * @param {number} k
 * @return {number}
 */
var maxLength = function (ribbons, k) {
    let left = 0;
    let right = Math.max(...ribbons);
    while (left < right) {
        const mid = (left + right + 1) >> 1;
        let cnt = 0;
        for (const x of ribbons) {
            cnt += Math.floor(x / mid);
        }
        if (cnt >= k) {
            left = mid;
        } else {
            right = mid - 1;
        }
    }
    return left;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
