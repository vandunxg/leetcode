---
comments: true
difficulty: Medium
rating: 1345
source: Weekly Contest 218 Q2
tags:
    - Array
    - Hash Table
    - Two Pointers
    - Sorting
---

<!-- problem:start -->

# [1679. Max Number of K-Sum Pairs](https://leetcode.com/problems/max-number-of-k-sum-pairs)

[中文文档](/solution/1600-1699/1679.Max%20Number%20of%20K-Sum%20Pairs/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> và số nguyên <code>k</code>.</p>

<p>Trong một thao tác, bạn có thể chọn hai số trong mảng có tổng bằng <code>k</code> rồi xóa chúng khỏi mảng.</p>

<p>Hãy trả về <em>số thao tác lớn nhất có thể thực hiện trên mảng</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,2,3,4], k = 5
<strong>Output:</strong> 2
<strong>Giải thích:</strong> Bắt đầu với nums = [1,2,3,4]:
- Remove numbers 1 and 4, then nums = [2,3]
- Remove numbers 2 and 3, then nums = []
Không còn cặp nào có tổng bằng 5, nên tổng cộng có 2 thao tác.</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [3,1,3,4,3], k = 6
<strong>Output:</strong> 1
<strong>Giải thích:</strong> Bắt đầu với nums = [3,1,3,4,3]:
- Remove the first two 3&#39;s, then nums = [1,4,3]
Không còn cặp nào có tổng bằng 6, nên tổng cộng có 1 thao tác.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Các cặp phải có tổng bằng $k$ và mỗi giá trị chỉ được dùng một lần. Vì $n$ có thể bằng $10^5$, sau khi sắp xếp ta dùng hai con trỏ: dịch đầu phải khi tổng quá lớn và đầu trái khi tổng quá nhỏ.

<!-- thinking:end -->

Ta sắp xếp $nums$. Khi đó $l$ và $r$ lần lượt trỏ đến phần tử đầu và cuối của $nums$, rồi so sánh tổng $s$ của hai số với $k$.

- Nếu $s = k$, ta tìm được hai số có tổng bằng $k$. Tăng đáp án rồi đưa $l$ và $r$ tiến vào giữa;
- Nếu $s > k$, dịch con trỏ $r$ sang trái;
- Nếu $s < k$, dịch con trỏ $l$ sang phải;
- Tiếp tục cho đến khi $l \geq r$.

Sau khi vòng lặp kết thúc, trả về đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(\log n)$, trong đó $n$ là độ dài của $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxOperations(self, nums: List[int], k: int) -> int:
        nums.sort()
        l, r, ans = 0, len(nums) - 1, 0
        while l < r:
            s = nums[l] + nums[r]
            if s == k:
                ans += 1
                l, r = l + 1, r - 1
            elif s > k:
                r -= 1
            else:
                l += 1
        return ans
```

#### Java

```java
class Solution {
    public int maxOperations(int[] nums, int k) {
        Arrays.sort(nums);
        int l = 0, r = nums.length - 1;
        int ans = 0;
        while (l < r) {
            int s = nums[l] + nums[r];
            if (s == k) {
                ++ans;
                ++l;
                --r;
            } else if (s > k) {
                --r;
            } else {
                ++l;
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
    int maxOperations(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end());
        int cnt = 0;
        int i = 0, j = nums.size() - 1;
        while (i < j) {
            if (nums[i] + nums[j] == k) {
                i++;
                j--;
                cnt++;
            } else if (nums[i] + nums[j] > k) {
                j--;
            } else {
                i++;
            }
        }
        return cnt;
    }
};
```

#### Go

```go
func maxOperations(nums []int, k int) int {
	sort.Ints(nums)
	l, r, ans := 0, len(nums)-1, 0
	for l < r {
		s := nums[l] + nums[r]
		if s == k {
			ans++
			l++
			r--
		} else if s > k {
			r--
		} else {
			l++
		}
	}
	return ans
}
```

#### TypeScript

```ts
function maxOperations(nums: number[], k: number): number {
    const cnt = new Map();
    let ans = 0;
    for (const x of nums) {
        if (cnt.get(k - x)) {
            cnt.set(k - x, cnt.get(k - x) - 1);
            ++ans;
        } else {
            cnt.set(x, (cnt.get(x) | 0) + 1);
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_operations(nums: Vec<i32>, k: i32) -> i32 {
        let mut nums = nums.clone();
        nums.sort();
        let (mut l, mut r, mut ans) = (0, nums.len() - 1, 0);
        while l < r {
            match nums[l] + nums[r] {
                sum if sum == k => {
                    ans += 1;
                    l += 1;
                    r -= 1;
                }
                sum if sum > k => {
                    r -= 1;
                }
                _ => {
                    l += 1;
                }
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 cần sắp xếp. Frequency map của các giá trị chưa ghép sẽ ghép $x$ với $k-x$ nếu còn giá trị phù hợp; nếu không, lưu $x$, tất cả trong thời gian tuyến tính.

<!-- thinking:end -->

Ta dùng hash table $cnt$ để lưu các số nguyên còn lại và số lần xuất hiện của chúng.

Ta duyệt $nums$. Với số hiện tại $x$, kiểm tra $k - x$ có trong $cnt$ không. Nếu có, ta tìm được hai số có tổng bằng $k$, tăng đáp án và giảm số lần xuất hiện của $k - x$; nếu không, tăng số lần xuất hiện của $x$.

Sau khi duyệt xong, trả về đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của $nums$ và các cặp cần có tổng $k$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxOperations(self, nums: List[int], k: int) -> int:
        cnt = Counter()
        ans = 0
        for x in nums:
            if cnt[k - x]:
                ans += 1
                cnt[k - x] -= 1
            else:
                cnt[x] += 1
        return ans
```

#### Java

```java
class Solution {
    public int maxOperations(int[] nums, int k) {
        Map<Integer, Integer> cnt = new HashMap<>();
        int ans = 0;
        for (int x : nums) {
            if (cnt.containsKey(k - x)) {
                ++ans;
                if (cnt.merge(k - x, -1, Integer::sum) == 0) {
                    cnt.remove(k - x);
                }
            } else {
                cnt.merge(x, 1, Integer::sum);
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
    int maxOperations(vector<int>& nums, int k) {
        unordered_map<int, int> cnt;
        int ans = 0;
        for (int& x : nums) {
            if (cnt[k - x]) {
                --cnt[k - x];
                ++ans;
            } else {
                ++cnt[x];
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxOperations(nums []int, k int) (ans int) {
	cnt := map[int]int{}
	for _, x := range nums {
		if cnt[k-x] > 0 {
			cnt[k-x]--
			ans++
		} else {
			cnt[x]++
		}
	}
	return
}
```

#### Rust

```rust
impl Solution {
    pub fn max_operations(nums: Vec<i32>, k: i32) -> i32 {
        let mut cnt = std::collections::HashMap::new();
        let mut ans = 0;
        for x in nums {
            let m = k - x;
            if let Some(v) = cnt.get_mut(&m) {
                ans += 1;
                *v -= 1;
                if *v == 0 {
                    cnt.remove(&m);
                }
            } else if let Some(v) = cnt.get_mut(&x) {
                *v += 1;
            } else {
                cnt.insert(x, 1);
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
