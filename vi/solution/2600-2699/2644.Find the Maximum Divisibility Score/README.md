---
comments: true
difficulty: Easy
rating: 1257
source: Weekly Contest 341 Q2
tags:
    - Array
---

<!-- problem:start -->

# [2644. Find the Maximum Divisibility Score](https://leetcode.com/problems/find-the-maximum-divisibility-score)

[中文文档](/solution/2600-2699/2644.Find%20the%20Maximum%20Divisibility%20Score/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng số nguyên <code>nums</code> và <code>divisors</code>.</p>

<p><strong>Điểm chia hết</strong> của <code>divisors[i]</code> là số lượng chỉ số <code>j</code> sao cho <code>nums[j]</code> chia hết cho <code>divisors[i]</code>.</p>

<p>Hãy trả về số nguyên <code>divisors[i]</code> có điểm chia hết <strong>lớn nhất</strong>. Nếu có nhiều số nguyên có cùng điểm lớn nhất, hãy trả về số nhỏ nhất.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,9,15,50], divisors = [5,3,7,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Điểm chia hết của <code>divisors[0]</code> là 2 vì <code>nums[2]</code> và <code>nums[3]</code> chia hết cho 5.</p>

<p>Điểm chia hết của <code>divisors[1]</code> là 2 vì <code>nums[1]</code> và <code>nums[2]</code> chia hết cho 3.</p>

<p>Điểm chia hết của <code>divisors[2]</code> là 0 vì không có số nào trong <code>nums</code> chia hết cho 7.</p>

<p>Điểm chia hết của <code>divisors[3]</code> là 2 vì <code>nums[0]</code> và <code>nums[3]</code> chia hết cho 2.</p>

<p>Vì <code>divisors[0]</code>,&nbsp;<code>divisors[1]</code> và <code>divisors[3]</code> có cùng điểm chia hết, ta trả về số nhỏ hơn, đó là <code>divisors[3]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,7,9,3,9], divisors = [5,2,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Điểm chia hết của <code>divisors[0]</code> là 0 vì không có số nào trong <code>nums</code> chia hết cho 5.</p>

<p>Điểm chia hết của <code>divisors[1]</code> là 1 vì chỉ có <code>nums[0]</code> chia hết cho 2.</p>

<p>Điểm chia hết của <code>divisors[2]</code> là 3 vì <code>nums[2]</code>, <code>nums[3]</code> và <code>nums[4]</code> chia hết cho 3.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [20,14,21,10], divisors = [10,16,20]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">10</span></p>

<p><strong>Giải thích:</strong></p>

<p>Điểm chia hết của <code>divisors[0]</code> là 2 vì <code>nums[0]</code> và <code>nums[3]</code> chia hết cho 10.</p>

<p>Điểm chia hết của <code>divisors[1]</code> là 0 vì không có số nào trong <code>nums</code> chia hết cho 16.</p>

<p>Điểm chia hết của <code>divisors[2]</code> là 1 vì <code>nums[0]</code> chia hết cho 20.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length, divisors.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i], divisors[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Điểm số là số lượng phần tử trong $nums$ chia hết cho một ứng viên; nếu hòa, ta chọn ước số nhỏ hơn. Cả hai mảng đều có độ dài $\le 1000$, nên ta có thể duyệt qua $nums$ với từng ước số.
>
> Ta lưu lại số lượng tốt nhất và ước số tương ứng; nếu hòa, thay bằng $div$ nhỏ hơn.

<!-- thinking:end -->

Ta có thể liệt kê từng phần tử $div$ trong $divisors$, rồi tính số lượng phần tử trong $nums$ có thể chia hết cho $div$, ký hiệu là $cnt$.

- Nếu $cnt$ lớn hơn điểm chia hết lớn nhất hiện tại $mx$, cập nhật $mx = cnt$ và cập nhật $ans = div$.
- Nếu $cnt$ bằng $mx$ và $div$ nhỏ hơn $ans$, cập nhật $ans = div$.

Cuối cùng, trả về $ans$.

Độ phức tạp thời gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là độ dài của $nums$ và $divisors$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxDivScore(self, nums: List[int], divisors: List[int]) -> int:
        ans, mx = divisors[0], 0
        for div in divisors:
            cnt = sum(x % div == 0 for x in nums)
            if mx < cnt:
                mx, ans = cnt, div
            elif mx == cnt and ans > div:
                ans = div
        return ans
```

#### Java

```java
class Solution {
    public int maxDivScore(int[] nums, int[] divisors) {
        int ans = divisors[0];
        int mx = 0;
        for (int div : divisors) {
            int cnt = 0;
            for (int x : nums) {
                if (x % div == 0) {
                    ++cnt;
                }
            }
            if (mx < cnt) {
                mx = cnt;
                ans = div;
            } else if (mx == cnt) {
                ans = Math.min(ans, div);
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
    int maxDivScore(vector<int>& nums, vector<int>& divisors) {
        int ans = divisors[0];
        int mx = 0;
        for (int div : divisors) {
            int cnt = 0;
            for (int x : nums) {
                cnt += x % div == 0;
            }
            if (mx < cnt) {
                mx = cnt;
                ans = div;
            } else if (mx == cnt) {
                ans = min(ans, div);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxDivScore(nums []int, divisors []int) int {
	ans, mx := divisors[0], 0
	for _, div := range divisors {
		cnt := 0
		for _, x := range nums {
			if x%div == 0 {
				cnt++
			}
		}
		if mx < cnt {
			ans, mx = div, cnt
		} else if mx == cnt && ans > div {
			ans = div
		}
	}
	return ans
}
```

#### TypeScript

```ts
function maxDivScore(nums: number[], divisors: number[]): number {
    let ans: number = divisors[0];
    let mx: number = 0;
    for (const div of divisors) {
        const cnt = nums.reduce((a, b) => a + (b % div == 0 ? 1 : 0), 0);
        if (mx < cnt) {
            mx = cnt;
            ans = div;
        } else if (mx === cnt && ans > div) {
            ans = div;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_div_score(nums: Vec<i32>, divisors: Vec<i32>) -> i32 {
        let mut ans = divisors[0];
        let mut mx = 0;

        for &div in &divisors {
            let mut cnt = 0;

            for &n in &nums {
                if n % div == 0 {
                    cnt += 1;
                }
            }

            if cnt > mx || (cnt >= mx && div < ans) {
                mx = cnt;
                ans = div;
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
