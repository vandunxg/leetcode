---
comments: true
difficulty: Hard
tags:
    - Array
    - Dynamic Programming
    - Prefix Sum
    - Sliding Window
---

<!-- problem:start -->

# [689. Maximum Sum of 3 Non-Overlapping Subarrays](https://leetcode.com/problems/maximum-sum-of-3-non-overlapping-subarrays)

[中文文档](/solution/0600-0699/0689.Maximum%20Sum%20of%203%20Non-Overlapping%20Subarrays/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> và số nguyên <code>k</code>, hãy tìm ba mảng con không chồng lấn, mỗi mảng dài <code>k</code>, có tổng lớn nhất và trả về chúng.</p>

<p>Trả về danh sách các chỉ số bắt đầu của từng đoạn (<strong>đánh chỉ số từ 0</strong>). Nếu có nhiều đáp án, hãy trả về dãy nhỏ nhất theo thứ tự từ điển.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,1,2,6,7,5,1], k = 2
<strong>Đầu ra:</strong> [0,3,5]
<strong>Giải thích:</strong> Các mảng con [1, 2], [2, 6], [7, 5] tương ứng với các chỉ số bắt đầu [0, 3, 5].
Cũng có thể chọn [2, 1], nhưng đáp án [1, 3, 5] lớn hơn theo thứ tự từ điển.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,1,2,1,2,1,2,1], k = 2
<strong>Đầu ra:</strong> [0,2,4]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;&nbsp;2<sup>16</sup></code></li>
	<li><code>1 &lt;= k &lt;= floor(nums.length / 3)</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sliding Window

<!-- thinking:start -->

> **Tư duy**
>
> Cần tìm ba mảng con độ dài $k$ không chồng lấn sao cho tổng lớn nhất, rồi tối ưu thứ tự từ điển của các chỉ số bắt đầu. Liệt kê cả ba vị trí có độ phức tạp bậc ba.
>
> Trượt đồng thời ba cửa sổ: lưu đoạn đầu tốt nhất và cặp hai đoạn tốt nhất, sau đó cộng với đoạn thứ ba hiện tại. Duyệt từ trái sang phải giúp thu được các chỉ số bắt đầu nhỏ nhất theo thứ tự từ điển.

<!-- thinking:end -->

Ta dùng sliding window để duyệt vị trí của mảng con thứ ba, đồng thời duy trì tổng lớn nhất cùng vị trí của hai mảng con không chồng lấn đầu tiên.

Độ phức tạp thời gian là $O(n)$, với $n$ là độ dài mảng $nums$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSumOfThreeSubarrays(self, nums: List[int], k: int) -> List[int]:
        s = s1 = s2 = s3 = 0
        mx1 = mx12 = 0
        idx1, idx12 = 0, ()
        ans = []
        for i in range(k * 2, len(nums)):
            s1 += nums[i - k * 2]
            s2 += nums[i - k]
            s3 += nums[i]
            if i >= k * 3 - 1:
                if s1 > mx1:
                    mx1 = s1
                    idx1 = i - k * 3 + 1
                if mx1 + s2 > mx12:
                    mx12 = mx1 + s2
                    idx12 = (idx1, i - k * 2 + 1)
                if mx12 + s3 > s:
                    s = mx12 + s3
                    ans = [*idx12, i - k + 1]
                s1 -= nums[i - k * 3 + 1]
                s2 -= nums[i - k * 2 + 1]
                s3 -= nums[i - k + 1]
        return ans
```

#### Java

```java
class Solution {
    public int[] maxSumOfThreeSubarrays(int[] nums, int k) {
        int[] ans = new int[3];
        int s = 0, s1 = 0, s2 = 0, s3 = 0;
        int mx1 = 0, mx12 = 0;
        int idx1 = 0, idx121 = 0, idx122 = 0;
        for (int i = k * 2; i < nums.length; ++i) {
            s1 += nums[i - k * 2];
            s2 += nums[i - k];
            s3 += nums[i];
            if (i >= k * 3 - 1) {
                if (s1 > mx1) {
                    mx1 = s1;
                    idx1 = i - k * 3 + 1;
                }
                if (mx1 + s2 > mx12) {
                    mx12 = mx1 + s2;
                    idx121 = idx1;
                    idx122 = i - k * 2 + 1;
                }
                if (mx12 + s3 > s) {
                    s = mx12 + s3;
                    ans = new int[] {idx121, idx122, i - k + 1};
                }
                s1 -= nums[i - k * 3 + 1];
                s2 -= nums[i - k * 2 + 1];
                s3 -= nums[i - k + 1];
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
    vector<int> maxSumOfThreeSubarrays(vector<int>& nums, int k) {
        vector<int> ans(3);
        int s = 0, s1 = 0, s2 = 0, s3 = 0;
        int mx1 = 0, mx12 = 0;
        int idx1 = 0, idx121 = 0, idx122 = 0;
        for (int i = k * 2; i < nums.size(); ++i) {
            s1 += nums[i - k * 2];
            s2 += nums[i - k];
            s3 += nums[i];
            if (i >= k * 3 - 1) {
                if (s1 > mx1) {
                    mx1 = s1;
                    idx1 = i - k * 3 + 1;
                }
                if (mx1 + s2 > mx12) {
                    mx12 = mx1 + s2;
                    idx121 = idx1;
                    idx122 = i - k * 2 + 1;
                }
                if (mx12 + s3 > s) {
                    s = mx12 + s3;
                    ans = {idx121, idx122, i - k + 1};
                }
                s1 -= nums[i - k * 3 + 1];
                s2 -= nums[i - k * 2 + 1];
                s3 -= nums[i - k + 1];
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxSumOfThreeSubarrays(nums []int, k int) []int {
	ans := make([]int, 3)
	s, s1, s2, s3 := 0, 0, 0, 0
	mx1, mx12 := 0, 0
	idx1, idx121, idx122 := 0, 0, 0
	for i := k * 2; i < len(nums); i++ {
		s1 += nums[i-k*2]
		s2 += nums[i-k]
		s3 += nums[i]
		if i >= k*3-1 {
			if s1 > mx1 {
				mx1 = s1
				idx1 = i - k*3 + 1
			}
			if mx1+s2 > mx12 {
				mx12 = mx1 + s2
				idx121 = idx1
				idx122 = i - k*2 + 1
			}
			if mx12+s3 > s {
				s = mx12 + s3
				ans = []int{idx121, idx122, i - k + 1}
			}
			s1 -= nums[i-k*3+1]
			s2 -= nums[i-k*2+1]
			s3 -= nums[i-k+1]
		}
	}
	return ans
}
```

#### TypeScript

```ts
function maxSumOfThreeSubarrays(nums: number[], k: number): number[] {
    const n: number = nums.length;
    const s: number[] = Array(n + 1).fill(0);

    for (let i = 0; i < n; ++i) {
        s[i + 1] = s[i] + nums[i];
    }

    const pre: number[][] = Array(n)
        .fill([])
        .map(() => new Array(2).fill(0));
    const suf: number[][] = Array(n)
        .fill([])
        .map(() => new Array(2).fill(0));

    for (let i = 0, t = 0, idx = 0; i < n - k + 1; ++i) {
        const cur: number = s[i + k] - s[i];
        if (cur > t) {
            pre[i + k - 1] = [cur, i];
            t = cur;
            idx = i;
        } else {
            pre[i + k - 1] = [t, idx];
        }
    }

    for (let i = n - k, t = 0, idx = 0; i >= 0; --i) {
        const cur: number = s[i + k] - s[i];
        if (cur >= t) {
            suf[i] = [cur, i];
            t = cur;
            idx = i;
        } else {
            suf[i] = [t, idx];
        }
    }

    let ans: number[] = [];
    for (let i = k, t = 0; i < n - 2 * k + 1; ++i) {
        const cur: number = s[i + k] - s[i] + pre[i - 1][0] + suf[i + k][0];
        if (cur > t) {
            ans = [pre[i - 1][1], i, suf[i + k][1]];
            t = cur;
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Tiền xử lý prefix và suffix + Duyệt mảng con ở giữa

<!-- thinking:start -->

> **Tư duy**
>
> Cách trượt đồng thời các cửa sổ gọn nhưng dễ sai. Dùng prefix sum cùng $pre[i]$ (cửa sổ độ dài $k$ tốt nhất bên trái) và $suf[i]$ (cửa sổ tốt nhất bên phải), ta chỉ cần duyệt vị trí bắt đầu của đoạn giữa.

<!-- thinking:end -->

Ta tiền xử lý mảng prefix sum $s$ của mảng $nums$, trong đó $s[i] = \sum_{j=0}^{i-1} nums[j]$. Khi đó, với mọi $i$, $j$, hiệu $s[j] - s[i]$ là tổng của mảng con $[i, j)$.

Tiếp theo, ta dùng quy hoạch động để duy trì hai mảng $pre$ và $suf$ có độ dài $n$. Trong đó, $pre[i]$ lưu tổng lớn nhất và vị trí bắt đầu của mảng con độ dài $k$ trong đoạn $[0, i]$; $suf[i]$ lưu tổng lớn nhất và vị trí bắt đầu của mảng con độ dài $k$ trong đoạn $[i, n)$.

Sau đó, ta duyệt vị trí bắt đầu $i$ của mảng con ở giữa. Tổng của ba mảng con là $pre[i-1][0] + suf[i+k][0] + (s[i+k] - s[i])$. Trong đó, $pre[i-1][0]$ là tổng lớn nhất của mảng con độ dài $k$ trong đoạn $[0, i-1]$, $suf[i+k][0]$ là tổng lớn nhất của mảng con độ dài $k$ trong đoạn $[i+k, n)$, còn $(s[i+k] - s[i])$ là tổng của mảng con độ dài $k$ trong đoạn $[i, i+k)$. Ta tìm vị trí bắt đầu của ba mảng con tạo nên tổng lớn nhất.

Độ phức tạp thời gian và không gian đều là $O(n)$. Ở đây, $n$ là độ dài mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSumOfThreeSubarrays(self, nums: List[int], k: int) -> List[int]:
        n = len(nums)
        s = list(accumulate(nums, initial=0))
        pre = [[] for _ in range(n)]
        suf = [[] for _ in range(n)]
        t = idx = 0
        for i in range(n - k + 1):
            if (cur := s[i + k] - s[i]) > t:
                pre[i + k - 1] = [cur, i]
                t, idx = pre[i + k - 1]
            else:
                pre[i + k - 1] = [t, idx]
        t = idx = 0
        for i in range(n - k, -1, -1):
            if (cur := s[i + k] - s[i]) >= t:
                suf[i] = [cur, i]
                t, idx = suf[i]
            else:
                suf[i] = [t, idx]
        t = 0
        ans = []
        for i in range(k, n - 2 * k + 1):
            if (cur := s[i + k] - s[i] + pre[i - 1][0] + suf[i + k][0]) > t:
                ans = [pre[i - 1][1], i, suf[i + k][1]]
                t = cur
        return ans
```

#### Java

```java
class Solution {
    public int[] maxSumOfThreeSubarrays(int[] nums, int k) {
        int n = nums.length;
        int[] s = new int[n + 1];
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + nums[i];
        }
        int[][] pre = new int[n][0];
        int[][] suf = new int[n][0];
        for (int i = 0, t = 0, idx = 0; i < n - k + 1; ++i) {
            int cur = s[i + k] - s[i];
            if (cur > t) {
                pre[i + k - 1] = new int[] {cur, i};
                t = cur;
                idx = i;
            } else {
                pre[i + k - 1] = new int[] {t, idx};
            }
        }
        for (int i = n - k, t = 0, idx = 0; i >= 0; --i) {
            int cur = s[i + k] - s[i];
            if (cur >= t) {
                suf[i] = new int[] {cur, i};
                t = cur;
                idx = i;
            } else {
                suf[i] = new int[] {t, idx};
            }
        }
        int[] ans = new int[0];
        for (int i = k, t = 0; i < n - 2 * k + 1; ++i) {
            int cur = s[i + k] - s[i] + pre[i - 1][0] + suf[i + k][0];
            if (cur > t) {
                ans = new int[] {pre[i - 1][1], i, suf[i + k][1]};
                t = cur;
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
    vector<int> maxSumOfThreeSubarrays(vector<int>& nums, int k) {
        int n = nums.size();
        vector<int> s(n + 1, 0);
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + nums[i];
        }

        vector<vector<int>> pre(n, vector<int>(2, 0));
        vector<vector<int>> suf(n, vector<int>(2, 0));

        for (int i = 0, t = 0, idx = 0; i < n - k + 1; ++i) {
            int cur = s[i + k] - s[i];
            if (cur > t) {
                pre[i + k - 1] = {cur, i};
                t = cur;
                idx = i;
            } else {
                pre[i + k - 1] = {t, idx};
            }
        }

        for (int i = n - k, t = 0, idx = 0; i >= 0; --i) {
            int cur = s[i + k] - s[i];
            if (cur >= t) {
                suf[i] = {cur, i};
                t = cur;
                idx = i;
            } else {
                suf[i] = {t, idx};
            }
        }

        vector<int> ans;
        for (int i = k, t = 0; i < n - 2 * k + 1; ++i) {
            int cur = s[i + k] - s[i] + pre[i - 1][0] + suf[i + k][0];
            if (cur > t) {
                ans = {pre[i - 1][1], i, suf[i + k][1]};
                t = cur;
            }
        }

        return ans;
    }
};
```

#### Go

```go
func maxSumOfThreeSubarrays(nums []int, k int) (ans []int) {
	n := len(nums)
	s := make([]int, n+1)
	for i := 0; i < n; i++ {
		s[i+1] = s[i] + nums[i]
	}

	pre := make([][]int, n)
	suf := make([][]int, n)

	for i, t, idx := 0, 0, 0; i < n-k+1; i++ {
		cur := s[i+k] - s[i]
		if cur > t {
			pre[i+k-1] = []int{cur, i}
			t, idx = cur, i
		} else {
			pre[i+k-1] = []int{t, idx}
		}
	}

	for i, t, idx := n-k, 0, 0; i >= 0; i-- {
		cur := s[i+k] - s[i]
		if cur >= t {
			suf[i] = []int{cur, i}
			t, idx = cur, i
		} else {
			suf[i] = []int{t, idx}
		}
	}

	for i, t := k, 0; i < n-2*k+1; i++ {
		cur := s[i+k] - s[i] + pre[i-1][0] + suf[i+k][0]
		if cur > t {
			ans = []int{pre[i-1][1], i, suf[i+k][1]}
			t = cur
		}
	}

	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
