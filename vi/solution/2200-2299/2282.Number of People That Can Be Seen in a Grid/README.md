---
comments: true
difficulty: Medium
tags:
    - Stack
    - Array
    - Matrix
    - Monotonic Stack
---

<!-- problem:start -->

# [2282. Number of People That Can Be Seen in a Grid 🔒](https://leetcode.com/problems/number-of-people-that-can-be-seen-in-a-grid)

[中文文档](/solution/2200-2299/2282.Number%20of%20People%20That%20Can%20Be%20Seen%20in%20a%20Grid/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng 2D số nguyên dương <code>m x n</code> <strong>đánh chỉ số từ 0</strong> là <code>heights</code>, trong đó <code>heights[i][j]</code> là chiều cao của người đứng tại vị trí <code>(i, j)</code>.</p>

<p>Một người đứng tại vị trí <code>(row<sub>1</sub>, col<sub>1</sub>)</code> có thể nhìn thấy người đứng tại vị trí <code>(row<sub>2</sub>, col<sub>2</sub>)</code> nếu:</p>

<ul>
	<li>Người ở <code>(row<sub>2</sub>, col<sub>2</sub>)</code> ở bên phải <strong>hoặc</strong> phía dưới người ở <code>(row<sub>1</sub>, col<sub>1</sub>)</code>. Cụ thể, điều này có nghĩa là <code>row<sub>1</sub> == row<sub>2</sub></code> và <code>col<sub>1</sub> &lt; col<sub>2</sub></code> <strong>hoặc</strong> <code>row<sub>1</sub> &lt; row<sub>2</sub></code> và <code>col<sub>1</sub> == col<sub>2</sub></code>.</li>
	<li>Mọi người đứng giữa họ đều thấp hơn <strong>cả hai</strong> người.</li>
</ul>

<p>Hãy trả về<em> một mảng 2D số nguyên </em><code>m x n</code><em> là </em><code>answer</code><em>, trong đó </em><code>answer[i][j]</code><em> là số người mà người đứng tại vị trí </em><code>(i, j)</code><em> có thể nhìn thấy.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2282.Number%20of%20People%20That%20Can%20Be%20Seen%20in%20a%20Grid/images/image-20220524180458-1.png" style="width: 700px; height: 164px;" />
<pre>
<strong>Đầu vào:</strong> heights = [[3,1,4,2,5]]
<strong>Đầu ra:</strong> [[2,1,2,1,0]]
<strong>Giải thích:</strong>
- Người ở (0, 0) có thể nhìn thấy những người ở (0, 1) và (0, 2).
  Lưu ý rằng người này không thể nhìn thấy người ở (0, 4) vì người ở (0, 2) cao hơn người này.
- Người ở (0, 1) có thể nhìn thấy người ở (0, 2).
- Người ở (0, 2) có thể nhìn thấy những người ở (0, 3) và (0, 4).
- Người ở (0, 3) có thể nhìn thấy người ở (0, 4).
- Người ở (0, 4) không thể nhìn thấy ai.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2282.Number%20of%20People%20That%20Can%20Be%20Seen%20in%20a%20Grid/images/image-20220523113533-2.png" style="width: 400px; height: 249px;" />
<pre>
<strong>Đầu vào:</strong> heights = [[5,1],[3,1],[4,1]]
<strong>Đầu ra:</strong> [[3,1],[2,1],[1,0]]
<strong>Giải thích:</strong>
- Người ở (0, 0) có thể nhìn thấy những người ở (0, 1), (1, 0) và (2, 0).
- Người ở (0, 1) có thể nhìn thấy người ở (1, 1).
- Người ở (1, 0) có thể nhìn thấy những người ở (1, 1) và (2, 0).
- Người ở (1, 1) có thể nhìn thấy người ở (2, 1).
- Người ở (2, 0) có thể nhìn thấy người ở (2, 1).
- Người ở (2, 1) không thể nhìn thấy ai.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= heights.length &lt;= 400</code></li>
	<li><code>1 &lt;= heights[i].length &lt;= 400</code></li>
	<li><code>1 &lt;= heights[i][j] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Stack đơn điệu

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi người nhìn sang phải và xuống dưới; một người cao hơn hẳn sẽ che khuất những người còn lại, còn một người có chiều cao bằng nhau chỉ được nhìn thấy nếu là người gần nhất. Các hàng và cột độc lập, giống như bài toán đếm số người có thể nhìn thấy trong một hàng người. Một stack giảm dần, duyệt từ phải sang (hoặc từ dưới lên), có thể xử lý một hàng hoặc cột.
>
> Ta loại các người thấp hơn và đếm họ; nếu stack vẫn còn phần tử thì cộng thêm một người, sau đó loại các người bằng chiều cao. Chạy hàm hỗ trợ trên mọi hàng và mọi cột rồi cộng kết quả.

<!-- thinking:end -->

Ta nhận thấy rằng với người thứ $i$, những người mà họ có thể nhìn thấy phải có chiều cao tăng nghiêm ngặt từ trái sang phải (hoặc từ trên xuống dưới).

Do đó, với mỗi hàng, ta có thể dùng một stack đơn điệu để tìm số người mà mỗi người có thể nhìn thấy.

Cụ thể, ta duyệt mảng theo thứ tự ngược lại, sử dụng một stack $stk$ có thứ tự tăng từ đỉnh xuống đáy để lưu chiều cao của những người đã duyệt qua.

Với người thứ $i$, nếu stack không rỗng và phần tử trên đỉnh nhỏ hơn $heights[i]$, ta tăng số người mà người thứ $i$ có thể nhìn thấy lên 1, sau đó loại phần tử trên đỉnh stack; lặp lại thao tác này cho đến khi stack rỗng hoặc phần tử trên đỉnh stack lớn hơn hoặc bằng $heights[i]$. Nếu stack vẫn còn phần tử, điều đó có nghĩa là phần tử trên đỉnh stack lớn hơn hoặc bằng $heights[i]$, vì vậy ta tăng số người mà người thứ $i$ có thể nhìn thấy lên 1. Tiếp theo, nếu stack không rỗng và phần tử trên đỉnh stack bằng $heights[i]$, ta loại phần tử trên đỉnh stack. Cuối cùng, ta đưa $heights[i]$ vào stack và chuyển sang người tiếp theo.

Sau khi xử lý như trên, ta nhận được số người mà mỗi người có thể nhìn thấy trong từng hàng.

Tương tự, ta có thể xử lý từng cột để nhận được số người mà mỗi người có thể nhìn thấy trong từng cột. Cuối cùng, ta cộng đáp án theo hàng và theo cột để thu được đáp án cuối cùng.

Độ phức tạp thời gian là $O(m \times n)$ và độ phức tạp không gian là $O(\max(m, n))$. Trong đó, $m$ và $n$ lần lượt là số hàng và số cột của mảng $heights$.

Bài toán tương tự:

- [1944. Number of Visible People in a Queue](https://github.com/doocs/leetcode/blob/main/solution/1900-1999/1944.Number%20of%20Visible%20People%20in%20a%20Queue/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def seePeople(self, heights: List[List[int]]) -> List[List[int]]:
        def f(nums: List[int]) -> List[int]:
            n = len(nums)
            stk = []
            ans = [0] * n
            for i in range(n - 1, -1, -1):
                while stk and stk[-1] < nums[i]:
                    ans[i] += 1
                    stk.pop()
                if stk:
                    ans[i] += 1
                while stk and stk[-1] == nums[i]:
                    stk.pop()
                stk.append(nums[i])
            return ans

        ans = [f(row) for row in heights]
        m, n = len(heights), len(heights[0])
        for j in range(n):
            add = f([heights[i][j] for i in range(m)])
            for i in range(m):
                ans[i][j] += add[i]
        return ans
```

#### Java

```java
class Solution {
    public int[][] seePeople(int[][] heights) {
        int m = heights.length, n = heights[0].length;
        int[][] ans = new int[m][0];
        for (int i = 0; i < m; ++i) {
            ans[i] = f(heights[i]);
        }
        for (int j = 0; j < n; ++j) {
            int[] nums = new int[m];
            for (int i = 0; i < m; ++i) {
                nums[i] = heights[i][j];
            }
            int[] add = f(nums);
            for (int i = 0; i < m; ++i) {
                ans[i][j] += add[i];
            }
        }
        return ans;
    }

    private int[] f(int[] nums) {
        int n = nums.length;
        int[] ans = new int[n];
        Deque<Integer> stk = new ArrayDeque<>();
        for (int i = n - 1; i >= 0; --i) {
            while (!stk.isEmpty() && stk.peek() < nums[i]) {
                stk.pop();
                ++ans[i];
            }
            if (!stk.isEmpty()) {
                ++ans[i];
            }
            while (!stk.isEmpty() && stk.peek() == nums[i]) {
                stk.pop();
            }
            stk.push(nums[i]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> seePeople(vector<vector<int>>& heights) {
        int m = heights.size(), n = heights[0].size();
        auto f = [](vector<int>& nums) {
            int n = nums.size();
            vector<int> ans(n);
            stack<int> stk;
            for (int i = n - 1; ~i; --i) {
                while (stk.size() && stk.top() < nums[i]) {
                    ++ans[i];
                    stk.pop();
                }
                if (stk.size()) {
                    ++ans[i];
                }
                while (stk.size() && stk.top() == nums[i]) {
                    stk.pop();
                }
                stk.push(nums[i]);
            }
            return ans;
        };
        vector<vector<int>> ans;
        for (auto& row : heights) {
            ans.push_back(f(row));
        }
        for (int j = 0; j < n; ++j) {
            vector<int> col;
            for (int i = 0; i < m; ++i) {
                col.push_back(heights[i][j]);
            }
            vector<int> add = f(col);
            for (int i = 0; i < m; ++i) {
                ans[i][j] += add[i];
            }
        }
        return ans;
    }
};
```

#### Go

```go
func seePeople(heights [][]int) (ans [][]int) {
	f := func(nums []int) []int {
		n := len(nums)
		ans := make([]int, n)
		stk := []int{}
		for i := n - 1; i >= 0; i-- {
			for len(stk) > 0 && stk[len(stk)-1] < nums[i] {
				ans[i]++
				stk = stk[:len(stk)-1]
			}
			if len(stk) > 0 {
				ans[i]++
			}
			for len(stk) > 0 && stk[len(stk)-1] == nums[i] {
				stk = stk[:len(stk)-1]
			}
			stk = append(stk, nums[i])
		}
		return ans
	}
	for _, row := range heights {
		ans = append(ans, f(row))
	}
	n := len(heights[0])
	for j := 0; j < n; j++ {
		col := make([]int, len(heights))
		for i := range heights {
			col[i] = heights[i][j]
		}
		for i, v := range f(col) {
			ans[i][j] += v
		}
	}
	return
}
```

#### TypeScript

```ts
function seePeople(heights: number[][]): number[][] {
    const f = (nums: number[]): number[] => {
        const n = nums.length;
        const ans: number[] = new Array(n).fill(0);
        const stk: number[] = [];
        for (let i = n - 1; ~i; --i) {
            while (stk.length && stk.at(-1) < nums[i]) {
                stk.pop();
                ++ans[i];
            }
            if (stk.length) {
                ++ans[i];
            }
            while (stk.length && stk.at(-1) === nums[i]) {
                stk.pop();
            }
            stk.push(nums[i]);
        }
        return ans;
    };
    const ans: number[][] = [];
    for (const row of heights) {
        ans.push(f(row));
    }
    const n = heights[0].length;
    for (let j = 0; j < n; ++j) {
        const col: number[] = [];
        for (const row of heights) {
            col.push(row[j]);
        }
        const add = f(col);
        for (let i = 0; i < ans.length; ++i) {
            ans[i][j] += add[i];
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
