---
comments: true
difficulty: Easy
rating: 1190
source: Biweekly Contest 126 Q1
tags:
    - Array
    - Math
---

<!-- problem:start -->

# [3079. Find the Sum of Encrypted Integers](https://leetcode.com/problems/find-the-sum-of-encrypted-integers)

[中文文档](/solution/3000-3099/3079.Find%20the%20Sum%20of%20Encrypted%20Integers/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> chứa các số nguyên <strong>dương</strong>. Ta định nghĩa một hàm <code>encrypt</code> sao cho <code>encrypt(x)</code> thay thế <strong>mọi</strong> chữ số trong <code>x</code> bằng chữ số <strong>lớn nhất</strong> trong <code>x</code>. Ví dụ, <code>encrypt(523) = 555</code> và <code>encrypt(213) = 333</code>.</p>

<p>Trả về <em><strong>tổng </strong>các phần tử sau khi mã hóa</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">nums = [1,2,3]</span></p>

<p><strong>Đầu ra: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">6</span></p>

<p><strong>Giải thích:</strong> Các phần tử sau khi mã hóa là&nbsp;<code>[1,2,3]</code>. Tổng các phần tử sau khi mã hóa là <code>1 + 2 + 3 == 6</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">nums = [10,21,31]</span></p>

<p><strong>Đầu ra: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">66</span></p>

<p><strong>Giải thích:</strong> Các phần tử sau khi mã hóa là <code>[11,22,33]</code>. Tổng các phần tử sau khi mã hóa là <code>11 + 22 + 33 == 66</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 50</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Phép mã hóa thay thế mọi chữ số của một số bằng chữ số lớn nhất của số đó. $n \le 50$ và $x \le 1000$, nên ta mô phỏng từng giá trị.
>
> Trong khi tách từng chữ số, ta theo dõi giá trị lớn nhất và xây dựng $p=1,11,111,\ldots$; giá trị sau khi mã hóa là $mx \cdot p$.
>
> Tính tổng trên toàn bộ mảng là đáp án.

<!-- thinking:end -->

Ta mô phỏng trực tiếp quá trình mã hóa bằng cách định nghĩa một hàm $encrypt(x)$, hàm này thay thế mỗi chữ số trong một số nguyên $x$ bằng chữ số lớn nhất trong $x$. Cách triển khai hàm như sau:

Ta có thể lấy từng chữ số của $x$ bằng cách liên tục lấy phần dư và chia nguyên $x$ cho $10$, đồng thời tìm chữ số lớn nhất, ký hiệu là $mx$. Trong vòng lặp, ta cũng có thể dùng một biến $p$ để lưu số cơ sở của $mx$, tức là $p = 1, 11, 111, \cdots$. Cuối cùng, trả về $mx \times p$.

Độ phức tạp thời gian là $O(n \times \log M)$, trong đó $n$ là độ dài của mảng và $M$ là giá trị lớn nhất trong mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumOfEncryptedInt(self, nums: List[int]) -> int:
        def encrypt(x: int) -> int:
            mx = p = 0
            while x:
                x, v = divmod(x, 10)
                mx = max(mx, v)
                p = p * 10 + 1
            return mx * p

        return sum(encrypt(x) for x in nums)
```

#### Java

```java
class Solution {
    public int sumOfEncryptedInt(int[] nums) {
        int ans = 0;
        for (int x : nums) {
            ans += encrypt(x);
        }
        return ans;
    }

    private int encrypt(int x) {
        int mx = 0, p = 0;
        for (; x > 0; x /= 10) {
            mx = Math.max(mx, x % 10);
            p = p * 10 + 1;
        }
        return mx * p;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int sumOfEncryptedInt(vector<int>& nums) {
        auto encrypt = [&](int x) {
            int mx = 0, p = 0;
            for (; x; x /= 10) {
                mx = max(mx, x % 10);
                p = p * 10 + 1;
            }
            return mx * p;
        };
        int ans = 0;
        for (int x : nums) {
            ans += encrypt(x);
        }
        return ans;
    }
};
```

#### Go

```go
func sumOfEncryptedInt(nums []int) (ans int) {
	encrypt := func(x int) int {
		mx, p := 0, 0
		for ; x > 0; x /= 10 {
			mx = max(mx, x%10)
			p = p*10 + 1
		}
		return mx * p
	}
	for _, x := range nums {
		ans += encrypt(x)
	}
	return
}
```

#### TypeScript

```ts
function sumOfEncryptedInt(nums: number[]): number {
    const encrypt = (x: number): number => {
        let [mx, p] = [0, 0];
        for (; x > 0; x = Math.floor(x / 10)) {
            mx = Math.max(mx, x % 10);
            p = p * 10 + 1;
        }
        return mx * p;
    };
    return nums.reduce((acc, x) => acc + encrypt(x), 0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
