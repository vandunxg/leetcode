---
comments: true
difficulty: Medium
rating: 1538
source: Biweekly Contest 72 Q3
tags:
    - Greedy
    - Math
    - Backtracking
---

<!-- problem:start -->

# [2178. Maximum Split of Positive Even Integers](https://leetcode.com/problems/maximum-split-of-positive-even-integers)

[中文文档](/solution/2100-2199/2178.Maximum%20Split%20of%20Positive%20Even%20Integers/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>finalSum</code>. Hãy phân tách nó thành tổng của số lượng <strong>lớn nhất</strong> các số nguyên dương chẵn <strong>khác nhau</strong>.</p>

<ul>
	<li>Ví dụ, với <code>finalSum = 12</code>, các cách phân tách sau là <strong>hợp lệ</strong> (các số nguyên dương chẵn khác nhau có tổng bằng <code>finalSum</code>): <code>(12)</code>, <code>(2 + 10)</code>, <code>(2 + 4 + 6)</code> và <code>(4 + 8)</code>. Trong số đó, <code>(2 + 4 + 6)</code> chứa số lượng số nguyên lớn nhất. Lưu ý rằng không thể phân tách <code>finalSum</code> thành <code>(2 + 2 + 4 + 4)</code> vì tất cả các số phải khác nhau.</li>
</ul>

<p>Trả về <em>một danh sách các số nguyên biểu diễn một cách phân tách hợp lệ chứa số lượng số nguyên <strong>lớn nhất</strong></em>. Nếu không tồn tại cách phân tách hợp lệ nào cho <code>finalSum</code>, hãy trả về <em>một mảng <strong>rỗng</strong></em>. Bạn có thể trả về các số nguyên theo <strong>bất kỳ</strong> thứ tự nào.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> finalSum = 12
<strong>Đầu ra:</strong> [2,4,6]
<strong>Giải thích:</strong> Các cách phân tách hợp lệ là: <code>(12)</code>, <code>(2 + 10)</code>, <code>(2 + 4 + 6)</code> và <code>(4 + 8)</code>.
(2 + 4 + 6) chứa số lượng số nguyên lớn nhất, là 3. Vì vậy, ta trả về [2,4,6].
Lưu ý rằng [2,6,4], [6,2,4], v.v. cũng được chấp nhận.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> finalSum = 7
<strong>Đầu ra:</strong> []
<strong>Giải thích:</strong> Không có cách phân tách hợp lệ nào cho finalSum đã cho.
Vì vậy, ta trả về một mảng rỗng.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> finalSum = 28
<strong>Đầu ra:</strong> [6,8,2,12]
<strong>Giải thích:</strong> Các cách phân tách hợp lệ là: <code>(2 + 26)</code>, <code>(6 + 8 + 2 + 12)</code> và <code>(4 + 24)</code>.
<code>(6 + 8 + 2 + 12)</code> chứa số lượng số nguyên lớn nhất, là 4. Vì vậy, ta trả về [6,8,2,12].
Lưu ý rằng [10,2,4,12], [6,2,4,16], v.v. cũng được chấp nhận.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= finalSum &lt;= 10<sup>10</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Hãy phân tách một tổng chẵn thành càng nhiều số nguyên dương chẵn phân biệt càng tốt. Tổng lẻ là không thể phân tách. Việc giữ lại các giá trị lớn sẽ làm giảm số lượng phần tử.
>
> Lấy lần lượt $2,4,6,\ldots$ cho đến khi phần còn lại nhỏ hơn số chẵn tiếp theo, rồi cộng phần còn lại vào phần tử cuối cùng. Phần tử cuối cùng vẫn phân biệt vì phần còn lại nhỏ hơn số chẵn chưa được sử dụng tiếp theo.
>
> Trả về một danh sách rỗng khi tổng là số lẻ.

<!-- thinking:end -->

Nếu $\textit{finalSum}$ là số lẻ, nó không thể được phân tách thành tổng của các số nguyên dương chẵn phân biệt, nên ta trả về một mảng rỗng ngay lập tức.

Ngược lại, ta có thể tham lam phân tách $\textit{finalSum}$ theo thứ tự $2, 4, 6, \cdots$, cho đến khi $\textit{finalSum}$ không thể tiếp tục được phân tách thành một số nguyên dương chẵn khác. Khi đó, ta cộng $\textit{finalSum}$ còn lại vào số nguyên dương chẵn cuối cùng.

Độ phức tạp thời gian là $O(\sqrt{\textit{finalSum}})$, và không tính phần bộ nhớ được sử dụng bởi mảng kết quả, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumEvenSplit(self, finalSum: int) -> List[int]:
        if finalSum & 1:
            return []
        ans = []
        i = 2
        while i <= finalSum:
            finalSum -= i
            ans.append(i)
            i += 2
        ans[-1] += finalSum
        return ans
```

#### Java

```java
class Solution {
    public List<Long> maximumEvenSplit(long finalSum) {
        List<Long> ans = new ArrayList<>();
        if (finalSum % 2 == 1) {
            return ans;
        }
        for (long i = 2; i <= finalSum; i += 2) {
            ans.add(i);
            finalSum -= i;
        }
        ans.add(ans.remove(ans.size() - 1) + finalSum);
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<long long> maximumEvenSplit(long long finalSum) {
        vector<long long> ans;
        if (finalSum % 2) {
            return ans;
        }
        for (long long i = 2; i <= finalSum; i += 2) {
            ans.push_back(i);
            finalSum -= i;
        }
        ans.back() += finalSum;
        return ans;
    }
};
```

#### Go

```go
func maximumEvenSplit(finalSum int64) (ans []int64) {
	if finalSum%2 == 1 {
		return
	}
	for i := int64(2); i <= finalSum; i += 2 {
		ans = append(ans, i)
		finalSum -= i
	}
	ans[len(ans)-1] += finalSum
	return
}
```

#### TypeScript

```ts
function maximumEvenSplit(finalSum: number): number[] {
    const ans: number[] = [];
    if (finalSum % 2 === 1) {
        return ans;
    }
    for (let i = 2; i <= finalSum; i += 2) {
        ans.push(i);
        finalSum -= i;
    }
    ans[ans.length - 1] += finalSum;
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn maximum_even_split(mut final_sum: i64) -> Vec<i64> {
        let mut ans = Vec::new();
        if final_sum % 2 != 0 {
            return ans;
        }
        let mut i = 2;
        while i <= final_sum {
            ans.push(i);
            final_sum -= i;
            i += 2;
        }
        if let Some(last) = ans.last_mut() {
            *last += final_sum;
        }
        ans
    }
}
```

#### C#

```cs
public class Solution {
    public IList<long> MaximumEvenSplit(long finalSum) {
        IList<long> ans = new List<long>();
        if (finalSum % 2 == 1) {
            return ans;
        }
        for (long i = 2; i <= finalSum; i += 2) {
            ans.Add(i);
            finalSum -= i;
        }
        ans[ans.Count - 1] += finalSum;
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
