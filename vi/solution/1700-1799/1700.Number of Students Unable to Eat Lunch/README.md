---
comments: true
difficulty: Easy
rating: 1404
source: Biweekly Contest 42 Q1
tags:
    - Stack
    - Queue
    - Array
    - Simulation
---

<!-- problem:start -->

# [1700. Number of Students Unable to Eat Lunch](https://leetcode.com/problems/number-of-students-unable-to-eat-lunch)

[中文文档](/solution/1700-1799/1700.Number%20of%20Students%20Unable%20to%20Eat%20Lunch/README.md)

## Mô tả

<!-- description:start -->

<p>Căng tin trường phục vụ sandwich hình tròn và hình vuông vào giờ nghỉ trưa, lần lượt được biểu diễn bằng số <code>0</code> và <code>1</code>. Tất cả học sinh xếp thành một hàng. Mỗi học sinh thích sandwich hình vuông hoặc hình tròn.</p>

<p>Số sandwich trong căng tin bằng số học sinh. Sandwich được xếp vào một <strong>stack</strong>. Ở mỗi bước:</p>

<ul>
	<li>Nếu học sinh ở đầu hàng <strong>thích</strong> loại sandwich trên cùng của stack, học sinh đó sẽ <strong>lấy nó</strong> và rời khỏi hàng.</li>
	<li>Nếu không, học sinh đó sẽ <strong>bỏ qua</strong> sandwich và đi xuống cuối hàng.</li>
</ul>

<p>Quá trình tiếp tục cho đến khi không học sinh nào trong hàng muốn lấy sandwich trên cùng, và những học sinh đó không thể ăn.</p>

<p>Cho hai mảng số nguyên <code>students</code> và <code>sandwiches</code>, trong đó <code>sandwiches[i]</code> là loại của sandwich thứ <code>i<sup>​​​​​​th</sup></code> trong stack (<code>i = 0</code> là sandwich trên cùng), còn <code>students[j]</code> là sở thích của học sinh thứ <code>j<sup>​​​​​​th</sup></code> trong hàng ban đầu (<code>j = 0</code> là học sinh đầu hàng). Hãy trả về <em>số học sinh không thể ăn</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> students = [1,1,0,0], sandwiches = [0,1,0,1]
<strong>Đầu ra:</strong> 0<strong>
Giải thích:</strong>
- Học sinh đầu hàng bỏ qua sandwich trên cùng và quay xuống cuối hàng, khi đó students = [1,0,0,1].
- Học sinh đầu hàng bỏ qua sandwich trên cùng và quay xuống cuối hàng, khi đó students = [0,0,1,1].
- Học sinh đầu hàng lấy sandwich trên cùng rồi rời hàng, khi đó students = [0,1,1] và sandwiches = [1,0,1].
- Học sinh đầu hàng bỏ qua sandwich trên cùng và quay xuống cuối hàng, khi đó students = [1,1,0].
- Học sinh đầu hàng lấy sandwich trên cùng rồi rời hàng, khi đó students = [1,0] và sandwiches = [0,1].
- Học sinh đầu hàng bỏ qua sandwich trên cùng và quay xuống cuối hàng, khi đó students = [0,1].
- Học sinh đầu hàng lấy sandwich trên cùng rồi rời hàng, khi đó students = [1] và sandwiches = [1].
- Học sinh đầu hàng lấy sandwich trên cùng rồi rời hàng, khi đó students = [] và sandwiches = [].
Vì vậy, tất cả học sinh đều có thể ăn.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> students = [1,1,1,0,0,1], sandwiches = [1,0,0,0,1,1]
<strong>Đầu ra:</strong> 3
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= students.length, sandwiches.length &lt;= 100</code></li>
	<li><code>students.length == sandwiches.length</code></li>
	<li><code>sandwiches[i]</code> là <code>0</code> hoặc <code>1</code>.</li>
	<li><code>students[i]</code> là <code>0</code> hoặc <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Mô phỏng hàng học sinh bằng cách xoay hàng sẽ so sánh học sinh đầu hàng với sandwich trên cùng ở mỗi lượt. Học sinh từ chối sẽ đi xuống cuối hàng, nên quá trình có thể lặp nhiều lần và phụ thuộc vào thứ tự.
>
> Thứ tự sandwich là cố định, còn học sinh có thể đổi vị trí. Khi không còn học sinh nào muốn loại sandwich trên cùng, mọi sandwich phía sau cũng bị kẹt. Vì vậy, chỉ cần đếm số học sinh thích mỗi loại rồi trừ dần theo thứ tự sandwich.
>
> Khi $cnt[v]=0$, tất cả học sinh còn lại đều thích loại kia, tức là $cnt[v\oplus 1]$. Duyệt tuyến tính sẽ cho biết số học sinh không thể ăn.

<!-- thinking:end -->

Ta nhận thấy vị trí của học sinh có thể thay đổi, nhưng vị trí của sandwich thì không. Nói cách khác, nếu sandwich phía trước không được lấy thì mọi sandwich phía sau cũng không thể được lấy.

Do đó, trước hết ta dùng bộ đếm $cnt$ để đếm số học sinh thích từng loại sandwich.

Sau đó, ta duyệt các sandwich. Nếu trong $cnt$ không còn học sinh thích sandwich hiện tại, các sandwich phía sau cũng không thể được lấy, và ta trả về số học sinh còn lại.

Nếu duyệt hết, nghĩa là tất cả học sinh đều có sandwich để ăn, nên ta trả về $0$.

Độ phức tạp thời gian là $O(n)$, với $n$ là số sandwich. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countStudents(self, students: List[int], sandwiches: List[int]) -> int:
        cnt = Counter(students)
        for v in sandwiches:
            if cnt[v] == 0:
                return cnt[v ^ 1]
            cnt[v] -= 1
        return 0
```

#### Java

```java
class Solution {
    public int countStudents(int[] students, int[] sandwiches) {
        int[] cnt = new int[2];
        for (int v : students) {
            ++cnt[v];
        }
        for (int v : sandwiches) {
            if (cnt[v]-- == 0) {
                return cnt[v ^ 1];
            }
        }
        return 0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countStudents(vector<int>& students, vector<int>& sandwiches) {
        int cnt[2] = {0};
        for (int& v : students) ++cnt[v];
        for (int& v : sandwiches) {
            if (cnt[v]-- == 0) {
                return cnt[v ^ 1];
            }
        }
        return 0;
    }
};
```

#### Go

```go
func countStudents(students []int, sandwiches []int) int {
	cnt := [2]int{}
	for _, v := range students {
		cnt[v]++
	}
	for _, v := range sandwiches {
		if cnt[v] == 0 {
			return cnt[v^1]
		}
		cnt[v]--
	}
	return 0
}
```

#### TypeScript

```ts
function countStudents(students: number[], sandwiches: number[]): number {
    const count = [0, 0];
    for (const v of students) {
        count[v]++;
    }
    for (const v of sandwiches) {
        if (count[v] === 0) {
            return count[v ^ 1];
        }
        count[v]--;
    }
    return 0;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_students(students: Vec<i32>, sandwiches: Vec<i32>) -> i32 {
        let mut count = [0, 0];
        for &v in students.iter() {
            count[v as usize] += 1;
        }
        for &v in sandwiches.iter() {
            let v = v as usize;
            if count[v as usize] == 0 {
                return count[v ^ 1];
            }
            count[v] -= 1;
        }
        0
    }
}
```

#### C

```c
int countStudents(int* students, int studentsSize, int* sandwiches, int sandwichesSize) {
    int count[2] = {0};
    for (int i = 0; i < studentsSize; i++) {
        count[students[i]]++;
    }
    for (int i = 0; i < sandwichesSize; i++) {
        int j = sandwiches[i];
        if (count[j] == 0) {
            return count[j ^ 1];
        }
        count[j]--;
    }
    return 0;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
