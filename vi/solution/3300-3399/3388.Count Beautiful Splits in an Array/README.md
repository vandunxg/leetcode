---
comments: true
difficulty: Medium
rating: 2364
source: Weekly Contest 428 Q3
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3388. Count Beautiful Splits in an Array](https://leetcode.com/problems/count-beautiful-splits-in-an-array)

[中文文档](/solution/3300-3399/3388.Count%20Beautiful%20Splits%20in%20an%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng <code>nums</code>.</p>

<p>Một phép chia mảng <code>nums</code> được gọi là <strong>đẹp</strong> nếu:</p>

<ol>
	<li>Mảng <code>nums</code> được chia thành ba <span data-keyword="subarray-nonempty">mảng con</span>: <code>nums1</code>, <code>nums2</code> và <code>nums3</code>, sao cho có thể tạo thành <code>nums</code> bằng cách nối <code>nums1</code>, <code>nums2</code> và <code>nums3</code> theo thứ tự đó.</li>
	<li>Mảng con <code>nums1</code> là một <span data-keyword="array-prefix">tiền tố</span> của <code>nums2</code> <strong>HOẶC</strong> <code>nums2</code> là một <span data-keyword="array-prefix">tiền tố</span> của <code>nums3</code>.</li>
</ol>

<p>Trả về <strong>số cách</strong> có thể thực hiện phép chia này.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1,2,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các phép chia đẹp là:</p>

<ol>
	<li>Một phép chia với <code>nums1 = [1]</code>, <code>nums2 = [1,2]</code>, <code>nums3 = [1]</code>.</li>
	<li>Một phép chia với <code>nums1 = [1]</code>, <code>nums2 = [1]</code>, <code>nums3 = [2,1]</code>.</li>
</ol>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có 0 phép chia đẹp.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 5000</code></li>
	<li><code><font face="monospace">0 &lt;= nums[i] &lt;= 50</font></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: LCP + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Ta chia mảng thành ba phần sao cho phần thứ nhất là tiền tố của phần thứ hai, hoặc phần thứ hai là tiền tố của phần thứ ba. Với $n \le 5000$, hai vị trí cắt tạo ra $O(n^2)$ trường hợp, nhưng cách so sánh tiền tố ngây thơ sẽ thêm một thừa số tuyến tính.
>
> $\textit{lcp}[i][j]$ là LCP của hai hậu tố, được xây dựng từ phía sau dựa trên $\textit{lcp}[i+1][j+1]$, sau đó việc so sánh chỉ mất $O(1)$.
>
> Các vị trí cắt $(i,j)$ là đẹp khi $\textit{lcp}[0][i] \ge i$ hoặc $\textit{lcp}[i][j] \ge j-i$, kèm theo các ràng buộc hiển nhiên về độ dài.

<!-- thinking:end -->

Ta có thể tiền xử lý $\text{LCP}[i][j]$ để biểu diễn độ dài tiền tố chung dài nhất của $\textit{nums}[i:]$ và $\textit{nums}[j:]$. Ban đầu, $\text{LCP}[i][j] = 0$.

Tiếp theo, ta liệt kê $i$ và $j$ theo thứ tự ngược. Với mỗi cặp $i$ và $j$, nếu $\textit{nums}[i] = \textit{nums}[j]$, ta có thể tính $\text{LCP}[i][j] = \text{LCP}[i + 1][j + 1] + 1$.

Cuối cùng, ta liệt kê vị trí kết thúc $i$ của mảng con thứ nhất (không bao gồm vị trí $i$) và vị trí kết thúc $j$ của mảng con thứ hai (không bao gồm vị trí $j$). Độ dài của mảng con thứ nhất là $i$, độ dài của mảng con thứ hai là $j - i$, và độ dài của mảng con thứ ba là $n - j$. Nếu $i \leq j - i$ và $\text{LCP}[0][i] \geq i$, hoặc $j - i \leq n - j$ và $\text{LCP}[i][j] \geq j - i$, thì phép chia này đẹp và ta tăng đáp án lên một.

Sau khi liệt kê, đáp án là số phép chia đẹp.

Độ phức tạp thời gian là $O(n^2)$, còn độ phức tạp không gian là $O(n^2)$. Trong đó, $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def beautifulSplits(self, nums: List[int]) -> int:
        n = len(nums)
        lcp = [[0] * (n + 1) for _ in range(n + 1)]
        for i in range(n - 1, -1, -1):
            for j in range(n - 1, i - 1, -1):
                if nums[i] == nums[j]:
                    lcp[i][j] = lcp[i + 1][j + 1] + 1
        ans = 0
        for i in range(1, n - 1):
            for j in range(i + 1, n):
                a = i <= j - i and lcp[0][i] >= i
                b = j - i <= n - j and lcp[i][j] >= j - i
                ans += int(a or b)
        return ans
```

#### Java

```java
class Solution {
    public int beautifulSplits(int[] nums) {
        int n = nums.length;
        int[][] lcp = new int[n + 1][n + 1];

        for (int i = n - 1; i >= 0; i--) {
            for (int j = n - 1; j > i; j--) {
                if (nums[i] == nums[j]) {
                    lcp[i][j] = lcp[i + 1][j + 1] + 1;
                }
            }
        }

        int ans = 0;
        for (int i = 1; i < n - 1; i++) {
            for (int j = i + 1; j < n; j++) {
                boolean a = (i <= j - i) && (lcp[0][i] >= i);
                boolean b = (j - i <= n - j) && (lcp[i][j] >= j - i);
                if (a || b) {
                    ans++;
                }
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
    int beautifulSplits(vector<int>& nums) {
        int n = nums.size();
        vector<vector<int>> lcp(n + 1, vector<int>(n + 1, 0));

        for (int i = n - 1; i >= 0; i--) {
            for (int j = n - 1; j > i; j--) {
                if (nums[i] == nums[j]) {
                    lcp[i][j] = lcp[i + 1][j + 1] + 1;
                }
            }
        }

        int ans = 0;
        for (int i = 1; i < n - 1; i++) {
            for (int j = i + 1; j < n; j++) {
                bool a = (i <= j - i) && (lcp[0][i] >= i);
                bool b = (j - i <= n - j) && (lcp[i][j] >= j - i);
                if (a || b) {
                    ans++;
                }
            }
        }

        return ans;
    }
};
```

#### Go

```go
func beautifulSplits(nums []int) (ans int) {
    n := len(nums)
    lcp := make([][]int, n+1)
    for i := range lcp {
        lcp[i] = make([]int, n+1)
    }

    for i := n - 1; i >= 0; i-- {
        for j := n - 1; j > i; j-- {
            if nums[i] == nums[j] {
                lcp[i][j] = lcp[i+1][j+1] + 1
            }
        }
    }

    for i := 1; i < n-1; i++ {
        for j := i + 1; j < n; j++ {
            a := i <= j-i && lcp[0][i] >= i
            b := j-i <= n-j && lcp[i][j] >= j-i
            if a || b {
                ans++
            }
        }
    }

    return
}
```

#### TypeScript

```ts
function beautifulSplits(nums: number[]): number {
    const n = nums.length;
    const lcp: number[][] = Array.from({ length: n + 1 }, () => Array(n + 1).fill(0));

    for (let i = n - 1; i >= 0; i--) {
        for (let j = n - 1; j > i; j--) {
            if (nums[i] === nums[j]) {
                lcp[i][j] = lcp[i + 1][j + 1] + 1;
            }
        }
    }

    let ans = 0;
    for (let i = 1; i < n - 1; i++) {
        for (let j = i + 1; j < n; j++) {
            const a = i <= j - i && lcp[0][i] >= i;
            const b = j - i <= n - j && lcp[i][j] >= j - i;
            if (a || b) {
                ans++;
            }
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
