---
comments: true
difficulty: Easy
tags:
    - Math
---

<!-- problem:start -->

# [492. Construct the Rectangle](https://leetcode.com/problems/construct-the-rectangle)

[中文文档](/solution/0400-0499/0492.Construct%20the%20Rectangle/README.md)

## Mô tả

<!-- description:start -->

<p>Một web developer cần biết cách thiết kế kích thước trang web. Cho diện tích của một trang web hình chữ nhật, hãy thiết kế trang có chiều dài L và chiều rộng W thỏa mãn các yêu cầu sau:</p>

<ol>
	<li>Diện tích trang web hình chữ nhật phải bằng diện tích mục tiêu đã cho.</li>
	<li>Chiều rộng <code>W</code> không được lớn hơn chiều dài <code>L</code>, tức là <code>L &gt;= W</code>.</li>
	<li>Chênh lệch giữa chiều dài <code>L</code> và chiều rộng <code>W</code> phải nhỏ nhất có thể.</li>
</ol>

<p>Hãy trả về <em>mảng <code>[L, W]</code>, trong đó <code>L</code> và <code>W</code> lần lượt là chiều dài và chiều rộng của trang web đã thiết kế.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> area = 4
<strong>Đầu ra:</strong> [2,2]
<strong>Giải thích:</strong> Diện tích mục tiêu là 4; các cặp kích thước khả dĩ là [1,4], [2,2], [4,1]. 
Tuy nhiên, theo yêu cầu 2, [1,4] không hợp lệ; theo yêu cầu 3, [4,1] không tối ưu bằng [2,2]. Vì vậy, chiều dài L là 2 và chiều rộng W là 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> area = 37
<strong>Đầu ra:</strong> [37,1]
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> area = 122122
<strong>Đầu ra:</strong> [427,286]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= area &lt;= 10<sup>7</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Diện tích đã cố định; ta cần $L\ge W$ và tối thiểu hóa $L-W$, tức là chọn hình chữ nhật gần vuông nhất. Duyệt mọi ước số bắt đầu từ $1$ là không cần thiết.
>
> Bắt đầu với $W=\lfloor\sqrt{\textit{area}}\rfloor$ rồi giảm dần đến khi $W$ là ước của diện tích. Khi đó $L=\textit{area}/W$ không nhỏ hơn $W$, và chênh lệch giữa hai cạnh là nhỏ nhất có thể.
>
> Bắt đầu từ căn bậc hai rồi giảm dần sẽ gặp cặp thừa số gần nhau nhất trước tiên.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def constructRectangle(self, area: int) -> List[int]:
        w = int(sqrt(area))
        while area % w != 0:
            w -= 1
        return [area // w, w]
```

#### Java

```java
class Solution {
    public int[] constructRectangle(int area) {
        int w = (int) Math.sqrt(area);
        while (area % w != 0) {
            --w;
        }
        return new int[] {area / w, w};
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> constructRectangle(int area) {
        int w = sqrt(1.0 * area);
        while (area % w != 0) --w;
        return {area / w, w};
    }
};
```

#### Go

```go
func constructRectangle(area int) []int {
	w := int(math.Sqrt(float64(area)))
	for area%w != 0 {
		w--
	}
	return []int{area / w, w}
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
