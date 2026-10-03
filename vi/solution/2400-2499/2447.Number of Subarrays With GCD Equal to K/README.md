---
comments: true
difficulty: Medium
rating: 1602
source: Weekly Contest 316 Q2
tags:
    - Array
    - Math
    - Number Theory
---

<!-- problem:start -->

# [2447. Number of Subarrays With GCD Equal to K](https://leetcode.com/problems/number-of-subarrays-with-gcd-equal-to-k)

[中文文档](/solution/2400-2499/2447.Number%20of%20Subarrays%20With%20GCD%20Equal%20to%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>, hãy trả về <em>số lượng <strong>mảng con</strong> của </em><code>nums</code><em> mà ước chung lớn nhất của các phần tử trong mảng con bằng </em><code>k</code>.</p>

<p><strong>Mảng con</strong> là một dãy phần tử liên tiếp, không rỗng trong một mảng.</p>

<p><strong>Ước chung lớn nhất của một mảng</strong> là số nguyên lớn nhất chia hết cho tất cả các phần tử trong mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [9,3,1,2,6,3], k = 3
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Các mảng con của nums mà 3 là ước chung lớn nhất của tất cả phần tử là:
- [9,<u><strong>3</strong></u>,1,2,6,3]
- [9,3,1,2,6,<u><strong>3</strong></u>]
- [<u><strong>9,3</strong></u>,1,2,6,3]
- [9,3,1,2,<u><strong>6,3</strong></u>]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4], k = 7
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có mảng con nào của nums có 7 là ước chung lớn nhất của tất cả phần tử.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i], k &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt trực tiếp

<!-- thinking:start -->

> **Tư duy**
>
> Với $n\le 10^3$, ta cố định đầu trái $i$ rồi mở rộng đầu phải, đồng thời duy trì GCD hiện tại. GCD không tăng, nên vòng lặp kép kết hợp với phép tính $gcd$ vẫn đáp ứng được giới hạn. Mỗi khi $g=k$ thì tăng đáp án.

<!-- thinking:end -->

Ta có thể duyệt $nums[i]$ làm đầu trái của mảng con, sau đó duyệt $nums[j]$ làm đầu phải của mảng con, với $i \le j$. Trong quá trình duyệt đầu phải, ta dùng biến $g$ để duy trì ước chung lớn nhất của mảng con hiện tại. Mỗi khi duyệt một đầu phải mới, ta cập nhật ước chung lớn nhất theo công thức $g = \gcd(g, nums[j])$. Nếu $g=k$, ước chung lớn nhất của mảng con hiện tại bằng $k$, ta tăng đáp án lên $1$.

Sau khi duyệt xong, trả về đáp án.

Độ phức tạp thời gian là $O(n \times (n + \log M))$, trong đó $n$ và $M$ lần lượt là độ dài mảng $nums$ và giá trị lớn nhất trong mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def subarrayGCD(self, nums: List[int], k: int) -> int:
        ans = 0
        for i in range(len(nums)):
            g = 0
            for x in nums[i:]:
                g = gcd(g, x)
                ans += g == k
        return ans
```

#### Java

```java
class Solution {
    public int subarrayGCD(int[] nums, int k) {
        int n = nums.length;
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            int g = 0;
            for (int j = i; j < n; ++j) {
                g = gcd(g, nums[j]);
                if (g == k) {
                    ++ans;
                }
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
    int subarrayGCD(vector<int>& nums, int k) {
        int n = nums.size();
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            int g = 0;
            for (int j = i; j < n; ++j) {
                g = gcd(g, nums[j]);
                ans += g == k;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func subarrayGCD(nums []int, k int) (ans int) {
	for i := range nums {
		g := 0
		for _, x := range nums[i:] {
			g = gcd(g, x)
			if g == k {
				ans++
			}
		}
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
function subarrayGCD(nums: number[], k: number): number {
    let ans = 0;
    const n = nums.length;
    for (let i = 0; i < n; ++i) {
        let g = 0;
        for (let j = i; j < n; ++j) {
            g = gcd(g, nums[j]);
            if (g === k) {
                ++ans;
            }
        }
    }
    return ans;
}

function gcd(a: number, b: number): number {
    return b === 0 ? a : gcd(b, a % b);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
