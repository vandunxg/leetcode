---
comments: true
difficulty: Medium
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Array
    - Hash Table
---

<!-- problem:start -->

# [690. Employee Importance](https://leetcode.com/problems/employee-importance)

[中文文档](/solution/0600-0699/0690.Employee%20Importance/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có một cấu trúc dữ liệu chứa thông tin nhân viên, gồm ID duy nhất, độ quan trọng và ID của các cấp dưới trực tiếp.</p>

<p>Cho mảng nhân viên <code>employees</code>, trong đó:</p>

<ul>
	<li><code>employees[i].id</code> là ID của nhân viên thứ <code>i<sup>th</sup></code>.</li>
	<li><code>employees[i].importance</code> là độ quan trọng của nhân viên thứ <code>i<sup>th</sup></code>.</li>
	<li><code>employees[i].subordinates</code> là danh sách ID của các cấp dưới trực tiếp của nhân viên thứ <code>i<sup>th</sup></code>.</li>
</ul>

<p>Cho số nguyên <code>id</code> biểu thị ID của một nhân viên, hãy trả về <em>tổng độ quan trọng của nhân viên đó cùng tất cả cấp dưới trực tiếp và gián tiếp</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0600-0699/0690.Employee%20Importance/images/emp1-tree.jpg" style="width: 400px; height: 258px;" />
<pre>
<strong>Đầu vào:</strong> employees = [[1,5,[2,3]],[2,3,[]],[3,3,[]]], id = 1
<strong>Đầu ra:</strong> 11
<strong>Giải thích:</strong> Nhân viên 1 có độ quan trọng là 5 và có hai cấp dưới trực tiếp: nhân viên 2 và nhân viên 3.
Cả hai đều có độ quan trọng là 3.
Vì vậy, tổng độ quan trọng của nhân viên 1 là 5 + 3 + 3 = 11.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0600-0699/0690.Employee%20Importance/images/emp2-tree.jpg" style="width: 362px; height: 361px;" />
<pre>
<strong>Đầu vào:</strong> employees = [[1,2,[5]],[5,-3,[]]], id = 5
<strong>Đầu ra:</strong> -3
<strong>Giải thích:</strong> Nhân viên 5 có độ quan trọng là -3 và không có cấp dưới trực tiếp.
Vì vậy, tổng độ quan trọng của nhân viên 5 là -3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= employees.length &lt;= 2000</code></li>
	<li><code>1 &lt;= employees[i].id &lt;= 2000</code></li>
	<li>Tất cả <code>employees[i].id</code> đều <strong>khác nhau</strong>.</li>
	<li><code>-100 &lt;= employees[i].importance &lt;= 100</code></li>
	<li>Mỗi nhân viên có tối đa một cấp trên trực tiếp và có thể có nhiều cấp dưới.</li>
	<li>Các ID trong <code>employees[i].subordinates</code> đều hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + DFS

<!-- thinking:start -->

> **Tư duy**
>
> Tổng độ quan trọng gồm nhân viên đó và tất cả cấp dưới. Tìm ID trong danh sách ban đầu sẽ tốn thời gian tuyến tính ở mỗi bước.
>
> Ánh xạ ID tới object nhân viên, rồi DFS từ ID đã cho: cộng `importance` với kết quả đệ quy trên các cấp dưới.

<!-- thinking:end -->

Ta dùng hash table $d$ để lưu thông tin nhân viên, với key là ID và value là object nhân viên. Sau đó, bắt đầu depth-first search từ ID được cho. Mỗi khi duyệt tới một nhân viên, ta cộng độ quan trọng của người đó vào kết quả, rồi đệ quy duyệt tất cả cấp dưới và cộng độ quan trọng của họ vào kết quả.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số nhân viên.

<!-- tabs:start -->

#### Python3

```python
"""
# Definition for Employee.
class Employee:
    def __init__(self, id: int, importance: int, subordinates: List[int]):
        self.id = id
        self.importance = importance
        self.subordinates = subordinates
"""


class Solution:
    def getImportance(self, employees: List["Employee"], id: int) -> int:
        def dfs(i: int) -> int:
            return d[i].importance + sum(dfs(j) for j in d[i].subordinates)

        d = {e.id: e for e in employees}
        return dfs(id)
```

#### Java

```java
/*
// Definition for Employee.
class Employee {
    public int id;
    public int importance;
    public List<Integer> subordinates;
};
*/

class Solution {
    private final Map<Integer, Employee> d = new HashMap<>();

    public int getImportance(List<Employee> employees, int id) {
        for (var e : employees) {
            d.put(e.id, e);
        }
        return dfs(id);
    }

    private int dfs(int i) {
        Employee e = d.get(i);
        int s = e.importance;
        for (int j : e.subordinates) {
            s += dfs(j);
        }
        return s;
    }
}
```

#### C++

```cpp
/*
// Definition for Employee.
class Employee {
public:
    int id;
    int importance;
    vector<int> subordinates;
};
*/

class Solution {
public:
    int getImportance(vector<Employee*> employees, int id) {
        unordered_map<int, Employee*> d;
        for (auto& e : employees) {
            d[e->id] = e;
        }
        function<int(int)> dfs = [&](int i) -> int {
            int s = d[i]->importance;
            for (int j : d[i]->subordinates) {
                s += dfs(j);
            }
            return s;
        };
        return dfs(id);
    }
};
```

#### Go

```go
/**
 * Definition for Employee.
 * type Employee struct {
 *     Id int
 *     Importance int
 *     Subordinates []int
 * }
 */

func getImportance(employees []*Employee, id int) int {
	d := map[int]*Employee{}
	for _, e := range employees {
		d[e.Id] = e
	}
	var dfs func(int) int
	dfs = func(i int) int {
		s := d[i].Importance
		for _, j := range d[i].Subordinates {
			s += dfs(j)
		}
		return s
	}
	return dfs(id)
}
```

#### TypeScript

```ts
/**
 * Definition for Employee.
 * class Employee {
 *     id: number
 *     importance: number
 *     subordinates: number[]
 *     constructor(id: number, importance: number, subordinates: number[]) {
 *         this.id = (id === undefined) ? 0 : id;
 *         this.importance = (importance === undefined) ? 0 : importance;
 *         this.subordinates = (subordinates === undefined) ? [] : subordinates;
 *     }
 * }
 */

function getImportance(employees: Employee[], id: number): number {
    const d = new Map<number, Employee>();
    for (const e of employees) {
        d.set(e.id, e);
    }
    const dfs = (i: number): number => {
        let s = d.get(i)!.importance;
        for (const j of d.get(i)!.subordinates) {
            s += dfs(j);
        }
        return s;
    };
    return dfs(id);
}
```

#### JavaScript

```js
/**
 * Definition for Employee.
 * function Employee(id, importance, subordinates) {
 *     this.id = id;
 *     this.importance = importance;
 *     this.subordinates = subordinates;
 * }
 */

/**
 * @param {Employee[]} employees
 * @param {number} id
 * @return {number}
 */
var GetImportance = function (employees, id) {
    const d = new Map();
    for (const e of employees) {
        d.set(e.id, e);
    }
    const dfs = i => {
        let s = d.get(i).importance;
        for (const j of d.get(i).subordinates) {
            s += dfs(j);
        }
        return s;
    };
    return dfs(id);
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
