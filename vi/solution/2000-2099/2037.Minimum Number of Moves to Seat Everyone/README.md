---
comments: true
difficulty: Easy
rating: 1356
source: Biweekly Contest 63 Q1
tags:
    - Greedy
    - Array
    - Counting Sort
    - Sorting
---

<!-- problem:start -->

# [2037. Minimum Number of Moves to Seat Everyone](https://leetcode.com/problems/minimum-number-of-moves-to-seat-everyone)

[中文文档](/solution/2000-2099/2037.Minimum%20Number%20of%20Moves%20to%20Seat%20Everyone/README.md)

## Mô tả

<!-- description:start -->

<p>Trong một căn phòng có <code>n</code> <strong>ghế trống</strong> và <code>n</code> học sinh đang <strong>đứng</strong>. Bạn được cho một mảng <code>seats</code> có độ dài <code>n</code>, trong đó <code>seats[i]</code> là vị trí của ghế thứ <code>i<sup>th</sup></code>. Bạn cũng được cho mảng <code>students</code> có độ dài <code>n</code>, trong đó <code>students[j]</code> là vị trí của học sinh thứ <code>j<sup>th</sup></code>.</p>

<p>Bạn có thể thực hiện thao tác sau nhiều lần tùy ý:</p>

<ul>
	<li>Tăng hoặc giảm vị trí của học sinh thứ <code>i<sup>th</sup></code> đi <code>1</code> (tức là di chuyển học sinh thứ <code>i<sup>th</sup></code> từ vị trí&nbsp;<code>x</code>&nbsp;đến <code>x + 1</code> hoặc <code>x - 1</code>)</li>
</ul>

<p>Hãy trả về <em><strong>số thao tác ít nhất</strong> cần thực hiện để đưa mỗi học sinh đến một chiếc ghế</em><em> sao cho không có hai học sinh nào ngồi cùng một ghế.</em></p>

<p>Lưu ý rằng ban đầu có thể có <strong>nhiều ghế</strong> hoặc học sinh ở <strong>cùng một</strong> vị trí.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> seats = [3,1,5], students = [2,7,4]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Các học sinh được di chuyển như sau:
- Học sinh thứ nhất được di chuyển từ vị trí 2 đến vị trí 1 bằng 1 thao tác.
- Học sinh thứ hai được di chuyển từ vị trí 7 đến vị trí 5 bằng 2 thao tác.
- Học sinh thứ ba được di chuyển từ vị trí 4 đến vị trí 3 bằng 1 thao tác.
Tổng cộng đã sử dụng 1 + 2 + 1 = 4 thao tác.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> seats = [4,1,5,9], students = [1,3,2,6]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Các học sinh được di chuyển như sau:
- Học sinh thứ nhất không cần di chuyển.
- Học sinh thứ hai được di chuyển từ vị trí 3 đến vị trí 4 bằng 1 thao tác.
- Học sinh thứ ba được di chuyển từ vị trí 2 đến vị trí 5 bằng 3 thao tác.
- Học sinh thứ tư được di chuyển từ vị trí 6 đến vị trí 9 bằng 3 thao tác.
Tổng cộng đã sử dụng 0 + 1 + 3 + 3 = 7 thao tác.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> seats = [2,2,6,6], students = [1,3,2,6]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Lưu ý rằng có hai ghế ở vị trí 2 và hai ghế ở vị trí 6.
Các học sinh được di chuyển như sau:
- Học sinh thứ nhất được di chuyển từ vị trí 1 đến vị trí 2 bằng 1 thao tác.
- Học sinh thứ hai được di chuyển từ vị trí 3 đến vị trí 6 bằng 3 thao tác.
- Học sinh thứ ba không cần di chuyển.
- Học sinh thứ tư không cần di chuyển.
Tổng cộng đã sử dụng 1 + 3 + 0 + 0 = 4 thao tác.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == seats.length == students.length</code></li>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>1 &lt;= seats[i], students[j] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Ghế và học sinh tạo thành một phép ghép cặp; chi phí là khoảng cách $L_1$. Các cặp giao nhau không thể cho kết quả tốt hơn, vì vậy sau khi sắp xếp, ghế thứ $i$ được ghép với học sinh thứ $i$.
>
> Với $n \le 100$, chỉ cần sắp xếp hai mảng rồi tính tổng các hiệu tuyệt đối.

<!-- thinking:end -->

Sắp xếp cả hai mảng, sau đó duyệt qua chúng, tính khoảng cách giữa vị trí của từng ghế và vị trí của học sinh tương ứng, rồi cộng tất cả khoảng cách để thu được đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(\log n)$. Trong đó, $n$ là độ dài của các mảng `seats` và `students`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minMovesToSeat(self, seats: List[int], students: List[int]) -> int:
        seats.sort()
        students.sort()
        return sum(abs(a - b) for a, b in zip(seats, students))
```

#### Java

```java
class Solution {
    public int minMovesToSeat(int[] seats, int[] students) {
        Arrays.sort(seats);
        Arrays.sort(students);
        int ans = 0;
        for (int i = 0; i < seats.length; ++i) {
            ans += Math.abs(seats[i] - students[i]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minMovesToSeat(vector<int>& seats, vector<int>& students) {
        sort(seats.begin(), seats.end());
        sort(students.begin(), students.end());
        int ans = 0;
        for (int i = 0; i < seats.size(); ++i) {
            ans += abs(seats[i] - students[i]);
        }
        return ans;
    }
};
```

#### Go

```go
func minMovesToSeat(seats []int, students []int) (ans int) {
	sort.Ints(seats)
	sort.Ints(students)
	for i, a := range seats {
		b := students[i]
		ans += abs(a - b)
	}
	return
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function minMovesToSeat(seats: number[], students: number[]): number {
    seats.sort((a, b) => a - b);
    students.sort((a, b) => a - b);
    return seats.reduce((acc, seat, i) => acc + Math.abs(seat - students[i]), 0);
}
```

#### Rust

```rust
impl Solution {
    pub fn min_moves_to_seat(mut seats: Vec<i32>, mut students: Vec<i32>) -> i32 {
        seats.sort();
        students.sort();
        let n = seats.len();
        let mut ans = 0;
        for i in 0..n {
            ans += (seats[i] - students[i]).abs();
        }
        ans
    }
}
```

#### C

```c
int cmp(const void* a, const void* b) {
    return *(int*) a - *(int*) b;
}

int minMovesToSeat(int* seats, int seatsSize, int* students, int studentsSize) {
    qsort(seats, seatsSize, sizeof(int), cmp);
    qsort(students, studentsSize, sizeof(int), cmp);
    int ans = 0;
    for (int i = 0; i < seatsSize; i++) {
        ans += abs(seats[i] - students[i]);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
