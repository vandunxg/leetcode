---
comments: true
difficulty: Hard
rating: 1983
source: Weekly Contest 161 Q4
tags:
    - Array
    - Math
    - Greatest Common Divisor
    - Number Theory
    - Euclidean Algorithm
    - Extended Euclidean Algorithm
    - Bézout's Identity
---

<!-- problem:start -->

# [1250. Check If It Is a Good Array](https://leetcode.com/problems/check-if-it-is-a-good-array)

[中文文档](/solution/1200-1299/1250.Check%20If%20It%20Is%20a%20Good%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên dương <code>nums</code>. Nhiệm vụ của bạn là chọn một tập con của <code>nums</code>, nhân mỗi phần tử với một số nguyên rồi cộng các tích lại. Mảng được gọi là <strong>tốt</strong> nếu có thể tạo ra tổng bằng <code>1</code> từ một tập con và các hệ số nguyên tương ứng.</p>

<p>Trả về <code>True</code> nếu mảng <strong>tốt</strong>; nếu không, trả về <code>False</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [12,5,7,23]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Chọn các số 5 và 7.
5*3 + 7*(-2) = 1
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [29,6,10]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Chọn các số 29, 6 và 10.
29*1 + 6*(-3) + 10*(-1) = 1
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,6]
<strong>Đầu ra:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10^5</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10^9</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học (Đẳng thức Bézout)

<!-- thinking:start -->

> **Tư duy**
>
> Đẳng thức Bézout: phương trình $a_1 x_1+\cdots+a_n x_n=1$ có nghiệm nguyên khi và chỉ khi $\gcd(a_1,\ldots,a_n)=1$. Vì $n \le 10^5$, không thể thử mọi tập con.
>
> Chỉ cần tính gcd của cả mảng. Nếu tồn tại tập con gồm các số nguyên tố cùng nhau thì gcd của toàn bộ mảng cũng bằng $1$, nên không cần tìm riêng tập con đó.

<!-- thinking:end -->

Trước tiên, xét trường hợp chọn hai số. Nếu hai số được chọn là $a$ và $b$, theo yêu cầu đề bài, ta cần thỏa mãn $a \times x + b \times y = 1$, trong đó $x$ và $y$ là các số nguyên bất kỳ.

Theo đẳng thức Bézout, nếu $a$ và $b$ nguyên tố cùng nhau thì phương trình trên chắc chắn có nghiệm. Đẳng thức Bézout cũng mở rộng được cho nhiều số: nếu $a_1, a_2, \cdots, a_i$ nguyên tố cùng nhau thì phương trình $a_1 \times x_1 + a_2 \times x_2 + \cdots + a_i \times x_i = 1$ chắc chắn có nghiệm, trong đó $x_1, x_2, \cdots, x_i$ là các số nguyên bất kỳ.

Vì vậy, ta chỉ cần xác định liệu mảng `nums` có chứa một tập con gồm $i$ số nguyên tố cùng nhau hay không. Hai số nguyên tố cùng nhau khi và chỉ khi gcd của chúng bằng $1$. Nếu tồn tại tập con như vậy trong `nums`, gcd của tất cả các số trong mảng `nums` cũng bằng $1$.

Do đó, bài toán trở thành kiểm tra gcd của tất cả số trong mảng `nums` có bằng $1$ hay không. Ta duyệt mảng `nums` và lần lượt tính gcd của các phần tử.

Độ phức tạp thời gian là $O(n + \log m)$ và độ phức tạp không gian là $O(1)$, trong đó $n$ là độ dài mảng `nums`, còn $m$ là giá trị lớn nhất trong `nums`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isGoodArray(self, nums: List[int]) -> bool:
        return reduce(gcd, nums) == 1
```

#### Java

```java
class Solution {
    public boolean isGoodArray(int[] nums) {
        int g = 0;
        for (int x : nums) {
            g = gcd(x, g);
        }
        return g == 1;
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
    bool isGoodArray(vector<int>& nums) {
        int g = 0;
        for (int x : nums) {
            g = gcd(x, g);
        }
        return g == 1;
    }
};
```

#### Go

```go
func isGoodArray(nums []int) bool {
	g := 0
	for _, x := range nums {
		g = gcd(x, g)
	}
	return g == 1
}

func gcd(a, b int) int {
	if b == 0 {
		return a
	}
	return gcd(b, a%b)
}
```

#### TypeScript

```ts
function isGoodArray(nums: number[]): boolean {
    return nums.reduce(gcd) === 1;
}

function gcd(a: number, b: number): number {
    return b === 0 ? a : gcd(b, a % b);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
