---
comments: true
difficulty: Easy
rating: 1151
source: Weekly Contest 317 Q1
tags:
    - Array
    - Math
---

<!-- problem:start -->

# [2455. Average Value of Even Numbers That Are Divisible by Three](https://leetcode.com/problems/average-value-of-even-numbers-that-are-divisible-by-three)

[中文文档](/solution/2400-2499/2455.Average%20Value%20of%20Even%20Numbers%20That%20Are%20Divisible%20by%20Three/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> gồm các số nguyên <strong>dương</strong>, hãy trả về <em>giá trị trung bình của tất cả các số nguyên chẵn chia hết cho</em> <code>3</code><i>.</i></p>

<p>Lưu ý rằng <strong>giá trị trung bình</strong> của <code>n</code> phần tử là <strong>tổng</strong> của <code>n</code> phần tử đó chia cho <code>n</code> và <strong>làm tròn xuống</strong> đến số nguyên gần nhất.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,6,10,12,15]
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong> 6 và 12 là các số chẵn chia hết cho 3. (6 + 12) / 2 = 9.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,4,7,10]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có số nào thỏa mãn yêu cầu, vì vậy trả về 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Với $n\le 1000$, số chẵn và chia hết cho ba cũng chính là số chia hết cho $6$. Cộng các giá trị đó và đếm số lượng; nếu số lượng bằng 0 thì trả về $0$, nếu không thì chia lấy phần nguyên.

<!-- thinking:end -->

Ta nhận thấy một số chẵn chia hết cho $3$ chắc chắn là bội của $6$. Vì vậy, ta chỉ cần duyệt qua mảng, tính tổng và số lượng của tất cả các bội của $6$, sau đó tính giá trị trung bình.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def averageValue(self, nums: List[int]) -> int:
        s = n = 0
        for x in nums:
            if x % 6 == 0:
                s += x
                n += 1
        return 0 if n == 0 else s // n
```

#### Java

```java
class Solution {
    public int averageValue(int[] nums) {
        int s = 0, n = 0;
        for (int x : nums) {
            if (x % 6 == 0) {
                s += x;
                ++n;
            }
        }
        return n == 0 ? 0 : s / n;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int averageValue(vector<int>& nums) {
        int s = 0, n = 0;
        for (int x : nums) {
            if (x % 6 == 0) {
                s += x;
                ++n;
            }
        }
        return n == 0 ? 0 : s / n;
    }
};
```

#### Go

```go
func averageValue(nums []int) int {
	var s, n int
	for _, x := range nums {
		if x%6 == 0 {
			s += x
			n++
		}
	}
	if n == 0 {
		return 0
	}
	return s / n
}
```

#### TypeScript

```ts
function averageValue(nums: number[]): number {
    let s = 0;
    let n = 0;
    for (const x of nums) {
        if (x % 6 === 0) {
            s += x;
            ++n;
        }
    }
    return n === 0 ? 0 : ~~(s / n);
}
```

#### Rust

```rust
impl Solution {
    pub fn average_value(nums: Vec<i32>) -> i32 {
        let mut s = 0;
        let mut n = 0;
        for x in nums.iter() {
            if x % 6 == 0 {
                s += x;
                n += 1;
            }
        }
        if n == 0 {
            return 0;
        }
        s / n
    }
}
```

#### C

```c
int averageValue(int* nums, int numsSize) {
    int s = 0, n = 0;
    for (int i = 0; i < numsSize; ++i) {
        if (nums[i] % 6 == 0) {
            s += nums[i];
            ++n;
        }
    }
    return n == 0 ? 0 : s / n;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 đã cộng dồn các giá trị thỏa mãn $x\bmod 6=0$. Lọc chúng vào một danh sách rồi chia tổng cho độ dài danh sách cho cùng một giá trị trung bình, nhưng tốn thêm bộ nhớ để cấp phát.

<!-- thinking:end -->

<!-- tabs:start -->

#### Rust

```rust
impl Solution {
    pub fn average_value(nums: Vec<i32>) -> i32 {
        let filtered_nums: Vec<i32> = nums.iter().cloned().filter(|&n| n % 6 == 0).collect();

        if filtered_nums.is_empty() {
            return 0;
        }

        filtered_nums.iter().sum::<i32>() / (filtered_nums.len() as i32)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
