---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [1564. Put Boxes Into the Warehouse I 🔒](https://leetcode.com/problems/put-boxes-into-the-warehouse-i)

[中文文档](/solution/1500-1599/1564.Put%20Boxes%20Into%20the%20Warehouse%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên dương <code>boxes</code> và <code>warehouse</code>, lần lượt biểu diễn chiều cao của các hộp có chiều rộng đơn vị và chiều cao của <code>n</code> phòng trong nhà kho. Các phòng được đánh số từ <code>0</code> đến <code>n - 1</code> từ trái sang phải, trong đó <code>warehouse[i]</code> (đánh chỉ số từ 0) là chiều cao của phòng thứ <code>i<sup>th</sup></code>.</p>

<p>Đưa các hộp vào nhà kho theo các quy tắc sau:</p>

<ul>
	<li>Không được xếp chồng các hộp.</li>
	<li>Có thể sắp xếp lại thứ tự đưa hộp vào.</li>
	<li>Chỉ được đẩy hộp vào nhà kho theo hướng từ trái sang phải.</li>
	<li>Nếu một phòng có chiều cao nhỏ hơn chiều cao của hộp, hộp đó và mọi hộp phía sau sẽ bị chặn trước phòng đó.</li>
</ul>

<p>Trả về <em>số hộp lớn nhất có thể đưa vào nhà kho.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1564.Put%20Boxes%20Into%20the%20Warehouse%20I/images/11.png" style="width: 400px; height: 242px;" />
<pre>
<strong>Input:</strong> boxes = [4,3,4,1], warehouse = [5,3,3,4,1]
<strong>Output:</strong> 3
<strong>Giải thích:</strong> 
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1564.Put%20Boxes%20Into%20the%20Warehouse%20I/images/12.png" style="width: 280px; height: 242px;" />
Trước tiên có thể đặt hộp chiều cao 1 vào phòng 4. Sau đó đặt hộp chiều cao 3 vào một trong ba phòng 1, 2 hoặc 3. Cuối cùng đặt một hộp chiều cao 4 vào phòng 0.
Không có cách nào đặt cả 4 hộp vào nhà kho.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1564.Put%20Boxes%20Into%20the%20Warehouse%20I/images/21.png" style="width: 400px; height: 202px;" />
<pre>
<strong>Input:</strong> boxes = [1,2,2,3,4], warehouse = [3,4,1,2]
<strong>Output:</strong> 3
<strong>Explanation:</strong> 
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1564.Put%20Boxes%20Into%20the%20Warehouse%20I/images/22.png" style="width: 280px; height: 202px;" />
Notice that it&#39;s not possible to put the box of height 4 into the warehouse since it cannot pass the first room of height 3.
Also, for the last two rooms, 2 and 3, only boxes of height 1 can fit.
We can fit 3 boxes maximum as shown above. The yellow box can also be put in room 2 instead.
Swapping the orange and green boxes is also valid, or swapping one of them with the red box.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> boxes = [1,2,3], warehouse = [1,2,3,4]
<strong>Output:</strong> 1
<strong>Explanation:</strong> Since the first room in the warehouse is of height 1, we can only put boxes of height 1.
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

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Hộp chỉ được đẩy từ bên trái vào và không thể đi qua phòng thấp hơn. Vì $n\le 10^5$, không thể thử mọi hoán vị. Phòng $i$ bị giới hạn bởi phòng thấp nhất ở bên trái nó.
>
> Tính trước $left[i]=\min(warehouse[0..i])$ rồi sắp xếp các hộp. Hộp nhỏ đi sâu nhất có thể: con trỏ phải đi sang trái đến $left[j]\ge boxes[i]$, sau đó tăng cả hai con trỏ. Các phòng còn lại cao hơn vẫn dành cho hộp lớn hơn.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxBoxesInWarehouse(self, boxes: List[int], warehouse: List[int]) -> int:
        n = len(warehouse)
        left = [warehouse[0]] * n
        for i in range(1, n):
            left[i] = min(left[i - 1], warehouse[i])
        boxes.sort()
        i, j = 0, n - 1
        while i < len(boxes):
            while j >= 0 and left[j] < boxes[i]:
                j -= 1
            if j < 0:
                break
            i, j = i + 1, j - 1
        return i
```

#### Java

```java
class Solution {
    public int maxBoxesInWarehouse(int[] boxes, int[] warehouse) {
        int n = warehouse.length;
        int[] left = new int[n];
        left[0] = warehouse[0];
        for (int i = 1; i < n; ++i) {
            left[i] = Math.min(left[i - 1], warehouse[i]);
        }
        Arrays.sort(boxes);
        int i = 0, j = n - 1;
        while (i < boxes.length) {
            while (j >= 0 && left[j] < boxes[i]) {
                --j;
            }
            if (j < 0) {
                break;
            }
            ++i;
            --j;
        }
        return i;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxBoxesInWarehouse(vector<int>& boxes, vector<int>& warehouse) {
        int n = warehouse.size();
        int left[n];
        left[0] = warehouse[0];
        for (int i = 1; i < n; ++i) {
            left[i] = min(left[i - 1], warehouse[i]);
        }
        sort(boxes.begin(), boxes.end());
        int i = 0, j = n - 1;
        while (i < boxes.size()) {
            while (j >= 0 && left[j] < boxes[i]) {
                --j;
            }
            if (j < 0) {
                break;
            }
            ++i;
            --j;
        }
        return i;
    }
};
```

#### Go

```go
func maxBoxesInWarehouse(boxes []int, warehouse []int) int {
	n := len(warehouse)
	left := make([]int, n)
	left[0] = warehouse[0]
	for i := 1; i < n; i++ {
		left[i] = min(left[i-1], warehouse[i])
	}
	sort.Ints(boxes)
	i, j := 0, n-1
	for i < len(boxes) {
		for j >= 0 && left[j] < boxes[i] {
			j--
		}
		if j < 0 {
			break
		}
		i, j = i+1, j-1
	}
	return i
}
```

#### TypeScript

```ts
function maxBoxesInWarehouse(boxes: number[], warehouse: number[]): number {
    const n = warehouse.length;
    const left: number[] = new Array(n);
    left[0] = warehouse[0];
    for (let i = 1; i < n; ++i) {
        left[i] = Math.min(left[i - 1], warehouse[i]);
    }
    boxes.sort((a, b) => a - b);
    let i = 0;
    let j = n - 1;
    while (i < boxes.length) {
        while (j >= 0 && left[j] < boxes[i]) {
            --j;
        }
        if (j < 0) {
            break;
        }
        ++i;
        --j;
    }
    return i;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
