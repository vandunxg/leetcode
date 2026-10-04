---
comments: true
difficulty: Hard
rating: 2763
source: Weekly Contest 440 Q4
tags:
    - Segment Tree
    - Array
    - Enumeration
    - Prefix Sum
---

<!-- problem:start -->

# [3480. Maximize Subarrays After Removing One Conflicting Pair](https://leetcode.com/problems/maximize-subarrays-after-removing-one-conflicting-pair)

[中文文档](/solution/3400-3499/3480.Maximize%20Subarrays%20After%20Removing%20One%20Conflicting%20Pair/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code>, biểu diễn một mảng <code>nums</code> chứa các số từ 1 đến <code>n</code> theo thứ tự. Ngoài ra, bạn được cho một mảng 2D <code>conflictingPairs</code>, trong đó <code>conflictingPairs[i] = [a, b]</code> cho biết <code>a</code> và <code>b</code> tạo thành một cặp xung đột.</p>

<p>Xóa <strong>chính xác</strong> một phần tử khỏi <code>conflictingPairs</code>. Sau đó, đếm số <span data-keyword="subarray-nonempty">mảng con không rỗng</span> của <code>nums</code> không chứa đồng thời cả <code>a</code> và <code>b</code> đối với bất kỳ cặp xung đột <code>[a, b]</code> nào còn lại.</p>

<p>Trả về số lượng <strong>lớn nhất</strong> các mảng con có thể đạt được sau khi xóa <strong>chính xác</strong> một cặp xung đột.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, conflictingPairs = [[2,3],[1,4]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">9</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Xóa <code>[2, 3]</code> khỏi <code>conflictingPairs</code>. Khi đó, <code>conflictingPairs = [[1, 4]]</code>.</li>
    <li>Có 9 mảng con trong <code>nums</code> không chứa đồng thời <code>[1, 4]</code>. Đó là <code>[1]</code>, <code>[2]</code>, <code>[3]</code>, <code>[4]</code>, <code>[1, 2]</code>, <code>[2, 3]</code>, <code>[3, 4]</code>, <code>[1, 2, 3]</code> và <code>[2, 3, 4]</code>.</li>
    <li>Số lượng mảng con lớn nhất có thể đạt được sau khi xóa một phần tử khỏi <code>conflictingPairs</code> là 9.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5, conflictingPairs = [[1,2],[2,5],[3,5]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">12</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Xóa <code>[1, 2]</code> khỏi <code>conflictingPairs</code>. Khi đó, <code>conflictingPairs = [[2, 5], [3, 5]]</code>.</li>
    <li>Có 12 mảng con trong <code>nums</code> không chứa đồng thời <code>[2, 5]</code> và <code>[3, 5]</code>.</li>
    <li>Số lượng mảng con lớn nhất có thể đạt được sau khi xóa một phần tử khỏi <code>conflictingPairs</code> là 12.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= conflictingPairs.length &lt;= 2 * n</code></li>
    <li><code>conflictingPairs[i].length == 2</code></li>
    <li><code>1 &lt;= conflictingPairs[i][j] &lt;= n</code></li>
    <li><code>conflictingPairs[i][0] != conflictingPairs[i][1]</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê + Duy trì Giá trị nhỏ nhất và nhỏ thứ hai

<!-- thinking:start -->

> **Tư duy**
>
> Một mảng con hợp lệ không được chứa cả hai đầu mút của bất kỳ cặp xung đột nào. Ta xóa chính xác một cặp và muốn có càng nhiều mảng con hợp lệ càng tốt. Việc duyệt cần có độ phức tạp tuyến tính.
>
> Nếu không xóa, với đầu trái $a$, mảng con không thể đi tới đầu phải xung đột nhỏ nhất $b_1$ được xét từ $a$ về bên phải. Nó đóng góp $b_1-a$.
>
> Xóa cặp tạo ra $b_1$ sẽ thay nó bằng giá trị nhỏ thứ hai $b_2$ và tăng thêm $b_2-b_1$. Ta nhóm các phần tăng thêm này theo $b_1$; sau đó cộng phần tăng thêm lớn nhất vào tổng khi không xóa.

<!-- thinking:end -->

Ta lưu tất cả các cặp xung đột $(a, b)$ (giả sử $a < b$) trong một danh sách $g$, trong đó $g[a]$ biểu diễn tập hợp tất cả các số $b$ xung đột với $a$.

Nếu không xóa cặp nào, ta có thể duyệt ngược điểm bắt đầu $a$ của mỗi mảng con. Cận trên của điểm kết thúc là giá trị nhỏ nhất $b_1$ trong tất cả $g[x \geq a]$ (không bao gồm $b_1$), và phần đóng góp vào đáp án là $b_1 - a$.

Nếu xóa một cặp xung đột chứa $b_1$, thì $b_1$ mới sẽ là giá trị nhỏ thứ hai $b_2$ trong tất cả $g[x \geq a]$, và phần đóng góp thêm vào đáp án là $b_2 - b_1$. Ta dùng một mảng $\text{cnt}$ để ghi nhận phần đóng góp thêm cho mỗi $b_1$.

Đáp án cuối cùng là tổng của tất cả các phần đóng góp $b_1 - a$ cộng với giá trị lớn nhất của $\text{cnt}[b_1]$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số lượng cặp xung đột.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSubarrays(self, n: int, conflictingPairs: List[List[int]]) -> int:
        g = [[] for _ in range(n + 1)]
        for a, b in conflictingPairs:
            if a > b:
                a, b = b, a
            g[a].append(b)
        cnt = [0] * (n + 2)
        ans = add = 0
        b1 = b2 = n + 1
        for a in range(n, 0, -1):
            for b in g[a]:
                if b < b1:
                    b2, b1 = b1, b
                elif b < b2:
                    b2 = b
            ans += b1 - a
            cnt[b1] += b2 - b1
            add = max(add, cnt[b1])
        ans += add
        return ans
```

#### Java

```java
class Solution {
    public long maxSubarrays(int n, int[][] conflictingPairs) {
        List<Integer>[] g = new List[n + 1];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (int[] pair : conflictingPairs) {
            int a = pair[0], b = pair[1];
            if (a > b) {
                int c = a;
                a = b;
                b = c;
            }
            g[a].add(b);
        }
        long[] cnt = new long[n + 2];
        long ans = 0, add = 0;
        int b1 = n + 1, b2 = n + 1;
        for (int a = n; a > 0; --a) {
            for (int b : g[a]) {
                if (b < b1) {
                    b2 = b1;
                    b1 = b;
                } else if (b < b2) {
                    b2 = b;
                }
            }
            ans += b1 - a;
            cnt[b1] += b2 - b1;
            add = Math.max(add, cnt[b1]);
        }
        ans += add;
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maxSubarrays(int n, vector<vector<int>>& conflictingPairs) {
        vector<vector<int>> g(n + 1);
        for (auto& pair : conflictingPairs) {
            int a = pair[0], b = pair[1];
            if (a > b) {
                swap(a, b);
            }
            g[a].push_back(b);
        }

        vector<long long> cnt(n + 2, 0);
        long long ans = 0, add = 0;
        int b1 = n + 1, b2 = n + 1;

        for (int a = n; a > 0; --a) {
            for (int b : g[a]) {
                if (b < b1) {
                    b2 = b1;
                    b1 = b;
                } else if (b < b2) {
                    b2 = b;
                }
            }
            ans += b1 - a;
            cnt[b1] += b2 - b1;
            add = max(add, cnt[b1]);
        }

        ans += add;
        return ans;
    }
};
```

#### Go

```go
func maxSubarrays(n int, conflictingPairs [][]int) (ans int64) {
    g := make([][]int, n+1)
    for _, pair := range conflictingPairs {
        a, b := pair[0], pair[1]
        if a > b {
            a, b = b, a
        }
        g[a] = append(g[a], b)
    }

    cnt := make([]int64, n+2)
    var add int64
    b1, b2 := n+1, n+1

    for a := n; a > 0; a-- {
        for _, b := range g[a] {
            if b < b1 {
                b2 = b1
                b1 = b
            } else if b < b2 {
                b2 = b
            }
        }
        ans += int64(b1 - a)
        cnt[b1] += int64(b2 - b1)
        if cnt[b1] > add {
            add = cnt[b1]
        }
    }

    ans += add
    return ans
}
```

#### TypeScript

```ts
function maxSubarrays(n: number, conflictingPairs: number[][]): number {
    const g: number[][] = Array.from({ length: n + 1 }, () => []);
    for (let [a, b] of conflictingPairs) {
        if (a > b) {
            [a, b] = [b, a];
        }
        g[a].push(b);
    }

    const cnt: number[] = Array(n + 2).fill(0);
    let ans = 0,
        add = 0;
    let b1 = n + 1,
        b2 = n + 1;

    for (let a = n; a > 0; a--) {
        for (const b of g[a]) {
            if (b < b1) {
                b2 = b1;
                b1 = b;
            } else if (b < b2) {
                b2 = b;
            }
        }
        ans += b1 - a;
        cnt[b1] += b2 - b1;
        add = Math.max(add, cnt[b1]);
    }

    ans += add;
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_subarrays(n: i32, conflicting_pairs: Vec<Vec<i32>>) -> i64 {
        let mut g: Vec<Vec<i32>> = vec![vec![]; (n + 1) as usize];
        for pair in conflicting_pairs {
            let mut a = pair[0];
            let mut b = pair[1];
            if a > b {
                std::mem::swap(&mut a, &mut b);
            }
            g[a as usize].push(b);
        }

        let mut cnt: Vec<i64> = vec![0; (n + 2) as usize];
        let mut ans = 0i64;
        let mut add = 0i64;
        let mut b1 = n + 1;
        let mut b2 = n + 1;

        for a in (1..=n).rev() {
            for &b in &g[a as usize] {
                if b < b1 {
                    b2 = b1;
                    b1 = b;
                } else if b < b2 {
                    b2 = b;
                }
            }
            ans += (b1 - a) as i64;
            cnt[b1 as usize] += (b2 - b1) as i64;
            add = std::cmp::max(add, cnt[b1 as usize]);
        }

        ans += add;
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
