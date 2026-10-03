---
comments: true
difficulty: Medium
tags:
    - Stack
    - Array
    - Monotonic Stack
---

<!-- problem:start -->

# [1762. Buildings With an Ocean View 🔒](https://leetcode.com/problems/buildings-with-an-ocean-view)

[中文文档](/solution/1700-1799/1762.Buildings%20With%20an%20Ocean%20View/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> tòa nhà nằm trên một hàng. Bạn được cho mảng số nguyên <code>heights</code> có kích thước <code>n</code>, biểu thị chiều cao các tòa nhà.</p>

<p>Đại dương nằm bên phải các tòa nhà. Một tòa nhà có tầm nhìn ra đại dương nếu có thể nhìn thấy đại dương mà không bị cản trở. Cụ thể, một tòa nhà có tầm nhìn nếu mọi tòa nhà bên phải nó đều có chiều cao <strong>nhỏ hơn</strong>.</p>

<p>Trả về danh sách chỉ số <strong>(đánh số từ 0)</strong> của các tòa nhà có tầm nhìn ra đại dương, theo thứ tự tăng dần.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> heights = [4,2,3,1]
<strong>Đầu ra:</strong> [0,2,3]
<strong>Giải thích:</strong> Tòa nhà 1 (đánh số từ 0) không có tầm nhìn vì tòa nhà 2 cao hơn.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> heights = [4,3,2,1]
<strong>Đầu ra:</strong> [0,1,2,3]
<strong>Giải thích:</strong> Tất cả các tòa nhà đều có tầm nhìn ra đại dương.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> heights = [1,3,2,4]
<strong>Đầu ra:</strong> [3]
<strong>Giải thích:</strong> Chỉ tòa nhà 3 có tầm nhìn ra đại dương.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= heights.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= heights[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt ngược để tìm giá trị lớn nhất bên phải

<!-- thinking:start -->

> **Tư duy**
>
> Một tòa nhà nhìn thấy đại dương khi và chỉ khi không có tòa nhà nào bên phải cao bằng hoặc cao hơn nó. Duyệt từ phải sang trái và duy trì giá trị lớn nhất bên phải sẽ giúp quyết định điều này.
>
> Nếu chiều cao lớn hơn $mx$, ghi nhận chỉ số và cập nhật $mx$. Đảo ngược các chỉ số đã thu thập để đưa về thứ tự từ trái sang phải.

<!-- thinking:end -->

Ta duyệt mảng $\textit{height}$ theo thứ tự ngược. Với mỗi phần tử $v$, ta so sánh $v$ với phần tử lớn nhất $mx$ bên phải. Nếu $mx \lt v$, mọi phần tử bên phải đều nhỏ hơn phần tử hiện tại, nên vị trí hiện tại có thể nhìn thấy đại dương và được thêm vào mảng kết quả $\textit{ans}$. Sau đó cập nhật $mx$ thành $v$.

Sau khi duyệt, trả về $\textit{ans}$ theo thứ tự ngược lại.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng. Không tính phần không gian của mảng kết quả, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findBuildings(self, heights: List[int]) -> List[int]:
        ans = []
        mx = 0
        for i in range(len(heights) - 1, -1, -1):
            if heights[i] > mx:
                ans.append(i)
                mx = heights[i]
        return ans[::-1]
```

#### Java

```java
class Solution {
    public int[] findBuildings(int[] heights) {
        int n = heights.length;
        List<Integer> ans = new ArrayList<>();
        int mx = 0;
        for (int i = heights.length - 1; i >= 0; --i) {
            if (heights[i] > mx) {
                ans.add(i);
                mx = heights[i];
            }
        }
        Collections.reverse(ans);
        return ans.stream().mapToInt(Integer::intValue).toArray();
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> findBuildings(vector<int>& heights) {
        vector<int> ans;
        int mx = 0;
        for (int i = heights.size() - 1; ~i; --i) {
            if (heights[i] > mx) {
                ans.push_back(i);
                mx = heights[i];
            }
        }
        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```

#### Go

```go
func findBuildings(heights []int) (ans []int) {
	mx := 0
	for i := len(heights) - 1; i >= 0; i-- {
		if v := heights[i]; v > mx {
			ans = append(ans, i)
			mx = v
		}
	}
	for i, j := 0, len(ans)-1; i < j; i, j = i+1, j-1 {
		ans[i], ans[j] = ans[j], ans[i]
	}
	return
}
```

#### TypeScript

```ts
function findBuildings(heights: number[]): number[] {
    const ans: number[] = [];
    let mx = 0;
    for (let i = heights.length - 1; ~i; --i) {
        if (heights[i] > mx) {
            ans.push(i);
            mx = heights[i];
        }
    }
    return ans.reverse();
}
```

#### JavaScript

```js
/**
 * @param {number[]} heights
 * @return {number[]}
 */
var findBuildings = function (heights) {
    const ans = [];
    let mx = 0;
    for (let i = heights.length - 1; ~i; --i) {
        if (heights[i] > mx) {
            ans.push(i);
            mx = heights[i];
        }
    }
    return ans.reverse();
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
