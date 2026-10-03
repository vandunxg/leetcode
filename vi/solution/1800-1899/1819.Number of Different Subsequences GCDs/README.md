---
comments: true
difficulty: Hard
rating: 2539
source: Weekly Contest 235 Q4
tags:
    - Array
    - Math
    - Counting
    - Greatest Common Divisor
    - Number Theory
    - Euclidean Algorithm
---

<!-- problem:start -->

# [1819. Number of Different Subsequences GCDs](https://leetcode.com/problems/number-of-different-subsequences-gcds)

[中文文档](/solution/1800-1899/1819.Number%20of%20Different%20Subsequences%20GCDs/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng <code>nums</code> gồm các số nguyên dương.</p>

<p><strong>GCD</strong> của một dãy số được định nghĩa là số nguyên lớn nhất chia hết cho <strong>tất cả</strong> các số trong dãy.</p>

<ul>
	<li>Ví dụ, GCD của dãy <code>[4,6,16]</code> là <code>2</code>.</li>
</ul>

<p><strong>Dãy con</strong> của một mảng là một dãy có thể được tạo thành bằng cách loại bỏ một số phần tử (có thể không loại bỏ phần tử nào) khỏi mảng.</p>

<ul>
	<li>Ví dụ, <code>[2,5,10]</code> là một dãy con của <code>[1,2,1,<strong><u>2</u></strong>,4,1,<u><strong>5</strong></u>,<u><strong>10</strong></u>]</code>.</li>
</ul>

<p>Trả về <em><strong>số lượng</strong> GCD <strong>khác nhau</strong> trong tất cả các dãy con <strong>không rỗng</strong> của</em> <code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1800-1899/1819.Number%20of%20Different%20Subsequences%20GCDs/images/image-1.png" style="width: 149px; height: 309px;" />
<pre>
<strong>Đầu vào:</strong> nums = [6,10,3]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Hình minh họa tất cả các dãy con không rỗng và GCD tương ứng.
Các GCD khác nhau là 6, 10, 3, 2 và 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,15,40,5,6]
<strong>Đầu ra:</strong> 7
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 2 * 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê + Toán học

<!-- thinking:start -->

> **Tư duy**
>
> GCD của mọi dãy con không vượt quá giá trị lớn nhất $mx$ của mảng, nhưng có tới $2^n$ dãy con và $n\le 10^5$, nên không thể liệt kê chúng.
>
> Một giá trị $x$ là GCD của một dãy con nào đó khi và chỉ khi GCD của các bội của $x$ xuất hiện trong mảng bằng đúng $x$. Với mỗi $x\in[1,mx]$, ta duyệt các bội của nó, cập nhật GCD hiện tại và đếm $x$ ngay khi GCD bằng $x$. Tổng số bội cần duyệt là $O(mx\log mx)$.

<!-- thinking:end -->

Với mọi dãy con của mảng $nums$, ước chung lớn nhất (GCD) của chúng không vượt quá giá trị lớn nhất $mx$ trong mảng.

Vì vậy, ta có thể liệt kê từng số $x$ trong $[1,.. mx]$ và xác định xem $x$ có phải là GCD của một dãy con trong mảng $nums$ hay không. Nếu đúng, ta tăng đáp án lên một.

Ta chuyển bài toán thành việc xác định liệu $x$ có phải là GCD của một dãy con trong mảng $nums$ hay không. Ta có thể làm điều này bằng cách liệt kê các bội $y$ của $x$ và kiểm tra xem $y$ có xuất hiện trong mảng $nums$ hay không. Nếu $y$ xuất hiện, ta tính GCD $g$ của $y$ trong mảng $nums$. Nếu $g = x$ thì $x$ là GCD của một dãy con trong mảng $nums$.

Độ phức tạp thời gian là $O(n + M \times \log M)$ và độ phức tạp không gian là $O(M)$. Trong đó, $n$ và $M$ lần lượt là độ dài mảng $nums$ và giá trị lớn nhất trong mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countDifferentSubsequenceGCDs(self, nums: List[int]) -> int:
        mx = max(nums)
        vis = set(nums)
        ans = 0
        for x in range(1, mx + 1):
            g = 0
            for y in range(x, mx + 1, x):
                if y in vis:
                    g = gcd(g, y)
                    if g == x:
                        ans += 1
                        break
        return ans
```

#### Java

```java
class Solution {
    public int countDifferentSubsequenceGCDs(int[] nums) {
        int mx = Arrays.stream(nums).max().getAsInt();
        boolean[] vis = new boolean[mx + 1];
        for (int x : nums) {
            vis[x] = true;
        }
        int ans = 0;
        for (int x = 1; x <= mx; ++x) {
            int g = 0;
            for (int y = x; y <= mx; y += x) {
                if (vis[y]) {
                    g = gcd(g, y);
                    if (x == g) {
                        ++ans;
                        break;
                    }
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
    int countDifferentSubsequenceGCDs(vector<int>& nums) {
        int mx = *max_element(nums.begin(), nums.end());
        vector<bool> vis(mx + 1);
        for (int& x : nums) {
            vis[x] = true;
        }
        int ans = 0;
        for (int x = 1; x <= mx; ++x) {
            int g = 0;
            for (int y = x; y <= mx; y += x) {
                if (vis[y]) {
                    g = gcd(g, y);
                    if (g == x) {
                        ++ans;
                        break;
                    }
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countDifferentSubsequenceGCDs(nums []int) (ans int) {
	mx := slices.Max(nums)
	vis := make([]bool, mx+1)
	for _, x := range nums {
		vis[x] = true
	}
	for x := 1; x <= mx; x++ {
		g := 0
		for y := x; y <= mx; y += x {
			if vis[y] {
				g = gcd(g, y)
				if g == x {
					ans++
					break
				}
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

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
