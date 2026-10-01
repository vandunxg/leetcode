---
comments: true
difficulty: Medium
tags:
    - Array
    - Binary Search
---

<!-- problem:start -->

# [275. H-Index II](https://leetcode.com/problems/h-index-ii)

[中文文档](/solution/0200-0299/0275.H-Index%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>citations</code>, trong đó <code>citations[i]</code> là số lượt trích dẫn mà bài báo thứ <code>i<sup>th</sup></code> của một nhà nghiên cứu nhận được, và <code>citations</code> được sắp xếp theo <strong>thứ tự không giảm</strong>. Hãy trả về <em>h-index của nhà nghiên cứu đó</em>.</p>

<p>Theo <a href="https://en.wikipedia.org/wiki/H-index" target="_blank">định nghĩa h-index trên Wikipedia</a>: H-index là giá trị lớn nhất của <code>h</code> sao cho nhà nghiên cứu đã xuất bản ít nhất <code>h</code> bài báo, và mỗi bài được trích dẫn ít nhất <code>h</code> lần.</p>

<p>Bạn phải viết thuật toán chạy trong thời gian logarit.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> citations = [0,1,3,5,6]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> [0,1,3,5,6] nghĩa là nhà nghiên cứu có tổng cộng 5 bài báo, lần lượt nhận được 0, 1, 3, 5 và 6 lượt trích dẫn.
Vì nhà nghiên cứu có 3 bài báo, mỗi bài được trích dẫn ít nhất 3 lần, còn hai bài kia được trích dẫn không quá 3 lần, nên h-index bằng 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> citations = [1,2,100]
<strong>Đầu ra:</strong> 2
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == citations.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= citations[i] &lt;= 1000</code></li>
	<li><code>citations</code> được sắp xếp theo <strong>thứ tự tăng dần</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Mảng đã được sắp xếp sẵn nên không cần bước sắp xếp như trước. $citations[n-h]\ge h$ khi và chỉ khi $h$ thỏa điều kiện; tìm kiếm nhị phân giúp tìm giá trị $h$ lớn nhất trong thời gian logarit.

<!-- thinking:end -->

Ta nhận thấy nếu có ít nhất $x$ bài báo được trích dẫn từ $x$ lần trở lên, thì với mọi $y \lt x$, số bài báo được trích dẫn ít nhất $y$ lần cũng không nhỏ hơn $y$. Điều này cho thấy tính đơn điệu.

Vì vậy, ta dùng tìm kiếm nhị phân để tìm giá trị $h$ lớn nhất thỏa mãn điều kiện. Để có ít nhất $h$ bài báo được trích dẫn ít nhất $h$ lần, ta cần $citations[n - mid] \ge mid$.

Độ phức tạp thời gian là $O(\log n)$, với $n$ là độ dài mảng $citations$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def hIndex(self, citations: List[int]) -> int:
        n = len(citations)
        left, right = 0, n
        while left < right:
            mid = (left + right + 1) >> 1
            if citations[n - mid] >= mid:
                left = mid
            else:
                right = mid - 1
        return left
```

#### Java

```java
class Solution {
    public int hIndex(int[] citations) {
        int n = citations.length;
        int left = 0, right = n;
        while (left < right) {
            int mid = (left + right) >>> 1;
            if (citations[mid] >= n - mid) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return n - left;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int hIndex(vector<int>& citations) {
        int n = citations.size();
        int left = 0, right = n;
        while (left < right) {
            int mid = (left + right + 1) >> 1;
            if (citations[n - mid] >= mid)
                left = mid;
            else
                right = mid - 1;
        }
        return left;
    }
};
```

#### Go

```go
func hIndex(citations []int) int {
	n := len(citations)
	left, right := 0, n
	for left < right {
		mid := (left + right + 1) >> 1
		if citations[n-mid] >= mid {
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
function hIndex(citations: number[]): number {
    const n = citations.length;
    let left = 0,
        right = n;
    while (left < right) {
        const mid = (left + right + 1) >> 1;
        if (citations[n - mid] >= mid) {
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
    pub fn h_index(citations: Vec<i32>) -> i32 {
        let n = citations.len();
        let (mut left, mut right) = (0, n);
        while left < right {
            let mid = ((left + right + 1) >> 1) as usize;
            if citations[n - mid] >= (mid as i32) {
                left = mid;
            } else {
                right = mid - 1;
            }
        }
        left as i32
    }
}
```

#### C#

```cs
public class Solution {
    public int HIndex(int[] citations) {
        int n = citations.Length;
        int left = 0, right = n;
        while (left < right) {
            int mid = (left + right + 1) >> 1;
            if (citations[n - mid] >= mid) {
                left = mid;
            } else {
                right = mid - 1;
            }
        }
        return left;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
