---
comments: true
difficulty: Easy
---

<!-- problem:start -->

# [08.03. Magic Index](https://leetcode.cn/problems/magic-index-lcci)

[中文文档](/lcci/08.03.Magic%20Index/README.md)

## Mô tả

<!-- description:start -->

<p>Magic index trong một mảng <code>A[0...n-1]</code> được định nghĩa là một chỉ số sao cho <code>A[i] = i</code>. Cho một mảng số nguyên phân biệt đã được sắp xếp, hãy viết một method để tìm magic index, nếu tồn tại, trong mảng A. Nếu không tồn tại, trả về -1. Nếu có nhiều magic index, hãy trả về magic index nhỏ nhất.</p>

<p><strong>Ví dụ 1:</strong></p>

<pre>

<strong>Đầu vào</strong>: nums = [0, 2, 3, 4, 5]

<strong>Đầu ra</strong>: 0

</pre>

<p><strong>Ví dụ 2:</strong></p>

<pre>

<strong>Đầu vào</strong>: nums = [1, 1, 1]

<strong>Đầu ra</strong>: 1

</pre>

<p><strong>Lưu ý:</strong></p>

<ol>
	<li><code>1 &lt;= nums.length &lt;= 1000000</code></li>
</ol>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Magic index thỏa mãn $nums[i]=i$. Duyệt từ trái sang phải sẽ tìm được magic index nhỏ nhất trong thời gian tuyến tính.
>
> Các phần tử trùng nhau làm mất cơ sở cho lập luận “so sánh $mid$ với $nums[mid]$ rồi loại bỏ một nửa”, vì một kết quả khớp vẫn có thể nằm ở bên trái.
>
> Trước tiên, tìm kiếm toàn bộ nửa bên trái (vì đáp án ở đó sẽ là đáp án nhỏ nhất), sau đó kiểm tra vị trí giữa rồi đến nửa bên phải. Trong trường hợp xấu nhất, độ phức tạp vẫn là tuyến tính, nhưng với dữ liệu đã có thứ tự, nhiều đoạn thường được bỏ qua.

<!-- thinking:end -->

Ta xây dựng một hàm $dfs(i, j)$ để tìm magic index trong mảng $nums[i, j]$. Nếu tìm thấy, trả về giá trị của magic index; nếu không, trả về $-1$. Do đó, đáp án là $dfs(0, n-1)$.

Cách triển khai hàm $dfs(i, j)$ như sau:

1. Nếu $i > j$, trả về $-1$.
2. Ngược lại, lấy vị trí giữa $mid = (i + j) / 2$, sau đó gọi đệ quy $dfs(i, mid-1)$. Nếu giá trị trả về khác $-1$, nghĩa là đã tìm thấy magic index ở nửa bên trái, nên trả về ngay giá trị đó. Nếu không, nếu $nums[mid] = mid$, nghĩa là đã tìm thấy magic index, nên trả về ngay giá trị đó. Nếu không, gọi đệ quy $dfs(mid+1, j)$ và trả về kết quả.

Trong trường hợp xấu nhất, độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findMagicIndex(self, nums: List[int]) -> int:
        def dfs(i: int, j: int) -> int:
            if i > j:
                return -1
            mid = (i + j) >> 1
            l = dfs(i, mid - 1)
            if l != -1:
                return l
            if nums[mid] == mid:
                return mid
            return dfs(mid + 1, j)

        return dfs(0, len(nums) - 1)
```

#### Java

```java
class Solution {
    public int findMagicIndex(int[] nums) {
        return dfs(nums, 0, nums.length - 1);
    }

    private int dfs(int[] nums, int i, int j) {
        if (i > j) {
            return -1;
        }
        int mid = (i + j) >> 1;
        int l = dfs(nums, i, mid - 1);
        if (l != -1) {
            return l;
        }
        if (nums[mid] == mid) {
            return mid;
        }
        return dfs(nums, mid + 1, j);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findMagicIndex(vector<int>& nums) {
        function<int(int, int)> dfs = [&](int i, int j) {
            if (i > j) {
                return -1;
            }
            int mid = (i + j) >> 1;
            int l = dfs(i, mid - 1);
            if (l != -1) {
                return l;
            }
            if (nums[mid] == mid) {
                return mid;
            }
            return dfs(mid + 1, j);
        };
        return dfs(0, nums.size() - 1);
    }
};
```

#### Go

```go
func findMagicIndex(nums []int) int {
	var dfs func(i, j int) int
	dfs = func(i, j int) int {
		if i > j {
			return -1
		}
		mid := (i + j) >> 1
		if l := dfs(i, mid-1); l != -1 {
			return l
		}
		if nums[mid] == mid {
			return mid
		}
		return dfs(mid+1, j)
	}
	return dfs(0, len(nums)-1)
}
```

#### TypeScript

```ts
function findMagicIndex(nums: number[]): number {
    const dfs = (i: number, j: number): number => {
        if (i > j) {
            return -1;
        }
        const mid = (i + j) >> 1;
        const l = dfs(i, mid - 1);
        if (l !== -1) {
            return l;
        }
        if (nums[mid] === mid) {
            return mid;
        }
        return dfs(mid + 1, j);
    };
    return dfs(0, nums.length - 1);
}
```

#### Rust

```rust
impl Solution {
    fn dfs(nums: &Vec<i32>, i: usize, j: usize) -> i32 {
        if i >= j || nums[j - 1] < 0 {
            return -1;
        }
        let mid = (i + j) >> 1;
        if nums[mid] >= (i as i32) {
            let l = Self::dfs(nums, i, mid);
            if l != -1 {
                return l;
            }
        }
        if nums[mid] == (mid as i32) {
            return mid as i32;
        }
        Self::dfs(nums, mid + 1, j)
    }

    pub fn find_magic_index(nums: Vec<i32>) -> i32 {
        Self::dfs(&nums, 0, nums.len())
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var findMagicIndex = function (nums) {
    const dfs = (i, j) => {
        if (i > j) {
            return -1;
        }
        const mid = (i + j) >> 1;
        const l = dfs(i, mid - 1);
        if (l !== -1) {
            return l;
        }
        if (nums[mid] === mid) {
            return mid;
        }
        return dfs(mid + 1, j);
    };
    return dfs(0, nums.length - 1);
};
```

#### Swift

```swift
class Solution {
    func findMagicIndex(_ nums: [Int]) -> Int {
        return find(nums, 0, nums.count - 1)
    }

    private func find(_ nums: [Int], _ i: Int, _ j: Int) -> Int {
        if i > j {
            return -1
        }
        let mid = (i + j) >> 1
        let l = find(nums, i, mid - 1)
        if l != -1 {
            return l
        }
        if nums[mid] == mid {
            return mid
        }
        return find(nums, mid + 1, j)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
