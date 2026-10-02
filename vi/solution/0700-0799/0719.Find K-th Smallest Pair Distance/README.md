---
comments: true
difficulty: Hard
tags:
    - Array
    - Two Pointers
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [719. Find K-th Smallest Pair Distance](https://leetcode.com/problems/find-k-th-smallest-pair-distance)

[中文文档](/solution/0700-0799/0719.Find%20K-th%20Smallest%20Pair%20Distance/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Khoảng cách của một cặp</strong> số nguyên <code>a</code> và <code>b</code> được định nghĩa là độ chênh lệch tuyệt đối giữa <code>a</code> và <code>b</code>.</p>

<p>Cho mảng số nguyên <code>nums</code> và số nguyên <code>k</code>, hãy trả về <em>khoảng cách nhỏ thứ </em><code>k<sup>th</sup></code> <em>trong tất cả các cặp</em> <code>nums[i]</code> <em>và</em> <code>nums[j]</code> <em>thỏa mãn</em> <code>0 &lt;= i &lt; j &lt; nums.length</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,1], k = 1
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Các cặp là:
(1,3) -&gt; 2
(1,1) -&gt; 0
(3,1) -&gt; 2
Vậy cặp có khoảng cách nhỏ thứ 1 là (1,1), với khoảng cách bằng 0.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,1], k = 2
<strong>Đầu ra:</strong> 0
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,6,1], k = 3
<strong>Đầu ra:</strong> 5
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>2 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
	<li><code>1 &lt;= k &lt;= n * (n - 1) / 2</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Solution 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm khoảng cách cặp nhỏ thứ $k$. Vì $n\le 10^4$, không thể liệt kê mọi cặp trong $O(n^2)$. Khoảng cách nằm trong $[0,\max-\min]$ và số cặp có khoảng cách $\le d$ là hàm đơn điệu theo $d$.
>
> Binary search trên $d$ và đếm số cặp có khoảng cách $\le d$. Sau khi sắp xếp, với mỗi giá trị bên phải $b$, tìm vị trí đầu tiên có giá trị ít nhất $b-d$; lower bound giúp đếm các cặp trong $O(n\log n)$.
>
> Đáp án là $d$ nhỏ nhất có số cặp đếm được ít nhất bằng $k$. Ta tìm kiếm trong đoạn $[0, nums[-1]-nums[0]]$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def smallestDistancePair(self, nums: List[int], k: int) -> int:
        def count(dist):
            cnt = 0
            for i, b in enumerate(nums):
                a = b - dist
                j = bisect_left(nums, a, 0, i)
                cnt += i - j
            return cnt

        nums.sort()
        return bisect_left(range(nums[-1] - nums[0]), k, key=count)
```

#### Java

```java
class Solution {
    public int smallestDistancePair(int[] nums, int k) {
        Arrays.sort(nums);
        int left = 0, right = nums[nums.length - 1] - nums[0];
        while (left < right) {
            int mid = (left + right) >> 1;
            if (count(mid, nums) >= k) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }

    private int count(int dist, int[] nums) {
        int cnt = 0;
        for (int i = 0; i < nums.length; ++i) {
            int left = 0, right = i;
            while (left < right) {
                int mid = (left + right) >> 1;
                int target = nums[i] - dist;
                if (nums[mid] >= target) {
                    right = mid;
                } else {
                    left = mid + 1;
                }
            }
            cnt += i - left;
        }
        return cnt;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int smallestDistancePair(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end());
        int left = 0, right = nums.back() - nums.front();
        while (left < right) {
            int mid = (left + right) >> 1;
            if (count(mid, k, nums) >= k)
                right = mid;
            else
                left = mid + 1;
        }
        return left;
    }

    int count(int dist, int k, vector<int>& nums) {
        int cnt = 0;
        for (int i = 0; i < nums.size(); ++i) {
            int target = nums[i] - dist;
            int j = lower_bound(nums.begin(), nums.end(), target) - nums.begin();
            cnt += i - j;
        }
        return cnt;
    }
};
```

#### Go

```go
func smallestDistancePair(nums []int, k int) int {
	sort.Ints(nums)
	n := len(nums)
	left, right := 0, nums[n-1]-nums[0]
	count := func(dist int) int {
		cnt := 0
		for i, v := range nums {
			target := v - dist
			left, right := 0, i
			for left < right {
				mid := (left + right) >> 1
				if nums[mid] >= target {
					right = mid
				} else {
					left = mid + 1
				}
			}
			cnt += i - left
		}
		return cnt
	}
	for left < right {
		mid := (left + right) >> 1
		if count(mid) >= k {
			right = mid
		} else {
			left = mid + 1
		}
	}
	return left
}
```

#### TypeScript

```ts
function smallestDistancePair(nums: number[], k: number): number {
    nums.sort((a, b) => a - b);
    const n = nums.length;
    let left = 0,
        right = nums[n - 1] - nums[0];
    while (left < right) {
        let mid = (left + right) >> 1;
        let count = 0,
            i = 0;
        for (let j = 0; j < n; j++) {
            // 索引[i, j]距离nums[j]的距离<=mid
            while (nums[j] - nums[i] > mid) {
                i++;
            }
            count += j - i;
        }
        if (count >= k) {
            right = mid;
        } else {
            left = mid + 1;
        }
    }
    return left;
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number}
 */
function smallestDistancePair(nums, k) {
    nums.sort((a, b) => a - b);
    const n = nums.length;
    let left = 0,
        right = nums[n - 1] - nums[0];

    while (left < right) {
        const mid = (left + right) >> 1;
        let count = 0,
            i = 0;

        for (let j = 0; j < n; j++) {
            while (nums[j] - nums[i] > mid) {
                i++;
            }
            count += j - i;
        }

        if (count >= k) {
            right = mid;
        } else {
            left = mid + 1;
        }
    }

    return left;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
