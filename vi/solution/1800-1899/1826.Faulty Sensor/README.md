---
comments: true
difficulty: Easy
tags:
    - Array
    - Two Pointers
---

<!-- problem:start -->

# [1826. Faulty Sensor 🔒](https://leetcode.com/problems/faulty-sensor)

[中文文档](/solution/1800-1899/1826.Faulty%20Sensor/README.md)

## Mô tả

<!-- description:start -->

<p>Một thí nghiệm đang được tiến hành trong phòng thí nghiệm. Để đảm bảo độ chính xác, có<strong> hai </strong>cảm biến cùng thu thập dữ liệu. Cho hai mảng <code>sensor1</code> và <code>sensor2</code>, trong đó <code>sensor1[i]</code> và <code>sensor2[i]</code> là điểm dữ liệu thứ <code>i<sup>th</sup></code> được hai cảm biến thu thập.</p>

<p>Tuy nhiên, loại cảm biến này có thể bị lỗi, khiến <strong>chính xác một</strong> điểm dữ liệu bị loại bỏ. Sau khi dữ liệu bị loại bỏ, mọi điểm dữ liệu ở <strong>bên phải</strong> điểm đó được <strong>dịch</strong> sang trái một vị trí, và điểm dữ liệu cuối cùng được thay bằng một <strong>giá trị ngẫu nhiên</strong>. Đảm bảo rằng giá trị ngẫu nhiên này <strong>không</strong> bằng giá trị bị loại bỏ.</p>

<ul>
<li>Ví dụ, nếu dữ liệu đúng là <code>[1,2,<u><strong>3</strong></u>,4,5]</code> và <code>3</code> bị loại bỏ, cảm biến có thể trả về <code>[1,2,4,5,<u><strong>7</strong></u>]</code> (vị trí cuối có thể là <strong>bất kỳ</strong> giá trị nào, không nhất thiết là <code>7</code>).</li>
</ul>

<p>Biết rằng có lỗi ở <strong>nhiều nhất một</strong> cảm biến. Trả về <em>số hiệu cảm biến (</em><code>1</code><em> hoặc </em><code>2</code><em>) bị lỗi. Nếu <strong>không có lỗi</strong> ở cả hai cảm biến hoặc nếu cảm biến<strong> không thể</strong> xác định cảm biến bị lỗi, trả về </em><code>-1</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> sensor1 = [2,3,4,5], sensor2 = [2,1,3,4]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Cảm biến 2 có các giá trị đúng.
Điểm dữ liệu thứ hai của cảm biến 2 bị loại bỏ, và giá trị cuối của cảm biến 1 được thay bằng 5.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> sensor1 = [2,2,2,2,2], sensor2 = [2,2,2,2,5]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không thể xác định cảm biến nào bị lỗi.
Loại bỏ giá trị cuối của một trong hai cảm biến đều có thể tạo ra đầu ra của cảm biến còn lại.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> sensor1 = [2,3,2,2,3,2], sensor2 = [2,3,2,3,2,7]
<strong>Đầu ra:</strong> 2
<strong>Giải thích: </strong>Cảm biến 1 có các giá trị đúng.
Điểm dữ liệu thứ tư của cảm biến 1 bị loại bỏ, và giá trị cuối của cảm biến 1 được thay bằng 7.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>sensor1.length == sensor2.length</code></li>
	<li><code>1 &lt;= sensor1.length &lt;= 100</code></li>
	<li><code>1 &lt;= sensor1[i], sensor2[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Một cảm biến loại bỏ một giá trị và dịch các giá trị còn lại; cảm biến kia là đúng. Ta phải xác định cảm biến lỗi hoặc báo rằng không thể phân biệt. Thử mọi vị trí bị loại bỏ có độ phức tạp $O(n^2)$; hai mảng chỉ khác nhau bởi một lần dịch, nên chỉ cần kiểm tra căn chỉnh một lần.
>
> Tìm vị trí lệch đầu tiên $i$, sau đó so sánh $sensor1[i+1:]$ với $sensor2[i:]$ và cặp hoán đổi. Phía không khớp là cảm biến bị lỗi; nếu cả hai cách căn chỉnh đều đúng, không thể xác định đáp án.

<!-- thinking:end -->

Duyệt hai mảng và tìm vị trí khác nhau đầu tiên $i$. Nếu $i \lt n - 1$, lặp để so sánh $sensor1[i + 1]$ và $sensor2[i]$; nếu chúng khác nhau, cảm biến $1$ bị lỗi, trả về $1$; nếu không, so sánh $sensor1[i]$ và $sensor2[i + 1]$; nếu chúng khác nhau, cảm biến $2$ bị lỗi, trả về $2$.

Nếu duyệt xong, nghĩa là không thể xác định cảm biến bị lỗi, trả về $-1$.

Độ phức tạp thời gian là $O(n)$, với $n$ là độ dài mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def badSensor(self, sensor1: List[int], sensor2: List[int]) -> int:
        i, n = 0, len(sensor1)
        while i < n - 1:
            if sensor1[i] != sensor2[i]:
                break
            i += 1
        while i < n - 1:
            if sensor1[i + 1] != sensor2[i]:
                return 1
            if sensor1[i] != sensor2[i + 1]:
                return 2
            i += 1
        return -1
```

#### Java

```java
class Solution {
    public int badSensor(int[] sensor1, int[] sensor2) {
        int i = 0;
        int n = sensor1.length;
        for (; i < n - 1 && sensor1[i] == sensor2[i]; ++i) {
        }
        for (; i < n - 1; ++i) {
            if (sensor1[i + 1] != sensor2[i]) {
                return 1;
            }
            if (sensor1[i] != sensor2[i + 1]) {
                return 2;
            }
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int badSensor(vector<int>& sensor1, vector<int>& sensor2) {
        int i = 0;
        int n = sensor1.size();
        for (; i < n - 1 && sensor1[i] == sensor2[i]; ++i) {}
        for (; i < n - 1; ++i) {
            if (sensor1[i + 1] != sensor2[i]) return 1;
            if (sensor1[i] != sensor2[i + 1]) return 2;
        }
        return -1;
    }
};
```

#### Go

```go
func badSensor(sensor1 []int, sensor2 []int) int {
	i, n := 0, len(sensor1)
	for ; i < n-1 && sensor1[i] == sensor2[i]; i++ {
	}
	for ; i < n-1; i++ {
		if sensor1[i+1] != sensor2[i] {
			return 1
		}
		if sensor1[i] != sensor2[i+1] {
			return 2
		}
	}
	return -1
}
```

#### TypeScript

```ts
function badSensor(sensor1: number[], sensor2: number[]): number {
    let i = 0;
    const n = sensor1.length;
    while (i < n - 1) {
        if (sensor1[i] !== sensor2[i]) {
            break;
        }
        ++i;
    }
    while (i < n - 1) {
        if (sensor1[i + 1] !== sensor2[i]) {
            return 1;
        }
        if (sensor1[i] !== sensor2[i + 1]) {
            return 2;
        }
        ++i;
    }
    return -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
