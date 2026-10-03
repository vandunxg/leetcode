---
comments: true
difficulty: Hard
rating: 2081
source: Biweekly Contest 51 Q4
tags:
    - Array
    - Binary Search
    - Ordered Set
    - Sorting
---

<!-- problem:start -->

# [1847. Closest Room](https://leetcode.com/problems/closest-room)

[中文文档](/solution/1800-1899/1847.Closest%20Room/README.md)

## Mô tả

<!-- description:start -->

<p>Có một khách sạn với <code>n</code> phòng. Các phòng được biểu diễn bởi một mảng số nguyên 2 chiều <code>rooms</code>, trong đó <code>rooms[i] = [roomId<sub>i</sub>, size<sub>i</sub>]</code> cho biết có một phòng mang số <code>roomId<sub>i</sub></code> và có kích thước bằng <code>size<sub>i</sub></code>. Đảm bảo mỗi <code>roomId<sub>i</sub></code> là <strong>duy nhất</strong>.</p>

<p>Ta cũng được cho <code>k</code> truy vấn trong một mảng 2 chiều <code>queries</code>, trong đó <code>queries[j] = [preferred<sub>j</sub>, minSize<sub>j</sub>]</code>. Câu trả lời cho truy vấn thứ <code>j<sup>th</sup></code> là số phòng <code>id</code> của một phòng thỏa mãn:</p>

<ul>
	<li>Phòng có kích thước <strong>ít nhất</strong> <code>minSize<sub>j</sub></code>, và</li>
	<li><code>abs(id - preferred<sub>j</sub>)</code> được <strong>tối thiểu hóa</strong>, trong đó <code>abs(x)</code> là giá trị tuyệt đối của <code>x</code>.</li>
</ul>

<p>Nếu có <strong>hòa</strong> về khoảng cách tuyệt đối, chọn phòng có <code>id</code> <strong>nhỏ nhất</strong>. Nếu <strong>không tồn tại phòng phù hợp</strong>, câu trả lời là <code>-1</code>.</p>

<p>Trả về <em>một mảng </em><code>answer</code><em> có độ dài </em><code>k</code><em>, trong đó </em><code>answer[j]</code><em> chứa câu trả lời cho truy vấn thứ </em><code>j<sup>th</sup></code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> rooms = [[2,2],[1,2],[3,2]], queries = [[3,1],[3,3],[5,2]]
<strong>Đầu ra:</strong> [3,-1,3]
<strong>Giải thích: </strong>Câu trả lời cho các truy vấn như sau:
Query = [3,1]: Phòng số 3 gần nhất vì abs(3 - 3) = 0 và kích thước 2 của phòng ít nhất là 1. Câu trả lời là 3.
Query = [3,3]: Không có phòng nào có kích thước ít nhất là 3, nên câu trả lời là -1.
Query = [5,2]: Phòng số 3 gần nhất vì abs(3 - 5) = 2 và kích thước 2 của phòng ít nhất là 2. Câu trả lời là 3.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> rooms = [[1,4],[2,3],[3,5],[4,1],[5,2]], queries = [[2,3],[2,4],[2,5]]
<strong>Đầu ra:</strong> [2,1,3]
<strong>Giải thích: </strong>Câu trả lời cho các truy vấn như sau:
Query = [2,3]: Phòng số 2 gần nhất vì abs(2 - 2) = 0 và kích thước 3 của phòng ít nhất là 3. Câu trả lời là 2.
Query = [2,4]: Phòng số 1 và 3 đều có kích thước ít nhất là 4. Câu trả lời là 1 vì số nhỏ hơn.
Query = [2,5]: Phòng số 3 là phòng duy nhất có kích thước ít nhất là 5. Câu trả lời là 3.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == rooms.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>k == queries.length</code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= roomId<sub>i</sub>, preferred<sub>j</sub> &lt;= 10<sup>7</sup></code></li>
	<li><code>1 &lt;= size<sub>i</sub>, minSize<sub>j</sub> &lt;= 10<sup>7</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Truy vấn offline + Ordered Set + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi truy vấn cần phòng có id gần $\textit{preferred}$ nhất trong số các phòng có diện tích ít nhất $\textit{minSize}$. Duyệt mọi phòng cho từng truy vấn là quá chậm khi $n,k\le 10^5$.
>
> Các truy vấn độc lập, nên ta xử lý offline theo $\textit{minSize}$ tăng dần. Sắp xếp các phòng theo diện tích và giữ các id còn hợp lệ trong một ordered set, xóa những phòng nhỏ hơn ngưỡng hiện tại. Dùng tìm kiếm nhị phân để tìm hai phần tử lân cận của $\textit{preferred}$ trong set đó.

<!-- thinking:end -->

Ta nhận thấy thứ tự xử lý các truy vấn không ảnh hưởng đến đáp án, và bài toán liên quan đến quan hệ kích thước phòng. Vì vậy, ta có thể sắp xếp các truy vấn theo diện tích tối thiểu tăng dần để xử lý từ nhỏ đến lớn. Đồng thời, ta sắp xếp các phòng theo diện tích tăng dần.

Tiếp theo, ta tạo một danh sách có thứ tự và thêm tất cả số phòng vào danh sách đó.

Sau đó, ta xử lý các truy vấn từ nhỏ đến lớn. Với mỗi truy vấn, trước tiên ta xóa khỏi danh sách có thứ tự tất cả phòng có diện tích nhỏ hơn diện tích tối thiểu của truy vấn hiện tại. Trong các phòng còn lại, ta dùng tìm kiếm nhị phân để tìm số phòng gần truy vấn hiện tại nhất. Nếu không có phòng phù hợp, ta trả về $-1$.

Độ phức tạp thời gian là $O(n \times \log n + k \times \log k)$ và độ phức tạp không gian là $O(n + k)$, trong đó $n$ và $k$ lần lượt là số phòng và số truy vấn.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def closestRoom(
        self, rooms: List[List[int]], queries: List[List[int]]
    ) -> List[int]:
        rooms.sort(key=lambda x: x[1])
        k = len(queries)
        idx = sorted(range(k), key=lambda i: queries[i][1])
        ans = [-1] * k
        i, n = 0, len(rooms)
        sl = SortedList(x[0] for x in rooms)
        for j in idx:
            prefer, minSize = queries[j]
            while i < n and rooms[i][1] < minSize:
                sl.remove(rooms[i][0])
                i += 1
            if i == n:
                break
            p = sl.bisect_left(prefer)
            if p < len(sl):
                ans[j] = sl[p]
            if p and (ans[j] == -1 or ans[j] - prefer >= prefer - sl[p - 1]):
                ans[j] = sl[p - 1]
        return ans
```

#### Java

```java
class Solution {
    public int[] closestRoom(int[][] rooms, int[][] queries) {
        int n = rooms.length;
        int k = queries.length;
        Arrays.sort(rooms, (a, b) -> a[1] - b[1]);
        Integer[] idx = new Integer[k];
        for (int i = 0; i < k; i++) {
            idx[i] = i;
        }
        Arrays.sort(idx, (i, j) -> queries[i][1] - queries[j][1]);
        int i = 0;
        TreeMap<Integer, Integer> tm = new TreeMap<>();
        for (int[] room : rooms) {
            tm.merge(room[0], 1, Integer::sum);
        }
        int[] ans = new int[k];
        Arrays.fill(ans, -1);
        for (int j : idx) {
            int prefer = queries[j][0], minSize = queries[j][1];
            while (i < n && rooms[i][1] < minSize) {
                if (tm.merge(rooms[i][0], -1, Integer::sum) == 0) {
                    tm.remove(rooms[i][0]);
                }
                ++i;
            }
            if (i == n) {
                break;
            }
            Integer p = tm.ceilingKey(prefer);
            if (p != null) {
                ans[j] = p;
            }
            p = tm.floorKey(prefer);
            if (p != null && (ans[j] == -1 || ans[j] - prefer >= prefer - p)) {
                ans[j] = p;
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
    vector<int> closestRoom(vector<vector<int>>& rooms, vector<vector<int>>& queries) {
        int n = rooms.size();
        int k = queries.size();
        sort(rooms.begin(), rooms.end(), [](const vector<int>& a, const vector<int>& b) {
            return a[1] < b[1];
        });
        vector<int> idx(k);
        iota(idx.begin(), idx.end(), 0);
        sort(idx.begin(), idx.end(), [&](int i, int j) {
            return queries[i][1] < queries[j][1];
        });
        vector<int> ans(k, -1);
        int i = 0;
        multiset<int> s;
        for (auto& room : rooms) {
            s.insert(room[0]);
        }
        for (int j : idx) {
            int prefer = queries[j][0], minSize = queries[j][1];
            while (i < n && rooms[i][1] < minSize) {
                s.erase(s.find(rooms[i][0]));
                ++i;
            }
            if (i == n) {
                break;
            }
            auto it = s.lower_bound(prefer);
            if (it != s.end()) {
                ans[j] = *it;
            }
            if (it != s.begin()) {
                --it;
                if (ans[j] == -1 || abs(*it - prefer) <= abs(ans[j] - prefer)) {
                    ans[j] = *it;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func closestRoom(rooms [][]int, queries [][]int) []int {
	n, k := len(rooms), len(queries)
	sort.Slice(rooms, func(i, j int) bool { return rooms[i][1] < rooms[j][1] })
	idx := make([]int, k)
	ans := make([]int, k)
	for i := range idx {
		idx[i] = i
		ans[i] = -1
	}
	sort.Slice(idx, func(i, j int) bool { return queries[idx[i]][1] < queries[idx[j]][1] })
	rbt := redblacktree.NewWithIntComparator()
	merge := func(rbt *redblacktree.Tree, key, value int) {
		if v, ok := rbt.Get(key); ok {
			nxt := v.(int) + value
			if nxt == 0 {
				rbt.Remove(key)
			} else {
				rbt.Put(key, nxt)
			}
		} else {
			rbt.Put(key, value)
		}
	}
	for _, room := range rooms {
		merge(rbt, room[0], 1)
	}
	i := 0

	for _, j := range idx {
		prefer, minSize := queries[j][0], queries[j][1]
		for i < n && rooms[i][1] < minSize {
			merge(rbt, rooms[i][0], -1)
			i++
		}
		if i == n {
			break
		}
		c, _ := rbt.Ceiling(prefer)
		f, _ := rbt.Floor(prefer)
		if c != nil {
			ans[j] = c.Key.(int)
		}
		if f != nil && (ans[j] == -1 || ans[j]-prefer >= prefer-f.Key.(int)) {
			ans[j] = f.Key.(int)
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
