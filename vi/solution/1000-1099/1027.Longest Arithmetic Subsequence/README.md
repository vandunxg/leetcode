---
comments: true
difficulty: Medium
rating: 1758
source: Weekly Contest 132 Q3
tags:
    - Array
    - Hash Table
    - Binary Search
    - Dynamic Programming
---

<!-- problem:start -->

# [1027. Longest Arithmetic Subsequence](https://leetcode.com/problems/longest-arithmetic-subsequence)

[中文文档](/solution/1000-1099/1027.Longest%20Arithmetic%20Subsequence/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code>, hãy trả về độ dài của dãy con số học dài nhất trong <code>nums</code>.</p>

<p><strong>Lưu ý:</strong></p>

<ul>
	<li><strong>Dãy con</strong> là mảng thu được từ một mảng khác bằng cách xóa một số phần tử hoặc không xóa phần tử nào, nhưng vẫn giữ nguyên thứ tự các phần tử còn lại.</li>
	<li>Dãy <code>seq</code> là dãy số học nếu các hiệu <code>seq[i + 1] - seq[i]</code> đều bằng nhau (với <code>0 &lt;= i &lt; seq.length - 1</code>).</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,6,9,12]
<strong>Đầu ra:</strong> 4
<strong>Giải thích: </strong> Toàn bộ mảng là một dãy số học có công sai bằng 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [9,4,7,2,10]
<strong>Đầu ra:</strong> 3
<strong>Giải thích: </strong> Dãy con số học dài nhất là [4,7,10].
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [20,1,15,3,10,5,8]
<strong>Đầu ra:</strong> 4
<strong>Giải thích: </strong> Dãy con số học dài nhất là [20,15,10,5].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 500</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Thử mọi hiệu rồi tìm dãy con vẫn khả thi: $n\le 1000$ và các giá trị nằm trong $[0,500]$, nên số hiệu có thể có chỉ ở mức vài nghìn. Dãy con không cần liên tiếp, vì vậy trạng thái nên biểu diễn “dãy kết thúc tại chỉ số $i$ với hiệu $j$”.
>
> $f[i][j]$ lưu độ dài đó. Cộng $500$ vào $j$ sẽ đưa các hiệu về đoạn $[0,1000]$. Chuyển trạng thái bằng cách xét $k<i$ và cập nhật $f[i][j]=\max(f[i][j],f[k][j]+1)$.
>
> Mỗi cặp chỉ số được xét một lần; đáp án là giá trị lớn nhất trong bảng.

<!-- thinking:end -->

Định nghĩa $f[i][j]$ là độ dài lớn nhất của dãy số học kết thúc tại $nums[i]$ và có công sai $j$. Ban đầu, $f[i][j]=1$, vì một phần tử tự nó tạo thành dãy số học có độ dài $1$.

> Vì công sai có thể âm và độ lệch lớn nhất là $500$, ta cộng đồng loạt $500$ vào công sai để đưa miền giá trị về $[0, 1000]$.

Với mỗi $f[i]$, xét các phần tử đứng trước $nums[i]$, gọi phần tử đó là $nums[k]$. Khi đó công sai đã dịch là $j=nums[i]-nums[k]+500$, và cập nhật $f[i][j]=\max(f[i][j], f[k][j]+1)$. Sau đó cập nhật đáp án $ans=\max(ans, f[i][j]).

Cuối cùng, trả về đáp án.

> Nếu khởi tạo $f[i][j]=0$, cần cộng thêm $1$ vào đáp án trước khi trả về.

Độ phức tạp thời gian là $O(n \times (d + n))$ và độ phức tạp không gian là $O(n \times d)$, trong đó $n$ là độ dài mảng $nums$, còn $d$ là hiệu giữa giá trị lớn nhất và nhỏ nhất trong mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestArithSeqLength(self, nums: List[int]) -> int:
        n = len(nums)
        f = [[1] * 1001 for _ in range(n)]
        ans = 0
        for i in range(1, n):
            for k in range(i):
                j = nums[i] - nums[k] + 500
                f[i][j] = max(f[i][j], f[k][j] + 1)
                ans = max(ans, f[i][j])
        return ans
```

#### Java

```java
class Solution {
    public int longestArithSeqLength(int[] nums) {
        int n = nums.length;
        int ans = 0;
        int[][] f = new int[n][1001];
        for (int i = 1; i < n; ++i) {
            for (int k = 0; k < i; ++k) {
                int j = nums[i] - nums[k] + 500;
                f[i][j] = Math.max(f[i][j], f[k][j] + 1);
                ans = Math.max(ans, f[i][j]);
            }
        }
        return ans + 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int longestArithSeqLength(vector<int>& nums) {
        int n = nums.size();
        int f[n][1001];
        memset(f, 0, sizeof(f));
        int ans = 0;
        for (int i = 1; i < n; ++i) {
            for (int k = 0; k < i; ++k) {
                int j = nums[i] - nums[k] + 500;
                f[i][j] = max(f[i][j], f[k][j] + 1);
                ans = max(ans, f[i][j]);
            }
        }
        return ans + 1;
    }
};
```

#### Go

```go
func longestArithSeqLength(nums []int) int {
	n := len(nums)
	f := make([][]int, n)
	for i := range f {
		f[i] = make([]int, 1001)
	}
	ans := 0
	for i := 1; i < n; i++ {
		for k := 0; k < i; k++ {
			j := nums[i] - nums[k] + 500
			f[i][j] = max(f[i][j], f[k][j]+1)
			ans = max(ans, f[i][j])
		}
	}
	return ans + 1
}
```

#### TypeScript

```ts
function longestArithSeqLength(nums: number[]): number {
    const n = nums.length;
    let ans = 0;
    const f: number[][] = Array.from({ length: n }, () => new Array(1001).fill(0));
    for (let i = 1; i < n; ++i) {
        for (let k = 0; k < i; ++k) {
            const j = nums[i] - nums[k] + 500;
            f[i][j] = Math.max(f[i][j], f[k][j] + 1);
            ans = Math.max(ans, f[i][j]);
        }
    }
    return ans + 1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
