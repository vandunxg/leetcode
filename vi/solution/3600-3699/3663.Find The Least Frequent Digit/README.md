---
comments: true
difficulty: Easy
rating: 1284
source: Biweekly Contest 164 Q1
tags:
    - Array
    - Hash Table
    - Math
    - Counting
---

<!-- problem:start -->

# [3663. Find The Least Frequent Digit](https://leetcode.com/problems/find-the-least-frequent-digit)

[中文文档](/solution/3600-3699/3663.Find%20The%20Least%20Frequent%20Digit/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>n</code>, hãy tìm chữ số xuất hiện <strong>ít nhất</strong> trong biểu diễn thập phân của nó. Nếu có nhiều chữ số có cùng tần suất xuất hiện, hãy chọn chữ số <strong>nhỏ nhất</strong>.</p>

<p>Trả về chữ số được chọn dưới dạng số nguyên.</p>
<strong>Tần suất</strong> của một chữ số <code>x</code> là số lần chữ số đó xuất hiện trong biểu diễn thập phân của <code>n</code>.
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 1553322</span></p>

<p><strong>Đầu ra:</strong> 1</p>

<p><strong>Giải thích:</strong></p>

<p>Chữ số xuất hiện ít nhất trong <code>n</code> là 1, chỉ xuất hiện một lần. Các chữ số khác đều xuất hiện hai lần.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 723344511</span></p>

<p><strong>Đầu ra:</strong> 2</p>

<p><strong>Giải thích:</strong></p>

<p>Các chữ số xuất hiện ít nhất trong <code>n</code> là 7, 2 và 5; mỗi chữ số chỉ xuất hiện một lần.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 2<sup>31</sup>​​​​​​​ - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Trong các chữ số thập phân của $n$, ta cần tìm chữ số có tần suất nhỏ nhất và ưu tiên chữ số nhỏ hơn nếu hòa. Chỉ cần mười ô đếm là đủ.
>
> Tách từng chữ số bằng $\textit{divmod}$, sau đó duyệt các ô đếm và giữ lại tần suất dương nhỏ hơn nghiêm ngặt.
>
> Bỏ qua các chữ số không xuất hiện. Có $O(\log n)$ chữ số.

<!-- thinking:end -->

Ta sử dụng một mảng $\textit{cnt}$ để đếm tần suất của mỗi chữ số. Ta duyệt qua từng chữ số của số $n$ và cập nhật mảng $\textit{cnt}$.

Sau đó, ta dùng biến $f$ để ghi nhận tần suất nhỏ nhất hiện tại trong các chữ số, và biến $\textit{ans}$ để ghi nhận chữ số tương ứng.

Tiếp theo, ta duyệt qua mảng $\textit{cnt}$. Nếu $0 < \textit{cnt}[x] < f$, nghĩa là ta đã tìm thấy một chữ số có tần suất nhỏ hơn, nên cập nhật $f = \textit{cnt}[x]$ và $\textit{ans} = x$.

Sau khi duyệt xong, ta trả về $\textit{ans}$ làm đáp án.

Độ phức tạp thời gian là $O(\log n)$, còn độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getLeastFrequentDigit(self, n: int) -> int:
        cnt = [0] * 10
        while n:
            n, x = divmod(n, 10)
            cnt[x] += 1
        ans, f = 0, inf
        for x, v in enumerate(cnt):
            if 0 < v < f:
                f = v
                ans = x
        return ans
```

#### Java

```java
class Solution {
    public int getLeastFrequentDigit(int n) {
        int[] cnt = new int[10];
        for (; n > 0; n /= 10) {
            ++cnt[n % 10];
        }
        int ans = 0, f = 1 << 30;
        for (int x = 0; x < 10; ++x) {
            if (cnt[x] > 0 && cnt[x] < f) {
                f = cnt[x];
                ans = x;
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
    int getLeastFrequentDigit(int n) {
        int cnt[10]{};
        for (; n > 0; n /= 10) {
            ++cnt[n % 10];
        }
        int ans = 0, f = 1 << 30;
        for (int x = 0; x < 10; ++x) {
            if (cnt[x] > 0 && cnt[x] < f) {
                f = cnt[x];
                ans = x;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func getLeastFrequentDigit(n int) (ans int) {
	cnt := [10]int{}
	for ; n > 0; n /= 10 {
		cnt[n%10]++
	}
	f := 1 << 30
	for x, v := range cnt {
		if v > 0 && v < f {
			f = v
			ans = x
		}
	}
	return
}
```

#### TypeScript

```ts
function getLeastFrequentDigit(n: number): number {
    const cnt: number[] = Array(10).fill(0);
    for (; n; n = (n / 10) | 0) {
        cnt[n % 10]++;
    }
    let [ans, f] = [0, Number.MAX_SAFE_INTEGER];
    for (let x = 0; x < 10; ++x) {
        if (cnt[x] > 0 && cnt[x] < f) {
            f = cnt[x];
            ans = x;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
