---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [4062. Transform Array Using Pair Operations](https://leetcode.com/problems/transform-array-using-pair-operations)

[中文文档](/solution/4000-4099/4062.Transform%20Array%20Using%20Pair%20Operations/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng số nguyên <code>source</code> và <code>target</code>.</p>

<p>Trong một <strong>thao tác</strong>, bạn có thể chọn hai chỉ số <strong>khác nhau</strong> <code>i</code> và <code>j</code> trong <code>source</code>, cùng với một số nguyên bất kỳ <code>delta</code>. Sau đó cập nhật <code>source</code> như sau:</p>

<ul>
	<li><code>source[i] = source[i] + source[j] - delta</code></li>
	<li><code>source[j] = delta</code></li>
</ul>

<p>Trả về <code>true</code> nếu có thể biến <code>source</code> thành <code>target</code> sau khi thực hiện thao tác <strong>bất kỳ</strong> số lần nào, kể cả không lần nào. Nếu không, trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">source = [1,2,3], target = [0,2,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chọn các chỉ số <code>i = 0</code> và <code>j = 2</code>, đồng thời đặt <code>delta = 4</code>.</li>
	<li>Trước thao tác, <code>source[0] = 1</code> và <code>source[2] = 3</code>.</li>
	<li>Sau thao tác,
	<ul>
		<li><code>source[0] = 1 + 3 - 4 = 0</code></li>
		<li><code>source[2] = 4</code></li>
	</ul>
	</li>
	<li>Do đó, <code>source</code> trở thành <code>[0, 2, 4]</code>, bằng với <code>target</code>.</li>
	<li>Vì vậy, đáp án là <code>true</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">source = [-5,-5], target = [-15,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chọn các chỉ số <code>i = 1</code> và <code>j = 0</code>, đồng thời đặt <code>delta = -15</code>.</li>
	<li>Trước thao tác, <code>source[1] = -5</code> và <code>source[0] = -5</code>.</li>
	<li>Sau thao tác,
	<ul>
		<li><code>source[1] = -5 + (-5) - (-15) = 5</code></li>
		<li><code>source[0] = -15</code></li>
	</ul>
	</li>
	<li>Do đó, <code>source</code> trở thành <code>[-15, 5]</code>, bằng với <code>target</code>.</li>
	<li>Vì vậy, đáp án là <code>true</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">source = [1,2,1], target = [0,2,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có thể chứng minh rằng dù thực hiện các thao tác nào, không thể biến <code>source</code> thành <code>target</code>. Vì vậy, đáp án là <code>false</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= source.length == target.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>9</sup> &lt;= source[i], target[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: So sánh tổng các phần tử của mảng

<!-- thinking:start -->

> **Tư duy**
>
> Các mảng có thể có độ dài $10^5$, nên việc tìm kiếm các chuỗi thao tác sẽ không thể hoàn tất trong thời gian cho phép. Một thao tác thay $\textit{source}[j]$ bằng một số nguyên bất kỳ $\textit{delta}$ và cộng $\textit{source}[j]-\textit{delta}$ vào $\textit{source}[i]$. Tổng của hai vị trí được giữ nguyên, nên tổng của toàn bộ mảng cũng không đổi.
>
> Với $n\ge 2$, hai tổng bằng nhau là điều kiện đủ. Giữ chỉ số cuối làm vị trí đối tác và lần lượt từ trái sang phải đưa $n-1$ phần tử đầu tiên về các giá trị tương ứng trong $\textit{target}$. Khi đó, tổng được bảo toàn sẽ buộc phần tử cuối trở thành $\textit{target}[n-1]$.
>
> Chỉ cần so sánh hai tổng. Mỗi giá trị có trị tuyệt đối không quá $10^9$ và độ dài không quá $10^5$, nên tổng cần được lưu bằng số nguyên $64$ bit.

<!-- thinking:end -->

Một thao tác chọn hai chỉ số khác nhau $i$ và $j$ cùng với một số nguyên $\textit{delta}$, thay $\textit{source}[i]$ bằng $\textit{source}[i]+\textit{source}[j]-\textit{delta}$, đồng thời thay $\textit{source}[j]$ bằng $\textit{delta}$. Tổng của hai vị trí vẫn bằng tổng cũ, nên tổng của mảng không đổi. Hai mảng có tổng khác nhau không thể biến đổi lẫn nhau.

Hai mảng có tổng bằng nhau luôn có thể biến đổi lẫn nhau. Giữ chỉ số $n-1$ làm vị trí đối tác. Với $i=0,1,\ldots,n-2$, chọn

$$
\textit{delta}=\textit{source}[i]+\textit{source}[n-1]-\textit{target}[i].
$$

Sau thao tác, $\textit{source}[i]=\textit{target}[i]$. Khi $n-1$ phần tử đầu tiên khớp với $\textit{target}$, tổng bằng nhau buộc phần tử cuối bằng $\textit{target}[n-1]$.

Cộng dồn tổng bằng một số nguyên $64$ bit. Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canTransform(self, source: list[int], target: list[int]) -> bool:
        return sum(source) == sum(target)
```

#### Java

```java
class Solution {
    public boolean canTransform(int[] source, int[] target) {
        long d = 0;
        for (int i = 0; i < source.length; ++i) {
            d += source[i] - target[i];
        }
        return d == 0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canTransform(vector<int>& source, vector<int>& target) {
        long long s = 0;
        for (int x : source) {
            s += x;
        }
        for (int x : target) {
            s -= x;
        }
        return s == 0;
    }
};
```

#### Go

```go
func canTransform(source []int, target []int) bool {
	var s, t int64
	for _, x := range source {
		s += int64(x)
	}
	for _, x := range target {
		t += int64(x)
	}
	return s == t
}
```

#### TypeScript

```ts
function canTransform(source: number[], target: number[]): boolean {
    return (
        source.reduce((s, x) => s + BigInt(x), 0n) === target.reduce((s, x) => s + BigInt(x), 0n)
    );
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
