---
comments: true
difficulty: Easy
rating: 1256
source: Biweekly Contest 2 Q1
tags:
    - Array
    - Math
---

<!-- problem:start -->

# [1085. Sum of Digits in the Minimum Number 🔒](https://leetcode.com/problems/sum-of-digits-in-the-minimum-number)

[中文文档](/solution/1000-1099/1085.Sum%20of%20Digits%20in%20the%20Minimum%20Number/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code>, trả về <code>0</code><em> nếu tổng các chữ số của số nguyên nhỏ nhất trong </em><code>nums</code><em> là số lẻ; nếu không, trả về </em><code>1</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [34,23,1,24,75,33,54,8]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Phần tử nhỏ nhất là 1, tổng các chữ số của nó là 1 (số lẻ), nên đáp án là 0.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [99,77,33,66,55]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Phần tử nhỏ nhất là 33, tổng các chữ số của nó là 3 + 3 = 6 (số chẵn), nên đáp án là 1.
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

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ cần xét tính chẵn lẻ của tổng chữ số của giá trị nhỏ nhất: tổng chẵn cho kết quả $1$, tổng lẻ cho kết quả $0$. Tìm số nhỏ nhất rồi tách lần lượt các chữ số.
>
> Bit thấp nhất của $s$ cho biết tính chẵn lẻ; ta trả về $s\&1\oplus 1$.
>
> Không cần chuyển số thành chuỗi.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumOfDigits(self, nums: List[int]) -> int:
        x = min(nums)
        s = 0
        while x:
            s += x % 10
            x //= 10
        return s & 1 ^ 1
```

#### Java

```java
class Solution {
    public int sumOfDigits(int[] nums) {
        int x = 100;
        for (int v : nums) {
            x = Math.min(x, v);
        }
        int s = 0;
        for (; x > 0; x /= 10) {
            s += x % 10;
        }
        return s & 1 ^ 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int sumOfDigits(vector<int>& nums) {
        int x = *min_element(nums.begin(), nums.end());
        int s = 0;
        for (; x > 0; x /= 10) {
            s += x % 10;
        }
        return s & 1 ^ 1;
    }
};
```

#### Go

```go
func sumOfDigits(nums []int) int {
	s := 0
	for x := slices.Min(nums); x > 0; x /= 10 {
		s += x % 10
	}
	return s&1 ^ 1
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
