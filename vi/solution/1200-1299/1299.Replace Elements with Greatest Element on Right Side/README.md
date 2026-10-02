---
comments: true
difficulty: Easy
rating: 1219
source: Biweekly Contest 16 Q1
tags:
    - Array
---

<!-- problem:start -->

# [1299. Replace Elements with Greatest Element on Right Side](https://leetcode.com/problems/replace-elements-with-greatest-element-on-right-side)

[中文文档](/solution/1200-1299/1299.Replace%20Elements%20with%20Greatest%20Element%20on%20Right%20Side/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>arr</code>, hãy thay mỗi phần tử bằng phần tử lớn nhất nằm bên phải nó, đồng thời thay phần tử cuối cùng bằng <code>-1</code>.</p>

<p>Sau đó, trả về mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [17,18,5,4,6,1]
<strong>Đầu ra:</strong> [18,6,6,6,1,-1]
<strong>Giải thích:</strong> 
- chỉ số 0 --&gt; phần tử lớn nhất bên phải chỉ số 0 nằm ở chỉ số 1 (18).
- chỉ số 1 --&gt; phần tử lớn nhất bên phải chỉ số 1 nằm ở chỉ số 4 (6).
- chỉ số 2 --&gt; phần tử lớn nhất bên phải chỉ số 2 nằm ở chỉ số 4 (6).
- chỉ số 3 --&gt; phần tử lớn nhất bên phải chỉ số 3 nằm ở chỉ số 4 (6).
- chỉ số 4 --&gt; phần tử lớn nhất bên phải chỉ số 4 nằm ở chỉ số 5 (1).
- chỉ số 5 --&gt; không có phần tử nào bên phải chỉ số 5, nên ta đặt giá trị là -1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [400]
<strong>Đầu ra:</strong> [-1]
<strong>Giải thích:</strong> Không có phần tử nào bên phải chỉ số 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= arr[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt ngược

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi vị trí được thay bằng giá trị lớn nhất bên phải nó; vị trí cuối cùng nhận $-1$. Duyệt hậu tố từ trái sang sẽ lặp lại công việc. Thay vào đó, duyệt từ phải sang trái và giữ giá trị lớn nhất của hậu tố $mx$: ghi $mx$ cũ vào ô hiện tại rồi cập nhật $mx$ bằng giá trị ban đầu của ô. Chỉ cần một lượt duyệt ngược và $O(1)$ không gian phụ.

<!-- thinking:end -->

Ta dùng biến $mx$ để lưu giá trị lớn nhất bên phải vị trí hiện tại; ban đầu $mx = -1$.

Sau đó, duyệt mảng từ phải sang trái. Với mỗi vị trí $i$, gọi giá trị hiện tại là $x$, gán giá trị tại vị trí đó thành $mx$, rồi cập nhật $mx = \max(mx, x)$.

Cuối cùng, trả về mảng đã được cập nhật.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def replaceElements(self, arr: List[int]) -> List[int]:
        mx = -1
        for i in reversed(range(len(arr))):
            x = arr[i]
            arr[i] = mx
            mx = max(mx, x)
        return arr
```

#### Java

```java
class Solution {
    public int[] replaceElements(int[] arr) {
        for (int i = arr.length - 1, mx = -1; i >= 0; --i) {
            int x = arr[i];
            arr[i] = mx;
            mx = Math.max(mx, x);
        }
        return arr;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> replaceElements(vector<int>& arr) {
        for (int i = arr.size() - 1, mx = -1; ~i; --i) {
            int x = arr[i];
            arr[i] = mx;
            mx = max(mx, x);
        }
        return arr;
    }
};
```

#### Go

```go
func replaceElements(arr []int) []int {
	for i, mx := len(arr)-1, -1; i >= 0; i-- {
		x := arr[i]
		arr[i] = mx
		mx = max(mx, x)
	}
	return arr
}
```

#### TypeScript

```ts
function replaceElements(arr: number[]): number[] {
    for (let i = arr.length - 1, mx = -1; ~i; --i) {
        const x = arr[i];
        arr[i] = mx;
        mx = Math.max(mx, x);
    }
    return arr;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
