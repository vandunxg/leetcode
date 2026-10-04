---
comments: true
difficulty: Easy
rating: 1149
source: Weekly Contest 402 Q1
tags:
    - Array
    - Hash Table
    - Counting
---

<!-- problem:start -->

# [3184. Count Pairs That Form a Complete Day I](https://leetcode.com/problems/count-pairs-that-form-a-complete-day-i)

[中文文档](/solution/3100-3199/3184.Count%20Pairs%20That%20Form%20a%20Complete%20Day%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>hours</code> biểu diễn các khoảng thời gian tính theo <strong>giờ</strong>, hãy trả về một số nguyên biểu thị số cặp <code>i</code>, <code>j</code> sao cho <code>i &lt; j</code> và <code>hours[i] + hours[j]</code> tạo thành một <strong>ngày trọn vẹn</strong>.</p>

<p>Một <strong>ngày trọn vẹn</strong> được định nghĩa là khoảng thời gian là một <strong>bội số</strong> <strong>chính xác</strong> của 24 giờ.</p>

<p>Ví dụ, 1 ngày là 24 giờ, 2 ngày là 48 giờ, 3 ngày là 72 giờ, v.v.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">hours = [12,12,30,24,24]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các cặp chỉ số tạo thành một ngày trọn vẹn là <code>(0, 1)</code> và <code>(3, 4)</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">hours = [72,48,24,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các cặp chỉ số tạo thành một ngày trọn vẹn là <code>(0, 1)</code>, <code>(0, 2)</code> và <code>(1, 2)</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= hours.length &lt;= 100</code></li>
	<li><code>1 &lt;= hours[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Một cặp có tổng là bội số của $24$. Với phần I, vòng lặp kép là đủ, nhưng các phần dư chỉ nằm trong $24$ nhóm.
>
> Khi $x$ xuất hiện, các cặp hợp lệ đến từ phần dư đã thấy $(24-x\bmod 24)\bmod 24$.
>
> Trước tiên cộng số lượng đó, sau đó tăng phần dư $x\bmod 24$, để chỉ lấy các cặp có $i<j$.

<!-- thinking:end -->

Ta có thể dùng một hash table hoặc một mảng $\textit{cnt}$ có độ dài $24$ để ghi nhận số lần xuất hiện của mỗi giá trị giờ modulo $24$.

Duyệt qua mảng $\textit{hours}$. Với mỗi giờ $x$, ta tìm số mà khi cộng với $x$ sẽ cho kết quả là một bội số của $24$. Sau khi lấy modulo $24$, số này là $(24 - x \bmod 24) \bmod 24$. Sau đó, ta cộng số lần xuất hiện của số này trong hash table hoặc mảng vào đáp án. Cuối cùng, ta tăng số lần xuất hiện của $x$ modulo $24$ lên một.

Sau khi duyệt qua mảng $\textit{hours}$, ta thu được số cặp chỉ số thỏa mãn yêu cầu của bài toán.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{hours}$. Độ phức tạp không gian là $O(C)$, trong đó $C=24$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countCompleteDayPairs(self, hours: List[int]) -> int:
        cnt = Counter()
        ans = 0
        for x in hours:
            ans += cnt[(24 - (x % 24)) % 24]
            cnt[x % 24] += 1
        return ans
```

#### Java

```java
class Solution {
    public int countCompleteDayPairs(int[] hours) {
        int[] cnt = new int[24];
        int ans = 0;
        for (int x : hours) {
            ans += cnt[(24 - x % 24) % 24];
            ++cnt[x % 24];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countCompleteDayPairs(vector<int>& hours) {
        int cnt[24]{};
        int ans = 0;
        for (int x : hours) {
            ans += cnt[(24 - x % 24) % 24];
            ++cnt[x % 24];
        }
        return ans;
    }
};
```

#### Go

```go
func countCompleteDayPairs(hours []int) (ans int) {
	cnt := [24]int{}
	for _, x := range hours {
		ans += cnt[(24-x%24)%24]
		cnt[x%24]++
	}
	return
}
```

#### TypeScript

```ts
function countCompleteDayPairs(hours: number[]): number {
    const cnt: number[] = Array(24).fill(0);
    let ans: number = 0;
    for (const x of hours) {
        ans += cnt[(24 - (x % 24)) % 24];
        ++cnt[x % 24];
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
