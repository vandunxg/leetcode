---
comments: true
difficulty: Medium
rating: 1495
source: Weekly Contest 312 Q2
tags:
    - Bit Manipulation
    - Brainteaser
    - Array
---

<!-- problem:start -->

# [2419. Longest Subarray With Maximum Bitwise AND](https://leetcode.com/problems/longest-subarray-with-maximum-bitwise-and)

[中文文档](/solution/2400-2499/2419.Longest%20Subarray%20With%20Maximum%20Bitwise%20AND/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có kích thước <code>n</code>.</p>

<p>Xét một subarray <strong>không rỗng</strong> của <code>nums</code> có phép <strong>AND bitwise</strong> <strong>lớn nhất</strong> có thể.</p>

<ul>
	<li>Nói cách khác, gọi <code>k</code> là giá trị lớn nhất của phép AND bitwise của <strong>bất kỳ</strong> subarray nào của <code>nums</code>. Khi đó, chỉ các subarray có phép AND bitwise bằng <code>k</code> mới được xét.</li>
</ul>

<p>Hãy trả về <em>độ dài của subarray như vậy <strong>dài nhất</strong></em>.</p>

<p>Phép AND bitwise của một mảng là phép AND bitwise của tất cả các số trong mảng.</p>

<p><strong>Subarray</strong> là một dãy phần tử liên tiếp trong một mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,3,2,2]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Giá trị AND bitwise lớn nhất có thể của một subarray là 3.
Subarray dài nhất có giá trị đó là [3,3], nên ta trả về 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>
Giá trị AND bitwise lớn nhất có thể của một subarray là 4.
Subarray dài nhất có giá trị đó là [4], nên ta trả về 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Câu đố tư duy

<!-- thinking:start -->

> **Tư duy**
>
> Phép AND bitwise không bao giờ làm tăng giá trị, nên AND lớn nhất của mọi subarray chính là giá trị lớn nhất toàn mảng $\textit{mx}$. AND của một subarray bằng $\textit{mx}$ khi và chỉ khi mọi phần tử đều bằng $\textit{mx}$.
>
> Bài toán trở thành tìm đoạn liên tiếp dài nhất gồm các phần tử $\textit{mx}$. Trước hết tìm giá trị lớn nhất, sau đó duyệt một lần để tìm chuỗi liên tiếp dài nhất.

<!-- thinking:end -->

Vì phép AND bitwise không làm tăng số, giá trị lớn nhất chính là giá trị lớn nhất trong mảng.

Có thể chuyển bài toán thành tìm số lượng lớn nhất các lần xuất hiện liên tiếp của giá trị lớn nhất trong mảng.

Trước tiên, duyệt mảng $\textit{nums}$ để tìm giá trị lớn nhất $\textit{mx}$, sau đó duyệt lại mảng để tìm số lượng lớn nhất các lần xuất hiện liên tiếp của giá trị lớn nhất. Cuối cùng, trả về số lượng này.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestSubarray(self, nums: List[int]) -> int:
        mx = max(nums)
        ans = cnt = 0
        for x in nums:
            if x == mx:
                cnt += 1
                ans = max(ans, cnt)
            else:
                cnt = 0
        return ans
```

#### Java

```java
class Solution {
    public int longestSubarray(int[] nums) {
        int mx = Arrays.stream(nums).max().getAsInt();
        int ans = 0, cnt = 0;
        for (int x : nums) {
            if (x == mx) {
                ans = Math.max(ans, ++cnt);
            } else {
                cnt = 0;
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
    int longestSubarray(vector<int>& nums) {
        int mx = ranges::max(nums);
        int ans = 0, cnt = 0;
        for (int x : nums) {
            if (x == mx) {
                ans = max(ans, ++cnt);
            } else {
                cnt = 0;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func longestSubarray(nums []int) (ans int) {
	mx := slices.Max(nums)
	cnt := 0
	for _, x := range nums {
		if x == mx {
			cnt++
			ans = max(ans, cnt)
		} else {
			cnt = 0
		}
	}
	return
}
```

#### TypeScript

```ts
function longestSubarray(nums: number[]): number {
    const mx = Math.max(...nums);
    let [ans, cnt] = [0, 0];
    for (const x of nums) {
        if (x === mx) {
            ans = Math.max(ans, ++cnt);
        } else {
            cnt = 0;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn longest_subarray(nums: Vec<i32>) -> i32 {
        let mx = *nums.iter().max().unwrap();
        let mut ans = 0;
        let mut cnt = 0;

        for &x in nums.iter() {
            if x == mx {
                cnt += 1;
                ans = ans.max(cnt);
            } else {
                cnt = 0;
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
var longestSubarray = function (nums) {
    const mx = Math.max(...nums);
    let [ans, cnt] = [0, 0];
    for (const x of nums) {
        if (x === mx) {
            ans = Math.max(ans, ++cnt);
        } else {
            cnt = 0;
        }
    }
    return ans;
};
```

#### C#

```cs
public class Solution {
    public int LongestSubarray(int[] nums) {
        int mx = nums.Max();
        int ans = 0, cnt = 0;
        foreach (int x in nums) {
            if (x == mx) {
                ans = Math.Max(ans, ++cnt);
            } else {
                cnt = 0;
            }
        }
        return ans;
    }
}
```

#### PHP

```php
class Solution {
    /**
     * @param Integer[] $nums
     * @return Integer
     */
    function longestSubarray($nums) {
        $mx = max($nums);
        $ans = 0;
        $cnt = 0;

        foreach ($nums as $x) {
            if ($x == $mx) {
                $ans = max($ans, ++$cnt);
            } else {
                $cnt = 0;
            }
        }

        return $ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
