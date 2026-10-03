---
comments: true
difficulty: Medium
rating: 1803
source: Weekly Contest 252 Q2
tags:
    - Greedy
    - Array
---

<!-- problem:start -->

# [1953. Maximum Number of Weeks for Which You Can Work](https://leetcode.com/problems/maximum-number-of-weeks-for-which-you-can-work)

[中文文档](/solution/1900-1999/1953.Maximum%20Number%20of%20Weeks%20for%20Which%20You%20Can%20Work/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> dự án được đánh số từ <code>0</code> đến <code>n - 1</code>. Bạn được cho một mảng số nguyên <code>milestones</code>, trong đó mỗi <code>milestones[i]</code> biểu thị số milestone của dự án <code>i<sup>th</sup></code>.</p>

<p>Bạn có thể làm việc trên các dự án theo hai quy tắc sau:</p>

<ul>
	<li>Mỗi tuần, bạn sẽ hoàn thành <strong>chính xác một</strong> milestone của <strong>một</strong> dự án. Bạn <strong>phải</strong> làm việc mỗi tuần.</li>
	<li>Bạn <strong>không thể</strong> làm hai milestone của cùng một dự án trong hai <strong>tuần liên tiếp</strong>.</li>
</ul>

<p>Khi đã hoàn thành tất cả milestone của mọi dự án, hoặc nếu những milestone duy nhất bạn có thể làm sẽ khiến bạn vi phạm các quy tắc trên, bạn sẽ <strong>dừng làm việc</strong>. Lưu ý rằng do những ràng buộc này, bạn có thể không hoàn thành được tất cả milestone của mọi dự án.</p>

<p>Trả về <em><strong>số tuần tối đa</strong> mà bạn có thể làm việc trên các dự án mà không vi phạm những quy tắc đã nêu</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> milestones = [1,2,3]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Một kịch bản có thể xảy ra là:
- Trong tuần thứ 1<sup>st</sup>, bạn sẽ làm một milestone của dự án 0.
- Trong tuần thứ 2<sup>nd</sup>, bạn sẽ làm một milestone của dự án 2.
- Trong tuần thứ 3<sup>rd</sup>, bạn sẽ làm một milestone của dự án 1.
- Trong tuần thứ 4<sup>th</sup>, bạn sẽ làm một milestone của dự án 2.
- Trong tuần thứ 5<sup>th</sup>, bạn sẽ làm một milestone của dự án 1.
- Trong tuần thứ 6<sup>th</sup>, bạn sẽ làm một milestone của dự án 2.
Tổng số tuần là 6.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> milestones = [5,2,1]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Một kịch bản có thể xảy ra là:
- Trong tuần thứ 1<sup>st</sup>, bạn sẽ làm một milestone của dự án 0.
- Trong tuần thứ 2<sup>nd</sup>, bạn sẽ làm một milestone của dự án 1.
- Trong tuần thứ 3<sup>rd</sup>, bạn sẽ làm một milestone của dự án 0.
- Trong tuần thứ 4<sup>th</sup>, bạn sẽ làm một milestone của dự án 1.
- Trong tuần thứ 5<sup>th</sup>, bạn sẽ làm một milestone của dự án 0.
- Trong tuần thứ 6<sup>th</sup>, bạn sẽ làm một milestone của dự án 2.
- Trong tuần thứ 7<sup>th</sup>, bạn sẽ làm một milestone của dự án 0.
Tổng số tuần là 7.
Lưu ý rằng bạn không thể làm milestone cuối cùng của dự án 0 vào tuần thứ 8<sup>th</sup> vì điều đó sẽ vi phạm các quy tắc.
Do đó, một milestone của dự án 0 sẽ vẫn chưa được hoàn thành.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == milestones.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= milestones[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Các tuần liên tiếp không thể cùng chọn một dự án. Nếu một dự án có số milestone lớn hơn tổng số milestone của các dự án còn lại cộng thêm một, phần dư đó không thể được sắp lịch.
>
> Gọi $s$ là tổng, $mx$ là giá trị lớn nhất và $rest=s-mx$. Nếu $mx>rest+1$, giới hạn là $2\cdot rest+1$; ngược lại, tất cả $s$ tuần đều khả thi.
>
> Chỉ cần một lượt duyệt để tìm giá trị lớn nhất và tổng, không cần mô phỏng từng tuần.

<!-- thinking:end -->

Ta xét những trường hợp không thể hoàn thành tất cả milestone. Nếu có một dự án $i$ có số milestone lớn hơn tổng số milestone của tất cả dự án khác cộng thêm $1$, thì ta không thể hoàn thành tất cả milestone. Ngược lại, ta chắc chắn có thể hoàn thành tất cả milestone bằng cách xen kẽ các dự án khác nhau.

Gọi tổng số milestone của tất cả dự án là $s$, số milestone lớn nhất là $mx$, khi đó tổng số milestone của tất cả dự án khác là $rest = s - mx$.

Nếu $mx > rest + 1$, ta không thể hoàn thành tất cả milestone và nhiều nhất có thể hoàn thành $rest \times 2 + 1$ milestone. Ngược lại, ta có thể hoàn thành tất cả milestone, với số lượng là $s$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là số dự án. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfWeeks(self, milestones: List[int]) -> int:
        mx, s = max(milestones), sum(milestones)
        rest = s - mx
        return rest * 2 + 1 if mx > rest + 1 else s
```

#### Java

```java
class Solution {
    public long numberOfWeeks(int[] milestones) {
        int mx = 0;
        long s = 0;
        for (int e : milestones) {
            s += e;
            mx = Math.max(mx, e);
        }
        long rest = s - mx;
        return mx > rest + 1 ? rest * 2 + 1 : s;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long numberOfWeeks(vector<int>& milestones) {
        int mx = *max_element(milestones.begin(), milestones.end());
        long long s = accumulate(milestones.begin(), milestones.end(), 0LL);
        long long rest = s - mx;
        return mx > rest + 1 ? rest * 2 + 1 : s;
    }
};
```

#### Go

```go
func numberOfWeeks(milestones []int) int64 {
	mx := slices.Max(milestones)
	s := 0
	for _, x := range milestones {
		s += x
	}
	rest := s - mx
	if mx > rest+1 {
		return int64(rest*2 + 1)
	}
	return int64(s)
}
```

#### TypeScript

```ts
function numberOfWeeks(milestones: number[]): number {
    const mx = Math.max(...milestones);
    const s = milestones.reduce((a, b) => a + b, 0);
    const rest = s - mx;
    return mx > rest + 1 ? rest * 2 + 1 : s;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
