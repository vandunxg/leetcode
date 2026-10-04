---
comments: true
difficulty: Hard
tags:
    - Array
    - Math
    - Binary Search
    - Greatest Common Divisor
    - Number Theory
    - Euclidean Algorithm
---

<!-- problem:start -->

# [2941. Maximum GCD-Sum of a Subarray 🔒](https://leetcode.com/problems/maximum-gcd-sum-of-a-subarray)

[中文文档](/solution/2900-2999/2941.Maximum%20GCD-Sum%20of%20a%20Subarray/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>.</p>

<p><strong>gcd-sum</strong> của một mảng <code>a</code> được tính như sau:</p>

<ul>
	<li>Gọi <code>s</code> là tổng của tất cả các phần tử trong <code>a</code>.</li>
	<li>Gọi <code>g</code> là <strong>ước chung lớn nhất</strong> của tất cả các phần tử trong <code>a</code>.</li>
	<li>gcd-sum của <code>a</code> bằng <code>s * g</code>.</li>
</ul>

<p>Trả về <em><strong>gcd-sum lớn nhất</strong> của một mảng con của</em> <code>nums</code> <em>có ít nhất</em> <code>k</code> <em>phần tử.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,1,4,4,4,2], k = 2
<strong>Đầu ra:</strong> 48
<strong>Giải thích:</strong> Chọn mảng con [4,4,4], gcd-sum của mảng này là 4 * (4 + 4 + 4) = 48.
Có thể chứng minh rằng không thể chọn mảng con nào khác có gcd-sum lớn hơn 48.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [7,3,9,4], k = 1
<strong>Đầu ra:</strong> 81
<strong>Giải thích:</strong> Chọn mảng con [9], gcd-sum của mảng này là 9 * 9 = 81.
Có thể chứng minh rằng không thể chọn mảng con nào khác có gcd-sum lớn hơn 81.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
	<li><code>1 &lt;= k &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Điểm số của một mảng con bằng tổng của nó nhân với GCD của nó, và độ dài ít nhất là $k$. Thử mọi mảng con sẽ quá chậm khi $n$ lớn. Khi mở rộng đầu phải, các GCD trước đó chỉ giảm, và số lượng giá trị phân biệt là $O(\log A)$.
>
> Lưu trong $f$ chỉ số trái nhất của mỗi GCD. Sau khi đọc $v$, lấy $gcd$ với các cặp cũ, loại bỏ các giá trị trùng nhau vào $g$, rồi thêm $(i,v)$. Với các đoạn có độ dài ít nhất $k$, nhân tổng tiền tố với GCD tương ứng và cập nhật đáp án.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxGcdSum(self, nums: List[int], k: int) -> int:
        s = list(accumulate(nums, initial=0))
        f = []
        ans = 0
        for i, v in enumerate(nums):
            g = []
            for j, x in f:
                y = gcd(x, v)
                if not g or g[-1][1] != y:
                    g.append((j, y))
            f = g
            f.append((i, v))
            for j, x in f:
                if i - j + 1 >= k:
                    ans = max(ans, (s[i + 1] - s[j]) * x)
        return ans
```

#### Java

```java
class Solution {
    public long maxGcdSum(int[] nums, int k) {
        int n = nums.length;
        long[] s = new long[n + 1];
        for (int i = 1; i <= n; ++i) {
            s[i] = s[i - 1] + nums[i - 1];
        }
        List<int[]> f = new ArrayList<>();
        long ans = 0;
        for (int i = 0; i < n; ++i) {
            List<int[]> g = new ArrayList<>();
            for (var e : f) {
                int j = e[0], x = e[1];
                int y = gcd(x, nums[i]);
                if (g.isEmpty() || g.get(g.size() - 1)[1] != y) {
                    g.add(new int[] {j, y});
                }
            }
            f = g;
            f.add(new int[] {i, nums[i]});
            for (var e : f) {
                int j = e[0], x = e[1];
                if (i - j + 1 >= k) {
                    ans = Math.max(ans, (s[i + 1] - s[j]) * x);
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
    long long maxGcdSum(vector<int>& nums, int k) {
        int n = nums.size();
        long long s[n + 1];
        s[0] = 0;
        for (int i = 1; i <= n; ++i) {
            s[i] = s[i - 1] + nums[i - 1];
        }
        vector<pair<int, int>> f;
        long long ans = 0;
        for (int i = 0; i < n; ++i) {
            vector<pair<int, int>> g;
            for (auto [j, x] : f) {
                int y = gcd(x, nums[i]);
                if (g.empt() || g.back().second != y) {
                    g.emplace_back(j, y);
                }
            }
            f = move(g);
            f.emplace_back(i, nums[i]);
            for (auto [j, x] : f) {
                if (i - j + 1 >= k) {
                    ans = max(ans, (s[i + 1] - s[j]) * x);
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxGcdSum(nums []int, k int) int64 {
	n := len(nums)
	s := make([]int64, n+1)
	s[0] = 0
	for i, x := range nums {
		s[i+1] = s[i] + int64(x)
	}
	type pair [2]int
	var f []pair
	var ans int64
	for i := 0; i < n; i++ {
		var g []pair
		for _, p := range f {
			j, x := p[0], p[1]
			y := int(gcd(int(x), nums[i]))
			if len(g) == 0 || g[len(g)-1][1] != y {
				g = append(g, pair{j, y})
			}
		}
		f = g
		f = append(f, pair{i, nums[i]})
		for _, p := range f {
			j, x := p[0], p[1]
			if i-j+1 >= k {
				ans = max(ans, (s[i+1]-s[j])*int64(x))
			}
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
function maxGcdSum(nums: number[], k: number): number {
    const n: number = nums.length;
    const s: number[] = Array(n + 1).fill(0);
    for (let i = 1; i <= n; i++) {
        s[i] = s[i - 1] + nums[i - 1];
    }

    let f: [number, number][] = [];
    let ans: number = 0;

    for (let i = 0; i < n; ++i) {
        const g: [number, number][] = [];
        for (const [j, x] of f) {
            const y: number = gcd(x, nums[i]);
            if (g.length === 0 || g.at(-1)[1] !== y) {
                g.push([j, y]);
            }
        }
        f = g;
        f.push([i, nums[i]]);
        for (const [j, x] of f) {
            if (i - j + 1 >= k) {
                ans = Math.max(ans, (s[i + 1] - s[j]) * x);
            }
        }
    }

    return ans;
}

function gcd(a: number, b: number): number {
    return b === 0 ? a : gcd(b, a % b);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
