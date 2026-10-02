---
comments: true
difficulty: Easy
rating: 1558
source: Biweekly Contest 12 Q2
tags:
    - Array
    - Simulation
---

<!-- problem:start -->

# [1243. Array Transformation 🔒](https://leetcode.com/problems/array-transformation)

[中文文档](/solution/1200-1299/1243.Array%20Transformation/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng ban đầu <code>arr</code>. Mỗi ngày, bạn tạo một mảng mới dựa trên mảng của ngày trước đó.</p>

<p>Vào ngày thứ <code>i</code>, thực hiện các thao tác sau trên mảng của ngày&nbsp;<code>i-1</code>&nbsp;để tạo mảng của ngày <code>i</code>:</p>

<ol>
	<li>Nếu một phần tử nhỏ hơn cả phần tử bên trái lẫn phần tử bên phải, tăng giá trị của phần tử đó lên 1.</li>
	<li>Nếu một phần tử lớn hơn cả phần tử bên trái lẫn phần tử bên phải, giảm giá trị của phần tử đó đi 1.</li>
	<li>Phần tử đầu tiên&nbsp;và phần tử cuối cùng không bao giờ thay đổi.</li>
</ol>

<p>Sau một số ngày, mảng sẽ không còn thay đổi. Hãy trả về mảng cuối cùng đó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [6,2,3,4]
<strong>Đầu ra:</strong> [6,3,3,4]
<strong>Giải thích: </strong>
Trong ngày đầu tiên, mảng thay đổi từ [6,2,3,4] thành [6,3,3,4].
Không thể thực hiện thêm thao tác nào trên mảng này.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1,6,3,4,3,5]
<strong>Đầu ra:</strong> [1,4,4,4,4,5]
<strong>Giải thích: </strong>
Trong ngày đầu tiên, mảng thay đổi từ [1,6,3,4,3,5] thành [1,5,4,3,4,5].
Trong ngày thứ hai, mảng thay đổi từ [1,5,4,3,4,5] thành [1,4,4,4,4,5].
Không thể thực hiện thêm thao tác nào trên mảng này.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= arr.length &lt;= 100</code></li>
	<li><code>1 &lt;= arr[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Quy tắc chỉ xét các phần tử lân cận, còn hai đầu mảng không thay đổi. Cả $n$ và giá trị các phần tử đều không vượt quá $100$, nên có thể mô phỏng cho đến khi mảng ổn định. Khi cập nhật trong một ngày, phải đọc từ mảng cũ; nếu không, phần tử bên trái có thể dùng giá trị mới cập nhật của phần tử bên phải.
>
> Mỗi lượt tạo bản sao $t$ của mảng, cập nhật $arr$ dựa trên các phần tử lân cận trong $t$, rồi tiếp tục nếu có thay đổi. Cách mô phỏng này tương đương với việc cập nhật đồng thời.

<!-- thinking:end -->

Mô phỏng từng ngày. Với mỗi phần tử, nếu nó lớn hơn cả phần tử bên trái và bên phải thì giảm đi 1; nếu không thì tăng lên 1. Nếu đến một ngày nào đó mảng không còn thay đổi, trả về mảng đó.

Độ phức tạp thời gian là $O(n \times m)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài mảng và $m$ là giá trị lớn nhất trong mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def transformArray(self, arr: List[int]) -> List[int]:
        f = True
        while f:
            f = False
            t = arr[:]
            for i in range(1, len(t) - 1):
                if t[i] > t[i - 1] and t[i] > t[i + 1]:
                    arr[i] -= 1
                    f = True
                if t[i] < t[i - 1] and t[i] < t[i + 1]:
                    arr[i] += 1
                    f = True
        return arr
```

#### Java

```java
class Solution {
    public List<Integer> transformArray(int[] arr) {
        boolean f = true;
        while (f) {
            f = false;
            int[] t = arr.clone();
            for (int i = 1; i < t.length - 1; ++i) {
                if (t[i] > t[i - 1] && t[i] > t[i + 1]) {
                    --arr[i];
                    f = true;
                }
                if (t[i] < t[i - 1] && t[i] < t[i + 1]) {
                    ++arr[i];
                    f = true;
                }
            }
        }
        List<Integer> ans = new ArrayList<>();
        for (int x : arr) {
            ans.add(x);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> transformArray(vector<int>& arr) {
        bool f = true;
        while (f) {
            f = false;
            vector<int> t = arr;
            for (int i = 1; i < arr.size() - 1; ++i) {
                if (t[i] > t[i - 1] && t[i] > t[i + 1]) {
                    --arr[i];
                    f = true;
                }
                if (t[i] < t[i - 1] && t[i] < t[i + 1]) {
                    ++arr[i];
                    f = true;
                }
            }
        }
        return arr;
    }
};
```

#### Go

```go
func transformArray(arr []int) []int {
	f := true
	for f {
		f = false
		t := make([]int, len(arr))
		copy(t, arr)
		for i := 1; i < len(arr)-1; i++ {
			if t[i] > t[i-1] && t[i] > t[i+1] {
				arr[i]--
				f = true
			}
			if t[i] < t[i-1] && t[i] < t[i+1] {
				arr[i]++
				f = true
			}
		}
	}
	return arr
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
