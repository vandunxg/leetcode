---
comments: true
difficulty: Hard
rating: 2466
source: Biweekly Contest 42 Q4
tags:
    - Greedy
    - Array
    - Prefix Sum
    - Sliding Window
---

<!-- problem:start -->

# [1703. Minimum Adjacent Swaps for K Consecutive Ones](https://leetcode.com/problems/minimum-adjacent-swaps-for-k-consecutive-ones)

[中文文档](/solution/1700-1799/1703.Minimum%20Adjacent%20Swaps%20for%20K%20Consecutive%20Ones/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> và số nguyên <code>k</code>. <code>nums</code> chỉ gồm <code>0</code> và <code>1</code>. Trong một phép biến đổi, bạn có thể chọn hai chỉ số <strong>liền kề</strong> và đổi chỗ các giá trị của chúng.</p>

<p>Hãy trả về <em>số phép biến đổi <strong>nhỏ nhất</strong> cần thực hiện để </em><code>nums</code><em> có </em><code>k</code><em> số </em><code>1</code><em> <strong>liên tiếp</strong>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,0,0,1,0,1], k = 2
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Sau 1 phép biến đổi, nums có thể là [1,0,0,0,<u>1</u>,<u>1</u>] và có 2 số 1 liên tiếp.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,0,0,0,0,0,1,1], k = 3
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Sau 5 phép biến đổi, số 1 ngoài cùng bên trái có thể được dịch sang phải để nums = [0,0,0,0,0,<u>1</u>,<u>1</u>,<u>1</u>].
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,0,1], k = 2
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> nums đã có 2 số 1 liên tiếp.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>nums[i]</code> là <code>0</code> hoặc <code>1</code>.</li>
	<li><code>1 &lt;= k &lt;= sum(nums)</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tổng tiền tố + Duyệt trung vị

<!-- thinking:start -->

> **Tư duy**
>
> Các phép đổi chỗ liền kề để gom $k$ số 1 thành một khối liên tiếp tương đương với việc đưa chỉ số của chúng về một cửa sổ độ dài $k$. Duyệt mọi vị trí đích cho từng cửa sổ là quá chậm khi $n\le 10^5$.
>
> Một lần đổi chỗ liền kề làm thay đổi chỉ số đi $1$, nên chi phí bằng khoảng cách $L_1$ từ các số 1 được chọn đến vị trí đích. Tổng này nhỏ nhất khi vị trí đích là trung vị của $k$ chỉ số.
>
> Lưu các chỉ số của số 1 vào $arr$ và xây dựng tổng tiền tố. Duyệt trung vị của cửa sổ $arr[i]$, tính hai phía trong $O(1)$ bằng tổng tiền tố và giữ chi phí nhỏ nhất.

<!-- thinking:end -->

Ta lưu các chỉ số của những số $1$ trong mảng $nums$ vào mảng $arr$. Sau đó, ta tiền xử lý mảng tổng tiền tố $s$ của $arr$, trong đó $s[i]$ là tổng của $i$ phần tử đầu tiên trong $arr$.

Với một đoạn con độ dài $k$, số phần tử bên trái (bao gồm trung vị) là $x=\frac{k+1}{2}$, còn số phần tử bên phải là $y=k-x$.

Ta duyệt chỉ số $i$ của trung vị, với $x-1\leq i\leq len(arr)-y$. Tổng tiền tố bên trái là $ls=s[i+1]-s[i+1-x]$, còn tổng tiền tố bên phải là $rs=s[i+1+y]-s[i+1]$. Chỉ số trung vị hiện tại trong $nums$ là $j=arr[i]$. Số phép biến đổi để đưa $x$ phần tử bên trái về $[j-x+1,..j]$ là $a=(j+j-x+1)\times\frac{x}{2}-ls$, còn số phép biến đổi để đưa $y$ phần tử bên phải về $[j+1,..j+y]$ là $b=rs-(j+1+j+y)\times\frac{y}{2}$. Tổng chi phí là $a+b$, và ta lấy giá trị nhỏ nhất trong mọi cửa sổ.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(m)$. Ở đây, $n$ là độ dài mảng $nums$ và $m$ là số lượng số $1$ trong mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minMoves(self, nums: List[int], k: int) -> int:
        arr = [i for i, x in enumerate(nums) if x]
        s = list(accumulate(arr, initial=0))
        ans = inf
        x = (k + 1) // 2
        y = k - x
        for i in range(x - 1, len(arr) - y):
            j = arr[i]
            ls = s[i + 1] - s[i + 1 - x]
            rs = s[i + 1 + y] - s[i + 1]
            a = (j + j - x + 1) * x // 2 - ls
            b = rs - (j + 1 + j + y) * y // 2
            ans = min(ans, a + b)
        return ans
```

#### Java

```java
class Solution {
    public int minMoves(int[] nums, int k) {
        List<Integer> arr = new ArrayList<>();
        int n = nums.length;
        for (int i = 0; i < n; ++i) {
            if (nums[i] != 0) {
                arr.add(i);
            }
        }
        int m = arr.size();
        int[] s = new int[m + 1];
        for (int i = 0; i < m; ++i) {
            s[i + 1] = s[i] + arr.get(i);
        }
        long ans = 1 << 60;
        int x = (k + 1) / 2;
        int y = k - x;
        for (int i = x - 1; i < m - y; ++i) {
            int j = arr.get(i);
            int ls = s[i + 1] - s[i + 1 - x];
            int rs = s[i + 1 + y] - s[i + 1];
            long a = (j + j - x + 1L) * x / 2 - ls;
            long b = rs - (j + 1L + j + y) * y / 2;
            ans = Math.min(ans, a + b);
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minMoves(vector<int>& nums, int k) {
        vector<int> arr;
        for (int i = 0; i < nums.size(); ++i) {
            if (nums[i]) {
                arr.push_back(i);
            }
        }
        int m = arr.size();
        long s[m + 1];
        s[0] = 1;
        for (int i = 0; i < m; ++i) {
            s[i + 1] = s[i] + arr[i];
        }
        long ans = 1L << 60;
        int x = (k + 1) / 2;
        int y = k - x;
        for (int i = x - 1; i < m - y; ++i) {
            int j = arr[i];
            int ls = s[i + 1] - s[i + 1 - x];
            int rs = s[i + 1 + y] - s[i + 1];
            long a = (j + j - x + 1L) * x / 2 - ls;
            long b = rs - (j + 1L + j + y) * y / 2;
            ans = min(ans, a + b);
        }
        return ans;
    }
};
```

#### Go

```go
func minMoves(nums []int, k int) int {
	arr := []int{}
	for i, x := range nums {
		if x != 0 {
			arr = append(arr, i)
		}
	}
	s := make([]int, len(arr)+1)
	for i, x := range arr {
		s[i+1] = s[i] + x
	}
	ans := 1 << 60
	x := (k + 1) / 2
	y := k - x
	for i := x - 1; i < len(arr)-y; i++ {
		j := arr[i]
		ls := s[i+1] - s[i+1-x]
		rs := s[i+1+y] - s[i+1]
		a := (j+j-x+1)*x/2 - ls
		b := rs - (j+1+j+y)*y/2
		ans = min(ans, a+b)
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
