---
comments: true
difficulty: Easy
rating: 1134
source: Biweekly Contest 123 Q1
tags:
    - Array
    - Math
    - Polygon
    - Sorting
---

<!-- problem:start -->

# [3024. Type of Triangle](https://leetcode.com/problems/type-of-triangle)

[中文文档](/solution/3000-3099/3024.Type%20of%20Triangle/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>nums</code> có kích thước <code>3</code> và có thể tạo thành các cạnh của một tam giác.</p>

<ul>
	<li>Một tam giác được gọi là <strong>đều</strong> nếu tất cả các cạnh có độ dài bằng nhau.</li>
	<li>Một tam giác được gọi là <strong>cân</strong> nếu có đúng hai cạnh có độ dài bằng nhau.</li>
	<li>Một tam giác được gọi là <strong>thường</strong> nếu tất cả các cạnh có độ dài khác nhau.</li>
</ul>

<p>Trả về <em>một chuỗi biểu diễn</em> <em>loại tam giác có thể tạo thành </em><em>hoặc </em><code>&quot;none&quot;</code><em> nếu <strong>không thể</strong> tạo thành tam giác.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,3,3]
<strong>Đầu ra:</strong> &quot;equilateral&quot;
<strong>Giải thích:</strong> Vì tất cả các cạnh có độ dài bằng nhau nên tam giác tạo thành là tam giác đều.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,4,5]
<strong>Đầu ra:</strong> &quot;scalene&quot;
<strong>Giải thích:</strong>
nums[0] + nums[1] = 3 + 4 = 7, lớn hơn nums[2] = 5.
nums[0] + nums[2] = 3 + 5 = 8, lớn hơn nums[1] = 4.
nums[1] + nums[2] = 4 + 5 = 9, lớn hơn nums[0] = 3.
Vì tổng của hai cạnh lớn hơn cạnh thứ ba trong cả ba trường hợp nên có thể tạo thành một tam giác.
Vì tất cả các cạnh có độ dài khác nhau nên tam giác tạo thành là tam giác thường.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>nums.length == 3</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Phân tích các trường hợp

<!-- thinking:start -->

> **Tư duy**
>
> Có ba độ dài cạnh và kích thước đầu vào rất nhỏ. Trước tiên, chúng ta kiểm tra bất đẳng thức tam giác, sau đó phân biệt tam giác đều, tam giác cân và tam giác thường.
>
> Sau khi sắp xếp, bất đẳng thức trở thành $a+b>c$; tam giác đều là trường hợp $\min=\max$; tam giác cân là khi một cặp cạnh kề nhau bằng nhau.
>
> Sắp xếp, sau đó rẽ nhánh theo thứ tự đó.

<!-- thinking:end -->

Trước tiên, chúng ta sắp xếp mảng, rồi phân loại và xét theo định nghĩa của tam giác.

- Nếu tổng của hai số nhỏ nhất nhỏ hơn hoặc bằng số lớn nhất thì không thể tạo thành một tam giác, trả về "none".
- Nếu số nhỏ nhất bằng số lớn nhất thì đó là tam giác đều, trả về "equilateral".
- Nếu số nhỏ nhất bằng số ở giữa hoặc số ở giữa bằng số lớn nhất thì đó là tam giác cân, trả về "isosceles".
- Nếu không, trả về "scalene".

Độ phức tạp thời gian là $O(1)$, còn độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def triangleType(self, nums: List[int]) -> str:
        nums.sort()
        if nums[0] + nums[1] <= nums[2]:
            return "none"
        if nums[0] == nums[2]:
            return "equilateral"
        if nums[0] == nums[1] or nums[1] == nums[2]:
            return "isosceles"
        return "scalene"
```

#### Java

```java
class Solution {
    public String triangleType(int[] nums) {
        Arrays.sort(nums);
        if (nums[0] + nums[1] <= nums[2]) {
            return "none";
        }
        if (nums[0] == nums[2]) {
            return "equilateral";
        }
        if (nums[0] == nums[1] || nums[1] == nums[2]) {
            return "isosceles";
        }
        return "scalene";
    }
}
```

#### C++

```cpp
class Solution {
public:
    string triangleType(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        if (nums[0] + nums[1] <= nums[2]) {
            return "none";
        }
        if (nums[0] == nums[2]) {
            return "equilateral";
        }
        if (nums[0] == nums[1] || nums[1] == nums[2]) {
            return "isosceles";
        }
        return "scalene";
    }
};
```

#### Go

```go
func triangleType(nums []int) string {
	sort.Ints(nums)
	if nums[0]+nums[1] <= nums[2] {
		return "none"
	}
	if nums[0] == nums[2] {
		return "equilateral"
	}
	if nums[0] == nums[1] || nums[1] == nums[2] {
		return "isosceles"
	}
	return "scalene"
}
```

#### TypeScript

```ts
function triangleType(nums: number[]): string {
    nums.sort((a, b) => a - b);
    if (nums[0] + nums[1] <= nums[2]) {
        return 'none';
    }
    if (nums[0] === nums[2]) {
        return 'equilateral';
    }
    if (nums[0] === nums[1] || nums[1] === nums[2]) {
        return 'isosceles';
    }
    return 'scalene';
}
```

#### C#

```cs
public class Solution {
    public string TriangleType(int[] nums) {
        Array.Sort(nums);
        if (nums[0] + nums[1] <= nums[2]) {
            return "none";
        }
        if (nums[0] == nums[2]) {
            return "equilateral";
        }
        if (nums[0] == nums[1] || nums[1] == nums[2]) {
            return "isosceles";
        }
        return "scalene";
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
