---
comments: true
difficulty: Medium
tags:
    - Array
    - Sorting
---

<!-- problem:start -->

# [2655. Find Maximal Uncovered Ranges 🔒](https://leetcode.com/problems/find-maximal-uncovered-ranges)

[中文文档](/solution/2600-2699/2655.Find%20Maximal%20Uncovered%20Ranges/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code> là độ dài của mảng <code>nums</code> được <strong>đánh chỉ số từ 0</strong>, và một mảng 2 chiều <code>ranges</code> được <strong>đánh chỉ số từ 0</strong>, là danh sách các đoạn con của <code>nums</code>&nbsp;(các đoạn con có thể <strong>chồng lấn</strong>).</p>

<p>Mỗi hàng <code>ranges[i]</code> có đúng 2 ô:</p>

<ul>
	<li><code>ranges[i][0]</code>, cho biết điểm bắt đầu của đoạn thứ i<sup>th</sup> (bao gồm)</li>
	<li><code>ranges[i][1]</code>, cho biết điểm kết thúc của đoạn thứ i<sup>th</sup> (bao gồm)</li>
</ul>

<p>Các đoạn này phủ một số ô của <code>nums</code>&nbsp;và để lại một số ô chưa được phủ. Nhiệm vụ của bạn là tìm tất cả các đoạn <b>chưa được phủ</b> có độ dài <strong>cực đại</strong>.</p>

<p>Trả về <em>một mảng 2 chiều </em><code>answer</code><em> chứa các đoạn chưa được phủ, được <strong>sắp xếp</strong> theo điểm bắt đầu với <strong>thứ tự tăng dần</strong>.</em></p>

<p>Ta nói tất cả các đoạn <strong>chưa được phủ</strong> có <strong>độ dài cực đại</strong> khi thỏa mãn hai điều kiện:</p>

<ul>
	<li>Mỗi ô chưa được phủ phải thuộc về <strong>đúng</strong> một đoạn con</li>
	<li><strong>Không tồn tại</strong>&nbsp;hai đoạn (l<sub>1</sub>, r<sub>1</sub>) và (l<sub>2</sub>, r<sub>2</sub>) sao cho r<sub>1 </sub>+ 1 = l<sub>2</sub></li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 10, ranges = [[3,5],[7,8]]
<strong>Đầu ra:</strong> [[0,2],[6,6],[9,9]]
<strong>Giải thích:</strong> Các đoạn (3, 5) và (7, 8) được phủ, vì vậy nếu đơn giản hóa mảng nums thành một mảng nhị phân trong đó 0 biểu thị một ô chưa được phủ và 1 biểu thị một ô đã được phủ, mảng sẽ trở thành [0,0,0,1,1,1,0,1,1,0], trong đó ta có thể thấy các đoạn (0, 2), (6, 6) và (9, 9) chưa được phủ.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3, ranges = [[0,2]]
<strong>Đầu ra:</strong> []
<strong>Giải thích: </strong>Trong ví dụ này, toàn bộ mảng nums đều được phủ và không có ô nào chưa được phủ, nên đầu ra là một mảng rỗng.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 7, ranges = [[2,4],[0,3]]
<strong>Đầu ra:</strong> [[5,6]]
<strong>Giải thích:</strong> Các đoạn (0, 3) và (2, 4) được phủ, vì vậy nếu đơn giản hóa mảng nums thành một mảng nhị phân trong đó 0 biểu thị một ô chưa được phủ và 1 biểu thị một ô đã được phủ, mảng sẽ trở thành [1,1,1,1,1,0,0], trong đó ta có thể thấy đoạn (5, 6) chưa được phủ.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;=&nbsp;10<sup>9</sup></code></li>
	<li><code>0 &lt;= ranges.length &lt;= 10<sup>6</sup></code></li>
	<li><code>ranges[i].length = 2</code></li>
	<li><code>0 &lt;= ranges[i][j] &lt;= n - 1</code></li>
	<li><code>ranges[i][0] &lt;=&nbsp;ranges[i][1]</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm các khoảng trống liên tiếp cực đại của $[0,n-1]$ nằm ngoài các đoạn đã cho. Các đoạn chưa được sắp xếp rất khó gộp; kiểm tra từng cặp có độ phức tạp $O(m^2)$ và không phù hợp với $m \le 10^5$.
>
> Sắp xếp theo điểm đầu rồi duyệt với điểm cuối bên phải đã được phủ $last$: xuất ra khoảng trống trước đoạn tiếp theo, sau đó mở rộng $last$. Cuối cùng, thêm khoảng trống đến $n-1$ nếu cần.

<!-- thinking:end -->

Ta sắp xếp tất cả các đoạn theo điểm đầu của chúng với thứ tự tăng dần, sau đó duyệt qua các đoạn từ trái sang phải, duy trì một biến $\textit{last}$ biểu thị điểm cuối bên phải xa nhất đã được phủ, ban đầu $\textit{last}=-1$.

Nếu điểm đầu của đoạn hiện tại lớn hơn $\textit{last}+1$, điều đó có nghĩa là $[\textit{last}+1, l-1]$ là một đoạn chưa được phủ, nên ta thêm đoạn này vào mảng kết quả. Sau đó, ta cập nhật $\textit{last}$ thành điểm cuối của đoạn hiện tại và tiếp tục duyệt đoạn tiếp theo. Sau khi duyệt qua tất cả các đoạn, nếu $\textit{last}+1 < n$, điều đó có nghĩa là $[\textit{last}+1, n-1]$ là một đoạn chưa được phủ, nên ta thêm đoạn này vào mảng kết quả.

Cuối cùng, ta trả về mảng kết quả.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(\log n)$, trong đó $n$ là độ dài của mảng $\textit{ranges}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findMaximalUncoveredRanges(
        self, n: int, ranges: List[List[int]]
    ) -> List[List[int]]:
        ranges.sort()
        last = -1
        ans = []
        for l, r in ranges:
            if last + 1 < l:
                ans.append([last + 1, l - 1])
            last = max(last, r)
        if last + 1 < n:
            ans.append([last + 1, n - 1])
        return ans
```

#### Java

```java
class Solution {
    public int[][] findMaximalUncoveredRanges(int n, int[][] ranges) {
        Arrays.sort(ranges, (a, b) -> a[0] - b[0]);
        int last = -1;
        List<int[]> ans = new ArrayList<>();
        for (int[] range : ranges) {
            int l = range[0], r = range[1];
            if (last + 1 < l) {
                ans.add(new int[] {last + 1, l - 1});
            }
            last = Math.max(last, r);
        }
        if (last + 1 < n) {
            ans.add(new int[] {last + 1, n - 1});
        }
        return ans.toArray(new int[0][]);
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> findMaximalUncoveredRanges(int n, vector<vector<int>>& ranges) {
        sort(ranges.begin(), ranges.end(), [](const vector<int>& a, const vector<int>& b) {
            return a[0] < b[0];
        });
        int last = -1;
        vector<vector<int>> ans;
        for (auto& range : ranges) {
            int l = range[0], r = range[1];
            if (last + 1 < l) {
                ans.push_back({last + 1, l - 1});
            }
            last = max(last, r);
        }
        if (last + 1 < n) {
            ans.push_back({last + 1, n - 1});
        }
        return ans;
    }
};
```

#### Go

```go
func findMaximalUncoveredRanges(n int, ranges [][]int) (ans [][]int) {
	sort.Slice(ranges, func(i, j int) bool { return ranges[i][0] < ranges[j][0] })
	last := -1
	for _, r := range ranges {
		if last+1 < r[0] {
			ans = append(ans, []int{last + 1, r[0] - 1})
		}
		last = max(last, r[1])
	}
	if last+1 < n {
		ans = append(ans, []int{last + 1, n - 1})
	}
	return
}
```

#### TypeScript

```ts
function findMaximalUncoveredRanges(n: number, ranges: number[][]): number[][] {
    ranges.sort((a, b) => a[0] - b[0]);
    let last = -1;
    const ans: number[][] = [];
    for (const [l, r] of ranges) {
        if (last + 1 < l) {
            ans.push([last + 1, l - 1]);
        }
        last = Math.max(last, r);
    }
    if (last + 1 < n) {
        ans.push([last + 1, n - 1]);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn find_maximal_uncovered_ranges(n: i32, mut ranges: Vec<Vec<i32>>) -> Vec<Vec<i32>> {
        ranges.sort_by_key(|x| x[0]);
        let mut last = -1;
        let mut ans = Vec::new();
        for range in ranges {
            let l = range[0];
            let r = range[1];
            if last + 1 < l {
                ans.push(vec![last + 1, l - 1]);
            }
            last = last.max(r);
        }
        if last + 1 < n {
            ans.push(vec![last + 1, n - 1]);
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
