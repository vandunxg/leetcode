---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [10.03. Search Rotate Array](https://leetcode.cn/problems/search-rotate-array-lcci)

[中文文档](/lcci/10.03.Search%20Rotate%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng gồm n số nguyên đã được sắp xếp nhưng bị xoay một số lần không xác định, hãy viết code để tìm một phần tử trong mảng. Bạn có thể giả sử rằng mảng ban đầu được sắp xếp theo thứ tự tăng dần. Nếu có nhiều hơn một phần tử bằng target trong mảng, hãy trả về chỉ số nhỏ nhất.</p>
<p><strong>Ví dụ 1:</strong></p>
<pre>

<strong> Đầu vào</strong>: arr = [15, 16, 19, 20, 25, 1, 3, 4, 5, 7, 10, 14], target = 5

<strong> Đầu ra</strong>: 8 (chỉ số của 5 trong mảng)

</pre>
<p><strong>Ví dụ 2:</strong></p>
<pre>

<strong> Đầu vào</strong>: arr = [15, 16, 19, 20, 25, 1, 3, 4, 5, 7, 10, 14], target = 11

<strong> Đầu ra</strong>: -1 (không tìm thấy)

</pre>
<p><strong>Lưu ý:</strong></p>
<ol>
	<li><code>1 &lt;= arr.length &lt;= 1000000</code></li>
</ol>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Tìm $target$ trong một mảng đã sắp xếp nhưng bị xoay, có thể chứa các phần tử trùng lặp. Duyệt tuyến tính tìm được lần xuất hiện đầu tiên nhưng bỏ qua cấu trúc gồm các nửa đã sắp xếp.
>
> Midpoint chia mảng sao cho ít nhất một phía được sắp xếp; so sánh $arr[mid]$ với $arr[r]$ cho biết phía nào được sắp xếp và $target$ có nằm trong đó hay không.
>
> Khi hai đầu bằng nhau, trước tiên giảm $r$ để phép kiểm tra xác định được phía được sắp xếp. Sau vòng lặp, kiểm tra $arr[l]$. Các phần tử trùng lặp khiến trường hợp xấu nhất gần như tuyến tính.

<!-- thinking:end -->

Ta đặt biên trái của tìm kiếm nhị phân là $l=0$ và biên phải là $r=n-1$, trong đó $n$ là độ dài của mảng.

Trong mỗi bước tìm kiếm nhị phân, ta lấy midpoint hiện tại $mid=(l+r)/2$.

- Nếu $nums[mid] > nums[r]$, điều đó có nghĩa là $[l,mid]$ đã được sắp xếp. Nếu $nums[l] \leq target \leq nums[mid]$, thì $target$ nằm trong $[l,mid]$; ngược lại, $target$ nằm trong $[mid+1,r]$.
- Nếu $nums[mid] < nums[r]$, điều đó có nghĩa là $[mid+1,r]$ đã được sắp xếp. Nếu $nums[mid] < target \leq nums[r]$, thì $target$ nằm trong $[mid+1,r]$; ngược lại, $target$ nằm trong $[l,mid]$.
- Nếu $nums[mid] = nums[r]$, điều đó có nghĩa là hai phần tử $nums[mid]$ và $nums[r]$ bằng nhau. Khi đó, ta không thể xác định $target$ nằm trong khoảng nào, nên chỉ có thể giảm $r$ đi $1$.

Sau khi kết thúc tìm kiếm nhị phân, nếu $nums[l] = target$, điều đó có nghĩa là giá trị đích $target$ tồn tại trong mảng; nếu không thì nó không tồn tại.

Lưu ý rằng nếu ban đầu $nums[l] = nums[r]$, ta lặp để giảm $r$ đi $1$ cho đến khi $nums[l] \neq nums[r]$.

Độ phức tạp thời gian xấp xỉ $O(\log n)$, còn độ phức tạp không gian là $O(1)$. Ở đây, $n$ là độ dài của mảng.

Các bài tương tự:

- [81. Search in Rotated Sorted Array II](https://github.com/doocs/leetcode/blob/main/solution/0000-0099/0081.Search%20in%20Rotated%20Sorted%20Array%20II/README.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def search(self, arr: List[int], target: int) -> int:
        l, r = 0, len(arr) - 1
        while arr[l] == arr[r]:
            r -= 1
        while l < r:
            mid = (l + r) >> 1
            if arr[mid] > arr[r]:
                if arr[l] <= target <= arr[mid]:
                    r = mid
                else:
                    l = mid + 1
            elif arr[mid] < arr[r]:
                if arr[mid] < target <= arr[r]:
                    l = mid + 1
                else:
                    r = mid
            else:
                r -= 1
        return l if arr[l] == target else -1
```

#### Java

```java
class Solution {
    public int search(int[] arr, int target) {
        int l = 0, r = arr.length - 1;
        while (arr[l] == arr[r]) {
            --r;
        }
        while (l < r) {
            int mid = (l + r) >> 1;
            if (arr[mid] > arr[r]) {
                if (arr[l] <= target && target <= arr[mid]) {
                    r = mid;
                } else {
                    l = mid + 1;
                }
            } else if (arr[mid] < arr[r]) {
                if (arr[mid] < target && target <= arr[r]) {
                    l = mid + 1;
                } else {
                    r = mid;
                }
            } else {
                --r;
            }
        }
        return arr[l] == target ? l : -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int search(vector<int>& arr, int target) {
        int l = 0, r = arr.size() - 1;
        while (arr[l] == arr[r]) {
            --r;
        }
        while (l < r) {
            int mid = (l + r) >> 1;
            if (arr[mid] > arr[r]) {
                if (arr[l] <= target && target <= arr[mid]) {
                    r = mid;
                } else {
                    l = mid + 1;
                }
            } else if (arr[mid] < arr[r]) {
                if (arr[mid] < target && target <= arr[r]) {
                    l = mid + 1;
                } else {
                    r = mid;
                }
            } else {
                --r;
            }
        }
        return arr[l] == target ? l : -1;
    }
};
```

#### Go

```go
func search(arr []int, target int) int {
	l, r := 0, len(arr)-1
	for arr[l] == arr[r] {
		r--
	}
	for l < r {
		mid := (l + r) >> 1
		if arr[mid] > arr[r] {
			if arr[l] <= target && target <= arr[mid] {
				r = mid
			} else {
				l = mid + 1
			}
		} else if arr[mid] < arr[r] {
			if arr[mid] < target && target <= arr[r] {
				l = mid + 1
			} else {
				r = mid
			}
		} else {
			r--
		}
	}
	if arr[l] == target {
		return l
	}
	return -1
}
```

#### TypeScript

```ts
function search(arr: number[], target: number): number {
    let [l, r] = [0, arr.length - 1];
    while (arr[l] === arr[r]) {
        --r;
    }
    while (l < r) {
        const mid = (l + r) >> 1;
        if (arr[mid] > arr[r]) {
            if (arr[l] <= target && target <= arr[mid]) {
                r = mid;
            } else {
                l = mid + 1;
            }
        } else if (arr[mid] < arr[r]) {
            if (arr[mid] < target && target <= arr[r]) {
                l = mid + 1;
            } else {
                r = mid;
            }
        } else {
            --r;
        }
    }
    return arr[l] === target ? l : -1;
}
```

#### Swift

```swift
class Solution {
    func search(_ arr: [Int], _ target: Int) -> Int {
        var l = 0
        var r = arr.count - 1

        while arr[l] == arr[r] && l < r {
            r -= 1
        }

        while l < r {
            let mid = (l + r) >> 1
            if arr[mid] > arr[r] {
                if arr[l] <= target && target <= arr[mid] {
                    r = mid
                } else {
                    l = mid + 1
                }
            } else if arr[mid] < arr[r] {
                if arr[mid] < target && target <= arr[r] {
                    l = mid + 1
                } else {
                    r = mid
                }
            } else {
                r -= 1
            }
        }

        return arr[l] == target ? l : -1
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
