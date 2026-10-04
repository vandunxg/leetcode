---
comments: true
difficulty: Medium
rating: 1849
source: Biweekly Contest 132 Q3
tags:
    - Array
    - Hash Table
    - Dynamic Programming
---

<!-- problem:start -->

# [3176. Find the Maximum Length of a Good Subsequence I](https://leetcode.com/problems/find-the-maximum-length-of-a-good-subsequence-i)

[中文文档](/solution/3100-3199/3176.Find%20the%20Maximum%20Length%20of%20a%20Good%20Subsequence%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và một số nguyên <strong>không âm</strong> <code>k</code>. Một dãy số nguyên <code>seq</code> được gọi là <strong>tốt</strong> nếu có <strong>không quá</strong> <code>k</code> chỉ số <code>i</code> trong đoạn <code>[0, seq.length - 2]</code> sao cho <code>seq[i] != seq[i + 1]</code>.</p>

<p>Trả về độ dài <strong>lớn nhất</strong> của một <span data-keyword="subsequence-array">dãy con</span> <strong>tốt</strong> của <code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,1,1,3], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Dãy con có độ dài lớn nhất là <code>[<u>1</u>,<u>2</u>,<u>1</u>,<u>1</u>,3]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4,5,1], k = 0</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Dãy con có độ dài lớn nhất là <code>[<u>1</u>,2,3,4,5,<u>1</u>]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 500</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= k &lt;= min(nums.length, 25)</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Một dãy con tốt cho phép không quá $k$ lần thay đổi giữa hai phần tử kề nhau. Cả $n$ và $k$ đều đủ nhỏ để dùng DP trên chỉ số kết thúc và số lần thay đổi đã sử dụng.
>
> Khi mở rộng từ $j$ đến $i$, ta không tốn gì nếu hai giá trị bằng nhau và tốn một lần thay đổi trong trường hợp ngược lại.
>
> Gọi $f[i][h]$ là dãy con dài nhất kết thúc tại $i$ với không quá $h$ lần thay đổi. Ta duyệt mọi $j<i$ và lấy $\max f[i][k]$.

<!-- thinking:end -->

Ta định nghĩa $f[i][h]$ là độ dài của dãy con tốt dài nhất kết thúc tại $nums[i]$ và có không quá $h$ chỉ số thỏa mãn điều kiện. Ban đầu, $f[i][h] = 1$. Đáp án là $\max(f[i][k])$, trong đó $0 \le i < n$.

Ta xét cách tính $f[i][h]$. Ta có thể duyệt $0 \le j < i$. Nếu $nums[i] = nums[j]$, thì $f[i][h] = \max(f[i][h], f[j][h] + 1)$; ngược lại, nếu $h > 0$, thì $f[i][h] = \max(f[i][h], f[j][h - 1] + 1)$. Cụ thể:

$$
f[i][h]=
\begin{cases}
\max(f[i][h], f[j][h] + 1), & \textit{if } nums[i] = nums[j], \\
\max(f[i][h], f[j][h - 1] + 1), & \textit{if } h > 0.
\end{cases}
$$

Đáp án cuối cùng là $\max(f[i][k])$, trong đó $0 \le i < n$.

Độ phức tạp thời gian là $O(n^2 \times k)$ và độ phức tạp không gian là $O(n \times k)$. Trong đó, $n$ là độ dài của mảng `nums`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumLength(self, nums: List[int], k: int) -> int:
        n = len(nums)
        f = [[1] * (k + 1) for _ in range(n)]
        ans = 0
        for i, x in enumerate(nums):
            for h in range(k + 1):
                for j, y in enumerate(nums[:i]):
                    if x == y:
                        f[i][h] = max(f[i][h], f[j][h] + 1)
                    elif h:
                        f[i][h] = max(f[i][h], f[j][h - 1] + 1)
            ans = max(ans, f[i][k])
        return ans
```

#### Java

```java
class Solution {
    public int maximumLength(int[] nums, int k) {
        int n = nums.length;
        int[][] f = new int[n][k + 1];
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int h = 0; h <= k; ++h) {
                for (int j = 0; j < i; ++j) {
                    if (nums[i] == nums[j]) {
                        f[i][h] = Math.max(f[i][h], f[j][h]);
                    } else if (h > 0) {
                        f[i][h] = Math.max(f[i][h], f[j][h - 1]);
                    }
                }
                ++f[i][h];
            }
            ans = Math.max(ans, f[i][k]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumLength(vector<int>& nums, int k) {
        int n = nums.size();
        int f[n][k + 1];
        memset(f, 0, sizeof(f));
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int h = 0; h <= k; ++h) {
                for (int j = 0; j < i; ++j) {
                    if (nums[i] == nums[j]) {
                        f[i][h] = max(f[i][h], f[j][h]);
                    } else if (h) {
                        f[i][h] = max(f[i][h], f[j][h - 1]);
                    }
                }
                ++f[i][h];
            }
            ans = max(ans, f[i][k]);
        }
        return ans;
    }
};
```

#### Go

```go
func maximumLength(nums []int, k int) (ans int) {
	f := make([][]int, len(nums))
	for i := range f {
		f[i] = make([]int, k+1)
	}
	for i, x := range nums {
		for h := 0; h <= k; h++ {
			for j, y := range nums[:i] {
				if x == y {
					f[i][h] = max(f[i][h], f[j][h])
				} else if h > 0 {
					f[i][h] = max(f[i][h], f[j][h-1])
				}
			}
			f[i][h]++
		}
		ans = max(ans, f[i][k])
	}
	return
}
```

#### TypeScript

```ts
function maximumLength(nums: number[], k: number): number {
    const n = nums.length;
    const f: number[][] = Array.from({ length: n }, () => Array(k + 1).fill(0));
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        for (let h = 0; h <= k; ++h) {
            for (let j = 0; j < i; ++j) {
                if (nums[i] === nums[j]) {
                    f[i][h] = Math.max(f[i][h], f[j][h]);
                } else if (h) {
                    f[i][h] = Math.max(f[i][h], f[j][h - 1]);
                }
            }
            ++f[i][h];
        }
        ans = Math.max(ans, f[i][k]);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động tối ưu

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 duyệt lại mọi $j$ cho từng $(i,h)$, phù hợp với các ràng buộc hiện tại nhưng độ phức tạp đã là $O(n^2k)$.
>
> Chuyển trạng thái với giá trị bằng nhau chỉ cần giá trị $f$ lớn nhất của giá trị đó tại $h$; chuyển trạng thái với giá trị khác nhau chỉ cần giá trị lớn nhất và lớn thứ hai tại $h-1$ (bỏ qua giá trị hiện tại).
>
> Lưu các giá trị cực trị đó trong $mp[h][x]$ và bộ ba $g[h]$, loại bỏ vòng lặp $j$ và chuẩn bị cho lời giải $O(nk)$ của Phần II.

<!-- thinking:end -->

Theo công thức chuyển trạng thái trong Lời giải 1, nếu $nums[i] = nums[j]$, ta chỉ cần lấy giá trị lớn nhất của $f[j][h]$. Ta có thể duy trì giá trị này bằng một mảng $mp$ có độ dài $k + 1$. Nếu $nums[i] \neq nums[j]$, ta cần ghi nhận giá trị lớn nhất của $f[j][h - 1]$ tương ứng với $nums[j]$, cùng giá trị lớn nhất và lớn thứ hai. Ta có thể duy trì các giá trị này bằng một mảng $g$ có độ dài $k + 1$.

Độ phức tạp thời gian là $O(n \times k)$ và độ phức tạp không gian là $O(n \times k)$. Trong đó, $n$ là độ dài của mảng `nums`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumLength(self, nums: List[int], k: int) -> int:
        n = len(nums)
        f = [[0] * (k + 1) for _ in range(n)]
        mp = [defaultdict(int) for _ in range(k + 1)]
        g = [[0] * 3 for _ in range(k + 1)]
        ans = 0
        for i, x in enumerate(nums):
            for h in range(k + 1):
                f[i][h] = mp[h][x]
                if h:
                    if g[h - 1][0] != nums[i]:
                        f[i][h] = max(f[i][h], g[h - 1][1])
                    else:
                        f[i][h] = max(f[i][h], g[h - 1][2])
                f[i][h] += 1
                mp[h][nums[i]] = max(mp[h][nums[i]], f[i][h])
                if g[h][0] != x:
                    if f[i][h] >= g[h][1]:
                        g[h][2] = g[h][1]
                        g[h][1] = f[i][h]
                        g[h][0] = x
                    else:
                        g[h][2] = max(g[h][2], f[i][h])
                else:
                    g[h][1] = max(g[h][1], f[i][h])
                ans = max(ans, f[i][h])
        return ans
```

#### Java

```java
class Solution {
    public int maximumLength(int[] nums, int k) {
        int n = nums.length;
        int[][] f = new int[n][k + 1];
        Map<Integer, Integer>[] mp = new HashMap[k + 1];
        Arrays.setAll(mp, i -> new HashMap<>());
        int[][] g = new int[k + 1][3];
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int h = 0; h <= k; ++h) {
                f[i][h] = mp[h].getOrDefault(nums[i], 0);
                if (h > 0) {
                    if (g[h - 1][0] != nums[i]) {
                        f[i][h] = Math.max(f[i][h], g[h - 1][1]);
                    } else {
                        f[i][h] = Math.max(f[i][h], g[h - 1][2]);
                    }
                }
                ++f[i][h];
                mp[h].merge(nums[i], f[i][h], Integer::max);
                if (g[h][0] != nums[i]) {
                    if (f[i][h] >= g[h][1]) {
                        g[h][2] = g[h][1];
                        g[h][1] = f[i][h];
                        g[h][0] = nums[i];
                    } else {
                        g[h][2] = Math.max(g[h][2], f[i][h]);
                    }
                } else {
                    g[h][1] = Math.max(g[h][1], f[i][h]);
                }
                ans = Math.max(ans, f[i][h]);
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
    int maximumLength(vector<int>& nums, int k) {
        int n = nums.size();
        vector<vector<int>> f(n, vector<int>(k + 1));
        vector<unordered_map<int, int>> mp(k + 1);
        vector<vector<int>> g(k + 1, vector<int>(3));
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int h = 0; h <= k; ++h) {
                f[i][h] = mp[h][nums[i]];
                if (h > 0) {
                    if (g[h - 1][0] != nums[i]) {
                        f[i][h] = max(f[i][h], g[h - 1][1]);
                    } else {
                        f[i][h] = max(f[i][h], g[h - 1][2]);
                    }
                }
                ++f[i][h];
                mp[h][nums[i]] = max(mp[h][nums[i]], f[i][h]);
                if (g[h][0] != nums[i]) {
                    if (f[i][h] >= g[h][1]) {
                        g[h][2] = g[h][1];
                        g[h][1] = f[i][h];
                        g[h][0] = nums[i];
                    } else {
                        g[h][2] = max(g[h][2], f[i][h]);
                    }
                } else {
                    g[h][1] = max(g[h][1], f[i][h]);
                }
                ans = max(ans, f[i][h]);
            }
        }

        return ans;
    }
};
```

#### Go

```go
func maximumLength(nums []int, k int) int {
	n := len(nums)
	f := make([][]int, n)
	for i := range f {
		f[i] = make([]int, k+1)
	}
	mp := make([]map[int]int, k+1)
	for i := range mp {
		mp[i] = make(map[int]int)
	}
	g := make([][3]int, k+1)
	ans := 0

	for i := 0; i < n; i++ {
		for h := 0; h <= k; h++ {
			f[i][h] = mp[h][nums[i]]
			if h > 0 {
				if g[h-1][0] != nums[i] {
					if g[h-1][1] > f[i][h] {
						f[i][h] = g[h-1][1]
					}
				} else {
					if g[h-1][2] > f[i][h] {
						f[i][h] = g[h-1][2]
					}
				}
			}
			f[i][h]++
			if f[i][h] > mp[h][nums[i]] {
				mp[h][nums[i]] = f[i][h]
			}
			if g[h][0] != nums[i] {
				if f[i][h] >= g[h][1] {
					g[h][2] = g[h][1]
					g[h][1] = f[i][h]
					g[h][0] = nums[i]
				} else if f[i][h] > g[h][2] {
					g[h][2] = f[i][h]
				}
			} else {
				if f[i][h] > g[h][1] {
					g[h][1] = f[i][h]
				}
			}
			if f[i][h] > ans {
				ans = f[i][h]
			}
		}
	}

	return ans
}
```

#### TypeScript

```ts
function maximumLength(nums: number[], k: number): number {
    const n = nums.length;
    const f: number[][] = Array.from({ length: n }, () => Array(k + 1).fill(0));
    const mp: Map<number, number>[] = Array.from({ length: k + 1 }, () => new Map());
    const g: number[][] = Array.from({ length: k + 1 }, () => Array(3).fill(0));
    let ans = 0;

    for (let i = 0; i < n; i++) {
        for (let h = 0; h <= k; h++) {
            f[i][h] = mp[h].get(nums[i]) || 0;
            if (h > 0) {
                if (g[h - 1][0] !== nums[i]) {
                    f[i][h] = Math.max(f[i][h], g[h - 1][1]);
                } else {
                    f[i][h] = Math.max(f[i][h], g[h - 1][2]);
                }
            }
            f[i][h]++;
            mp[h].set(nums[i], Math.max(mp[h].get(nums[i]) || 0, f[i][h]));
            if (g[h][0] !== nums[i]) {
                if (f[i][h] >= g[h][1]) {
                    g[h][2] = g[h][1];
                    g[h][1] = f[i][h];
                    g[h][0] = nums[i];
                } else {
                    g[h][2] = Math.max(g[h][2], f[i][h]);
                }
            } else {
                g[h][1] = Math.max(g[h][1], f[i][h]);
            }
            ans = Math.max(ans, f[i][h]);
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
