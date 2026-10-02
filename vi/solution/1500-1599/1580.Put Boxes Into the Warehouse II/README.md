---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [1580. Put Boxes Into the Warehouse II 🔒](https://leetcode.com/problems/put-boxes-into-the-warehouse-ii)

[中文文档](/solution/1500-1599/1580.Put%20Boxes%20Into%20the%20Warehouse%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên dương <code>boxes</code> và <code>warehouse</code>, lần lượt biểu diễn chiều cao của các hộp có chiều rộng bằng một đơn vị và chiều cao của <code>n</code> phòng trong một nhà kho. Các phòng trong nhà kho được đánh số từ <code>0</code> đến <code>n - 1</code> từ trái sang phải, trong đó <code>warehouse[i]</code> (đánh số từ 0) là chiều cao của phòng thứ <code>i<sup>th</sup></code>.</p>

<p>Các hộp được đưa vào nhà kho theo các quy tắc sau:</p>

<ul>
	<li>Không được xếp chồng các hộp.</li>
	<li>Có thể sắp xếp lại thứ tự đưa các hộp vào.</li>
	<li>Có thể đẩy hộp vào nhà kho từ <strong>một trong hai phía</strong> (trái hoặc phải).</li>
	<li>Nếu chiều cao của một phòng nào đó trong nhà kho nhỏ hơn chiều cao của một hộp, hộp đó và tất cả các hộp phía sau nó sẽ bị chặn trước phòng đó.</li>
</ul>

<p>Trả về <em>số hộp lớn nhất có thể đưa vào nhà kho</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1580.Put%20Boxes%20Into%20the%20Warehouse%20II/images/22.png" style="width: 401px; height: 202px;" />
<pre>
<strong>Đầu vào:</strong> boxes = [1,2,2,3,4], warehouse = [3,4,1,2]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1580.Put%20Boxes%20Into%20the%20Warehouse%20II/images/22-1.png" style="width: 240px; height: 202px;" />
Ta có thể đặt các hộp theo thứ tự sau:
1- Đưa hộp màu vàng vào phòng 2 từ phía trái hoặc phải.
2- Đưa hộp màu cam vào phòng 3 từ phía phải.
3- Đưa hộp màu xanh lá vào phòng 1 từ phía trái.
4- Đưa hộp màu đỏ vào phòng 0 từ phía trái.
Lưu ý rằng còn những cách hợp lệ khác để đặt 4 hộp, chẳng hạn đổi chỗ hộp đỏ và xanh lá hoặc hộp đỏ và cam.
</pre>

<p><strong class="example">Example 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1580.Put%20Boxes%20Into%20the%20Warehouse%20II/images/22-2.png" style="width: 401px; height: 242px;" />
<pre>
<strong>Đầu vào:</strong> boxes = [3,5,5,2], warehouse = [2,1,3,4,5]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1580.Put%20Boxes%20Into%20the%20Warehouse%20II/images/22-3.png" style="width: 280px; height: 242px;" />
Không thể đặt hai hộp có chiều cao 5 vào nhà kho vì chỉ có 1 phòng có chiều cao &gt;= 5.
Các cách hợp lệ khác là đặt hộp xanh lá vào phòng 2 hoặc đặt hộp cam vào phòng 2 trước khi đặt hộp xanh lá và đỏ.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == warehouse.length</code></li>
	<li><code>1 &lt;= boxes.length, warehouse.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= boxes[i], warehouse[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tiền xử lý + Sắp xếp + Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Hộp có thể đi vào từ cả hai đầu nhưng không thể vượt qua một phòng thấp hơn. Vì $n\le 10^5$, không thể thử mọi cách chọn đầu vào. Chiều cao sử dụng được của phòng $i$ bị giới hạn bởi phòng thấp nhất trên đường tiếp cận tốt hơn trong hai phía.
>
> Tính trước các giá trị nhỏ nhất tích lũy $left[i]$ và $right[i]$, rồi đặt sức chứa bằng $\min(warehouse[i],\max(left[i],right[i]))$. Sắp xếp các sức chứa cùng các hộp, sau đó ghép hộp nhỏ nhất với phòng thấp nhất nhưng vẫn đủ cao.

<!-- thinking:end -->

Trước tiên, ta tiền xử lý nhà kho để tìm chiều cao tối đa mà mỗi phòng có thể nhận. Sau đó, ta sắp xếp cả boxes và warehouse. Bắt đầu với hộp nhỏ nhất và phòng thấp nhất, nếu chiều cao phòng hiện tại lớn hơn hoặc bằng chiều cao hộp hiện tại thì ta đặt hộp vào phòng đó; nếu không, ta chuyển sang phòng tiếp theo.

Cuối cùng, ta trả về số hộp có thể đặt được.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của warehouse.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxBoxesInWarehouse(self, boxes: List[int], warehouse: List[int]) -> int:
        n = len(warehouse)
        left = [0] * n
        right = [0] * n
        left[0] = right[-1] = inf
        for i in range(1, n):
            left[i] = min(left[i - 1], warehouse[i - 1])
        for i in range(n - 2, -1, -1):
            right[i] = min(right[i + 1], warehouse[i + 1])
        for i in range(n):
            warehouse[i] = min(warehouse[i], max(left[i], right[i]))
        boxes.sort()
        warehouse.sort()
        ans = i = 0
        for x in boxes:
            while i < n and warehouse[i] < x:
                i += 1
            if i == n:
                break
            ans, i = ans + 1, i + 1
        return ans
```

#### Java

```java
class Solution {
    public int maxBoxesInWarehouse(int[] boxes, int[] warehouse) {
        int n = warehouse.length;
        int[] left = new int[n];
        int[] right = new int[n];
        final int inf = 1 << 30;
        left[0] = inf;
        right[n - 1] = inf;
        for (int i = 1; i < n; ++i) {
            left[i] = Math.min(left[i - 1], warehouse[i - 1]);
        }
        for (int i = n - 2; i >= 0; --i) {
            right[i] = Math.min(right[i + 1], warehouse[i + 1]);
        }
        for (int i = 0; i < n; ++i) {
            warehouse[i] = Math.min(warehouse[i], Math.max(left[i], right[i]));
        }
        Arrays.sort(boxes);
        Arrays.sort(warehouse);
        int ans = 0, i = 0;
        for (int x : boxes) {
            while (i < n && warehouse[i] < x) {
                ++i;
            }
            if (i == n) {
                break;
            }
            ++ans;
            ++i;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxBoxesInWarehouse(vector<int>& boxes, vector<int>& warehouse) {
        int n = warehouse.size();
        const int inf = 1 << 30;
        vector<int> left(n, inf);
        vector<int> right(n, inf);
        for (int i = 1; i < n; ++i) {
            left[i] = min(left[i - 1], warehouse[i - 1]);
        }
        for (int i = n - 2; ~i; --i) {
            right[i] = min(right[i + 1], warehouse[i + 1]);
        }
        for (int i = 0; i < n; ++i) {
            warehouse[i] = min(warehouse[i], max(left[i], right[i]));
        }
        sort(boxes.begin(), boxes.end());
        sort(warehouse.begin(), warehouse.end());
        int ans = 0;
        int i = 0;
        for (int x : boxes) {
            while (i < n && warehouse[i] < x) {
                ++i;
            }
            if (i == n) {
                break;
            }
            ++ans;
            ++i;
        }
        return ans;
    }
};
```

#### Go

```go
func maxBoxesInWarehouse(boxes []int, warehouse []int) (ans int) {
	n := len(warehouse)
	left := make([]int, n)
	right := make([]int, n)
	const inf = 1 << 30
	left[0] = inf
	right[n-1] = inf
	for i := 1; i < n; i++ {
		left[i] = min(left[i-1], warehouse[i-1])
	}
	for i := n - 2; i >= 0; i-- {
		right[i] = min(right[i+1], warehouse[i+1])
	}
	for i := 0; i < n; i++ {
		warehouse[i] = min(warehouse[i], max(left[i], right[i]))
	}
	sort.Ints(boxes)
	sort.Ints(warehouse)
	i := 0
	for _, x := range boxes {
		for i < n && warehouse[i] < x {
			i++
		}
		if i == n {
			break
		}
		ans++
		i++
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
