---
comments: true
difficulty: Hard
rating: 2389
source: Weekly Contest 218 Q4
tags:
    - Bit Manipulation
    - Array
    - Hash Table
    - Dynamic Programming
    - Bitmask
---

<!-- problem:start -->

# [1681. Minimum Incompatibility](https://leetcode.com/problems/minimum-incompatibility)

[中文文档](/solution/1600-1699/1681.Minimum%20Incompatibility/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code>​​​ và số nguyên <code>k</code>. Hãy chia mảng thành <code>k</code> tập con có <strong>cùng kích thước</strong>, sao cho không có hai phần tử bằng nhau trong cùng một tập con.</p>

<p><strong>Độ không tương thích</strong> của một tập con là hiệu giữa phần tử lớn nhất và nhỏ nhất trong tập con đó.</p>

<p>Hãy trả về <em><strong>tổng độ không tương thích nhỏ nhất có thể</strong> của </em><code>k</code> <em>tập con sau khi chia mảng tối ưu, hoặc trả về </em><code>-1</code><em> nếu không thể chia.</em></p>

<p>Tập con là một nhóm các số nguyên xuất hiện trong mảng, không yêu cầu thứ tự cụ thể.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,2,1,4], k = 2
<strong>Output:</strong> 4
<strong>Giải thích:</strong> Cách chia tối ưu là [1,2] và [1,4].
Độ không tương thích là (2-1) + (4-1) = 4.
Lưu ý rằng [1,1] và [2,4] cho tổng nhỏ hơn, nhưng tập con đầu tiên chứa hai phần tử bằng nhau.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [6,3,8,1,3,1,2,2], k = 4
<strong>Output:</strong> 6
<strong>Giải thích:</strong> Cách chia tối ưu là [1,2], [2,3], [6,8] và [1,3].
Độ không tương thích là (2-1) + (3-2) + (8-6) + (3-1) = 6.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> nums = [5,3,3,6,3,3], k = 3
<strong>Output:</strong> -1
<strong>Giải thích:</strong> Không thể chia nums thành 3 tập con sao cho không có hai phần tử bằng nhau trong cùng một tập con.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= nums.length &lt;= 16</code></li>
	<li><code>nums.length</code> chia hết cho <code>k</code></li>
	<li><code>1 &lt;= nums[i] &lt;= nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tiền xử lý + Nén trạng thái + Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Chia thành $k$ nhóm có các giá trị phân biệt, đồng thời tối thiểu hóa tổng $(\max-\min)$. Vì $n \le 16$, ta có thể dùng subset DP.
>
> Tính trước độ không tương thích $g[i]$ cho mọi tập con kích thước $m=n/k$ không có phần tử trùng. $f[S]$ là chi phí nhỏ nhất của tập chỉ số đã chọn $S$; chuyển trạng thái qua các tập con kích thước $m$ không trùng của phần bù.
>
> Nếu $f[2^n-1]$ vẫn vô hạn, trả về $-1$.

<!-- thinking:end -->

Gọi kích thước mỗi tập con sau khi chia là $m$, khi đó $m=\frac{n}{k}$, với $n$ là độ dài mảng.

Ta có thể duyệt mọi tập con $i$, với $i \in [0, 2^n)$. Nếu biểu diễn nhị phân của tập con $i$ có $m$ bit 1 và các phần tử trong tập con $i$ không trùng nhau, ta tính được độ không tương thích của tập con $i$, ký hiệu là $g[i]$, tức $g[i]=\max_{j \in i} \{nums[j]\} - \min_{j \in i} \{nums[j]\}$.

Tiếp theo, ta dùng quy hoạch động để giải bài toán.

Đặt $f[i]$ là tổng độ không tương thích nhỏ nhất khi trạng thái các phần tử đã chia hiện tại là $i$. Ban đầu, $f[0]=0$, nghĩa là chưa có phần tử nào được chia, các giá trị còn lại $f[i]=+\infty$.

Với trạng thái $i$, ta tìm tất cả phần tử chưa chia và không trùng, biểu diễn bằng trạng thái $mask$. Nếu số phần tử trong $mask$ lớn hơn hoặc bằng $m$, ta duyệt mọi tập con $j$ của $mask$ thỏa $j \subset mask$, rồi cập nhật $f[i \cup j]=\min \{f[i \cup j], f[i]+g[j]\}$.

Cuối cùng, nếu $f[2^n-1]=+\infty$, nghĩa là không thể chia thành $k$ tập con, trả về $-1$; ngược lại trả về $f[2^n-1]$.

Độ phức tạp thời gian là $O(3^n)$ và độ phức tạp không gian là $O(2^n)$, trong đó $n$ là độ dài mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumIncompatibility(self, nums: List[int], k: int) -> int:
        n = len(nums)
        m = n // k
        g = [-1] * (1 << n)
        for i in range(1, 1 << n):
            if i.bit_count() != m:
                continue
            s = set()
            mi, mx = 20, 0
            for j, x in enumerate(nums):
                if i >> j & 1:
                    if x in s:
                        break
                    s.add(x)
                    mi = min(mi, x)
                    mx = max(mx, x)
            if len(s) == m:
                g[i] = mx - mi
        f = [inf] * (1 << n)
        f[0] = 0
        for i in range(1 << n):
            if f[i] == inf:
                continue
            s = set()
            mask = 0
            for j, x in enumerate(nums):
                if (i >> j & 1) == 0 and x not in s:
                    s.add(x)
                    mask |= 1 << j
            if len(s) < m:
                continue
            j = mask
            while j:
                if g[j] != -1:
                    f[i | j] = min(f[i | j], f[i] + g[j])
                j = (j - 1) & mask
        return f[-1] if f[-1] != inf else -1
```

#### Java

```java
class Solution {
    public int minimumIncompatibility(int[] nums, int k) {
        int n = nums.length;
        int m = n / k;
        int[] g = new int[1 << n];
        Arrays.fill(g, -1);
        for (int i = 1; i < 1 << n; ++i) {
            if (Integer.bitCount(i) != m) {
                continue;
            }
            Set<Integer> s = new HashSet<>();
            int mi = 20, mx = 0;
            for (int j = 0; j < n; ++j) {
                if ((i >> j & 1) == 1) {
                    if (!s.add(nums[j])) {
                        break;
                    }
                    mi = Math.min(mi, nums[j]);
                    mx = Math.max(mx, nums[j]);
                }
            }
            if (s.size() == m) {
                g[i] = mx - mi;
            }
        }
        int[] f = new int[1 << n];
        final int inf = 1 << 30;
        Arrays.fill(f, inf);
        f[0] = 0;
        for (int i = 0; i < 1 << n; ++i) {
            if (f[i] == inf) {
                continue;
            }
            Set<Integer> s = new HashSet<>();
            int mask = 0;
            for (int j = 0; j < n; ++j) {
                if ((i >> j & 1) == 0 && !s.contains(nums[j])) {
                    s.add(nums[j]);
                    mask |= 1 << j;
                }
            }
            if (s.size() < m) {
                continue;
            }
            for (int j = mask; j > 0; j = (j - 1) & mask) {
                if (g[j] != -1) {
                    f[i | j] = Math.min(f[i | j], f[i] + g[j]);
                }
            }
        }
        return f[(1 << n) - 1] == inf ? -1 : f[(1 << n) - 1];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumIncompatibility(vector<int>& nums, int k) {
        int n = nums.size();
        int m = n / k;
        int g[1 << n];
        memset(g, -1, sizeof(g));
        for (int i = 1; i < 1 << n; ++i) {
            if (__builtin_popcount(i) != m) {
                continue;
            }
            unordered_set<int> s;
            int mi = 20, mx = 0;
            for (int j = 0; j < n; ++j) {
                if (i >> j & 1) {
                    if (s.count(nums[j])) {
                        break;
                    }
                    s.insert(nums[j]);
                    mi = min(mi, nums[j]);
                    mx = max(mx, nums[j]);
                }
            }
            if (s.size() == m) {
                g[i] = mx - mi;
            }
        }
        int f[1 << n];
        memset(f, 0x3f, sizeof(f));
        f[0] = 0;
        for (int i = 0; i < 1 << n; ++i) {
            if (f[i] == 0x3f3f3f3f) {
                continue;
            }
            unordered_set<int> s;
            int mask = 0;
            for (int j = 0; j < n; ++j) {
                if ((i >> j & 1) == 0 && !s.count(nums[j])) {
                    s.insert(nums[j]);
                    mask |= 1 << j;
                }
            }
            if (s.size() < m) {
                continue;
            }
            for (int j = mask; j; j = (j - 1) & mask) {
                if (g[j] != -1) {
                    f[i | j] = min(f[i | j], f[i] + g[j]);
                }
            }
        }
        return f[(1 << n) - 1] == 0x3f3f3f3f ? -1 : f[(1 << n) - 1];
    }
};
```

#### Go

```go
func minimumIncompatibility(nums []int, k int) int {
	n := len(nums)
	m := n / k
	const inf = 1 << 30
	f := make([]int, 1<<n)
	g := make([]int, 1<<n)
	for i := range g {
		f[i] = inf
		g[i] = -1
	}
	for i := 1; i < 1<<n; i++ {
		if bits.OnesCount(uint(i)) != m {
			continue
		}
		s := map[int]struct{}{}
		mi, mx := 20, 0
		for j, x := range nums {
			if i>>j&1 == 1 {
				if _, ok := s[x]; ok {
					break
				}
				s[x] = struct{}{}
				mi = min(mi, x)
				mx = max(mx, x)
			}
		}
		if len(s) == m {
			g[i] = mx - mi
		}
	}
	f[0] = 0
	for i := 0; i < 1<<n; i++ {
		if f[i] == inf {
			continue
		}
		s := map[int]struct{}{}
		mask := 0
		for j, x := range nums {
			if _, ok := s[x]; !ok && i>>j&1 == 0 {
				s[x] = struct{}{}
				mask |= 1 << j
			}
		}
		if len(s) < m {
			continue
		}
		for j := mask; j > 0; j = (j - 1) & mask {
			if g[j] != -1 {
				f[i|j] = min(f[i|j], f[i]+g[j])
			}
		}
	}
	if f[1<<n-1] == inf {
		return -1
	}
	return f[1<<n-1]
}
```

#### TypeScript

```ts
function minimumIncompatibility(nums: number[], k: number): number {
    const n = nums.length;
    const m = Math.floor(n / k);
    const g: number[] = Array(1 << n).fill(-1);
    for (let i = 1; i < 1 << n; ++i) {
        if (bitCount(i) !== m) {
            continue;
        }
        const s: Set<number> = new Set();
        let [mi, mx] = [20, 0];
        for (let j = 0; j < n; ++j) {
            if ((i >> j) & 1) {
                if (s.has(nums[j])) {
                    break;
                }
                s.add(nums[j]);
                mi = Math.min(mi, nums[j]);
                mx = Math.max(mx, nums[j]);
            }
        }
        if (s.size === m) {
            g[i] = mx - mi;
        }
    }
    const inf = 1e9;
    const f: number[] = Array(1 << n).fill(inf);
    f[0] = 0;
    for (let i = 0; i < 1 << n; ++i) {
        if (f[i] === inf) {
            continue;
        }
        const s: Set<number> = new Set();
        let mask = 0;
        for (let j = 0; j < n; ++j) {
            if (((i >> j) & 1) === 0 && !s.has(nums[j])) {
                s.add(nums[j]);
                mask |= 1 << j;
            }
        }
        if (s.size < m) {
            continue;
        }
        for (let j = mask; j; j = (j - 1) & mask) {
            if (g[j] !== -1) {
                f[i | j] = Math.min(f[i | j], f[i] + g[j]);
            }
        }
    }
    return f[(1 << n) - 1] === inf ? -1 : f[(1 << n) - 1];
}

function bitCount(i: number): number {
    i = i - ((i >>> 1) & 0x55555555);
    i = (i & 0x33333333) + ((i >>> 2) & 0x33333333);
    i = (i + (i >>> 4)) & 0x0f0f0f0f;
    i = i + (i >>> 8);
    i = i + (i >>> 16);
    return i & 0x3f;
}
```

#### C#

```cs
public class Solution {
    public int MinimumIncompatibility(int[] nums, int k) {
        int n = nums.Length;
        int m = n / k;
        int[] g = new int[1 << n];
        Array.Fill(g, -1);
        for (int i = 1; i < 1 << n; ++i) {
            if (bitCount(i) != m) {
                continue;
            }
            HashSet<int> s = new();
            int mi = 20, mx = 0;
            for (int j = 0; j < n; ++j) {
                if ((i >> j & 1) == 1) {
                    if (s.Contains(nums[j])) {
                        break;
                    }
                    s.Add(nums[j]);
                    mi = Math.Min(mi, nums[j]);
                    mx = Math.Max(mx, nums[j]);
                }
            }
            if (s.Count == m) {
                g[i] = mx - mi;
            }
        }
        int[] f = new int[1 << n];
        int inf = 1 << 30;
        Array.Fill(f, inf);
        f[0] = 0;
        for (int i = 0; i < 1 << n; ++i) {
            if (f[i] == inf) {
                continue;
            }
            HashSet<int> s = new();
            int mask = 0;
            for (int j = 0; j < n; ++j) {
                if ((i >> j & 1) == 0 && !s.Contains(nums[j])) {
                    s.Add(nums[j]);
                    mask |= 1 << j;
                }
            }
            if (s.Count < m) {
                continue;
            }
            for (int j = mask; j > 0; j = (j - 1) & mask) {
                if (g[j] != -1) {
                    f[i | j] = Math.Min(f[i | j], f[i] + g[j]);
                }
            }
        }
        return f[(1 << n) - 1] == inf ? -1 : f[(1 << n) - 1];
    }

    private int bitCount(int x) {
        int cnt = 0;
        while (x > 0) {
            x &= x - 1;
            ++cnt;
        }
        return cnt;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
