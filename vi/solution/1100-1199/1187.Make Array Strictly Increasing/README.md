---
comments: true
difficulty: Hard
rating: 2315
source: Weekly Contest 153 Q4
tags:
    - Array
    - Binary Search
    - Dynamic Programming
    - Sorting
    - Longest Increasing Subsequence
---

<!-- problem:start -->

# [1187. Make Array Strictly Increasing](https://leetcode.com/problems/make-array-strictly-increasing)

[中文文档](/solution/1100-1199/1187.Make%20Array%20Strictly%20Increasing/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <code>arr1</code> và <code>arr2</code>, hãy trả về số thao tác ít nhất (có thể bằng 0) cần thực hiện để làm cho <code>arr1</code> tăng nghiêm ngặt.</p>

<p>Trong một thao tác, bạn có thể chọn hai chỉ số <code>0 &lt;=&nbsp;i &lt; arr1.length</code> và <code>0 &lt;= j &lt; arr2.length</code>, rồi gán <code>arr1[i] = arr2[j]</code>.</p>

<p>Nếu không thể làm cho <code>arr1</code> tăng nghiêm ngặt, hãy trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr1 = [1,5,3,6,7], arr2 = [1,3,2,4]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Thay <code>5</code> bằng <code>2</code>, khi đó <code>arr1 = [1, 2, 3, 6, 7]</code>.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr1 = [1,5,3,6,7], arr2 = [4,3,1]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Thay <code>5</code> bằng <code>3</code>, sau đó thay <code>3</code> bằng <code>4</code>. Khi đó, <code>arr1 = [1, 3, 4, 6, 7]</code>.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr1 = [1,5,3,6,7], arr2 = [1,6,3,3]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Bạn không thể làm cho <code>arr1</code> tăng nghiêm ngặt.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr1.length, arr2.length &lt;= 2000</code></li>
	<li><code>0 &lt;= arr1[i], arr2[i] &lt;= 10^9</code></li>
</ul>

<p>&nbsp;</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Dynamic Programming

<!-- thinking:start -->

> **Tư duy**
>
> Các giá trị thay thế phải lấy từ $arr2$ và ta cần dùng ít thao tác nhất. Thử mọi giá trị thay thế tại mọi chỉ số sẽ quá tốn kém. Gọi $f[i]$ là số lần thay ít nhất để prefix tăng nghiêm ngặt và giữ nguyên phần tử tại chỉ số $i$. Thay $k$ phần tử trước đó bằng $k$ giá trị lớn nhất trong $arr2$ nhưng nhỏ hơn $arr[i]$ cho phép chuyển trạng thái từ $f[i-k-1]+k$. Sắp xếp và loại trùng $arr2$ để tìm kiếm nhị phân; thêm sentinel để phần tử cuối không bị thay.

<!-- thinking:end -->

Ta định nghĩa $f[i]$ là số thao tác ít nhất để biến $arr1[0,..,i]$ thành mảng tăng nghiêm ngặt, với điều kiện không thay $arr1[i]$. Vì vậy, thêm hai sentinel $-\infty$ và $\infty$ vào đầu và cuối $arr1$. Phần tử cuối chắc chắn không bị thay, nên đáp án là $f[n-1]$. Khởi tạo $f[0]=0$, còn các $f[i]$ khác bằng $\infty$.

Tiếp theo, sắp xếp mảng $arr2$ và loại bỏ phần tử trùng lặp để thuận tiện cho tìm kiếm nhị phân.

Với $i=1,..,n-1$, xét trường hợp $arr1[i-1]$ được giữ nguyên. Nếu $arr1[i-1] \lt arr1[i]$, ta có thể chuyển trạng thái từ $f[i-1]$, tức $f[i] = f[i-1]$. Tiếp theo, xét trường hợp thay $arr[i-1]$. Rõ ràng, nên thay $arr[i-1]$ bằng số lớn nhất có thể nhưng nhỏ hơn $arr[i]$. Dùng tìm kiếm nhị phân trên $arr2$ để tìm chỉ số đầu tiên $j$ có giá trị lớn hơn hoặc bằng $arr[i]$. Sau đó, xét số phần tử cần thay trong khoảng $k \in [1, min(i-1, j)]$. Nếu $arr[i-k-1] \lt arr2[j-k]$, ta có thể chuyển trạng thái từ $f[i-k-1]$, tức cập nhật $f[i] = \min(f[i], f[i-k-1] + k)$.

Cuối cùng, nếu $f[n-1] \geq \infty$, nghĩa là không thể chuyển thành mảng tăng nghiêm ngặt, nên trả về $-1$; nếu không, trả về $f[n-1]$.

Độ phức tạp thời gian là $(n \times (\log m + \min(m, n)))$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của $arr1$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def makeArrayIncreasing(self, arr1: List[int], arr2: List[int]) -> int:
        arr2.sort()
        m = 0
        for x in arr2:
            if m == 0 or x != arr2[m - 1]:
                arr2[m] = x
                m += 1
        arr2 = arr2[:m]
        arr = [-inf] + arr1 + [inf]
        n = len(arr)
        f = [inf] * n
        f[0] = 0
        for i in range(1, n):
            if arr[i - 1] < arr[i]:
                f[i] = f[i - 1]
            j = bisect_left(arr2, arr[i])
            for k in range(1, min(i - 1, j) + 1):
                if arr[i - k - 1] < arr2[j - k]:
                    f[i] = min(f[i], f[i - k - 1] + k)
        return -1 if f[n - 1] >= inf else f[n - 1]
```

#### Java

```java
class Solution {
    public int makeArrayIncreasing(int[] arr1, int[] arr2) {
        Arrays.sort(arr2);
        int m = 0;
        for (int x : arr2) {
            if (m == 0 || x != arr2[m - 1]) {
                arr2[m++] = x;
            }
        }
        final int inf = 1 << 30;
        int[] arr = new int[arr1.length + 2];
        arr[0] = -inf;
        arr[arr.length - 1] = inf;
        System.arraycopy(arr1, 0, arr, 1, arr1.length);
        int[] f = new int[arr.length];
        Arrays.fill(f, inf);
        f[0] = 0;
        for (int i = 1; i < arr.length; ++i) {
            if (arr[i - 1] < arr[i]) {
                f[i] = f[i - 1];
            }
            int j = search(arr2, arr[i], m);
            for (int k = 1; k <= Math.min(i - 1, j); ++k) {
                if (arr[i - k - 1] < arr2[j - k]) {
                    f[i] = Math.min(f[i], f[i - k - 1] + k);
                }
            }
        }
        return f[arr.length - 1] >= inf ? -1 : f[arr.length - 1];
    }

    private int search(int[] nums, int x, int n) {
        int l = 0, r = n;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (nums[mid] >= x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int makeArrayIncreasing(vector<int>& arr1, vector<int>& arr2) {
        sort(arr2.begin(), arr2.end());
        arr2.erase(unique(arr2.begin(), arr2.end()), arr2.end());
        const int inf = 1 << 30;
        arr1.insert(arr1.begin(), -inf);
        arr1.push_back(inf);
        int n = arr1.size();
        vector<int> f(n, inf);
        f[0] = 0;
        for (int i = 1; i < n; ++i) {
            if (arr1[i - 1] < arr1[i]) {
                f[i] = f[i - 1];
            }
            int j = lower_bound(arr2.begin(), arr2.end(), arr1[i]) - arr2.begin();
            for (int k = 1; k <= min(i - 1, j); ++k) {
                if (arr1[i - k - 1] < arr2[j - k]) {
                    f[i] = min(f[i], f[i - k - 1] + k);
                }
            }
        }
        return f[n - 1] >= inf ? -1 : f[n - 1];
    }
};
```

#### Go

```go
func makeArrayIncreasing(arr1 []int, arr2 []int) int {
	sort.Ints(arr2)
	m := 0
	for _, x := range arr2 {
		if m == 0 || x != arr2[m-1] {
			arr2[m] = x
			m++
		}
	}
	arr2 = arr2[:m]
	const inf = 1 << 30
	arr1 = append([]int{-inf}, arr1...)
	arr1 = append(arr1, inf)
	n := len(arr1)
	f := make([]int, n)
	for i := range f {
		f[i] = inf
	}
	f[0] = 0
	for i := 1; i < n; i++ {
		if arr1[i-1] < arr1[i] {
			f[i] = f[i-1]
		}
		j := sort.SearchInts(arr2, arr1[i])
		for k := 1; k <= min(i-1, j); k++ {
			if arr1[i-k-1] < arr2[j-k] {
				f[i] = min(f[i], f[i-k-1]+k)
			}
		}
	}
	if f[n-1] >= inf {
		return -1
	}
	return f[n-1]
}
```

#### TypeScript

```ts
function makeArrayIncreasing(arr1: number[], arr2: number[]): number {
    arr2.sort((a, b) => a - b);
    let m = 0;
    for (const x of arr2) {
        if (m === 0 || x !== arr2[m - 1]) {
            arr2[m++] = x;
        }
    }
    arr2 = arr2.slice(0, m);
    const inf = 1 << 30;
    arr1 = [-inf, ...arr1, inf];
    const n = arr1.length;
    const f: number[] = new Array(n).fill(inf);
    f[0] = 0;
    const search = (arr: number[], x: number): number => {
        let l = 0;
        let r = arr.length;
        while (l < r) {
            const mid = (l + r) >> 1;
            if (arr[mid] >= x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    };
    for (let i = 1; i < n; ++i) {
        if (arr1[i - 1] < arr1[i]) {
            f[i] = f[i - 1];
        }
        const j = search(arr2, arr1[i]);
        for (let k = 1; k <= Math.min(i - 1, j); ++k) {
            if (arr1[i - k - 1] < arr2[j - k]) {
                f[i] = Math.min(f[i], f[i - k - 1] + k);
            }
        }
    }
    return f[n - 1] >= inf ? -1 : f[n - 1];
}
```

#### C#

```cs
public class Solution {
    public int MakeArrayIncreasing(int[] arr1, int[] arr2) {
        Array.Sort(arr2);
        int m = 0;
        foreach (int x in arr2) {
            if (m == 0 || x != arr2[m - 1]) {
                arr2[m++] = x;
            }
        }
        int inf = 1 << 30;
        int[] arr = new int[arr1.Length + 2];
        arr[0] = -inf;
        arr[arr.Length - 1] = inf;
        for (int i = 0; i < arr1.Length; ++i) {
            arr[i + 1] = arr1[i];
        }
        int[] f = new int[arr.Length];
        Array.Fill(f, inf);
        f[0] = 0;
        for (int i = 1; i < arr.Length; ++i) {
            if (arr[i - 1] < arr[i]) {
                f[i] = f[i - 1];
            }
            int j = search(arr2, arr[i], m);
            for (int k = 1; k <= Math.Min(i - 1, j); ++k) {
                if (arr[i - k - 1] < arr2[j - k]) {
                    f[i] = Math.Min(f[i], f[i - k - 1] + k);
                }
            }
        }
        return f[arr.Length - 1] >= inf ? -1 : f[arr.Length - 1];
    }

    private int search(int[] nums, int x, int n) {
        int l = 0, r = n;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (nums[mid] >= x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
