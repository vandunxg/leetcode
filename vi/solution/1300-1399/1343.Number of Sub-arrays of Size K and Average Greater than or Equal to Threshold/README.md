---
comments: true
difficulty: Medium
rating: 1317
source: Biweekly Contest 19 Q2
tags:
    - Array
    - Sliding Window
---

<!-- problem:start -->

# [1343. Number of Sub-arrays of Size K and Average Greater than or Equal to Threshold](https://leetcode.com/problems/number-of-sub-arrays-of-size-k-and-average-greater-than-or-equal-to-threshold)

[中文文档](/solution/1300-1399/1343.Number%20of%20Sub-arrays%20of%20Size%20K%20and%20Average%20Greater%20than%20or%20Equal%20to%20Threshold/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>arr</code> và hai số nguyên <code>k</code>, <code>threshold</code>. Hãy trả về <em>số mảng con có kích thước </em><code>k</code><em> và giá trị trung bình lớn hơn hoặc bằng </em><code>threshold</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [2,2,2,2,5,5,5,8], k = 3, threshold = 4
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Các mảng con [2,5,5], [5,5,5] và [5,5,8] lần lượt có giá trị trung bình là 4, 5 và 6. Các mảng con kích thước 3 còn lại đều có giá trị trung bình nhỏ hơn 4 (ngưỡng).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [11,13,17,23,29,31,7,5,2,3], k = 3, threshold = 5
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> 6 mảng con kích thước 3 đầu tiên có giá trị trung bình lớn hơn 5. Lưu ý rằng giá trị trung bình không nhất thiết là số nguyên.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= arr[i] &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= k &lt;= arr.length</code></li>
	<li><code>0 &lt;= threshold &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sliding window

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các cửa sổ độ dài $k$ có giá trị trung bình ít nhất là $\textit{threshold}$. Vì $n \le 10^5$, không thể tính lại tổng cho từng cửa sổ. Điều kiện về giá trị trung bình tương đương so sánh tổng với $k \times \textit{threshold}$. Tổng của sliding window độ dài $k$ được cập nhật trong $O(1)$, cho phép đếm các cửa sổ thỏa điều kiện chỉ trong một lượt duyệt.

<!-- thinking:end -->

Ta có thể nhân `threshold` với $k$ để so sánh trực tiếp tổng trong cửa sổ với `threshold` đã nhân.

Ta duy trì một sliding window độ dài $k$ và tính tổng $s$ của từng cửa sổ. Nếu $s$ lớn hơn hoặc bằng `threshold`, ta tăng đáp án lên 1.

Độ phức tạp thời gian là $O(n)$, với $n$ là độ dài của mảng `arr`. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numOfSubarrays(self, arr: List[int], k: int, threshold: int) -> int:
        threshold *= k
        s = sum(arr[:k])
        ans = int(s >= threshold)
        for i in range(k, len(arr)):
            s += arr[i] - arr[i - k]
            ans += int(s >= threshold)
        return ans
```

#### Java

```java
class Solution {
    public int numOfSubarrays(int[] arr, int k, int threshold) {
        threshold *= k;
        int s = 0;
        for (int i = 0; i < k; ++i) {
            s += arr[i];
        }
        int ans = s >= threshold ? 1 : 0;
        for (int i = k; i < arr.length; ++i) {
            s += arr[i] - arr[i - k];
            ans += s >= threshold ? 1 : 0;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numOfSubarrays(vector<int>& arr, int k, int threshold) {
        threshold *= k;
        int s = accumulate(arr.begin(), arr.begin() + k, 0);
        int ans = s >= threshold;
        for (int i = k; i < arr.size(); ++i) {
            s += arr[i] - arr[i - k];
            ans += s >= threshold;
        }
        return ans;
    }
};
```

#### Go

```go
func numOfSubarrays(arr []int, k int, threshold int) (ans int) {
	threshold *= k
	s := 0
	for _, x := range arr[:k] {
		s += x
	}
	if s >= threshold {
		ans++
	}
	for i := k; i < len(arr); i++ {
		s += arr[i] - arr[i-k]
		if s >= threshold {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function numOfSubarrays(arr: number[], k: number, threshold: number): number {
    threshold *= k;
    let s = arr.slice(0, k).reduce((acc, cur) => acc + cur, 0);
    let ans = s >= threshold ? 1 : 0;
    for (let i = k; i < arr.length; ++i) {
        s += arr[i] - arr[i - k];
        ans += s >= threshold ? 1 : 0;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
