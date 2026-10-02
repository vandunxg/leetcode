---
comments: true
difficulty: Easy
rating: 1295
source: Biweekly Contest 32 Q1
tags:
    - Array
    - Binary Search
---

<!-- problem:start -->

# [1539. Kth Missing Positive Number](https://leetcode.com/problems/kth-missing-positive-number)

[中文文档](/solution/1500-1599/1539.Kth%20Missing%20Positive%20Number/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng <code>arr</code> gồm các số nguyên dương được sắp xếp theo <strong>thứ tự tăng nghiêm ngặt</strong>, và một số nguyên <code>k</code>.</p>

<p>Trả về <em>số nguyên</em> <code>k<sup>th</sup></code> <em><strong>dương</strong> bị <strong>thiếu</strong> khỏi mảng này.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [2,3,4,7,11], k = 5
<strong>Đầu ra:</strong> 9
<strong>Giải thích: </strong>Các số nguyên dương bị thiếu là [1,5,6,8,9,10,12,13,...]. Số nguyên dương bị thiếu thứ 5<sup>th</sup> là 9.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1,2,3,4], k = 2
<strong>Đầu ra:</strong> 6
<strong>Giải thích: </strong>Các số nguyên dương bị thiếu là [5,6,7,...]. Số nguyên dương bị thiếu thứ 2<sup>nd</sup> là 6.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 1000</code></li>
	<li><code>1 &lt;= arr[i] &lt;= 1000</code></li>
	<li><code>1 &lt;= k &lt;= 1000</code></li>
	<li><code>arr[i] &lt; arr[j]</code> for <code>1 &lt;= i &lt; j &lt;= arr.length</code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong></p>

<p>Bạn có thể giải bài toán với độ phức tạp nhỏ hơn O(n) không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mảng là một tập con đã sắp xếp của các số dương; ta cần số dương bị thiếu thứ $k$. Duyệt tuyến tính phù hợp khi $n$ và $k$ vừa phải, nhưng số lượng phần tử thiếu $arr[i]-i-1$ đơn điệu theo $i$, nên ta có thể tìm kiếm nhị phân.
>
> Nếu $arr[0]>k$ thì đáp án là $k$. Nếu không, tìm chỉ số đầu tiên có số lượng phần tử thiếu ít nhất là $k$. Ngay trước chỉ số đó, $arr[left-1]$ đã bỏ qua $arr[left-1]-(left-1)-1$ số dương; cộng thêm khoảng còn lại sẽ cho số bị thiếu thứ $k$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findKthPositive(self, arr: List[int], k: int) -> int:
        if arr[0] > k:
            return k
        left, right = 0, len(arr)
        while left < right:
            mid = (left + right) >> 1
            if arr[mid] - mid - 1 >= k:
                right = mid
            else:
                left = mid + 1
        return arr[left - 1] + k - (arr[left - 1] - (left - 1) - 1)
```

#### Java

```java
class Solution {
    public int findKthPositive(int[] arr, int k) {
        if (arr[0] > k) {
            return k;
        }
        int left = 0, right = arr.length;
        while (left < right) {
            int mid = (left + right) >> 1;
            if (arr[mid] - mid - 1 >= k) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return arr[left - 1] + k - (arr[left - 1] - (left - 1) - 1);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findKthPositive(vector<int>& arr, int k) {
        if (arr[0] > k) return k;
        int left = 0, right = arr.size();
        while (left < right) {
            int mid = (left + right) >> 1;
            if (arr[mid] - mid - 1 >= k)
                right = mid;
            else
                left = mid + 1;
        }
        return arr[left - 1] + k - (arr[left - 1] - (left - 1) - 1);
    }
};
```

#### Go

```go
func findKthPositive(arr []int, k int) int {
	if arr[0] > k {
		return k
	}
	left, right := 0, len(arr)
	for left < right {
		mid := (left + right) >> 1
		if arr[mid]-mid-1 >= k {
			right = mid
		} else {
			left = mid + 1
		}
	}
	return arr[left-1] + k - (arr[left-1] - (left - 1) - 1)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
