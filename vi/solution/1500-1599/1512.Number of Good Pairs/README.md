---
comments: true
difficulty: Easy
rating: 1160
source: Weekly Contest 197 Q1
tags:
    - Array
    - Hash Table
    - Math
    - Counting
---

<!-- problem:start -->

# [1512. Number of Good Pairs](https://leetcode.com/problems/number-of-good-pairs)

[中文文档](/solution/1500-1599/1512.Number%20of%20Good%20Pairs/README.md)

## Mô tả

<!-- description:start -->

<p>Với một mảng số nguyên <code>nums</code>, hãy trả về <em>số lượng <strong>cặp tốt</strong></em>.</p>

<p>Một cặp <code>(i, j)</code> được gọi là <em>tốt</em> nếu <code>nums[i] == nums[j]</code> và <code>i</code> &lt; <code>j</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,2,3,1,1,3]
<strong>Output:</strong> 4
<strong>Explanation:</strong> Có 4 cặp tốt (0,3), (0,4), (3,4), (2,5), với chỉ số bắt đầu từ 0.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,1,1,1]
<strong>Output:</strong> 6
<strong>Explanation:</strong> Mọi cặp trong mảng đều <em>tốt</em>.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,2,3]
<strong>Output:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các cặp chỉ số thỏa $i<j$ và $nums[i]=nums[j]$. Hai vòng lặp vẫn đủ với $n\le 100$, nhưng sẽ quét lại các phần tử bằng nhau trước đó ở mỗi bước.
>
> Phần tử $x$ hiện tại tạo thành một cặp với mỗi $x$ trước đó. Một frequency map của các giá trị đã gặp cho phép cộng số lượng đó rồi tăng bộ đếm, vì vậy chỉ cần duyệt một lần.

<!-- thinking:end -->

Duyệt mảng, với mỗi phần tử $x$, đếm có bao nhiêu phần tử trước nó bằng $x$. Số lượng này chính là số cặp tốt được tạo bởi $x$ và các phần tử trước đó. Sau khi duyệt toàn bộ mảng, ta thu được đáp án.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(C)$. Ở đây, $n$ là độ dài mảng và $C$ là miền giá trị trong mảng. Trong bài này, $C = 101$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numIdenticalPairs(self, nums: List[int]) -> int:
        ans = 0
        cnt = Counter()
        for x in nums:
            ans += cnt[x]
            cnt[x] += 1
        return ans
```

#### Java

```java
class Solution {
    public int numIdenticalPairs(int[] nums) {
        int ans = 0;
        int[] cnt = new int[101];
        for (int x : nums) {
            ans += cnt[x]++;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numIdenticalPairs(vector<int>& nums) {
        int ans = 0;
        int cnt[101]{};
        for (int& x : nums) {
            ans += cnt[x]++;
        }
        return ans;
    }
};
```

#### Go

```go
func numIdenticalPairs(nums []int) (ans int) {
	cnt := [101]int{}
	for _, x := range nums {
		ans += cnt[x]
		cnt[x]++
	}
	return
}
```

#### TypeScript

```ts
function numIdenticalPairs(nums: number[]): number {
    const cnt: number[] = Array(101).fill(0);
    let ans = 0;
    for (const x of nums) {
        ans += cnt[x]++;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn num_identical_pairs(nums: Vec<i32>) -> i32 {
        let mut ans = 0;
        let mut cnt = [0; 101];
        for &x in nums.iter() {
            ans += cnt[x as usize];
            cnt[x as usize] += 1;
        }
        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var numIdenticalPairs = function (nums) {
    const cnt = Array(101).fill(0);
    let ans = 0;
    for (const x of nums) {
        ans += cnt[x]++;
    }
    return ans;
};
```

#### PHP

```php
class Solution {
    /**
     * @param Integer[] $nums
     * @return Integer
     */
    function numIdenticalPairs($nums) {
        $ans = 0;
        $cnt = array_fill(0, 101, 0);
        foreach ($nums as $x) {
            $ans += $cnt[$x]++;
        }
        return $ans;
    }
}
```

#### C

```c
int numIdenticalPairs(int* nums, int numsSize) {
    int cnt[101] = {0};
    int ans = 0;
    for (int i = 0; i < numsSize; i++) {
        ans += cnt[nums[i]]++;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
