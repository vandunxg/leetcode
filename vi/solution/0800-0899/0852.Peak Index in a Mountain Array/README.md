---
comments: true
difficulty: Medium
tags:
    - Array
    - Binary Search
    - Ternary Search
---

<!-- problem:start -->

# [852. Peak Index in a Mountain Array](https://leetcode.com/problems/peak-index-in-a-mountain-array)

[中文文档](/solution/0800-0899/0852.Peak%20Index%20in%20a%20Mountain%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <strong>dạng núi</strong> <code>arr</code> có độ dài <code>n</code>, trong đó các giá trị tăng dần đến một <strong>phần tử đỉnh</strong>, rồi giảm dần.</p>

<p>Hãy trả về chỉ số của phần tử đỉnh.</p>

<p>Yêu cầu giải bài toán với độ phức tạp thời gian <code>O(log(n))</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">arr = [0,1,0]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">arr = [0,2,1,0]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">arr = [0,10,5,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= arr.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= arr[i] &lt;= 10<sup>6</sup></code></li>
	<li><code>arr</code> được <strong>đảm bảo</strong> là mảng dạng núi.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mảng tăng nghiêm ngặt rồi giảm nghiêm ngặt; ta cần tìm chỉ số đỉnh. Có thể duyệt tuyến tính, nhưng $n\le 10^5$ và điều kiện xác định đỉnh có tính đơn điệu: ở bên trái đỉnh thì $arr[i]<arr[i+1]$, còn bên phải thì ngược lại.
>
> Tìm kiếm nhị phân trên đoạn $[1,n-2]$: nếu $arr[mid]>arr[mid+1]$ thì đỉnh nằm ở nửa trái (bao gồm cả $mid$), nếu không thì nằm ở nửa phải. Kết thúc, đầu trái chính là đỉnh.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def peakIndexInMountainArray(self, arr: List[int]) -> int:
        left, right = 1, len(arr) - 2
        while left < right:
            mid = (left + right) >> 1
            if arr[mid] > arr[mid + 1]:
                right = mid
            else:
                left = mid + 1
        return left
```

#### Java

```java
class Solution {
    public int peakIndexInMountainArray(int[] arr) {
        int left = 1, right = arr.length - 2;
        while (left < right) {
            int mid = (left + right) >> 1;
            if (arr[mid] > arr[mid + 1]) {
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
    int peakIndexInMountainArray(vector<int>& arr) {
        int left = 1, right = arr.size() - 2;
        while (left < right) {
            int mid = (left + right) >> 1;
            if (arr[mid] > arr[mid + 1])
                right = mid;
            else
                left = mid + 1;
        }
        return left;
    }
};
```

#### Go

```go
func peakIndexInMountainArray(arr []int) int {
	left, right := 1, len(arr)-2
	for left < right {
		mid := (left + right) >> 1
		if arr[mid] > arr[mid+1] {
			right = mid
		} else {
			left = mid + 1
		}
	}
	return left
}
```

#### TypeScript

```ts
function peakIndexInMountainArray(arr: number[]): number {
    let left = 1,
        right = arr.length - 2;
    while (left < right) {
        const mid = (left + right) >> 1;
        if (arr[mid] > arr[mid + 1]) {
            right = mid;
        } else {
            left = mid + 1;
        }
    }
    return left;
}
```

#### Rust

```rust
impl Solution {
    pub fn peak_index_in_mountain_array(arr: Vec<i32>) -> i32 {
        let mut left = 1;
        let mut right = arr.len() - 2;
        while left < right {
            let mid = left + (right - left) / 2;
            if arr[mid] > arr[mid + 1] {
                right = mid;
            } else {
                left = left + 1;
            }
        }
        left as i32
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} arr
 * @return {number}
 */
var peakIndexInMountainArray = function (arr) {
    let left = 1;
    let right = arr.length - 2;
    while (left < right) {
        const mid = (left + right) >> 1;
        if (arr[mid] < arr[mid + 1]) {
            left = mid + 1;
        } else {
            right = mid;
        }
    }
    return left;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
