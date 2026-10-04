---
comments: true
difficulty: Medium
rating: 1385
source: Weekly Contest 402 Q2
tags:
    - Array
    - Hash Table
    - Counting
---

<!-- problem:start -->

# [3185. Count Pairs That Form a Complete Day II](https://leetcode.com/problems/count-pairs-that-form-a-complete-day-ii)

[中文文档](/solution/3100-3199/3185.Count%20Pairs%20That%20Form%20a%20Complete%20Day%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>hours</code> biểu diễn thời lượng theo <strong>giờ</strong>, hãy trả về một số nguyên biểu thị số cặp <code>i</code>, <code>j</code> sao cho <code>i &lt; j</code> và <code>hours[i] + hours[j]</code> tạo thành một <strong>ngày trọn vẹn</strong>.</p>

<p>Một <strong>ngày trọn vẹn</strong> được định nghĩa là một khoảng thời gian là <strong>bội số</strong> <strong>nguyên</strong> của 24 giờ.</p>

<p>Ví dụ, 1 ngày là 24 giờ, 2 ngày là 48 giờ, 3 ngày là 72 giờ, v.v.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">hours = [12,12,30,24,24]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong> Các cặp chỉ số tạo thành một ngày trọn vẹn là <code>(0, 1)</code> và <code>(3, 4)</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">hours = [72,48,24,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong> Các cặp chỉ số tạo thành một ngày trọn vẹn là <code>(0, 1)</code>, <code>(0, 2)</code> và <code>(1, 2)</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= hours.length &lt;= 5 * 10<sup>5</sup></code></li>
	<li><code>1 &lt;= hours[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Điều kiện giống phần I, nhưng $n$ có thể đạt $10^5$, nên vòng lặp kép sẽ không phù hợp. Chỉ cần đếm các phần dư đã gặp là đủ.
>
> Phần dư bù không thay đổi; ta có thể dùng map hoặc mảng có độ dài $24$.
>
> Duyệt từ trái sang phải, cộng thêm $cnt[(24-x\bmod 24)\bmod 24]$, sau đó tăng $cnt[x\bmod 24]$.

<!-- thinking:end -->

Ta có thể dùng một bảng băm hoặc một mảng $\textit{cnt}$ có độ dài $24$ để ghi nhận số lần xuất hiện của mỗi số giờ modulo $24$.

Duyệt qua mảng $\textit{hours}$. Với mỗi số giờ $x$, ta tìm số mà khi cộng với $x$ sẽ cho kết quả là một bội số của $24$; sau khi lấy modulo $24$, số này là $(24 - x \bmod 24) \bmod 24$. Sau đó, ta cộng số lần xuất hiện của số này trong bảng băm hoặc mảng vào đáp án. Tiếp theo, ta tăng số lần xuất hiện của $x$ modulo $24$ lên một.

Sau khi duyệt qua mảng $\textit{hours}$, ta thu được số cặp chỉ số thỏa mãn yêu cầu của đề bài.

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
    public long countCompleteDayPairs(int[] hours) {
        int[] cnt = new int[24];
        long ans = 0;
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
    long long countCompleteDayPairs(vector<int>& hours) {
        int cnt[24]{};
        long long ans = 0;
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
func countCompleteDayPairs(hours []int) (ans int64) {
	cnt := [24]int{}
	for _, x := range hours {
		ans += int64(cnt[(24-x%24)%24])
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
