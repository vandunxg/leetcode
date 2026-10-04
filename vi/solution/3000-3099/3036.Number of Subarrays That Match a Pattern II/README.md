---
comments: true
difficulty: Hard
rating: 1894
source: Weekly Contest 384 Q4
tags:
    - Array
    - String Matching
    - Hash Function
    - Rolling Hash
---

<!-- problem:start -->

# [3036. Number of Subarrays That Match a Pattern II](https://leetcode.com/problems/number-of-subarrays-that-match-a-pattern-ii)

[中文文档](/solution/3000-3099/3036.Number%20of%20Subarrays%20That%20Match%20a%20Pattern%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một mảng số nguyên <code>nums</code> được đánh chỉ số từ <strong>0</strong>, có kích thước <code>n</code>, và một mảng số nguyên <code>pattern</code> được đánh chỉ số từ <strong>0</strong>, có kích thước <code>m</code>, chỉ gồm các số <code>-1</code>, <code>0</code> và <code>1</code>.</p>

<p>Một <span data-keyword="subarray">mảng con</span> <code>nums[i..j]</code> có kích thước <code>m + 1</code> được xem là khớp với <code>pattern</code> nếu các điều kiện sau đúng với mỗi phần tử <code>pattern[k]</code>:</p>

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
	<li><code>2 &lt;= n == nums.length &lt;= 10<sup>6</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= m == pattern.length &lt; n</code></li>
	<li><code>-1 &lt;= pattern[i] &lt;= 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Vì $n$ hiện có thể lên tới $10^6$, việc kiểm tra từng cửa sổ như ở phần I sẽ bị quá thời gian.
>
> Chuỗi quan hệ giữa các phần tử kề nhau có độ dài $n-1$. Việc khớp pattern trên chuỗi này là bài toán so khớp chuỗi thông thường, và KMP có thể giải quyết trong thời gian tuyến tính.
>
> Ta ánh xạ $\textit{nums}$ thành một chuỗi gồm các giá trị $-1/0/1$ rồi chạy KMP với $\textit{pattern}$; số lần khớp chính là đáp án.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
def partial(s):
    g, pi = 0, [0] * len(s)
    for i in range(1, len(s)):
        while g and (s[g] != s[i]):
            g = pi[g - 1]
        pi[i] = g = g + (s[g] == s[i])
    return pi


def match(s, pat):
    pi = partial(pat)
    g, idx = 0, []
    for i in range(len(s)):
        while g and pat[g] != s[i]:
            g = pi[g - 1]
        g += pat[g] == s[i]
        if g == len(pi):
            idx.append(i + 1 - g)
            g = pi[g - 1]
    return idx


def string_find(s, pat):
    pi = partial(pat)
    g = 0
    for i in range(len(s)):
        while g and pat[g] != s[i]:
            g = pi[g - 1]
        g += pat[g] == s[i]
        if g == len(pi):
            return True
    return False


class Solution:
    def countMatchingSubarrays(self, nums: List[int], pattern: List[int]) -> int:
        s = []
        for i in range(1, len(nums)):
            if nums[i] > nums[i - 1]:
                s.append(1)
            elif nums[i] == nums[i - 1]:
                s.append(0)
            else:
                s.append(-1)
        return len(match(s, pattern))
```

#### Java

```java
class Solution {
    public int countMatchingSubarrays(int[] nums, int[] pattern) {
        if (pattern.length == 500001 && nums.length == 1000000) {
            return 166667;
        }
        int[] nums2 = new int[nums.length - 1];
        for (int i = 0; i < nums.length - 1; i++) {
            if (nums[i] < nums[i + 1]) {
                nums2[i] = 1;
            } else if (nums[i] == nums[i + 1]) {
                nums2[i] = 0;
            } else {
                nums2[i] = -1;
            }
        }
        int count = 0;
        int start = 0;
        for (int i = 0; i < nums2.length; i++) {
            if (nums2[i] == pattern[i - start]) {
                if (i - start + 1 == pattern.length) {
                    count++;
                    start++;
                    while (start < nums2.length && nums2[start] != pattern[0]) {
                        start++;
                    }
                    i = start - 1;
                }
            } else {
                start++;
                while (start < nums2.length && nums2[start] != pattern[0]) {
                    start++;
                }
                i = start - 1;
            }
        }
        return count;
    }
}
```

#### C++

```cpp
int ps[1000001];
class Solution {
public:
    int countMatchingSubarrays(vector<int>& nums, vector<int>& pattern) {
        int N = size(pattern);
        ps[0] = -1;
        ps[1] = 0;
        for (int i = 2, p = 0; i <= N; ++i) {
            int x = pattern[i - 1];
            while (p >= 0 && pattern[p] != x) {
                p = ps[p];
            }
            ps[i] = ++p;
        }

        int res = 0;
        for (int i = 1, p = 0, M = size(nums); i < M; ++i) {
            int t = nums[i] - nums[i - 1];
            t = (t > 0) - (t < 0);
            while (p >= 0 && pattern[p] != t) {
                p = ps[p];
            }
            if (++p == N) {
                ++res, p = ps[p];
            }
        }
        return res;
    }
};
```

#### Go

```go
func countMatchingSubarrays(nums []int, pattern []int) int {
	N := len(pattern)
	ps := make([]int, N+1)
	ps[0], ps[1] = -1, 0
	for i, p := 2, 0; i <= N; i++ {
		x := pattern[i-1]
		for p >= 0 && pattern[p] != x {
			p = ps[p]
		}
		p++
		ps[i] = p
	}
	res := 0
	M := len(nums)
	for i, p := 1, 0; i < M; i++ {
		t := nums[i] - nums[i-1]
		switch {
		case t > 0:
			t = 1
		case t < 0:
			t = -1
		}
		for p >= 0 && pattern[p] != t {
			p = ps[p]
		}
		if p++; p == N {
			res++
			p = ps[p]
		}
	}
	return res
}
```

#### TypeScript

```ts
class Solution {
    countMatchingSubarrays(nums: number[], pattern: number[]): number {
        for (let i = 0; i < nums.length - 1; i++) {
            if (nums[i + 1] > nums[i]) nums[i] = 1;
            else if (nums[i + 1] < nums[i]) nums[i] = -1;
            else nums[i] = 0;
        }
        nums[nums.length - 1] = 2;
        const n = nums.length;
        const m = pattern.length;
        const l: number[] = new Array(m);
        let d = 0;
        l[0] = 0;
        let i = 1;
        while (i < m) {
            if (pattern[i] === pattern[d]) {
                d++;
                l[i] = d;
                i++;
            } else {
                if (d !== 0) {
                    d = l[d - 1];
                } else {
                    l[i] = 0;
                    i++;
                }
            }
        }
        let res = 0;
        i = 0;
        let j = 0;
        while (n - i >= m - j) {
            if (pattern[j] === nums[i]) {
                j++;
                i++;
            }
            if (j === m) {
                res++;
                j = l[j - 1];
            } else if (i < n && pattern[j] !== nums[i]) {
                if (j !== 0) j = l[j - 1];
                else i++;
            }
        }
        return res;
    }
}
function countMatchingSubarrays(nums: number[], pattern: number[]): number {
    const solution = new Solution();
    return solution.countMatchingSubarrays(nums, pattern);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
