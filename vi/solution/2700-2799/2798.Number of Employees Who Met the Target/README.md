---
comments: true
difficulty: Easy
rating: 1142
source: Weekly Contest 356 Q1
tags:
    - Array
---

<!-- problem:start -->

# [2798. Number of Employees Who Met the Target](https://leetcode.com/problems/number-of-employees-who-met-the-target)

[中文文档](/solution/2700-2799/2798.Number%20of%20Employees%20Who%20Met%20the%20Target/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> nhân viên trong một công ty, được đánh số từ <code>0</code> đến <code>n - 1</code>. Mỗi nhân viên <code>i</code> đã làm việc <code>hours[i]</code> giờ trong công ty.</p>

<p>Công ty yêu cầu mỗi nhân viên phải làm việc <strong>ít nhất</strong> <code>target</code> giờ.</p>

<p>Bạn được cho một mảng số nguyên không âm <strong>0-indexed</strong> <code>hours</code> có độ dài <code>n</code> và một số nguyên không âm <code>target</code>.</p>

<p>Hãy trả về <em>số nhân viên đã làm việc ít nhất</em> <code>target</code> <em>giờ</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> hours = [0,1,2,3,4], target = 2
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Công ty muốn mỗi nhân viên làm việc ít nhất 2 giờ.
- Nhân viên 0 đã làm việc 0 giờ và không đạt target.
- Nhân viên 1 đã làm việc 1 giờ và không đạt target.
- Nhân viên 2 đã làm việc 2 giờ và đạt target.
- Nhân viên 3 đã làm việc 3 giờ và đạt target.
- Nhân viên 4 đã làm việc 4 giờ và đạt target.
Có 3 nhân viên đạt target.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> hours = [5,1,4,2,2], target = 6
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Công ty muốn mỗi nhân viên làm việc ít nhất 6 giờ.
Có 0 nhân viên đạt target.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == hours.length &lt;= 50</code></li>
	<li><code>0 &lt;=&nbsp;hours[i], target &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt và đếm

<!-- thinking:start -->

> **Tư duy**
>
> Đếm số nhân viên có số giờ làm việc ít nhất là $target$. Chỉ cần duyệt tuyến tính một lần; không cần sắp xếp hay cấu trúc dữ liệu bổ sung.
>
> Tính tổng của điều kiện $x\ge target$ trên $hours$.

<!-- thinking:end -->

Ta có thể duyệt qua mảng $hours$. Với mỗi nhân viên, nếu số giờ làm việc $x$ lớn hơn hoặc bằng $target$, ta tăng bộ đếm $ans$ lên một.

Sau khi duyệt xong, ta trả về kết quả.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $hours$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfEmployeesWhoMetTarget(self, hours: List[int], target: int) -> int:
        return sum(x >= target for x in hours)
```

#### Java

```java
class Solution {
    public int numberOfEmployeesWhoMetTarget(int[] hours, int target) {
        int ans = 0;
        for (int x : hours) {
            if (x >= target) {
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
    int numberOfEmployeesWhoMetTarget(vector<int>& hours, int target) {
        return count_if(hours.begin(), hours.end(), [target](int h) { return h >= target; });
    }
};
```

#### Go

```go
func numberOfEmployeesWhoMetTarget(hours []int, target int) (ans int) {
	for _, x := range hours {
		if x >= target {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function numberOfEmployeesWhoMetTarget(hours: number[], target: number): number {
    return hours.filter(x => x >= target).length;
}
```

#### Rust

```rust
impl Solution {
    pub fn number_of_employees_who_met_target(hours: Vec<i32>, target: i32) -> i32 {
        hours.iter().filter(|&x| *x >= target).count() as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
