---
comments: true
difficulty: Easy
rating: 1368
source: Biweekly Contest 127 Q1
tags:
    - Bit Manipulation
    - Array
    - Sliding Window
---

<!-- problem:start -->

# [3095. Shortest Subarray With OR at Least K I](https://leetcode.com/problems/shortest-subarray-with-or-at-least-k-i)

[中文文档](/solution/3000-3099/3095.Shortest%20Subarray%20With%20OR%20at%20Least%20K%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng các số nguyên <code>nums</code> <strong>không âm</strong> và một số nguyên <code>k</code>.</p>

<p>Một mảng được gọi là <strong>đặc biệt</strong> nếu phép <code>OR</code> theo bit của tất cả các phần tử trong mảng <strong>lớn hơn hoặc bằng</strong> <code>k</code>.</p>

<p>Hãy trả về <em>độ dài của <span data-keyword="subarray-nonempty">mảng con</span> <strong>đặc biệt</strong> <strong>không rỗng</strong> <strong>ngắn nhất</strong> của</em> <code>nums</code>, <em>hoặc trả về</em> <code>-1</code> <em>nếu không tồn tại mảng con đặc biệt</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng con <code>[3]</code> có giá trị <code>OR</code> bằng <code>3</code>. Vì vậy, ta trả về <code>1</code>.</p>

<p>Lưu ý rằng <code>[2]</code> cũng là một mảng con đặc biệt.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,1,8], k = 10</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng con <code>[2,1,8]</code> có giá trị <code>OR</code> bằng <code>11</code>. Vì vậy, ta trả về <code>3</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2], k = 0</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng con <code>[1]</code> có giá trị <code>OR</code> bằng <code>1</code>. Vì vậy, ta trả về <code>1</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums.length &lt;= 50</code></li>
    <li><code>0 &lt;= nums[i] &lt;= 50</code></li>
    <li><code>0 &lt;= k &lt; 64</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ + Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm mảng con ngắn nhất có phép OR theo bit lớn hơn hoặc bằng $k$. Với $n \le 50$, ta có thể dùng ba vòng lặp, nhưng OR tăng đơn điệu theo điểm kết thúc bên phải, nên có thể áp dụng hai con trỏ.
>
> Điểm kết thúc bên phải chỉ có thể bật thêm các bit; điểm kết thúc bên trái chỉ có thể tắt một bit khi số lần xuất hiện của bit đó giảm xuống $0$.
>
> Ta duy trì số lần xuất hiện của $32$ bit và OR hiện tại $s$, mở rộng rồi thu hẹp cửa sổ, đồng thời ghi nhận cửa sổ ngắn nhất với $s \ge k$.

<!-- thinking:end -->

Ta nhận thấy rằng nếu cố định điểm bắt đầu bên trái của mảng con, khi điểm kết thúc bên phải dịch sang phải, giá trị OR theo bit của mảng con chỉ tăng chứ không giảm. Vì vậy, ta có thể sử dụng phương pháp hai con trỏ để duy trì một mảng con thỏa mãn điều kiện.

Cụ thể, ta dùng hai con trỏ $i$ và $j$ lần lượt biểu diễn điểm đầu và điểm cuối của mảng con. Ban đầu, cả hai con trỏ đều ở phần tử đầu tiên của mảng. Ta dùng biến $s$ để biểu diễn giá trị OR theo bit của mảng con, ban đầu $s$ bằng $0$. Ta cũng cần duy trì một mảng $cnt$ có độ dài $32$, biểu diễn số lần xuất hiện của mỗi bit trong biểu diễn nhị phân của từng phần tử trong mảng con.

Ở mỗi bước, ta dịch $j$ sang phải một vị trí rồi cập nhật $s$ và $cnt$. Nếu giá trị của $s$ lớn hơn hoặc bằng $k$, ta liên tục cập nhật độ dài nhỏ nhất của mảng con và dịch $i$ sang phải một vị trí cho đến khi giá trị của $s$ nhỏ hơn $k$. Trong quá trình này, ta cũng cần cập nhật $s$ và $cnt$.

Cuối cùng, ta trả về độ dài nhỏ nhất. Nếu không có mảng con nào thỏa mãn điều kiện, ta trả về $-1$.

Độ phức tạp thời gian là $O(n \times \log M)$ và độ phức tạp không gian là $O(\log M)$, trong đó $n$ và $M$ lần lượt là độ dài của mảng và giá trị lớn nhất trong các phần tử của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumSubarrayLength(self, nums: List[int], k: int) -> int:
        n = len(nums)
        cnt = [0] * 32
        ans = n + 1
        s = i = 0
        for j, x in enumerate(nums):
            s |= x
            for h in range(32):
                if x >> h & 1:
                    cnt[h] += 1
            while s >= k and i <= j:
                ans = min(ans, j - i + 1)
                y = nums[i]
                for h in range(32):
                    if y >> h & 1:
                        cnt[h] -= 1
                        if cnt[h] == 0:
                            s ^= 1 << h
                i += 1
        return -1 if ans > n else ans
```

#### Java

```java
class Solution {
    public int minimumSubarrayLength(int[] nums, int k) {
        int n = nums.length;
        int[] cnt = new int[32];
        int ans = n + 1;
        for (int i = 0, j = 0, s = 0; j < n; ++j) {
            s |= nums[j];
            for (int h = 0; h < 32; ++h) {
                if ((nums[j] >> h & 1) == 1) {
                    ++cnt[h];
                }
            }
            for (; s >= k && i <= j; ++i) {
                ans = Math.min(ans, j - i + 1);
                for (int h = 0; h < 32; ++h) {
                    if ((nums[i] >> h & 1) == 1) {
                        if (--cnt[h] == 0) {
                            s ^= 1 << h;
                        }
                    }
                }
            }
        }
        return ans > n ? -1 : ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumSubarrayLength(vector<int>& nums, int k) {
        int n = nums.size();
        int cnt[32]{};
        int ans = n + 1;
        for (int i = 0, j = 0, s = 0; j < n; ++j) {
            s |= nums[j];
            for (int h = 0; h < 32; ++h) {
                if ((nums[j] >> h & 1) == 1) {
                    ++cnt[h];
                }
            }
            for (; s >= k && i <= j; ++i) {
                ans = min(ans, j - i + 1);
                for (int h = 0; h < 32; ++h) {
                    if ((nums[i] >> h & 1) == 1) {
                        if (--cnt[h] == 0) {
                            s ^= 1 << h;
                        }
                    }
                }
            }
        }
        return ans > n ? -1 : ans;
    }
};
```

#### Go

```go
func minimumSubarrayLength(nums []int, k int) int {
	n := len(nums)
	cnt := [32]int{}
	ans := n + 1
	s, i := 0, 0
	for j, x := range nums {
		s |= x
		for h := 0; h < 32; h++ {
			if x>>h&1 == 1 {
				cnt[h]++
			}
		}
		for ; s >= k && i <= j; i++ {
			ans = min(ans, j-i+1)
			for h := 0; h < 32; h++ {
				if nums[i]>>h&1 == 1 {
					cnt[h]--
					if cnt[h] == 0 {
						s ^= 1 << h
					}
				}
			}
		}
	}
	if ans == n+1 {
		return -1
	}
	return ans
}
```

#### TypeScript

```ts
function minimumSubarrayLength(nums: number[], k: number): number {
    const n = nums.length;
    let ans = n + 1;
    const cnt: number[] = new Array<number>(32).fill(0);
    for (let i = 0, j = 0, s = 0; j < n; ++j) {
        s |= nums[j];
        for (let h = 0; h < 32; ++h) {
            if (((nums[j] >> h) & 1) === 1) {
                ++cnt[h];
            }
        }
        for (; s >= k && i <= j; ++i) {
            ans = Math.min(ans, j - i + 1);
            for (let h = 0; h < 32; ++h) {
                if (((nums[i] >> h) & 1) === 1 && --cnt[h] === 0) {
                    s ^= 1 << h;
                }
            }
        }
    }
    return ans === n + 1 ? -1 : ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn minimum_subarray_length(nums: Vec<i32>, k: i32) -> i32 {
        let n = nums.len();
        let mut cnt = vec![0; 32];
        let mut ans = n as i32 + 1;
        let mut s = 0;
        let mut i = 0;

        for (j, &x) in nums.iter().enumerate() {
            s |= x;
            for h in 0..32 {
                if (x >> h) & 1 == 1 {
                    cnt[h] += 1;
                }
            }

            while s >= k && i <= j {
                ans = ans.min((j - i + 1) as i32);
                let y = nums[i];
                for h in 0..32 {
                    if (y >> h) & 1 == 1 {
                        cnt[h] -= 1;
                        if cnt[h] == 0 {
                            s ^= 1 << h;
                        }
                    }
                }
                i += 1;
            }
        }
        if ans > n as i32 { -1 } else { ans }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
