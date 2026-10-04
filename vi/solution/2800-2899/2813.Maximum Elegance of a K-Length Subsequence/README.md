---
comments: true
difficulty: Hard
rating: 2582
source: Weekly Contest 357 Q4
tags:
    - Stack
    - Greedy
    - Array
    - Hash Table
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2813. Maximum Elegance of a K-Length Subsequence](https://leetcode.com/problems/maximum-elegance-of-a-k-length-subsequence)

[中文文档](/solution/2800-2899/2813.Maximum%20Elegance%20of%20a%20K-Length%20Subsequence/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên 2 chiều <code>items</code> có độ dài <code>n</code>, được <strong>đánh chỉ số từ 0</strong>, và một số nguyên <code>k</code>.</p>

<p><code>items[i] = [profit<sub>i</sub>, category<sub>i</sub>]</code>, trong đó <code>profit<sub>i</sub></code> và <code>category<sub>i</sub></code> lần lượt là lợi nhuận và danh mục của phần tử thứ <code>i<sup>th</sup></code>.</p>

<p>Ta định nghĩa <strong>độ thanh lịch</strong> của một <strong>dãy con</strong> của <code>items</code> là <code>total_profit + distinct_categories<sup>2</sup></code>, trong đó <code>total_profit</code> là tổng lợi nhuận của tất cả phần tử trong dãy con, còn <code>distinct_categories</code> là số lượng danh mục <strong>khác nhau</strong> trong các danh mục của dãy con được chọn.</p>

<p>Nhiệm vụ của bạn là tìm <strong>độ thanh lịch lớn nhất</strong> trong tất cả các dãy con có kích thước <code>k</code> của <code>items</code>.</p>

<p>Trả về <em>một số nguyên biểu thị độ thanh lịch lớn nhất của một dãy con của </em><code>items</code><em> có kích thước chính xác bằng </em><code>k</code>.</p>

<p><strong>Lưu ý:</strong> Dãy con của một mảng là một mảng mới được tạo từ mảng ban đầu bằng cách xóa một số phần tử (có thể không xóa phần tử nào) mà không thay đổi thứ tự tương đối của các phần tử còn lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> items = [[3,2],[5,1],[10,1]], k = 2
<strong>Đầu ra:</strong> 17
<strong>Giải thích: </strong>Trong ví dụ này, ta cần chọn một dãy con có kích thước 2.
Ta có thể chọn items[0] = [3,2] và items[2] = [10,1].
Tổng lợi nhuận của dãy con này là 3 + 10 = 13, và dãy con chứa 2 danh mục khác nhau [2,1].
Do đó, độ thanh lịch là 13 + 2<sup>2</sup> = 17, và có thể chứng minh đây là độ thanh lịch lớn nhất có thể đạt được.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> items = [[3,1],[3,1],[2,2],[5,3]], k = 3
<strong>Đầu ra:</strong> 19
<strong>Giải thích:</strong> Trong ví dụ này, ta cần chọn một dãy con có kích thước 3.
Ta có thể chọn items[0] = [3,1], items[2] = [2,2] và items[3] = [5,3].
Tổng lợi nhuận của dãy con này là 3 + 2 + 5 = 10, và dãy con chứa 3 danh mục khác nhau [1,2,3].
Do đó, độ thanh lịch là 10 + 3<sup>2</sup> = 19, và có thể chứng minh đây là độ thanh lịch lớn nhất có thể đạt được.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> items = [[1,1],[2,1],[3,1]], k = 3
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Trong ví dụ này, ta cần chọn một dãy con có kích thước 3.
Ta nên chọn tất cả các phần tử.
Tổng lợi nhuận sẽ là 1 + 2 + 3 = 6, và dãy con chứa 1 danh mục duy nhất [1].
Do đó, độ thanh lịch lớn nhất là 6 + 1<sup>2</sup> = 7.  </pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= items.length == n &lt;= 10<sup>5</sup></code></li>
	<li><code>items[i].length == 2</code></li>
	<li><code>items[i][0] == profit<sub>i</sub></code></li>
	<li><code>items[i][1] == category<sub>i</sub></code></li>
	<li><code>1 &lt;= profit<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= category<sub>i</sub> &lt;= n </code></li>
	<li><code>1 &lt;= k &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Độ thanh lịch bằng tổng lợi nhuận cộng với bình phương số danh mục khác nhau; việc liệt kê tất cả các dãy con độ dài $k$ là bất khả thi. Các phần tử có lợi nhuận cao nên được chọn trước: lấy $k$ phần tử có lợi nhuận lớn nhất, ghi nhận các danh mục vào một set và đưa lợi nhuận của các phần tử trùng danh mục vào một stack. Sau đó, một danh mục mới có thể thay thế phần tử trùng danh mục rẻ nhất trong stack, đánh đổi một phần lợi nhuận để tăng thành phần bình phương số danh mục.

<!-- thinking:end -->

Ta có thể sắp xếp tất cả các phần tử theo lợi nhuận giảm dần. Đầu tiên chọn $k$ phần tử đầu tiên và tính tổng lợi nhuận $tot$. Dùng một hash table $vis$ để ghi nhận các danh mục của $k$ phần tử này, dùng một stack $dup$ để lưu lợi nhuận của các danh mục bị lặp theo thứ tự, và dùng một biến $ans$ để lưu độ thanh lịch lớn nhất hiện tại.

Tiếp theo, ta xét phần tử bắt đầu từ phần tử thứ $k+1$. Nếu danh mục của phần tử này đã có trong $vis$, điều đó có nghĩa là nếu chọn danh mục này thì số danh mục khác nhau sẽ không tăng, nên ta có thể bỏ qua phần tử này. Nếu trước đó không còn danh mục nào bị lặp, ta cũng có thể bỏ qua phần tử này. Ngược lại, ta có thể thay phần tử trên cùng của stack $dup$ (phần tử có lợi nhuận nhỏ nhất trong các danh mục bị lặp) bằng phần tử hiện tại. Khi đó, tổng lợi nhuận tăng thêm $p - dup.pop()$ và số danh mục khác nhau tăng thêm $1$, nên ta có thể cập nhật $tot$ và $ans$.

Cuối cùng, ta trả về $ans$.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số lượng phần tử.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findMaximumElegance(self, items: List[List[int]], k: int) -> int:
        items.sort(key=lambda x: -x[0])
        tot = 0
        vis = set()
        dup = []
        for p, c in items[:k]:
            tot += p
            if c not in vis:
                vis.add(c)
            else:
                dup.append(p)
        ans = tot + len(vis) ** 2
        for p, c in items[k:]:
            if c in vis or not dup:
                continue
            vis.add(c)
            tot += p - dup.pop()
            ans = max(ans, tot + len(vis) ** 2)
        return ans
```

#### Java

```java
class Solution {
    public long findMaximumElegance(int[][] items, int k) {
        Arrays.sort(items, (a, b) -> b[0] - a[0]);
        int n = items.length;
        long tot = 0;
        Set<Integer> vis = new HashSet<>();
        Deque<Integer> dup = new ArrayDeque<>();
        for (int i = 0; i < k; ++i) {
            int p = items[i][0], c = items[i][1];
            tot += p;
            if (!vis.add(c)) {
                dup.push(p);
            }
        }
        long ans = tot + (long) vis.size() * vis.size();
        for (int i = k; i < n; ++i) {
            int p = items[i][0], c = items[i][1];
            if (vis.contains(c) || dup.isEmpty()) {
                continue;
            }
            vis.add(c);
            tot += p - dup.pop();
            ans = Math.max(ans, tot + (long) vis.size() * vis.size());
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long findMaximumElegance(vector<vector<int>>& items, int k) {
        sort(items.begin(), items.end(), [](const vector<int>& a, const vector<int>& b) {
            return a[0] > b[0];
        });
        long long tot = 0;
        unordered_set<int> vis;
        stack<int> dup;
        for (int i = 0; i < k; ++i) {
            int p = items[i][0], c = items[i][1];
            tot += p;
            if (vis.count(c)) {
                dup.push(p);
            } else {
                vis.insert(c);
            }
        }
        int n = items.size();
        long long ans = tot + 1LL * vis.size() * vis.size();
        for (int i = k; i < n; ++i) {
            int p = items[i][0], c = items[i][1];
            if (vis.count(c) || dup.empty()) {
                continue;
            }
            vis.insert(c);
            tot += p - dup.top();
            dup.pop();
            ans = max(ans, tot + (long long) (1LL * vis.size() * vis.size()));
        }
        return ans;
    }
};
```

#### Go

```go
func findMaximumElegance(items [][]int, k int) int64 {
	sort.Slice(items, func(i, j int) bool { return items[i][0] > items[j][0] })
	tot := 0
	vis := map[int]bool{}
	dup := []int{}
	for _, item := range items[:k] {
		p, c := item[0], item[1]
		tot += p
		if vis[c] {
			dup = append(dup, p)
		} else {
			vis[c] = true
		}
	}
	ans := tot + len(vis)*len(vis)
	for _, item := range items[k:] {
		p, c := item[0], item[1]
		if vis[c] || len(dup) == 0 {
			continue
		}
		vis[c] = true
		tot += p - dup[len(dup)-1]
		dup = dup[:len(dup)-1]
		ans = max(ans, tot+len(vis)*len(vis))
	}
	return int64(ans)
}
```

#### TypeScript

```ts
function findMaximumElegance(items: number[][], k: number): number {
    items.sort((a, b) => b[0] - a[0]);
    let tot = 0;
    const vis: Set<number> = new Set();
    const dup: number[] = [];
    for (const [p, c] of items.slice(0, k)) {
        tot += p;
        if (vis.has(c)) {
            dup.push(p);
        } else {
            vis.add(c);
        }
    }
    let ans = tot + vis.size ** 2;
    for (const [p, c] of items.slice(k)) {
        if (vis.has(c) || dup.length === 0) {
            continue;
        }
        tot += p - dup.pop()!;
        vis.add(c);
        ans = Math.max(ans, tot + vis.size ** 2);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
