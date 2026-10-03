---
comments: true
difficulty: Medium
rating: 1759
source: Biweekly Contest 93 Q3
tags:
    - Greedy
    - Array
    - Binary Search
---

<!-- problem:start -->

# [2498. Frog Jump II](https://leetcode.com/problems/frog-jump-ii)

[中文文档](/solution/2400-2499/2498.Frog%20Jump%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>stones</code> được đánh chỉ số từ <strong>0</strong>, sắp xếp theo thứ tự <strong>tăng nghiêm ngặt</strong>, biểu diễn vị trí của các hòn đá trong một con sông.</p>

<p>Một con ếch, ban đầu ở hòn đá đầu tiên, muốn đi đến hòn đá cuối cùng rồi quay trở lại hòn đá đầu tiên. Tuy nhiên, nó chỉ có thể nhảy đến mỗi hòn đá <strong>nhiều nhất một lần</strong>.</p>

<p><strong>Độ dài</strong> của một lần nhảy là hiệu tuyệt đối giữa vị trí của hòn đá mà ếch đang đứng và vị trí của hòn đá mà ếch nhảy đến.</p>

<ul>
	<li>Cụ thể hơn, nếu ếch đang ở <code>stones[i]</code> và nhảy đến <code>stones[j]</code>, độ dài của lần nhảy là <code>|stones[i] - stones[j]|</code>.</li>
</ul>

<p><strong>Chi phí</strong> của một lộ trình là <strong>độ dài lớn nhất của một lần nhảy</strong> trong toàn bộ lộ trình.</p>

<p>Hãy trả về <em><strong>chi phí nhỏ nhất</strong> của một lộ trình mà ếch có thể thực hiện</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2498.Frog%20Jump%20II/images/2498_ex0.png" style="width: 500px; height: 190px;" />
<pre>
<strong>Đầu vào:</strong> stones = [0,2,5,6,7]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Hình trên biểu diễn một trong những lộ trình tối ưu mà ếch có thể thực hiện.
Chi phí của lộ trình này là 5, là độ dài lớn nhất của một lần nhảy.
Vì không thể đạt được chi phí nhỏ hơn 5, ta trả về giá trị này.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2498.Frog%20Jump%20II/images/2498_ex1.png" style="width: 500px; height: 190px;" />
<pre>
<strong>Đầu vào:</strong> stones = [0,3,9]
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong>
Ếch có thể nhảy trực tiếp đến hòn đá cuối cùng rồi quay trở lại hòn đá đầu tiên.
Trong trường hợp này, độ dài của mỗi lần nhảy là 9. Chi phí của lộ trình là max(9, 9) = 9.
Có thể chứng minh rằng đây là chi phí nhỏ nhất có thể đạt được.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= stones.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= stones[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>stones[0] == 0</code></li>
	<li><code>stones</code> được sắp xếp theo thứ tự tăng nghiêm ngặt.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ếch phải đi qua mọi hòn đá theo cả hai chiều, đồng thời tối thiểu hóa lần nhảy dài nhất. Với $n\le 10^5$, cách tối ưu là bỏ qua một hòn đá sau mỗi lần nhảy: đi qua các chỉ số chẵn theo một chiều và các chỉ số lẻ theo chiều còn lại. Đoạn dài nhất là khoảng cách gồm hai bước, bao gồm cả $stones[1]-stones[0]$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxJump(self, stones: List[int]) -> int:
        ans = stones[1] - stones[0]
        for i in range(2, len(stones)):
            ans = max(ans, stones[i] - stones[i - 2])
        return ans
```

#### Java

```java
class Solution {
    public int maxJump(int[] stones) {
        int ans = stones[1] - stones[0];
        for (int i = 2; i < stones.length; ++i) {
            ans = Math.max(ans, stones[i] - stones[i - 2]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxJump(vector<int>& stones) {
        int ans = stones[1] - stones[0];
        for (int i = 2; i < stones.size(); ++i) ans = max(ans, stones[i] - stones[i - 2]);
        return ans;
    }
};
```

#### Go

```go
func maxJump(stones []int) int {
	ans := stones[1] - stones[0]
	for i := 2; i < len(stones); i++ {
		ans = max(ans, stones[i]-stones[i-2])
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
