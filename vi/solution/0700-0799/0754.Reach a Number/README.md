---
comments: true
difficulty: Medium
tags:
    - Math
    - Binary Search
---

<!-- problem:start -->

# [754. Reach a Number](https://leetcode.com/problems/reach-a-number)

[中文文档](/solution/0700-0799/0754.Reach%20a%20Number/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn đang đứng tại vị trí <code>0</code> trên trục số vô hạn. Điểm đích nằm tại vị trí <code>target</code>.</p>

<p>Bạn thực hiện một số lần di chuyển <code>numMoves</code> theo quy tắc:</p>

<ul>
	<li>Mỗi lần di chuyển, bạn có thể đi sang trái hoặc phải.</li>
	<li>Ở lần di chuyển thứ <code>i<sup>th</sup></code> (với <code>i == 1</code> đến <code>i == numMoves</code>), bạn đi <code>i</code> bước theo hướng đã chọn.</li>
</ul>

<p>Cho số nguyên <code>target</code>, hãy trả về <em>số lần di chuyển <strong>ít nhất</strong> cần thiết (tức giá trị <code>numMoves</code> nhỏ nhất) để tới đích</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> target = 2
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
Ở lần di chuyển thứ 1<sup>st</sup>, ta đi từ 0 đến 1 (1 bước).
Ở lần di chuyển thứ 2<sup>nd</sup>, ta đi từ 1 đến -1 (2 bước).
Ở lần di chuyển thứ 3<sup>rd</sup>, ta đi từ -1 đến 2 (3 bước).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> target = 3
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Ở lần di chuyển thứ 1<sup>st</sup>, ta đi từ 0 đến 1 (1 bước).
Ở lần di chuyển thứ 2<sup>nd</sup>, ta đi từ 1 đến 3 (2 bước).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>-10<sup>9</sup> &lt;= target &lt;= 10<sup>9</sup></code></li>
	<li><code>target != 0</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phân tích toán học

<!-- thinking:start -->

> **Tư duy**
>
> Ở bước $i$, quãng đường phải là $i$; ta chỉ chọn dấu hướng đi. $|\textit{target}|$ có thể lên đến $10^9$, nên không thể thử từng chuỗi di chuyển.
>
> Trục số đối xứng nên ta lấy giá trị tuyệt đối. Sau $k$ bước sang phải, ta tới số tam giác $s$. Nếu $s-\textit{target}$ là số chẵn, đổi hướng một bước có độ dài $(s-\textit{target})/2$ sẽ đưa ta tới đích mà không cần thêm bước.
>
> Tăng $k$ cho đến khi $s\ge \textit{target}$ và hiệu là số chẵn. $k$ có độ lớn $O(\sqrt{|\textit{target}|})$.

<!-- thinking:end -->

Do trục số đối xứng và mỗi lần đều có thể chọn đi trái hoặc phải, ta có thể lấy giá trị tuyệt đối của $\textit{target}$.

Gọi $s$ là vị trí hiện tại và dùng biến $k$ để đếm số lần di chuyển. Ban đầu, cả $s$ và $k$ đều bằng $0$.

Ta liên tục cộng vào $s$ cho đến khi $s \ge \textit{target}$ và $(s - \textit{target}) \bmod 2 = 0$. Khi đó, số lần di chuyển $k$ chính là đáp án và ta trả về ngay.

Vì sao? Nếu $s \ge \textit{target}$ và $(s - \textit{target}) \bmod 2 = 0$, ta chỉ cần đổi dấu số nguyên dương $\frac{s - \textit{target}}{2}$ thành số âm để $s$ bằng $\textit{target}$. Đổi dấu một số nguyên dương về cơ bản tương ứng với đổi hướng di chuyển, nhưng số lần di chuyển không đổi.

Độ phức tạp thời gian là $O(\sqrt{\left | \textit{target} \right | })$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def reachNumber(self, target: int) -> int:
        target = abs(target)
        s = k = 0
        while 1:
            if s >= target and (s - target) % 2 == 0:
                return k
            k += 1
            s += k
```

#### Java

```java
class Solution {
    public int reachNumber(int target) {
        target = Math.abs(target);
        int s = 0, k = 0;
        while (true) {
            if (s >= target && (s - target) % 2 == 0) {
                return k;
            }
            ++k;
            s += k;
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int reachNumber(int target) {
        target = abs(target);
        int s = 0, k = 0;
        while (1) {
            if (s >= target && (s - target) % 2 == 0) return k;
            ++k;
            s += k;
        }
    }
};
```

#### Go

```go
func reachNumber(target int) int {
	if target < 0 {
		target = -target
	}
	var s, k int
	for {
		if s >= target && (s-target)%2 == 0 {
			return k
		}
		k++
		s += k
	}
}
```

#### JavaScript

```js
/**
 * @param {number} target
 * @return {number}
 */
var reachNumber = function (target) {
    target = Math.abs(target);
    let [s, k] = [0, 0];
    while (1) {
        if (s >= target && (s - target) % 2 == 0) {
            return k;
        }
        ++k;
        s += k;
    }
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
