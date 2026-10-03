---
comments: true
difficulty: Easy
rating: 1256
source: Biweekly Contest 76 Q1
tags:
    - Array
---

<!-- problem:start -->

# [2239. Find Closest Number to Zero](https://leetcode.com/problems/find-closest-number-to-zero)

[中文文档](/solution/2200-2299/2239.Find%20Closest%20Number%20to%20Zero/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có kích thước <code>n</code>, hãy trả về <em>phần tử có giá trị <strong>gần nhất</strong> với </em><code>0</code><em> trong </em><code>nums</code>. Nếu có nhiều đáp án, hãy trả về <em>phần tử có <strong>giá trị lớn nhất</strong></em>.</p>
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [-4,-2,1,4,8]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>
Khoảng cách từ -4 đến 0 là |-4| = 4.
Khoảng cách từ -2 đến 0 là |-2| = 2.
Khoảng cách từ 1 đến 0 là |1| = 1.
Khoảng cách từ 4 đến 0 là |4| = 4.
Khoảng cách từ 8 đến 0 là |8| = 8.
Do đó, phần tử gần 0 nhất trong mảng là 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,-1,1]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> 1 và -1 đều là những phần tử gần 0 nhất, nên trả về 1 vì đây là phần tử lớn hơn.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
	<li><code>-10<sup>5</sup> &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt một lần

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm giá trị gần 0 nhất, và nếu có nhiều giá trị cùng khoảng cách thì chọn số lớn hơn. Với $n \le 10^3$, ta có thể sắp xếp mảng, nhưng chỉ cần duy trì một đáp án tối ưu là đủ.
>
> Duyệt qua mảng một lần: thay đáp án khi giá trị tuyệt đối nhỏ hơn, hoặc khi bằng nhau nhưng bản thân số đó lớn hơn.

<!-- thinking:end -->

Ta dùng biến $\textit{d}$ để lưu khoảng cách nhỏ nhất hiện tại, ban đầu $\textit{d}=\infty$. Sau đó duyệt mảng; với mỗi phần tử $x$, ta tính $y=|x|$. Nếu $y \lt d$ hoặc $y=d$ và $x \gt \textit{ans}$, ta cập nhật đáp án thành $\textit{ans}=x$ và $\textit{d}=y$.

Sau khi duyệt xong, trả về đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findClosestNumber(self, nums: List[int]) -> int:
        ans, d = 0, inf
        for x in nums:
            if (y := abs(x)) < d or (y == d and x > ans):
                ans, d = x, y
        return ans
```

#### Java

```java
class Solution {
    public int findClosestNumber(int[] nums) {
        int ans = 0, d = 1 << 30;
        for (int x : nums) {
            int y = Math.abs(x);
            if (y < d || (y == d && x > ans)) {
                ans = x;
                d = y;
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
    int findClosestNumber(vector<int>& nums) {
        int ans = 0, d = 1 << 30;
        for (int x : nums) {
            int y = abs(x);
            if (y < d || (y == d && x > ans)) {
                ans = x;
                d = y;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findClosestNumber(nums []int) int {
	ans, d := 0, 1<<30
	for _, x := range nums {
		if y := abs(x); y < d || (y == d && x > ans) {
			ans, d = x, y
		}
	}
	return ans
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function findClosestNumber(nums: number[]): number {
    let [ans, d] = [0, 1 << 30];
    for (const x of nums) {
        const y = Math.abs(x);
        if (y < d || (y == d && x > ans)) {
            [ans, d] = [x, y];
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
