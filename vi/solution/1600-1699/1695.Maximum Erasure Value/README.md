---
comments: true
difficulty: Medium
rating: 1528
source: Weekly Contest 220 Q2
tags:
    - Array
    - Hash Table
    - Sliding Window
---

<!-- problem:start -->

# [1695. Maximum Erasure Value](https://leetcode.com/problems/maximum-erasure-value)

[中文文档](/solution/1600-1699/1695.Maximum%20Erasure%20Value/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên dương <code>nums</code> và muốn xóa một mảng con chứa các <strong>phần tử khác nhau</strong>. <strong>Điểm số</strong> nhận được khi xóa mảng con bằng <strong>tổng</strong> các phần tử của nó.</p>

<p>Trả về <em><strong>điểm số lớn nhất</strong> có thể nhận được khi xóa <strong>chính xác một</strong> mảng con.</em></p>

<p>Mảng <code>b</code> được gọi là <span class="tex-font-style-it">mảng con</span> của <code>a</code> nếu nó là một dãy con liên tiếp của <code>a</code>, tức là bằng <code>a[l],a[l+1],...,a[r]</code> với một cặp <code>(l,r)</code> nào đó.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,2,4,5,6]
<strong>Đầu ra:</strong> 17
<strong>Giải thích:</strong> Mảng con tối ưu ở đây là [2,4,5,6].
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,2,1,2,5,2,1,2,5]
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong> Mảng con tối ưu ở đây là [5,2,1] hoặc [1,2,5].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mảng hoặc hash table + tổng tiền tố

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm tổng lớn nhất của một mảng con liên tiếp gồm các giá trị khác nhau. $n$ là $10^5$; chỉ số xuất hiện gần nhất đẩy đầu trái $j$ qua các phần tử trùng lặp, còn tổng tiền tố cho biết tổng trên đoạn.
>
> Mảng $d$ lưu chỉ số xuất hiện gần nhất của mỗi giá trị. Với $v$, đặt $j=\max(j,d[v])$ rồi cập nhật bằng $s[i]-s[j]$.

<!-- thinking:end -->

Ta sử dụng một mảng hoặc hash table $\text{d}$ để lưu vị trí xuất hiện gần nhất của mỗi số, và dùng mảng tổng tiền tố $\text{s}$ để lưu tổng từ vị trí bắt đầu đến vị trí hiện tại. Ta sử dụng biến $j$ để lưu đầu trái của mảng con hiện tại không có phần tử lặp.

Ta duyệt qua mảng. Với mỗi số $v$, nếu $\text{d}[v]$ tồn tại, ta cập nhật $j$ thành $\max(j, \text{d}[v])$, bảo đảm mảng con hiện tại không chứa $v$ lặp lại. Sau đó ta cập nhật đáp án thành $\max(\text{ans}, \text{s}[i] - \text{s}[j])$, rồi cập nhật $\text{d}[v]$ thành $i$.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\text{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumUniqueSubarray(self, nums: List[int]) -> int:
        d = [0] * (max(nums) + 1)
        s = list(accumulate(nums, initial=0))
        ans = j = 0
        for i, v in enumerate(nums, 1):
            j = max(j, d[v])
            ans = max(ans, s[i] - s[j])
            d[v] = i
        return ans
```

#### Java

```java
class Solution {
    public int maximumUniqueSubarray(int[] nums) {
        int[] d = new int[10001];
        int n = nums.length;
        int[] s = new int[n + 1];
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + nums[i];
        }
        int ans = 0, j = 0;
        for (int i = 1; i <= n; ++i) {
            int v = nums[i - 1];
            j = Math.max(j, d[v]);
            ans = Math.max(ans, s[i] - s[j]);
            d[v] = i;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumUniqueSubarray(vector<int>& nums) {
        int d[10001]{};
        int n = nums.size();
        int s[n + 1];
        s[0] = 0;
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + nums[i];
        }
        int ans = 0, j = 0;
        for (int i = 1; i <= n; ++i) {
            int v = nums[i - 1];
            j = max(j, d[v]);
            ans = max(ans, s[i] - s[j]);
            d[v] = i;
        }
        return ans;
    }
};
```

#### Go

```go
func maximumUniqueSubarray(nums []int) (ans int) {
	d := [10001]int{}
	n := len(nums)
	s := make([]int, n+1)
	for i, v := range nums {
		s[i+1] = s[i] + v
	}
	for i, j := 1, 0; i <= n; i++ {
		v := nums[i-1]
		j = max(j, d[v])
		ans = max(ans, s[i]-s[j])
		d[v] = i
	}
	return
}
```

#### TypeScript

```ts
function maximumUniqueSubarray(nums: number[]): number {
    const m = Math.max(...nums);
    const n = nums.length;
    const s: number[] = Array.from({ length: n + 1 }, () => 0);
    for (let i = 1; i <= n; ++i) {
        s[i] = s[i - 1] + nums[i - 1];
    }
    const d = Array.from({ length: m + 1 }, () => 0);
    let [ans, j] = [0, 0];
    for (let i = 1; i <= n; ++i) {
        j = Math.max(j, d[nums[i - 1]]);
        ans = Math.max(ans, s[i] - s[j]);
        d[nums[i - 1]] = i;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn maximum_unique_subarray(nums: Vec<i32>) -> i32 {
        let m = *nums.iter().max().unwrap() as usize;
        let mut d = vec![0; m + 1];
        let n = nums.len();

        let mut s = vec![0; n + 1];
        for i in 0..n {
            s[i + 1] = s[i] + nums[i];
        }

        let mut ans = 0;
        let mut j = 0;
        for (i, &v) in nums.iter().enumerate().map(|(i, v)| (i + 1, v)) {
            j = j.max(d[v as usize]);
            ans = ans.max(s[i] - s[j]);
            d[v as usize] = i;
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Hai con trỏ (sliding window)

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 sử dụng tổng tiền tố và bảng chỉ số. Sliding window có thể lưu trực tiếp tổng của các phần tử khác nhau: thu hẹp từ bên trái khi gặp phần tử trùng lặp và không cần mảng tổng tiền tố.

<!-- thinking:end -->

Bài toán thực chất yêu cầu tìm mảng con dài nhất trong đó mọi phần tử đều khác nhau. Ta có thể sử dụng hai con trỏ $i$ và $j$ trỏ đến hai đầu trái và phải của mảng con, ban đầu $i = 0$ và $j = 0$. Ngoài ra, ta dùng hash table $\text{vis}$ để lưu các phần tử trong mảng con.

Ta duyệt qua mảng. Với mỗi số $x$, nếu $x$ có trong $\text{vis}$, ta liên tục xóa $\text{nums}[i]$ khỏi $\text{vis}$ cho đến khi $x$ không còn trong $\text{vis}$. Nhờ đó, ta tìm được mảng con không chứa phần tử trùng lặp. Ta thêm $x$ vào $\text{vis}$, cập nhật tổng mảng con $s$, sau đó cập nhật đáp án $\text{ans} = \max(\text{ans}, s)$.

Sau khi duyệt xong, ta thu được tổng lớn nhất của mảng con.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\text{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumUniqueSubarray(self, nums: List[int]) -> int:
        vis = set()
        ans = s = i = 0
        for x in nums:
            while x in vis:
                y = nums[i]
                s -= y
                vis.remove(y)
                i += 1
            vis.add(x)
            s += x
            ans = max(ans, s)
        return ans
```

#### Java

```java
class Solution {
    public int maximumUniqueSubarray(int[] nums) {
        Set<Integer> vis = new HashSet<>();
        int ans = 0, s = 0, i = 0;
        for (int x : nums) {
            while (vis.contains(x)) {
                s -= nums[i];
                vis.remove(nums[i++]);
            }
            vis.add(x);
            s += x;
            ans = Math.max(ans, s);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumUniqueSubarray(vector<int>& nums) {
        unordered_set<int> vis;
        int ans = 0, s = 0, i = 0;
        for (int x : nums) {
            while (vis.contains(x)) {
                s -= nums[i];
                vis.erase(nums[i++]);
            }
            vis.insert(x);
            s += x;
            ans = max(ans, s);
        }
        return ans;
    }
};
```

#### Go

```go
func maximumUniqueSubarray(nums []int) (ans int) {
	vis := map[int]bool{}
	var s, i int
	for _, x := range nums {
		for vis[x] {
			s -= nums[i]
			vis[nums[i]] = false
			i++
		}
		vis[x] = true
		s += x
		ans = max(ans, s)
	}
	return
}
```

#### TypeScript

```ts
function maximumUniqueSubarray(nums: number[]): number {
    const vis: Set<number> = new Set();
    let [ans, s, i] = [0, 0, 0];
    for (const x of nums) {
        while (vis.has(x)) {
            s -= nums[i];
            vis.delete(nums[i++]);
        }
        vis.add(x);
        s += x;
        ans = Math.max(ans, s);
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashSet;

impl Solution {
    pub fn maximum_unique_subarray(nums: Vec<i32>) -> i32 {
        let mut vis = HashSet::new();
        let (mut ans, mut s, mut i) = (0, 0, 0);

        for &x in &nums {
            while vis.contains(&x) {
                let y = nums[i];
                s -= y;
                vis.remove(&y);
                i += 1;
            }
            vis.insert(x);
            s += x;
            ans = ans.max(s);
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
