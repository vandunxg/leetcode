---
comments: true
difficulty: Medium
tags:
    - Array
    - Two Pointers
    - Binary Search
    - Sorting
    - Sliding Window
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [658. Find K Closest Elements](https://leetcode.com/problems/find-k-closest-elements)

[中文文档](/solution/0600-0699/0658.Find%20K%20Closest%20Elements/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>arr</code> đã được <strong>sắp xếp</strong> cùng hai số nguyên <code>k</code> và <code>x</code>. Hãy trả về <code>k</code> số nguyên trong mảng gần với <code>x</code> nhất. Kết quả cũng phải được sắp xếp tăng dần.</p>

<p>Số nguyên <code>a</code> được xem là gần <code>x</code> hơn số nguyên <code>b</code> nếu:</p>

<ul>
	<li><code>|a - x| &lt; |b - x|</code>, hoặc</li>
	<li><code>|a - x| == |b - x|</code> và <code>a &lt; b</code></li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">arr = [1,2,3,4,5], k = 4, x = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,2,3,4]</span></p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">arr = [1,1,2,3,4,5], k = 4, x = -1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,1,2,3]</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= arr.length</code></li>
	<li><code>1 &lt;= arr.length &lt;= 10<sup>4</sup></code></li>
	<li><code>arr</code> được sắp xếp theo thứ tự <strong>tăng dần</strong>.</li>
	<li><code>-10<sup>4</sup> &lt;= arr[i], x &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm $k$ giá trị gần $x$ nhất và trả về theo thứ tự tăng dần. Sắp xếp theo khoảng cách rồi lấy $k$ phần tử đầu có độ phức tạp $O(n\log n)$.
>
> Sắp xếp theo $|v-x|$, giữ lại $k$ phần tử rồi sắp xếp chúng theo giá trị.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findClosestElements(self, arr: List[int], k: int, x: int) -> List[int]:
        arr.sort(key=lambda v: abs(v - x))
        return sorted(arr[:k])
```

#### Java

```java
class Solution {
    public List<Integer> findClosestElements(int[] arr, int k, int x) {
        List<Integer> ans = Arrays.stream(arr)
                                .boxed()
                                .sorted((a, b) -> {
                                    int v = Math.abs(a - x) - Math.abs(b - x);
                                    return v == 0 ? a - b : v;
                                })
                                .collect(Collectors.toList());
        ans = ans.subList(0, k);
        Collections.sort(ans);
        return ans;
    }
}
```

#### C++

```cpp
int target;

class Solution {
public:
    static bool cmp(int& a, int& b) {
        int v = abs(a - target) - abs(b - target);
        return v == 0 ? a < b : v < 0;
    }

    vector<int> findClosestElements(vector<int>& arr, int k, int x) {
        target = x;
        sort(arr.begin(), arr.end(), cmp);
        vector<int> ans(arr.begin(), arr.begin() + k);
        sort(ans.begin(), ans.end());
        return ans;
    }
};
```

#### Go

```go
func findClosestElements(arr []int, k int, x int) []int {
	sort.Slice(arr, func(i, j int) bool {
		v := abs(arr[i]-x) - abs(arr[j]-x)
		if v == 0 {
			return arr[i] < arr[j]
		}
		return v < 0
	})
	ans := arr[:k]
	sort.Ints(ans)
	return ans
}

func abs(x int) int {
	if x >= 0 {
		return x
	}
	return -x
}
```

#### TypeScript

```ts
function findClosestElements(arr: number[], k: number, x: number): number[] {
    let l = 0;
    let r = arr.length;
    while (r - l > k) {
        if (x - arr[l] <= arr[r - 1] - x) {
            --r;
        } else {
            ++l;
        }
    }
    return arr.slice(l, r);
}
```

#### Rust

```rust
impl Solution {
    pub fn find_closest_elements(arr: Vec<i32>, k: i32, x: i32) -> Vec<i32> {
        let n = arr.len();
        let mut l = 0;
        let mut r = n;
        while r - l != (k as usize) {
            if x - arr[l] <= arr[r - 1] - x {
                r -= 1;
            } else {
                l += 1;
            }
        }
        arr[l..r].to_vec()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Đáp án là một đoạn liên tiếp. Ta thu hẹp đầu mút xa hơn cho đến khi cửa sổ có độ dài $k$; đoạn thu được vốn đã được sắp xếp.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findClosestElements(self, arr: List[int], k: int, x: int) -> List[int]:
        l, r = 0, len(arr)
        while r - l > k:
            if x - arr[l] <= arr[r - 1] - x:
                r -= 1
            else:
                l += 1
        return arr[l:r]
```

#### Java

```java
class Solution {
    public List<Integer> findClosestElements(int[] arr, int k, int x) {
        int l = 0, r = arr.length;
        while (r - l > k) {
            if (x - arr[l] <= arr[r - 1] - x) {
                --r;
            } else {
                ++l;
            }
        }
        List<Integer> ans = new ArrayList<>();
        for (int i = l; i < r; ++i) {
            ans.add(arr[i]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> findClosestElements(vector<int>& arr, int k, int x) {
        int l = 0, r = arr.size();
        while (r - l > k) {
            if (x - arr[l] <= arr[r - 1] - x) {
                --r;
            } else {
                ++l;
            }
        }
        return vector<int>(arr.begin() + l, arr.begin() + r);
    }
};
```

#### Go

```go
func findClosestElements(arr []int, k int, x int) []int {
	l, r := 0, len(arr)
	for r-l > k {
		if x-arr[l] <= arr[r-1]-x {
			r--
		} else {
			l++
		}
	}
	return arr[l:r]
}
```

#### TypeScript

```ts
function findClosestElements(arr: number[], k: number, x: number): number[] {
    let left = 0;
    let right = arr.length - k;
    while (left < right) {
        const mid = (left + right) >> 1;
        if (x - arr[mid] <= arr[mid + k] - x) {
            right = mid;
        } else {
            left = mid + 1;
        }
    }
    return arr.slice(left, left + k);
}
```

#### Rust

```rust
impl Solution {
    pub fn find_closest_elements(arr: Vec<i32>, k: i32, x: i32) -> Vec<i32> {
        let k = k as usize;
        let n = arr.len();
        let mut left = 0;
        let mut right = n - k;
        while left < right {
            let mid = left + (right - left) / 2;
            if x - arr[mid] > arr[mid + k] - x {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        arr[left..left + k].to_vec()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3

<!-- thinking:start -->

> **Tư duy**
>
> Cách dùng two pointers có độ phức tạp tuyến tính. Vị trí bắt đầu tối ưu trong đoạn $[0,n-k]$ có tính đơn điệu: so sánh $x-arr[mid]$ với $arr[mid+k]-x$, rồi tìm kiếm nhị phân vị trí đó.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findClosestElements(self, arr: List[int], k: int, x: int) -> List[int]:
        left, right = 0, len(arr) - k
        while left < right:
            mid = (left + right) >> 1
            if x - arr[mid] <= arr[mid + k] - x:
                right = mid
            else:
                left = mid + 1
        return arr[left : left + k]
```

#### Java

```java
class Solution {
    public List<Integer> findClosestElements(int[] arr, int k, int x) {
        int left = 0;
        int right = arr.length - k;
        while (left < right) {
            int mid = (left + right) >> 1;
            if (x - arr[mid] <= arr[mid + k] - x) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        List<Integer> ans = new ArrayList<>();
        for (int i = left; i < left + k; ++i) {
            ans.add(arr[i]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> findClosestElements(vector<int>& arr, int k, int x) {
        int left = 0, right = arr.size() - k;
        while (left < right) {
            int mid = (left + right) >> 1;
            if (x - arr[mid] <= arr[mid + k] - x)
                right = mid;
            else
                left = mid + 1;
        }
        return vector<int>(arr.begin() + left, arr.begin() + left + k);
    }
};
```

#### Go

```go
func findClosestElements(arr []int, k int, x int) []int {
	left, right := 0, len(arr)-k
	for left < right {
		mid := (left + right) >> 1
		if x-arr[mid] <= arr[mid+k]-x {
			right = mid
		} else {
			left = mid + 1
		}
	}
	return arr[left : left+k]
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
