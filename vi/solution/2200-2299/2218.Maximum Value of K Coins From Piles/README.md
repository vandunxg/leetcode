---
comments: true
difficulty: Hard
rating: 2157
source: Weekly Contest 286 Q4
tags:
    - Array
    - Dynamic Programming
    - Prefix Sum
---

<!-- problem:start -->

# [2218. Maximum Value of K Coins From Piles](https://leetcode.com/problems/maximum-value-of-k-coins-from-piles)

[中文文档](/solution/2200-2299/2218.Maximum%20Value%20of%20K%20Coins%20From%20Piles/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> <strong>chồng xu</strong> trên bàn. Mỗi chồng gồm một <strong>số lượng dương</strong> các đồng xu với những mệnh giá khác nhau.</p>

<p>Trong một lượt, bạn có thể chọn đồng xu nằm trên <strong>đỉnh</strong> của bất kỳ chồng nào, lấy nó ra và bỏ vào ví.</p>

<p>Cho một danh sách <code>piles</code>, trong đó <code>piles[i]</code> là một danh sách các số nguyên biểu diễn thành phần của chồng thứ <code>i<sup>th</sup></code> theo thứ tự <strong>từ trên xuống dưới</strong>, cùng một số nguyên dương <code>k</code>. Hãy trả về <em><strong>tổng giá trị lớn nhất</strong> của các đồng xu trong ví nếu bạn chọn <strong>chính xác</strong></em> <code>k</code> <em>đồng xu một cách tối ưu</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2218.Maximum%20Value%20of%20K%20Coins%20From%20Piles/images/e1.png" style="width: 600px; height: 243px;" />
<pre>
<strong>Đầu vào:</strong> piles = [[1,100,3],[7,8,9]], k = 2
<strong>Đầu ra:</strong> 101
<strong>Giải thích:</strong>
Hình minh họa phía trên cho thấy những cách khác nhau để chọn k đồng xu.
Tổng giá trị lớn nhất có thể đạt được là 101.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> piles = [[100],[100],[100],[100],[100],[100],[1,1,1,1,1,1,700]], k = 7
<strong>Đầu ra:</strong> 706
<strong>Giải thích:
</strong>Tổng giá trị lớn nhất đạt được nếu ta chọn tất cả các đồng xu từ chồng cuối cùng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == piles.length</code></li>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
	<li><code>1 &lt;= piles[i][j] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k &lt;= sum(piles[i].length) &lt;= 2000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động (Balo nhóm)

<!-- thinking:start -->

> **Tư duy**
>
> Vì chỉ có thể lấy các đồng xu theo một tiền tố của mỗi chồng, tổng số xu lấy ra không vượt quá $k$. Nếu liệt kê các cách phân bổ, ta sẽ tính lại cùng một tiền tố nhiều lần; trong khi $n$ và $k$ có thể lên tới một nghìn, còn tổng số xu là $2000$. Các chồng là những nhóm độc lập: lấy $h$ đồng xu đầu tiên của một chồng có thể xem như một vật phẩm có trọng lượng $h$.
>
> Gọi $f[i][j]$ là giá trị lớn nhất có thể đạt được từ $i$ chồng đầu tiên khi lấy $j$ đồng xu. Ta tính tổng tiền tố của chồng $i$ vào $s[h]$, rồi cập nhật $f[i][j]$ bằng $f[i-1][j-h]+s[h]$. Độ phức tạp thời gian tỉ lệ với $k$ nhân tổng số đồng xu.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là tổng giá trị lớn nhất khi lấy $j$ đồng xu từ $i$ chồng đầu tiên. Đáp án là $f[n][k]$, trong đó $n$ là số chồng xu.

Với chồng thứ $i$, ta có thể chọn lấy $0$, $1$, $2$, $\cdots$, $k$ đồng xu đầu tiên. Ta sử dụng mảng tổng tiền tố $s$ để nhanh chóng tính tổng giá trị khi lấy $h$ đồng xu đầu tiên.

Công thức chuyển trạng thái là:

$$
f[i][j] = \max(f[i][j], f[i - 1][j - h] + s[h])
$$

trong đó $0 \leq h \leq j$, và $s[h]$ biểu diễn tổng giá trị khi lấy $h$ đồng xu đầu tiên từ chồng thứ $i$.

Độ phức tạp thời gian là $O(k \times L)$, độ phức tạp không gian là $O(n \times k)$. Ở đây, $L$ là tổng số đồng xu và $n$ là số chồng xu.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxValueOfCoins(self, piles: List[List[int]], k: int) -> int:
        n = len(piles)
        f = [[0] * (k + 1) for _ in range(n + 1)]
        for i, nums in enumerate(piles, 1):
            s = list(accumulate(nums, initial=0))
            for j in range(k + 1):
                for h, w in enumerate(s):
                    if j < h:
                        break
                    f[i][j] = max(f[i][j], f[i - 1][j - h] + w)
        return f[n][k]
```

#### Java

```java
class Solution {
    public int maxValueOfCoins(List<List<Integer>> piles, int k) {
        int n = piles.size();
        int[][] f = new int[n + 1][k + 1];
        for (int i = 1; i <= n; i++) {
            List<Integer> nums = piles.get(i - 1);
            int[] s = new int[nums.size() + 1];
            s[0] = 0;
            for (int j = 1; j <= nums.size(); j++) {
                s[j] = s[j - 1] + nums.get(j - 1);
            }
            for (int j = 0; j <= k; j++) {
                for (int h = 0; h < s.length && h <= j; h++) {
                    f[i][j] = Math.max(f[i][j], f[i - 1][j - h] + s[h]);
                }
            }
        }
        return f[n][k];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxValueOfCoins(vector<vector<int>>& piles, int k) {
        int n = piles.size();
        vector<vector<int>> f(n + 1, vector<int>(k + 1));
        for (int i = 1; i <= n; i++) {
            vector<int> nums = piles[i - 1];
            vector<int> s(nums.size() + 1);
            for (int j = 1; j <= nums.size(); j++) {
                s[j] = s[j - 1] + nums[j - 1];
            }
            for (int j = 0; j <= k; j++) {
                for (int h = 0; h < s.size() && h <= j; h++) {
                    f[i][j] = max(f[i][j], f[i - 1][j - h] + s[h]);
                }
            }
        }
        return f[n][k];
    }
};
```

#### Go

```go
func maxValueOfCoins(piles [][]int, k int) int {
	n := len(piles)
	f := make([][]int, n+1)
	for i := range f {
		f[i] = make([]int, k+1)
	}
	for i := 1; i <= n; i++ {
		nums := piles[i-1]
		s := make([]int, len(nums)+1)
		for j := 1; j <= len(nums); j++ {
			s[j] = s[j-1] + nums[j-1]
		}

		for j := 0; j <= k; j++ {
			for h, w := range s {
				if j < h {
					break
				}
				f[i][j] = max(f[i][j], f[i-1][j-h]+w)
			}
		}
	}
	return f[n][k]
}
```

#### TypeScript

```ts
function maxValueOfCoins(piles: number[][], k: number): number {
    const n = piles.length;
    const f: number[][] = Array.from({ length: n + 1 }, () => Array(k + 1).fill(0));
    for (let i = 1; i <= n; i++) {
        const nums = piles[i - 1];
        const s = Array(nums.length + 1).fill(0);
        for (let j = 1; j <= nums.length; j++) {
            s[j] = s[j - 1] + nums[j - 1];
        }
        for (let j = 0; j <= k; j++) {
            for (let h = 0; h < s.length && h <= j; h++) {
                f[i][j] = Math.max(f[i][j], f[i - 1][j - h] + s[h]);
            }
        }
    }
    return f[n][k];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động (Tối ưu không gian)

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 chỉ đọc hàng trước đó $f[i-1][\cdot]$, nên không cần lưu toàn bộ bảng. Sau khi làm phẳng $f$, phải duyệt sức chứa $j$ theo thứ tự giảm dần để không dùng cùng một chồng hai lần.
>
> Mỗi chồng vẫn được tính tổng tiền tố, sau đó $j$ và $h$ cập nhật trực tiếp $f[j]$. Thời gian không đổi, còn không gian bổ sung giảm xuống $O(k)$.

<!-- thinking:end -->

Ta nhận thấy với chồng thứ $i$, ta chỉ cần sử dụng $f[i - 1][j]$ và $f[i][j - h]$, vì vậy có thể tối ưu mảng hai chiều thành mảng một chiều.

Độ phức tạp thời gian là $O(k \times L)$, độ phức tạp không gian là $O(k)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxValueOfCoins(self, piles: List[List[int]], k: int) -> int:
        f = [0] * (k + 1)
        for nums in piles:
            s = list(accumulate(nums, initial=0))
            for j in range(k, -1, -1):
                for h, w in enumerate(s):
                    if j < h:
                        break
                    f[j] = max(f[j], f[j - h] + w)
        return f[k]
```

#### Java

```java
class Solution {
    public int maxValueOfCoins(List<List<Integer>> piles, int k) {
        int[] f = new int[k + 1];
        for (var nums : piles) {
            int[] s = new int[nums.size() + 1];
            for (int j = 1; j <= nums.size(); ++j) {
                s[j] = s[j - 1] + nums.get(j - 1);
            }
            for (int j = k; j >= 0; --j) {
                for (int h = 0; h < s.length && h <= j; ++h) {
                    f[j] = Math.max(f[j], f[j - h] + s[h]);
                }
            }
        }
        return f[k];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxValueOfCoins(vector<vector<int>>& piles, int k) {
        vector<int> f(k + 1);
        for (auto& nums : piles) {
            vector<int> s(nums.size() + 1);
            for (int j = 1; j <= nums.size(); ++j) {
                s[j] = s[j - 1] + nums[j - 1];
            }
            for (int j = k; j >= 0; --j) {
                for (int h = 0; h < s.size() && h <= j; ++h) {
                    f[j] = max(f[j], f[j - h] + s[h]);
                }
            }
        }
        return f[k];
    }
};
```

#### Go

```go
func maxValueOfCoins(piles [][]int, k int) int {
	f := make([]int, k+1)
	for _, nums := range piles {
		s := make([]int, len(nums)+1)
		for j := 1; j <= len(nums); j++ {
			s[j] = s[j-1] + nums[j-1]
		}
		for j := k; j >= 0; j-- {
			for h := 0; h < len(s) && h <= j; h++ {
				f[j] = max(f[j], f[j-h]+s[h])
			}
		}
	}
	return f[k]
}
```

#### TypeScript

```ts
function maxValueOfCoins(piles: number[][], k: number): number {
    const f: number[] = Array(k + 1).fill(0);
    for (const nums of piles) {
        const s: number[] = Array(nums.length + 1).fill(0);
        for (let j = 1; j <= nums.length; j++) {
            s[j] = s[j - 1] + nums[j - 1];
        }
        for (let j = k; j >= 0; j--) {
            for (let h = 0; h < s.length && h <= j; h++) {
                f[j] = Math.max(f[j], f[j - h] + s[h]);
            }
        }
    }
    return f[k];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
