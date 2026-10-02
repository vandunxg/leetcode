---
comments: true
difficulty: Medium
rating: 1931
source: Biweekly Contest 34 Q3
tags:
    - Stack
    - Array
    - Two Pointers
    - Binary Search
    - Monotonic Stack
---

<!-- problem:start -->

# [1574. Shortest Subarray to be Removed to Make Array Sorted](https://leetcode.com/problems/shortest-subarray-to-be-removed-to-make-array-sorted)

[中文文档](/solution/1500-1599/1574.Shortest%20Subarray%20to%20be%20Removed%20to%20Make%20Array%20Sorted/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>arr</code>, hãy xóa một mảng con (có thể rỗng) khỏi <code>arr</code> sao cho các phần tử còn lại trong <code>arr</code> <strong>không giảm</strong>.</p>

<p>Trả về <em>độ dài mảng con ngắn nhất cần xóa</em>.</p>

<p><strong>Mảng con</strong> là một dãy con liên tiếp của mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1,2,3,10,4,2,3,5]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Mảng con ngắn nhất có thể xóa là [10,4,2], có độ dài 3. Các phần tử còn lại là [1,2,3,3,5], đã được sắp xếp.
Một đáp án đúng khác là xóa mảng con [3,10,4].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [5,4,3,2,1]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Vì mảng giảm nghiêm ngặt, ta chỉ có thể giữ lại một phần tử. Do đó cần xóa một mảng con độ dài 4, là [5,4,3,2] hoặc [4,3,2,1].
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1,2,3]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Mảng đã không giảm, nên không cần xóa phần tử nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= arr[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Xóa một mảng con liên tiếp để phần còn lại không giảm và độ dài phần xóa nhỏ nhất. Vì $n\le 10^5$, ta không thể thử mọi khoảng. Phần còn lại phải là một tiền tố và một hậu tố nối với nhau mà vẫn được sắp xếp.
>
> Tìm tiền tố không giảm dài nhất $[0,i]$ và hậu tố không giảm $[j,n)$. Nếu chúng đã phủ toàn bộ mảng thì đáp án là $0$; nếu không, ta có thể xóa toàn bộ hậu tố hoặc toàn bộ tiền tố. Với mỗi điểm cuối tiền tố $l$, dùng tìm kiếm nhị phân để tìm chỉ số hậu tố đầu tiên $r$ thỏa $arr[r]\ge arr[l]$, xóa $(l,r)$ và giữ phương án ngắn nhất.

<!-- thinking:end -->

Đầu tiên, ta tìm tiền tố không giảm dài nhất và hậu tố không giảm dài nhất của mảng, lần lượt ký hiệu là $\textit{nums}[0..i]$ và $\textit{nums}[j..n-1]$.

Nếu $i \geq j$, mảng đã không giảm nên ta trả về $0$.

Ngược lại, ta có thể xóa hậu tố bên phải hoặc tiền tố bên trái. Vì vậy ban đầu đáp án là $\min(n - i - 1, j)$.

Tiếp theo, ta duyệt điểm cuối $l$ của tiền tố bên trái. Với mỗi $l$, dùng tìm kiếm nhị phân để tìm vị trí đầu tiên lớn hơn hoặc bằng $\textit{nums}[l]$ trong $\textit{nums}[j..n-1]$, ký hiệu là $r$. Khi đó, ta có thể xóa $\textit{nums}[l+1..r-1]$ và cập nhật đáp án $\textit{ans} = \min(\textit{ans}, r - l - 1)$. Duyệt tiếp các $l$ để thu được đáp án cuối cùng.

Độ phức tạp thời gian là $O(n \times \log n)$, trong đó $n$ là độ dài mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findLengthOfShortestSubarray(self, arr: List[int]) -> int:
        n = len(arr)
        i, j = 0, n - 1
        while i + 1 < n and arr[i] <= arr[i + 1]:
            i += 1
        while j - 1 >= 0 and arr[j - 1] <= arr[j]:
            j -= 1
        if i >= j:
            return 0
        ans = min(n - i - 1, j)
        for l in range(i + 1):
            r = bisect_left(arr, arr[l], lo=j)
            ans = min(ans, r - l - 1)
        return ans
```

#### Java

```java
class Solution {
    public int findLengthOfShortestSubarray(int[] arr) {
        int n = arr.length;
        int i = 0, j = n - 1;
        while (i + 1 < n && arr[i] <= arr[i + 1]) {
            ++i;
        }
        while (j - 1 >= 0 && arr[j - 1] <= arr[j]) {
            --j;
        }
        if (i >= j) {
            return 0;
        }
        int ans = Math.min(n - i - 1, j);
        for (int l = 0; l <= i; ++l) {
            int r = search(arr, arr[l], j);
            ans = Math.min(ans, r - l - 1);
        }
        return ans;
    }

    private int search(int[] arr, int x, int left) {
        int right = arr.length;
        while (left < right) {
            int mid = (left + right) >> 1;
            if (arr[mid] >= x) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findLengthOfShortestSubarray(vector<int>& arr) {
        int n = arr.size();
        int i = 0, j = n - 1;
        while (i + 1 < n && arr[i] <= arr[i + 1]) {
            ++i;
        }
        while (j - 1 >= 0 && arr[j - 1] <= arr[j]) {
            --j;
        }
        if (i >= j) {
            return 0;
        }
        int ans = min(n - 1 - i, j);
        for (int l = 0; l <= i; ++l) {
            int r = lower_bound(arr.begin() + j, arr.end(), arr[l]) - arr.begin();
            ans = min(ans, r - l - 1);
        }
        return ans;
    }
};
```

#### Go

```go
func findLengthOfShortestSubarray(arr []int) int {
	n := len(arr)
	i, j := 0, n-1
	for i+1 < n && arr[i] <= arr[i+1] {
		i++
	}
	for j-1 >= 0 && arr[j-1] <= arr[j] {
		j--
	}
	if i >= j {
		return 0
	}
	ans := min(n-i-1, j)
	for l := 0; l <= i; l++ {
		r := j + sort.SearchInts(arr[j:], arr[l])
		ans = min(ans, r-l-1)
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 tìm kiếm nhị phân cho mỗi $l$ nên tốn thêm $\log n$. Hai phía đều đã được sắp xếp, vì vậy $r$ chỉ di chuyển sang phải khi $l$ tăng. Dùng con trỏ phải đơn điệu giúp vòng lặp thứ hai chạy tuyến tính.

<!-- thinking:end -->

Tương tự Lời giải 1, trước hết ta tìm tiền tố không giảm dài nhất và hậu tố không giảm dài nhất của mảng, lần lượt ký hiệu là $\textit{nums}[0..i]$ và $\textit{nums}[j..n-1]$.

Nếu $i \geq j$, mảng đã không giảm nên ta trả về $0$.

Ngược lại, ta có thể xóa hậu tố bên phải hoặc tiền tố bên trái. Vì vậy ban đầu đáp án là $\min(n - i - 1, j)$.

Tiếp theo, ta duyệt điểm cuối $l$ của tiền tố bên trái. Với mỗi $l$, dùng hai con trỏ để tìm trực tiếp vị trí đầu tiên lớn hơn hoặc bằng $\textit{nums}[l]$ trong $\textit{nums}[j..n-1]$, ký hiệu là $r$. Khi đó, ta có thể xóa $\textit{nums}[l+1..r-1]$ và cập nhật đáp án $\textit{ans} = \min(\textit{ans}, r - l - 1)$. Duyệt tiếp các $l$ để thu được đáp án cuối cùng.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findLengthOfShortestSubarray(self, arr: List[int]) -> int:
        n = len(arr)
        i, j = 0, n - 1
        while i + 1 < n and arr[i] <= arr[i + 1]:
            i += 1
        while j - 1 >= 0 and arr[j - 1] <= arr[j]:
            j -= 1
        if i >= j:
            return 0
        ans = min(n - i - 1, j)
        r = j
        for l in range(i + 1):
            while r < n and arr[r] < arr[l]:
                r += 1
            ans = min(ans, r - l - 1)
        return ans
```

#### Java

```java
class Solution {
    public int findLengthOfShortestSubarray(int[] arr) {
        int n = arr.length;
        int i = 0, j = n - 1;
        while (i + 1 < n && arr[i] <= arr[i + 1]) {
            ++i;
        }
        while (j - 1 >= 0 && arr[j - 1] <= arr[j]) {
            --j;
        }
        if (i >= j) {
            return 0;
        }
        int ans = Math.min(n - i - 1, j);
        for (int l = 0, r = j; l <= i; ++l) {
            while (r < n && arr[r] < arr[l]) {
                ++r;
            }
            ans = Math.min(ans, r - l - 1);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findLengthOfShortestSubarray(vector<int>& arr) {
        int n = arr.size();
        int i = 0, j = n - 1;
        while (i + 1 < n && arr[i] <= arr[i + 1]) {
            ++i;
        }
        while (j - 1 >= 0 && arr[j - 1] <= arr[j]) {
            --j;
        }
        if (i >= j) {
            return 0;
        }
        int ans = min(n - 1 - i, j);
        for (int l = 0, r = j; l <= i; ++l) {
            while (r < n && arr[r] < arr[l]) {
                ++r;
            }
            ans = min(ans, r - l - 1);
        }
        return ans;
    }
};
```

#### Go

```go
func findLengthOfShortestSubarray(arr []int) int {
	n := len(arr)
	i, j := 0, n-1
	for i+1 < n && arr[i] <= arr[i+1] {
		i++
	}
	for j-1 >= 0 && arr[j-1] <= arr[j] {
		j--
	}
	if i >= j {
		return 0
	}
	ans := min(n-i-1, j)
	r := j
	for l := 0; l <= i; l++ {
		for r < n && arr[r] < arr[l] {
			r += 1
		}
		ans = min(ans, r-l-1)
	}
	return ans
}
```

#### TypeScript

```ts
function findLengthOfShortestSubarray(arr: number[]): number {
    let [l, r, n] = [0, arr.length - 1, arr.length];

    while (r && arr[r - 1] <= arr[r]) r--;
    if (r === 0) return 0;

    let ans = r;
    while (l < r && (!l || arr[l - 1] <= arr[l])) {
        while (r < n && arr[l] > arr[r]) r++;
        ans = Math.min(ans, r - l - 1);
        l++;
    }

    return ans;
}
```

#### JavaScript

```js
function findLengthOfShortestSubarray(arr) {
    let [l, r, n] = [0, arr.length - 1, arr.length];

    while (r && arr[r - 1] <= arr[r]) r--;
    if (r === 0) return 0;

    let ans = r;
    while (l < r && (!l || arr[l - 1] <= arr[l])) {
        while (r < n && arr[l] > arr[r]) r++;
        ans = Math.min(ans, r - l - 1);
        l++;
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
