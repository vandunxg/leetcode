---
comments: true
difficulty: Hard
tags:
    - Array
    - Hash Table
    - Sorting
---

<!-- problem:start -->

# [2459. Sort Array by Moving Items to Empty Space 🔒](https://leetcode.com/problems/sort-array-by-moving-items-to-empty-space)

[中文文档](/solution/2400-2499/2459.Sort%20Array%20by%20Moving%20Items%20to%20Empty%20Space/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> có kích thước <code>n</code>, chứa <strong>mỗi</strong> phần tử từ <code>0</code> đến <code>n - 1</code> (<strong>bao gồm cả hai đầu</strong>). Mỗi phần tử từ <code>1</code> đến <code>n - 1</code> đại diện cho một phần tử, còn phần tử <code>0</code> đại diện cho một ô trống.</p>

<p>Trong một thao tác, bạn có thể di chuyển <strong>bất kỳ</strong> phần tử nào vào ô trống. <code>nums</code> được coi là đã sắp xếp nếu các số của tất cả phần tử nằm theo <strong>thứ tự tăng dần</strong> và ô trống ở đầu hoặc cuối mảng.</p>

<p>Ví dụ, nếu <code>n = 4</code>, <code>nums</code> được coi là đã sắp xếp nếu:</p>

<ul>
	<li><code>nums = [0,1,2,3]</code> hoặc</li>
	<li><code>nums = [1,2,3,0]</code></li>
</ul>

<p>...và được coi là chưa sắp xếp trong các trường hợp khác.</p>

<p>Hãy trả về <em><strong>số thao tác ít nhất cần thực hiện để sắp xếp </strong></em><code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,2,0,3,1]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
- Di chuyển phần tử 2 vào ô trống. Khi đó, nums = [4,0,2,3,1].
- Di chuyển phần tử 1 vào ô trống. Khi đó, nums = [4,1,2,3,0].
- Di chuyển phần tử 4 vào ô trống. Khi đó, nums = [0,1,2,3,4].
Có thể chứng minh rằng 3 là số thao tác ít nhất cần thực hiện.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4,0]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> nums đã được sắp xếp, nên trả về 0.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,0,2,4,3]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
- Di chuyển phần tử 2 vào ô trống. Khi đó, nums = [1,2,0,4,3].
- Di chuyển phần tử 3 vào ô trống. Khi đó, nums = [1,2,3,4,0].
Có thể chứng minh rằng 2 là số thao tác ít nhất cần thực hiện.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt; n</code></li>
	<li>Tất cả giá trị của <code>nums</code> là <strong>duy nhất</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Chu trình hoán vị

<!-- thinking:start -->

> **Tư duy**
>
> Việc swap với ô trống chính là sắp xếp theo các chu trình hoán vị. Một chu trình có độ dài $m$ cần $m-1$ thao tác nếu chứa ô trống, và $m+1$ thao tác nếu không chứa ô trống. Đích đến có thể là $0,1,\ldots,n-1$ hoặc $1,\ldots,n-1,0$.
>
> Duyệt các chu trình, tính $m+1$ cho các chu trình không chứa ô trống, sau đó trừ $2$ nếu ô trống đang ở sai vị trí. Lấy giá trị nhỏ hơn trong hai đích đến.

<!-- thinking:end -->

Với một chu trình hoán vị có độ dài $m$, nếu $0$ nằm trong chu trình thì số lần swap là $m-1$; nếu không, số lần swap là $m+1$.

Ta tìm tất cả các chu trình hoán vị, trước tiên tính tổng số lần swap với giả định mỗi chu trình cần $m+1$ lần swap, sau đó kiểm tra xem $0$ có ở sai vị trí hay không. Nếu có, điều đó có nghĩa là $0$ nằm trong một chu trình hoán vị, nên ta trừ $2$ khỏi tổng số lần swap.

Ở đây, $0$ có thể nằm ở vị trí $0$ hoặc vị trí $n-1$. Ta lấy giá trị nhỏ hơn trong hai trường hợp này.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sortArray(self, nums: List[int]) -> int:
        def f(nums, k):
            vis = [False] * n
            cnt = 0
            for i, v in enumerate(nums):
                if i == v or vis[i]:
                    continue
                cnt += 1
                j = i
                while not vis[j]:
                    vis[j] = True
                    cnt += 1
                    j = nums[j]
            return cnt - 2 * (nums[k] != k)

        n = len(nums)
        a = f(nums, 0)
        b = f([(v - 1 + n) % n for v in nums], n - 1)
        return min(a, b)
```

#### Java

```java
class Solution {
    public int sortArray(int[] nums) {
        int n = nums.length;
        int[] arr = new int[n];
        for (int i = 0; i < n; ++i) {
            arr[i] = (nums[i] - 1 + n) % n;
        }
        int a = f(nums, 0);
        int b = f(arr, n - 1);
        return Math.min(a, b);
    }

    private int f(int[] nums, int k) {
        boolean[] vis = new boolean[nums.length];
        int cnt = 0;
        for (int i = 0; i < nums.length; ++i) {
            if (i == nums[i] || vis[i]) {
                continue;
            }
            ++cnt;
            int j = nums[i];
            while (!vis[j]) {
                vis[j] = true;
                ++cnt;
                j = nums[j];
            }
        }
        if (nums[k] != k) {
            cnt -= 2;
        }
        return cnt;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int sortArray(vector<int>& nums) {
        int n = nums.size();
        auto f = [&](vector<int>& nums, int k) {
            vector<bool> vis(n);
            int cnt = 0;
            for (int i = 0; i < n; ++i) {
                if (i == nums[i] || vis[i]) continue;
                int j = i;
                ++cnt;
                while (!vis[j]) {
                    vis[j] = true;
                    ++cnt;
                    j = nums[j];
                }
            }
            if (nums[k] != k) cnt -= 2;
            return cnt;
        };

        int a = f(nums, 0);
        vector<int> arr = nums;
        for (int& v : arr) v = (v - 1 + n) % n;
        int b = f(arr, n - 1);
        return min(a, b);
    }
};
```

#### Go

```go
func sortArray(nums []int) int {
	n := len(nums)
	f := func(nums []int, k int) int {
		vis := make([]bool, n)
		cnt := 0
		for i, v := range nums {
			if i == v || vis[i] {
				continue
			}
			cnt++
			j := i
			for !vis[j] {
				vis[j] = true
				cnt++
				j = nums[j]
			}
		}
		if nums[k] != k {
			cnt -= 2
		}
		return cnt
	}
	a := f(nums, 0)
	arr := make([]int, n)
	for i, v := range nums {
		arr[i] = (v - 1 + n) % n
	}
	b := f(arr, n-1)
	return min(a, b)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
