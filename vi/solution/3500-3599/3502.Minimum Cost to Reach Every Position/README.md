---
comments: true
difficulty: Easy
rating: 1243
source: Weekly Contest 443 Q1
tags:
    - Array
---

<!-- problem:start -->

# [3502. Minimum Cost to Reach Every Position](https://leetcode.com/problems/minimum-cost-to-reach-every-position)

[中文文档](/solution/3500-3599/3502.Minimum%20Cost%20to%20Reach%20Every%20Position/README.md)

## Mô tả

<!-- description:start -->

<p data-end="438" data-start="104">Bạn được cho một mảng số nguyên <code data-end="119" data-start="113">cost</code> có kích thước <code data-end="131" data-start="128">n</code>. Hiện tại bạn đang ở vị trí <code data-end="166" data-start="163">n</code> (cuối hàng) trong một hàng gồm <code data-end="187" data-start="180">n + 1</code> người (được đánh số từ 0 đến <code data-end="218" data-start="215">n</code>).</p>

<p data-end="438" data-start="104">Bạn muốn tiến lên trong hàng, nhưng mỗi người đứng trước bạn sẽ yêu cầu một khoản tiền cụ thể để <strong>đổi chỗ</strong>. Chi phí đổi chỗ với người <code data-end="375" data-start="372">i</code> là <code data-end="397" data-start="388">cost[i]</code>.</p>

<p data-end="487" data-start="440">Bạn được phép đổi chỗ với mọi người như sau:</p>

<ul data-end="632" data-start="488">
    <li data-end="572" data-start="488">Nếu họ đứng trước bạn, bạn <strong>phải</strong> trả cho họ <code data-end="546" data-start="537">cost[i]</code> để đổi chỗ.</li>
    <li data-end="632" data-start="573">Nếu họ đứng sau bạn, họ có thể đổi chỗ với bạn miễn phí.</li>
</ul>

<p data-end="755" data-start="634">Hãy trả về một mảng <code>answer</code> có kích thước <code>n</code>, trong đó <code>answer[i]</code> là tổng chi phí <strong data-end="680" data-start="664">nhỏ nhất</strong> để đến được mỗi vị trí <code>i</code> trong hàng<font face="monospace">.</font></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">cost = [5,3,4,1,3,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[5,3,3,1,1,1]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta có thể đến từng vị trí theo cách sau:</p>

<ul>
    <li><code>i = 0</code>. Ta có thể đổi chỗ với người 0 với chi phí là 5.</li>
    <li><span class="example-io"><code><font face="monospace">i = </font>1</code>. Ta có thể đổi chỗ với người 1 với chi phí là 3.</span></li>
    <li><span class="example-io"><code>i = 2</code>. Ta có thể đổi chỗ với người 1 với chi phí là 3, sau đó đổi chỗ miễn phí với người 2.</span></li>
    <li><span class="example-io"><code>i = 3</code>. Ta có thể đổi chỗ với người 3 với chi phí là 1.</span></li>
    <li><span class="example-io"><code>i = 4</code>. Ta có thể đổi chỗ với người 3 với chi phí là 1, sau đó đổi chỗ miễn phí với người 4.</span></li>
    <li><span class="example-io"><code>i = 5</code>. Ta có thể đổi chỗ với người 3 với chi phí là 1, sau đó đổi chỗ miễn phí với người 5.</span></li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">cost = [1,2,4,6,7]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,1,1,1,1]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta có thể đổi chỗ với người 0 với chi phí <span class="example-io">1, sau đó có thể đến mọi vị trí <code>i</code> miễn phí.</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= n == cost.length &lt;= 100</code></li>
    <li><code>1 &lt;= cost[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Câu đố tư duy

<!-- thinking:start -->

> **Tư duy**
>
> Vị trí $i$ có thể đạt được bằng cách trả $\textit{cost}[i]$ hoặc trước tiên lên vị trí nào đó $j < i$ rồi đi tiếp mà không mất thêm chi phí. Do đó $\textit{ans}[i] = \min_{0 \le j \le i} \textit{cost}[j]$.
>
> Chỉ cần tính minimum tiền tố từ trái sang phải là có thể điền toàn bộ đáp án; không cần xây dựng đồ thị đường đi ngắn nhất.

<!-- thinking:end -->

Theo mô tả bài toán, chi phí nhỏ nhất để đến mỗi vị trí $i$ là chi phí nhỏ nhất trong đoạn từ $0$ đến $i$. Ta có thể dùng biến $\textit{mi}$ để ghi nhận chi phí nhỏ nhất từ $0$ đến $i$.

Bắt đầu từ $0$, ta duyệt qua từng vị trí $i$, cập nhật $\textit{mi}$ bằng $\text{min}(\textit{mi}, \text{cost}[i])$ ở mỗi bước, rồi gán $\textit{mi}$ cho vị trí thứ $i$ trong mảng đáp án.

Cuối cùng, trả về mảng đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{cost}$. Bỏ qua phần không gian dùng cho mảng đáp án, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minCosts(self, cost: List[int]) -> List[int]:
        n = len(cost)
        ans = [0] * n
        mi = cost[0]
        for i, c in enumerate(cost):
            mi = min(mi, c)
            ans[i] = mi
        return ans
```

#### Java

```java
class Solution {
    public int[] minCosts(int[] cost) {
        int n = cost.length;
        int[] ans = new int[n];
        int mi = cost[0];
        for (int i = 0; i < n; ++i) {
            mi = Math.min(mi, cost[i]);
            ans[i] = mi;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> minCosts(vector<int>& cost) {
        int n = cost.size();
        vector<int> ans(n);
        int mi = cost[0];
        for (int i = 0; i < n; ++i) {
            mi = min(mi, cost[i]);
            ans[i] = mi;
        }
        return ans;
    }
};
```

#### Go

```go
func minCosts(cost []int) []int {
    n := len(cost)
    ans := make([]int, n)
    mi := cost[0]
    for i, c := range cost {
        mi = min(mi, c)
        ans[i] = mi
    }
    return ans
}
```

#### TypeScript

```ts
function minCosts(cost: number[]): number[] {
    const n = cost.length;
    const ans: number[] = Array(n).fill(0);
    let mi = cost[0];
    for (let i = 0; i < n; ++i) {
        mi = Math.min(mi, cost[i]);
        ans[i] = mi;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
