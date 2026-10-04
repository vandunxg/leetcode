---
comments: true
difficulty: Medium
rating: 1383
source: Weekly Contest 384 Q2
tags:
    - Array
    - String Matching
    - Hash Function
    - Rolling Hash
---

<!-- problem:start -->

# [3034. Number of Subarrays That Match a Pattern I](https://leetcode.com/problems/number-of-subarrays-that-match-a-pattern-i)

[中文文档](/solution/3000-3099/3034.Number%20of%20Subarrays%20That%20Match%20a%20Pattern%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> được đánh chỉ số từ <strong>0</strong> có kích thước <code>n</code>, và một mảng số nguyên <code>pattern</code> được đánh chỉ số từ <strong>0</strong> có kích thước <code>m</code>, chỉ gồm các số nguyên <code>-1</code>, <code>0</code> và <code>1</code>.</p>

<p>Một <span data-keyword="subarray">mảng con</span> <code>nums[i..j]</code> có kích thước <code>m + 1</code> được xem là khớp với <code>pattern</code> nếu với mỗi phần tử <code>pattern[k]</code>, các điều kiện sau được thỏa mãn:</p>

<ul>
	<li><code>nums[i + k + 1] &gt; nums[i + k]</code> nếu <code>pattern[k] == 1</code>.</li>
	<li><code>nums[i + k + 1] == nums[i + k]</code> nếu <code>pattern[k] == 0</code>.</li>
	<li><code>nums[i + k + 1] &lt; nums[i + k]</code> nếu <code>pattern[k] == -1</code>.</li>
</ul>

<p>Trả về <em><strong>số lượng</strong> mảng con trong</em> <code>nums</code> <em>khớp với</em> <code>pattern</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4,5,6], pattern = [1,1]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Pattern [1,1] cho biết ta cần tìm các mảng con tăng nghiêm ngặt có kích thước 3. Trong mảng nums, các mảng con [1,2,3], [2,3,4], [3,4,5] và [4,5,6] khớp với pattern này.
Do đó, có 4 mảng con trong nums khớp với pattern.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,4,4,1,3,5,5,3], pattern = [1,0,-1]
<strong>Đầu ra:</strong> 2
<strong>Giải thích: </strong>Pattern [1,0,-1] cho biết ta cần tìm một dãy trong đó số thứ nhất nhỏ hơn số thứ hai, số thứ hai bằng số thứ ba, và số thứ ba lớn hơn số thứ tư. Trong mảng nums, các mảng con [1,4,4,1] và [3,5,5,3] khớp với pattern này.
Do đó, có 2 mảng con trong nums khớp với pattern.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n == nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= m == pattern.length &lt; n</code></li>
	<li><code>-1 &lt;= pattern[i] &lt;= 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Pattern mã hóa quan hệ tăng/giảm/bằng giữa các phần tử kề nhau và $n \le 100$. Ta có thể kiểm tra trực tiếp mọi cửa sổ có độ dài $m+1$.
>
> Ánh xạ mỗi cặp phần tử kề nhau thành $-1,0,1$ giúp một cửa sổ khớp khi và chỉ khi dãy thu được bằng với pattern.
>
> Ta liệt kê các vị trí bắt đầu và kiểm tra từng vị trí trong $O(m)$.

<!-- thinking:end -->

Ta có thể liệt kê tất cả các mảng con của mảng `nums` có độ dài $m + 1$, sau đó kiểm tra xem chúng có khớp với mảng `pattern` hay không. Nếu khớp, ta tăng đáp án lên một.

Độ phức tạp thời gian là $O(n \times m)$, trong đó $n$ và $m$ lần lượt là độ dài của các mảng `nums` và `pattern`. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countMatchingSubarrays(self, nums: List[int], pattern: List[int]) -> int:
        def f(a: int, b: int) -> int:
            return 0 if a == b else (1 if a < b else -1)

        ans = 0
        for i in range(len(nums) - len(pattern)):
            ans += all(
                f(nums[i + k], nums[i + k + 1]) == p for k, p in enumerate(pattern)
            )
        return ans
```

#### Java

```java
class Solution {
    public int countMatchingSubarrays(int[] nums, int[] pattern) {
        int n = nums.length, m = pattern.length;
        int ans = 0;
        for (int i = 0; i < n - m; ++i) {
            int ok = 1;
            for (int k = 0; k < m && ok == 1; ++k) {
                if (f(nums[i + k], nums[i + k + 1]) != pattern[k]) {
                    ok = 0;
                }
            }
            ans += ok;
        }
        return ans;
    }

    private int f(int a, int b) {
        return a == b ? 0 : (a < b ? 1 : -1);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countMatchingSubarrays(vector<int>& nums, vector<int>& pattern) {
        int n = nums.size(), m = pattern.size();
        int ans = 0;
        auto f = [](int a, int b) {
            return a == b ? 0 : (a < b ? 1 : -1);
        };
        for (int i = 0; i < n - m; ++i) {
            int ok = 1;
            for (int k = 0; k < m && ok == 1; ++k) {
                if (f(nums[i + k], nums[i + k + 1]) != pattern[k]) {
                    ok = 0;
                }
            }
            ans += ok;
        }
        return ans;
    }
};
```

#### Go

```go
func countMatchingSubarrays(nums []int, pattern []int) (ans int) {
	f := func(a, b int) int {
		if a == b {
			return 0
		}
		if a < b {
			return 1
		}
		return -1
	}
	n, m := len(nums), len(pattern)
	for i := 0; i < n-m; i++ {
		ok := 1
		for k := 0; k < m && ok == 1; k++ {
			if f(nums[i+k], nums[i+k+1]) != pattern[k] {
				ok = 0
			}
		}
		ans += ok
	}
	return
}
```

#### TypeScript

```ts
function countMatchingSubarrays(nums: number[], pattern: number[]): number {
    const f = (a: number, b: number) => (a === b ? 0 : a < b ? 1 : -1);
    const n = nums.length;
    const m = pattern.length;
    let ans = 0;
    for (let i = 0; i < n - m; ++i) {
        let ok = 1;
        for (let k = 0; k < m && ok; ++k) {
            if (f(nums[i + k], nums[i + k + 1]) !== pattern[k]) {
                ok = 0;
            }
        }
        ans += ok;
    }
    return ans;
}
```

#### C#

```cs
public class Solution {
    public int CountMatchingSubarrays(int[] nums, int[] pattern) {
        int n = nums.Length, m = pattern.Length;
        int ans = 0;
        for (int i = 0; i < n - m; ++i) {
            int ok = 1;
            for (int k = 0; k < m && ok == 1; ++k) {
                if (f(nums[i + k], nums[i + k + 1]) != pattern[k]) {
                    ok = 0;
                }
            }
            ans += ok;
        }
        return ans;
    }

    private int f(int a, int b) {
        return a == b ? 0 : (a < b ? 1 : -1);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
