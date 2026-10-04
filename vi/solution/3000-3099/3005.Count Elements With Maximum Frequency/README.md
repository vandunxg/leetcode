---
comments: true
difficulty: Easy
rating: 1216
source: Weekly Contest 380 Q1
tags:
    - Array
    - Hash Table
    - Counting
---

<!-- problem:start -->

# [3005. Count Elements With Maximum Frequency](https://leetcode.com/problems/count-elements-with-maximum-frequency)

[中文文档](/solution/3000-3099/3005.Count%20Elements%20With%20Maximum%20Frequency/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng <code>nums</code> gồm các số nguyên <strong>dương</strong>.</p>

<p>Trả về <em><strong>tổng số lần xuất hiện</strong> của các phần tử trong</em><em> </em><code>nums</code>&nbsp;<em>sao cho tất cả các phần tử đó đều có tần suất <strong>lớn nhất</strong></em>.</p>

<p><strong>Tần suất</strong> của một phần tử là số lần phần tử đó xuất hiện trong mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,2,2,3,1,4]
<strong>Output:</strong> 4
<strong>Giải thích:</strong> Các phần tử 1 và 2 có tần suất bằng 2, là tần suất lớn nhất trong mảng.
Vì vậy, số phần tử trong mảng có tần suất lớn nhất là 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,2,3,4,5]
<strong>Output:</strong> 5
<strong>Giải thích:</strong> Mọi phần tử trong mảng đều có tần suất bằng 1, là tần suất lớn nhất.
Vì vậy, số phần tử trong mảng có tần suất lớn nhất là 5.
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
> $n \le 100$, vì vậy chỉ cần đếm tần suất rồi tính tổng. Đại lượng cần tìm là tổng của các tần suất lớn nhất, không phải số lượng giá trị phân biệt đạt tần suất đó.
>
> Sau khi có các tần suất, ta lấy $\textit{mx}$ rồi cộng mọi tần suất bằng $\textit{mx}$.
>
> Chỉ cần một lượt đếm và một lượt duyệt qua các giá trị.

<!-- thinking:end -->

Ta có thể dùng hash table hoặc mảng $cnt$ để ghi lại số lần xuất hiện của mỗi phần tử.

Sau đó, ta duyệt qua $cnt$ để tìm phần tử xuất hiện nhiều nhất và gọi số lần xuất hiện đó là $mx$. Ta cộng số lần xuất hiện của các phần tử xuất hiện $mx$ lần, đó là đáp án.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxFrequencyElements(self, nums: List[int]) -> int:
        cnt = Counter(nums)
        mx = max(cnt.values())
        return sum(x for x in cnt.values() if x == mx)
```

#### Java

```java
class Solution {
    public int maxFrequencyElements(int[] nums) {
        int[] cnt = new int[101];
        for (int x : nums) {
            ++cnt[x];
        }
        int ans = 0, mx = -1;
        for (int x : cnt) {
            if (mx < x) {
                mx = x;
                ans = x;
            } else if (mx == x) {
                ans += x;
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
    int maxFrequencyElements(vector<int>& nums) {
        int cnt[101]{};
        for (int x : nums) {
            ++cnt[x];
        }
        int ans = 0, mx = -1;
        for (int x : cnt) {
            if (mx < x) {
                mx = x;
                ans = x;
            } else if (mx == x) {
                ans += x;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxFrequencyElements(nums []int) (ans int) {
	cnt := [101]int{}
	for _, x := range nums {
		cnt[x]++
	}
	mx := -1
	for _, x := range cnt {
		if mx < x {
			mx, ans = x, x
		} else if mx == x {
			ans += x
		}
	}
	return
}
```

#### TypeScript

```ts
function maxFrequencyElements(nums: number[]): number {
    const cnt: number[] = Array(101).fill(0);
    for (const x of nums) {
        ++cnt[x];
    }
    let [ans, mx] = [0, -1];
    for (const x of cnt) {
        if (mx < x) {
            mx = x;
            ans = x;
        } else if (mx === x) {
            ans += x;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_frequency_elements(nums: Vec<i32>) -> i32 {
        let mut cnt = [0; 101];
        for &x in &nums {
            cnt[x as usize] += 1;
        }
        let mut ans = 0;
        let mut mx = -1;
        for &x in &cnt {
            if mx < x {
                mx = x;
                ans = x;
            } else if mx == x {
                ans += x;
            }
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
var maxFrequencyElements = function (nums) {
    const cnt = new Array(101).fill(0);
    for (const x of nums) {
        ++cnt[x];
    }
    let [ans, mx] = [0, -1];
    for (const x of cnt) {
        if (mx < x) {
            mx = x;
            ans = x;
        } else if (mx === x) {
            ans += x;
        }
    }
    return ans;
};
```

#### C#

```cs
public class Solution {
    public int MaxFrequencyElements(int[] nums) {
        int[] cnt = new int[101];
        foreach (int x in nums) {
            ++cnt[x];
        }
        int ans = 0, mx = -1;
        foreach (int x in cnt) {
            if (mx < x) {
                mx = x;
                ans = x;
            } else if (mx == x) {
                ans += x;
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
