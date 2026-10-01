---
comments: true
difficulty: Easy
tags:
    - Array
    - Sorting
---

<!-- problem:start -->

# [414. Third Maximum Number](https://leetcode.com/problems/third-maximum-number)

[中文文档](/solution/0400-0499/0414.Third%20Maximum%20Number/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code>.</p>

<p>Hãy trả về số <strong>lớn thứ ba phân biệt</strong> trong mảng. Nếu không có số <strong>lớn thứ ba</strong>, hãy trả về số <strong>lớn nhất</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,2,1]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>
Số lớn nhất phân biệt thứ nhất là 3.
Số lớn nhất phân biệt thứ hai là 2.
Số lớn nhất phân biệt thứ ba là 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Số lớn nhất phân biệt thứ nhất là 2.
Số lớn nhất phân biệt thứ hai là 1.
Không có số lớn nhất phân biệt thứ ba, nên thay vào đó trả về số lớn nhất (2).
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,2,3,1]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>
Số lớn nhất phân biệt thứ nhất là 3.
Số lớn nhất phân biệt thứ hai là 2 (hai số 2 được tính là một vì chúng có cùng giá trị).
Số lớn nhất phân biệt thứ ba là 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>4</sup></code></li>
	<li><code>-2<sup>31</sup> &lt;= nums[i] &lt;= 2<sup>31</sup> - 1</code></li>
</ul>

<p>&nbsp;</p>
<strong>Câu hỏi mở rộng:</strong> Bạn có thể tìm lời giải <code>O(n)</code> không?

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt một lượt

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm số lớn thứ ba phân biệt. Có thể sắp xếp rồi loại phần tử trùng, nhưng câu hỏi mở rộng yêu cầu duyệt tuyến tính và dùng bộ nhớ phụ hằng số.
>
> Theo dõi $m_1>m_2>m_3$, bỏ qua giá trị đã lưu và dịch chuyển ba biến khi thêm giá trị mới. Nếu $m_3$ vẫn bằng giá trị sentinel, mảng có ít hơn ba giá trị phân biệt và đáp án là giá trị lớn nhất.
>
> Cần bỏ qua phần tử trùng; nếu không, cùng một số có thể chiếm cả ba vị trí.

<!-- thinking:end -->

Ta dùng ba biến $m_1$, $m_2$ và $m_3$ lần lượt lưu số lớn nhất, lớn thứ hai và lớn thứ ba trong mảng. Ban đầu, đặt cả ba biến bằng âm vô cực.

Sau đó, duyệt lần lượt từng số trong mảng và xử lý như sau:

- Nếu số đó bằng $m_1$, $m_2$ hoặc $m_3$, bỏ qua.
- Nếu số đó lớn hơn $m_1$, cập nhật các giá trị của $m_1$, $m_2$ và $m_3$ lần lượt thành $m_2$, $m_3$ và số đó.
- Nếu số đó lớn hơn $m_2$, cập nhật các giá trị của $m_2$ và $m_3$ lần lượt thành $m_3$ và số đó.
- Nếu số đó lớn hơn $m_3$, cập nhật $m_3$ thành số đó.

Cuối cùng, nếu $m_3$ chưa được cập nhật thì mảng không có số lớn thứ ba phân biệt, nên trả về $m_1$. Nếu không, trả về $m_3$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng `nums`. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def thirdMax(self, nums: List[int]) -> int:
        m1 = m2 = m3 = -inf
        for num in nums:
            if num in [m1, m2, m3]:
                continue
            if num > m1:
                m3, m2, m1 = m2, m1, num
            elif num > m2:
                m3, m2 = m2, num
            elif num > m3:
                m3 = num
        return m3 if m3 != -inf else m1
```

#### Java

```java
class Solution {
    public int thirdMax(int[] nums) {
        long m1 = Long.MIN_VALUE;
        long m2 = Long.MIN_VALUE;
        long m3 = Long.MIN_VALUE;
        for (int num : nums) {
            if (num == m1 || num == m2 || num == m3) {
                continue;
            }
            if (num > m1) {
                m3 = m2;
                m2 = m1;
                m1 = num;
            } else if (num > m2) {
                m3 = m2;
                m2 = num;
            } else if (num > m3) {
                m3 = num;
            }
        }
        return (int) (m3 != Long.MIN_VALUE ? m3 : m1);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int thirdMax(vector<int>& nums) {
        long m1 = LONG_MIN, m2 = LONG_MIN, m3 = LONG_MIN;
        for (int num : nums) {
            if (num == m1 || num == m2 || num == m3) continue;
            if (num > m1) {
                m3 = m2;
                m2 = m1;
                m1 = num;
            } else if (num > m2) {
                m3 = m2;
                m2 = num;
            } else if (num > m3) {
                m3 = num;
            }
        }
        return (int) (m3 != LONG_MIN ? m3 : m1);
    }
};
```

#### Go

```go
func thirdMax(nums []int) int {
	m1, m2, m3 := math.MinInt64, math.MinInt64, math.MinInt64
	for _, num := range nums {
		if num == m1 || num == m2 || num == m3 {
			continue
		}
		if num > m1 {
			m3, m2, m1 = m2, m1, num
		} else if num > m2 {
			m3, m2 = m2, num
		} else if num > m3 {
			m3 = num
		}
	}
	if m3 != math.MinInt64 {
		return m3
	}
	return m1
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
