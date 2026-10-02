---
comments: true
difficulty: Hard
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [801. Minimum Swaps To Make Sequences Increasing](https://leetcode.com/problems/minimum-swaps-to-make-sequences-increasing)

[中文文档](/solution/0800-0899/0801.Minimum%20Swaps%20To%20Make%20Sequences%20Increasing/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <code>nums1</code> và <code>nums2</code> có cùng độ dài. Trong một thao tác, bạn được phép hoán đổi <code>nums1[i]</code> với <code>nums2[i]</code>.</p>

<ul>
	<li>Ví dụ, nếu <code>nums1 = [1,2,3,<u>8</u>]</code> và <code>nums2 = [5,6,7,<u>4</u>]</code>, bạn có thể hoán đổi các phần tử tại <code>i = 3</code> để được <code>nums1 = [1,2,3,4]</code> và <code>nums2 = [5,6,7,8]</code>.</li>
</ul>

<p>Hãy trả về <em>số thao tác ít nhất cần dùng để làm cho </em><code>nums1</code><em> và </em><code>nums2</code><em> <strong>tăng nghiêm ngặt</strong></em>. Các test case được tạo sao cho luôn tồn tại cách thực hiện với đầu vào đã cho.</p>

<p>Mảng <code>arr</code> <strong>tăng nghiêm ngặt</strong> khi và chỉ khi <code>arr[0] &lt; arr[1] &lt; arr[2] &lt; ... &lt; arr[arr.length - 1]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [1,3,5,4], nums2 = [1,2,3,7]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> 
Hoán đổi nums1[3] và nums2[3]. Khi đó, hai dãy là:
nums1 = [1, 3, 5, 7] và nums2 = [1, 2, 3, 4]
và cả hai đều tăng nghiêm ngặt.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [0,3,5,8,9], nums2 = [2,1,4,6,9]
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums1.length &lt;= 10<sup>5</sup></code></li>
	<li><code>nums2.length == nums1.length</code></li>
	<li><code>0 &lt;= nums1[i], nums2[i] &lt;= 2 * 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Dynamic Programming

<!-- thinking:start -->

> **Tư duy**
>
> Ở mỗi chỉ số, ta có thể hoán đổi hoặc giữ nguyên; không thể liệt kê $2^n$ cách chọn vì $n\le 10^5$. Việc cả hai dãy có tiếp tục tăng nghiêm ngặt tại một vị trí hay không chỉ phụ thuộc vào quyết định hoán đổi ở chỉ số trước đó và chỉ số hiện tại.
>
> Vì vậy, ta lưu số lần hoán đổi ít nhất để đến chỉ số hiện tại trong hai trạng thái: không hoán đổi vị trí này hoặc có hoán đổi. Các chuyển trạng thái dựa trên việc cặp phần tử hiện tại đã tăng hay chưa và hoán đổi chéo có giữ được thứ tự tăng hay không. Chỉ cần hai trạng thái trước đó nên có thể dùng hai biến luân phiên.

<!-- thinking:end -->

Gọi $a$ và $b$ lần lượt là số lần hoán đổi ít nhất để các dãy phần tử tăng nghiêm ngặt đến chỉ số $[0..i]$, trong đó phần tử thứ $i$ không bị hoán đổi và bị hoán đổi. Chỉ số bắt đầu từ $0$.

Khi $i=0$, ta có $a = 0$ và $b = 1$.

Khi $i \gt 0$, trước tiên ta lưu giá trị trước đó của $a$ và $b$ vào $x$ và $y$, rồi xét các trường hợp sau:

Nếu $nums1[i - 1] \ge nums1[i]$ hoặc $nums2[i - 1] \ge nums2[i]$, để cả hai dãy tăng nghiêm ngặt thì vị trí tương đối của các phần tử tại chỉ số $i-1$ và $i$ phải thay đổi. Nghĩa là nếu vị trí trước đã được hoán đổi thì vị trí hiện tại không nên hoán đổi, do đó $a = y$; nếu vị trí trước chưa được hoán đổi thì vị trí hiện tại phải được hoán đổi, do đó $b = x + 1$.

Ngược lại, vị trí tương đối của các phần tử tại chỉ số $i-1$ và $i$ không cần thay đổi, nên $b = y + 1$. Ngoài ra, nếu $nums1[i - 1] \lt nums2[i]$ và $nums2[i - 1] \lt nums1[i]$, vị trí tương đối của các phần tử tại hai chỉ số này có thể thay đổi. Khi đó, $a$ và $b$ có thể nhận giá trị nhỏ hơn: $a = \min(a, y)$ và $b = \min(b, x + 1)$.

Cuối cùng, trả về giá trị nhỏ hơn giữa $a$ và $b$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minSwap(self, nums1: List[int], nums2: List[int]) -> int:
        a, b = 0, 1
        for i in range(1, len(nums1)):
            x, y = a, b
            if nums1[i - 1] >= nums1[i] or nums2[i - 1] >= nums2[i]:
                a, b = y, x + 1
            else:
                b = y + 1
                if nums1[i - 1] < nums2[i] and nums2[i - 1] < nums1[i]:
                    a, b = min(a, y), min(b, x + 1)
        return min(a, b)
```

#### Java

```java
class Solution {
    public int minSwap(int[] nums1, int[] nums2) {
        int a = 0, b = 1;
        for (int i = 1; i < nums1.length; ++i) {
            int x = a, y = b;
            if (nums1[i - 1] >= nums1[i] || nums2[i - 1] >= nums2[i]) {
                a = y;
                b = x + 1;
            } else {
                b = y + 1;
                if (nums1[i - 1] < nums2[i] && nums2[i - 1] < nums1[i]) {
                    a = Math.min(a, y);
                    b = Math.min(b, x + 1);
                }
            }
        }
        return Math.min(a, b);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minSwap(vector<int>& nums1, vector<int>& nums2) {
        int a = 0, b = 1, n = nums1.size();
        for (int i = 1; i < n; ++i) {
            int x = a, y = b;
            if (nums1[i - 1] >= nums1[i] || nums2[i - 1] >= nums2[i]) {
                a = y, b = x + 1;
            } else {
                b = y + 1;
                if (nums1[i - 1] < nums2[i] && nums2[i - 1] < nums1[i]) {
                    a = min(a, y);
                    b = min(b, x + 1);
                }
            }
        }
        return min(a, b);
    }
};
```

#### Go

```go
func minSwap(nums1 []int, nums2 []int) int {
	a, b, n := 0, 1, len(nums1)
	for i := 1; i < n; i++ {
		x, y := a, b
		if nums1[i-1] >= nums1[i] || nums2[i-1] >= nums2[i] {
			a, b = y, x+1
		} else {
			b = y + 1
			if nums1[i-1] < nums2[i] && nums2[i-1] < nums1[i] {
				a = min(a, y)
				b = min(b, x+1)
			}
		}
	}
	return min(a, b)
}
```

#### TypeScript

```ts
function minSwap(nums1: number[], nums2: number[]): number {
    let [a, b] = [0, 1];
    for (let i = 1; i < nums1.length; ++i) {
        let x = a,
            y = b;
        if (nums1[i - 1] >= nums1[i] || nums2[i - 1] >= nums2[i]) {
            a = y;
            b = x + 1;
        } else {
            b = y + 1;
            if (nums1[i - 1] < nums2[i] && nums2[i - 1] < nums1[i]) {
                a = Math.min(a, y);
                b = Math.min(b, x + 1);
            }
        }
    }
    return Math.min(a, b);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
