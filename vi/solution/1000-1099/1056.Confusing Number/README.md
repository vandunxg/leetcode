---
comments: true
difficulty: Easy
tags:
    - Math
---

<!-- problem:start -->

# [1056. Confusing Number 🔒](https://leetcode.com/problems/confusing-number)

[中文文档](/solution/1000-1099/1056.Confusing%20Number/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Số gây nhầm lẫn</strong> là số mà khi xoay <code>180</code> độ sẽ tạo thành một số khác, trong đó <strong>mọi chữ số đều hợp lệ</strong>.</p>

<p>Khi xoay các chữ số của một số <code>180</code> độ, một số chữ số sẽ biến thành chữ số khác.</p>

<ul>
	<li>Khi xoay <code>0</code>, <code>1</code>, <code>6</code>, <code>8</code> và <code>9</code> <code>180</code> độ, chúng lần lượt trở thành <code>0</code>, <code>1</code>, <code>9</code>, <code>8</code> và <code>6</code>.</li>
	<li>Khi xoay <code>2</code>, <code>3</code>, <code>4</code>, <code>5</code> và <code>7</code> <code>180</code> độ, chúng trở thành chữ số <strong>không hợp lệ</strong>.</li>
</ul>

<p>Lưu ý rằng sau khi xoay số, có thể bỏ qua các số 0 ở đầu.</p>

<ul>
	<li>Ví dụ, xoay <code>8000</code> ta được <code>0008</code>, được xem như số <code>8</code>.</li>
</ul>

<p>Cho số nguyên <code>n</code>, trả về <code>true</code> nếu đó là <strong>số gây nhầm lẫn</strong>, nếu không thì trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1000-1099/1056.Confusing%20Number/images/1268_1.png" style="width: 281px; height: 121px;" />
<pre>
<strong>Đầu vào:</strong> n = 6
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Xoay 6 ta được 9. Đây là số hợp lệ và 9 != 6.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1000-1099/1056.Confusing%20Number/images/1268_2.png" style="width: 312px; height: 121px;" />
<pre>
<strong>Đầu vào:</strong> n = 89
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Xoay 89 ta được 68. Đây là số hợp lệ và 68 != 89.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1000-1099/1056.Confusing%20Number/images/1268_3.png" style="width: 301px; height: 121px;" />
<pre>
<strong>Đầu vào:</strong> n = 11
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Xoay 11 ta vẫn được 11. Đây là số hợp lệ nhưng giá trị không thay đổi, nên 11 không phải số gây nhầm lẫn.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= n &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ các chữ số $0,1,6,8,9$ còn hợp lệ sau khi xoay $180^\circ$, trong đó $6$ đổi thành $9$ và ngược lại. Duyệt các chữ số từ hàng thấp lên cao, đồng thời tạo số sau khi xoay, để xác định số đó có gây nhầm lẫn hay không.
>
> Mảng $d$ ánh xạ từng chữ số sang kết quả sau khi xoay; nếu gặp chữ số không hợp lệ thì trả về false ngay. Ghép các chữ số đã ánh xạ vào $y$ sẽ tạo ra số sau khi xoay.
>
> Số đó gây nhầm lẫn khi và chỉ khi $y\neq n$. Các số 0 ở đầu tự động bị bỏ qua khi biểu diễn dưới dạng số nguyên.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def confusingNumber(self, n: int) -> bool:
        x, y = n, 0
        d = [0, 1, -1, -1, -1, -1, 9, -1, 8, 6]
        while x:
            x, v = divmod(x, 10)
            if d[v] < 0:
                return False
            y = y * 10 + d[v]
        return y != n
```

#### Java

```java
class Solution {
    public boolean confusingNumber(int n) {
        int[] d = new int[] {0, 1, -1, -1, -1, -1, 9, -1, 8, 6};
        int x = n, y = 0;
        while (x > 0) {
            int v = x % 10;
            if (d[v] < 0) {
                return false;
            }
            y = y * 10 + d[v];
            x /= 10;
        }
        return y != n;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool confusingNumber(int n) {
        vector<int> d = {0, 1, -1, -1, -1, -1, 9, -1, 8, 6};
        int x = n, y = 0;
        while (x) {
            int v = x % 10;
            if (d[v] < 0) {
                return false;
            }
            y = y * 10 + d[v];
            x /= 10;
        }
        return y != n;
    }
};
```

#### Go

```go
func confusingNumber(n int) bool {
	d := []int{0, 1, -1, -1, -1, -1, 9, -1, 8, 6}
	x, y := n, 0
	for x > 0 {
		v := x % 10
		if d[v] < 0 {
			return false
		}
		y = y*10 + d[v]
		x /= 10
	}
	return y != n
}
```

#### PHP

```php
class Solution {
    /**
     * @param Integer $n
     * @return Boolean
     */
    function confusingNumber($n) {
        $d = [0, 1, -1, -1, -1, -1, 9, -1, 8, 6];
        $x = $n;
        $y = 0;
        while ($x > 0) {
            $v = $x % 10;
            if ($d[$v] < 0) {
                return false;
            }
            $y = $y * 10 + $d[$v];
            $x = intval($x / 10);
        }
        return $y != $n;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
