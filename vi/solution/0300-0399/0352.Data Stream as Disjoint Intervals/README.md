---
comments: true
difficulty: Hard
tags:
    - Union Find
    - Design
    - Hash Table
    - Binary Search
    - Data Stream
    - Ordered Set
---

<!-- problem:start -->

# [352. Data Stream as Disjoint Intervals](https://leetcode.com/problems/data-stream-as-disjoint-intervals)

[中文文档](/solution/0300-0399/0352.Data%20Stream%20as%20Disjoint%20Intervals/README.md)

## Mô tả

<!-- description:start -->

<p>Cho luồng dữ liệu gồm các số nguyên không âm <code>a<sub>1</sub>, a<sub>2</sub>, ..., a<sub>n</sub></code>; hãy tóm tắt các số đã xuất hiện thành danh sách các khoảng rời nhau.</p>

<p>Hãy triển khai class <code>SummaryRanges</code>:</p>

<ul>
	<li><code>SummaryRanges()</code> Khởi tạo đối tượng với luồng rỗng.</li>
	<li><code>void addNum(int value)</code> Thêm số nguyên <code>value</code> vào luồng.</li>
	<li><code>int[][] getIntervals()</code> Trả về bản tóm tắt các số hiện có trong luồng dưới dạng danh sách khoảng rời nhau <code>[start<sub>i</sub>, end<sub>i</sub>]</code>. Kết quả phải được sắp xếp theo <code>start<sub>i</sub></code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;SummaryRanges&quot;, &quot;addNum&quot;, &quot;getIntervals&quot;, &quot;addNum&quot;, &quot;getIntervals&quot;, &quot;addNum&quot;, &quot;getIntervals&quot;, &quot;addNum&quot;, &quot;getIntervals&quot;, &quot;addNum&quot;, &quot;getIntervals&quot;]
[[], [1], [], [3], [], [7], [], [2], [], [6], []]
<strong>Đầu ra</strong>
[null, null, [[1, 1]], null, [[1, 1], [3, 3]], null, [[1, 1], [3, 3], [7, 7]], null, [[1, 3], [7, 7]], null, [[1, 3], [6, 7]]]

<strong>Giải thích</strong>
SummaryRanges summaryRanges = new SummaryRanges();
summaryRanges.addNum(1);      // arr = [1]
summaryRanges.getIntervals(); // return [[1, 1]]
summaryRanges.addNum(3);      // arr = [1, 3]
summaryRanges.getIntervals(); // return [[1, 1], [3, 3]]
summaryRanges.addNum(7);      // arr = [1, 3, 7]
summaryRanges.getIntervals(); // return [[1, 1], [3, 3], [7, 7]]
summaryRanges.addNum(2);      // arr = [1, 2, 3, 7]
summaryRanges.getIntervals(); // return [[1, 3], [7, 7]]
summaryRanges.addNum(6);      // arr = [1, 2, 3, 6, 7]
summaryRanges.getIntervals(); // return [[1, 3], [6, 7]]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= value &lt;= 10<sup>4</sup></code></li>
	<li>Sẽ có nhiều nhất <code>3 * 10<sup>4</sup></code> lần gọi <code>addNum</code> và <code>getIntervals</code>.</li>
	<li>Sẽ có nhiều nhất <code>10<sup>2</sup></code>&nbsp;lần gọi&nbsp;<code>getIntervals</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Nếu có nhiều lần gộp và số khoảng rời nhau nhỏ hơn nhiều so với kích thước luồng dữ liệu thì sao?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các số trong luồng cần được gộp thành những khoảng rời nhau. Sắp xếp lại toàn bộ dữ liệu sau mỗi lần thêm sẽ rất tốn kém. Ordered map dùng đầu trái của khoảng làm key cho phép mỗi lần thêm chỉ cần kiểm tra các khoảng lân cận.
>
> Dùng binary search để tìm khoảng kế tiếp và khoảng trước đó: nếu giá trị lấp khoảng trống giữa hai phía thì gộp ba khoảng; nếu chỉ kề một phía thì mở rộng khoảng đó; nếu không thì thêm một khoảng đơn. `getIntervals` liệt kê các khoảng theo thứ tự đã sắp xếp.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class SummaryRanges:
    def __init__(self):
        self.mp = SortedDict()

    def addNum(self, val: int) -> None:
        n = len(self.mp)
        ridx = self.mp.bisect_right(val)
        lidx = n if ridx == 0 else ridx - 1
        keys = self.mp.keys()
        values = self.mp.values()
        if (
            lidx != n
            and ridx != n
            and values[lidx][1] + 1 == val
            and values[ridx][0] - 1 == val
        ):
            self.mp[keys[lidx]][1] = self.mp[keys[ridx]][1]
            self.mp.pop(keys[ridx])
        elif lidx != n and val <= values[lidx][1] + 1:
            self.mp[keys[lidx]][1] = max(val, self.mp[keys[lidx]][1])
        elif ridx != n and val >= values[ridx][0] - 1:
            self.mp[keys[ridx]][0] = min(val, self.mp[keys[ridx]][0])
        else:
            self.mp[val] = [val, val]

    def getIntervals(self) -> List[List[int]]:
        return list(self.mp.values())


# # Your SummaryRanges object will be instantiated and called as such:
# # obj = SummaryRanges()
# # obj.addNum(val)
# # param_2 = obj.getIntervals()
```

#### Java

```java
class SummaryRanges {
    private TreeMap<Integer, int[]> mp;

    public SummaryRanges() {
        mp = new TreeMap<>();
    }

    public void addNum(int val) {
        Integer l = mp.floorKey(val);
        Integer r = mp.ceilingKey(val);
        if (l != null && r != null && mp.get(l)[1] + 1 == val && mp.get(r)[0] - 1 == val) {
            mp.get(l)[1] = mp.get(r)[1];
            mp.remove(r);
        } else if (l != null && val <= mp.get(l)[1] + 1) {
            mp.get(l)[1] = Math.max(val, mp.get(l)[1]);
        } else if (r != null && val >= mp.get(r)[0] - 1) {
            mp.get(r)[0] = Math.min(val, mp.get(r)[0]);
        } else {
            mp.put(val, new int[] {val, val});
        }
    }

    public int[][] getIntervals() {
        int[][] res = new int[mp.size()][2];
        int i = 0;
        for (int[] range : mp.values()) {
            res[i++] = range;
        }
        return res;
    }
}

/**
 * Your SummaryRanges object will be instantiated and called as such:
 * SummaryRanges obj = new SummaryRanges();
 * obj.addNum(val);
 * int[][] param_2 = obj.getIntervals();
 */
```

#### C++

```cpp
class SummaryRanges {
private:
    map<int, vector<int>> mp;

public:
    SummaryRanges() {
    }

    void addNum(int val) {
        auto r = mp.upper_bound(val);
        auto l = r == mp.begin() ? mp.end() : prev(r);
        if (l != mp.end() && r != mp.end() && l->second[1] + 1 == val && r->second[0] - 1 == val) {
            l->second[1] = r->second[1];
            mp.erase(r);
        } else if (l != mp.end() && val <= l->second[1] + 1)
            l->second[1] = max(val, l->second[1]);
        else if (r != mp.end() && val >= r->second[0] - 1)
            r->second[0] = min(val, r->second[0]);
        else
            mp[val] = {val, val};
    }

    vector<vector<int>> getIntervals() {
        vector<vector<int>> res;
        for (auto& range : mp) res.push_back(range.second);
        return res;
    }
};

/**
 * Your SummaryRanges object will be instantiated and called as such:
 * SummaryRanges* obj = new SummaryRanges();
 * obj->addNum(val);
 * vector<vector<int>> param_2 = obj->getIntervals();
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
