---
comments: true
difficulty: Medium
rating: 1402
source: Biweekly Contest 30 Q2
tags:
    - Array
    - Two Pointers
    - Binary Search
    - Prefix Sum
    - Sorting
---

<!-- problem:start -->

# [1508. Range Sum of Sorted Subarray Sums](https://leetcode.com/problems/range-sum-of-sorted-subarray-sums)

[中文文档](/solution/1500-1599/1508.Range%20Sum%20of%20Sorted%20Subarray%20Sums/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>nums</code> gồm <code>n</code> số nguyên dương. Ta tính tổng của mọi mảng con liên tiếp không rỗng trong mảng rồi sắp xếp chúng theo thứ tự không giảm, tạo thành một mảng mới gồm <code>n * (n + 1) / 2</code> số.</p>

<p><em>Hãy trả về tổng các số từ chỉ số </em><code>left</code><em> đến chỉ số </em><code>right</code> (<strong>đánh chỉ số từ 1</strong>)<em>, bao gồm cả hai đầu, trong mảng mới. </em>Vì đáp án có thể là một số rất lớn, hãy trả về phần dư của nó khi chia cho <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4], n = 4, left = 1, right = 5
<strong>Đầu ra:</strong> 13
<strong>Giải thích:</strong> Tổng của mọi mảng con là 1, 3, 6, 10, 2, 5, 9, 3, 7, 4. Sau khi sắp xếp theo thứ tự không giảm, ta được mảng mới [1, 2, 3, 3, 4, 5, 6, 7, 9, 10]. Tổng các số từ chỉ số le = 1 đến ri = 5 là 1 + 2 + 3 + 3 + 4 = 13.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4], n = 4, left = 3, right = 4
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Mảng đã cho giống Ví dụ 1. Ta có mảng mới [1, 2, 3, 3, 4, 5, 6, 7, 9, 10]. Tổng các số từ chỉ số le = 3 đến ri = 4 là 3 + 3 = 6.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4], n = 4, left = 1, right = 10
<strong>Đầu ra:</strong> 50
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
	<li><code>1 &lt;= left &lt;= right &lt;= n * (n + 1) / 2</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần sắp xếp tổng của mọi mảng con rồi cộng các phần tử từ chỉ số $\textit{left}$ đến $\textit{right}$. Có $O(n^2)$ mảng con và $n\le 10^3$. Việc tạo lại danh sách đó cho mỗi truy vấn sẽ lãng phí, nhưng ở đây chỉ có một truy vấn.
>
> Ta duyệt điểm bắt đầu bên trái và cộng dồn tổng khi mở rộng sang phải để liệt kê mọi tổng mảng con trong $O(n^2)$. Sau khi sắp xếp, cộng đoạn đóng được yêu cầu rồi lấy phần dư theo $10^9+7$. Việc sắp xếp trong $n^2\log n$ phù hợp với giới hạn.

<!-- thinking:end -->

Ta có thể tạo mảng $\textit{arr}$ theo yêu cầu của đề bài, sau đó sắp xếp mảng và cuối cùng tính tổng tất cả phần tử trong đoạn $[\textit{left}-1, \textit{right}-1]$ để nhận được kết quả.

Độ phức tạp thời gian là $O(n^2 \times \log n)$, còn độ phức tạp không gian là $O(n^2)$. Trong đó, $n$ là độ dài của mảng được cho trong đề bài.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def rangeSum(self, nums: List[int], n: int, left: int, right: int) -> int:
        arr = []
        for i in range(n):
            s = 0
            for j in range(i, n):
                s += nums[j]
                arr.append(s)
        arr.sort()
        mod = 10**9 + 7
        return sum(arr[left - 1 : right]) % mod
```

#### Java

```java
class Solution {
    public int rangeSum(int[] nums, int n, int left, int right) {
        int[] arr = new int[n * (n + 1) / 2];
        for (int i = 0, k = 0; i < n; ++i) {
            int s = 0;
            for (int j = i; j < n; ++j) {
                s += nums[j];
                arr[k++] = s;
            }
        }
        Arrays.sort(arr);
        int ans = 0;
        final int mod = (int) 1e9 + 7;
        for (int i = left - 1; i < right; ++i) {
            ans = (ans + arr[i]) % mod;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int rangeSum(vector<int>& nums, int n, int left, int right) {
        int arr[n * (n + 1) / 2];
        for (int i = 0, k = 0; i < n; ++i) {
            int s = 0;
            for (int j = i; j < n; ++j) {
                s += nums[j];
                arr[k++] = s;
            }
        }
        sort(arr, arr + n * (n + 1) / 2);
        int ans = 0;
        const int mod = 1e9 + 7;
        for (int i = left - 1; i < right; ++i) {
            ans = (ans + arr[i]) % mod;
        }
        return ans;
    }
};
```

#### Go

```go
func rangeSum(nums []int, n int, left int, right int) (ans int) {
	var arr []int
	for i := 0; i < n; i++ {
		s := 0
		for j := i; j < n; j++ {
			s += nums[j]
			arr = append(arr, s)
		}
	}
	sort.Ints(arr)
	const mod int = 1e9 + 7
	for _, x := range arr[left-1 : right] {
		ans = (ans + x) % mod
	}
	return
}
```

#### TypeScript

```ts
function rangeSum(nums: number[], n: number, left: number, right: number): number {
    let arr = Array((n * (n + 1)) / 2).fill(0);
    const mod = 10 ** 9 + 7;

    for (let i = 0, s = 0, k = 0; i < n; i++, s = 0) {
        for (let j = i; j < n; j++, k++) {
            s += nums[j];
            arr[k] = s;
        }
    }

    arr = arr.sort((a, b) => a - b).slice(left - 1, right);
    return arr.reduce((acc, cur) => (acc + cur) % mod, 0);
}
```

#### JavaScript

```js
function rangeSum(nums, n, left, right) {
    let arr = Array((n * (n + 1)) / 2).fill(0);
    const mod = 10 ** 9 + 7;

    for (let i = 0, s = 0, k = 0; i < n; i++, s = 0) {
        for (let j = i; j < n; j++, k++) {
            s += nums[j];
            arr[k] = s;
        }
    }

    arr = arr.sort((a, b) => a - b).slice(left - 1, right);
    return arr.reduce((acc, cur) => acc + cur, 0) % mod;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
