---
comments: true
difficulty: Medium
rating: 1443
source: Weekly Contest 494 Q2
tags:
    - Array
    - Math
---

<!-- problem:start -->

# [3876. Construct Uniform Parity Array II](https://leetcode.com/problems/construct-uniform-parity-array-ii)

[中文文档](/solution/3800-3899/3876.Construct%20Uniform%20Parity%20Array%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng gồm <code>n</code> số nguyên <strong>phân biệt</strong> <code>nums1</code>.</p>

<p>Bạn muốn xây dựng một mảng khác <code>nums2</code> có độ dài <code>n</code> sao cho các phần tử trong <code>nums2</code> hoặc là <strong>đều lẻ hoặc đều chẵn</strong>.</p>

<p>Với mỗi chỉ số <code>i</code>, bạn phải chọn <strong>chính xác một</strong> trong các lựa chọn sau (theo bất kỳ thứ tự nào):</p>

<ul>
	<li><code>nums2[i] = nums1[i]</code>​​​​​​​</li>
	<li><code>nums2[i] = nums1[i] - nums1[j]</code>, với một chỉ số <code>j != i</code>, sao cho <code>nums1[i] - nums1[j] &gt;= 1</code></li>
</ul>

<p>Trả về <code>true</code> nếu có thể xây dựng mảng như vậy, ngược lại trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums1 = [1,4,7]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong>​​​​​​​​​​​​​​</p>

<ul>
	<li>Đặt <code>nums2[0] = nums1[0] = 1</code>.</li>
	<li>Đặt <code>nums2[1] = nums1[1] - nums1[0] = 4 - 1 = 3</code>.</li>
	<li>Đặt <code>nums2[2] = nums1[2] = 7</code>.</li>
	<li><code>nums2 = [1, 3, 7]</code>, và tất cả các phần tử đều lẻ. Do đó, đáp án là <code>true</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums1 = [2,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không thể xây dựng <code>nums2</code> sao cho tất cả các phần tử có cùng parity. Do đó, đáp án là <code>false</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums1 = [4,6]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Đặt <code>nums2[0] = nums1[0] = 4</code>.</li>
	<li>Đặt <code>nums2[1] = nums1[1] = 6</code>.</li>
	<li><code>nums2 = [4, 6]</code>, và tất cả các phần tử đều chẵn. Do đó, đáp án là <code>true</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums1.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums1[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>nums1</code> gồm các số nguyên phân biệt.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Brain Teaser

<!-- thinking:start -->

> **Tư duy**
>
> Tương tự phần I, nhưng hiệu phải dương. Mảng có cùng parity vẫn có thể được sao chép nguyên trạng.
>
> Để tạo ra toàn số lẻ, ta phải trừ đi một số chẵn lớn hơn. Nếu số lẻ nhỏ nhất nhỏ hơn một số chẵn nào đó, số chẵn ấy không thể trở thành một hiệu dương lẻ, cũng không thể giữ nguyên là số chẵn trong khi các số còn lại trở thành số lẻ.
>
> Vì vậy, nếu tồn tại một số chẵn nhỏ hơn số lẻ nhỏ nhất thì không thể thực hiện; ngược lại, ta luôn xây dựng được mảng hợp lệ.
>
> Nếu không có số lẻ, mảng đã gồm toàn số chẵn nên đáp án là true.

<!-- thinking:end -->

Nếu tất cả các phần tử trong $\textit{nums1}$ đều lẻ hoặc đều chẵn, ta có thể đặt trực tiếp $\textit{nums2}$ bằng $\textit{nums1}$, khi đó điều kiện được thỏa mãn.

Nếu $\textit{nums1}$ chứa cả số lẻ và số chẵn, ta cần tìm số lẻ nhỏ nhất $mn$, rồi kiểm tra xem trong $\textit{nums1}$ có số chẵn $x$ nào thỏa mãn $x < mn$ hay không. Nếu tồn tại số chẵn như vậy, ta không thể xây dựng $\textit{nums2}$ hợp lệ, nên trả về $\text{false}$; ngược lại, trả về $\text{true}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums1}$. Độ phức tạp không gian là $O(1)$.

Bài toán tương tự:

- [3875. Construct Uniform Parity Array I](https://github.com/doocs/leetcode/blob/main/solution/3800-3899/3875.Construct%20Uniform%20Parity%20Array%20I/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def uniformArray(self, nums1: List[int]) -> bool:
        mn = min((x for x in nums1 if x & 1), default=None)
        return mn is None or all(x >= mn for x in nums1 if x & 1 == 0)
```

#### Java

```java
class Solution {
    public boolean uniformArray(int[] nums1) {
        final int inf = Integer.MAX_VALUE;
        int mn = inf;
        for (int x : nums1) {
            if (x % 2 == 1) {
                mn = Math.min(mn, x);
            }
        }
        for (int x : nums1) {
            if (x % 2 == 0 && mn != inf && x < mn) {
                return false;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool uniformArray(vector<int>& nums1) {
        int mn = INT_MAX;
        for (int x : nums1) {
            if (x % 2 == 1) {
                mn = min(mn, x);
            }
        }
        for (int x : nums1) {
            if (x % 2 == 0 && mn != INT_MAX && x < mn) {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func uniformArray(nums1 []int) bool {
	mn := int(^uint(0) >> 1)
	for _, x := range nums1 {
		if x%2 == 1 && x < mn {
			mn = x
		}
	}
	for _, x := range nums1 {
		if x%2 == 0 && mn != int(^uint(0)>>1) && x < mn {
			return false
		}
	}
	return true
}
```

#### TypeScript

```ts
function uniformArray(nums1: number[]): boolean {
    let mn = Number.MAX_SAFE_INTEGER;
    for (const x of nums1) {
        if (x % 2 === 1) {
            mn = Math.min(mn, x);
        }
    }
    for (const x of nums1) {
        if (x % 2 === 0 && mn !== Number.MAX_SAFE_INTEGER && x < mn) {
            return false;
        }
    }
    return true;
}
```

#### Rust

```rust
impl Solution {
    pub fn uniform_array(nums1: Vec<i32>) -> bool {
        match nums1.iter().filter(|&&x| x % 2 == 1).min() {
            None => true,
            Some(&mn) => !nums1.iter().any(|&x| x % 2 == 0 && x < mn),
        }
    }
}
```

#### C#

```cs
public class Solution {
    public bool UniformArray(int[] nums1) {
        int mn = int.MaxValue;
        foreach (int x in nums1) {
            if (x % 2 == 1) {
                mn = Math.Min(mn, x);
            }
        }
        if (mn == int.MaxValue) {
            return true;
        }
        foreach (int x in nums1) {
            if (x % 2 == 0 && x < mn) {
                return false;
            }
        }
        return true;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
