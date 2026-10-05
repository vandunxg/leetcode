---
comments: true
difficulty: Medium
rating: 1397
source: Weekly Contest 507 Q2
tags:
    - Array
    - Hash Table
    - Enumeration
    - Prefix Sum
    - Sliding Window
---

<!-- problem:start -->

# [3969. Valid Subarrays With Matching Sum Digits I](https://leetcode.com/problems/valid-subarrays-with-matching-sum-digits-i)

[中文文档](/solution/3900-3999/3969.Valid%20Subarrays%20With%20Matching%20Sum%20Digits%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> và một chữ số nguyên <code>x</code>.</p>

<p>Một <span data-keyword="subarray-nonempty"><strong>mảng con</strong></span> <code>nums[l..r]</code> được gọi là <strong>hợp lệ</strong> nếu tổng các phần tử của nó thỏa mãn cả hai điều kiện sau:</p>

<ul>
	<li>Chữ số đầu tiên của tổng bằng <code>x</code>.</li>
	<li>Chữ số cuối cùng của tổng bằng <code>x</code>.</li>
</ul>

<p>Trả về số lượng mảng con hợp lệ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,100,1], x = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng con hợp lệ là:</p>

<ul>
	<li><code>nums[0..0]</code>: <code>sum = 1</code></li>
	<li><code>nums[0..1]</code>: <code>sum = 1 + 100 = 101</code></li>
	<li><code>nums[1..2]</code>: <code>sum = 100 + 1 = 101</code></li>
	<li><code>nums[2..2]</code>: <code>sum = 1</code></li>
</ul>

<p>Vì vậy, đáp án là 4.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1], x = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng con duy nhất là <code>nums[0..0]</code>, có tổng bằng 1 nên không thỏa mãn các điều kiện.</p>

<p>Vì vậy, đáp án là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1500</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= x &lt;= 9</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Enumeration

<!-- thinking:start -->

> **Tư duy**
>
> $n\le 1500$, nên có thể liệt kê các mảng con trong $O(n^2)$. Cố định đầu trái, cộng dồn $s$, rồi kiểm tra xem chữ số cuối và chữ số đầu của $s$ có đều bằng $x$ hay không.
>
> Chữ số cuối là $s\bmod 10$; chữ số đầu là ký tự đầu tiên của $\mathrm{str}(s)$. Đếm bằng hai vòng lặp.

<!-- thinking:end -->

Ta có thể liệt kê đầu trái $l$ của mảng con. Với mỗi $l$, ta liệt kê đầu phải $r$ trong đoạn $[l, n)$ và tính tổng của $nums[l..r]$. Nếu tổng thỏa mãn các điều kiện, tăng đáp án lên một.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(1)$, trong đó $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countValidSubarrays(self, nums: list[int], x: int) -> int:
        n = len(nums)
        ans = 0
        for l in range(n):
            s = 0
            for r in range(l, n):
                s += nums[r]
                if s % 10 == x and int(str(s)[0]) == x:
                    ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int countValidSubarrays(int[] nums, int x) {
        int n = nums.length;
        int ans = 0;

        for (int l = 0; l < n; l++) {
            long s = 0;
            for (int r = l; r < n; r++) {
                s += nums[r];
                if (s % 10 == x && Long.toString(s).charAt(0) - '0' == x) {
                    ans++;
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
    int countValidSubarrays(vector<int>& nums, int x) {
        int n = nums.size();
        int ans = 0;

        for (int l = 0; l < n; ++l) {
            long long s = 0;
            for (int r = l; r < n; ++r) {
                s += nums[r];
                if (s % 10 == x && to_string(s)[0] - '0' == x) {
                    ++ans;
                }
            }
        }

        return ans;
    }
};
```

#### Go

```go
func countValidSubarrays(nums []int, x int) (ans int) {
    n := len(nums)

	for l := 0; l < n; l++ {
		var s int64
		for r := l; r < n; r++ {
			s += int64(nums[r])
			if s%10 == int64(x) && int(strconv.FormatInt(s, 10)[0]-'0') == x {
				ans++
			}
		}
	}

	return
}
```

#### TypeScript

```ts
function countValidSubarrays(nums: number[], x: number): number {
    const n = nums.length;
    let ans = 0;

    for (let l = 0; l < n; l++) {
        let s = 0;

        for (let r = l; r < n; r++) {
            s += nums[r];

            if (s % 10 === x && Number(s.toString()[0]) === x) {
                ans++;
            }
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
