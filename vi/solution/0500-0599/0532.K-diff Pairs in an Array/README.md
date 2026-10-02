---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - Two Pointers
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [532. K-diff Pairs in an Array](https://leetcode.com/problems/k-diff-pairs-in-an-array)

[中文文档](/solution/0500-0599/0532.K-diff%20Pairs%20in%20an%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> và số nguyên <code>k</code>, hãy trả về <em>số cặp k-diff <b>phân biệt</b> trong mảng</em>.</p>

<p>Cặp <strong>k-diff</strong> là cặp số nguyên <code>(nums[i], nums[j])</code> thỏa mãn:</p>

<ul>
	<li><code>0 &lt;= i, j &lt; nums.length</code></li>
	<li><code>i != j</code></li>
	<li><code>|nums[i] - nums[j]| == k</code></li>
</ul>

<p><strong>Lưu ý</strong> <code>|val|</code> là giá trị tuyệt đối của <code>val</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,1,4,1,5], k = 2
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Có hai cặp 2-diff trong mảng: (1, 3) và (3, 5).
Dù đầu vào có hai số 1, ta chỉ tính số cặp <strong>phân biệt</strong>.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4,5], k = 1
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Có bốn cặp 1-diff trong mảng: (1, 2), (2, 3), (3, 4) và (4, 5).
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,1,5,4], k = 0
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Có một cặp 0-diff trong mảng: (1, 1).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>4</sup></code></li>
	<li><code>-10<sup>7</sup> &lt;= nums[i] &lt;= 10<sup>7</sup></code></li>
	<li><code>0 &lt;= k &lt;= 10<sup>7</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash table

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các cặp giá trị phân biệt thỏa mãn $|a-b|=k$. Kiểm tra mọi cặp chỉ số sẽ tốn $O(n^2)$.
>
> Duyệt một lần và lưu các giá trị đã gặp: nếu $x-k$ hoặc $x+k$ đã xuất hiện, thêm đầu mút nhỏ hơn vào set kết quả để mỗi cặp chỉ được lưu một lần. Mỗi lần tra cứu có độ phức tạp kỳ vọng $O(1)$.

<!-- thinking:end -->

Vì $k$ là giá trị cố định, ta có thể dùng hash table $\textit{ans}$ để ghi nhận giá trị nhỏ hơn trong mỗi cặp; từ đó xác định được giá trị lớn hơn tương ứng. Cuối cùng, trả về kích thước của $\textit{ans}$.

Ta duyệt mảng $\textit{nums}$. Với số hiện tại $x$, dùng hash table $\textit{vis}$ để lưu các số đã duyệt. Nếu $x-k$ có trong $\textit{vis}$, ta thêm $x-k$ vào $\textit{ans}$. Nếu $x+k$ có trong $\textit{vis}$, ta thêm $x$ vào $\textit{ans}$. Sau đó, thêm $x$ vào $\textit{vis}$. Tiếp tục đến hết mảng $\textit{nums}$.

Cuối cùng, ta trả về kích thước của $\textit{ans}$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findPairs(self, nums: List[int], k: int) -> int:
        ans = set()
        vis = set()
        for x in nums:
            if x - k in vis:
                ans.add(x - k)
            if x + k in vis:
                ans.add(x)
            vis.add(x)
        return len(ans)
```

#### Java

```java
class Solution {
    public int findPairs(int[] nums, int k) {
        Set<Integer> ans = new HashSet<>();
        Set<Integer> vis = new HashSet<>();
        for (int x : nums) {
            if (vis.contains(x - k)) {
                ans.add(x - k);
            }
            if (vis.contains(x + k)) {
                ans.add(x);
            }
            vis.add(x);
        }
        return ans.size();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findPairs(vector<int>& nums, int k) {
        unordered_set<int> ans, vis;
        for (int x : nums) {
            if (vis.count(x - k)) {
                ans.insert(x - k);
            }
            if (vis.count(x + k)) {
                ans.insert(x);
            }
            vis.insert(x);
        }
        return ans.size();
    }
};
```

#### Go

```go
func findPairs(nums []int, k int) int {
	ans := make(map[int]struct{})
	vis := make(map[int]struct{})

	for _, x := range nums {
		if _, ok := vis[x-k]; ok {
			ans[x-k] = struct{}{}
		}
		if _, ok := vis[x+k]; ok {
			ans[x] = struct{}{}
		}
		vis[x] = struct{}{}
	}
	return len(ans)
}
```

#### TypeScript

```ts
function findPairs(nums: number[], k: number): number {
    const ans = new Set<number>();
    const vis = new Set<number>();
    for (const x of nums) {
        if (vis.has(x - k)) {
            ans.add(x - k);
        }
        if (vis.has(x + k)) {
            ans.add(x);
        }
        vis.add(x);
    }
    return ans.size;
}
```

#### Rust

```rust
use std::collections::HashSet;

impl Solution {
    pub fn find_pairs(nums: Vec<i32>, k: i32) -> i32 {
        let mut ans = HashSet::new();
        let mut vis = HashSet::new();

        for &x in &nums {
            if vis.contains(&(x - k)) {
                ans.insert(x - k);
            }
            if vis.contains(&(x + k)) {
                ans.insert(x);
            }
            vis.insert(x);
        }
        ans.len() as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
