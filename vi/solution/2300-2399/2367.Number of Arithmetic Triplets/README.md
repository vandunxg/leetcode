---
comments: true
difficulty: Easy
rating: 1203
source: Weekly Contest 305 Q1
tags:
    - Array
    - Hash Table
    - Two Pointers
    - Enumeration
---

<!-- problem:start -->

# [2367. Number of Arithmetic Triplets](https://leetcode.com/problems/number-of-arithmetic-triplets)

[中文文档](/solution/2300-2399/2367.Number%20of%20Arithmetic%20Triplets/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> <strong>tăng nghiêm ngặt</strong> và được đánh chỉ số từ <strong>0</strong>, cùng một số nguyên dương <code>diff</code>. Bộ ba <code>(i, j, k)</code> được gọi là một <strong>cấp số cộng</strong> nếu thỏa mãn các điều kiện sau:</p>

<ul>
	<li><code>i &lt; j &lt; k</code>,</li>
	<li><code>nums[j] - nums[i] == diff</code>, và</li>
	<li><code>nums[k] - nums[j] == diff</code>.</li>
</ul>

<p>Trả về <em>số lượng <strong>bộ ba cấp số cộng</strong> phân biệt.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,1,4,6,7,10], diff = 3
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
(1, 2, 4) là một bộ ba cấp số cộng vì cả 7 - 4 == 3 và 4 - 1 == 3 đều đúng.
(2, 4, 5) là một bộ ba cấp số cộng vì cả 10 - 7 == 3 và 7 - 4 == 3 đều đúng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,5,6,7,8,9], diff = 2
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
(0, 2, 4) là một bộ ba cấp số cộng vì cả 8 - 6 == 2 và 6 - 4 == 2 đều đúng.
(1, 3, 5) là một bộ ba cấp số cộng vì cả 9 - 7 == 2 và 7 - 5 == 2 đều đúng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= nums.length &lt;= 200</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 200</code></li>
	<li><code>1 &lt;= diff &lt;= 50</code></li>
	<li><code>nums</code> tăng <strong>nghiêm ngặt</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Vét cạn

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các bộ ba có công sai $diff$ trong một mảng tăng nghiêm ngặt. Vì $n \le 200$, ta có thể dùng ba vòng lặp lồng nhau.
>
> Liệt kê ba chỉ số và kiểm tra cả hai khoảng cách có bằng $diff$ hay không.

<!-- thinking:end -->

Ta nhận thấy độ dài mảng $nums$ không vượt quá $200$. Vì vậy, ta có thể trực tiếp liệt kê $i$, $j$, $k$ và kiểm tra xem chúng có thỏa mãn các điều kiện hay không. Nếu có, ta tăng số lượng bộ ba lên một.

Độ phức tạp thời gian là $O(n^3)$, trong đó $n$ là độ dài mảng $nums$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def arithmeticTriplets(self, nums: List[int], diff: int) -> int:
        return sum(b - a == diff and c - b == diff for a, b, c in combinations(nums, 3))
```

#### Java

```java
class Solution {
    public int arithmeticTriplets(int[] nums, int diff) {
        int ans = 0;
        int n = nums.length;
        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                for (int k = j + 1; k < n; ++k) {
                    if (nums[j] - nums[i] == diff && nums[k] - nums[j] == diff) {
                        ++ans;
                    }
                }
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int arithmeticTriplets(vector<int>& nums, int diff) {
        int ans = 0;
        int n = nums.size();
        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                for (int k = j + 1; k < n; ++k) {
                    if (nums[j] - nums[i] == diff && nums[k] - nums[j] == diff) {
                        ++ans;
                    }
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func arithmeticTriplets(nums []int, diff int) (ans int) {
	n := len(nums)
	for i := 0; i < n; i++ {
		for j := i + 1; j < n; j++ {
			for k := j + 1; k < n; k++ {
				if nums[j]-nums[i] == diff && nums[k]-nums[j] == diff {
					ans++
				}
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function arithmeticTriplets(nums: number[], diff: number): number {
    const n = nums.length;
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        for (let j = i + 1; j < n; ++j) {
            for (let k = j + 1; k < n; ++k) {
                if (nums[j] - nums[i] === diff && nums[k] - nums[j] === diff) {
                    ++ans;
                }
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Mảng hoặc Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 có độ phức tạp bậc ba. Các giá trị và độ dài mảng đều nhỏ, nên một set cùng với việc kiểm tra $x+diff$ và $x+2diff$ cho phép giải quyết bài toán trong thời gian tuyến tính.

<!-- thinking:end -->

Trước tiên, ta lưu các phần tử của $nums$ vào một hash table hoặc mảng $vis$. Sau đó, với mỗi phần tử $x$ trong $nums$, ta kiểm tra xem $x+diff$ và $x+diff+diff$ có nằm trong $vis$ hay không. Nếu có, ta tăng số lượng bộ ba lên một.

Sau khi liệt kê xong, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def arithmeticTriplets(self, nums: List[int], diff: int) -> int:
        vis = set(nums)
        return sum(x + diff in vis and x + diff * 2 in vis for x in nums)
```

#### Java

```java
class Solution {
    public int arithmeticTriplets(int[] nums, int diff) {
        boolean[] vis = new boolean[301];
        for (int x : nums) {
            vis[x] = true;
        }
        int ans = 0;
        for (int x : nums) {
            if (vis[x + diff] && vis[x + diff + diff]) {
                ++ans;
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int arithmeticTriplets(vector<int>& nums, int diff) {
        bitset<301> vis;
        for (int x : nums) {
            vis[x] = 1;
        }
        int ans = 0;
        for (int x : nums) {
            ans += vis[x + diff] && vis[x + diff + diff];
        }
        return ans;
    }
};
```

#### Go

```go
func arithmeticTriplets(nums []int, diff int) (ans int) {
	vis := [301]bool{}
	for _, x := range nums {
		vis[x] = true
	}
	for _, x := range nums {
		if vis[x+diff] && vis[x+diff+diff] {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function arithmeticTriplets(nums: number[], diff: number): number {
    const vis: boolean[] = new Array(301).fill(false);
    for (const x of nums) {
        vis[x] = true;
    }
    let ans = 0;
    for (const x of nums) {
        if (vis[x + diff] && vis[x + diff + diff]) {
            ++ans;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
