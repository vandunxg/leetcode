---
comments: true
difficulty: Medium
rating: 1559
source: Weekly Contest 319 Q2
tags:
    - Array
    - Math
    - Number Theory
---

<!-- problem:start -->

# [2470. Number of Subarrays With LCM Equal to K](https://leetcode.com/problems/number-of-subarrays-with-lcm-equal-to-k)

[中文文档](/solution/2400-2499/2470.Number%20of%20Subarrays%20With%20LCM%20Equal%20to%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>, hãy trả về <em>số lượng <strong>mảng con</strong> của </em><code>nums</code><em> mà bội chung nhỏ nhất của các phần tử trong mảng con bằng </em><code>k</code>.</p>

<p><strong>Mảng con</strong> là một dãy phần tử liên tiếp, không rỗng trong một mảng.</p>

<p><strong>Bội chung nhỏ nhất của một mảng</strong> là số nguyên dương nhỏ nhất chia hết cho tất cả các phần tử trong mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,6,2,7,1], k = 6
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Các mảng con của nums mà 6 là bội chung nhỏ nhất của tất cả phần tử là:
- [<u><strong>3</strong></u>,<u><strong>6</strong></u>,2,7,1]
- [<u><strong>3</strong></u>,<u><strong>6</strong></u>,<u><strong>2</strong></u>,7,1]
- [3,<u><strong>6</strong></u>,2,7,1]
- [3,<u><strong>6</strong></u>,<u><strong>2</strong></u>,7,1]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3], k = 2
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có mảng con nào của nums mà 2 là bội chung nhỏ nhất của tất cả phần tử.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i], k &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt trực tiếp

<!-- thinking:start -->

> **Tư duy**
>
> Với $n\le 1000$, ta cố định đầu trái rồi mở rộng, đồng thời duy trì bội chung nhỏ nhất. Bội chung nhỏ nhất không giảm, nên chỉ cần đếm số lần nó bằng $k$.

<!-- thinking:end -->

Ta lần lượt chọn mỗi phần tử làm phần tử đầu tiên của mảng con, sau đó lần lượt chọn mỗi phần tử làm phần tử cuối của mảng con. Tính bội chung nhỏ nhất của mảng con này. Nếu bội chung nhỏ nhất bằng $k$, ta tăng đáp án lên một.

Độ phức tạp thời gian là $O(n^2)$. Trong đó, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def subarrayLCM(self, nums: List[int], k: int) -> int:
        n = len(nums)
        ans = 0
        for i in range(n):
            a = nums[i]
            for b in nums[i:]:
                x = lcm(a, b)
                ans += x == k
                a = x
        return ans
```

#### Java

```java
class Solution {
    public int subarrayLCM(int[] nums, int k) {
        int n = nums.length;
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            int a = nums[i];
            for (int j = i; j < n; ++j) {
                int b = nums[j];
                int x = lcm(a, b);
                if (x == k) {
                    ++ans;
                }
                a = x;
            }
        }
        return ans;
    }

    private int lcm(int a, int b) {
        return a * b / gcd(a, b);
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int subarrayLCM(vector<int>& nums, int k) {
        int n = nums.size();
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            int a = nums[i];
            for (int j = i; j < n; ++j) {
                int b = nums[j];
                int x = lcm(a, b);
                ans += x == k;
                a = x;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func subarrayLCM(nums []int, k int) (ans int) {
	for i, a := range nums {
		for _, b := range nums[i:] {
			x := lcm(a, b)
			if x == k {
				ans++
			}
			a = x
		}
	}
	return
}

func gcd(a, b int) int {
	if b == 0 {
		return a
	}
	return gcd(b, a%b)
}

func lcm(a, b int) int {
	return a * b / gcd(a, b)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
