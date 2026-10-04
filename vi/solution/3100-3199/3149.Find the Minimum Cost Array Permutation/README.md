---
comments: true
difficulty: Hard
rating: 2641
source: Weekly Contest 397 Q4
tags:
    - Bit Manipulation
    - Array
    - Dynamic Programming
    - Bitmask
---

<!-- problem:start -->

# [3149. Find the Minimum Cost Array Permutation](https://leetcode.com/problems/find-the-minimum-cost-array-permutation)

[中文文档](/solution/3100-3199/3149.Find%20the%20Minimum%20Cost%20Array%20Permutation/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng <code>nums</code>, đây là một <span data-keyword="permutation">hoán vị</span> của <code>[0, 1, 2, ..., n - 1]</code>. <strong>score</strong> của một hoán vị bất kỳ của <code>[0, 1, 2, ..., n - 1]</code> có tên <code>perm</code> được định nghĩa như sau:</p>

<p><code>score(perm) = |perm[0] - nums[perm[1]]| + |perm[1] - nums[perm[2]]| + ... + |perm[n - 1] - nums[perm[0]]|</code></p>

<p>Trả về hoán vị <code>perm</code> có <strong>score</strong> nhỏ nhất có thể. Nếu có <em>nhiều</em> hoán vị đạt score này, hãy trả về hoán vị <span data-keyword="lexicographically-smaller-array">nhỏ nhất theo thứ tự từ điển</span>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,0,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0,1,2]</span></p>

<p><strong>Giải thích:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3100-3199/3149.Find%20the%20Minimum%20Cost%20Array%20Permutation/images/example0gif.gif" style="width: 235px; height: 235px;" /></strong></p>

<p>Hoán vị nhỏ nhất theo thứ tự từ điển có cost nhỏ nhất là <code>[0,1,2]</code>. Cost của hoán vị này là <code>|0 - 0| + |1 - 2| + |2 - 1| = 2</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [0,2,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0,2,1]</span></p>

<p><strong>Giải thích:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3100-3199/3149.Find%20the%20Minimum%20Cost%20Array%20Permutation/images/example1gif.gif" style="width: 235px; height: 235px;" /></strong></p>

<p>Hoán vị nhỏ nhất theo thứ tự từ điển có cost nhỏ nhất là <code>[0,2,1]</code>. Cost của hoán vị này là <code>|0 - 1| + |2 - 2| + |1 - 0| = 2</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n == nums.length &lt;= 14</code></li>
	<li><code>nums</code> là một hoán vị của <code>[0, 1, 2, ..., n - 1]</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có memoization

<!-- thinking:start -->

> **Tư duy**
>
> score là tổng của $|perm[i]-nums[perm[i+1]]|$ quanh chu trình và hoán vị phải nhỏ nhất theo thứ tự từ điển. Vì $n\le 14$, việc duyệt $14!$ hoán vị là không khả thi.
>
> Phép xoay không làm thay đổi score, nên có thể cố định phần tử đầu tiên là $0$. Phần còn lại là DP trên tập con đã dùng $mask$ và giá trị trước đó $pre$.
>
> Memoize $dfs(mask,pre)$ trên các $cur$ chưa dùng, cộng thêm $|pre-nums[cur]|$, rồi khép chu trình về $0$. Dựng lại đường đi nhỏ nhất theo thứ tự từ điển từ các nghiệm tối ưu tương tự.

<!-- thinking:end -->

Ta nhận thấy rằng với mọi hoán vị $\textit{perm}$, nếu dịch vòng sang trái một số lần bất kỳ thì score của hoán vị không thay đổi. Vì bài toán yêu cầu trả về hoán vị nhỏ nhất theo thứ tự từ điển, ta có thể xác định phần tử đầu tiên của hoán vị phải là $0$.

Ngoài ra, vì miền dữ liệu của bài toán không vượt quá $14$, ta có thể dùng phương pháp nén trạng thái để biểu diễn tập hợp các số đã được chọn trong hoán vị hiện tại. Ta dùng một số nhị phân $\textit{mask}$ có độ dài $n$ để biểu diễn tập hợp các số đã chọn, trong đó bit thứ $i$ của $\textit{mask}$ bằng $1$ cho biết số $i$ đã được chọn, còn bằng $0$ cho biết số $i$ chưa được chọn.

Ta xây dựng hàm $\textit{dfs}(\textit{mask}, \textit{pre})$, biểu diễn score nhỏ nhất của hoán vị nhận được khi tập hợp các số đã chọn trong hoán vị hiện tại là $\textit{mask}$ và số được chọn sau cùng là $\textit{pre}$. Ban đầu, ta thêm số $0$ vào hoán vị.

Quá trình tính hàm $\textit{dfs}(\textit{mask}, \textit{pre})$ như sau:

- Nếu số lượng bit $1$ trong biểu diễn nhị phân của $\textit{mask}$ là $n$, tức là $\textit{mask} = 2^n - 1$, điều đó có nghĩa là tất cả các số đã được chọn, khi đó trả về $|\textit{pre} - \textit{nums}[0]|$;
- Ngược lại, ta liệt kê số tiếp theo $\textit{cur}$ được chọn. Nếu số $\textit{cur}$ chưa được chọn thì ta có thể thêm số $\textit{cur}$ vào hoán vị. Khi đó, score của hoán vị là $|\textit{pre} - \textit{nums}[\textit{cur}]| + \textit{dfs}(\textit{mask} \, | \, 1 << \textit{cur}, \textit{cur})$. Ta cần lấy score nhỏ nhất trong tất cả các $\textit{cur}$.

Cuối cùng, ta dùng hàm $\textit{g}(\textit{mask}, \textit{pre})$ để xây dựng hoán vị đạt score nhỏ nhất. Trước tiên, ta thêm số $\textit{pre}$ vào hoán vị, sau đó liệt kê số tiếp theo $\textit{cur}$ được chọn. Nếu số $\textit{cur}$ chưa được chọn và thỏa mãn $|\textit{pre} - \textit{nums}[\textit{cur}]| + \textit{dfs}(\textit{mask} \, | \, 1 << \textit{cur}, \textit{cur})$ bằng $\textit{dfs}(\textit{mask}, \textit{pre})$, ta có thể thêm số $\textit{cur}$ vào hoán vị.

Độ phức tạp thời gian là $(n^2 \times 2^n)$, còn độ phức tạp không gian là $O(n \times 2^n)$. Trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findPermutation(self, nums: List[int]) -> List[int]:
        @cache
        def dfs(mask: int, pre: int) -> int:
            if mask == (1 << n) - 1:
                return abs(pre - nums[0])
            res = inf
            for cur in range(1, n):
                if mask >> cur & 1 ^ 1:
                    res = min(res, abs(pre - nums[cur]) + dfs(mask | 1 << cur, cur))
            return res

        def g(mask: int, pre: int):
            ans.append(pre)
            if mask == (1 << n) - 1:
                return
            res = dfs(mask, pre)
            for cur in range(1, n):
                if mask >> cur & 1 ^ 1:
                    if abs(pre - nums[cur]) + dfs(mask | 1 << cur, cur) == res:
                        g(mask | 1 << cur, cur)
                        break

        n = len(nums)
        ans = []
        g(1, 0)
        return ans
```

#### Java

```java
class Solution {
    private Integer[][] f;
    private int[] nums;
    private int[] ans;
    private int n;

    public int[] findPermutation(int[] nums) {
        n = nums.length;
        ans = new int[n];
        this.nums = nums;
        f = new Integer[1 << n][n];
        g(1, 0, 0);
        return ans;
    }

    private int dfs(int mask, int pre) {
        if (mask == (1 << n) - 1) {
            return Math.abs(pre - nums[0]);
        }
        if (f[mask][pre] != null) {
            return f[mask][pre];
        }
        int res = Integer.MAX_VALUE;
        for (int cur = 1; cur < n; ++cur) {
            if ((mask >> cur & 1) == 0) {
                res = Math.min(res, Math.abs(pre - nums[cur]) + dfs(mask | 1 << cur, cur));
            }
        }
        return f[mask][pre] = res;
    }

    private void g(int mask, int pre, int k) {
        ans[k] = pre;
        if (mask == (1 << n) - 1) {
            return;
        }
        int res = dfs(mask, pre);
        for (int cur = 1; cur < n; ++cur) {
            if ((mask >> cur & 1) == 0) {
                if (Math.abs(pre - nums[cur]) + dfs(mask | 1 << cur, cur) == res) {
                    g(mask | 1 << cur, cur, k + 1);
                    break;
                }
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> findPermutation(vector<int>& nums) {
        int n = nums.size();
        vector<int> ans;
        int f[1 << n][n];
        memset(f, -1, sizeof(f));
        function<int(int, int)> dfs = [&](int mask, int pre) {
            if (mask == (1 << n) - 1) {
                return abs(pre - nums[0]);
            }
            int* res = &f[mask][pre];
            if (*res != -1) {
                return *res;
            }
            *res = INT_MAX;
            for (int cur = 1; cur < n; ++cur) {
                if (mask >> cur & 1 ^ 1) {
                    *res = min(*res, abs(pre - nums[cur]) + dfs(mask | 1 << cur, cur));
                }
            }
            return *res;
        };
        function<void(int, int)> g = [&](int mask, int pre) {
            ans.push_back(pre);
            if (mask == (1 << n) - 1) {
                return;
            }
            int res = dfs(mask, pre);
            for (int cur = 1; cur < n; ++cur) {
                if (mask >> cur & 1 ^ 1) {
                    if (abs(pre - nums[cur]) + dfs(mask | 1 << cur, cur) == res) {
                        g(mask | 1 << cur, cur);
                        break;
                    }
                }
            }
        };
        g(1, 0);
        return ans;
    }
};
```

#### Go

```go
func findPermutation(nums []int) (ans []int) {
	n := len(nums)
	f := make([][]int, 1<<n)
	for i := range f {
		f[i] = make([]int, n)
		for j := range f[i] {
			f[i][j] = -1
		}
	}
	var dfs func(int, int) int
	dfs = func(mask, pre int) int {
		if mask == 1<<n-1 {
			return abs(pre - nums[0])
		}
		if f[mask][pre] != -1 {
			return f[mask][pre]
		}
		res := &f[mask][pre]
		*res = math.MaxInt32
		for cur := 1; cur < n; cur++ {
			if mask>>cur&1 == 0 {
				*res = min(*res, abs(pre-nums[cur])+dfs(mask|1<<cur, cur))
			}
		}
		return *res
	}
	var g func(int, int)
	g = func(mask, pre int) {
		ans = append(ans, pre)
		if mask == 1<<n-1 {
			return
		}
		res := dfs(mask, pre)
		for cur := 1; cur < n; cur++ {
			if mask>>cur&1 == 0 {
				if abs(pre-nums[cur])+dfs(mask|1<<cur, cur) == res {
					g(mask|1<<cur, cur)
					break
				}
			}
		}
	}
	g(1, 0)
	return
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
function findPermutation(nums: number[]): number[] {
    const n = nums.length;
    const ans: number[] = [];
    const f: number[][] = Array.from({ length: 1 << n }, () => Array(n).fill(-1));
    const dfs = (mask: number, pre: number): number => {
        if (mask === (1 << n) - 1) {
            return Math.abs(pre - nums[0]);
        }
        if (f[mask][pre] !== -1) {
            return f[mask][pre];
        }
        let res = Infinity;
        for (let cur = 1; cur < n; ++cur) {
            if (((mask >> cur) & 1) ^ 1) {
                res = Math.min(res, Math.abs(pre - nums[cur]) + dfs(mask | (1 << cur), cur));
            }
        }
        return (f[mask][pre] = res);
    };
    const g = (mask: number, pre: number) => {
        ans.push(pre);
        if (mask === (1 << n) - 1) {
            return;
        }
        const res = dfs(mask, pre);
        for (let cur = 1; cur < n; ++cur) {
            if (((mask >> cur) & 1) ^ 1) {
                if (Math.abs(pre - nums[cur]) + dfs(mask | (1 << cur), cur) === res) {
                    g(mask | (1 << cur), cur);
                    break;
                }
            }
        }
    };
    g(1, 0);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
