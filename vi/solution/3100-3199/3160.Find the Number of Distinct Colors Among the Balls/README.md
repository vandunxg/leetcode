---
comments: true
difficulty: Medium
rating: 1517
source: Biweekly Contest 131 Q3
tags:
    - Array
    - Hash Table
    - Simulation
---

<!-- problem:start -->

# [3160. Find the Number of Distinct Colors Among the Balls](https://leetcode.com/problems/find-the-number-of-distinct-colors-among-the-balls)

[中文文档](/solution/3100-3199/3160.Find%20the%20Number%20of%20Distinct%20Colors%20Among%20the%20Balls/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>limit</code> và một mảng hai chiều <code>queries</code> có kích thước <code>n x 2</code>.</p>

<p>Có <code>limit + 1</code> quả bóng với các nhãn <strong>khác nhau</strong> trong phạm vi <code>[0, limit]</code>. Ban đầu, tất cả các quả bóng đều chưa được tô màu. Với mỗi truy vấn trong <code>queries</code> có dạng <code>[x, y]</code>, bạn tô quả bóng <code>x</code> bằng màu <code>y</code>. Sau mỗi truy vấn, bạn cần tìm số lượng màu xuất hiện trên các quả bóng.</p>

<p>Trả về một mảng <code>result</code> có độ dài <code>n</code>, trong đó <code>result[i]</code> là số lượng màu <em>sau</em> truy vấn thứ <code>i<sup>th</sup></code>.</p>

<p><strong>Lưu ý</strong> rằng khi trả lời một truy vấn, việc không có màu sẽ <em>không</em> được tính là một màu.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">limit = 4, queries = [[1,4],[2,5],[1,3],[3,4]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,2,2,3]</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3100-3199/3160.Find%20the%20Number%20of%20Distinct%20Colors%20Among%20the%20Balls/images/ezgifcom-crop.gif" style="width: 455px; height: 145px;" /></p>

<ul>
    <li>Sau truy vấn 0, quả bóng 1 có màu 4.</li>
    <li>Sau truy vấn 1, quả bóng 1 có màu 4 và quả bóng 2 có màu 5.</li>
    <li>Sau truy vấn 2, quả bóng 1 có màu 3 và quả bóng 2 có màu 5.</li>
    <li>Sau truy vấn 3, quả bóng 1 có màu 3, quả bóng 2 có màu 5 và quả bóng 3 có màu 4.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">limit = 4, queries = [[0,1],[1,2],[2,2],[3,4],[4,5]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,2,2,3,4]</span></p>

<p><strong>Giải thích:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3100-3199/3160.Find%20the%20Number%20of%20Distinct%20Colors%20Among%20the%20Balls/images/ezgifcom-crop2.gif" style="width: 457px; height: 144px;" /></strong></p>

<ul>
    <li>Sau truy vấn 0, quả bóng 0 có màu 1.</li>
    <li>Sau truy vấn 1, quả bóng 0 có màu 1 và quả bóng 1 có màu 2.</li>
    <li>Sau truy vấn 2, quả bóng 0 có màu 1, còn các quả bóng 1 và 2 có màu 2.</li>
    <li>Sau truy vấn 3, quả bóng 0 có màu 1, các quả bóng 1 và 2 có màu 2, còn quả bóng 3 có màu 4.</li>
    <li>Sau truy vấn 4, quả bóng 0 có màu 1, các quả bóng 1 và 2 có màu 2, quả bóng 3 có màu 4 và quả bóng 4 có màu 5.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= limit &lt;= 10<sup>9</sup></code></li>
    <li><code>1 &lt;= n == queries.length &lt;= 10<sup>5</sup></code></li>
    <li><code>queries[i].length == 2</code></li>
    <li><code>0 &lt;= queries[i][0] &lt;= limit</code></li>
    <li><code>1 &lt;= queries[i][1] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai bảng băm

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lần cập nhật sẽ tô màu một quả bóng và yêu cầu đếm số màu phân biệt đang tồn tại. Vì $limit$ có thể lên tới $10^9$, không thể dùng một mảng để lưu các quả bóng, còn việc duyệt lại sau mỗi lần tô màu thì quá chậm.
>
> Chỉ có quả bóng được tô màu thay đổi: tăng số lượng của màu mới, giảm số lượng của màu cũ và xóa màu đó khi số lượng giảm về 0. Số lượng khóa chính là đáp án.
>
> Dùng $g$ để ánh xạ quả bóng tới màu và $cnt$ để ánh xạ màu tới số lượng. Sau mỗi truy vấn, thêm $len(cnt)$ vào kết quả.

<!-- thinking:end -->

Ta sử dụng một bảng băm `g` để ghi lại màu của mỗi quả bóng, và một bảng băm khác `cnt` để ghi lại số lượng của mỗi màu.

Tiếp theo, ta duyệt mảng `queries`. Với mỗi truy vấn $(x, y)$, ta tăng số lượng của màu $y$ lên $1$, sau đó kiểm tra xem quả bóng $x$ đã được tô màu hay chưa. Nếu đã được tô, ta giảm số lượng của màu trên quả bóng $x$ đi $1$. Nếu số lượng giảm xuống $0$, ta xóa màu đó khỏi bảng băm `cnt`. Sau đó, ta cập nhật màu của quả bóng $x$ thành $y$ và thêm kích thước hiện tại của bảng băm `cnt` vào mảng kết quả.

Sau khi duyệt xong, ta trả về mảng kết quả.

Độ phức tạp thời gian là $O(m)$, và độ phức tạp không gian là $O(m)$, trong đó $m$ là độ dài của mảng `queries`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def queryResults(self, limit: int, queries: List[List[int]]) -> List[int]:
        g = {}
        cnt = Counter()
        ans = []
        for x, y in queries:
            cnt[y] += 1
            if x in g:
                cnt[g[x]] -= 1
                if cnt[g[x]] == 0:
                    cnt.pop(g[x])
            g[x] = y
            ans.append(len(cnt))
        return ans
```

#### Java

```java
class Solution {
    public int[] queryResults(int limit, int[][] queries) {
        Map<Integer, Integer> g = new HashMap<>();
        Map<Integer, Integer> cnt = new HashMap<>();
        int m = queries.length;
        int[] ans = new int[m];
        for (int i = 0; i < m; ++i) {
            int x = queries[i][0], y = queries[i][1];
            cnt.merge(y, 1, Integer::sum);
            if (g.containsKey(x) && cnt.merge(g.get(x), -1, Integer::sum) == 0) {
                cnt.remove(g.get(x));
            }
            g.put(x, y);
            ans[i] = cnt.size();
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> queryResults(int limit, vector<vector<int>>& queries) {
        unordered_map<int, int> g;
        unordered_map<int, int> cnt;
        vector<int> ans;
        for (auto& q : queries) {
            int x = q[0], y = q[1];
            cnt[y]++;
            if (g.contains(x) && --cnt[g[x]] == 0) {
                cnt.erase(g[x]);
            }
            g[x] = y;
            ans.push_back(cnt.size());
        }
        return ans;
    }
};
```

#### Go

```go
func queryResults(limit int, queries [][]int) (ans []int) {
    g := map[int]int{}
    cnt := map[int]int{}
    for _, q := range queries {
        x, y := q[0], q[1]
        cnt[y]++
        if v, ok := g[x]; ok {
            cnt[v]--
            if cnt[v] == 0 {
                delete(cnt, v)
            }
        }
        g[x] = y
        ans = append(ans, len(cnt))
    }
    return
}
```

#### TypeScript

```ts
function queryResults(limit: number, queries: number[][]): number[] {
    const g = new Map<number, number>();
    const cnt = new Map<number, number>();
    const ans: number[] = [];
    for (const [x, y] of queries) {
        cnt.set(y, (cnt.get(y) ?? 0) + 1);
        if (g.has(x)) {
            const v = g.get(x)!;
            cnt.set(v, cnt.get(v)! - 1);
            if (cnt.get(v) === 0) {
                cnt.delete(v);
            }
        }
        g.set(x, y);
        ans.push(cnt.size);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
