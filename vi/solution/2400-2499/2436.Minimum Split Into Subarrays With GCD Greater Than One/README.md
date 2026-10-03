---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - Math
    - Dynamic Programming
    - Number Theory
    - Euclidean Algorithm
---

<!-- problem:start -->

# [2436. Minimum Split Into Subarrays With GCD Greater Than One 🔒](https://leetcode.com/problems/minimum-split-into-subarrays-with-gcd-greater-than-one)

[中文文档](/solution/2400-2499/2436.Minimum%20Split%20Into%20Subarrays%20With%20GCD%20Greater%20Than%20One/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <code>nums</code> gồm các số nguyên dương.</p>

<p>Hãy chia mảng thành <strong>một hoặc nhiều</strong> mảng con không giao nhau sao cho:</p>

<ul>
	<li>Mỗi phần tử của mảng thuộc về <strong>đúng một</strong> mảng con, và</li>
	<li><strong>GCD</strong> của các phần tử trong mỗi mảng con lớn hơn <code>1</code>.</li>
</ul>

<p>Trả về <em>số lượng mảng con nhỏ nhất có thể thu được sau khi chia</em>.</p>

<p><strong>Lưu ý</strong> rằng:</p>

<ul>
	<li><strong>GCD</strong> của một mảng con là số nguyên dương lớn nhất chia hết cho tất cả các phần tử trong mảng con.</li>
	<li>Một <strong>mảng con</strong> là một phần liên tiếp của mảng.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [12,6,3,14,8]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ta có thể chia mảng thành các mảng con: [12,6,3] và [14,8].
- GCD của 12, 6 và 3 là 3, lớn hơn 1.
- GCD của 14 và 8 là 2, lớn hơn 1.
Có thể chứng minh rằng nếu chia thành một mảng con thì GCD = 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,12,6,14]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Ta có thể chia mảng thành chỉ một mảng con, chính là toàn bộ mảng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 2000</code></li>
	<li><code>2 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi phần cần có GCD lớn hơn $1$, và ta muốn số phần ít nhất, nên mở rộng một phần cho đến khi GCD trở thành $1$.
>
> Duy trì GCD hiện tại $g$. Khi $gcd(g,x)=1$, bắt đầu một phần mới tại $x$. Cắt muộn hơn không giúp ích: thêm phần tử chỉ làm GCD giảm.

<!-- thinking:end -->

Với mỗi phần tử trong mảng, nếu ước chung lớn nhất (gcd) của nó với phần tử trước đó bằng $1$, thì phần tử đó phải là phần tử đầu tiên của một mảng con mới. Nếu không, nó có thể được đặt cùng mảng con với các phần tử trước đó.

Trước tiên, ta khởi tạo biến $g$, biểu diễn gcd của mảng con hiện tại. Ban đầu, $g=0$ và biến đáp án $ans=1$.

Tiếp theo, ta duyệt mảng từ đầu đến cuối, duy trì gcd $g$ của mảng con hiện tại. Nếu gcd của phần tử hiện tại $x$ và $g$ bằng $1$, ta cần đặt phần tử hiện tại làm phần tử đầu tiên của một mảng con mới. Do đó, đáp án tăng thêm $1$ và $g$ được cập nhật thành $x$. Ngược lại, phần tử hiện tại có thể được đặt cùng mảng con với các phần tử trước đó. Tiếp tục duyệt cho đến khi kết thúc.

Độ phức tạp thời gian là $O(n \times \log m)$, trong đó $n$ và $m$ lần lượt là độ dài mảng và giá trị lớn nhất trong mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumSplits(self, nums: List[int]) -> int:
        ans, g = 1, 0
        for x in nums:
            g = gcd(g, x)
            if g == 1:
                ans += 1
                g = x
        return ans
```

#### Java

```java
class Solution {
    public int minimumSplits(int[] nums) {
        int ans = 1, g = 0;
        for (int x : nums) {
            g = gcd(g, x);
            if (g == 1) {
                ++ans;
                g = x;
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
    int minimumSplits(vector<int>& nums) {
        int ans = 1, g = 0;
        for (int x : nums) {
            g = gcd(g, x);
            if (g == 1) {
                ++ans;
                g = x;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minimumSplits(nums []int) int {
	ans, g := 1, 0
	for _, x := range nums {
		g = gcd(g, x)
		if g == 1 {
			ans++
			g = x
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

#### TypeScript

```ts
function minimumSplits(nums: number[]): number {
    let ans = 1;
    let g = 0;
    for (const x of nums) {
        g = gcd(g, x);
        if (g == 1) {
            ++ans;
            g = x;
        }
    }
    return ans;
}

function gcd(a: number, b: number): number {
    return b ? gcd(b, a % b) : a;
}
```

#### Rust

```rust
impl Solution {
    pub fn minimum_splits(nums: Vec<i32>) -> i32 {
        let mut ans = 1;
        let mut g = 0;
        for &x in &nums {
            g = Self::gcd(g, x);
            if g == 1 {
                ans += 1;
                g = x;
            }
        }
        ans
    }

    fn gcd(a: i32, b: i32) -> i32 {
        if b == 0 {
            a
        } else {
            Self::gcd(b, a % b)
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
