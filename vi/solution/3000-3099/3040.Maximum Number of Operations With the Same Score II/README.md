---
comments: true
difficulty: Medium
rating: 1708
source: Biweekly Contest 124 Q3
tags:
    - Memoization
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3040. Maximum Number of Operations With the Same Score II](https://leetcode.com/problems/maximum-number-of-operations-with-the-same-score-ii)

[中文文档](/solution/3000-3099/3040.Maximum%20Number%20of%20Operations%20With%20the%20Same%20Score%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên có tên là <code>nums</code>, bạn có thể thực hiện <strong>bất kỳ</strong> thao tác nào sau đây khi <code>nums</code> còn <strong>ít nhất</strong> <code>2</code> phần tử:</p>

<ul>
	<li>Chọn và xóa hai phần tử đầu tiên của <code>nums</code>.</li>
	<li>Chọn và xóa hai phần tử cuối cùng của <code>nums</code>.</li>
	<li>Chọn và xóa phần tử đầu tiên và phần tử cuối cùng của <code>nums</code>.</li>
</ul>

<p><strong>Điểm số</strong> của một thao tác là tổng các phần tử bị xóa.</p>

<p>Nhiệm vụ của bạn là tìm <strong>số thao tác lớn nhất</strong> có thể thực hiện sao cho <strong>tất cả các thao tác có cùng điểm số</strong>.</p>

<p>Hãy trả về <em><strong>số thao tác lớn nhất</strong> có thể thực hiện thỏa mãn điều kiện trên</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,2,1,2,3,4]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ta thực hiện các thao tác sau:
- Xóa hai phần tử đầu tiên, với điểm số 3 + 2 = 5, nums = [1,2,3,4].
- Xóa phần tử đầu tiên và phần tử cuối cùng, với điểm số 1 + 4 = 5, nums = [2,3].
- Xóa phần tử đầu tiên và phần tử cuối cùng, với điểm số 2 + 3 = 5, nums = [].
Vì nums rỗng nên ta không thể thực hiện thêm thao tác nào.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,2,6,1,4]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ta thực hiện các thao tác sau:
- Xóa hai phần tử đầu tiên, với điểm số 3 + 2 = 5, nums = [6,1,4].
- Xóa hai phần tử cuối cùng, với điểm số 1 + 4 = 5, nums = [6].
Có thể chứng minh rằng ta thực hiện được nhiều nhất 2 thao tác.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 2000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Khác với phần I, mỗi thao tác có thể xóa hai phần tử ở đầu, hai phần tử ở cuối hoặc một phần tử ở mỗi đầu, nhưng điểm số phải không đổi. $n \le 2000$.
>
> Thao tác đầu tiên có ba lựa chọn, tương ứng với ba điểm số khả dĩ $s$. Sau đó, số thao tác lớn nhất trên $[i,j]$ chỉ phụ thuộc vào $s$.
>
> Với mỗi $s$, ta ghi nhớ kết quả của $\textit{dfs}(i,j)$ khi xét ba cách xóa phù hợp với $s$. Đáp án bằng $1$ cộng với kết quả tốt nhất trong ba lựa chọn đầu tiên.

<!-- thinking:end -->

Có ba giá trị có thể có của điểm số $s$, lần lượt là $s = nums[0] + nums[1]$, $s = nums[0] + nums[n-1]$ và $s = nums[n-1] + nums[n-2]$. Ta có thể thực hiện tìm kiếm có ghi nhớ riêng cho ba trường hợp này.

Ta xây dựng hàm $dfs(i, j)$, biểu diễn số thao tác lớn nhất có thể thực hiện trên các chỉ số từ $i$ đến $j$ khi điểm số là $s$. Hàm $dfs(i, j)$ được thực hiện như sau:

- Nếu $j - i < 1$, nghĩa là độ dài đoạn $[i, j]$ nhỏ hơn $2$ và không thể thực hiện thao tác nào, nên trả về $0$.
- Nếu $nums[i] + nums[i+1] = s$, nghĩa là có thể xóa phần tử ở chỉ số $i$ và $i+1$. Khi đó, số thao tác lớn nhất là $1 + dfs(i+2, j)$.
- Nếu $nums[i] + nums[j] = s$, nghĩa là có thể xóa phần tử ở chỉ số $i$ và $j$. Khi đó, số thao tác lớn nhất là $1 + dfs(i+1, j-1)$.
- Nếu $nums[j-1] + nums[j] = s$, nghĩa là có thể xóa phần tử ở chỉ số $j-1$ và $j$. Khi đó, số thao tác lớn nhất là $1 + dfs(i, j-2)$.
- Trả về giá trị lớn nhất trong các trường hợp trên.

Cuối cùng, ta tính số thao tác lớn nhất cho ba trường hợp riêng biệt rồi trả về giá trị lớn nhất.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n^2)$. Trong đó, $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxOperations(self, nums: List[int]) -> int:
        @cache
        def dfs(i: int, j: int, s: int) -> int:
            if j - i < 1:
                return 0
            ans = 0
            if nums[i] + nums[i + 1] == s:
                ans = max(ans, 1 + dfs(i + 2, j, s))
            if nums[i] + nums[j] == s:
                ans = max(ans, 1 + dfs(i + 1, j - 1, s))
            if nums[j - 1] + nums[j] == s:
                ans = max(ans, 1 + dfs(i, j - 2, s))
            return ans

        n = len(nums)
        a = dfs(2, n - 1, nums[0] + nums[1])
        b = dfs(0, n - 3, nums[-1] + nums[-2])
        c = dfs(1, n - 2, nums[0] + nums[-1])
        return 1 + max(a, b, c)
```

#### Java

```java
class Solution {
    private Integer[][] f;
    private int[] nums;
    private int s;
    private int n;

    public int maxOperations(int[] nums) {
        this.nums = nums;
        n = nums.length;
        int a = g(2, n - 1, nums[0] + nums[1]);
        int b = g(0, n - 3, nums[n - 2] + nums[n - 1]);
        int c = g(1, n - 2, nums[0] + nums[n - 1]);
        return 1 + Math.max(a, Math.max(b, c));
    }

    private int g(int i, int j, int s) {
        f = new Integer[n][n];
        this.s = s;
        return dfs(i, j);
    }

    private int dfs(int i, int j) {
        if (j - i < 1) {
            return 0;
        }
        if (f[i][j] != null) {
            return f[i][j];
        }
        int ans = 0;
        if (nums[i] + nums[i + 1] == s) {
            ans = Math.max(ans, 1 + dfs(i + 2, j));
        }
        if (nums[i] + nums[j] == s) {
            ans = Math.max(ans, 1 + dfs(i + 1, j - 1));
        }
        if (nums[j - 1] + nums[j] == s) {
            ans = Math.max(ans, 1 + dfs(i, j - 2));
        }
        return f[i][j] = ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxOperations(vector<int>& nums) {
        int n = nums.size();
        int f[n][n];
        auto g = [&](int i, int j, int s) -> int {
            memset(f, -1, sizeof(f));
            function<int(int, int)> dfs = [&](int i, int j) -> int {
                if (j - i < 1) {
                    return 0;
                }
                if (f[i][j] != -1) {
                    return f[i][j];
                }
                int ans = 0;
                if (nums[i] + nums[i + 1] == s) {
                    ans = max(ans, 1 + dfs(i + 2, j));
                }
                if (nums[i] + nums[j] == s) {
                    ans = max(ans, 1 + dfs(i + 1, j - 1));
                }
                if (nums[j - 1] + nums[j] == s) {
                    ans = max(ans, 1 + dfs(i, j - 2));
                }
                return f[i][j] = ans;
            };
            return dfs(i, j);
        };
        int a = g(2, n - 1, nums[0] + nums[1]);
        int b = g(0, n - 3, nums[n - 2] + nums[n - 1]);
        int c = g(1, n - 2, nums[0] + nums[n - 1]);
        return 1 + max({a, b, c});
    }
};
```

#### Go

```go
func maxOperations(nums []int) int {
	n := len(nums)
	var g func(i, j, s int) int
	g = func(i, j, s int) int {
		f := make([][]int, n)
		for i := range f {
			f[i] = make([]int, n)
			for j := range f {
				f[i][j] = -1
			}
		}
		var dfs func(i, j int) int
		dfs = func(i, j int) int {
			if j-i < 1 {
				return 0
			}
			if f[i][j] != -1 {
				return f[i][j]
			}
			ans := 0
			if nums[i]+nums[i+1] == s {
				ans = max(ans, 1+dfs(i+2, j))
			}

			if nums[i]+nums[j] == s {
				ans = max(ans, 1+dfs(i+1, j-1))
			}

			if nums[j-1]+nums[j] == s {
				ans = max(ans, 1+dfs(i, j-2))
			}
			f[i][j] = ans
			return ans
		}
		return dfs(i, j)
	}
	a := g(2, n-1, nums[0]+nums[1])
	b := g(0, n-3, nums[n-1]+nums[n-2])
	c := g(1, n-2, nums[0]+nums[n-1])
	return 1 + max(a, b, c)
}
```

#### TypeScript

```ts
function maxOperations(nums: number[]): number {
    const n = nums.length;
    const f: number[][] = Array.from({ length: n }, () => Array(n));
    const g = (i: number, j: number, s: number): number => {
        f.forEach(row => row.fill(-1));
        const dfs = (i: number, j: number): number => {
            if (j - i < 1) {
                return 0;
            }
            if (f[i][j] !== -1) {
                return f[i][j];
            }
            let ans = 0;
            if (nums[i] + nums[i + 1] === s) {
                ans = Math.max(ans, 1 + dfs(i + 2, j));
            }
            if (nums[i] + nums[j] === s) {
                ans = Math.max(ans, 1 + dfs(i + 1, j - 1));
            }
            if (nums[j - 1] + nums[j] === s) {
                ans = Math.max(ans, 1 + dfs(i, j - 2));
            }
            return (f[i][j] = ans);
        };
        return dfs(i, j);
    };
    const a = g(2, n - 1, nums[0] + nums[1]);
    const b = g(0, n - 3, nums[n - 2] + nums[n - 1]);
    const c = g(1, n - 2, nums[0] + nums[n - 1]);
    return 1 + Math.max(a, b, c);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
