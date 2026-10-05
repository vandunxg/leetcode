---
comments: true
difficulty: Medium
rating: 1626
source: Biweekly Contest 183 Q2
tags:
    - Array
    - Enumeration
---

<!-- problem:start -->

# [3937. Minimum Operations to Make Array Modulo Alternating I](https://leetcode.com/problems/minimum-operations-to-make-array-modulo-alternating-i)

[中文文档](/solution/3900-3999/3937.Minimum%20Operations%20to%20Make%20Array%20Modulo%20Alternating%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>.</p>

<p>Trong một thao tác, bạn có thể <strong>tăng</strong> hoặc <strong>giảm</strong> bất kỳ phần tử nào của <code>nums</code> đi 1.</p>

<p>Một mảng được gọi là <strong>xen kẽ theo modulo</strong> nếu tồn tại hai số nguyên <strong>khác nhau</strong> <code>x</code> và <code>y</code> (<code>0 &lt;= x, y &lt; k</code>) sao cho:</p>

<ul>
	<li>Với mọi chỉ số <strong>chẵn</strong> <code>i</code>, <code>nums[i] % k == x</code></li>
	<li>Với mọi chỉ số <strong>lẻ</strong> <code>i</code>, <code>nums[i] % k == y</code></li>
</ul>

<p>Trả về số thao tác <strong>ít nhất</strong> cần thực hiện để biến <code>nums</code> thành mảng <strong>xen kẽ theo modulo</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,4,2,8], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chọn <code>x = 1</code> cho các chỉ số chẵn và <code>y = 2</code> cho các chỉ số lẻ.</li>
	<li>Thực hiện các thao tác sau:
	<ul>
		<li>Tăng <code>nums[1] = 4</code> lên 1, thu được <code>nums = [1, 5, 2, 8]</code>.</li>
		<li>Giảm <code>nums[2] = 2</code> đi 1, thu được <code>nums = [1, 5, 1, 8]</code>.</li>
	</ul>
	</li>
	<li>Lúc này, với các chỉ số chẵn, <code>nums[i] % k = 1</code>, còn với các chỉ số lẻ, <code>nums[i] % k = 2</code>.</li>
	<li>Vì vậy, tổng số thao tác cần thực hiện là 2.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1,1], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Tăng <code>nums[1]</code> lên 1, thu được <code>nums = [1, 2, 1]</code>, thỏa mãn điều kiện với <code>x = 1</code> và <code>y = 2</code>.</li>
	<li>Vì vậy, tổng số thao tác cần thực hiện là 1.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>2 &lt;= k &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Vì $n\le 100$ và $k\le 100$, ta có thể liệt kê phần dư đích $x$ tại các chỉ số chẵn và phần dư đích $y$ tại các chỉ số lẻ ($x\neq y$), tức là $k(k-1)$ cặp, rồi tính tổng các khoảng cách trên vòng tròn $\min(|t-v|,k-|t-v|)$.
>
> Trước hết, lấy phần dư của mảng theo $k$, sau đó thực hiện phép liệt kê kép. Độ phức tạp $O(nk^2)$ đáp ứng được các ràng buộc.

<!-- thinking:end -->

Ta có thể liệt kê giá trị đích $x$ cho các chỉ số chẵn và giá trị đích $y$ cho các chỉ số lẻ, với $0 \leq x, y < k$ và $x \neq y$. Với mỗi phần tử, ta tính số thao tác cần thực hiện để đưa phần tử đó về giá trị đích, rồi cộng dồn tổng số thao tác. Cuối cùng, ta trả về giá trị nhỏ nhất trong tất cả kết quả liệt kê.

Độ phức tạp thời gian là $O(n \times k^2)$, trong đó $n$ là độ dài mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, nums: list[int], k: int) -> int:
        for i, v in enumerate(nums):
            nums[i] = v % k
        ans = inf
        for x in range(k):
            for y in range(k):
                if x != y:
                    cnt = 0
                    for i, v in enumerate(nums):
                        target = x if i % 2 == 0 else y
                        diff = abs(target - v)
                        cnt += min(diff, k - diff)
                    ans = min(ans, cnt)
        return ans
```

#### Java

```java
class Solution {
    public int minOperations(int[] nums, int k) {
        int n = nums.length;
        for (int i = 0; i < n; ++i) {
            nums[i] %= k;
        }

        int ans = Integer.MAX_VALUE;

        for (int x = 0; x < k; ++x) {
            for (int y = 0; y < k; ++y) {
                if (x != y) {
                    int cnt = 0;

                    for (int i = 0; i < n; ++i) {
                        int target = (i & 1) == 0 ? x : y;
                        int diff = Math.abs(target - nums[i]);
                        cnt += Math.min(diff, k - diff);
                    }

                    ans = Math.min(ans, cnt);
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
    int minOperations(vector<int>& nums, int k) {
        int n = nums.size();

        for (int& v : nums) {
            v %= k;
        }

        int ans = INT_MAX;

        for (int x = 0; x < k; ++x) {
            for (int y = 0; y < k; ++y) {
                if (x != y) {
                    int cnt = 0;

                    for (int i = 0; i < n; ++i) {
                        int target = (i & 1) ? y : x;
                        int diff = abs(target - nums[i]);
                        cnt += min(diff, k - diff);
                    }

                    ans = min(ans, cnt);
                }
            }
        }

        return ans;
    }
};
```

#### Go

```go
func minOperations(nums []int, k int) int {
	for i, v := range nums {
		nums[i] = v % k
	}

	ans := int(^uint(0) >> 1)

	for x := 0; x < k; x++ {
		for y := 0; y < k; y++ {
			if x != y {
				cnt := 0

				for i, v := range nums {
					target := x
					if i&1 == 1 {
						target = y
					}

					diff := abs(target - v)
					cnt += min(diff, k-diff)
				}

				ans = min(ans, cnt)
			}
		}
	}

	return ans
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function minOperations(nums: number[], k: number): number {
    const n = nums.length;

    for (let i = 0; i < n; ++i) {
        nums[i] %= k;
    }

    let ans = Infinity;

    for (let x = 0; x < k; ++x) {
        for (let y = 0; y < k; ++y) {
            if (x !== y) {
                let cnt = 0;

                for (let i = 0; i < n; ++i) {
                    const target = (i & 1) === 0 ? x : y;
                    const diff = Math.abs(target - nums[i]);
                    cnt += Math.min(diff, k - diff);
                }

                ans = Math.min(ans, cnt);
            }
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
