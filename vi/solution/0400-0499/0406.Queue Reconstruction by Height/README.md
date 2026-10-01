---
comments: true
difficulty: Medium
tags:
    - Binary Indexed Tree
    - Segment Tree
    - Array
    - Sorting
---

<!-- problem:start -->

# [406. Queue Reconstruction by Height](https://leetcode.com/problems/queue-reconstruction-by-height)

[中文文档](/solution/0400-0499/0406.Queue%20Reconstruction%20by%20Height/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng người <code>people</code>, lưu thông tin của một số người trong hàng đợi (thứ tự chưa nhất thiết đúng). Mỗi <code>people[i] = [h<sub>i</sub>, k<sub>i</sub>]</code> biểu diễn người thứ <code>i<sup>th</sup></code> có chiều cao <code>h<sub>i</sub></code>, phía trước có <strong>chính xác</strong> <code>k<sub>i</sub></code> người khác có chiều cao lớn hơn hoặc bằng <code>h<sub>i</sub></code>.</p>

<p>Hãy khôi phục và trả về <em>hàng đợi được biểu diễn bởi mảng đầu vào </em><code>people</code>. Hàng đợi trả về cần có định dạng mảng <code>queue</code>, trong đó <code>queue[j] = [h<sub>j</sub>, k<sub>j</sub>]</code> là thông tin của người thứ <code>j<sup>th</sup></code> trong hàng (<code>queue[0]</code> là người đứng đầu hàng).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> people = [[7,0],[4,4],[7,1],[5,0],[6,1],[5,2]]
<strong>Đầu ra:</strong> [[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]]
<strong>Giải thích:</strong>
Người 0 cao 5 và không có ai cao hơn hoặc bằng đứng phía trước.
Người 1 cao 7 và không có ai cao hơn hoặc bằng đứng phía trước.
Người 2 cao 5 và có hai người cao hơn hoặc bằng đứng phía trước: người 0 và người 1.
Người 3 cao 6 và có một người cao hơn hoặc bằng đứng phía trước: người 1.
Người 4 cao 4 và có bốn người cao hơn hoặc bằng đứng phía trước: người 0, 1, 2 và 3.
Người 5 cao 7 và có một người cao hơn hoặc bằng đứng phía trước: người 1.
Vì vậy, hàng đợi sau khi khôi phục là [[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> people = [[6,0],[5,0],[4,0],[3,2],[2,2],[1,4]]
<strong>Đầu ra:</strong> [[4,0],[5,0],[2,2],[3,2],[1,4],[6,0]]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= people.length &lt;= 2000</code></li>
	<li><code>0 &lt;= h<sub>i</sub> &lt;= 10<sup>6</sup></code></li>
	<li><code>0 &lt;= k<sub>i</sub> &lt; people.length</code></li>
	<li>Đảm bảo có thể khôi phục được hàng đợi.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi người $[h,k]$ cần có chính xác $k$ người cao ít nhất $h$ đứng trước. Nếu xếp người thấp trước, việc chèn người cao hơn sau đó có thể làm thay đổi số lượng này.
>
> Sắp xếp theo chiều cao giảm dần, rồi theo $k$ tăng dần; sau đó chèn mỗi người tại chỉ số $k$. Tất cả người đã có trong hàng đều cao hơn hoặc bằng người đang chèn, nên chỉ số đó đúng bằng số người cần đứng trước; các người đã xếp trước đó vẫn thỏa điều kiện.
>
> Người cao hơn được xếp vào vị trí trước; việc chèn người thấp hơn không ảnh hưởng đến họ.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def reconstructQueue(self, people: List[List[int]]) -> List[List[int]]:
        people.sort(key=lambda x: (-x[0], x[1]))
        ans = []
        for p in people:
            ans.insert(p[1], p)
        return ans
```

#### Java

```java
class Solution {
    public int[][] reconstructQueue(int[][] people) {
        Arrays.sort(people, (a, b) -> a[0] == b[0] ? a[1] - b[1] : b[0] - a[0]);
        List<int[]> ans = new ArrayList<>(people.length);
        for (int[] p : people) {
            ans.add(p[1], p);
        }
        return ans.toArray(new int[ans.size()][]);
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        sort(people.begin(), people.end(), [](const vector<int>& a, const vector<int>& b) {
            return a[0] > b[0] || (a[0] == b[0] && a[1] < b[1]);
        });
        vector<vector<int>> ans;
        for (const vector<int>& p : people)
            ans.insert(ans.begin() + p[1], p);
        return ans;
    }
};
```

#### Go

```go
func reconstructQueue(people [][]int) [][]int {
	sort.Slice(people, func(i, j int) bool {
		a, b := people[i], people[j]
		return a[0] > b[0] || a[0] == b[0] && a[1] < b[1]
	})
	var ans [][]int
	for _, p := range people {
		i := p[1]
		ans = append(ans[:i], append([][]int{p}, ans[i:]...)...)
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
