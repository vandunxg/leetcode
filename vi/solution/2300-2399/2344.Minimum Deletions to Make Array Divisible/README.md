---
comments: true
difficulty: Hard
rating: 1640
source: Weekly Contest 302 Q4
tags:
    - Array
    - Math
    - Greatest Common Divisor
    - Number Theory
    - Sorting
    - Heap (Priority Queue)
    - Euclidean Algorithm
---

<!-- problem:start -->

# [2344. Minimum Deletions to Make Array Divisible](https://leetcode.com/problems/minimum-deletions-to-make-array-divisible)

[中文文档](/solution/2300-2399/2344.Minimum%20Deletions%20to%20Make%20Array%20Divisible/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng số nguyên dương <code>nums</code> và <code>numsDivide</code>. Bạn có thể xóa bất kỳ số lượng phần tử nào khỏi <code>nums</code>.</p>

<p>Hãy trả về <em>số lượng phần tử <strong>ít nhất</strong> cần xóa sao cho phần tử <strong>nhỏ nhất</strong> trong </em><code>nums</code><em> <strong>chia hết</strong> cho mọi phần tử trong </em><code>numsDivide</code>. Nếu không thể thực hiện, trả về <code>-1</code>.</p>

<p>Lưu ý rằng một số nguyên <code>x</code> chia hết cho <code>y</code> nếu <code>y % x == 0</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,3,2,4,3], numsDivide = [9,6,9,3,15]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Phần tử nhỏ nhất trong [2,3,2,4,3] là 2, không chia hết cho mọi phần tử của numsDivide.
Ta dùng 2 lần xóa để xóa các phần tử trong nums bằng 2, khi đó nums = [3,4,3].
Phần tử nhỏ nhất trong [3,4,3] là 3, chia hết cho mọi phần tử của numsDivide.
Có thể chứng minh rằng 2 là số lần xóa ít nhất cần thực hiện.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,3,6], numsDivide = [8,2,6,10]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong>
Ta muốn phần tử nhỏ nhất trong nums chia hết cho mọi phần tử của numsDivide.
Không có cách nào xóa các phần tử khỏi nums để đạt được điều này.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length, numsDivide.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i], numsDivide[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Sau khi xóa, phần tử nhỏ nhất còn lại phải chia hết cho mọi phần tử của $numsDivide$, nên nó là một ước của $x=\gcd(numsDivide)$. $n$ có thể lên tới $10^5$.
>
> Tính $x$, sắp xếp $nums$, rồi trả về chỉ số của ước đầu tiên của $x$. Nếu không có phần tử nào như vậy, đáp án là $-1$.

<!-- thinking:end -->

Nếu một phần tử có thể chia hết cho mọi giá trị trong `numsDivide`, thì nó là một ước của UCLN của chúng, ký hiệu là $x$. Hãy tính $x$, sắp xếp `nums`, rồi trả về chỉ số của ước đầu tiên của $x$.

Độ phức tạp thời gian là $O(m + \log M + n \times \log n)$, trong đó $n$ và $m$ lần lượt là độ dài của `nums` và `numsDivide`, còn $M$ là giá trị lớn nhất trong `numsDivide`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, nums: List[int], numsDivide: List[int]) -> int:
        x = numsDivide[0]
        for v in numsDivide[1:]:
            x = gcd(x, v)
        nums.sort()
        for i, v in enumerate(nums):
            if x % v == 0:
                return i
        return -1
```

#### Java

```java
class Solution {
    public int minOperations(int[] nums, int[] numsDivide) {
        int x = 0;
        for (int v : numsDivide) {
            x = gcd(x, v);
        }
        Arrays.sort(nums);
        for (int i = 0; i < nums.length; ++i) {
            if (x % nums[i] == 0) {
                return i;
            }
        }
        return -1;
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
    int minOperations(vector<int>& nums, vector<int>& numsDivide) {
        int x = 0;
        for (int& v : numsDivide) {
            x = gcd(x, v);
        }
        sort(nums.begin(), nums.end());
        for (int i = 0; i < nums.size(); ++i) {
            if (x % nums[i] == 0) {
                return i;
            }
        }
        return -1;
    }
};
```

#### Go

```go
func minOperations(nums []int, numsDivide []int) int {
	x := 0
	for _, v := range numsDivide {
		x = gcd(x, v)
	}
	sort.Ints(nums)
	for i, v := range nums {
		if x%v == 0 {
			return i
		}
	}
	return -1
}

func gcd(a, b int) int {
	if b == 0 {
		return a
	}
	return gcd(b, a%b)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Toán học + Liệt kê (Không sắp xếp)

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 chỉ sắp xếp để tìm ước hợp lệ nhỏ nhất. Ta có thể quét tuyến tính để tìm giá trị nhỏ nhất $y$ chia hết cho $x$, sau đó đếm các giá trị nhỏ hơn $y$.

<!-- thinking:end -->

Sau khi tính UCLN $x$ của `numsDivide`, hãy quét `nums` để tìm ước hợp lệ nhỏ nhất $y$, rồi đếm số phần tử nhỏ hơn $y$. Không cần sắp xếp.

Độ phức tạp thời gian là $O(m + \log M + n)$, còn độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, nums: List[int], numsDivide: List[int]) -> int:
        x = gcd(*numsDivide)
        y = min((v for v in nums if x % v == 0), default=0)
        return sum(v < y for v in nums) if y else -1
```

#### Java

```java
class Solution {
    public int minOperations(int[] nums, int[] numsDivide) {
        int x = 0;
        for (int v : numsDivide) {
            x = gcd(x, v);
        }
        int y = 1 << 30;
        for (int v : nums) {
            if (x % v == 0) {
                y = Math.min(y, v);
            }
        }
        if (y == 1 << 30) {
            return -1;
        }
        int ans = 0;
        for (int v : nums) {
            if (v < y) {
                ++ans;
            }
        }
        return ans;
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
    int minOperations(vector<int>& nums, vector<int>& numsDivide) {
        int x = 0;
        for (int& v : numsDivide) {
            x = gcd(x, v);
        }
        int y = 1 << 30;
        for (int& v : nums) {
            if (x % v == 0) {
                y = min(y, v);
            }
        }
        if (y == 1 << 30) {
            return -1;
        }
        int ans = 0;
        for (int& v : nums) {
            ans += v < y;
        }
        return ans;
    }
};
```

#### Go

```go
func minOperations(nums []int, numsDivide []int) int {
	x := 0
	for _, v := range numsDivide {
		x = gcd(x, v)
	}
	y := 1 << 30
	for _, v := range nums {
		if x%v == 0 {
			y = min(y, v)
		}
	}
	if y == 1<<30 {
		return -1
	}
	ans := 0
	for _, v := range nums {
		if v < y {
			ans++
		}
	}
	return ans
}

func gcd(a, b int) int {
	if b == 0 {
		return a
	}
	return gcd(b, a%b)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
