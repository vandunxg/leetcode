---
comments: true
difficulty: Easy
rating: 1256
source: Weekly Contest 427 Q1
tags:
    - Array
    - Simulation
---

<!-- problem:start -->

# [3379. Transformed Array](https://leetcode.com/problems/transformed-array)

[中文文档](/solution/3300-3399/3379.Transformed%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> biểu diễn một mảng vòng. Nhiệm vụ của bạn là tạo một mảng mới <code>result</code> có <strong>cùng</strong> kích thước, theo các quy tắc sau:</p>
Với mỗi chỉ số <code>i</code> (trong đó <code>0 &lt;= i &lt; nums.length</code>), thực hiện các thao tác <strong>độc lập</strong> sau:

<ul>
	<li>Nếu <code>nums[i] &gt; 0</code>: Bắt đầu tại chỉ số <code>i</code> và di chuyển <code>nums[i]</code> bước về <strong>bên phải</strong> trong mảng vòng. Gán <code>result[i]</code> bằng giá trị tại chỉ số dừng lại.</li>
	<li>Nếu <code>nums[i] &lt; 0</code>: Bắt đầu tại chỉ số <code>i</code> và di chuyển <code>abs(nums[i])</code> bước về <strong>bên trái</strong> trong mảng vòng. Gán <code>result[i]</code> bằng giá trị tại chỉ số dừng lại.</li>
	<li>Nếu <code>nums[i] == 0</code>: Gán <code>result[i]</code> bằng <code>nums[i]</code>.</li>
</ul>

<p>Trả về mảng mới <code>result</code>.</p>

<p><strong>Lưu ý:</strong> Vì <code>nums</code> là mảng vòng, khi đi qua phần tử cuối cùng, ta sẽ quay lại đầu mảng; khi đi trước phần tử đầu tiên, ta sẽ quay lại cuối mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,-2,1,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,1,1,3]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Với <code>nums[0]</code> bằng 3, nếu di chuyển 3 bước sang phải, ta đến <code>nums[3]</code>. Vì vậy, <code>result[0]</code> phải là 1.</li>
	<li>Với <code>nums[1]</code> bằng -2, nếu di chuyển 2 bước sang trái, ta đến <code>nums[3]</code>. Vì vậy, <code>result[1]</code> phải là 1.</li>
	<li>Với <code>nums[2]</code> bằng 1, nếu di chuyển 1 bước sang phải, ta đến <code>nums[3]</code>. Vì vậy, <code>result[2]</code> phải là 1.</li>
	<li>Với <code>nums[3]</code> bằng 1, nếu di chuyển 1 bước sang phải, ta đến <code>nums[0]</code>. Vì vậy, <code>result[3]</code> phải là 3.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [-1,4,-1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[-1,-1,4]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Với <code>nums[0]</code> bằng -1, nếu di chuyển 1 bước sang trái, ta đến <code>nums[2]</code>. Vì vậy, <code>result[0]</code> phải là -1.</li>
	<li>Với <code>nums[1]</code> bằng 4, nếu di chuyển 4 bước sang phải, ta đến <code>nums[2]</code>. Vì vậy, <code>result[1]</code> phải là -1.</li>
	<li>Với <code>nums[2]</code> bằng -1, nếu di chuyển 1 bước sang trái, ta đến <code>nums[1]</code>. Vì vậy, <code>result[2]</code> phải là 4.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>-100 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Từ $i$, ta di chuyển $|nums[i]|$ bước theo hướng được xác định bởi dấu của $nums[i]$ trên một vòng tròn, rồi ghi lại giá trị tại vị trí dừng. Vì $n \le 100$, ta chỉ cần tính chỉ số đó.
>
> Với bước di chuyển âm, cần cẩn thận khi dùng modulo: $(i + x \bmod n + n) \bmod n$.
>
> Đáp án được xây dựng trong một mảng mới để các giá trị $nums[i]$ chưa được sử dụng không bị ghi đè.

<!-- thinking:end -->

Chúng ta tạo một mảng kết quả $\textit{ans}$. Với mỗi chỉ số, ta di chuyển sang phải hoặc sang trái $|nums[i]|$ bước tùy theo việc $nums[i]$ là số dương hay số âm, tính chỉ số dừng lại, rồi gán giá trị tại chỉ số đó cho $\textit{ans}[i]$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def constructTransformedArray(self, nums: List[int]) -> List[int]:
        n = len(nums)
        return [nums[(i + x % n + n) % n] for i, x in enumerate(nums)]
```

#### Java

```java
class Solution {
    public int[] constructTransformedArray(int[] nums) {
        int n = nums.length;
        int[] ans = new int[n];
        for (int i = 0; i < n; ++i) {
            ans[i] = nums[(i + nums[i] % n + n) % n];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> constructTransformedArray(vector<int>& nums) {
        int n = nums.size();
        vector<int> ans(n);
        for (int i = 0; i < n; ++i) {
            ans[i] = nums[(i + nums[i] % n + n) % n];
        }
        return ans;
    }
};
```

#### Go

```go
func constructTransformedArray(nums []int) []int {
	n := len(nums)
	ans := make([]int, n)
	for i, x := range nums {
		ans[i] = nums[(i+x%n+n)%n]
	}
	return ans
}
```

#### TypeScript

```ts
function constructTransformedArray(nums: number[]): number[] {
    const n = nums.length;
    const ans: number[] = [];
    for (let i = 0; i < n; ++i) {
        ans.push(nums[(i + (nums[i] % n) + n) % n]);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn construct_transformed_array(nums: Vec<i32>) -> Vec<i32> {
        let n = nums.len() as i32;
        let mut ans = vec![0; nums.len()];
        for (i, &x) in nums.iter().enumerate() {
            ans[i] = nums[(((i as i32 + x % n + n) % n) as usize)];
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
