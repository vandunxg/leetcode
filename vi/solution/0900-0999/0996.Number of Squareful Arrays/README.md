---
comments: true
difficulty: Hard
tags:
    - Bit Manipulation
    - Array
    - Hash Table
    - Math
    - Dynamic Programming
    - Backtracking
    - Bitmask
---

<!-- problem:start -->

# [996. Number of Squareful Arrays](https://leetcode.com/problems/number-of-squareful-arrays)

[中文文档](/solution/0900-0999/0996.Number%20of%20Squareful%20Arrays/README.md)

## Mô tả

<!-- description:start -->

<p>Một mảng được gọi là <strong>squareful</strong> nếu tổng của mọi cặp phần tử liền kề đều là <strong>số chính phương</strong>.</p>

<p>Cho mảng số nguyên nums, trả về <em>số hoán vị của </em><code>nums</code><em> có tính chất <strong>squareful</strong></em>.</p>

<p>Hai hoán vị <code>perm1</code> và <code>perm2</code> được xem là khác nhau nếu tồn tại chỉ số <code>i</code> sao cho <code>perm1[i] != perm2[i]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,17,8]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> [1,8,17] và [17,8,1] là các hoán vị hợp lệ.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,2,2]
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 12</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các hoán vị sao cho tổng của mọi cặp phần tử liền kề là số chính phương. Vì $n\le 12$, có thể dùng subset DP. $f[S][j]$ là số cách dùng các phần tử trong $S$ và kết thúc tại $j$; chỉ chuyển trạng thái từ $k$ khi $nums[j]+nums[k]$ là số chính phương. Các phần tử trùng nhau bị đếm lặp theo chỉ số, nên chia kết quả cho giai thừa của số lần xuất hiện của từng giá trị.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numSquarefulPerms(self, nums: List[int]) -> int:
        n = len(nums)
        f = [[0] * n for _ in range(1 << n)]
        for j in range(n):
            f[1 << j][j] = 1
        for i in range(1 << n):
            for j in range(n):
                if i >> j & 1:
                    for k in range(n):
                        if (i >> k & 1) and k != j:
                            s = nums[j] + nums[k]
                            t = int(sqrt(s))
                            if t * t == s:
                                f[i][j] += f[i ^ (1 << j)][k]

        ans = sum(f[(1 << n) - 1][j] for j in range(n))
        for v in Counter(nums).values():
            ans //= factorial(v)
        return ans
```

#### Java

```java
class Solution {
    public int numSquarefulPerms(int[] nums) {
        int n = nums.length;
        int[][] f = new int[1 << n][n];
        for (int j = 0; j < n; ++j) {
            f[1 << j][j] = 1;
        }
        for (int i = 0; i < 1 << n; ++i) {
            for (int j = 0; j < n; ++j) {
                if ((i >> j & 1) == 1) {
                    for (int k = 0; k < n; ++k) {
                        if ((i >> k & 1) == 1 && k != j) {
                            int s = nums[j] + nums[k];
                            int t = (int) Math.sqrt(s);
                            if (t * t == s) {
                                f[i][j] += f[i ^ (1 << j)][k];
                            }
                        }
                    }
                }
            }
        }
        long ans = 0;
        for (int j = 0; j < n; ++j) {
            ans += f[(1 << n) - 1][j];
        }
        Map<Integer, Integer> cnt = new HashMap<>();
        for (int x : nums) {
            cnt.merge(x, 1, Integer::sum);
        }
        int[] g = new int[13];
        g[0] = 1;
        for (int i = 1; i < 13; ++i) {
            g[i] = g[i - 1] * i;
        }
        for (int v : cnt.values()) {
            ans /= g[v];
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numSquarefulPerms(vector<int>& nums) {
        int n = nums.size();
        int f[1 << n][n];
        memset(f, 0, sizeof(f));
        for (int j = 0; j < n; ++j) {
            f[1 << j][j] = 1;
        }
        for (int i = 0; i < 1 << n; ++i) {
            for (int j = 0; j < n; ++j) {
                if ((i >> j & 1) == 1) {
                    for (int k = 0; k < n; ++k) {
                        if ((i >> k & 1) == 1 && k != j) {
                            int s = nums[j] + nums[k];
                            int t = sqrt(s);
                            if (t * t == s) {
                                f[i][j] += f[i ^ (1 << j)][k];
                            }
                        }
                    }
                }
            }
        }
        long long ans = 0;
        for (int j = 0; j < n; ++j) {
            ans += f[(1 << n) - 1][j];
        }
        unordered_map<int, int> cnt;
        for (int x : nums) {
            ++cnt[x];
        }
        int g[13] = {1};
        for (int i = 1; i < 13; ++i) {
            g[i] = g[i - 1] * i;
        }
        for (auto& [_, v] : cnt) {
            ans /= g[v];
        }
        return ans;
    }
};
```

#### Go

```go
func numSquarefulPerms(nums []int) (ans int) {
	n := len(nums)
	f := make([][]int, 1<<n)
	for i := range f {
		f[i] = make([]int, n)
	}
	for j := range nums {
		f[1<<j][j] = 1
	}
	for i := 0; i < 1<<n; i++ {
		for j := 0; j < n; j++ {
			if i>>j&1 == 1 {
				for k := 0; k < n; k++ {
					if i>>k&1 == 1 && k != j {
						s := nums[j] + nums[k]
						t := int(math.Sqrt(float64(s)))
						if t*t == s {
							f[i][j] += f[i^(1<<j)][k]
						}
					}
				}
			}
		}
	}
	for j := 0; j < n; j++ {
		ans += f[(1<<n)-1][j]
	}
	g := [13]int{1}
	for i := 1; i < 13; i++ {
		g[i] = g[i-1] * i
	}
	cnt := map[int]int{}
	for _, x := range nums {
		cnt[x]++
	}
	for _, v := range cnt {
		ans /= g[v]
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
