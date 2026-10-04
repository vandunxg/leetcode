---
comments: true
difficulty: Medium
rating: 1883
source: Biweekly Contest 154 Q3
tags:
    - Bit Manipulation
    - Array
    - Math
    - Enumeration
---

<!-- problem:start -->

# [3514. Number of Unique XOR Triplets II](https://leetcode.com/problems/number-of-unique-xor-triplets-ii)

[中文文档](/solution/3500-3599/3514.Number%20of%20Unique%20XOR%20Triplets%20II/README.md)

## Mô tả

<!-- description:start -->

<p data-end="261" data-start="147">Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Một <strong>bộ ba XOR</strong> được định nghĩa là XOR của ba phần tử <code>nums[i] XOR nums[j] XOR nums[k]</code> với <code>i &lt;= j &lt;= k</code>.</p>

<p>Trả về số lượng giá trị XOR <strong>khác nhau</strong> từ tất cả các bộ ba <code>(i, j, k)</code> có thể có.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p data-end="158" data-start="101">Các giá trị XOR của những bộ ba có thể có là:</p>

<ul data-end="280" data-start="159">
	<li data-end="188" data-start="159"><code>(0, 0, 0) &rarr; 1 XOR 1 XOR 1 = 1</code></li>
	<li data-end="218" data-start="189"><code>(0, 0, 1) &rarr; 1 XOR 1 XOR 3 = 3</code></li>
	<li data-end="248" data-start="219"><code>(0, 1, 1) &rarr; 1 XOR 3 XOR 3 = 1</code></li>
	<li data-end="280" data-start="249"><code>(1, 1, 1) &rarr; 3 XOR 3 XOR 3 = 3</code></li>
 </ul>

<p data-end="343" data-start="282">Các giá trị XOR khác nhau là <code data-end="316" data-start="308">{1, 3}</code>. Vì vậy, kết quả là 2.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [6,7,8,9]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các giá trị XOR của những bộ ba có thể có là <code data-end="275" data-start="267">{6, 7, 8, 9}</code>. Vì vậy, kết quả là 4.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1500</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 1500</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Mảng không còn là một hoán vị, nên không có công thức đóng. XOR của hai giá trị bất kỳ nhỏ hơn $2M$ với $M = \max(\textit{nums})$, vì vậy chỉ cần một mảng boolean.
>
> Đánh dấu mọi giá trị $a \oplus b$, sau đó XOR từng giá trị đã đánh dấu với một phần tử thứ ba vào $s$ và đếm các phần tử khác 0. Do XOR có tính giao hoán, thứ tự chỉ số không còn quan trọng.

<!-- thinking:end -->

Với các chỉ số thỏa mãn $i \le j \le k$, cùng một chỉ số có thể được chọn nhiều lần, và phép XOR có tính giao hoán. Do đó, đáp án bằng số lượng giá trị XOR phân biệt có thể nhận được khi chọn bất kỳ ba phần tử nào trong mảng (cho phép lặp lại).

Gọi $M = \max(\textit{nums})$. XOR của hai số nguyên không âm không lớn hơn $M$ luôn nhỏ hơn $2M$, nên có thể dùng một mảng boolean có độ dài $2M$ để đánh dấu.

Trước tiên, liệt kê tất cả các cặp $(a, b)$ và đánh dấu $a \oplus b$ trong mảng $\textit{st}$. Sau đó, với mỗi giá trị XOR của một cặp đã xuất hiện $\textit{ab}$ và mỗi phần tử thứ ba $c$, đánh dấu $\textit{ab} \oplus c$ trong mảng $s$. Cuối cùng, đếm số phần tử khác 0 trong $s$.

Độ phức tạp thời gian là $O(n^2 + M \cdot n)$, và độ phức tạp không gian là $O(M)$, trong đó $n$ là độ dài của mảng và $M$ là giá trị lớn nhất trong mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def uniqueXorTriplets(self, nums: List[int]) -> int:
        mx = max(nums) << 1
        st = [False] * mx
        for a in nums:
            for b in nums:
                st[a ^ b] = True
        s = [0] * mx
        for ab in range(mx):
            if st[ab]:
                for c in nums:
                    s[ab ^ c] = 1
        return sum(s)
```

#### Java

```java
class Solution {
    public int uniqueXorTriplets(int[] nums) {
        int mx = 0;
        for (int x : nums) {
            mx = Math.max(mx, x);
        }
        mx <<= 1;

        boolean[] st = new boolean[mx];
        for (int a : nums) {
            for (int b : nums) {
                st[a ^ b] = true;
            }
        }

        int[] s = new int[mx];
        for (int ab = 0; ab < mx; ab++) {
            if (st[ab]) {
                for (int c : nums) {
                    s[ab ^ c] = 1;
                }
            }
        }

        int ans = 0;
        for (int v : s) {
            ans += v;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int uniqueXorTriplets(vector<int>& nums) {
        int mx = ranges::max(nums) << 1;

        vector<bool> st(mx, false);
        for (int a : nums) {
            for (int b : nums) {
                st[a ^ b] = true;
            }
        }

        vector<int> s(mx, 0);
        for (int ab = 0; ab < mx; ab++) {
            if (st[ab]) {
                for (int c : nums) {
                    s[ab ^ c] = 1;
                }
            }
        }

        return accumulate(s.begin(), s.end(), 0);
    }
};
```

#### Go

```go
func uniqueXorTriplets(nums []int) int {
	mx := slices.Max(nums) << 1

	st := make([]bool, mx)
	for _, a := range nums {
		for _, b := range nums {
			st[a^b] = true
		}
	}

	s := make([]int, mx)
	for ab := 0; ab < mx; ab++ {
		if st[ab] {
			for _, c := range nums {
				s[ab^c] = 1
			}
		}
	}

	ans := 0
	for _, v := range s {
		ans += v
	}
	return ans
}
```

#### TypeScript

```ts
function uniqueXorTriplets(nums: number[]): number {
    const mx = Math.max(...nums) << 1;

    const st = new Array<boolean>(mx).fill(false);
    for (const a of nums) {
        for (const b of nums) {
            st[a ^ b] = true;
        }
    }

    const s = new Array<number>(mx).fill(0);
    for (let ab = 0; ab < mx; ab++) {
        if (st[ab]) {
            for (const c of nums) {
                s[ab ^ c] = 1;
            }
        }
    }

    let ans = 0;
    for (const v of s) {
        ans += v;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
