---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [2098. Subsequence of Size K With the Largest Even Sum 🔒](https://leetcode.com/problems/subsequence-of-size-k-with-the-largest-even-sum)

[中文文档](/solution/2000-2099/2098.Subsequence%20of%20Size%20K%20With%20the%20Largest%20Even%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>. Hãy tìm <strong>tổng chẵn lớn nhất</strong> của một dãy con trong <code>nums</code> có độ dài <code>k</code>.</p>

<p>Trả về <em>tổng này hoặc </em><code>-1</code><em> nếu không tồn tại tổng như vậy</em>.</p>

<p><strong>Dãy con</strong> là một mảng có thể được tạo ra từ một mảng khác bằng cách xóa một số hoặc không xóa phần tử nào mà không thay đổi thứ tự của các phần tử còn lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,1,5,3,1], k = 3
<strong>Đầu ra:</strong> 12
<strong>Giải thích:</strong>
Dãy con có tổng chẵn lớn nhất có thể là [4,5,3]. Tổng của nó là 4 + 5 + 3 = 12.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,6,2], k = 3
<strong>Đầu ra:</strong> 12
<strong>Giải thích:</strong>
Dãy con có tổng chẵn lớn nhất có thể là [4,6,2]. Tổng của nó là 4 + 6 + 2 = 12.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,5], k = 1
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong>
Không có dãy con nào của nums có độ dài 1 và tổng chẵn.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k &lt;= nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Thứ tự không quan trọng; tính chẵn lẻ của tổng được quyết định bởi số lượng số lẻ. Chọn $k$ số lớn nhất; nếu tổng chẵn thì dừng. Nếu không, thay một giá trị để đổi tính chẵn lẻ với tổn thất nhỏ nhất.
>
> Ta có thể thay số chẵn nhỏ nhất trong các số đã chọn bằng số lẻ lớn nhất còn lại, hoặc thay số lẻ nhỏ nhất trong các số đã chọn bằng số chẵn lớn nhất còn lại. Chọn phương án tốt hơn; nếu cả hai đều không thực hiện được, trả về $-1$.

<!-- thinking:end -->

Ta nhận thấy bài toán yêu cầu chọn một dãy con, nên trước tiên có thể sắp xếp mảng.

Tiếp theo, ta tham lam chọn $k$ số lớn nhất. Nếu tổng của các số này là số chẵn, ta trả về trực tiếp tổng này là $ans$.

Nếu không, ta có hai chiến lược tham lam:

1. Trong $k$ số lớn nhất, tìm số chẵn nhỏ nhất $mi1$, sau đó trong $n - k$ số còn lại, tìm số lẻ lớn nhất $mx1$. Thay $mi1$ bằng $mx1$. Nếu thực hiện được phép thay thế này, tổng sau khi thay $ans - mi1 + mx1$ chắc chắn là số chẵn;
2. Trong $k$ số lớn nhất, tìm số lẻ nhỏ nhất $mi2$, sau đó trong $n - k$ số còn lại, tìm số chẵn lớn nhất $mx2$. Thay $mi2$ bằng $mx2$. Nếu thực hiện được phép thay thế này, tổng sau khi thay $ans - mi2 + mx2$ chắc chắn là số chẵn.

Ta chọn tổng chẵn lớn nhất làm đáp án. Nếu không tồn tại tổng chẵn, trả về $-1$.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(\log n)$. Trong đó, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def largestEvenSum(self, nums: List[int], k: int) -> int:
        nums.sort()
        ans = sum(nums[-k:])
        if ans % 2 == 0:
            return ans
        n = len(nums)
        mx1 = mx2 = -inf
        for x in nums[: n - k]:
            if x & 1:
                mx1 = x
            else:
                mx2 = x
        mi1 = mi2 = inf
        for x in nums[-k:][::-1]:
            if x & 1:
                mi2 = x
            else:
                mi1 = x
        ans = max(ans - mi1 + mx1, ans - mi2 + mx2, -1)
        return -1 if ans < 0 else ans
```

#### Java

```java
class Solution {
    public long largestEvenSum(int[] nums, int k) {
        Arrays.sort(nums);
        long ans = 0;
        int n = nums.length;
        for (int i = 0; i < k; ++i) {
            ans += nums[n - i - 1];
        }
        if (ans % 2 == 0) {
            return ans;
        }
        final int inf = 1 << 29;
        int mx1 = -inf, mx2 = -inf;
        for (int i = 0; i < n - k; ++i) {
            if (nums[i] % 2 == 1) {
                mx1 = nums[i];
            } else {
                mx2 = nums[i];
            }
        }
        int mi1 = inf, mi2 = inf;
        for (int i = n - 1; i >= n - k; --i) {
            if (nums[i] % 2 == 1) {
                mi2 = nums[i];
            } else {
                mi1 = nums[i];
            }
        }
        ans = Math.max(ans - mi1 + mx1, ans - mi2 + mx2);
        return ans < 0 ? -1 : ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long largestEvenSum(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end());
        long long ans = 0;
        int n = nums.size();
        for (int i = 0; i < k; ++i) {
            ans += nums[n - i - 1];
        }
        if (ans % 2 == 0) {
            return ans;
        }
        const int inf = 1 << 29;
        int mx1 = -inf, mx2 = -inf;
        for (int i = 0; i < n - k; ++i) {
            if (nums[i] % 2) {
                mx1 = nums[i];
            } else {
                mx2 = nums[i];
            }
        }
        int mi1 = inf, mi2 = inf;
        for (int i = n - 1; i >= n - k; --i) {
            if (nums[i] % 2) {
                mi2 = nums[i];
            } else {
                mi1 = nums[i];
            }
        }
        ans = max(ans - mi1 + mx1, ans - mi2 + mx2);
        return ans < 0 ? -1 : ans;
    }
};
```

#### Go

```go
func largestEvenSum(nums []int, k int) int64 {
	sort.Ints(nums)
	ans := 0
	n := len(nums)
	for i := 0; i < k; i++ {
		ans += nums[n-1-i]
	}
	if ans%2 == 0 {
		return int64(ans)
	}
	const inf = 1 << 29
	mx1, mx2 := -inf, -inf
	for _, x := range nums[:n-k] {
		if x%2 == 1 {
			mx1 = x
		} else {
			mx2 = x
		}
	}
	mi1, mi2 := inf, inf
	for i := n - 1; i >= n-k; i-- {
		if nums[i]%2 == 1 {
			mi2 = nums[i]
		} else {
			mi1 = nums[i]
		}
	}
	ans = max(-1, max(ans-mi1+mx1, ans-mi2+mx2))
	if ans%2 < 0 {
		return -1
	}
	return int64(ans)
}
```

#### TypeScript

```ts
function largestEvenSum(nums: number[], k: number): number {
    nums.sort((a, b) => a - b);
    let ans = 0;
    const n = nums.length;
    for (let i = 0; i < k; ++i) {
        ans += nums[n - i - 1];
    }
    if (ans % 2 === 0) {
        return ans;
    }
    const inf = 1 << 29;
    let mx1 = -inf,
        mx2 = -inf;
    for (let i = 0; i < n - k; ++i) {
        if (nums[i] % 2 === 1) {
            mx1 = nums[i];
        } else {
            mx2 = nums[i];
        }
    }
    let mi1 = inf,
        mi2 = inf;
    for (let i = n - 1; i >= n - k; --i) {
        if (nums[i] % 2 === 1) {
            mi2 = nums[i];
        } else {
            mi1 = nums[i];
        }
    }
    ans = Math.max(ans - mi1 + mx1, ans - mi2 + mx2);
    return ans < 0 ? -1 : ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
