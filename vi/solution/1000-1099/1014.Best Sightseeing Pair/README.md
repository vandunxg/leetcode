---
comments: true
difficulty: Medium
rating: 1730
source: Weekly Contest 129 Q3
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [1014. Best Sightseeing Pair](https://leetcode.com/problems/best-sightseeing-pair)

[中文文档](/solution/1000-1099/1014.Best%20Sightseeing%20Pair/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>values</code>, trong đó values[i] là giá trị của địa điểm tham quan thứ <code>i<sup>th</sup></code>. Khoảng cách giữa hai địa điểm <code>i</code> và <code>j</code> là <code>j - i</code>.</p>

<p>Điểm số của một cặp địa điểm tham quan (<code>i &lt; j</code>) là <code>values[i] + values[j] + i - j</code>: tổng giá trị của hai địa điểm trừ đi khoảng cách giữa chúng.</p>

<p>Hãy trả về <em>điểm số lớn nhất của một cặp địa điểm tham quan</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> values = [8,1,5,2,6]
<strong>Output:</strong> 11
<strong>Giải thích:</strong> i = 0, j = 2, values[i] + values[j] + i - j = 8 + 5 + 0 - 2 = 11
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> values = [1,2]
<strong>Output:</strong> 2
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= values.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= values[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Tính $values[i]+values[j]+i-j$ cho mọi cặp $i<j$ sẽ tốn thời gian bậc hai. Viết lại thành $(values[i]+i)+(values[j]-j)$ cho thấy, với mỗi $j$ cố định, ta chỉ cần giá trị lớn nhất của $values[i]+i$ ở bên trái.
>
> Duyệt $j$ từ trái sang phải và dùng một biến lưu prefix maximum đó, nhờ vậy có thể tính điểm tốt nhất của cặp kết thúc tại $j$ trong thời gian hằng số.
>
> Mỗi chỉ số một lần đóng vai trò điểm cuối bên phải và một lần làm ứng viên bên trái, nên tổng thời gian duyệt là tuyến tính.

<!-- thinking:end -->

Ta duyệt $j$ từ trái sang phải, đồng thời duy trì giá trị lớn nhất của $values[i] + i$ với các phần tử nằm bên trái $j$, ký hiệu là $mx$. Với mỗi $j$, điểm số lớn nhất là $mx + values[j] - j$. Đáp án là điểm số lớn nhất trong tất cả các vị trí.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{values}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxScoreSightseeingPair(self, values: List[int]) -> int:
        ans = mx = 0
        for j, x in enumerate(values):
            ans = max(ans, mx + x - j)
            mx = max(mx, x + j)
        return ans
```

#### Java

```java
class Solution {
    public int maxScoreSightseeingPair(int[] values) {
        int ans = 0, mx = 0;
        for (int j = 0; j < values.length; ++j) {
            ans = Math.max(ans, mx + values[j] - j);
            mx = Math.max(mx, values[j] + j);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxScoreSightseeingPair(vector<int>& values) {
        int ans = 0, mx = 0;
        for (int j = 0; j < values.size(); ++j) {
            ans = max(ans, mx + values[j] - j);
            mx = max(mx, values[j] + j);
        }
        return ans;
    }
};
```

#### Go

```go
func maxScoreSightseeingPair(values []int) (ans int) {
	mx := 0
	for j, x := range values {
		ans = max(ans, mx+x-j)
		mx = max(mx, x+j)
	}
	return
}
```

#### TypeScript

```ts
function maxScoreSightseeingPair(values: number[]): number {
    let [ans, mx] = [0, 0];
    for (let j = 0; j < values.length; ++j) {
        ans = Math.max(ans, mx + values[j] - j);
        mx = Math.max(mx, values[j] + j);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_score_sightseeing_pair(values: Vec<i32>) -> i32 {
        let mut ans = 0;
        let mut mx = 0;
        for (j, &x) in values.iter().enumerate() {
            ans = ans.max(mx + x - j as i32);
            mx = mx.max(x + j as i32);
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
