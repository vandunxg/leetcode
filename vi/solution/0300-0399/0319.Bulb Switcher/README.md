---
comments: true
difficulty: Medium
tags:
    - Brainteaser
    - Math
---

<!-- problem:start -->

# [319. Bulb Switcher](https://leetcode.com/problems/bulb-switcher)

[中文文档](/solution/0300-0399/0319.Bulb%20Switcher/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> bóng đèn ban đầu đều tắt. Trước tiên, bạn bật tất cả bóng đèn, sau đó tắt các bóng đèn ở mỗi vị trí thứ hai.</p>

<p>Ở lượt thứ ba, bạn đổi trạng thái mỗi bóng đèn thứ ba (đang tắt thì bật, đang bật thì tắt). Ở lượt thứ <code>i<sup>th</sup></code>, bạn đổi trạng thái mỗi bóng đèn thứ <code>i</code>. Ở lượt thứ <code>n<sup>th</sup></code>, bạn chỉ đổi trạng thái bóng đèn cuối cùng.</p>

<p>Hãy trả về <em>số bóng đèn đang bật sau <code>n</code> lượt</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0319.Bulb%20Switcher/images/bulb.jpg" style="width: 421px; height: 321px;" />
<pre>
<strong>Đầu vào:</strong> n = 3
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Ban đầu, ba bóng đèn ở trạng thái [tắt, tắt, tắt].
Sau lượt đầu tiên, ba bóng đèn ở trạng thái [bật, bật, bật].
Sau lượt thứ hai, ba bóng đèn ở trạng thái [bật, tắt, bật].
Sau lượt thứ ba, ba bóng đèn ở trạng thái [bật, tắt, tắt]. 
Vì chỉ còn một bóng đèn đang bật, nên kết quả cần trả về là 1.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 0
<strong>Đầu ra:</strong> 0
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= n &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Bóng đèn $i$ được đổi trạng thái một lần với mỗi ước của $i$, và cuối cùng sẽ bật khi số lần đổi trạng thái là số lẻ. Mô phỏng $n$ lượt tốn $O(n\log n)$, trong khi $n$ có thể lên đến $10^9$.
>
> Các ước thường đi thành từng cặp, ngoại trừ trường hợp số chính phương. Số lượng số chính phương trong đoạn $1\ldots n$ là $\lfloor\sqrt{n}\rfloor$, cũng chính là đáp án.

<!-- thinking:end -->

Ta đánh số $n$ bóng đèn từ $1$ đến $n$. Bóng đèn thứ $i$ được đổi trạng thái ở lượt thứ $d$ khi và chỉ khi $d$ là ước của $i$.

Mỗi số $i$ có hữu hạn ước. Nếu số lượng ước là lẻ, bóng đèn tương ứng sẽ bật ở trạng thái cuối; nếu không, bóng đèn sẽ tắt.

Vì vậy, ta chỉ cần đếm các số từ $1$ đến $n$ có số lượng ước là số lẻ.

Nếu số $i$ có ước $d$, thì nó cũng có ước $i/d$. Vì vậy, những số có số lượng ước lẻ phải là số chính phương.

Ví dụ, các ước của $12$ là $1, 2, 3, 4, 6, 12$, có $6$ ước nên số lượng là chẵn. Với số chính phương $16$, các ước là $1, 2, 4, 8, 16$, có $5$ ước nên số lượng là lẻ.

Do đó, ta chỉ cần đếm số chính phương từ $1$ đến $n$, bằng $\lfloor \sqrt{n} \rfloor$.

Độ phức tạp thời gian và không gian đều là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def bulbSwitch(self, n: int) -> int:
        return int(sqrt(n))
```

#### Java

```java
class Solution {
    public int bulbSwitch(int n) {
        return (int) Math.sqrt(n);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int bulbSwitch(int n) {
        return (int) sqrt(n);
    }
};
```

#### Go

```go
func bulbSwitch(n int) int {
	return int(math.Sqrt(float64(n)))
}
```

#### TypeScript

```ts
function bulbSwitch(n: number): number {
    return Math.floor(Math.sqrt(n));
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
