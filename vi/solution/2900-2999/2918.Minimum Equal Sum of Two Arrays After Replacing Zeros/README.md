---
comments: true
difficulty: Medium
rating: 1526
source: Weekly Contest 369 Q2
tags:
    - Greedy
    - Array
---

<!-- problem:start -->

# [2918. Minimum Equal Sum of Two Arrays After Replacing Zeros](https://leetcode.com/problems/minimum-equal-sum-of-two-arrays-after-replacing-zeros)

[中文文档](/solution/2900-2999/2918.Minimum%20Equal%20Sum%20of%20Two%20Arrays%20After%20Replacing%20Zeros/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng <code>nums1</code> và <code>nums2</code> gồm các số nguyên dương.</p>

<p>Bạn phải thay thế <strong>tất cả</strong> các số <code>0</code> trong cả hai mảng bằng các số nguyên <strong>nghiêm ngặt</strong> dương sao cho tổng các phần tử của hai mảng <strong>bằng nhau</strong>.</p>

<p>Trả về <em>tổng bằng nhau <strong>nhỏ nhất</strong> có thể đạt được, hoặc </em><code>-1</code><em> nếu không thể</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [3,2,0,1,0], nums2 = [6,5,0]
<strong>Đầu ra:</strong> 12
<strong>Giải thích:</strong> Ta có thể thay thế các số 0 như sau:
- Thay hai số 0 trong nums1 bằng các giá trị 2 và 4. Mảng thu được là nums1 = [3,2,2,1,4].
- Thay số 0 trong nums2 bằng giá trị 1. Mảng thu được là nums2 = [6,5,1].
Cả hai mảng có tổng bằng 12. Có thể chứng minh rằng đây là tổng nhỏ nhất có thể đạt được.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [2,0,2,0], nums2 = [1,4]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không thể làm cho tổng của hai mảng bằng nhau.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums1.length, nums2.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums1[i], nums2[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phân tích trường hợp

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi số 0 được thay bằng ít nhất $1$, vì vậy các cận dưới $s_1,s_2$ là tổng sau khi coi các số 0 là 1. Một mảng không có số 0 thì không thể tăng tổng của nó.
>
> Giả sử $s_1 \le s_2$. Giá trị chung này là kết quả khi hai tổng bằng nhau; nếu $s_1 < s_2$, mảng nhỏ hơn vẫn phải chứa một số 0 để đạt tới $s_2$, nếu không thì bài toán là bất khả thi. Chỉ cần một lượt duyệt để tính tổng và đếm số 0 là có thể quyết định các trường hợp.

<!-- thinking:end -->

Ta xét trường hợp coi tất cả các số $0$ trong mảng là $1$ và tính riêng tổng của hai mảng, lần lượt ký hiệu là $s_1$ và $s_2$. Không mất tính tổng quát, giả sử $s_1 \le s_2$.

- Nếu $s_1 = s_2$, kết quả là $s_1$.
- Nếu $s_1 \lt s_2$, phải tồn tại một số $0$ trong $nums1$ để làm cho tổng của hai mảng bằng nhau. Khi đó, kết quả là $s_2$. Nếu không, nghĩa là không thể làm cho tổng của hai mảng bằng nhau, và ta trả về $-1$.

Độ phức tạp thời gian là $O(n + m)$, trong đó $n$ và $m$ lần lượt là độ dài của hai mảng $nums1$ và $nums2$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minSum(self, nums1: List[int], nums2: List[int]) -> int:
        s1 = sum(nums1) + nums1.count(0)
        s2 = sum(nums2) + nums2.count(0)
        if s1 > s2:
            return self.minSum(nums2, nums1)
        if s1 == s2:
            return s1
        return -1 if nums1.count(0) == 0 else s2
```

#### Java

```java
class Solution {
    public long minSum(int[] nums1, int[] nums2) {
        long s1 = 0, s2 = 0;
        boolean hasZero = false;
        for (int x : nums1) {
            hasZero |= x == 0;
            s1 += Math.max(x, 1);
        }
        for (int x : nums2) {
            s2 += Math.max(x, 1);
        }
        if (s1 > s2) {
            return minSum(nums2, nums1);
        }
        if (s1 == s2) {
            return s1;
        }
        return hasZero ? s2 : -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minSum(vector<int>& nums1, vector<int>& nums2) {
        long long s1 = 0, s2 = 0;
        bool hasZero = false;
        for (int x : nums1) {
            hasZero |= x == 0;
            s1 += max(x, 1);
        }
        for (int x : nums2) {
            s2 += max(x, 1);
        }
        if (s1 > s2) {
            return minSum(nums2, nums1);
        }
        if (s1 == s2) {
            return s1;
        }
        return hasZero ? s2 : -1;
    }
};
```

#### Go

```go
func minSum(nums1 []int, nums2 []int) int64 {
	s1, s2 := 0, 0
	hasZero := false
	for _, x := range nums1 {
		if x == 0 {
			hasZero = true
		}
		s1 += max(x, 1)
	}
	for _, x := range nums2 {
		s2 += max(x, 1)
	}
	if s1 > s2 {
		return minSum(nums2, nums1)
	}
	if s1 == s2 {
		return int64(s1)
	}
	if hasZero {
		return int64(s2)
	}
	return -1
}
```

#### TypeScript

```ts
function minSum(nums1: number[], nums2: number[]): number {
    let [s1, s2] = [0, 0];
    let hasZero = false;
    for (const x of nums1) {
        if (x === 0) {
            hasZero = true;
        }
        s1 += Math.max(x, 1);
    }
    for (const x of nums2) {
        s2 += Math.max(x, 1);
    }
    if (s1 > s2) {
        return minSum(nums2, nums1);
    }
    if (s1 === s2) {
        return s1;
    }
    return hasZero ? s2 : -1;
}
```

#### C#

```cs
public class Solution {
    public long MinSum(int[] nums1, int[] nums2) {
        long s1 = 0, s2 = 0;
        bool hasZero = false;
        foreach (int x in nums1) {
            hasZero |= x == 0;
            s1 += Math.Max(x, 1);
        }
        foreach (int x in nums2) {
            s2 += Math.Max(x, 1);
        }
        if (s1 > s2) {
            return MinSum(nums2, nums1);
        }
        if (s1 == s2) {
            return s1;
        }
        return hasZero ? s2 : -1;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
