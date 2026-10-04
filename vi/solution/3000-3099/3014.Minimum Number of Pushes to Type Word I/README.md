---
comments: true
difficulty: Easy
rating: 1324
source: Weekly Contest 381 Q1
tags:
    - Greedy
    - Math
    - String
---

<!-- problem:start -->

# [3014. Minimum Number of Pushes to Type Word I](https://leetcode.com/problems/minimum-number-of-pushes-to-type-word-i)

[中文文档](/solution/3000-3099/3014.Minimum%20Number%20of%20Pushes%20to%20Type%20Word%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>word</code> chứa các chữ cái tiếng Anh viết thường <strong>khác nhau</strong>.</p>

<p>Trên bàn phím điện thoại, các phím được ánh xạ tới các tập hợp chữ cái tiếng Anh viết thường <strong>khác nhau</strong>, có thể dùng để tạo từ bằng cách nhấn phím. Ví dụ, phím <code>2</code> được ánh xạ tới <code>[&quot;a&quot;,&quot;b&quot;,&quot;c&quot;]</code>; cần nhấn phím một lần để nhập <code>&quot;a&quot;</code>, hai lần để nhập <code>&quot;b&quot;</code> và ba lần để nhập <code>&quot;c&quot;</code> <em>.</em></p>

<p>Bạn được phép ánh xạ lại các phím có số từ <code>2</code> đến <code>9</code> tới các tập hợp chữ cái <strong>khác nhau</strong>. Các phím có thể được ánh xạ tới <strong>số lượng chữ cái bất kỳ</strong>, nhưng mỗi chữ cái <strong>phải</strong> được ánh xạ tới <strong>chính xác</strong> một phím. Bạn cần tìm <strong>số lần nhấn phím nhỏ nhất</strong> để nhập chuỗi <code>word</code>.</p>

<p>Trả về <em><strong>số lần nhấn phím nhỏ nhất</strong> cần thiết để nhập </em><code>word</code> <em>sau khi ánh xạ lại các phím</em>.</p>

<p>Hình dưới đây minh họa một cách ánh xạ chữ cái tới các phím trên bàn phím điện thoại. Lưu ý rằng <code>1</code>, <code>*</code>, <code>#</code> và <code>0</code> <strong>không</strong> được ánh xạ tới chữ cái nào.</p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3014.Minimum%20Number%20of%20Pushes%20to%20Type%20Word%20I/images/keypaddesc.png" style="width: 329px; height: 313px;" />
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3014.Minimum%20Number%20of%20Pushes%20to%20Type%20Word%20I/images/keypadv1e1.png" style="width: 329px; height: 313px;" />
<pre>
<strong>Đầu vào:</strong> word = &quot;abcde&quot;
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Bàn phím sau khi ánh xạ lại như trong hình cho chi phí nhỏ nhất.
&quot;a&quot; -&gt; nhấn phím 2 một lần
&quot;b&quot; -&gt; nhấn phím 3 một lần
&quot;c&quot; -&gt; nhấn phím 4 một lần
&quot;d&quot; -&gt; nhấn phím 5 một lần
&quot;e&quot; -&gt; nhấn phím 6 một lần
Tổng chi phí là 1 + 1 + 1 + 1 + 1 = 5.
Có thể chứng minh rằng không có cách ánh xạ nào khác cho chi phí nhỏ hơn.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3014.Minimum%20Number%20of%20Pushes%20to%20Type%20Word%20I/images/keypadv1e2.png" style="width: 329px; height: 313px;" />
<pre>
<strong>Đầu vào:</strong> word = &quot;xycdefghij&quot;
<strong>Đầu ra:</strong> 12
<strong>Giải thích:</strong> Bàn phím sau khi ánh xạ lại như trong hình cho chi phí nhỏ nhất.
&quot;x&quot; -&gt; nhấn phím 2 một lần
&quot;y&quot; -&gt; nhấn phím 2 hai lần
&quot;c&quot; -&gt; nhấn phím 3 một lần
&quot;d&quot; -&gt; nhấn phím 3 hai lần
&quot;e&quot; -&gt; nhấn phím 4 một lần
&quot;f&quot; -&gt; nhấn phím 5 một lần
&quot;g&quot; -&gt; nhấn phím 6 một lần
&quot;h&quot; -&gt; nhấn phím 7 một lần
&quot;i&quot; -&gt; nhấn phím 8 một lần
&quot;j&quot; -&gt; nhấn phím 9 một lần
Tổng chi phí là 1 + 2 + 1 + 2 + 1 + 1 + 1 + 1 + 1 + 1 = 12.
Có thể chứng minh rằng không có cách ánh xạ nào khác cho chi phí nhỏ hơn.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= word.length &lt;= 26</code></li>
    <li><code>word</code> chỉ bao gồm các chữ cái tiếng Anh viết thường.</li>
    <li>Tất cả chữ cái trong <code>word</code> đều khác nhau.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thuật toán tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Các chữ cái khác nhau và có nhiều nhất $26$ chữ cái. Có tám phím, với chi phí là $1,2,\ldots$ lần nhấn tùy theo số chữ cái đã được đặt trên mỗi phím.
>
> Khi tần suất của mọi chữ cái đều bằng nhau, cách tối ưu là phân bố đều các chữ cái trên tám phím, trước tiên lấp đầy từng lượt gồm tám “ô” có chi phí $k$ lần nhấn.
>
> Ta cộng $k \times 8$ cho mỗi lượt đầy đủ và tính phần dư ở mức $k$ tiếp theo mà không cần gán cụ thể các chữ cái.

<!-- thinking:end -->

Ta nhận thấy tất cả chữ cái trong chuỗi $word$ đều khác nhau. Vì vậy, ta có thể dùng chiến lược greedy để phân bố đều các chữ cái trên $8$ phím, từ đó giảm thiểu số lần nhấn phím.

Độ phức tạp thời gian là $O(n / 8)$, trong đó $n$ là độ dài của chuỗi $word$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumPushes(self, word: str) -> int:
        n = len(word)
        ans, k = 0, 1
        for _ in range(n // 8):
            ans += k * 8
            k += 1
        ans += k * (n % 8)
        return ans
```

#### Java

```java
class Solution {
    public int minimumPushes(String word) {
        int n = word.length();
        int ans = 0, k = 1;
        for (int i = 0; i < n / 8; ++i) {
            ans += k * 8;
            ++k;
        }
        ans += k * (n % 8);
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumPushes(string word) {
        int n = word.size();
        int ans = 0, k = 1;
        for (int i = 0; i < n / 8; ++i) {
            ans += k * 8;
            ++k;
        }
        ans += k * (n % 8);
        return ans;
    }
};
```

#### Go

```go
func minimumPushes(word string) (ans int) {
	n := len(word)
	k := 1
	for i := 0; i < n/8; i++ {
		ans += k * 8
		k++
	}
	ans += k * (n % 8)
	return
}
```

#### TypeScript

```ts
function minimumPushes(word: string): number {
    const n = word.length;
    let ans = 0;
    let k = 1;
    for (let i = 0; i < ((n / 8) | 0); ++i) {
        ans += k * 8;
        ++k;
    }
    ans += k * (n % 8);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
