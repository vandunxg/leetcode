---
comments: true
difficulty: Medium
tags:
    - Array
    - String
    - Backtracking
    - Enumeration
---

<!-- problem:start -->

# [949. Largest Time for Given Digits](https://leetcode.com/problems/largest-time-for-given-digits)

[中文文档](/solution/0900-0999/0949.Largest%20Time%20for%20Given%20Digits/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>arr</code> gồm 4 chữ số, hãy tìm thời gian muộn nhất theo định dạng 24 giờ có thể tạo bằng cách dùng mỗi chữ số <strong>đúng một lần</strong>.</p>

<p>Thời gian 24 giờ có định dạng <code>&quot;HH:MM&quot;</code>, trong đó <code>HH</code> nằm trong khoảng <code>00</code> đến <code>23</code>, còn <code>MM</code> nằm trong khoảng <code>00</code> đến <code>59</code>. Thời gian sớm nhất là <code>00:00</code>, muộn nhất là <code>23:59</code>.</p>

<p>Trả về <em>thời gian 24 giờ muộn nhất theo định dạng <code>&quot;HH:MM&quot;</code></em>. Nếu không thể tạo thời gian hợp lệ, trả về chuỗi rỗng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> arr = [1,2,3,4]
<strong>Output:</strong> &quot;23:41&quot;
<strong>Giải thích:</strong> Các thời gian 24 giờ hợp lệ là &quot;12:34&quot;, &quot;12:43&quot;, &quot;13:24&quot;, &quot;13:42&quot;, &quot;14:23&quot;, &quot;14:32&quot;, &quot;21:34&quot;, &quot;21:43&quot;, &quot;23:14&quot; và &quot;23:41&quot;. Trong số đó, &quot;23:41&quot; là thời gian muộn nhất.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> arr = [5,5,5,5]
<strong>Output:</strong> &quot;&quot;
<strong>Giải thích:</strong> Không có thời gian 24 giờ nào hợp lệ vì &quot;55:55&quot; không hợp lệ.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>arr.length == 4</code></li>
	<li><code>0 &lt;= arr[i] &lt;= 9</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê giờ và phút

<!-- thinking:start -->

> **Tư duy**
>
> Tạo thời gian hợp lệ muộn nhất từ bốn chữ số. Chỉ có $24\times 60$ cặp giờ-phút hợp lệ. Liệt kê từ lớn đến nhỏ và chọn cặp đầu tiên có tần suất chữ số trùng với đầu vào.

<!-- thinking:end -->

Liệt kê giờ hợp lệ $h \in [0,23]$ và phút $m \in [0,59]$ theo thứ tự giảm dần, rồi dùng mảng đếm để kiểm tra bốn chữ số có khớp với đầu vào hay không. Cặp đầu tiên khớp là thời gian hợp lệ muộn nhất.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def largestTimeFromDigits(self, arr: List[int]) -> str:
        cnt = [0] * 10
        for v in arr:
            cnt[v] += 1
        for h in range(23, -1, -1):
            for m in range(59, -1, -1):
                t = [0] * 10
                t[h // 10] += 1
                t[h % 10] += 1
                t[m // 10] += 1
                t[m % 10] += 1
                if cnt == t:
                    return f'{h:02}:{m:02}'
        return ''
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Brute Force (Hoán vị)

<!-- thinking:start -->

> **Tư duy**
>
> Việc liệt kê thời gian có độ phức tạp hằng số; ta cũng có thể hoán vị bốn chỉ số, tạo giờ và phút rồi giữ giá trị hợp lệ lớn nhất. Có $4!=24$ hoán vị nên cách này cũng đủ nhẹ.

<!-- thinking:end -->

Liệt kê mọi hoán vị của bốn chữ số, kiểm tra xem chúng có tạo thành thời gian hợp lệ không rồi giữ giá trị lớn nhất.

Độ phức tạp thời gian là $O(4^3)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def largestTimeFromDigits(self, arr: List[int]) -> str:
        ans = -1
        for i in range(4):
            for j in range(4):
                for k in range(4):
                    if i != j and i != k and j != k:
                        h = arr[i] * 10 + arr[j]
                        m = arr[k] * 10 + arr[6 - i - j - k]
                        if h < 24 and m < 60:
                            ans = max(ans, h * 60 + m)
        return '' if ans < 0 else f'{ans // 60:02}:{ans % 60:02}'
```

#### Java

```java
class Solution {
    public String largestTimeFromDigits(int[] arr) {
        int ans = -1;
        for (int i = 0; i < 4; ++i) {
            for (int j = 0; j < 4; ++j) {
                for (int k = 0; k < 4; ++k) {
                    if (i != j && j != k && i != k) {
                        int h = arr[i] * 10 + arr[j];
                        int m = arr[k] * 10 + arr[6 - i - j - k];
                        if (h < 24 && m < 60) {
                            ans = Math.max(ans, h * 60 + m);
                        }
                    }
                }
            }
        }
        return ans < 0 ? "" : String.format("%02d:%02d", ans / 60, ans % 60);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string largestTimeFromDigits(vector<int>& arr) {
        int ans = -1;
        for (int i = 0; i < 4; ++i) {
            for (int j = 0; j < 4; ++j) {
                for (int k = 0; k < 4; ++k) {
                    if (i != j && j != k && i != k) {
                        int h = arr[i] * 10 + arr[j];
                        int m = arr[k] * 10 + arr[6 - i - j - k];
                        if (h < 24 && m < 60) {
                            ans = max(ans, h * 60 + m);
                        }
                    }
                }
            }
        }
        if (ans < 0) return "";
        int h = ans / 60, m = ans % 60;
        return to_string(h / 10) + to_string(h % 10) + ":" + to_string(m / 10) + to_string(m % 10);
    }
};
```

#### Go

```go
func largestTimeFromDigits(arr []int) string {
	ans := -1
	for i := 0; i < 4; i++ {
		for j := 0; j < 4; j++ {
			for k := 0; k < 4; k++ {
				if i != j && j != k && i != k {
					h := arr[i]*10 + arr[j]
					m := arr[k]*10 + arr[6-i-j-k]
					if h < 24 && m < 60 {
						ans = max(ans, h*60+m)
					}
				}
			}
		}
	}
	if ans < 0 {
		return ""
	}
	return fmt.Sprintf("%02d:%02d", ans/60, ans%60)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
