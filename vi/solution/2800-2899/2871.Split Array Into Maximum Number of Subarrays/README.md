---
comments: true
difficulty: Medium
rating: 1749
source: Biweekly Contest 114 Q3
tags:
    - Greedy
    - Bit Manipulation
    - Array
---

<!-- problem:start -->

# [2871. Split Array Into Maximum Number of Subarrays](https://leetcode.com/problems/split-array-into-maximum-number-of-subarrays)

[中文文档](/solution/2800-2899/2871.Split%20Array%20Into%20Maximum%20Number%20of%20Subarrays/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <code>nums</code> gồm các số nguyên <strong>không âm</strong>.</p>

<p>Ta định nghĩa điểm số của mảng con <code>nums[l..r]</code> với <code>l &lt;= r</code> là <code>nums[l] AND nums[l + 1] AND ... AND nums[r]</code>, trong đó <strong>AND</strong> là phép toán <code>AND</code> theo bit.</p>

<p>Hãy chia mảng thành một hoặc nhiều mảng con sao cho thỏa mãn các điều kiện sau:</p>

<ul>
	<li><strong>Mỗi</strong><strong> phần tử</strong> của mảng thuộc về <strong>duy nhất</strong> một mảng con.</li>
	<li>Tổng điểm số của các mảng con là <strong>nhỏ nhất</strong> có thể.</li>
</ul>

<p>Trả về <em><strong>số lượng mảng con lớn nhất</strong> trong một cách chia thỏa mãn các điều kiện trên.</em></p>

<p><strong>Mảng con</strong> là một phần liên tiếp của mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,0,2,0,1,2]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ta có thể chia mảng thành các mảng con sau:
- [1,0]. Điểm số của mảng con này là 1 AND 0 = 0.
- [2,0]. Điểm số của mảng con này là 2 AND 0 = 0.
- [1,2]. Điểm số của mảng con này là 1 AND 2 = 0.
Tổng điểm số là 0 + 0 + 0 = 0, đây là tổng điểm số nhỏ nhất có thể đạt được.
Có thể chứng minh rằng không thể chia mảng thành nhiều hơn 3 mảng con với tổng điểm số bằng 0. Vì vậy, ta trả về 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,7,1,3]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Ta có thể chia mảng thành một mảng con: [5,7,1,3] với điểm số bằng 1, đây là tổng điểm số nhỏ nhất có thể đạt được.
Có thể chứng minh rằng không thể chia mảng thành nhiều hơn 1 mảng con với tổng điểm số bằng 1. Vì vậy, ta trả về 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Phép toán theo bit

<!-- thinking:start -->

> **Tư duy**
>
> Điểm số của một mảng con là phép AND theo bit của các phần tử trong đó, vì vậy tổng điểm số sau khi chia bằng phép AND của toàn bộ mảng. Để tổng này nhỏ nhất, ta cần cắt được nhiều mảng con có điểm số bằng 0 nhất có thể. Ta tích lũy phép AND từ trái sang phải và cắt ngay khi kết quả trở thành $0$; nếu điều này không xảy ra, mảng vẫn là một mảng con duy nhất.

<!-- thinking:end -->

Ta khởi tạo biến $score$ để lưu điểm số của mảng con hiện tại và ban đầu đặt $score=-1$. Sau đó, ta duyệt mảng; với mỗi phần tử $num$, ta thực hiện phép AND theo bit giữa $score$ và $num$, rồi gán kết quả cho $score$. Nếu $score=0$, điều đó có nghĩa điểm số của mảng con hiện tại bằng 0, nên ta có thể tách mảng con hiện tại và đặt lại $score$ thành $-1$. Cuối cùng, ta trả về số lượng mảng con đã chia.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSubarrays(self, nums: List[int]) -> int:
        score, ans = -1, 1
        for num in nums:
            score &= num
            if score == 0:
                score = -1
                ans += 1
        return 1 if ans == 1 else ans - 1
```

#### Java

```java
class Solution {
    public int maxSubarrays(int[] nums) {
        int score = -1;
        int ans = 1;
        for (int num : nums) {
            score &= num;
            if (score == 0) {
                ans++;
                score = -1;
            }
        }
        return ans == 1 ? 1 : ans - 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxSubarrays(vector<int>& nums) {
        int score = -1, ans = 1;
        for (int num : nums) {
            score &= num;
            if (score == 0) {
                --score;
                ++ans;
            }
        }
        return ans == 1 ? 1 : ans - 1;
    }
};
```

#### Go

```go
func maxSubarrays(nums []int) int {
	ans, score := 1, -1
	for _, num := range nums {
		score &= num
		if score == 0 {
			score--
			ans++
		}
	}
	if ans == 1 {
		return 1
	}
	return ans - 1
}
```

#### TypeScript

```ts
function maxSubarrays(nums: number[]): number {
    let [ans, score] = [1, -1];
    for (const num of nums) {
        score &= num;
        if (score === 0) {
            --score;
            ++ans;
        }
    }
    return ans == 1 ? 1 : ans - 1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
