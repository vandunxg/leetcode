---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - Counting
---

<!-- problem:start -->

# [3837. Delayed Count of Equal Elements 🔒](https://leetcode.com/problems/delayed-count-of-equal-elements)

[中文文档](/solution/3800-3899/3837.Delayed%20Count%20of%20Equal%20Elements/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code> và một số nguyên <code>k</code>.</p>

<p>Với mỗi chỉ số <code>i</code>, định nghĩa <strong>số đếm trễ</strong> là số lượng chỉ số <code>j</code> thỏa mãn:</p>

<ul>
	<li><code>i + k &lt; j &lt;= n - 1</code>, và</li>
	<li><code>nums[j] == nums[i]</code></li>
</ul>

<p>Trả về một mảng <code>ans</code> sao cho <code>ans[i]</code> là <strong>số đếm trễ</strong> của chỉ số <code>i</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,1,1], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,0,0,0]</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;"><code>i</code></th>
			<th style="border: 1px solid black;"><code>nums[i]</code></th>
			<th style="border: 1px solid black;">các <code>j</code> có thể chọn</th>
			<th style="border: 1px solid black;"><code>nums[j]</code></th>
			<th style="border: 1px solid black;">thỏa mãn<br />
			<code>nums[j] == nums[i]</code></th>
			<th style="border: 1px solid black;"><code>ans[i]</code></th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">[2, 3]</td>
			<td style="border: 1px solid black;">[1, 1]</td>
			<td style="border: 1px solid black;">[2, 3]</td>
			<td style="border: 1px solid black;">2</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">[3]</td>
			<td style="border: 1px solid black;">[1]</td>
			<td style="border: 1px solid black;">[]</td>
			<td style="border: 1px solid black;">0</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">[]</td>
			<td style="border: 1px solid black;">[]</td>
			<td style="border: 1px solid black;">[]</td>
			<td style="border: 1px solid black;">0</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">[]</td>
			<td style="border: 1px solid black;">[]</td>
			<td style="border: 1px solid black;">[]</td>
			<td style="border: 1px solid black;">0</td>
		</tr>
	</tbody>
</table>

<p>Do đó, <code>ans = [2, 0, 0, 0]</code>​​​​​​​.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,1,3,1], k = 0</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,1,0,0]</span></p>

<p><strong>Giải thích:</strong></p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;"><code>i</code></th>
			<th style="border: 1px solid black;"><code>nums[i]</code></th>
			<th style="border: 1px solid black;">các <code>j</code> có thể chọn</th>
			<th style="border: 1px solid black;"><code>nums[j]</code></th>
			<th style="border: 1px solid black;">thỏa mãn<br />
			<code>nums[j] == nums[i]</code></th>
			<th style="border: 1px solid black;"><code>ans[i]</code></th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">[1, 2, 3]</td>
			<td style="border: 1px solid black;">[1, 3, 1]</td>
			<td style="border: 1px solid black;">[2]</td>
			<td style="border: 1px solid black;">1</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">[2, 3]</td>
			<td style="border: 1px solid black;">[3, 1]</td>
			<td style="border: 1px solid black;">[3]</td>
			<td style="border: 1px solid black;">1</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">[3]</td>
			<td style="border: 1px solid black;">[1]</td>
			<td style="border: 1px solid black;">[]</td>
			<td style="border: 1px solid black;">0</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">[]</td>
			<td style="border: 1px solid black;">[]</td>
			<td style="border: 1px solid black;">[]</td>
			<td style="border: 1px solid black;">0</td>
		</tr>
	</tbody>
</table>

<p>Do đó, <code>ans = [1, 1, 0, 0]</code>​​​​​​​.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= k &lt;= n - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Duyệt ngược

<!-- thinking:start -->

> **Tư duy**
>
> $ans[i]$ đếm các chỉ số $j > i+k$ sao cho $nums[j]=nums[i]$. Vì $n \le 10^5$, không thể duyệt sang phải với từng $i$.
>
> Cửa sổ $(i+k,n-1]$ mở rộng khi $i$ giảm; chỉ số mới được thêm vào là $i+k+1$.
>
> Duyệt $i$ giảm dần từ $n-k-2$, thêm $nums[i+k+1]$ vào hash map, sau đó đọc số lần xuất hiện của $nums[i]$.
>
> Mỗi chỉ số chỉ được thêm một lần nên tổng thời gian là tuyến tính.

<!-- thinking:end -->

Ta có thể dùng một hash table $\textit{cnt}$ để ghi nhận số lần xuất hiện của mỗi số trong khoảng chỉ số $(i + k, n - 1]$. Ta duyệt chỉ số $i$ theo thứ tự ngược, bắt đầu từ chỉ số $n - k - 2$. Trong quá trình duyệt, trước tiên ta thêm số ở chỉ số $i + k + 1$ vào hash table $\textit{cnt}$, sau đó gán giá trị $\textit{cnt}[nums[i]]$ vào mảng kết quả $\textit{ans}[i]$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def delayedCount(self, nums: List[int], k: int) -> List[int]:
        n = len(nums)
        cnt = Counter()
        ans = [0] * n
        for i in range(n - k - 2, -1, -1):
            cnt[nums[i + k + 1]] += 1
            ans[i] = cnt[nums[i]]
        return ans
```

#### Java

```java
class Solution {
    public int[] delayedCount(int[] nums, int k) {
        int n = nums.length;
        var cnt = new HashMap<Integer, Integer>();
        int[] ans = new int[n];
        for (int i = n - k - 2; i >= 0; --i) {
            cnt.merge(nums[i + k + 1], 1, Integer::sum);
            ans[i] = cnt.getOrDefault(nums[i], 0);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> delayedCount(vector<int>& nums, int k) {
        int n = nums.size();
        unordered_map<int, int> cnt;
        vector<int> ans(n);
        for (int i = n - k - 2; i >= 0; --i) {
            ++cnt[nums[i + k + 1]];
            ans[i] = cnt[nums[i]];
        }
        return ans;
    }
};
```

#### Go

```go
func delayedCount(nums []int, k int) []int {
	n := len(nums)
	cnt := map[int]int{}
	ans := make([]int, n)
	for i := n - k - 2; i >= 0; i-- {
		cnt[nums[i+k+1]]++
		ans[i] = cnt[nums[i]]
	}
	return ans
}
```

#### TypeScript

```ts
function delayedCount(nums: number[], k: number): number[] {
    const n = nums.length;
    const cnt = new Map<number, number>();
    const ans = Array(n).fill(0);
    for (let i = n - k - 2; i >= 0; i--) {
        cnt.set(nums[i + k + 1], (cnt.get(nums[i + k + 1]) ?? 0) + 1);
        ans[i] = cnt.get(nums[i]) ?? 0;
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn delayed_count(nums: Vec<i32>, k: i32) -> Vec<i32> {
        let n = nums.len();
        let mut cnt: HashMap<i32, i32> = HashMap::new();
        let mut ans = vec![0; n];
        let k = k as usize;
        let mut i = n as i32 - k as i32 - 2;
        while i >= 0 {
            let idx = i as usize;
            *cnt.entry(nums[idx + k + 1]).or_insert(0) += 1;
            ans[idx] = *cnt.get(&nums[idx]).unwrap_or(&0);
            i -= 1;
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
