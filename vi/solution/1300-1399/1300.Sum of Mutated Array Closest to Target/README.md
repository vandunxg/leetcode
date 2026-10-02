---
comments: true
difficulty: Medium
rating: 1606
source: Biweekly Contest 16 Q2
tags:
    - Array
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [1300. Sum of Mutated Array Closest to Target](https://leetcode.com/problems/sum-of-mutated-array-closest-to-target)

[中文文档](/solution/1300-1399/1300.Sum%20of%20Mutated%20Array%20Closest%20to%20Target/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>arr</code> và giá trị đích <code>target</code>. Hãy trả về số nguyên <code>value</code> sao cho khi thay mọi số trong mảng lớn hơn <code>value</code> bằng chính <code>value</code>, tổng mảng gần <code>target</code> nhất (theo độ chênh lệch tuyệt đối).</p>

<p>Nếu có nhiều giá trị cùng đạt độ chênh lệch nhỏ nhất, trả về giá trị nhỏ nhất.</p>

<p>Lưu ý đáp án không nhất thiết phải là một số có trong <code>arr</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [4,9,3], target = 10
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Khi chọn 3, arr trở thành [3, 3, 3], có tổng bằng 9; đây là đáp án tối ưu.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [2,3,5], target = 10
<strong>Đầu ra:</strong> 5
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [60864,25176,27249,21296,20204], target = 56803
<strong>Đầu ra:</strong> 11361
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= arr[i], target &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sorting + Prefix Sum + Binary Search + Enumeration

<!-- thinking:start -->

> **Tư duy**
>
> Quét toàn bộ mảng cho mỗi ứng viên $\textit{value}$ tốn $O(n \times M)$. Với $n \le 10^4$ và $M \le 10^5$, cách này quá chậm.
>
> Tổng sau khi biến đổi chỉ phụ thuộc vào các giá trị không vượt quá $\textit{value}$ và việc thay các giá trị còn lại bằng $\textit{value}$. Sau khi sắp xếp, phần được giữ lại tạo thành một prefix: binary search tìm ranh giới, còn mảng prefix sum tính tổng phần này. Duyệt $\textit{value}$ từ $0$ đến $\max(\textit{arr})$ tốn $O(\log n)$ cho mỗi ứng viên; ta chọn giá trị có tổng sau biến đổi gần $\textit{target}$ nhất, và nếu hòa thì chọn giá trị nhỏ hơn.

<!-- thinking:end -->

Ta nhận thấy bài toán yêu cầu thay mọi giá trị lớn hơn `value` bằng `value`, rồi tính tổng. Vì vậy, trước tiên ta sắp xếp mảng `arr` và tính mảng prefix sum $s$, trong đó $s[i]$ là tổng của $i$ phần tử đầu tiên.

Tiếp theo, ta duyệt các giá trị `value` từ nhỏ đến lớn. Với mỗi `value`, dùng binary search tìm chỉ số $i$ của phần tử đầu tiên trong mảng lớn hơn `value`. Khi đó, có $n - i$ phần tử lớn hơn `value` và $i$ phần tử nhỏ hơn hoặc bằng `value`. Tổng các phần tử nhỏ hơn hoặc bằng `value` là $s[i]$, còn tổng các phần tử lớn hơn `value` sau khi thay thế là $(n - i) \times value$. Vì vậy, tổng mảng sau biến đổi là $s[i] + (n - i) \times \textit{value}$. Nếu độ chênh lệch tuyệt đối giữa tổng này và `target` nhỏ hơn độ chênh lệch nhỏ nhất hiện tại `diff`, cập nhật `diff` và `ans`.

Sau khi duyệt hết các giá trị `value`, ta thu được đáp án cuối cùng `ans`.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài mảng `arr`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findBestValue(self, arr: List[int], target: int) -> int:
        arr.sort()
        s = list(accumulate(arr, initial=0))
        ans, diff = 0, inf
        for value in range(max(arr) + 1):
            i = bisect_right(arr, value)
            d = abs(s[i] + (len(arr) - i) * value - target)
            if diff > d:
                diff = d
                ans = value
        return ans
```

#### Java

```java
class Solution {
    public int findBestValue(int[] arr, int target) {
        Arrays.sort(arr);
        int n = arr.length;
        int[] s = new int[n + 1];
        int mx = 0;
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + arr[i];
            mx = Math.max(mx, arr[i]);
        }
        int ans = 0, diff = 1 << 30;
        for (int value = 0; value <= mx; ++value) {
            int i = search(arr, value);
            int d = Math.abs(s[i] + (n - i) * value - target);
            if (diff > d) {
                diff = d;
                ans = value;
            }
        }
        return ans;
    }

    private int search(int[] arr, int x) {
        int left = 0, right = arr.length;
        while (left < right) {
            int mid = (left + right) >> 1;
            if (arr[mid] > x) {
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
    int findBestValue(vector<int>& arr, int target) {
        sort(arr.begin(), arr.end());
        int n = arr.size();
        int s[n + 1];
        s[0] = 0;
        int mx = 0;
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + arr[i];
            mx = max(mx, arr[i]);
        }
        int ans = 0, diff = 1 << 30;
        for (int value = 0; value <= mx; ++value) {
            int i = upper_bound(arr.begin(), arr.end(), value) - arr.begin();
            int d = abs(s[i] + (n - i) * value - target);
            if (diff > d) {
                diff = d;
                ans = value;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findBestValue(arr []int, target int) (ans int) {
	sort.Ints(arr)
	n := len(arr)
	s := make([]int, n+1)
	mx := slices.Max(arr)
	for i, x := range arr {
		s[i+1] = s[i] + x
	}
	diff := 1 << 30
	for value := 0; value <= mx; value++ {
		i := sort.SearchInts(arr, value+1)
		d := abs(s[i] + (n-i)*value - target)
		if diff > d {
			diff = d
			ans = value
		}
	}
	return
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
