---
comments: true
difficulty: Easy
rating: 1295
source: Weekly Contest 348 Q2
tags:
    - Array
    - Simulation
---

<!-- problem:start -->

# [2717. Semi-Ordered Permutation](https://leetcode.com/problems/semi-ordered-permutation)

[中文文档](/solution/2700-2799/2717.Semi-Ordered%20Permutation/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một hoán vị <strong>được đánh chỉ số từ 0</strong> gồm <code>n</code> số nguyên <code>nums</code>.</p>

<p>Một hoán vị được gọi là <strong>semi-ordered</strong> nếu số đầu tiên bằng <code>1</code> và số cuối cùng bằng <code>n</code>. Bạn có thể thực hiện thao tác dưới đây bao nhiêu lần tùy ý cho đến khi biến <code>nums</code> thành một hoán vị <strong>semi-ordered</strong>:</p>

<ul>
	<li>Chọn hai phần tử kề nhau trong <code>nums</code>, rồi đổi chỗ chúng.</li>
</ul>

<p>Trả về <em>số thao tác ít nhất để biến </em><code>nums</code><em> thành một <strong>hoán vị semi-ordered</strong></em>.</p>

<p>Một <strong>hoán vị</strong> là một dãy số nguyên từ <code>1</code> đến <code>n</code>, có độ dài <code>n</code> và chứa mỗi số đúng một lần.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,1,4,3]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ta có thể biến hoán vị thành semi-ordered bằng chuỗi thao tác sau:
1 - đổi chỗ i = 0 và j = 1. Hoán vị trở thành [1,2,4,3].
2 - đổi chỗ i = 2 và j = 3. Hoán vị trở thành [1,2,3,4].
Có thể chứng minh rằng không có chuỗi nào gồm ít hơn hai thao tác có thể biến nums thành một hoán vị semi-ordered.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,4,1,3]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ta có thể biến hoán vị thành semi-ordered bằng chuỗi thao tác sau:
1 - đổi chỗ i = 1 và j = 2. Hoán vị trở thành [2,1,4,3].
2 - đổi chỗ i = 0 và j = 1. Hoán vị trở thành [1,2,4,3].
3 - đổi chỗ i = 2 và j = 3. Hoán vị trở thành [1,2,3,4].
Có thể chứng minh rằng không có chuỗi nào gồm ít hơn ba thao tác có thể biến nums thành một hoán vị semi-ordered.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,4,2,5]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Hoán vị đã là một hoán vị semi-ordered.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length == n &lt;= 50</code></li>
	<li><code>1 &lt;= nums[i]&nbsp;&lt;= 50</code></li>
	<li><code>nums is a permutation.</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm vị trí của 1 và n

<!-- thinking:start -->

> **Tư duy**
>
> Các phép đổi chỗ kề nhau cần đưa $1$ về đầu và $n$ về cuối. Không cần sắp xếp toàn bộ; chỉ cần di chuyển hai phần tử này.
>
> $1$ cần $i$ lần đổi chỗ sang trái và $n$ cần $n-1-j$ lần đổi chỗ sang phải. Nếu $1$ bắt đầu ở bên phải $n$, hai đường đi sẽ dùng chung một lần đổi chỗ, nên trừ thêm một. Chỉ cần một lần duyệt để ghi nhận hai chỉ số.

<!-- thinking:end -->

Trước hết, ta tìm các chỉ số $i$ và $j$ của $1$ và $n$ tương ứng. Sau đó, dựa vào vị trí tương đối của $i$ và $j$, ta có thể xác định số lần đổi chỗ cần thiết.

Nếu $i < j$, số lần đổi chỗ cần thiết là $i + n - j - 1$. Nếu $i > j$, số lần đổi chỗ cần thiết là $i + n - j - 2$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def semiOrderedPermutation(self, nums: List[int]) -> int:
        n = len(nums)
        i = nums.index(1)
        j = nums.index(n)
        k = 1 if i < j else 2
        return i + n - j - k
```

#### Java

```java
class Solution {
    public int semiOrderedPermutation(int[] nums) {
        int n = nums.length;
        int i = 0, j = 0;
        for (int k = 0; k < n; ++k) {
            if (nums[k] == 1) {
                i = k;
            }
            if (nums[k] == n) {
                j = k;
            }
        }
        int k = i < j ? 1 : 2;
        return i + n - j - k;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int semiOrderedPermutation(vector<int>& nums) {
        int n = nums.size();
        int i = find(nums.begin(), nums.end(), 1) - nums.begin();
        int j = find(nums.begin(), nums.end(), n) - nums.begin();
        int k = i < j ? 1 : 2;
        return i + n - j - k;
    }
};
```

#### Go

```go
func semiOrderedPermutation(nums []int) int {
	n := len(nums)
	var i, j int
	for k, x := range nums {
		if x == 1 {
			i = k
		}
		if x == n {
			j = k
		}
	}
	k := 1
	if i > j {
		k = 2
	}
	return i + n - j - k
}
```

#### TypeScript

```ts
function semiOrderedPermutation(nums: number[]): number {
    const n = nums.length;
    const i = nums.indexOf(1);
    const j = nums.indexOf(n);
    const k = i < j ? 1 : 2;
    return i + n - j - k;
}
```

#### Rust

```rust
impl Solution {
    pub fn semi_ordered_permutation(nums: Vec<i32>) -> i32 {
        let n = nums.len();
        let (mut i, mut j) = (0, 0);

        for k in 0..n {
            if nums[k] == 1 {
                i = k;
            }
            if nums[k] == (n as i32) {
                j = k;
            }
        }

        let k = if i < j { 1 } else { 2 };
        (i + n - j - k) as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
