---
comments: true
difficulty: Hard
rating: 2162
source: Weekly Contest 400 Q4
tags:
    - Bit Manipulation
    - Segment Tree
    - Array
    - Binary Search
---

<!-- problem:start -->

# [3171. Find Subarray With Bitwise OR Closest to K](https://leetcode.com/problems/find-subarray-with-bitwise-or-closest-to-k)

[中文文档](/solution/3100-3199/3171.Find%20Subarray%20With%20Bitwise%20OR%20Closest%20to%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <code>nums</code> và một số nguyên <code>k</code>. Hãy tìm một <span data-keyword="subarray-nonempty">mảng con</span> của <code>nums</code> sao cho <strong>hiệu tuyệt đối</strong> giữa <code>k</code> và phép <code>OR</code> bitwise của các phần tử trong mảng con là <strong>nhỏ nhất</strong>. Nói cách khác, chọn một mảng con <code>nums[l..r]</code> sao cho <code>|k - (nums[l] OR nums[l + 1] ... OR nums[r])|</code> là nhỏ nhất.</p>

<p>Trả về giá trị <strong>nhỏ nhất</strong> có thể của hiệu tuyệt đối.</p>

<p><strong>Mảng con</strong> là một dãy phần tử liên tiếp và <b>không rỗng</b> trong một mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,4,5], k = 3</span></p>

<p><strong>Đầu ra:</strong> 0</p>

<p><strong>Giải thích:</strong></p>

<p>Mảng con <code>nums[0..1]</code> có giá trị <code>OR</code> bằng 3, nên hiệu tuyệt đối nhỏ nhất là <code>|3 - 3| = 0</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,3,1,3], k = 2</span></p>

<p><strong>Đầu ra:</strong> 1</p>

<p><strong>Giải thích:</strong></p>

<p>Mảng con <code>nums[1..1]</code> có giá trị <code>OR</code> bằng 3, nên hiệu tuyệt đối nhỏ nhất là <code>|3 - 2| = 1</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1], k = 10</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">9</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chỉ có một mảng con với giá trị <code>OR</code> bằng 1, nên hiệu tuyệt đối nhỏ nhất là <code>|10 - 1| = 9</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Two Pointers + Bitwise Operations

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tối thiểu hóa khoảng cách tuyệt đối giữa OR của một mảng con và $k$. Duyệt bằng hai vòng lặp sẽ tốn $O(n^2)$. Vì OR tăng khi mở rộng cửa sổ, ta có thể dùng two pointers để giữ nó gần với $k$.
>
> Mở rộng đầu phải chỉ có thể thêm các bit; nếu $s>k$ thì tăng đầu trái. Một bit chỉ được xóa khỏi $s$ khi số lần xuất hiện của nó trong cửa sổ giảm về 0.
>
> Duy trì số lần xuất hiện của từng bit và $s$, đồng thời cập nhật $|s-k|$ sau mỗi lần di chuyển. Mỗi pointer di chuyển $O(n)$ lần, nhân với số bit.

<!-- thinking:end -->

Theo mô tả bài toán, ta cần tính kết quả của phép OR bitwise của các phần tử từ chỉ số $l$ đến $r$ trong mảng $\textit{nums}$, tức là $\textit{nums}[l] \lor \textit{nums}[l + 1] \lor \cdots \lor \textit{nums}[r]$, trong đó $\lor$ là phép OR bitwise.

Nếu cố định đầu phải $r$, thì khoảng giá trị của đầu trái $l$ là $[0, r]$. Mỗi lần di chuyển đầu phải $r$, kết quả của phép OR bitwise chỉ có thể tăng. Ta dùng biến $s$ để ghi lại kết quả hiện tại của phép OR bitwise. Nếu $s$ lớn hơn $k$, ta di chuyển đầu trái $l$ sang phải cho đến khi $s$ nhỏ hơn hoặc bằng $k$. Trong quá trình di chuyển đầu trái $l$, ta cần duy trì một mảng $cnt$ để ghi lại số lượng bit $0$ ở mỗi vị trí nhị phân trong khoảng hiện tại. Khi $cnt[h] = 0$, điều đó có nghĩa là tất cả phần tử trong khoảng hiện tại đều có bit thứ $h^{th}$ bằng $0$, và ta có thể đặt bit thứ $h^{th}$ của $s$ về $0$.

Độ phức tạp thời gian là $O(n \times \log M)$, còn độ phức tạp không gian là $O(\log M)$. Ở đây, $n$ và $M$ lần lượt là độ dài của mảng $\textit{nums}$ và giá trị lớn nhất trong mảng $\textit{nums}$.

Các bài tương tự:

- [3097. Shortest Subarray With OR at Least K II](https://github.com/doocs/leetcode/blob/main/solution/3000-3099/3097.Shortest%20Subarray%20With%20OR%20at%20Least%20K%20II/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumDifference(self, nums: List[int], k: int) -> int:
        m = max(nums).bit_length()
        cnt = [0] * m
        s = i = 0
        ans = inf
        for j, x in enumerate(nums):
            s |= x
            ans = min(ans, abs(s - k))
            for h in range(m):
                if x >> h & 1:
                    cnt[h] += 1
            while i < j and s > k:
                y = nums[i]
                for h in range(m):
                    if y >> h & 1:
                        cnt[h] -= 1
                        if cnt[h] == 0:
                            s ^= 1 << h
                i += 1
                ans = min(ans, abs(s - k))
        return ans
```

#### Java

```java
class Solution {
    public int minimumDifference(int[] nums, int k) {
        int mx = 0;
        for (int x : nums) {
            mx = Math.max(mx, x);
        }
        int m = 32 - Integer.numberOfLeadingZeros(mx);
        int[] cnt = new int[m];
        int n = nums.length;
        int ans = Integer.MAX_VALUE;
        for (int i = 0, j = 0, s = 0; j < n; ++j) {
            s |= nums[j];
            ans = Math.min(ans, Math.abs(s - k));
            for (int h = 0; h < m; ++h) {
                if ((nums[j] >> h & 1) == 1) {
                    ++cnt[h];
                }
            }
            while (i < j && s > k) {
                for (int h = 0; h < m; ++h) {
                    if ((nums[i] >> h & 1) == 1 && --cnt[h] == 0) {
                        s ^= 1 << h;
                    }
                }
                ++i;
                ans = Math.min(ans, Math.abs(s - k));
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
    int minimumDifference(vector<int>& nums, int k) {
        int mx = *max_element(nums.begin(), nums.end());
        int m = 32 - __builtin_clz(mx);
        int n = nums.size();
        int ans = INT_MAX;
        vector<int> cnt(m);
        for (int i = 0, j = 0, s = 0; j < n; ++j) {
            s |= nums[j];
            ans = min(ans, abs(s - k));
            for (int h = 0; h < m; ++h) {
                if (nums[j] >> h & 1) {
                    ++cnt[h];
                }
            }
            while (i < j && s > k) {
                for (int h = 0; h < m; ++h) {
                    if (nums[i] >> h & 1 && --cnt[h] == 0) {
                        s ^= 1 << h;
                    }
                }
                ans = min(ans, abs(s - k));
                ++i;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minimumDifference(nums []int, k int) int {
	m := bits.Len(uint(slices.Max(nums)))
	cnt := make([]int, m)
	ans := math.MaxInt32
	s, i := 0, 0
	for j, x := range nums {
		s |= x
		ans = min(ans, abs(s-k))
		for h := 0; h < m; h++ {
			if x>>h&1 == 1 {
				cnt[h]++
			}
		}
		for i < j && s > k {
			y := nums[i]
			for h := 0; h < m; h++ {
				if y>>h&1 == 1 {
					cnt[h]--
					if cnt[h] == 0 {
						s ^= 1 << h
					}
				}
			}
			ans = min(ans, abs(s-k))
			i++
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
function minimumDifference(nums: number[], k: number): number {
    const m = Math.max(...nums).toString(2).length;
    const n = nums.length;
    const cnt: number[] = Array(m).fill(0);
    let ans = Infinity;
    for (let i = 0, j = 0, s = 0; j < n; ++j) {
        s |= nums[j];
        ans = Math.min(ans, Math.abs(s - k));
        for (let h = 0; h < m; ++h) {
            if ((nums[j] >> h) & 1) {
                ++cnt[h];
            }
        }
        while (i < j && s > k) {
            for (let h = 0; h < m; ++h) {
                if ((nums[i] >> h) & 1 && --cnt[h] === 0) {
                    s ^= 1 << h;
                }
            }
            ans = Math.min(ans, Math.abs(s - k));
            ++i;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Hash Table + Enumeration

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 cần duy trì số lượng bit một cách tường minh. Với một đầu phải cố định, chỉ có $O(\log M)$ giá trị OR khác nhau, vì khi di chuyển đầu trái sang trái, các bit 0 chỉ có thể chuyển thành 1.
>
> Lưu các giá trị OR đó trong một set. Với $x$ mới, thay set bằng $\{x\mid y : y\in s\}\cup\{x\}$.
>
> Cập nhật $|y-k|$ cho mọi giá trị trong set. Set vẫn có kích thước logarit, tương ứng với cận trước đó nhưng không cần quản lý nhiều thông tin như vậy.

<!-- thinking:end -->

Theo mô tả bài toán, ta cần tính kết quả của phép OR bitwise của các phần tử từ chỉ số $l$ đến $r$ trong mảng $nums$, tức là $nums[l] \lor nums[l + 1] \lor \cdots \lor nums[r]$. Ở đây, $\lor$ là phép OR bitwise.

Nếu cố định đầu phải $r$, thì khoảng giá trị của đầu trái $l$ là $[0, r]$. Vì giá trị OR bitwise tăng đơn điệu khi $l$ giảm, và giá trị của $\textit{nums}[i]$ không vượt quá $10^9$, khoảng $[0, r]$ có thể có nhiều nhất $30$ giá trị khác nhau. Do đó, ta có thể dùng một set để lưu tất cả giá trị của $nums[l] \lor nums[l + 1] \lor \cdots \lor nums[r]$. Khi duyệt từ $r$ đến $r+1$, các giá trị có $r+1$ làm đầu phải là kết quả của việc thực hiện phép OR bitwise giữa từng giá trị trong set với $nums[r + 1]$, cộng thêm chính $nums[r + 1]$. Vì vậy, ta chỉ cần liệt kê từng giá trị trong set và thực hiện phép OR bitwise với $nums[r]$ để nhận được tất cả giá trị có $r$ làm đầu phải. Sau đó, lấy hiệu tuyệt đối giữa từng giá trị với $k$, và giá trị nhỏ nhất của các hiệu này là đáp án.

Độ phức tạp thời gian là $O(n \times \log M)$, còn độ phức tạp không gian là $O(\log M)$. Ở đây, $n$ và $M$ lần lượt là độ dài của mảng $nums$ và giá trị lớn nhất trong mảng $nums$.

Các bài tương tự:

- [1521. Find a Value of a Mysterious Function Closest to Target](https://github.com/doocs/leetcode/blob/main/solution/1500-1599/1521.Find%20a%20Value%20of%20a%20Mysterious%20Function%20Closest%20to%20Target/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumDifference(self, nums: List[int], k: int) -> int:
        ans = inf
        s = set()
        for x in nums:
            s = {x | y for y in s} | {x}
            ans = min(ans, min(abs(y - k) for y in s))
        return ans
```

#### Java

```java
class Solution {
    public int minimumDifference(int[] nums, int k) {
        int ans = Integer.MAX_VALUE;
        Set<Integer> pre = new HashSet<>();
        for (int x : nums) {
            Set<Integer> cur = new HashSet<>();
            for (int y : pre) {
                cur.add(x | y);
            }
            cur.add(x);
            for (int y : cur) {
                ans = Math.min(ans, Math.abs(y - k));
            }
            pre = cur;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumDifference(vector<int>& nums, int k) {
        int ans = INT_MAX;
        unordered_set<int> pre;
        for (int x : nums) {
            unordered_set<int> cur;
            cur.insert(x);
            for (int y : pre) {
                cur.insert(x | y);
            }
            for (int y : cur) {
                ans = min(ans, abs(y - k));
            }
            pre = move(cur);
        }
        return ans;
    }
};
```

#### Go

```go
func minimumDifference(nums []int, k int) int {
	ans := math.MaxInt32
	pre := map[int]bool{}
	for _, x := range nums {
		cur := map[int]bool{x: true}
		for y := range pre {
			cur[x|y] = true
		}
		for y := range cur {
			ans = min(ans, max(y-k, k-y))
		}
		pre = cur
	}
	return ans
}
```

#### TypeScript

```ts
function minimumDifference(nums: number[], k: number): number {
    let ans = Infinity;
    let pre = new Set<number>();
    for (const x of nums) {
        const cur = new Set<number>();
        cur.add(x);
        for (const y of pre) {
            cur.add(x | y);
        }
        for (const y of cur) {
            ans = Math.min(ans, Math.abs(y - k));
        }
        pre = cur;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
