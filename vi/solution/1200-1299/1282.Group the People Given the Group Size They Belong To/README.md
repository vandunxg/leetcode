---
comments: true
difficulty: Medium
rating: 1267
source: Weekly Contest 166 Q2
tags:
    - Greedy
    - Array
    - Hash Table
---

<!-- problem:start -->

# [1282. Group the People Given the Group Size They Belong To](https://leetcode.com/problems/group-the-people-given-the-group-size-they-belong-to)

[中文文档](/solution/1200-1299/1282.Group%20the%20People%20Given%20the%20Group%20Size%20They%20Belong%20To/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> người được chia thành một số nhóm chưa biết. Mỗi người được gán một <strong>ID duy nhất</strong> từ <code>0</code> đến <code>n - 1</code>.</p>

<p>Bạn được cho mảng số nguyên <code>groupSizes</code>, trong đó <code>groupSizes[i]</code> là kích thước nhóm mà người <code>i</code> thuộc về. Ví dụ, nếu <code>groupSizes[1] = 3</code>, thì người <code>1</code> phải thuộc một nhóm có kích thước <code>3</code>.</p>

<p>Hãy trả về <em>danh sách các nhóm sao cho mỗi người <code>i</code> thuộc một nhóm có kích thước <code>groupSizes[i]</code></em>.</p>

<p>Mỗi người phải xuất hiện trong <strong>đúng một nhóm</strong>, và không ai bị bỏ ngoài nhóm. Nếu có nhiều đáp án, <strong>hãy trả về bất kỳ đáp án nào</strong>. Đề bài <strong>đảm bảo</strong> luôn có <strong>ít nhất một</strong> cách chia hợp lệ cho dữ liệu đầu vào.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> groupSizes = [3,3,3,3,3,1,3]
<strong>Đầu ra:</strong> [[5],[0,1,2],[3,4,6]]
<b>Giải thích:</b> 
Nhóm thứ nhất là [5]. Nhóm có kích thước 1 và groupSizes[5] = 1.
Nhóm thứ hai là [0,1,2]. Nhóm có kích thước 3 và groupSizes[0] = groupSizes[1] = groupSizes[2] = 3.
Nhóm thứ ba là [3,4,6]. Nhóm có kích thước 3 và groupSizes[3] = groupSizes[4] = groupSizes[6] = 3.
Một số cách chia khác là [[2,1,6],[5],[0,4,3]] và [[5],[0,6,2],[4,3,1]].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> groupSizes = [2,1,3,3,3,2]
<strong>Đầu ra:</strong> [[1],[0,5],[2,3,4]]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>groupSizes.length == n</code></li>
	<li><code>1 &lt;= n&nbsp;&lt;= 500</code></li>
	<li><code>1 &lt;=&nbsp;groupSizes[i] &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash table hoặc mảng

<!-- thinking:start -->

> **Tư duy**
>
> Những người có cùng kích thước nhóm cần được xếp đủ vào các nhóm có kích thước đó. Vì $n \le 500$, ta gom chỉ số theo $groupSize$, rồi chia từng nhóm chỉ số thành các đoạn liên tiếp có độ dài tương ứng. Đề bài đảm bảo luôn có lời giải, nên các đoạn này chính là các nhóm cần trả về.

<!-- thinking:end -->

Ta dùng hash table $g$ để lưu những người cần thuộc nhóm có kích thước $groupSize$. Sau đó, ta chia danh sách người của mỗi kích thước nhóm thành các phần bằng nhau, mỗi phần gồm $groupSize$ người.

Vì giới hạn của $n$ trong đề bài nhỏ, ta cũng có thể tạo trực tiếp một mảng kích thước $n+1$ để lưu dữ liệu; cách này hiệu quả hơn.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của $groupSizes$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def groupThePeople(self, groupSizes: List[int]) -> List[List[int]]:
        g = defaultdict(list)
        for i, v in enumerate(groupSizes):
            g[v].append(i)
        return [v[j : j + i] for i, v in g.items() for j in range(0, len(v), i)]
```

#### Java

```java
class Solution {
    public List<List<Integer>> groupThePeople(int[] groupSizes) {
        int n = groupSizes.length;
        List<Integer>[] g = new List[n + 1];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (int i = 0; i < n; ++i) {
            g[groupSizes[i]].add(i);
        }
        List<List<Integer>> ans = new ArrayList<>();
        for (int i = 0; i < g.length; ++i) {
            List<Integer> v = g[i];
            for (int j = 0; j < v.size(); j += i) {
                ans.add(v.subList(j, j + i));
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
    vector<vector<int>> groupThePeople(vector<int>& groupSizes) {
        int n = groupSizes.size();
        vector<vector<int>> g(n + 1);
        for (int i = 0; i < n; ++i) {
            g[groupSizes[i]].push_back(i);
        }
        vector<vector<int>> ans;
        for (int i = 0; i < g.size(); ++i) {
            for (int j = 0; j < g[i].size(); j += i) {
                vector<int> t(g[i].begin() + j, g[i].begin() + j + i);
                ans.push_back(t);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func groupThePeople(groupSizes []int) [][]int {
	n := len(groupSizes)
	g := make([][]int, n+1)
	for i, v := range groupSizes {
		g[v] = append(g[v], i)
	}
	ans := [][]int{}
	for i, v := range g {
		for j := 0; j < len(v); j += i {
			ans = append(ans, v[j:j+i])
		}
	}
	return ans
}
```

#### TypeScript

```ts
function groupThePeople(groupSizes: number[]): number[][] {
    const n: number = groupSizes.length;
    const g: number[][] = Array.from({ length: n + 1 }, () => []);

    for (let i = 0; i < groupSizes.length; i++) {
        const size: number = groupSizes[i];
        g[size].push(i);
    }
    const ans: number[][] = [];
    for (let i = 1; i <= n; i++) {
        const group: number[] = [];
        for (let j = 0; j < g[i].length; j += i) {
            group.push(...g[i].slice(j, j + i));
            ans.push([...group]);
            group.length = 0;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn group_the_people(group_sizes: Vec<i32>) -> Vec<Vec<i32>> {
        let n: usize = group_sizes.len();
        let mut g: Vec<Vec<usize>> = vec![Vec::new(); n + 1];

        for (i, &size) in group_sizes.iter().enumerate() {
            g[size as usize].push(i);
        }

        let mut ans: Vec<Vec<i32>> = Vec::new();
        for (i, v) in g.into_iter().enumerate() {
            for j in (0..v.len()).step_by(i.max(1)) {
                ans.push(
                    v[j..(j + i).min(v.len())]
                        .iter()
                        .map(|&x| x as i32)
                        .collect(),
                );
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
