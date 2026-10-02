---
comments: true
difficulty: Hard
tags:
    - Memoization
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [546. Remove Boxes](https://leetcode.com/problems/remove-boxes)

[中文文档](/solution/0500-0599/0546.Remove%20Boxes/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số <code>boxes</code> có màu khác nhau, mỗi màu được biểu diễn bằng một số nguyên dương riêng.</p>

<p>Bạn có thể xóa các hộp qua nhiều lượt cho đến khi không còn hộp nào. Mỗi lượt, bạn chọn một nhóm hộp liên tiếp cùng màu (gồm <code>k</code> hộp, với <code>k &gt;= 1</code>), xóa chúng và nhận được <code>k * k</code> điểm.</p>

<p>Hãy trả về <em>số điểm tối đa có thể đạt được</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> boxes = [1,3,2,2,2,3,4,3,1]
<strong>Đầu ra:</strong> 23
<strong>Giải thích:</strong>
[1, 3, 2, 2, 2, 3, 4, 3, 1] 
----&gt; [1, 3, 3, 4, 3, 1] (3*3=9 điểm) 
----&gt; [1, 3, 3, 3, 1] (1*1=1 điểm) 
----&gt; [1, 1] (3*3=9 điểm) 
----&gt; [] (2*2=4 điểm)
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> boxes = [1,1,1]
<strong>Đầu ra:</strong> 9
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> boxes = [1]
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= boxes.length &lt;= 100</code></li>
	<li><code>1 &lt;= boxes[i]&nbsp;&lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Xóa một nhóm hộp được số điểm bằng bình phương độ dài nhóm, và thứ tự xóa ảnh hưởng đến việc gộp các nhóm sau đó. Interval DP thông thường không thể biểu diễn thao tác “xóa phần giữa rồi gộp nhóm bên phải”.
>
> $dfs(i,j,k)$ biểu diễn đoạn $[i,j]$ cùng với $k$ hộp bổ sung đã có màu bằng $boxes[j]$. Gộp nhóm kết thúc tại $j$ vào $k$, rồi hoặc xóa $boxes[j]$ ngay, hoặc tìm vị trí trước đó $h$ có cùng màu, xóa đoạn $(h,j)$ rồi gộp hai nhóm. Memoization lưu kết quả của từng bộ ba tham số.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def removeBoxes(self, boxes: List[int]) -> int:
        @cache
        def dfs(i, j, k):
            if i > j:
                return 0
            while i < j and boxes[j] == boxes[j - 1]:
                j, k = j - 1, k + 1
            ans = dfs(i, j - 1, 0) + (k + 1) * (k + 1)
            for h in range(i, j):
                if boxes[h] == boxes[j]:
                    ans = max(ans, dfs(h + 1, j - 1, 0) + dfs(i, h, k + 1))
            return ans

        n = len(boxes)
        ans = dfs(0, n - 1, 0)
        dfs.cache_clear()
        return ans
```

#### Java

```java
class Solution {
    private int[][][] f;
    private int[] b;

    public int removeBoxes(int[] boxes) {
        b = boxes;
        int n = b.length;
        f = new int[n][n][n];
        return dfs(0, n - 1, 0);
    }

    private int dfs(int i, int j, int k) {
        if (i > j) {
            return 0;
        }
        while (i < j && b[j] == b[j - 1]) {
            --j;
            ++k;
        }
        if (f[i][j][k] > 0) {
            return f[i][j][k];
        }
        int ans = dfs(i, j - 1, 0) + (k + 1) * (k + 1);
        for (int h = i; h < j; ++h) {
            if (b[h] == b[j]) {
                ans = Math.max(ans, dfs(h + 1, j - 1, 0) + dfs(i, h, k + 1));
            }
        }
        f[i][j][k] = ans;
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int removeBoxes(vector<int>& boxes) {
        int n = boxes.size();
        vector<vector<vector<int>>> f(n, vector<vector<int>>(n, vector<int>(n)));
        function<int(int, int, int)> dfs;
        dfs = [&](int i, int j, int k) {
            if (i > j) return 0;
            while (i < j && boxes[j] == boxes[j - 1]) {
                --j;
                ++k;
            }
            if (f[i][j][k]) return f[i][j][k];
            int ans = dfs(i, j - 1, 0) + (k + 1) * (k + 1);
            for (int h = i; h < j; ++h) {
                if (boxes[h] == boxes[j]) {
                    ans = max(ans, dfs(h + 1, j - 1, 0) + dfs(i, h, k + 1));
                }
            }
            f[i][j][k] = ans;
            return ans;
        };
        return dfs(0, n - 1, 0);
    }
};
```

#### Go

```go
func removeBoxes(boxes []int) int {
	n := len(boxes)
	f := make([][][]int, n)
	for i := range f {
		f[i] = make([][]int, n)
		for j := range f[i] {
			f[i][j] = make([]int, n)
		}
	}
	var dfs func(i, j, k int) int
	dfs = func(i, j, k int) int {
		if i > j {
			return 0
		}
		for i < j && boxes[j] == boxes[j-1] {
			j, k = j-1, k+1
		}
		if f[i][j][k] > 0 {
			return f[i][j][k]
		}
		ans := dfs(i, j-1, 0) + (k+1)*(k+1)
		for h := i; h < j; h++ {
			if boxes[h] == boxes[j] {
				ans = max(ans, dfs(h+1, j-1, 0)+dfs(i, h, k+1))
			}
		}
		f[i][j][k] = ans
		return ans
	}
	return dfs(0, n-1, 0)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
