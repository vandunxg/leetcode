---
comments: true
difficulty: Easy
tags:
    - Array
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [506. Relative Ranks](https://leetcode.com/problems/relative-ranks)

[中文文档](/solution/0500-0599/0506.Relative%20Ranks/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>score</code> có độ dài <code>n</code>, trong đó <code>score[i]</code> là điểm của vận động viên thứ <code>i</code> trong một cuộc thi. Đảm bảo mọi điểm số đều <strong>khác nhau</strong>.</p>

<p>Các vận động viên được <strong>xếp hạng</strong> theo điểm số: người ở vị trí thứ nhất có điểm cao nhất, người ở vị trí thứ hai có điểm cao thứ hai, v.v. Vị trí của mỗi vận động viên quyết định thứ hạng:</p>

<ul>
	<li>Vận động viên ở vị trí thứ nhất có thứ hạng <code>&quot;Gold Medal&quot;</code>.</li>
	<li>Vận động viên ở vị trí thứ hai có thứ hạng <code>&quot;Silver Medal&quot;</code>.</li>
	<li>Vận động viên ở vị trí thứ ba có thứ hạng <code>&quot;Bronze Medal&quot;</code>.</li>
	<li>Với các vận động viên ở vị trí từ thứ tư đến thứ <code>n</code>, thứ hạng chính là số thứ tự vị trí (tức vận động viên ở vị trí thứ <code>x</code> có thứ hạng <code>&quot;x&quot;</code>).</li>
</ul>

<p>Trả về mảng <code>answer</code> có độ dài <code>n</code>, trong đó <code>answer[i]</code> là <strong>thứ hạng</strong> của vận động viên thứ <code>i</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> score = [5,4,3,2,1]
<strong>Đầu ra:</strong> [&quot;Gold Medal&quot;,&quot;Silver Medal&quot;,&quot;Bronze Medal&quot;,&quot;4&quot;,&quot;5&quot;]
<strong>Giải thích:</strong> Thứ hạng theo vị trí là [thứ nhất, thứ hai, thứ ba, thứ tư, thứ năm].</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> score = [10,3,8,9,4]
<strong>Đầu ra:</strong> [&quot;Gold Medal&quot;,&quot;5&quot;,&quot;Bronze Medal&quot;,&quot;Silver Medal&quot;,&quot;4&quot;]
<strong>Giải thích:</strong> Thứ hạng theo vị trí là [thứ nhất, thứ năm, thứ ba, thứ hai, thứ tư].

</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == score.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= score[i] &lt;= 10<sup>6</sup></code></li>
	<li>Mọi giá trị trong <code>score</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Thứ hạng được xác định theo điểm từ cao xuống thấp; ba vị trí đầu nhận huy chương. Nếu chỉ sắp xếp điểm số thì sẽ mất chỉ số ban đầu của vận động viên.
>
> Sắp xếp các chỉ số theo điểm giảm dần, sau đó gán huy chương cho ba vị trí đầu và thứ hạng dạng số cho các vị trí còn lại. Chỉ cần một lần sắp xếp là xác định được cả thứ hạng lẫn vị trí ban đầu.

<!-- thinking:end -->

Ta dùng mảng $\textit{idx}$ để lưu các chỉ số từ $0$ đến $n-1$, rồi sắp xếp $\textit{idx}$ theo giá trị trong $\textit{score}$ theo thứ tự giảm dần.

Tiếp theo, ta định nghĩa mảng $\textit{top3} = [\text{Gold Medal}, \text{Silver Medal}, \text{Bronze Medal}]$. Ta duyệt $\textit{idx}$; với mỗi chỉ số $j$, nếu $j$ nhỏ hơn $3$ thì $\textit{ans}[j]$ bằng $\textit{top3}[j]$, ngược lại bằng $j+1$.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{score}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findRelativeRanks(self, score: List[int]) -> List[str]:
        n = len(score)
        idx = list(range(n))
        idx.sort(key=lambda x: -score[x])
        top3 = ["Gold Medal", "Silver Medal", "Bronze Medal"]
        ans = [None] * n
        for i, j in enumerate(idx):
            ans[j] = top3[i] if i < 3 else str(i + 1)
        return ans
```

#### Java

```java
class Solution {
    public String[] findRelativeRanks(int[] score) {
        int n = score.length;
        Integer[] idx = new Integer[n];
        Arrays.setAll(idx, i -> i);
        Arrays.sort(idx, (i1, i2) -> score[i2] - score[i1]);
        String[] ans = new String[n];
        String[] top3 = new String[] {"Gold Medal", "Silver Medal", "Bronze Medal"};
        for (int i = 0; i < n; ++i) {
            ans[idx[i]] = i < 3 ? top3[i] : String.valueOf(i + 1);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> findRelativeRanks(vector<int>& score) {
        int n = score.size();
        vector<int> idx(n);
        iota(idx.begin(), idx.end(), 0);
        sort(idx.begin(), idx.end(), [&score](int a, int b) {
            return score[a] > score[b];
        });
        vector<string> ans(n);
        vector<string> top3 = {"Gold Medal", "Silver Medal", "Bronze Medal"};
        for (int i = 0; i < n; ++i) {
            ans[idx[i]] = i < 3 ? top3[i] : to_string(i + 1);
        }
        return ans;
    }
};
```

#### Go

```go
func findRelativeRanks(score []int) []string {
	n := len(score)
	idx := make([][]int, n)
	for i := 0; i < n; i++ {
		idx[i] = []int{score[i], i}
	}
	sort.Slice(idx, func(i1, i2 int) bool {
		return idx[i1][0] > idx[i2][0]
	})
	ans := make([]string, n)
	top3 := []string{"Gold Medal", "Silver Medal", "Bronze Medal"}
	for i := 0; i < n; i++ {
		if i < 3 {
			ans[idx[i][1]] = top3[i]
		} else {
			ans[idx[i][1]] = strconv.Itoa(i + 1)
		}
	}
	return ans
}
```

#### TypeScript

```ts
function findRelativeRanks(score: number[]): string[] {
    const n = score.length;
    const idx = Array.from({ length: n }, (_, i) => i);
    idx.sort((a, b) => score[b] - score[a]);
    const top3 = ['Gold Medal', 'Silver Medal', 'Bronze Medal'];
    const ans: string[] = Array(n);
    for (let i = 0; i < n; i++) {
        if (i < 3) {
            ans[idx[i]] = top3[i];
        } else {
            ans[idx[i]] = (i + 1).toString();
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
