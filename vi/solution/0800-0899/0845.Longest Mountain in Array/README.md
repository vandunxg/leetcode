---
comments: true
difficulty: Medium
tags:
    - Array
    - Two Pointers
    - Dynamic Programming
    - Enumeration
---

<!-- problem:start -->

# [845. Longest Mountain in Array](https://leetcode.com/problems/longest-mountain-in-array)

[中文文档](/solution/0800-0899/0845.Longest%20Mountain%20in%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Nhắc lại, mảng <code>arr</code> là <strong>mảng hình núi</strong> khi và chỉ khi:</p>

<ul>
	<li><code>arr.length &gt;= 3</code></li>
	<li>Tồn tại một chỉ số <code>i</code> (<strong>đánh số từ 0</strong>) thỏa mãn <code>0 &lt; i &lt; arr.length - 1</code> sao cho:
	<ul>
		<li><code>arr[0] &lt; arr[1] &lt; ... &lt; arr[i - 1] &lt; arr[i]</code></li>
		<li><code>arr[i] &gt; arr[i + 1] &gt; ... &gt; arr[arr.length - 1]</code></li>
	</ul>
	</li>
</ul>

<p>Cho mảng số nguyên <code>arr</code>, hãy trả về <em>độ dài lớn nhất của mảng con có dạng hình núi</em>. Nếu không có mảng con hình núi, trả về <code>0</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [2,1,4,7,3,2,5]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Mảng hình núi dài nhất là [1,4,7,3,2], có độ dài 5.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [2,2,2]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có mảng hình núi nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= arr[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong></p>

<ul>
	<li>Bạn có thể giải bài toán chỉ với một lượt duyệt không?</li>
	<li>Bạn có thể giải bài toán với không gian <code>O(1)</code> không?</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tiền xử lý + liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Mảng hình núi tăng nghiêm ngặt rồi giảm nghiêm ngặt, và có độ dài ít nhất $3$. $n\le 10^4$, nên có thể mở rộng từ từng đỉnh núi nhưng sẽ duyệt lại các đoạn tăng giảm giống nhau.
>
> $f[i]$ là độ dài đoạn tăng dài nhất kết thúc tại $i$, còn $g[i]$ là độ dài đoạn giảm dài nhất bắt đầu tại $i$. $i$ là đỉnh núi khi cả hai giá trị đều lớn hơn $1$; khi đó độ dài mảng hình núi là $f[i]+g[i]-1$.

<!-- thinking:end -->

Ta định nghĩa hai mảng $f$ và $g$, trong đó $f[i]$ là độ dài đoạn tăng liên tiếp dài nhất kết thúc tại $arr[i]$, còn $g[i]$ là độ dài đoạn giảm liên tiếp dài nhất bắt đầu tại $arr[i]$. Với mỗi chỉ số $i$, nếu $f[i] \gt 1$ và $g[i] \gt 1$, độ dài mảng hình núi có đỉnh tại $arr[i]$ là $f[i] + g[i] - 1$. Chỉ cần duyệt tất cả $i$ và tìm giá trị lớn nhất.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài mảng $arr$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestMountain(self, arr: List[int]) -> int:
        n = len(arr)
        f = [1] * n
        g = [1] * n
        for i in range(1, n):
            if arr[i] > arr[i - 1]:
                f[i] = f[i - 1] + 1
        ans = 0
        for i in range(n - 2, -1, -1):
            if arr[i] > arr[i + 1]:
                g[i] = g[i + 1] + 1
                if f[i] > 1:
                    ans = max(ans, f[i] + g[i] - 1)
        return ans
```

#### Java

```java
class Solution {
    public int longestMountain(int[] arr) {
        int n = arr.length;
        int[] f = new int[n];
        int[] g = new int[n];
        Arrays.fill(f, 1);
        Arrays.fill(g, 1);
        for (int i = 1; i < n; ++i) {
            if (arr[i] > arr[i - 1]) {
                f[i] = f[i - 1] + 1;
            }
        }
        int ans = 0;
        for (int i = n - 2; i >= 0; --i) {
            if (arr[i] > arr[i + 1]) {
                g[i] = g[i + 1] + 1;
                if (f[i] > 1) {
                    ans = Math.max(ans, f[i] + g[i] - 1);
                }
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int longestMountain(vector<int>& arr) {
        int n = arr.size();
        int f[n];
        int g[n];
        fill(f, f + n, 1);
        fill(g, g + n, 1);
        for (int i = 1; i < n; ++i) {
            if (arr[i] > arr[i - 1]) {
                f[i] = f[i - 1] + 1;
            }
        }
        int ans = 0;
        for (int i = n - 2; ~i; --i) {
            if (arr[i] > arr[i + 1]) {
                g[i] = g[i + 1] + 1;
                if (f[i] > 1) {
                    ans = max(ans, f[i] + g[i] - 1);
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func longestMountain(arr []int) (ans int) {
	n := len(arr)
	f := make([]int, n)
	g := make([]int, n)
	for i := range f {
		f[i] = 1
		g[i] = 1
	}
	for i := 1; i < n; i++ {
		if arr[i] > arr[i-1] {
			f[i] = f[i-1] + 1
		}
	}
	for i := n - 2; i >= 0; i-- {
		if arr[i] > arr[i+1] {
			g[i] = g[i+1] + 1
			if f[i] > 1 {
				ans = max(ans, f[i]+g[i]-1)
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function longestMountain(arr: number[]): number {
    const n = arr.length;
    const f: number[] = Array(n).fill(1);
    const g: number[] = Array(n).fill(1);
    for (let i = 1; i < n; ++i) {
        if (arr[i] > arr[i - 1]) {
            f[i] = f[i - 1] + 1;
        }
    }
    let ans = 0;
    for (let i = n - 2; i >= 0; --i) {
        if (arr[i] > arr[i + 1]) {
            g[i] = g[i + 1] + 1;
            if (f[i] > 1) {
                ans = Math.max(ans, f[i] + g[i] - 1);
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Một lượt duyệt (liệt kê chân trái của mảng hình núi)

<!-- thinking:start -->

> **Tư duy**
>
> Không cần dùng hai mảng phụ. Bắt đầu từ chân trái: đi lên đến đỉnh, đi xuống đến chân phải, cập nhật đáp án rồi chuyển con trỏ trái đến vị trí hiện tại của con trỏ phải.
>
> Bỏ qua các vị trí không thể tạo thành đỉnh núi. Mỗi phần tử được duyệt số lần hằng số.

<!-- thinking:end -->

Ta có thể duyệt chân trái của mảng hình núi rồi tìm chân phải ở phía bên phải. Dùng hai con trỏ $l$ và $r$, trong đó $l$ là chỉ số chân trái còn $r$ là chỉ số chân phải. Ban đầu, $l=0$ và $r=0$. Sau đó, dịch $r$ sang phải để tìm vị trí đỉnh núi. Tại đó, kiểm tra xem $r + 1 \lt n$ và $arr[r] \gt arr[r + 1]$ có đúng không. Nếu đúng, tiếp tục dịch $r$ sang phải đến chân phải. Khi đó, độ dài mảng hình núi là $r - l + 1$. Cập nhật đáp án rồi gán $l$ bằng $r$ để tiếp tục tìm mảng hình núi tiếp theo.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng $arr$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestMountain(self, arr: List[int]) -> int:
        n = len(arr)
        ans = l = 0
        while l + 2 < n:
            r = l + 1
            if arr[l] < arr[r]:
                while r + 1 < n and arr[r] < arr[r + 1]:
                    r += 1
                if r < n - 1 and arr[r] > arr[r + 1]:
                    while r < n - 1 and arr[r] > arr[r + 1]:
                        r += 1
                    ans = max(ans, r - l + 1)
                else:
                    r += 1
            l = r
        return ans
```

#### Java

```java
class Solution {
    public int longestMountain(int[] arr) {
        int n = arr.length;
        int ans = 0;
        for (int l = 0, r = 0; l + 2 < n; l = r) {
            r = l + 1;
            if (arr[l] < arr[r]) {
                while (r + 1 < n && arr[r] < arr[r + 1]) {
                    ++r;
                }
                if (r + 1 < n && arr[r] > arr[r + 1]) {
                    while (r + 1 < n && arr[r] > arr[r + 1]) {
                        ++r;
                    }
                    ans = Math.max(ans, r - l + 1);
                } else {
                    ++r;
                }
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int longestMountain(vector<int>& arr) {
        int n = arr.size();
        int ans = 0;
        for (int l = 0, r = 0; l + 2 < n; l = r) {
            r = l + 1;
            if (arr[l] < arr[r]) {
                while (r + 1 < n && arr[r] < arr[r + 1]) {
                    ++r;
                }
                if (r + 1 < n && arr[r] > arr[r + 1]) {
                    while (r + 1 < n && arr[r] > arr[r + 1]) {
                        ++r;
                    }
                    ans = max(ans, r - l + 1);
                } else {
                    ++r;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func longestMountain(arr []int) (ans int) {
	n := len(arr)
	for l, r := 0, 0; l+2 < n; l = r {
		r = l + 1
		if arr[l] < arr[r] {
			for r+1 < n && arr[r] < arr[r+1] {
				r++
			}
			if r+1 < n && arr[r] > arr[r+1] {
				for r+1 < n && arr[r] > arr[r+1] {
					r++
				}
				ans = max(ans, r-l+1)
			} else {
				r++
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function longestMountain(arr: number[]): number {
    const n = arr.length;
    let ans = 0;
    for (let l = 0, r = 0; l + 2 < n; l = r) {
        r = l + 1;
        if (arr[l] < arr[r]) {
            while (r + 1 < n && arr[r] < arr[r + 1]) {
                ++r;
            }
            if (r + 1 < n && arr[r] > arr[r + 1]) {
                while (r + 1 < n && arr[r] > arr[r + 1]) {
                    ++r;
                }
                ans = Math.max(ans, r - l + 1);
            } else {
                ++r;
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
