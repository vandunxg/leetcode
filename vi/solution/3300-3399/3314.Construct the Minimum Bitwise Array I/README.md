---
comments: true
difficulty: Easy
rating: 1378
source: Biweekly Contest 141 Q1
tags:
    - Bit Manipulation
    - Array
---

<!-- problem:start -->

# [3314. Construct the Minimum Bitwise Array I](https://leetcode.com/problems/construct-the-minimum-bitwise-array-i)

[中文文档](/solution/3300-3399/3314.Construct%20the%20Minimum%20Bitwise%20Array%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng <code>nums</code> gồm <code>n</code> <span data-keyword="prime-number">số nguyên tố</span>.</p>

<p>Cần xây dựng một mảng <code>ans</code> có độ dài <code>n</code>, sao cho với mỗi chỉ số <code>i</code>, phép <code>OR</code> theo bit của <code>ans[i]</code> và <code>ans[i] + 1</code> bằng <code>nums[i]</code>, tức là <code>ans[i] OR (ans[i] + 1) == nums[i]</code>.</p>

<p>Ngoài ra, cần <strong>tối thiểu hóa</strong> từng giá trị của <code>ans[i]</code> trong mảng kết quả.</p>

<p>Nếu <em>không thể</em> tìm được giá trị của <code>ans[i]</code> thỏa mãn <strong>điều kiện</strong>, hãy đặt <code>ans[i] = -1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,3,5,7]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[-1,1,4,3]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Với <code>i = 0</code>, không có giá trị nào của <code>ans[0]</code> thỏa mãn <code>ans[0] OR (ans[0] + 1) = 2</code>, nên <code>ans[0] = -1</code>.</li>
	<li>Với <code>i = 1</code>, giá trị nhỏ nhất của <code>ans[1]</code> thỏa mãn <code>ans[1] OR (ans[1] + 1) = 3</code> là <code>1</code>, vì <code>1 OR (1 + 1) = 3</code>.</li>
	<li>Với <code>i = 2</code>, giá trị nhỏ nhất của <code>ans[2]</code> thỏa mãn <code>ans[2] OR (ans[2] + 1) = 5</code> là <code>4</code>, vì <code>4 OR (4 + 1) = 5</code>.</li>
	<li>Với <code>i = 3</code>, giá trị nhỏ nhất của <code>ans[3]</code> thỏa mãn <code>ans[3] OR (ans[3] + 1) = 7</code> là <code>3</code>, vì <code>3 OR (3 + 1) = 7</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [11,13,31]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[9,12,15]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Với <code>i = 0</code>, giá trị nhỏ nhất của <code>ans[0]</code> thỏa mãn <code>ans[0] OR (ans[0] + 1) = 11</code> là <code>9</code>, vì <code>9 OR (9 + 1) = 11</code>.</li>
	<li>Với <code>i = 1</code>, giá trị nhỏ nhất của <code>ans[1]</code> thỏa mãn <code>ans[1] OR (ans[1] + 1) = 13</code> là <code>12</code>, vì <code>12 OR (12 + 1) = 13</code>.</li>
	<li>Với <code>i = 2</code>, giá trị nhỏ nhất của <code>ans[2]</code> thỏa mãn <code>ans[2] OR (ans[2] + 1) = 31</code> là <code>15</code>, vì <code>15 OR (15 + 1) = 31</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>2 &lt;= nums[i] &lt;= 1000</code></li>
	<li><code>nums[i]</code> là một số nguyên tố.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> $a \lor (a+1)$ luôn bật bit 0 thấp nhất của $a$, nên kết quả luôn là số lẻ. Số nguyên tố chẵn duy nhất ở đây là $2$, và nó không có lời giải.
>
> Với $x$ lẻ, giá trị $a$ nhỏ nhất nhận được bằng cách tắt bit ngay trước bit 0 thấp nhất của $x$, đồng thời vẫn thỏa mãn $a \lor (a+1)=x$.
>
> Duyệt các bit từ thấp đến cao, lấy bit 0 đầu tiên ở vị trí $i$, rồi đặt $a = x \oplus 2^{i-1}$. Với các ràng buộc của bài, việc duyệt 32 bit là đủ nhanh.

<!-- thinking:end -->

Với một số nguyên $a$, kết quả của $a \lor (a + 1)$ luôn là số lẻ. Do đó, nếu $\text{nums[i]}$ là số chẵn thì $\text{ans}[i]$ không tồn tại, và ta trả về trực tiếp $-1$. Trong bài này, $\textit{nums}[i]$ là một số nguyên tố, nên để kiểm tra nó có chẵn hay không, ta chỉ cần kiểm tra nó có bằng $2$ hay không.

Nếu $\text{nums[i]}$ là số lẻ, giả sử $\text{nums[i]} = \text{0b1101101}$. Vì $a \lor (a + 1) = \text{nums[i]}$, điều này tương đương với việc đổi bit $0$ cuối cùng của $a$ thành $1$. Để tìm $a$, ta cần đổi bit ngay sau bit $0$ cuối cùng trong $\text{nums[i]}$ thành $0$. Ta bắt đầu duyệt từ bit ít quan trọng nhất (chỉ số $1$) và tìm bit $0$ đầu tiên. Nếu bit đó ở vị trí $i$, ta đổi bit thứ $(i - 1)$ của $\text{nums[i]}$ thành $1$, tức là $\text{ans}[i] = \text{nums[i]} \oplus 2^{i - 1}$.

Sau khi duyệt qua tất cả phần tử trong $\text{nums}$, ta thu được đáp án.

Độ phức tạp thời gian là $O(n \times \log M)$, trong đó $n$ và $M$ lần lượt là độ dài mảng $\text{nums}$ và giá trị lớn nhất trong mảng. Không tính phần bộ nhớ dành cho mảng đáp án, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minBitwiseArray(self, nums: List[int]) -> List[int]:
        ans = []
        for x in nums:
            if x == 2:
                ans.append(-1)
            else:
                for i in range(1, 32):
                    if x >> i & 1 ^ 1:
                        ans.append(x ^ 1 << (i - 1))
                        break
        return ans
```

#### Java

```java
class Solution {
    public int[] minBitwiseArray(List<Integer> nums) {
        int n = nums.size();
        int[] ans = new int[n];
        for (int i = 0; i < n; ++i) {
            int x = nums.get(i);
            if (x == 2) {
                ans[i] = -1;
            } else {
                for (int j = 1; j < 32; ++j) {
                    if ((x >> j & 1) == 0) {
                        ans[i] = x ^ 1 << (j - 1);
                        break;
                    }
                }
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
    vector<int> minBitwiseArray(vector<int>& nums) {
        vector<int> ans;
        for (int x : nums) {
            if (x == 2) {
                ans.push_back(-1);
            } else {
                for (int i = 1; i < 32; ++i) {
                    if (x >> i & 1 ^ 1) {
                        ans.push_back(x ^ 1 << (i - 1));
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
func minBitwiseArray(nums []int) (ans []int) {
	for _, x := range nums {
		if x == 2 {
			ans = append(ans, -1)
		} else {
			for i := 1; i < 32; i++ {
				if x>>i&1 == 0 {
					ans = append(ans, x^1<<(i-1))
					break
				}
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function minBitwiseArray(nums: number[]): number[] {
    const ans: number[] = [];
    for (const x of nums) {
        if (x === 2) {
            ans.push(-1);
        } else {
            for (let i = 1; i < 32; ++i) {
                if (((x >> i) & 1) ^ 1) {
                    ans.push(x ^ (1 << (i - 1)));
                    break;
                }
            }
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_bitwise_array(nums: Vec<i32>) -> Vec<i32> {
        let mut ans = Vec::with_capacity(nums.len());
        for x in nums {
            if x == 2 {
                ans.push(-1);
            } else {
                for i in 1..32 {
                    if (((x >> i) & 1) ^ 1) == 1 {
                        ans.push(x ^ (1 << (i - 1)));
                        break;
                    }
                }
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
