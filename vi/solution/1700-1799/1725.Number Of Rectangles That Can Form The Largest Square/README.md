---
comments: true
difficulty: Easy
rating: 1229
source: Weekly Contest 224 Q1
tags:
    - Array
---

<!-- problem:start -->

# [1725. Number Of Rectangles That Can Form The Largest Square](https://leetcode.com/problems/number-of-rectangles-that-can-form-the-largest-square)

[中文文档](/solution/1700-1799/1725.Number%20Of%20Rectangles%20That%20Can%20Form%20The%20Largest%20Square/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>rectangles</code>, trong đó <code>rectangles[i] = [l<sub>i</sub>, w<sub>i</sub>]</code> biểu diễn hình chữ nhật thứ <code>i<sup>th</sup></code> có chiều dài <code>l<sub>i</sub></code> và chiều rộng <code>w<sub>i</sub></code>.</p>

<p>Có thể cắt hình chữ nhật thứ <code>i<sup>th</sup></code> để tạo thành hình vuông có cạnh dài <code>k</code> nếu <code>k &lt;= l<sub>i</sub></code> và <code>k &lt;= w<sub>i</sub></code>. Ví dụ, với hình chữ nhật <code>[4,6]</code>, ta có thể cắt được hình vuông có cạnh dài nhiều nhất là <code>4</code>.</p>

<p>Gọi <code>maxLen</code> là độ dài cạnh của hình vuông <strong>lớn nhất</strong> có thể tạo ra từ một trong các hình chữ nhật đã cho.</p>

<p>Trả về <em><strong>số lượng</strong> hình chữ nhật có thể tạo hình vuông với cạnh dài </em><code>maxLen</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> rectangles = [[5,8],[3,9],[5,12],[16,5]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Các hình vuông lớn nhất có thể tạo từ từng hình chữ nhật có cạnh [5,3,5,5].
Hình vuông lớn nhất có cạnh dài 5 và có thể tạo được từ 3 hình chữ nhật.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> rectangles = [[2,3],[3,7],[4,3],[3,7]]
<strong>Đầu ra:</strong> 3
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= rectangles.length &lt;= 1000</code></li>
	<li><code>rectangles[i].length == 2</code></li>
	<li><code>1 &lt;= l<sub>i</sub>, w<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
	<li><code>l<sub>i</sub> != w<sub>i</sub></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt một lần

<!-- thinking:start -->

> **Tư duy**
>
> Hình vuông lớn nhất từ một hình chữ nhật có cạnh $\min(l,w)$. Ta cần đếm số hình chữ nhật đạt cạnh lớn nhất trên toàn bộ mảng.
>
> Một lần duyệt duy trì giá trị lớn nhất hiện tại $mx$ và số lần xuất hiện: đặt lại bộ đếm khi gặp cạnh lớn hơn, tăng bộ đếm khi bằng nhau. Không cần duyệt lần hai.

<!-- thinking:end -->

Ta định nghĩa biến $ans$ để ghi nhận số hình vuông có độ dài cạnh lớn nhất hiện tại, và biến $mx$ để ghi nhận độ dài cạnh lớn nhất hiện tại.

Ta duyệt mảng $rectangles$. Với mỗi hình chữ nhật $[l, w]$, đặt $x = \min(l, w)$. Nếu $mx < x$, ta đã tìm thấy cạnh dài hơn nên cập nhật $mx$ thành $x$ và đặt $ans$ thành $1$. Nếu $mx = x$, ta tìm thấy cạnh bằng cạnh lớn nhất hiện tại nên tăng $ans$ thêm $1$.

Cuối cùng, ta trả về $ans$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng $rectangles$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countGoodRectangles(self, rectangles: List[List[int]]) -> int:
        ans = mx = 0
        for l, w in rectangles:
            x = min(l, w)
            if mx < x:
                ans = 1
                mx = x
            elif mx == x:
                ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int countGoodRectangles(int[][] rectangles) {
        int ans = 0, mx = 0;
        for (var e : rectangles) {
            int x = Math.min(e[0], e[1]);
            if (mx < x) {
                mx = x;
                ans = 1;
            } else if (mx == x) {
                ++ans;
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countGoodRectangles(vector<vector<int>>& rectangles) {
        int ans = 0, mx = 0;
        for (auto& e : rectangles) {
            int x = min(e[0], e[1]);
            if (mx < x) {
                mx = x;
                ans = 1;
            } else if (mx == x) {
                ++ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countGoodRectangles(rectangles [][]int) (ans int) {
	mx := 0
	for _, e := range rectangles {
		x := min(e[0], e[1])
		if mx < x {
			mx = x
			ans = 1
		} else if mx == x {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function countGoodRectangles(rectangles: number[][]): number {
    let [ans, mx] = [0, 0];
    for (const [l, w] of rectangles) {
        const x = Math.min(l, w);
        if (mx < x) {
            mx = x;
            ans = 1;
        } else if (mx === x) {
            ++ans;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
