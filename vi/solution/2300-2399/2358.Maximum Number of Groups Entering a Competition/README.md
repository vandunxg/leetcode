---
comments: true
difficulty: Medium
rating: 1502
source: Weekly Contest 304 Q2
tags:
    - Greedy
    - Array
    - Math
    - Binary Search
---

<!-- problem:start -->

# [2358. Maximum Number of Groups Entering a Competition](https://leetcode.com/problems/maximum-number-of-groups-entering-a-competition)

[中文文档](/solution/2300-2399/2358.Maximum%20Number%20of%20Groups%20Entering%20a%20Competition/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên dương <code>grades</code> biểu diễn điểm số của sinh viên trong một trường đại học. Bạn muốn đưa <strong>tất cả</strong> sinh viên này vào một cuộc thi, được chia thành các nhóm không rỗng theo <strong>thứ tự</strong>, sao cho thứ tự đó thỏa mãn các điều kiện sau:</p>

<ul>
	<li>Tổng điểm của sinh viên trong nhóm <code>i<sup>th</sup></code> <strong>nhỏ hơn</strong> tổng điểm của sinh viên trong nhóm <code>(i + 1)<sup>th</sup></code>, với mọi nhóm (trừ nhóm cuối).</li>
	<li>Tổng số sinh viên trong nhóm <code>i<sup>th</sup></code> <strong>nhỏ hơn</strong> tổng số sinh viên trong nhóm <code>(i + 1)<sup>th</sup></code>, với mọi nhóm (trừ nhóm cuối).</li>
</ul>

<p>Trả về <em><strong>số nhóm tối đa</strong> có thể tạo thành</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> grades = [10,6,12,7,3,5]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Sau đây là một cách có thể tạo thành 3 nhóm sinh viên:
- Nhóm thứ <sup>1st</sup> gồm sinh viên có điểm = [12]. Tổng điểm: 12. Số sinh viên: 1
- Nhóm thứ <sup>2nd</sup> gồm sinh viên có điểm = [6,7]. Tổng điểm: 6 + 7 = 13. Số sinh viên: 2
- Nhóm thứ <sup>3rd</sup> gồm sinh viên có điểm = [10,3,5]. Tổng điểm: 10 + 3 + 5 = 18. Số sinh viên: 3
Có thể chứng minh rằng không thể tạo thành hơn 3 nhóm.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> grades = [8,8]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Ta chỉ có thể tạo thành 1 nhóm, vì nếu tạo thành 2 nhóm thì số sinh viên trong cả hai nhóm sẽ bằng nhau.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= grades.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= grades[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Kích thước nhóm và tổng điểm đều phải tăng nghiêm ngặt. $n \le 10^5$ không cho phép tìm kiếm các cách chia nhóm. Sau khi sắp xếp điểm, các kích thước $1,2,\ldots,k$ sẽ tự động làm tổng điểm tăng.
>
> Chỉ còn điều kiện $\frac{k(k+1)}{2}\le n$. Ta tìm kiếm nhị phân $k$ (thông qua $bisect$ trên $x^2+x$ so với $2n$) để tìm số nhóm khả thi lớn nhất.

<!-- thinking:end -->

Quan sát các điều kiện của đề bài, số sinh viên trong nhóm thứ $i$ phải nhỏ hơn số sinh viên trong nhóm thứ $(i+1)$, đồng thời tổng điểm của sinh viên trong nhóm thứ $i$ phải nhỏ hơn tổng điểm của sinh viên trong nhóm thứ $(i+1)$. Ta chỉ cần sắp xếp sinh viên theo điểm số tăng dần, sau đó lần lượt phân bổ $1$, $2$, ..., $k$ sinh viên cho mỗi nhóm. Nếu nhóm cuối không đủ $k$ sinh viên, ta có thể phân bổ các sinh viên còn lại vào nhóm cuối trước đó.

Do đó, ta cần tìm $k$ lớn nhất sao cho $\frac{(1 + k) \times k}{2} \leq n$, trong đó $n$ là tổng số sinh viên. Ta có thể dùng tìm kiếm nhị phân để giải quyết.

Ta đặt cận trái của tìm kiếm nhị phân là $l = 1$ và cận phải là $r = n$. Ở mỗi bước, phần tử giữa là $mid = \lfloor \frac{l + r + 1}{2} \rfloor$. Nếu $(1 + mid) \times mid \gt 2 \times n$, điều đó có nghĩa là $mid$ quá lớn, nên ta thu hẹp cận phải về $mid - 1$; ngược lại, ta tăng cận trái lên $mid$.

Cuối cùng, ta trả về $l$ làm đáp án.

Độ phức tạp thời gian là $O(\log n)$ và độ phức tạp không gian là $O(1)$, trong đó $n$ là tổng số sinh viên.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumGroups(self, grades: List[int]) -> int:
        n = len(grades)
        return bisect_right(range(n + 1), n * 2, key=lambda x: x * x + x) - 1
```

#### Java

```java
class Solution {
    public int maximumGroups(int[] grades) {
        int n = grades.length;
        int l = 0, r = n;
        while (l < r) {
            int mid = (l + r + 1) >> 1;
            if (1L * mid * mid + mid > n * 2L) {
                r = mid - 1;
            } else {
                l = mid;
            }
        }
        return l;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumGroups(vector<int>& grades) {
        int n = grades.size();
        int l = 0, r = n;
        while (l < r) {
            int mid = (l + r + 1) >> 1;
            if (1LL * mid * mid + mid > n * 2LL) {
                r = mid - 1;
            } else {
                l = mid;
            }
        }
        return l;
    }
};
```

#### Go

```go
func maximumGroups(grades []int) int {
	n := len(grades)
	return sort.Search(n, func(k int) bool {
		k++
		return k*k+k > n*2
	})
}
```

#### TypeScript

```ts
function maximumGroups(grades: number[]): number {
    const n = grades.length;
    let l = 1;
    let r = n;
    while (l < r) {
        const mid = (l + r + 1) >> 1;
        if (mid * mid + mid > n * 2) {
            r = mid - 1;
        } else {
            l = mid;
        }
    }
    return l;
}
```

#### Rust

```rust
impl Solution {
    pub fn maximum_groups(grades: Vec<i32>) -> i32 {
        let n = grades.len() as i64;
        let (mut l, mut r) = (0i64, n);
        while l < r {
            let mid = (l + r + 1) / 2;
            if mid * mid + mid > 2 * n {
                r = mid - 1;
            } else {
                l = mid;
            }
        }
        l as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
