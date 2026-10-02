---
comments: true
difficulty: Medium
tags:
    - Array
---

<!-- problem:start -->

# [849. Maximize Distance to Closest Person](https://leetcode.com/problems/maximize-distance-to-closest-person)

[中文文档](/solution/0800-0899/0849.Maximize%20Distance%20to%20Closest%20Person/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng biểu diễn một hàng ghế <code>seats</code>, trong đó <code>seats[i] = 1</code> nghĩa là có người ngồi ở ghế thứ <code>i<sup>th</sup></code>, còn <code>seats[i] = 0</code> nghĩa là ghế thứ <code>i<sup>th</sup></code> đang trống <strong>(đánh chỉ số từ 0)</strong>.</p>

<p>Có ít nhất một ghế trống và ít nhất một người đang ngồi.</p>

<p>Alex muốn chọn ghế sao cho khoảng cách đến người gần nhất là lớn nhất.&nbsp;</p>

<p>Trả về <em>khoảng cách lớn nhất đó đến người gần nhất</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0849.Maximize%20Distance%20to%20Closest%20Person/images/distance.jpg" style="width: 650px; height: 257px;" />
<pre>
<strong>Đầu vào:</strong> seats = [1,0,0,0,1,0,1]
<strong>Đầu ra:</strong> 2
<strong>Giải thích: </strong>
Nếu Alex ngồi vào ghế trống thứ hai (tức <code>seats[2]</code>), người gần nhất cách anh 2 ghế.
Nếu Alex ngồi vào bất kỳ ghế trống nào khác, người gần nhất cách anh 1 ghế.
Vì vậy, khoảng cách lớn nhất đến người gần nhất là 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> seats = [1,0,0,0]
<strong>Đầu ra:</strong> 3
<strong>Giải thích: </strong>
Nếu Alex ngồi vào ghế cuối cùng (tức <code>seats[3]</code>), người gần nhất cách anh 3 ghế.
Đây là khoảng cách lớn nhất có thể đạt được, nên đáp án là 3.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> seats = [0,1]
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= seats.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>seats[i]</code>&nbsp;là <code>0</code> hoặc&nbsp;<code>1</code>.</li>
	<li>Có ít nhất một ghế <strong>trống</strong>.</li>
	<li>Có ít nhất một ghế <strong>có người ngồi</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt một lượt

<!-- thinking:start -->

> **Tư duy**
>
> Ta chọn một ghế trống để tối đa hóa khoảng cách đến người gần nhất. Ở hai đầu hàng ghế, khoảng cách là đến người ngồi đầu tiên hoặc cuối cùng; với khoảng trống ở giữa, vị trí tốt nhất là chính giữa.
>
> Duyệt một lượt để ghi nhận chỉ số người đầu tiên, người cuối cùng và khoảng cách lớn nhất giữa hai người. Đáp án là giá trị lớn nhất trong khoảng trống bên trái, khoảng trống bên phải và một nửa khoảng trống ở giữa.

<!-- thinking:end -->

Ta định nghĩa hai biến $\textit{first}$ và $\textit{last}$ lần lượt lưu vị trí người đầu tiên và người cuối cùng. Biến $d$ lưu khoảng cách lớn nhất giữa hai người.

Sau đó, duyệt mảng $\textit{seats}$. Nếu vị trí hiện tại có người ngồi và $\textit{last}$ đã được cập nhật, tức là đã có người trước đó, ta cập nhật $d = \max(d, i - \textit{last})$. Nếu $\textit{first}$ chưa được cập nhật, tức là chưa có người nào trước đó, ta gán $\textit{first} = i$. Cuối cùng, cập nhật $\textit{last} = i$.

Cuối cùng, trả về $\max(\textit{first}, n - \textit{last} - 1, d / 2)$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{seats}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxDistToClosest(self, seats: List[int]) -> int:
        first = last = None
        d = 0
        for i, c in enumerate(seats):
            if c:
                if last is not None:
                    d = max(d, i - last)
                if first is None:
                    first = i
                last = i
        return max(first, len(seats) - last - 1, d // 2)
```

#### Java

```java
class Solution {
    public int maxDistToClosest(int[] seats) {
        int first = -1, last = -1;
        int d = 0, n = seats.length;
        for (int i = 0; i < n; ++i) {
            if (seats[i] == 1) {
                if (last != -1) {
                    d = Math.max(d, i - last);
                }
                if (first == -1) {
                    first = i;
                }
                last = i;
            }
        }
        return Math.max(d / 2, Math.max(first, n - last - 1));
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxDistToClosest(vector<int>& seats) {
        int first = -1, last = -1;
        int d = 0, n = seats.size();
        for (int i = 0; i < n; ++i) {
            if (seats[i] == 1) {
                if (last != -1) {
                    d = max(d, i - last);
                }
                if (first == -1) {
                    first = i;
                }
                last = i;
            }
        }
        return max({d / 2, max(first, n - last - 1)});
    }
};
```

#### Go

```go
func maxDistToClosest(seats []int) int {
	first, last := -1, -1
	d := 0
	for i, c := range seats {
		if c == 1 {
			if last != -1 {
				d = max(d, i-last)
			}
			if first == -1 {
				first = i
			}
			last = i
		}
	}
	return max(d/2, max(first, len(seats)-last-1))
}
```

#### TypeScript

```ts
function maxDistToClosest(seats: number[]): number {
    let first = -1,
        last = -1;
    let d = 0,
        n = seats.length;
    for (let i = 0; i < n; ++i) {
        if (seats[i] === 1) {
            if (last !== -1) {
                d = Math.max(d, i - last);
            }
            if (first === -1) {
                first = i;
            }
            last = i;
        }
    }
    return Math.max(first, n - last - 1, d >> 1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
