---
comments: true
difficulty: Hard
rating: 2364
source: Biweekly Contest 132 Q4
tags:
    - Array
    - Hash Table
    - Dynamic Programming
---

<!-- problem:start -->

# [3177. Find the Maximum Length of a Good Subsequence II](https://leetcode.com/problems/find-the-maximum-length-of-a-good-subsequence-ii)

[中文文档](/solution/3100-3199/3177.Find%20the%20Maximum%20Length%20of%20a%20Good%20Subsequence%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> và một số nguyên <strong>không âm</strong> <code>k</code>. Một dãy số nguyên <code>seq</code> được gọi là <strong>tốt</strong> nếu có <strong>nhiều nhất</strong> <code>k</code> chỉ số <code>i</code> trong phạm vi <code>[0, seq.length - 2]</code> sao cho <code>seq[i] != seq[i + 1]</code>.</p>

<p>Trả về độ dài <strong>lớn nhất</strong> có thể của một <span data-keyword="subsequence-array">dãy con</span> <strong>tốt</strong> của <code>nums</code>.</p>

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
	<li><code>1 &lt;= nums.length &lt;= 5 * 10<sup>3</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= k &lt;= min(50, nums.length)</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Định nghĩa này giống Phần I, nhưng $n$ khiến $O(n^2k)$ không đáp ứng được. Công thức truy hồi không thay đổi.
>
> Với các giá trị bằng nhau, ta lấy kết quả tốt nhất trước đó của giá trị đó; khi giá trị thay đổi, ta lấy kết quả tốt nhất ở $h-1$, hoặc kết quả tốt thứ hai nếu kết quả tốt nhất đó thuộc về giá trị hiện tại.
>
> Đọc $f[i][h]$ từ $mp$ và $g$, sau đó ghi các giá trị cực trị mới trở lại. Đáp án là giá trị lớn nhất toàn cục của $f[i][h]$.

<!-- thinking:end -->

Ta định nghĩa $f[i][h]$ là độ dài của dãy con tốt dài nhất kết thúc bằng $nums[i]$ và có không quá $h$ chỉ số thỏa mãn điều kiện. Ban đầu, $f[i][h] = 1$. Đáp án là $\max(f[i][k])$, với $0 \le i < n$.

Ta xét cách tính $f[i][h]$. Ta có thể duyệt mọi $0 \le j < i$. Nếu $nums[i] = nums[j]$, thì $f[i][h] = \max(f[i][h], f[j][h] + 1)$; ngược lại, nếu $h > 0$, thì $f[i][h] = \max(f[i][h], f[j][h - 1] + 1)$. Cụ thể:

$$
f[i][h]=
\begin{cases}
\max(f[i][h], f[j][h] + 1), & \textit{if } nums[i] = nums[j], \\
\max(f[i][h], f[j][h - 1] + 1), & \textit{if } h > 0.
\end{cases}
$$

Đáp án cuối cùng là $\max(f[i][k])$, với $0 \le i < n$.

Độ phức tạp thời gian là $O(n^2 \times k)$, còn độ phức tạp không gian là $O(n \times k)$. Trong đó, $n$ là độ dài của mảng `nums`.

Do miền dữ liệu lớn trong bài toán này, lời giải trên sẽ bị quá thời gian và cần được tối ưu.

Theo công thức chuyển trạng thái, nếu $nums[i] = nums[j]$, ta chỉ cần lấy giá trị lớn nhất của $f[j][h]$; ta có thể duy trì giá trị này bằng một mảng $mp$ có độ dài $k + 1$. Nếu $nums[i] \neq nums[j]$, ta cần lưu lại giá trị lớn nhất của $f[j][h - 1]$ tương ứng với $nums[j]$, gồm giá trị lớn nhất và giá trị lớn thứ hai; ta có thể duy trì chúng bằng một mảng $g$ có độ dài $k + 1$.

Độ phức tạp thời gian là $O(n \times k)$, còn độ phức tạp không gian là $O(n \times k)$. Trong đó, $n$ là độ dài của mảng `nums`.

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
