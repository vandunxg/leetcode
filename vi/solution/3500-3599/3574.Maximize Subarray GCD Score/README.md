---
comments: true
difficulty: Hard
rating: 2257
source: Biweekly Contest 158 Q3
tags:
    - Array
    - Math
    - Enumeration
    - Number Theory
---

<!-- problem:start -->

# [3574. Maximize Subarray GCD Score](https://leetcode.com/problems/maximize-subarray-gcd-score)

[中文文档](/solution/3500-3599/3574.Maximize%20Subarray%20GCD%20Score/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên dương <code>nums</code> và một số nguyên <code>k</code>.</p>

<p>Bạn có thể thực hiện nhiều nhất <code>k</code> thao tác. Trong mỗi thao tác, bạn có thể chọn một phần tử trong mảng và <strong>nhân đôi</strong> giá trị của nó. Mỗi phần tử chỉ có thể được nhân đôi <strong>nhiều nhất</strong> một lần.</p>

<p><strong>Điểm số</strong> của một <strong><span data-keyword="subarray">mảng con</span></strong> liên tiếp được định nghĩa là <strong>tích</strong> của độ dài và <em>ước chung lớn nhất (GCD)</em> của tất cả các phần tử trong đó.</p>

<p>Nhiệm vụ của bạn là trả về <strong>điểm số</strong> <strong>lớn nhất</strong> có thể đạt được bằng cách chọn một mảng con liên tiếp từ mảng đã được thay đổi.</p>

<p><strong>Lưu ý:</strong></p>

<ul>
	<li><strong>Ước chung lớn nhất (GCD)</strong> của một mảng là số nguyên lớn nhất chia hết cho tất cả các phần tử trong mảng.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,4], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">8</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Nhân đôi <code>nums[0]</code> thành 4 bằng một thao tác. Mảng sau khi thay đổi là <code>[4, 4]</code>.</li>
	<li>GCD của mảng con <code>[4, 4]</code> là 4, và độ dài là 2.</li>
	<li>Vì vậy, điểm số lớn nhất có thể đạt được là <code>2 &times; 4 = 8</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,5,7], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">14</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Nhân đôi <code>nums[2]</code> thành 14 bằng một thao tác. Mảng sau khi thay đổi là <code>[3, 5, 14]</code>.</li>
	<li>GCD của mảng con <code>[14]</code> là 14, và độ dài là 1.</li>
	<li>Vì vậy, điểm số lớn nhất có thể đạt được là <code>1 &times; 14 = 14</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,5,5], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">15</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Mảng con <code>[5, 5, 5]</code> có GCD bằng 5 và độ dài là 3.</li>
	<li>Vì nhân đôi bất kỳ phần tử nào cũng không cải thiện điểm số, nên điểm số lớn nhất là <code>3 &times; 5 = 15</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length &lt;= 1500</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= k &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê + Toán học

<!-- thinking:start -->

> **Tư duy**
>
> $n \le 1500$, nên ta có thể liệt kê mọi mảng con. Mỗi giá trị chỉ có thể được nhân đôi một lần, vì vậy GCD nhiều nhất chỉ tăng gấp đôi, và điều này chỉ xảy ra khi số vị trí có ít thừa số $2$ nhất không vượt quá $k$.
>
> Ta tiền xử lý số mũ $2$-adic của mỗi phần tử. Với mỗi $l$, ta mở rộng $r$, duy trì GCD hiện tại, số mũ nhỏ nhất và số lần xuất hiện của nó, rồi tính điểm số bằng độ dài nhân với $g$ hoặc $2g$.

<!-- thinking:end -->

Ta nhận thấy độ dài của mảng trong bài toán này là $n \leq 1500$, nên ta có thể liệt kê tất cả các mảng con. Với mỗi mảng con, ta tính GCD và điểm số của nó, rồi tìm giá trị lớn nhất làm đáp án.

Vì mỗi số chỉ có thể được nhân đôi nhiều nhất một lần, GCD của một mảng con nhiều nhất có thể nhân với $2$. Do đó, ta cần đếm số mũ của $2$ nhỏ nhất trong tất cả các số của mảng con, cũng như số lần số mũ nhỏ nhất này xuất hiện. Nếu số lần xuất hiện lớn hơn $k$, điểm số của mảng con là chính GCD; ngược lại, điểm số là GCD nhân với $2$.

Vì vậy, ta có thể tiền xử lý số mũ của $2$ trong mỗi số. Khi liệt kê các mảng con, ta duy trì GCD hiện tại, số mũ của $2$ nhỏ nhất và số lần số mũ nhỏ nhất này xuất hiện.

Độ phức tạp thời gian là $O(n^2 \times \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxGCDScore(self, nums: List[int], k: int) -> int:
        n = len(nums)
        cnt = [0] * n
        for i, x in enumerate(nums):
            while x % 2 == 0:
                cnt[i] += 1
                x //= 2
        ans = 0
        for l in range(n):
            g = 0
            mi = inf
            t = 0
            for r in range(l, n):
                g = gcd(g, nums[r])
                if cnt[r] < mi:
                    mi = cnt[r]
                    t = 1
                elif cnt[r] == mi:
                    t += 1
                ans = max(ans, (g if t > k else g * 2) * (r - l + 1))
        return ans
```

#### Java

```java
class Solution {
    public long maxGCDScore(int[] nums, int k) {
        int n = nums.length;
        int[] cnt = new int[n];
        for (int i = 0; i < n; ++i) {
            for (int x = nums[i]; x % 2 == 0; x /= 2) {
                ++cnt[i];
            }
        }
        long ans = 0;
        for (int l = 0; l < n; ++l) {
            int g = 0;
            int mi = 1 << 30;
            int t = 0;
            for (int r = l; r < n; ++r) {
                g = gcd(g, nums[r]);
                if (cnt[r] < mi) {
                    mi = cnt[r];
                    t = 1;
                } else if (cnt[r] == mi) {
                    ++t;
                }
                ans = Math.max(ans, (r - l + 1L) * (t > k ? g : g * 2));
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
    long long maxGCDScore(vector<int>& nums, int k) {
        int n = nums.size();
        vector<int> cnt(n);
        for (int i = 0; i < n; ++i) {
            for (int x = nums[i]; x % 2 == 0; x /= 2) {
                ++cnt[i];
            }
        }

        long long ans = 0;
        for (int l = 0; l < n; ++l) {
            int g = 0;
            int mi = INT32_MAX;
            int t = 0;
            for (int r = l; r < n; ++r) {
                g = gcd(g, nums[r]);
                if (cnt[r] < mi) {
                    mi = cnt[r];
                    t = 1;
                } else if (cnt[r] == mi) {
                    ++t;
                }
                long long score = static_cast<long long>(r - l + 1) * (t > k ? g : g * 2);
                ans = max(ans, score);
            }
        }

        return ans;
    }
};
```

#### Go

```go
func maxGCDScore(nums []int, k int) int64 {
	n := len(nums)
	cnt := make([]int, n)
	for i, x := range nums {
		for x%2 == 0 {
			cnt[i]++
			x /= 2
		}
	}

	ans := 0
	for l := 0; l < n; l++ {
		g := 0
		mi := math.MaxInt32
		t := 0
		for r := l; r < n; r++ {
			g = gcd(g, nums[r])
			if cnt[r] < mi {
				mi = cnt[r]
				t = 1
			} else if cnt[r] == mi {
				t++
			}
			length := r - l + 1
			score := g * length
			if t <= k {
				score *= 2
			}
			ans = max(ans, score)
		}
	}

	return int64(ans)
}

func gcd(a, b int) int {
	for b != 0 {
		a, b = b, a%b
	}
	return a
}
```

#### TypeScript

```ts
function maxGCDScore(nums: number[], k: number): number {
    const n = nums.length;
    const cnt: number[] = Array(n).fill(0);

    for (let i = 0; i < n; ++i) {
        let x = nums[i];
        while (x % 2 === 0) {
            cnt[i]++;
            x /= 2;
        }
    }

    let ans = 0;
    for (let l = 0; l < n; ++l) {
        let g = 0;
        let mi = Number.MAX_SAFE_INTEGER;
        let t = 0;
        for (let r = l; r < n; ++r) {
            g = gcd(g, nums[r]);
            if (cnt[r] < mi) {
                mi = cnt[r];
                t = 1;
            } else if (cnt[r] === mi) {
                t++;
            }
            const len = r - l + 1;
            const score = (t > k ? g : g * 2) * len;
            ans = Math.max(ans, score);
        }
    }

    return ans;
}

function gcd(a: number, b: number): number {
    while (b !== 0) {
        const temp = b;
        b = a % b;
        a = temp;
    }
    return a;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_gcd_score(nums: Vec<i32>, k: i32) -> i64 {
        let n = nums.len();
        let mut cnt = vec![0i32; n];
        for i in 0..n {
            let mut x = nums[i];
            while x % 2 == 0 {
                cnt[i] += 1;
                x /= 2;
            }
        }

        let mut ans: i64 = 0;
        for l in 0..n {
            let mut g: i32 = 0;
            let mut mi: i32 = 1 << 30;
            let mut t: i32 = 0;
            for r in l..n {
                g = Self::gcd(g, nums[r]);
                if cnt[r] < mi {
                    mi = cnt[r];
                    t = 1;
                } else if cnt[r] == mi {
                    t += 1;
                }
                let val = if t > k { g as i64 } else { (g * 2) as i64 };
                ans = ans.max((r as i64 - l as i64 + 1) * val);
            }
        }
        ans
    }

    fn gcd(a: i32, b: i32) -> i32 {
        if b == 0 { a } else { Self::gcd(b, a % b) }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
