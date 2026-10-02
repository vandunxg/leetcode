---
comments: true
difficulty: Hard
tags:
    - Math
    - Dynamic Programming
    - Combinatorics
---

<!-- problem:start -->

# [458. Poor Pigs](https://leetcode.com/problems/poor-pigs)

[中文文档](/solution/0400-0499/0458.Poor%20Pigs/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>buckets</code> xô chất lỏng, trong đó <strong>chính xác một</strong> xô có độc. Để tìm ra xô nào có độc, bạn cho một số chú heo (tội nghiệp) uống chất lỏng rồi quan sát chúng có chết hay không. Không may là bạn chỉ có <code>minutesToTest</code> phút để xác định xô có độc.</p>

<p>Bạn có thể cho heo uống theo các bước sau:</p>

<ol>
	<li>Chọn một số heo còn sống để cho uống.</li>
	<li>Với mỗi con heo, chọn các xô để nó uống. Heo sẽ uống đồng thời từ tất cả các xô đã chọn và việc uống không tốn thời gian. Mỗi con heo có thể uống từ bất kỳ số lượng xô nào, và mỗi xô có thể được cho bất kỳ số lượng heo nào uống.</li>
	<li>Chờ <code>minutesToDie</code> phút. Trong thời gian này, bạn <strong>không được</strong> cho heo nào khác uống.</li>
	<li>Sau <code>minutesToDie</code> phút, những con heo đã uống từ xô có độc sẽ chết; các con còn lại sẽ sống.</li>
	<li>Lặp lại quy trình này cho đến khi hết thời gian.</li>
</ol>

<p>Cho <code>buckets</code>, <code>minutesToDie</code> và <code>minutesToTest</code>. Hãy trả về <em>số heo <strong>ít nhất</strong> cần dùng để xác định xô có độc trong khoảng thời gian cho phép</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> buckets = 4, minutesToDie = 15, minutesToTest = 15
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ta có thể xác định xô có độc như sau:
Ở thời điểm 0, cho heo thứ nhất uống từ xô 1 và 2, heo thứ hai uống từ xô 2 và 3.
Ở thời điểm 15, có 4 khả năng:
- Nếu chỉ heo thứ nhất chết thì xô 1 có độc.
- Nếu chỉ heo thứ hai chết thì xô 3 có độc.
- Nếu cả hai con đều chết thì xô 2 có độc.
- Nếu cả hai con đều sống thì xô 4 có độc.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> buckets = 4, minutesToDie = 15, minutesToTest = 30
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ta có thể xác định xô có độc như sau:
Ở thời điểm 0, cho heo thứ nhất uống từ xô 1, heo thứ hai uống từ xô 2.
Ở thời điểm 15, có 2 khả năng:
- Nếu một trong hai con heo chết thì xô đã cho con đó uống là xô có độc.
- Nếu cả hai con đều sống thì cho heo thứ nhất uống từ xô 3, heo thứ hai uống từ xô 4.
Ở thời điểm 30, một trong hai con heo chắc chắn sẽ chết; xô đã cho con đó uống chính là xô có độc.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= buckets &lt;= 1000</code></li>
	<li><code>1 &lt;=&nbsp;minutesToDie &lt;=&nbsp;minutesToTest &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi con heo có thể uống nhiều lần; thời điểm nó chết tương ứng với một chữ số trong hệ cơ số $b$, chứ không chỉ là một trạng thái sống/chết.
>
> Mỗi khoảng thời gian thử phân biệt được $\textit{minutesToTest}/\textit{minutesToDie}+1$ trạng thái (bao gồm cả sống sót). $x$ con heo phân biệt được $\textit{base}^x$ xô; ta cần tìm $x$ nhỏ nhất thỏa mãn điều đó.
>
> Nhân $1$ với $\textit{base}$ liên tiếp cho đến khi tích đạt hoặc vượt số lượng xô; kết quả chính là logarit làm tròn lên theo cơ số đó.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def poorPigs(self, buckets: int, minutesToDie: int, minutesToTest: int) -> int:
        base = minutesToTest // minutesToDie + 1
        res, p = 0, 1
        while p < buckets:
            p *= base
            res += 1
        return res
```

#### Java

```java
class Solution {
    public int poorPigs(int buckets, int minutesToDie, int minutesToTest) {
        int base = minutesToTest / minutesToDie + 1;
        int res = 0;
        for (int p = 1; p < buckets; p *= base) {
            ++res;
        }
        return res;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int poorPigs(int buckets, int minutesToDie, int minutesToTest) {
        int base = minutesToTest / minutesToDie + 1;
        int res = 0;
        for (int p = 1; p < buckets; p *= base) ++res;
        return res;
    }
};
```

#### Go

```go
func poorPigs(buckets int, minutesToDie int, minutesToTest int) int {
	base := minutesToTest/minutesToDie + 1
	res := 0
	for p := 1; p < buckets; p *= base {
		res++
	}
	return res
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
