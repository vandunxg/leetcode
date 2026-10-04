---
comments: true
difficulty: Easy
rating: 1272
source: Biweekly Contest 114 Q1
tags:
    - Bit Manipulation
    - Array
    - Hash Table
---

<!-- problem:start -->

# [2869. Minimum Operations to Collect Elements](https://leetcode.com/problems/minimum-operations-to-collect-elements)

[中文文档](/solution/2800-2899/2869.Minimum%20Operations%20to%20Collect%20Elements/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <code>nums</code> gồm các số nguyên dương và một số nguyên <code>k</code>.</p>

<p>Trong một thao tác, bạn có thể xóa phần tử cuối cùng của mảng và thêm phần tử đó vào tập hợp của mình.</p>

<p>Trả về <em><strong>số thao tác nhỏ nhất</strong> cần thực hiện để thu thập các phần tử</em> <code>1, 2, ..., k</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,1,5,4,2], k = 2
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Sau 4 thao tác, ta thu thập các phần tử 2, 4, 5 và 1 theo thứ tự này. Tập hợp của ta chứa các phần tử 1 và 2. Do đó, đáp án là 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,1,5,4,2], k = 5
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Sau 5 thao tác, ta thu thập các phần tử 2, 4, 5, 1 và 3 theo thứ tự này. Tập hợp của ta chứa các phần tử từ 1 đến 5. Do đó, đáp án là 5.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,2,5,3,1], k = 3
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Sau 4 thao tác, ta thu thập các phần tử 1, 3, 5 và 2 theo thứ tự này. Tập hợp của ta chứa các phần tử từ 1 đến 3. Do đó, đáp án là 4.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 50</code></li>
	<li><code>1 &lt;= nums[i] &lt;= nums.length</code></li>
	<li><code>1 &lt;= k &lt;= nums.length</code></li>
	<li>Dữ liệu đầu vào được tạo sao cho bạn có thể thu thập các phần tử <code>1, 2, ..., k</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt theo thứ tự ngược

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác lấy phần tử ở cuối hiện tại, nên đáp án là hậu tố ngắn nhất chứa mọi số nguyên trong $1..k$. Duyệt ngược cùng một mảng boolean sẽ dừng ở lần đầu tiên thu thập đủ $k$ mục tiêu phân biệt.

<!-- thinking:end -->

Ta có thể duyệt mảng theo thứ tự ngược. Với mỗi phần tử gặp trong quá trình duyệt có giá trị nhỏ hơn hoặc bằng $k$ và chưa được thêm vào tập hợp, ta thêm phần tử đó vào tập hợp cho đến khi tập hợp chứa các phần tử từ $1$ đến $k$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$. Độ phức tạp không gian là $O(k)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, nums: List[int], k: int) -> int:
        is_added = [False] * k
        count = 0
        n = len(nums)
        for i in range(n - 1, -1, -1):
            if nums[i] > k or is_added[nums[i] - 1]:
                continue
            is_added[nums[i] - 1] = True
            count += 1
            if count == k:
                return n - i
```

#### Java

```java
class Solution {
    public int minOperations(List<Integer> nums, int k) {
        boolean[] isAdded = new boolean[k];
        int n = nums.size();
        int count = 0;
        for (int i = n - 1;; i--) {
            if (nums.get(i) > k || isAdded[nums.get(i) - 1]) {
                continue;
            }
            isAdded[nums.get(i) - 1] = true;
            count++;
            if (count == k) {
                return n - i;
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minOperations(vector<int>& nums, int k) {
        int n = nums.size();
        vector<bool> isAdded(n);
        int count = 0;
        for (int i = n - 1;; --i) {
            if (nums[i] > k || isAdded[nums[i] - 1]) {
                continue;
            }
            isAdded[nums[i] - 1] = true;
            if (++count == k) {
                return n - i;
            }
        }
    }
};
```

#### Go

```go
func minOperations(nums []int, k int) int {
	isAdded := make([]bool, k)
	count := 0
	n := len(nums)
	for i := n - 1; ; i-- {
		if nums[i] > k || isAdded[nums[i]-1] {
			continue
		}
		isAdded[nums[i]-1] = true
		count++
		if count == k {
			return n - i
		}
	}
}
```

#### TypeScript

```ts
function minOperations(nums: number[], k: number): number {
    const n = nums.length;
    const isAdded = Array(k).fill(false);
    let count = 0;
    for (let i = n - 1; ; --i) {
        if (nums[i] > k || isAdded[nums[i] - 1]) {
            continue;
        }
        isAdded[nums[i] - 1] = true;
        ++count;
        if (count === k) {
            return n - i;
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
