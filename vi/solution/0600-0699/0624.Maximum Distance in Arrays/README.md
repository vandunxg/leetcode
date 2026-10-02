---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
---

<!-- problem:start -->

# [624. Maximum Distance in Arrays](https://leetcode.com/problems/maximum-distance-in-arrays)

[中文文档](/solution/0600-0699/0624.Maximum%20Distance%20in%20Arrays/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>m</code> <code>arrays</code>, trong đó mỗi mảng được sắp xếp theo <strong>thứ tự tăng dần</strong>.</p>

<p>Chọn hai số nguyên từ hai mảng khác nhau (mỗi mảng chọn một số) rồi tính khoảng cách giữa chúng. Khoảng cách giữa hai số nguyên <code>a</code> và <code>b</code> được định nghĩa là hiệu tuyệt đối <code>|a - b|</code>.</p>

<p>Hãy trả về <em>khoảng cách lớn nhất</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arrays = [[1,2,3],[4,5],[1,2,3]]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Một cách để đạt khoảng cách lớn nhất là 4: chọn 1 trong mảng thứ nhất hoặc thứ ba, và chọn 5 trong mảng thứ hai.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arrays = [[1],[1]]
<strong>Đầu ra:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == arrays.length</code></li>
	<li><code>2 &lt;= m &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= arrays[i].length &lt;= 500</code></li>
	<li><code>-10<sup>4</sup> &lt;= arrays[i][j] &lt;= 10<sup>4</sup></code></li>
	<li><code>arrays[i]</code> được sắp xếp theo <strong>thứ tự tăng dần</strong>.</li>
	<li>Tổng số phần tử trong tất cả các mảng không quá <code>10<sup>5</sup></code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duy trì giá trị lớn nhất và nhỏ nhất

<!-- thinking:start -->

> **Tư duy**
>
> Khoảng cách được tính giữa các đầu mút thuộc hai mảng khác nhau. Xét mọi cặp mảng sẽ có độ phức tạp bậc hai khi có $10^4$ mảng.
>
> Đáp án tối ưu luôn là khoảng cách giữa một đầu mút của mảng hiện tại với giá trị nhỏ nhất hoặc lớn nhất toàn cục của các mảng trước đó. Hãy so sánh trước, rồi mới cập nhật $\textit{mi}$ và $\textit{mx}$ bằng hai đầu mút của mảng hiện tại, để hai đầu mút dùng tính khoảng cách không thuộc cùng một mảng.

<!-- thinking:end -->

Ta nhận thấy khoảng cách lớn nhất phải là khoảng cách giữa giá trị lớn nhất của một mảng và giá trị nhỏ nhất của một mảng khác. Vì vậy, ta duy trì hai biến $\textit{mi}$ và $\textit{mx}$, lần lượt biểu diễn giá trị nhỏ nhất và lớn nhất trong các mảng đã duyệt. Ban đầu, $\textit{mi}$ và $\textit{mx}$ lần lượt là phần tử đầu tiên và cuối cùng của mảng đầu tiên.

Tiếp theo, ta duyệt từ mảng thứ hai. Với mỗi mảng, trước tiên tính khoảng cách giữa phần tử đầu tiên của mảng hiện tại và $\textit{mx}$, đồng thời tính khoảng cách giữa phần tử cuối cùng của mảng hiện tại và $\textit{mi}$. Sau đó, cập nhật khoảng cách lớn nhất. Đồng thời, cập nhật $\textit{mi} = \min(\textit{mi}, \textit{arr}[0])$ và $\textit{mx} = \max(\textit{mx}, \textit{arr}[\textit{size} - 1])$.

Sau khi duyệt hết các mảng, ta thu được khoảng cách lớn nhất.

Độ phức tạp thời gian là $O(m)$, trong đó $m$ là số lượng mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxDistance(self, arrays: List[List[int]]) -> int:
        ans = 0
        mi, mx = arrays[0][0], arrays[0][-1]
        for arr in arrays[1:]:
            a, b = abs(arr[0] - mx), abs(arr[-1] - mi)
            ans = max(ans, a, b)
            mi = min(mi, arr[0])
            mx = max(mx, arr[-1])
        return ans
```

#### Java

```java
class Solution {
    public int maxDistance(List<List<Integer>> arrays) {
        int ans = 0;
        int mi = arrays.get(0).get(0);
        int mx = arrays.get(0).get(arrays.get(0).size() - 1);
        for (int i = 1; i < arrays.size(); ++i) {
            var arr = arrays.get(i);
            int a = Math.abs(arr.get(0) - mx);
            int b = Math.abs(arr.get(arr.size() - 1) - mi);
            ans = Math.max(ans, Math.max(a, b));
            mi = Math.min(mi, arr.get(0));
            mx = Math.max(mx, arr.get(arr.size() - 1));
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxDistance(vector<vector<int>>& arrays) {
        int ans = 0;
        int mi = arrays[0][0], mx = arrays[0][arrays[0].size() - 1];
        for (int i = 1; i < arrays.size(); ++i) {
            auto& arr = arrays[i];
            int a = abs(arr[0] - mx), b = abs(arr[arr.size() - 1] - mi);
            ans = max({ans, a, b});
            mi = min(mi, arr[0]);
            mx = max(mx, arr[arr.size() - 1]);
        }
        return ans;
    }
};
```

#### Go

```go
func maxDistance(arrays [][]int) (ans int) {
	mi, mx := arrays[0][0], arrays[0][len(arrays[0])-1]
	for _, arr := range arrays[1:] {
		a, b := abs(arr[0]-mx), abs(arr[len(arr)-1]-mi)
		ans = max(ans, max(a, b))
		mi = min(mi, arr[0])
		mx = max(mx, arr[len(arr)-1])
	}
	return ans
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
function maxDistance(arrays: number[][]): number {
    let ans = 0;
    let [mi, mx] = [arrays[0][0], arrays[0].at(-1)!];
    for (let i = 1; i < arrays.length; ++i) {
        const arr = arrays[i];
        const a = Math.abs(arr[0] - mx);
        const b = Math.abs(arr.at(-1)! - mi);
        ans = Math.max(ans, a, b);
        mi = Math.min(mi, arr[0]);
        mx = Math.max(mx, arr.at(-1)!);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_distance(arrays: Vec<Vec<i32>>) -> i32 {
        let mut ans = 0;
        let mut mi = arrays[0][0];
        let mut mx = arrays[0][arrays[0].len() - 1];

        for i in 1..arrays.len() {
            let arr = &arrays[i];
            let a = (arr[0] - mx).abs();
            let b = (arr[arr.len() - 1] - mi).abs();
            ans = ans.max(a).max(b);

            mi = mi.min(arr[0]);
            mx = mx.max(arr[arr.len() - 1]);
        }

        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {number[][]} arrays
 * @return {number}
 */
var maxDistance = function (arrays) {
    let ans = 0;
    let [mi, mx] = [arrays[0][0], arrays[0].at(-1)];
    for (let i = 1; i < arrays.length; ++i) {
        const arr = arrays[i];
        const a = Math.abs(arr[0] - mx);
        const b = Math.abs(arr.at(-1) - mi);
        ans = Math.max(ans, a, b);
        mi = Math.min(mi, arr[0]);
        mx = Math.max(mx, arr.at(-1));
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
