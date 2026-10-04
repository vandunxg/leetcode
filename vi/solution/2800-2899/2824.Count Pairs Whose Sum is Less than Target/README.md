---
comments: true
difficulty: Easy
rating: 1165
source: Biweekly Contest 111 Q1
tags:
    - Array
    - Two Pointers
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [2824. Count Pairs Whose Sum is Less than Target](https://leetcode.com/problems/count-pairs-whose-sum-is-less-than-target)

[中文文档](/solution/2800-2899/2824.Count%20Pairs%20Whose%20Sum%20is%20Less%20than%20Target/README.md)

## Mô tả

<!-- description:start -->

Cho một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>nums</code> có độ dài <code>n</code> và một số nguyên <code>target</code>, hãy trả về <em>số lượng cặp</em> <code>(i, j)</code> <em>thỏa mãn</em> <code>0 &lt;= i &lt; j &lt; n</code> <em>và</em> <code>nums[i] + nums[j] &lt; target</code>.
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [-1,1,2,3,1], target = 2
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Có 3 cặp chỉ số thỏa mãn các điều kiện trong đề bài:
- (0, 1) vì 0 &lt; 1 và nums[0] + nums[1] = 0 &lt; target
- (0, 2) vì 0 &lt; 2 và nums[0] + nums[2] = 1 &lt; target
- (0, 4) vì 0 &lt; 4 và nums[0] + nums[4] = 0 &lt; target
Lưu ý rằng (0, 3) không được tính vì nums[0] + nums[3] không nhỏ hơn target một cách nghiêm ngặt.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [-6,2,5,-2,-7,-1,3], target = -2
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong> Có 10 cặp chỉ số thỏa mãn các điều kiện trong đề bài:
- (0, 1) vì 0 &lt; 1 và nums[0] + nums[1] = -4 &lt; target
- (0, 3) vì 0 &lt; 3 và nums[0] + nums[3] = -8 &lt; target
- (0, 4) vì 0 &lt; 4 và nums[0] + nums[4] = -13 &lt; target
- (0, 5) vì 0 &lt; 5 và nums[0] + nums[5] = -7 &lt; target
- (0, 6) vì 0 &lt; 6 và nums[0] + nums[6] = -3 &lt; target
- (1, 4) vì 1 &lt; 4 và nums[1] + nums[4] = -5 &lt; target
- (3, 4) vì 3 &lt; 4 và nums[3] + nums[4] = -9 &lt; target
- (3, 5) vì 3 &lt; 5 và nums[3] + nums[5] = -3 &lt; target
- (4, 5) vì 4 &lt; 5 và nums[4] + nums[5] = -8 &lt; target
- (4, 6) vì 4 &lt; 6 và nums[4] + nums[6] = -4 &lt; target
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length == n &lt;= 50</code></li>
	<li><code>-50 &lt;= nums[i], target &lt;= 50</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Với $n$ nhỏ, ta có thể dùng hai vòng lặp, nhưng sau khi sắp xếp, với mỗi chỉ số phải $j$, ta có thể dùng tìm kiếm nhị phân để tìm số lượng $i<j$ thỏa mãn $nums[i]+nums[j]<target$.

<!-- thinking:end -->

Trước tiên, ta sắp xếp mảng $nums$. Sau đó, với mỗi $j$, ta dùng tìm kiếm nhị phân trên đoạn $[0, j)$ để tìm chỉ số đầu tiên $i$ lớn hơn hoặc bằng $target - nums[j]$. Mọi chỉ số $k$ trong đoạn $[0, i)$ đều thỏa mãn điều kiện, nên ta tăng đáp án thêm $i$.

Sau khi duyệt xong, ta trả về đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$, độ phức tạp không gian là $O(\log n)$. Trong đó, $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countPairs(self, nums: List[int], target: int) -> int:
        nums.sort()
        ans = 0
        for j, x in enumerate(nums):
            i = bisect_left(nums, target - x, hi=j)
            ans += i
        return ans
```

#### Java

```java
class Solution {
    public int countPairs(List<Integer> nums, int target) {
        Collections.sort(nums);
        int ans = 0;
        for (int j = 0; j < nums.size(); ++j) {
            int x = nums.get(j);
            int i = search(nums, target - x, j);
            ans += i;
        }
        return ans;
    }

    private int search(List<Integer> nums, int x, int r) {
        int l = 0;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (nums.get(mid) >= x) {
                r = mid;
            } else {
                l = mid + 1;
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
    int countPairs(vector<int>& nums, int target) {
        sort(nums.begin(), nums.end());
        int ans = 0;
        for (int j = 0; j < nums.size(); ++j) {
            int i = lower_bound(nums.begin(), nums.begin() + j, target - nums[j]) - nums.begin();
            ans += i;
        }
        return ans;
    }
};
```

#### Go

```go
func countPairs(nums []int, target int) (ans int) {
	sort.Ints(nums)
	for j, x := range nums {
		i := sort.SearchInts(nums[:j], target-x)
		ans += i
	}
	return
}
```

#### TypeScript

```ts
function countPairs(nums: number[], target: number): number {
    nums.sort((a, b) => a - b);
    let ans = 0;
    const search = (x: number, r: number): number => {
        let l = 0;
        while (l < r) {
            const mid = (l + r) >> 1;
            if (nums[mid] >= x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    };
    for (let j = 0; j < nums.length; ++j) {
        const i = search(target - nums[j], j);
        ans += i;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
