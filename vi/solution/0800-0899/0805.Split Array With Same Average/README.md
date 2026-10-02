---
comments: true
difficulty: Hard
tags:
    - Bit Manipulation
    - Array
    - Hash Table
    - Math
    - Dynamic Programming
    - Bitmask
    - Meet in the Middle
---

<!-- problem:start -->

# [805. Split Array With Same Average](https://leetcode.com/problems/split-array-with-same-average)

[中文文档](/solution/0800-0899/0805.Split%20Array%20With%20Same%20Average/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code>.</p>

<p>Hãy đưa mỗi phần tử của <code>nums</code> vào một trong hai mảng <code>A</code> và <code>B</code> sao cho cả <code>A</code> và <code>B</code> đều không rỗng, đồng thời <code>average(A) == average(B)</code>.</p>

<p>Trả về <code>true</code> nếu có thể thực hiện được, ngược lại trả về <code>false</code>.</p>

<p><strong>Lưu ý</strong>, với mảng <code>arr</code>, <code>average(arr)</code> bằng tổng tất cả phần tử trong <code>arr</code> chia cho độ dài của <code>arr</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4,5,6,7,8]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Ta có thể chia mảng thành [1,4,5,8] và [2,3,6,7]; cả hai mảng đều có giá trị trung bình bằng 4.5.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,1]
<strong>Đầu ra:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 30</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Binary Search + Liệt kê nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Chia thành hai phần có cùng trung bình tương đương tìm một tập con không rỗng, không phải toàn bộ mảng, có trung bình bằng phần còn lại. Vì $n\le 30$, duyệt hết $2^n$ trường hợp khá tốn kém, nhưng mỗi nửa chỉ có khoảng $15$ phần tử nên có thể dùng meet-in-the-middle.
>
> Thay $a_i$ bằng $n\cdot a_i-S$ để bài toán trở thành tìm một tập con không rỗng, không phải toàn bộ mảng, có tổng bằng $0$. Lưu các tổng của nửa trái; nếu nửa phải có tổng bằng $0$ hoặc có tổng đối ứng đã xuất hiện (loại trừ trường hợp lấy cả hai nửa), thì đáp án là true.

<!-- thinking:end -->

Theo yêu cầu đề bài, ta cần xác định liệu có thể chia mảng $\textit{nums}$ thành hai mảng $A$ và $B$ sao cho giá trị trung bình của chúng bằng nhau hay không.

Gọi tổng các phần tử của mảng $\textit{nums}$ là $s$ và số phần tử là $n$. Giả sử tổng và số phần tử của mảng $A$ lần lượt là $s_1$ và $k$. Khi đó, tổng của mảng $B$ là $s_2 = s - s_1$ và số phần tử là $n - k$. Ta có:

$$
\frac{s_1}{k} = \frac{s_2}{n - k} = \frac{s-s_1}{n-k}
$$

Biến đổi biểu thức, ta được:

$$
s_1 \times (n-k) = (s-s_1) \times k
$$

Rút gọn, ta được:

$$
\frac{s_1}{k} = \frac{s}{n}
$$

Điều này có nghĩa là ta cần tìm tập con $A$ có giá trị trung bình bằng giá trị trung bình của toàn bộ mảng $\textit{nums}$. Nếu trừ giá trị trung bình của $\textit{nums}$ khỏi từng phần tử, bài toán sẽ trở thành tìm một tập con có tổng bằng $0$.

Tuy nhiên, giá trị trung bình của $\textit{nums}$ có thể không phải số nguyên, và phép tính số thực có thể gặp sai số. Ta có thể nhân mỗi phần tử của $\textit{nums}$ với $n$, tức là $nums[i] \leftarrow nums[i] \times n$. Khi đó, phương trình trên trở thành:

$$
\frac{s_1\times n}{k} = s
$$

Tiếp theo, trừ số nguyên $s$ khỏi mỗi phần tử của $\textit{nums}$. Bài toán được chuyển thành tìm tập con $A$ trong $nums$ có tổng bằng $0$.

Độ dài của mảng $\textit{nums}$ nằm trong khoảng $[1, 30]$. Nếu dùng brute force để liệt kê các tập con, độ phức tạp thời gian là $O(2^n)$ và sẽ quá thời gian. Ta có thể dùng binary search để giảm độ phức tạp thời gian xuống $O(2^{n/2})$.

Chia mảng $\textit{nums}$ thành nửa trái và nửa phải. Tập con $A$ có thể thuộc một trong ba trường hợp:

1. Tập con $A$ chỉ nằm trong nửa trái của $\textit{nums}$;
2. Tập con $A$ chỉ nằm trong nửa phải của $\textit{nums}$;
3. Tập con $A$ gồm một phần của nửa trái và một phần của nửa phải của $\textit{nums}$.

Ta có thể dùng binary enumeration để liệt kê tổng của mọi tập con ở nửa trái. Nếu có tập con tổng bằng $0$, lập tức trả về `true`; nếu không, lưu các tổng vào hash table $\textit{vis}$. Tiếp theo, liệt kê tổng các tập con ở nửa phải. Nếu có tổng bằng $0$, lập tức trả về `true`; nếu không, kiểm tra xem $\textit{vis}$ có chứa số đối của tổng hiện tại hay không. Nếu có thì trả về `true`.

Lưu ý, không được đồng thời chọn toàn bộ phần tử ở cả nửa trái lẫn nửa phải, vì như vậy mảng $B$ sẽ rỗng và không thỏa mãn yêu cầu đề bài. Khi cài đặt, chỉ cần xét $n-1$ phần tử của mảng.

Độ phức tạp thời gian là $O(n \times 2^{\frac{n}{2}})$ và độ phức tạp không gian là $O(2^{\frac{n}{2}})$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def splitArraySameAverage(self, nums: List[int]) -> bool:
        n = len(nums)
        if n == 1:
            return False
        s = sum(nums)
        for i, v in enumerate(nums):
            nums[i] = v * n - s
        m = n >> 1
        vis = set()
        for i in range(1, 1 << m):
            t = sum(v for j, v in enumerate(nums[:m]) if i >> j & 1)
            if t == 0:
                return True
            vis.add(t)
        for i in range(1, 1 << (n - m)):
            t = sum(v for j, v in enumerate(nums[m:]) if i >> j & 1)
            if t == 0 or (i != (1 << (n - m)) - 1 and -t in vis):
                return True
        return False
```

#### Java

```java
class Solution {
    public boolean splitArraySameAverage(int[] nums) {
        int n = nums.length;
        if (n == 1) {
            return false;
        }
        int s = Arrays.stream(nums).sum();
        for (int i = 0; i < n; ++i) {
            nums[i] = nums[i] * n - s;
        }
        int m = n >> 1;
        Set<Integer> vis = new HashSet<>();
        for (int i = 1; i < 1 << m; ++i) {
            int t = 0;
            for (int j = 0; j < m; ++j) {
                if (((i >> j) & 1) == 1) {
                    t += nums[j];
                }
            }
            if (t == 0) {
                return true;
            }
            vis.add(t);
        }
        for (int i = 1; i < 1 << (n - m); ++i) {
            int t = 0;
            for (int j = 0; j < (n - m); ++j) {
                if (((i >> j) & 1) == 1) {
                    t += nums[m + j];
                }
            }
            if (t == 0 || (i != (1 << (n - m)) - 1) && vis.contains(-t)) {
                return true;
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool splitArraySameAverage(vector<int>& nums) {
        int n = nums.size();
        if (n == 1) return false;
        int s = accumulate(nums.begin(), nums.end(), 0);
        for (int& v : nums) v = v * n - s;
        int m = n >> 1;
        unordered_set<int> vis;
        for (int i = 1; i < 1 << m; ++i) {
            int t = 0;
            for (int j = 0; j < m; ++j)
                if (i >> j & 1) t += nums[j];
            if (t == 0) return true;
            vis.insert(t);
        }
        for (int i = 1; i < 1 << (n - m); ++i) {
            int t = 0;
            for (int j = 0; j < (n - m); ++j)
                if (i >> j & 1) t += nums[m + j];
            if (t == 0 || (i != (1 << (n - m)) - 1 && vis.count(-t))) return true;
        }
        return false;
    }
};
```

#### Go

```go
func splitArraySameAverage(nums []int) bool {
	n := len(nums)
	if n == 1 {
		return false
	}
	s := 0
	for _, v := range nums {
		s += v
	}
	for i, v := range nums {
		nums[i] = v*n - s
	}
	m := n >> 1
	vis := map[int]bool{}
	for i := 1; i < 1<<m; i++ {
		t := 0
		for j, v := range nums[:m] {
			if (i >> j & 1) == 1 {
				t += v
			}
		}
		if t == 0 {
			return true
		}
		vis[t] = true
	}
	for i := 1; i < 1<<(n-m); i++ {
		t := 0
		for j, v := range nums[m:] {
			if (i >> j & 1) == 1 {
				t += v
			}
		}
		if t == 0 || (i != (1<<(n-m))-1 && vis[-t]) {
			return true
		}
	}
	return false
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
