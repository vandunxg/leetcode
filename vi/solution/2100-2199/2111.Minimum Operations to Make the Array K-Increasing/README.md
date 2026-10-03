---
comments: true
difficulty: Hard
rating: 1940
source: Weekly Contest 272 Q4
tags:
    - Array
    - Binary Search
    - Longest Increasing Subsequence
---

<!-- problem:start -->

# [2111. Minimum Operations to Make the Array K-Increasing](https://leetcode.com/problems/minimum-operations-to-make-the-array-k-increasing)

[中文文档](/solution/2100-2199/2111.Minimum%20Operations%20to%20Make%20the%20Array%20K-Increasing/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <strong>0-indexed</strong> <code>arr</code> gồm <code>n</code> số nguyên dương và một số nguyên dương <code>k</code>.</p>

<p>Mảng <code>arr</code> được gọi là <strong>K-increasing</strong> nếu <code>arr[i-k] &lt;= arr[i]</code> đúng với mọi chỉ số <code>i</code>, trong đó <code>k &lt;= i &lt;= n-1</code>.</p>

<ul>
	<li>Ví dụ, <code>arr = [4, 1, 5, 2, 6, 2]</code> là K-increasing với <code>k = 2</code> vì:

    <ul>
    <li><code>arr[0] &lt;= arr[2] (4 &lt;= 5)</code></li>
    <li><code>arr[1] &lt;= arr[3] (1 &lt;= 2)</code></li>
    <li><code>arr[2] &lt;= arr[4] (5 &lt;= 6)</code></li>
    <li><code>arr[3] &lt;= arr[5] (2 &lt;= 2)</code></li>
    </ul>
    </li>
    <li>Tuy nhiên, cùng mảng <code>arr</code> đó không phải là K-increasing với <code>k = 1</code> (vì <code>arr[0] &gt; arr[1]</code>) hoặc <code>k = 3</code> (vì <code>arr[0] &gt; arr[3]</code>).</li>

</ul>

<p>Trong một <strong>thao tác</strong>, bạn có thể chọn một chỉ số <code>i</code> và <strong>thay đổi</strong> <code>arr[i]</code> thành <strong>bất kỳ</strong> số nguyên dương nào.</p>

<p>Hãy trả về <em><strong>số thao tác nhỏ nhất</strong> cần thực hiện để biến mảng thành K-increasing với </em><code>k</code> đã cho.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [5,4,3,2,1], k = 1
<strong>Đầu ra:</strong> 4
<strong>Giải thích:
</strong>Với k = 1, mảng kết quả phải là mảng không giảm.
Một số mảng K-increasing có thể tạo ra là [5,<u><strong>6</strong></u>,<u><strong>7</strong></u>,<u><strong>8</strong></u>,<u><strong>9</strong></u>], [<u><strong>1</strong></u>,<u><strong>1</strong></u>,<u><strong>1</strong></u>,<u><strong>1</strong></u>,1], [<u><strong>2</strong></u>,<u><strong>2</strong></u>,3,<u><strong>4</strong></u>,<u><strong>4</strong></u>]. Tất cả đều cần 4 thao tác.
Sẽ không tối ưu nếu biến mảng thành, chẳng hạn, [<u><strong>6</strong></u>,<u><strong>7</strong></u>,<u><strong>8</strong></u>,<u><strong>9</strong></u>,<u><strong>10</strong></u>] vì cách này cần 5 thao tác.
Có thể chứng minh rằng không thể biến mảng thành K-increasing với ít hơn 4 thao tác.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [4,1,5,2,6,2], k = 2
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>
Đây chính là ví dụ đã nêu trong phần mô tả bài toán.
Ở đây, với mọi chỉ số i thỏa mãn 2 &lt;= i &lt;= 5, arr[i-2] &lt;=<b> </b>arr[i].
Vì mảng đã cho đã là K-increasing nên không cần thực hiện thao tác nào.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [4,1,5,2,6,2], k = 3
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Các chỉ số 3 và 5 là những chỉ số duy nhất không thỏa mãn arr[i-3] &lt;= arr[i] với 3 &lt;= i &lt;= 5.
Một cách để biến mảng thành K-increasing là đổi arr[3] thành 4 và arr[5] thành 5.
Khi đó mảng sẽ là [4,1,5,<u><strong>4</strong></u>,6,<u><strong>5</strong></u>].
Lưu ý rằng còn những cách khác để biến mảng thành K-increasing, nhưng không cách nào cần ít hơn 2 thao tác.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= arr[i], k &lt;= arr.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Điều kiện $k$-increasing chỉ ràng buộc các dãy con có chỉ số cùng phần dư khi chia cho $k$. $k$ nhóm này độc lập với nhau, nên đáp án là tổng số thao tác của từng nhóm. Trong một nhóm, mỗi thao tác có thể thay đổi một giá trị bất kỳ, vì vậy số lần thay đổi nhỏ nhất bằng độ dài nhóm trừ đi độ dài dãy con không giảm dài nhất.
>
> Với $n\le 10^5$, cách tính LIS bậc hai trên từng nhóm là quá chậm. Các giá trị bằng nhau được phép, nên dùng $\texttt{bisect\_right}$ trên mảng patience để tính độ dài dãy con không giảm dài nhất trong $O(L\log L)$.
>
> Vì vậy, chúng ta lần lượt xử lý $\textit{arr}[i::k]$ với mỗi $i<k$ và cộng “độ dài nhóm trừ độ dài LIS”.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def kIncreasing(self, arr: List[int], k: int) -> int:
        def lis(arr):
            t = []
            for x in arr:
                idx = bisect_right(t, x)
                if idx == len(t):
                    t.append(x)
                else:
                    t[idx] = x
            return len(arr) - len(t)

        return sum(lis(arr[i::k]) for i in range(k))
```

#### Java

```java
class Solution {
    public int kIncreasing(int[] arr, int k) {
        int n = arr.length;
        int ans = 0;
        for (int i = 0; i < k; ++i) {
            List<Integer> t = new ArrayList<>();
            for (int j = i; j < n; j += k) {
                t.add(arr[j]);
            }
            ans += lis(t);
        }
        return ans;
    }

    private int lis(List<Integer> arr) {
        List<Integer> t = new ArrayList<>();
        for (int x : arr) {
            int idx = searchRight(t, x);
            if (idx == t.size()) {
                t.add(x);
            } else {
                t.set(idx, x);
            }
        }
        return arr.size() - t.size();
    }

    private int searchRight(List<Integer> arr, int x) {
        int left = 0, right = arr.size();
        while (left < right) {
            int mid = (left + right) >> 1;
            if (arr.get(mid) > x) {
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
    int kIncreasing(vector<int>& arr, int k) {
        int ans = 0, n = arr.size();
        for (int i = 0; i < k; ++i) {
            vector<int> t;
            for (int j = i; j < n; j += k) t.push_back(arr[j]);
            ans += lis(t);
        }
        return ans;
    }

    int lis(vector<int>& arr) {
        vector<int> t;
        for (int x : arr) {
            auto it = upper_bound(t.begin(), t.end(), x);
            if (it == t.end())
                t.push_back(x);
            else
                *it = x;
        }
        return arr.size() - t.size();
    }
};
```

#### Go

```go
func kIncreasing(arr []int, k int) int {
	searchRight := func(arr []int, x int) int {
		left, right := 0, len(arr)
		for left < right {
			mid := (left + right) >> 1
			if arr[mid] > x {
				right = mid
			} else {
				left = mid + 1
			}
		}
		return left
	}

	lis := func(arr []int) int {
		var t []int
		for _, x := range arr {
			idx := searchRight(t, x)
			if idx == len(t) {
				t = append(t, x)
			} else {
				t[idx] = x
			}
		}
		return len(arr) - len(t)
	}

	n := len(arr)
	ans := 0
	for i := 0; i < k; i++ {
		var t []int
		for j := i; j < n; j += k {
			t = append(t, arr[j])
		}
		ans += lis(t)
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
