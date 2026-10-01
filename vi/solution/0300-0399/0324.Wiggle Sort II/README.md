---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - Divide and Conquer
    - Quickselect
    - Sorting
---

<!-- problem:start -->

# [324. Wiggle Sort II](https://leetcode.com/problems/wiggle-sort-ii)

[中文文档](/solution/0300-0399/0324.Wiggle%20Sort%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code>, hãy sắp xếp lại sao cho <code>nums[0] &lt; nums[1] &gt; nums[2] &lt; nums[3]...</code>.</p>

<p>Có thể giả sử mảng đầu vào luôn có cách sắp xếp thỏa mãn.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,5,1,1,6,4]
<strong>Đầu ra:</strong> [1,6,1,5,1,4]
<strong>Giải thích:</strong> [1,4,1,5,1,6] cũng được chấp nhận.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,2,2,3,1]
<strong>Đầu ra:</strong> [2,3,1,3,1,2]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 5000</code></li>
	<li>Đảm bảo luôn tồn tại đáp án cho mảng đầu vào <code>nums</code>.</li>
</ul>

<p>&nbsp;</p>
<strong>Câu hỏi mở rộng:</strong> Bạn có thể giải bài toán trong thời gian <code>O(n)</code> và/hoặc <strong>in-place</strong> với bộ nhớ phụ <code>O(1)</code> không?

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần đạt được $nums[0]<nums[1]>nums[2]<\cdots$ và có thể sắp xếp lại mảng tại chỗ.
>
> Sắp xếp mảng, rồi điền các chỉ số chẵn từ trung vị giảm dần và các chỉ số lẻ từ cuối mảng giảm dần. Việc điền hai nửa theo thứ tự ngược giúp các giá trị bằng nhau không nằm cạnh nhau.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def wiggleSort(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        arr = sorted(nums)
        n = len(arr)
        i, j = (n - 1) >> 1, n - 1
        for k in range(n):
            if k % 2 == 0:
                nums[k] = arr[i]
                i -= 1
            else:
                nums[k] = arr[j]
                j -= 1
```

#### Java

```java
class Solution {
    public void wiggleSort(int[] nums) {
        int[] arr = nums.clone();
        Arrays.sort(arr);
        int n = nums.length;
        int i = (n - 1) >> 1, j = n - 1;
        for (int k = 0; k < n; ++k) {
            if (k % 2 == 0) {
                nums[k] = arr[i--];
            } else {
                nums[k] = arr[j--];
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    void wiggleSort(vector<int>& nums) {
        vector<int> arr = nums;
        sort(arr.begin(), arr.end());
        int n = nums.size();
        int i = (n - 1) >> 1, j = n - 1;
        for (int k = 0; k < n; ++k) {
            if (k % 2 == 0)
                nums[k] = arr[i--];
            else
                nums[k] = arr[j--];
        }
    }
};
```

#### Go

```go
func wiggleSort(nums []int) {
	n := len(nums)
	arr := make([]int, n)
	copy(arr, nums)
	sort.Ints(arr)
	i, j := (n-1)>>1, n-1
	for k := 0; k < n; k++ {
		if k%2 == 0 {
			nums[k] = arr[i]
			i--
		} else {
			nums[k] = arr[j]
			j--
		}
	}
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {void} Do not return anything, modify nums in-place instead.
 */
var wiggleSort = function (nums) {
    const arr = nums.slice().sort((a, b) => a - b);
    const n = arr.length;
    let i = (n - 1) >> 1;
    let j = n - 1;
    for (let k = 0; k < n; ++k) {
        if (k % 2 === 0) {
            nums[k] = arr[i--];
        } else {
            nums[k] = arr[j--];
        }
    }
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Cách 1 sắp xếp trong $O(n\log n)$ và dùng thêm một mảng. Các giá trị nằm trong $[0,5000]$, nên ta có thể dùng counting sort rồi lần lượt điền giá trị lớn vào chỉ số lẻ trước, sau đó đến chỉ số chẵn. Nhờ vậy, các đỉnh nhận giá trị lớn và thuật toán chạy tuyến tính.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def wiggleSort(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        bucket = [0] * 5001
        for v in nums:
            bucket[v] += 1
        n = len(nums)
        j = 5000
        for i in range(1, n, 2):
            while bucket[j] == 0:
                j -= 1
            nums[i] = j
            bucket[j] -= 1
        for i in range(0, n, 2):
            while bucket[j] == 0:
                j -= 1
            nums[i] = j
            bucket[j] -= 1
```

#### Java

```java
class Solution {
    public void wiggleSort(int[] nums) {
        int[] bucket = new int[5001];
        for (int v : nums) {
            ++bucket[v];
        }
        int n = nums.length;
        int j = 5000;
        for (int i = 1; i < n; i += 2) {
            while (bucket[j] == 0) {
                --j;
            }
            nums[i] = j;
            --bucket[j];
        }
        for (int i = 0; i < n; i += 2) {
            while (bucket[j] == 0) {
                --j;
            }
            nums[i] = j;
            --bucket[j];
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    void wiggleSort(vector<int>& nums) {
        vector<int> bucket(5001);
        for (int& v : nums) ++bucket[v];
        int n = nums.size();
        int j = 5000;
        for (int i = 1; i < n; i += 2) {
            while (bucket[j] == 0) --j;
            nums[i] = j;
            --bucket[j];
        }
        for (int i = 0; i < n; i += 2) {
            while (bucket[j] == 0) --j;
            nums[i] = j;
            --bucket[j];
        }
    }
};
```

#### Go

```go
func wiggleSort(nums []int) {
	bucket := make([]int, 5001)
	for _, v := range nums {
		bucket[v]++
	}
	n, j := len(nums), 5000
	for i := 1; i < n; i += 2 {
		for bucket[j] == 0 {
			j--
		}
		nums[i] = j
		bucket[j]--
	}
	for i := 0; i < n; i += 2 {
		for bucket[j] == 0 {
			j--
		}
		nums[i] = j
		bucket[j]--
	}
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {void} Do not return anything, modify nums in-place instead.
 */
var wiggleSort = function (nums) {
    let bucket = new Array(5001).fill(0);
    for (const v of nums) {
        bucket[v]++;
    }
    const n = nums.length;
    let j = 5000;
    for (let i = 1; i < n; i += 2) {
        while (bucket[j] == 0) {
            --j;
        }
        nums[i] = j;
        --bucket[j];
    }
    for (let i = 0; i < n; i += 2) {
        while (bucket[j] == 0) {
            --j;
        }
        nums[i] = j;
        --bucket[j];
    }
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
