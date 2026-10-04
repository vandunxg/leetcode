---
comments: true
difficulty: Easy
rating: 1301
source: Weekly Contest 351 Q1
tags:
    - Array
    - Hash Table
    - Math
    - Counting
    - Number Theory
---

<!-- problem:start -->

# [2748. Number of Beautiful Pairs](https://leetcode.com/problems/number-of-beautiful-pairs)

[中文文档](/solution/2700-2799/2748.Number%20of%20Beautiful%20Pairs/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>nums</code>. Một cặp chỉ số <code>i</code>, <code>j</code> thỏa mãn <code>0 &lt;=&nbsp;i &lt; j &lt; nums.length</code> được gọi là đẹp nếu <strong>chữ số đầu tiên</strong> của <code>nums[i]</code> và <strong>chữ số cuối cùng</strong> của <code>nums[j]</code> là <strong>nguyên tố cùng nhau</strong>.</p>

<p>Trả về <em>tổng số cặp đẹp trong </em><code>nums</code>.</p>

<p>Hai số nguyên <code>x</code> và <code>y</code> là <strong>nguyên tố cùng nhau</strong> nếu không có số nguyên nào lớn hơn 1 chia hết cho cả hai số. Nói cách khác, <code>x</code> và <code>y</code> nguyên tố cùng nhau khi <code>gcd(x, y) == 1</code>, trong đó <code>gcd(x, y)</code> là <strong>ước chung lớn nhất</strong> của <code>x</code> và <code>y</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,5,1,4]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Có 5 cặp đẹp trong nums:
Khi i = 0 và j = 1: chữ số đầu tiên của nums[0] là 2, còn chữ số cuối cùng của nums[1] là 5. Ta có thể xác nhận rằng 2 và 5 nguyên tố cùng nhau, vì gcd(2,5) == 1.
Khi i = 0 và j = 2: chữ số đầu tiên của nums[0] là 2, còn chữ số cuối cùng của nums[2] là 1. Đúng vậy, gcd(2,1) == 1.
Khi i = 1 và j = 2: chữ số đầu tiên của nums[1] là 5, còn chữ số cuối cùng của nums[2] là 1. Đúng vậy, gcd(5,1) == 1.
Khi i = 1 và j = 3: chữ số đầu tiên của nums[1] là 5, còn chữ số cuối cùng của nums[3] là 4. Đúng vậy, gcd(5,4) == 1.
Khi i = 2 và j = 3: chữ số đầu tiên của nums[2] là 1, còn chữ số cuối cùng của nums[3] là 4. Đúng vậy, gcd(1,4) == 1.
Do đó, ta trả về 5.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [11,21,12]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Có 2 cặp đẹp:
Khi i = 0 và j = 1: chữ số đầu tiên của nums[0] là 1, còn chữ số cuối cùng của nums[1] là 1. Đúng vậy, gcd(1,1) == 1.
Khi i = 0 và j = 2: chữ số đầu tiên của nums[0] là 1, còn chữ số cuối cùng của nums[2] là 2. Đúng vậy, gcd(1,2) == 1.
Do đó, ta trả về 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 9999</code></li>
	<li><code>nums[i] % 10 != 0</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các cặp chỉ số mà chữ số đầu tiên và chữ số cuối cùng nguyên tố cùng nhau. Với $n\le 100$, một vòng lặp kép là đủ nhanh, nhưng nó phải đọc lại chữ số đầu tiên của mọi phần tử bên trái.
>
> Duyệt từ trái sang phải. Một bộ đếm có độ dài $10$ lưu các chữ số đầu tiên đã gặp. Với chữ số cuối cùng hiện tại, cộng số lượng các chữ số đầu tiên nguyên tố cùng nhau với nó, sau đó tăng số lượng chữ số đầu tiên hiện tại.

<!-- thinking:end -->

Ta có thể sử dụng một mảng $\textit{cnt}$ có độ dài $10$ để ghi lại số lần xuất hiện của chữ số đầu tiên trong mỗi số.

Duyệt qua mảng $\textit{nums}$. Với mỗi số $x$, ta duyệt qua từng chữ số $y$ từ $0$ đến $9$. Nếu $\textit{cnt}[y]$ khác $0$ và $\textit{gcd}(x \mod 10, y) = 1$, ta tăng đáp án thêm $\textit{cnt}[y]$. Sau đó, tăng số lượng chữ số đầu tiên của $x$ lên $1$.

Sau khi duyệt xong, trả về đáp án.

Độ phức tạp thời gian là $O(n \times (k + \log M))$, còn độ phức tạp không gian là $O(k + \log M)$. Trong đó, $n$ là độ dài của mảng $\textit{nums}$, còn $k$ và $M$ lần lượt biểu thị số lượng các số phân biệt và giá trị lớn nhất trong mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countBeautifulPairs(self, nums: List[int]) -> int:
        cnt = [0] * 10
        ans = 0
        for x in nums:
            for y in range(10):
                if cnt[y] and gcd(x % 10, y) == 1:
                    ans += cnt[y]
            cnt[int(str(x)[0])] += 1
        return ans
```

#### Java

```java
class Solution {
    public int countBeautifulPairs(int[] nums) {
        int[] cnt = new int[10];
        int ans = 0;
        for (int x : nums) {
            for (int y = 0; y < 10; ++y) {
                if (cnt[y] > 0 && gcd(x % 10, y) == 1) {
                    ans += cnt[y];
                }
            }
            while (x > 9) {
                x /= 10;
            }
            ++cnt[x];
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
    int countBeautifulPairs(vector<int>& nums) {
        int cnt[10]{};
        int ans = 0;
        for (int x : nums) {
            for (int y = 0; y < 10; ++y) {
                if (cnt[y] && gcd(x % 10, y) == 1) {
                    ans += cnt[y];
                }
            }
            while (x > 9) {
                x /= 10;
            }
            ++cnt[x];
        }
        return ans;
    }
};
```

#### Go

```go
func countBeautifulPairs(nums []int) (ans int) {
	cnt := [10]int{}
	for _, x := range nums {
		for y := 0; y < 10; y++ {
			if cnt[y] > 0 && gcd(x%10, y) == 1 {
				ans += cnt[y]
			}
		}
		for x > 9 {
			x /= 10
		}
		cnt[x]++
	}
	return
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
function countBeautifulPairs(nums: number[]): number {
    const cnt: number[] = Array(10).fill(0);
    let ans = 0;
    for (let x of nums) {
        for (let y = 0; y < 10; ++y) {
            if (cnt[y] > 0 && gcd(x % 10, y) === 1) {
                ans += cnt[y];
            }
        }
        while (x > 9) {
            x = Math.floor(x / 10);
        }
        ++cnt[x];
    }
    return ans;
}

function gcd(a: number, b: number): number {
    if (b === 0) {
        return a;
    }
    return gcd(b, a % b);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
