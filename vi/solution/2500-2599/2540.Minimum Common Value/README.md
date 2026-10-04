---
comments: true
difficulty: Easy
rating: 1249
source: Biweekly Contest 96 Q1
tags:
    - Array
    - Hash Table
    - Two Pointers
    - Binary Search
---

<!-- problem:start -->

# [2540. Minimum Common Value](https://leetcode.com/problems/minimum-common-value)

[中文文档](/solution/2500-2599/2540.Minimum%20Common%20Value/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <code>nums1</code> và <code>nums2</code> đã được sắp xếp theo thứ tự không giảm, hãy trả về <em><strong>số nguyên chung nhỏ nhất</strong> của cả hai mảng</em>. Nếu <code>nums1</code> và <code>nums2</code> không có số nguyên chung, trả về <code>-1</code>.</p>

<p>Một số nguyên được gọi là <strong>chung</strong> của <code>nums1</code> và <code>nums2</code> nếu cả hai mảng đều chứa <strong>ít nhất một</strong> lần xuất hiện của số nguyên đó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> nums1 = [1,2,3], nums2 = [2,4]
<strong>Output:</strong> 2
<strong>Giải thích:</strong> Phần tử nhỏ nhất cùng xuất hiện trong cả hai mảng là 2, vì vậy ta trả về 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> nums1 = [1,2,3,6], nums2 = [2,3,4,5]
<strong>Output:</strong> 2
<strong>Giải thích:</strong> Có hai phần tử chung trong hai mảng là 2 và 3, trong đó 2 là phần tử nhỏ hơn, nên trả về 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums1.length, nums2.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums1[i], nums2[j] &lt;= 10<sup>9</sup></code></li>
	<li>Cả <code>nums1</code> và <code>nums2</code> đều được sắp xếp theo thứ tự <strong>không giảm</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Two Pointers

<!-- thinking:start -->

> **Tư duy**
>
> Cả hai mảng đều tăng nghiêm ngặt; ta cần tìm giá trị chung nhỏ nhất. Đưa một phía vào set sẽ dùng thêm không gian tuyến tính.
>
> Di chuyển đồng thời hai con trỏ: nếu hai giá trị bằng nhau thì đó là giá trị chung nhỏ nhất; nếu không, di chuyển phía có phần tử đầu nhỏ hơn. Mỗi mảng được duyệt nhiều nhất một lần.

<!-- thinking:end -->

Duyệt qua hai mảng. Nếu các phần tử mà hai con trỏ đang trỏ tới bằng nhau, trả về phần tử đó. Nếu các phần tử mà hai con trỏ đang trỏ tới khác nhau, di chuyển con trỏ đang trỏ tới phần tử nhỏ hơn sang phải một vị trí cho đến khi tìm thấy hai phần tử bằng nhau hoặc duyệt hết một mảng.

Độ phức tạp thời gian là $O(m + n)$, trong đó $m$ và $n$ lần lượt là độ dài của hai mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getCommon(self, nums1: List[int], nums2: List[int]) -> int:
        i = j = 0
        m, n = len(nums1), len(nums2)
        while i < m and j < n:
            if nums1[i] == nums2[j]:
                return nums1[i]
            if nums1[i] < nums2[j]:
                i += 1
            else:
                j += 1
        return -1
```

#### Java

```java
class Solution {
    public int getCommon(int[] nums1, int[] nums2) {
        int m = nums1.length, n = nums2.length;
        for (int i = 0, j = 0; i < m && j < n;) {
            if (nums1[i] == nums2[j]) {
                return nums1[i];
            }
            if (nums1[i] < nums2[j]) {
                ++i;
            } else {
                ++j;
            }
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int getCommon(vector<int>& nums1, vector<int>& nums2) {
        int m = nums1.size(), n = nums2.size();
        for (int i = 0, j = 0; i < m && j < n;) {
            if (nums1[i] == nums2[j]) {
                return nums1[i];
            }
            if (nums1[i] < nums2[j]) {
                ++i;
            } else {
                ++j;
            }
        }
        return -1;
    }
};
```

#### Go

```go
func getCommon(nums1 []int, nums2 []int) int {
	m, n := len(nums1), len(nums2)
	for i, j := 0, 0; i < m && j < n; {
		if nums1[i] == nums2[j] {
			return nums1[i]
		}
		if nums1[i] < nums2[j] {
			i++
		} else {
			j++
		}
	}
	return -1
}
```

#### TypeScript

```ts
function getCommon(nums1: number[], nums2: number[]): number {
    const m = nums1.length;
    const n = nums2.length;
    let i = 0;
    let j = 0;
    while (i < m && j < n) {
        if (nums1[i] === nums2[j]) {
            return nums1[i];
        }
        if (nums1[i] < nums2[j]) {
            i++;
        } else {
            j++;
        }
    }
    return -1;
}
```

#### Rust

```rust
impl Solution {
    pub fn get_common(nums1: Vec<i32>, nums2: Vec<i32>) -> i32 {
        let m = nums1.len();
        let n = nums2.len();
        let mut i = 0;
        let mut j = 0;
        while i < m && j < n {
            if nums1[i] == nums2[j] {
                return nums1[i];
            }
            if nums1[i] < nums2[j] {
                i += 1;
            } else {
                j += 1;
            }
        }
        -1
    }
}
```

#### C

```c
int getCommon(int* nums1, int nums1Size, int* nums2, int nums2Size) {
    int i = 0;
    int j = 0;
    while (i < nums1Size && j < nums2Size) {
        if (nums1[i] == nums2[j]) {
            return nums1[i];
        }
        if (nums1[i] < nums2[j]) {
            i++;
        } else {
            j++;
        }
    }
    return -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
