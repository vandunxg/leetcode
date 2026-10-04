---
comments: true
difficulty: Easy
tags:
    - Array
    - Hash Table
---

<!-- problem:start -->

# [3450. Maximum Students on a Single Bench 🔒](https://leetcode.com/problems/maximum-students-on-a-single-bench)

[中文文档](/solution/3400-3499/3450.Maximum%20Students%20on%20a%20Single%20Bench/README.md)

## Mô tả

<!-- description:start -->

<p data-pm-slice="1 1 []">Bạn được cho một mảng số nguyên 2 chiều <code>students</code>, trong đó <code>students[i] = [student_id, bench_id]</code> biểu thị rằng học sinh <code>student_id</code> đang ngồi trên băng ghế <code>bench_id</code>.</p>

<p>Hãy trả về số lượng <strong>lớn nhất</strong> học sinh <em>khác nhau</em> ngồi trên một băng ghế bất kỳ. Nếu không có học sinh nào, trả về 0.</p>

<p><strong>Lưu ý</strong>: Một học sinh có thể xuất hiện nhiều lần trên cùng một băng ghế trong dữ liệu đầu vào, nhưng chỉ được tính một lần trên mỗi băng ghế.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">students = [[1,2],[2,2],[3,3],[1,3],[2,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Băng ghế 2 có hai học sinh khác nhau: <code>[1, 2]</code>.</li>
	<li>Băng ghế 3 có ba học sinh khác nhau: <code>[1, 2, 3]</code>.</li>
	<li>Số học sinh khác nhau lớn nhất trên một băng ghế là 3.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">students = [[1,1],[2,1],[3,1],[4,2],[5,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Băng ghế 1 có ba học sinh khác nhau: <code>[1, 2, 3]</code>.</li>
	<li>Băng ghế 2 có hai học sinh khác nhau: <code>[4, 5]</code>.</li>
	<li>Số học sinh khác nhau lớn nhất trên một băng ghế là 3.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">students = [[1,1],[1,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Số học sinh khác nhau lớn nhất trên một băng ghế là 1.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 4:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">students = []</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Vì không có học sinh nào, đầu ra là 0.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= students.length &lt;= 100</code></li>
	<li><code>students[i] = [student_id, bench_id]</code></li>
	<li><code>1 &lt;= student_id &lt;= 100</code></li>
	<li><code>1 &lt;= bench_id &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm

<!-- thinking:start -->

> **Tư duy**
>
> Một học sinh có thể xuất hiện nhiều lần trên cùng một băng ghế; ta cần số lượng học sinh khác nhau lớn nhất. Có nhiều nhất $100$ dòng dữ liệu.
>
> Một set sẽ loại bỏ phần tử trùng lặp trực tiếp hơn so với việc sắp xếp rồi đếm.
>
> Ta dùng hash map ánh xạ mã băng ghế tới một set các mã học sinh, sau đó lấy kích thước set lớn nhất, hoặc trả về $0$ nếu đầu vào rỗng.

<!-- thinking:end -->

Ta dùng một hash table $d$ để lưu các học sinh trên mỗi băng ghế, trong đó khóa là số băng ghế và giá trị là một set chứa các mã học sinh trên băng ghế đó.

Duyệt qua mảng học sinh $\textit{students}$ và lưu mã học sinh cùng số băng ghế vào hash table $d$.

Cuối cùng, ta duyệt qua các giá trị của hash table $d$ và lấy kích thước lớn nhất của các set, chính là số lượng học sinh khác nhau lớn nhất trên một băng ghế.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng học sinh $\textit{students}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxStudentsOnBench(self, students: List[List[int]]) -> int:
        if not students:
            return 0
        d = defaultdict(set)
        for student_id, bench_id in students:
            d[bench_id].add(student_id)
        return max(map(len, d.values()))
```

#### Java

```java
class Solution {
    public int maxStudentsOnBench(int[][] students) {
        Map<Integer, Set<Integer>> d = new HashMap<>();
        for (var e : students) {
            int studentId = e[0], benchId = e[1];
            d.computeIfAbsent(benchId, k -> new HashSet<>()).add(studentId);
        }
        int ans = 0;
        for (var s : d.values()) {
            ans = Math.max(ans, s.size());
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxStudentsOnBench(vector<vector<int>>& students) {
        unordered_map<int, unordered_set<int>> d;
        for (const auto& e : students) {
            int studentId = e[0], benchId = e[1];
            d[benchId].insert(studentId);
        }
        int ans = 0;
        for (const auto& s : d) {
            ans = max(ans, (int) s.second.size());
        }
        return ans;
    }
};
```

#### Go

```go
func maxStudentsOnBench(students [][]int) (ans int) {
	d := make(map[int]map[int]struct{})
	for _, e := range students {
		studentId, benchId := e[0], e[1]
		if _, exists := d[benchId]; !exists {
			d[benchId] = make(map[int]struct{})
		}
		d[benchId][studentId] = struct{}{}
	}
	for _, s := range d {
		ans = max(ans, len(s))
	}
	return
}
```

#### TypeScript

```ts
function maxStudentsOnBench(students: number[][]): number {
    const d: Map<number, Set<number>> = new Map();
    for (const [studentId, benchId] of students) {
        if (!d.has(benchId)) {
            d.set(benchId, new Set());
        }
        d.get(benchId)?.add(studentId);
    }
    let ans = 0;
    for (const s of d.values()) {
        ans = Math.max(ans, s.size);
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::{HashMap, HashSet};

impl Solution {
    pub fn max_students_on_bench(students: Vec<Vec<i32>>) -> i32 {
        let mut d: HashMap<i32, HashSet<i32>> = HashMap::new();
        for e in students {
            let student_id = e[0];
            let bench_id = e[1];
            d.entry(bench_id)
                .or_insert_with(HashSet::new)
                .insert(student_id);
        }
        let mut ans = 0;
        for s in d.values() {
            ans = ans.max(s.len() as i32);
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
