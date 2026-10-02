---
comments: true
difficulty: Hard
rating: 1827
source: Weekly Contest 142 Q3
tags:
    - Array
    - Binary Search
    - Interactive
    - Ternary Search
---

<!-- problem:start -->

# [1095. Find in Mountain Array](https://leetcode.com/problems/find-in-mountain-array)

[中文文档](/solution/1000-1099/1095.Find%20in%20Mountain%20Array/README.md)

## Mô tả

<!-- description:start -->

<p><em>(Đây là một bài toán <strong>tương tác</strong>.)</em></p>

<p>Nhắc lại, mảng <code>arr</code> là <strong>mountain array</strong> khi và chỉ khi:</p>

<ul>
	<li><code>arr.length &gt;= 3</code></li>
	<li>Tồn tại chỉ số <code>i</code> thỏa mãn <code>0 &lt; i &lt; arr.length - 1</code> sao cho:
	<ul>
		<li><code>arr[0] &lt; arr[1] &lt; ... &lt; arr[i - 1] &lt; arr[i]</code></li>
		<li><code>arr[i] &gt; arr[i + 1] &gt; ... &gt; arr[arr.length - 1]</code></li>
	</ul>
	</li>
</ul>

<p>Cho mountain array <code>mountainArr</code>, hãy trả về <code>index</code> <strong>nhỏ nhất</strong> sao cho <code>mountainArr.get(index) == target</code>. Nếu không tồn tại <code>index</code> như vậy, trả về <code>-1</code>.</p>

<p><strong>Bạn không thể truy cập trực tiếp vào mountain array.</strong> Chỉ có thể truy cập mảng thông qua interface <code>MountainArray</code>:</p>

<ul>
	<li><code>MountainArray.get(k)</code> trả về phần tử ở chỉ số <code>k</code> của mảng (đánh số từ 0).</li>
	<li><code>MountainArray.length()</code> trả về độ dài của mảng.</li>
</ul>

<p>Bài nộp gọi <code>MountainArray.get</code> hơn <code>100</code> lần sẽ bị chấm <em>Sai</em>. Các lời giải tìm cách qua mặt hệ thống chấm cũng sẽ bị loại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> mountainArr = [1,2,3,4,5,3,1], target = 3
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Giá trị 3 xuất hiện trong mảng tại các chỉ số 2 và 5. Trả về chỉ số nhỏ nhất là 2.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> mountainArr = [0,1,2,4,2,1], target = 3
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Vì <code>the array,</code> không chứa giá trị 3, ta trả về -1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= mountainArr.length() &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= target &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= mountainArr.get(index) &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ có thể truy cập mountain array thông qua `get`; với $n\le 10^4$, cần giới hạn số lần gọi theo logarit. Tìm đỉnh trước, sau đó binary search ở nửa trái tăng dần và nửa phải giảm dần.
>
> So sánh $get(mid)$ với $get(mid+1)$ để xác định đỉnh. `search` dùng $k=\pm 1$ để có thể áp dụng cùng một vòng lặp lower bound cho cả hai phía.
>
> Tìm ở nửa trái trước để trả về chỉ số nhỏ nhất.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
# """
# This is MountainArray's API interface.
# You should not implement it, or speculate about its implementation
# """
# class MountainArray:
#    def get(self, index: int) -> int:
#    def length(self) -> int:


class Solution:
    def findInMountainArray(self, target: int, mountain_arr: 'MountainArray') -> int:
        def search(l: int, r: int, k: int) -> int:
            while l < r:
                mid = (l + r) >> 1
                if k * mountain_arr.get(mid) >= k * target:
                    r = mid
                else:
                    l = mid + 1
            return -1 if mountain_arr.get(l) != target else l

        n = mountain_arr.length()
        l, r = 0, n - 1
        while l < r:
            mid = (l + r) >> 1
            if mountain_arr.get(mid) > mountain_arr.get(mid + 1):
                r = mid
            else:
                l = mid + 1
        ans = search(0, l, 1)
        return search(l + 1, n - 1, -1) if ans == -1 else ans
```

#### Java

```java
/**
 * // This is MountainArray's API interface.
 * // You should not implement it, or speculate about its implementation
 * interface MountainArray {
 *     public int get(int index) {}
 *     public int length() {}
 * }
 */

class Solution {
    private MountainArray mountainArr;
    private int target;

    public int findInMountainArray(int target, MountainArray mountainArr) {
        int n = mountainArr.length();
        int l = 0, r = n - 1;
        while (l < r) {
            int mid = (l + r) >>> 1;
            if (mountainArr.get(mid) > mountainArr.get(mid + 1)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        this.mountainArr = mountainArr;
        this.target = target;
        int ans = search(0, l, 1);
        return ans == -1 ? search(l + 1, n - 1, -1) : ans;
    }

    private int search(int l, int r, int k) {
        while (l < r) {
            int mid = (l + r) >>> 1;
            if (k * mountainArr.get(mid) >= k * target) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return mountainArr.get(l) == target ? l : -1;
    }
}
```

#### C++

```cpp
/**
 * // This is the MountainArray's API interface.
 * // You should not implement it, or speculate about its implementation
 * class MountainArray {
 *   public:
 *     int get(int index);
 *     int length();
 * };
 */

class Solution {
public:
    int findInMountainArray(int target, MountainArray& mountainArr) {
        int n = mountainArr.length();
        int l = 0, r = n - 1;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (mountainArr.get(mid) > mountainArr.get(mid + 1)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        auto search = [&](int l, int r, int k) -> int {
            while (l < r) {
                int mid = (l + r) >> 1;
                if (k * mountainArr.get(mid) >= k * target) {
                    r = mid;
                } else {
                    l = mid + 1;
                }
            }
            return mountainArr.get(l) == target ? l : -1;
        };
        int ans = search(0, l, 1);
        return ans == -1 ? search(l + 1, n - 1, -1) : ans;
    }
};
```

#### Go

```go
/**
 * // This is the MountainArray's API interface.
 * // You should not implement it, or speculate about its implementation
 * type MountainArray struct {
 * }
 *
 * func (this *MountainArray) get(index int) int {}
 * func (this *MountainArray) length() int {}
 */

func findInMountainArray(target int, mountainArr *MountainArray) int {
	n := mountainArr.length()
	l, r := 0, n-1
	for l < r {
		mid := (l + r) >> 1
		if mountainArr.get(mid) > mountainArr.get(mid+1) {
			r = mid
		} else {
			l = mid + 1
		}
	}
	search := func(l, r, k int) int {
		for l < r {
			mid := (l + r) >> 1
			if k*mountainArr.get(mid) >= k*target {
				r = mid
			} else {
				l = mid + 1
			}
		}
		if mountainArr.get(l) == target {
			return l
		}
		return -1
	}
	ans := search(0, l, 1)
	if ans == -1 {
		return search(l+1, n-1, -1)
	}
	return ans
}
```

#### TypeScript

```ts
/**
 * // This is the MountainArray's API interface.
 * // You should not implement it, or speculate about its implementation
 * class Master {
 *      get(index: number): number {}
 *
 *      length(): number {}
 * }
 */

function findInMountainArray(target: number, mountainArr: MountainArray) {
    const n = mountainArr.length();
    let l = 0;
    let r = n - 1;
    while (l < r) {
        const mid = (l + r) >> 1;
        if (mountainArr.get(mid) > mountainArr.get(mid + 1)) {
            r = mid;
        } else {
            l = mid + 1;
        }
    }
    const search = (l: number, r: number, k: number): number => {
        while (l < r) {
            const mid = (l + r) >> 1;
            if (k * mountainArr.get(mid) >= k * target) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return mountainArr.get(l) === target ? l : -1;
    };
    const ans = search(0, l, 1);
    return ans === -1 ? search(l + 1, n - 1, -1) : ans;
}
```

#### Rust

```rust
impl Solution {
    #[allow(dead_code)]
    pub fn find_in_mountain_array(target: i32, mountain_arr: &MountainArray) -> i32 {
        let n = mountain_arr.length();

        // First find the maximum element in the array
        let mut l = 0;
        let mut r = n - 1;

        while l < r {
            let mid = (l + r) >> 1;
            if mountain_arr.get(mid) > mountain_arr.get(mid + 1) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }

        let left = Self::binary_search(mountain_arr, 0, l, 1, target);

        if left == -1 {
            Self::binary_search(mountain_arr, l, n - 1, -1, target)
        } else {
            left
        }
    }

    #[allow(dead_code)]
    fn binary_search(m: &MountainArray, mut l: i32, mut r: i32, k: i32, target: i32) -> i32 {
        let n = m.length();

        while l < r {
            let mid = (l + r) >> 1;
            if k * m.get(mid) >= k * target {
                r = mid;
            } else {
                l = mid + 1;
            }
        }

        if m.get(l) == target {
            l
        } else {
            -1
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
