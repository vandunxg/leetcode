---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - Counting
---

<!-- problem:start -->

# [1940. Longest Common Subsequence Between Sorted Arrays 🔒](https://leetcode.com/problems/longest-common-subsequence-between-sorted-arrays)

[中文文档](/solution/1900-1999/1940.Longest%20Common%20Subsequence%20Between%20Sorted%20Arrays/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng gồm các mảng số nguyên <code>arrays</code>, trong đó mỗi <code>arrays[i]</code> được sắp xếp theo thứ tự <strong>tăng nghiêm ngặt</strong>, hãy trả về <em>một mảng số nguyên biểu diễn <strong>dãy con chung dài nhất</strong> của&nbsp;<strong>tất cả</strong> các mảng</em>.</p>

<p><strong>Dãy con</strong> là một dãy có thể được tạo ra từ một dãy khác bằng cách xóa một số phần tử (có thể không xóa phần tử nào) mà không thay đổi thứ tự của các phần tử còn lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arrays = [[<u>1</u>,3,<u>4</u>],
                 [<u>1</u>,<u>4</u>,7,9]]
<strong>Đầu ra:</strong> [1,4]
<strong>Giải thích:</strong> Dãy con chung dài nhất trong hai mảng là [1,4].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arrays = [[<u>2</u>,<u>3</u>,<u>6</u>,8],
                 [1,<u>2</u>,<u>3</u>,5,<u>6</u>,7,10],
                 [<u>2</u>,<u>3</u>,4,<u>6</u>,9]]
<strong>Đầu ra:</strong> [2,3,6]
<strong>Giải thích:</strong> Dãy con chung dài nhất trong cả ba mảng là [2,3,6].
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> arrays = [[1,2,3,4,5],
                 [6,7,8]]
<strong>Đầu ra:</strong> []
<strong>Giải thích:</strong> Không có dãy con chung nào giữa hai mảng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= arrays.length &lt;= 100</code></li>
	<li><code>1 &lt;= arrays[i].length &lt;= 100</code></li>
	<li><code>1 &lt;= arrays[i][j] &lt;= 100</code></li>
	<li><code>arrays[i]</code> được sắp xếp theo thứ tự <strong>tăng nghiêm ngặt</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Việc trộn bằng nhiều con trỏ trở nên rườm rà khi có nhiều mảng. Các giá trị nằm trong $[1,100]$ và mỗi hàng tăng nghiêm ngặt, nên một giá trị xuất hiện nhiều nhất một lần trong mỗi hàng.
>
> Ta đếm số hàng chứa mỗi giá trị; những giá trị có số lần đếm bằng số mảng sẽ tạo thành dãy con chung dài nhất, vốn đã ở thứ tự tăng dần.

<!-- thinking:end -->

Ta nhận thấy miền giá trị của các phần tử là $[1, 100]$, nên có thể dùng một mảng $\textit{cnt}$ có độ dài $101$ để ghi lại số lần xuất hiện của mỗi phần tử.

Vì mỗi mảng trong $\textit{arrays}$ đều tăng nghiêm ngặt, các phần tử của dãy con chung phải tăng đơn điệu, và số lần xuất hiện của mỗi phần tử này phải bằng độ dài của $\textit{arrays}$.

Do đó, ta duyệt từng mảng trong $\textit{arrays}$ và đếm số lần xuất hiện của mỗi phần tử. Cuối cùng, ta duyệt từng phần tử của $\textit{cnt}$ từ nhỏ đến lớn. Nếu số lần xuất hiện bằng độ dài của $\textit{arrays}$, thì phần tử này thuộc dãy con chung và ta thêm nó vào mảng kết quả.

Sau khi duyệt xong, trả về mảng kết quả.

Độ phức tạp thời gian là $O(M + N)$, và độ phức tạp không gian là $O(M)$. Trong đó, $M$ là miền giá trị của các phần tử; với bài toán này, $M = 101$, còn $N$ là tổng số phần tử trong các mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestCommonSubsequence(self, arrays: List[List[int]]) -> List[int]:
        cnt = [0] * 101
        for row in arrays:
            for x in row:
                cnt[x] += 1
        return [x for x, v in enumerate(cnt) if v == len(arrays)]
```

#### Java

```java
class Solution {
    public List<Integer> longestCommonSubsequence(int[][] arrays) {
        int[] cnt = new int[101];
        for (var row : arrays) {
            for (int x : row) {
                ++cnt[x];
            }
        }
        List<Integer> ans = new ArrayList<>();
        for (int i = 0; i < 101; ++i) {
            if (cnt[i] == arrays.length) {
                ans.add(i);
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
    vector<int> longestCommonSubsequence(vector<vector<int>>& arrays) {
        int cnt[101]{};
        for (const auto& row : arrays) {
            for (int x : row) {
                ++cnt[x];
            }
        }
        vector<int> ans;
        for (int i = 0; i < 101; ++i) {
            if (cnt[i] == arrays.size()) {
                ans.push_back(i);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func longestCommonSubsequence(arrays [][]int) (ans []int) {
	cnt := [101]int{}
	for _, row := range arrays {
		for _, x := range row {
			cnt[x]++
		}
	}
	for x, v := range cnt {
		if v == len(arrays) {
			ans = append(ans, x)
		}
	}
	return
}
```

#### TypeScript

```ts
function longestCommonSubsequence(arrays: number[][]): number[] {
    const cnt: number[] = Array(101).fill(0);
    for (const row of arrays) {
        for (const x of row) {
            ++cnt[x];
        }
    }
    const ans: number[] = [];
    for (let i = 0; i < 101; ++i) {
        if (cnt[i] === arrays.length) {
            ans.push(i);
        }
    }
    return ans;
}
```

#### JavaScript

```js
/**
 * @param {number[][]} arrays
 * @return {number[]}
 */
var longestCommonSubsequence = function (arrays) {
    const cnt = Array(101).fill(0);
    for (const row of arrays) {
        for (const x of row) {
            ++cnt[x];
        }
    }
    const ans = [];
    for (let i = 0; i < 101; ++i) {
        if (cnt[i] === arrays.length) {
            ans.push(i);
        }
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
