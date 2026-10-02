---
comments: true
difficulty: Hard
rating: 1866
source: Weekly Contest 174 Q4
tags:
    - Array
    - Dynamic Programming
    - Sorting
---

<!-- problem:start -->

# [1340. Jump Game V](https://leetcode.com/problems/jump-game-v)

[中文文档](/solution/1300-1399/1340.Jump%20Game%20V/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>arr</code> và số nguyên <code>d</code>. Trong một bước, bạn có thể nhảy từ chỉ số <code>i</code> đến chỉ số:</p>

<ul>
	<li><code>i + x</code>, với điều kiện:&nbsp;<code>i + x &lt; arr.length</code> và <code> 0 &lt;&nbsp;x &lt;= d</code>.</li>
	<li><code>i - x</code>, với điều kiện:&nbsp;<code>i - x &gt;= 0</code> và <code> 0 &lt;&nbsp;x &lt;= d</code>.</li>
</ul>

<p>Ngoài ra, bạn chỉ có thể nhảy từ chỉ số <code>i</code> đến chỉ số <code>j</code>&nbsp;nếu <code>arr[i] &gt; arr[j]</code> và <code>arr[i] &gt; arr[k]</code> với mọi chỉ số <code>k</code> nằm giữa <code>i</code> và <code>j</code> (cụ thể là <code>min(i,&nbsp;j) &lt; k &lt; max(i, j)</code>).</p>

<p>Bạn có thể chọn bất kỳ chỉ số nào trong mảng để bắt đầu nhảy. Trả về <em>số chỉ số tối đa</em>&nbsp;mà bạn có thể ghé thăm.</p>

<p>Lưu ý rằng bạn không thể nhảy ra ngoài mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1340.Jump%20Game%20V/images/meta-chart.jpeg" style="width: 633px; height: 419px;" />
<pre>
<strong>Đầu vào:</strong> arr = [6,4,14,6,8,13,9,7,10,6,12], d = 2
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Bạn có thể bắt đầu ở chỉ số 10 và nhảy theo đường đi 10 --&gt; 8 --&gt; 6 --&gt; 7 như hình.
Lưu ý, nếu bắt đầu ở chỉ số 6 thì bạn chỉ có thể nhảy đến chỉ số 7. Bạn không thể nhảy đến chỉ số 5 vì 13 &gt; 9. Bạn cũng không thể nhảy đến chỉ số 4 vì chỉ số 5 nằm giữa chỉ số 4 và 6, đồng thời 13 &gt; 9.
Tương tự, bạn không thể nhảy từ chỉ số 3 đến chỉ số 2 hoặc 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [3,3,3,3,3], d = 3
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Bạn có thể bắt đầu ở bất kỳ chỉ số nào, nhưng không thể nhảy đến chỉ số nào khác.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [7,6,5,4,3,2,1], d = 1
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Bắt đầu ở chỉ số 0, bạn có thể ghé thăm tất cả các chỉ số. 
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 1000</code></li>
	<li><code>1 &lt;= arr[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= d &lt;= arr.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Từ $i$, ta có thể nhảy xa nhất $d$ vị trí đến một vị trí có giá trị nhỏ hơn nghiêm ngặt, miễn là không có giá trị cao hơn nằm giữa; mục tiêu là tìm hành trình dài nhất từ mọi vị trí bắt đầu. Vì $n \le 1000$, có thể dùng $dfs(i)$ với memoization: quét sang trái và phải cho đến khi ra khỏi phạm vi hoặc gặp cột không thấp hơn, gọi đệ quy rồi cộng thêm một. Đáp án là giá trị lớn nhất trong tất cả vị trí bắt đầu.

<!-- thinking:end -->

Ta định nghĩa hàm $\text{dfs}(i)$ là số chỉ số tối đa có thể ghé thăm khi bắt đầu tại chỉ số $i$. Ta xét các đích nhảy hợp lệ $j$ của $i$, với $i - d \leq j \leq i + d$ và $\text{arr}[i] > \text{arr}[j]$. Với mỗi $j$ hợp lệ, ta tính đệ quy $\text{dfs}(j)$ và lấy giá trị lớn nhất. Đáp án là giá trị lớn nhất của $\text{dfs}(i)$ trên mọi chỉ số $i$.

Ta có thể tối ưu bằng tìm kiếm có ghi nhớ: dùng mảng $f$ lưu giá trị $\text{dfs}$ cho mỗi chỉ số để tránh tính toán lặp lại.

Độ phức tạp thời gian là $O(n \times d)$ và độ phức tạp không gian là $O(n)$, với $n$ là độ dài của mảng $\text{arr}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxJumps(self, arr: List[int], d: int) -> int:
        @cache
        def dfs(i):
            ans = 1
            for j in range(i - 1, -1, -1):
                if i - j > d or arr[j] >= arr[i]:
                    break
                ans = max(ans, 1 + dfs(j))
            for j in range(i + 1, n):
                if j - i > d or arr[j] >= arr[i]:
                    break
                ans = max(ans, 1 + dfs(j))
            return ans

        n = len(arr)
        return max(dfs(i) for i in range(n))
```

#### Java

```java
class Solution {
    private int n;
    private int d;
    private int[] arr;
    private Integer[] f;

    public int maxJumps(int[] arr, int d) {
        n = arr.length;
        this.d = d;
        this.arr = arr;
        f = new Integer[n];
        int ans = 1;
        for (int i = 0; i < n; ++i) {
            ans = Math.max(ans, dfs(i));
        }
        return ans;
    }

    private int dfs(int i) {
        if (f[i] != null) {
            return f[i];
        }
        int ans = 1;
        for (int j = i - 1; j >= 0; --j) {
            if (i - j > d || arr[j] >= arr[i]) {
                break;
            }
            ans = Math.max(ans, 1 + dfs(j));
        }
        for (int j = i + 1; j < n; ++j) {
            if (j - i > d || arr[j] >= arr[i]) {
                break;
            }
            ans = Math.max(ans, 1 + dfs(j));
        }
        return f[i] = ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxJumps(vector<int>& arr, int d) {
        int n = arr.size();
        int f[n];
        memset(f, 0, sizeof(f));
        auto dfs = [&](this auto&& dfs, int i) -> int {
            if (f[i]) {
                return f[i];
            }
            int ans = 1;
            for (int j = i - 1; j >= 0; --j) {
                if (i - j > d || arr[j] >= arr[i]) {
                    break;
                }
                ans = max(ans, 1 + dfs(j));
            }
            for (int j = i + 1; j < n; ++j) {
                if (j - i > d || arr[j] >= arr[i]) {
                    break;
                }
                ans = max(ans, 1 + dfs(j));
            }
            return f[i] = ans;
        };
        int ans = 1;
        for (int i = 0; i < n; ++i) {
            ans = max(ans, dfs(i));
        }
        return ans;
    }
};
```

#### Go

```go
func maxJumps(arr []int, d int) (ans int) {
	n := len(arr)
	f := make([]int, n)
	var dfs func(int) int
	dfs = func(i int) int {
		if f[i] != 0 {
			return f[i]
		}
		ans := 1
		for j := i - 1; j >= 0; j-- {
			if i-j > d || arr[j] >= arr[i] {
				break
			}
			ans = max(ans, 1+dfs(j))
		}
		for j := i + 1; j < n; j++ {
			if j-i > d || arr[j] >= arr[i] {
				break
			}
			ans = max(ans, 1+dfs(j))
		}
		f[i] = ans
		return ans
	}
	for i := 0; i < n; i++ {
		ans = max(ans, dfs(i))
	}
	return
}
```

#### TypeScript

```ts
function maxJumps(arr: number[], d: number): number {
    const n = arr.length;
    const f: number[] = new Array(n).fill(0);
    const dfs = (i: number): number => {
        if (f[i] !== 0) {
            return f[i];
        }
        let ans = 1;
        for (let j = i - 1; j >= 0; j--) {
            if (i - j > d || arr[j] >= arr[i]) {
                break;
            }
            ans = Math.max(ans, 1 + dfs(j));
        }
        for (let j = i + 1; j < n; j++) {
            if (j - i > d || arr[j] >= arr[i]) {
                break;
            }
            ans = Math.max(ans, 1 + dfs(j));
        }
        f[i] = ans;
        return ans;
    };
    let ans = 0;
    for (let i = 0; i < n; i++) {
        ans = Math.max(ans, dfs(i));
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_jumps(arr: Vec<i32>, d: i32) -> i32 {
        fn dfs(i: usize, n: usize, d: usize, arr: &[i32], f: &mut Vec<i32>) -> i32 {
            if f[i] != 0 {
                return f[i];
            }
            let mut ans = 1;
            let mut j = (i as isize) - 1;
            while j >= 0 {
                if i - (j as usize) > d || arr[j as usize] >= arr[i] {
                    break;
                }
                ans = ans.max(1 + dfs(j as usize, n, d, arr, f));
                j -= 1;
            }
            j = (i as isize) + 1;
            while (j as usize) < n {
                if j as usize - i > d || arr[j as usize] >= arr[i] {
                    break;
                }
                ans = ans.max(1 + dfs(j as usize, n, d, arr, f));
                j += 1;
            }
            f[i] = ans;
            ans
        }
        let n = arr.len();
        let d = d as usize;
        let mut f = vec![0; n];
        let mut ans = 0;
        for i in 0..n {
            ans = ans.max(dfs(i, n, d, &arr, &mut f));
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Sắp xếp + Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Memoization tính các trạng thái theo thứ tự gọi hàm. Nếu điền $f[i]$ từ các cột thấp đến cao, mọi $j$ hợp lệ đều đã được tính, nên ta có thể dùng cùng công thức chuyển trạng thái trong quy hoạch động lặp mà không cần recursion stack.

<!-- thinking:end -->

Ta ghép mỗi phần tử $x$ trong mảng $\text{arr}$ với chỉ số $i$ của nó để tạo tuple $(x, i)$, rồi sắp xếp các tuple này theo $x$ tăng dần.

Tiếp theo, định nghĩa $f[i]$ là số chỉ số tối đa có thể ghé thăm khi bắt đầu tại chỉ số $i$. Ban đầu, $f[i] = 1$, vì mỗi chỉ số có thể được ghé thăm riêng lẻ.

Ta duyệt các chỉ số $i$ theo thứ tự tuple $(x, i)$, đồng thời xét mọi đích nhảy hợp lệ $j$ của $i$, tức là $i - d \leq j \leq i + d$ và $\text{arr}[i] > \text{arr}[j]$. Với mỗi $j$ hợp lệ, ta cập nhật $f[i]$ theo công thức $f[i] = \max(f[i], 1 + f[j])$.

Đáp án cuối cùng là $\max_{0 \leq i < n} f[i]$.

Độ phức tạp thời gian là $O(n \log n + n \times d)$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $\text{arr}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxJumps(self, arr: List[int], d: int) -> int:
        n = len(arr)
        f = [1] * n
        for x, i in sorted(zip(arr, range(n))):
            for j in range(i - 1, -1, -1):
                if i - j > d or arr[j] >= x:
                    break
                f[i] = max(f[i], 1 + f[j])
            for j in range(i + 1, n):
                if j - i > d or arr[j] >= x:
                    break
                f[i] = max(f[i], 1 + f[j])
        return max(f)
```

#### Java

```java
class Solution {
    public int maxJumps(int[] arr, int d) {
        int n = arr.length;
        Integer[] idx = new Integer[n];
        Arrays.setAll(idx, i -> i);
        Arrays.sort(idx, (i, j) -> arr[i] - arr[j]);
        int[] f = new int[n];
        Arrays.fill(f, 1);
        int ans = 0;
        for (int i : idx) {
            for (int j = i - 1; j >= 0; --j) {
                if (i - j > d || arr[j] >= arr[i]) {
                    break;
                }
                f[i] = Math.max(f[i], 1 + f[j]);
            }
            for (int j = i + 1; j < n; ++j) {
                if (j - i > d || arr[j] >= arr[i]) {
                    break;
                }
                f[i] = Math.max(f[i], 1 + f[j]);
            }
            ans = Math.max(ans, f[i]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxJumps(vector<int>& arr, int d) {
        int n = arr.size();
        vector<int> idx(n);
        iota(idx.begin(), idx.end(), 0);
        sort(idx.begin(), idx.end(), [&](int i, int j) { return arr[i] < arr[j]; });
        vector<int> f(n, 1);
        for (int i : idx) {
            for (int j = i - 1; j >= 0; --j) {
                if (i - j > d || arr[j] >= arr[i]) {
                    break;
                }
                f[i] = max(f[i], 1 + f[j]);
            }
            for (int j = i + 1; j < n; ++j) {
                if (j - i > d || arr[j] >= arr[i]) {
                    break;
                }
                f[i] = max(f[i], 1 + f[j]);
            }
        }
        return ranges::max(f);
    }
};
```

#### Go

```go
func maxJumps(arr []int, d int) int {
	n := len(arr)
	idx := make([]int, n)
	f := make([]int, n)
	for i := range f {
		idx[i] = i
		f[i] = 1
	}
	sort.Slice(idx, func(i, j int) bool { return arr[idx[i]] < arr[idx[j]] })
	for _, i := range idx {
		for j := i - 1; j >= 0; j-- {
			if i-j > d || arr[j] >= arr[i] {
				break
			}
			f[i] = max(f[i], 1+f[j])
		}
		for j := i + 1; j < n; j++ {
			if j-i > d || arr[j] >= arr[i] {
				break
			}
			f[i] = max(f[i], 1+f[j])
		}
	}
	return slices.Max(f)
}
```

#### TypeScript

```ts
function maxJumps(arr: number[], d: number): number {
    const n = arr.length;
    const f: number[] = new Array(n).fill(1);
    const idx: number[] = Array.from({ length: n }, (_, i) => i);
    idx.sort((a, b) => arr[a] - arr[b]);
    for (const i of idx) {
        for (let j = i - 1; j >= 0; j--) {
            if (i - j > d || arr[j] >= arr[i]) {
                break;
            }
            f[i] = Math.max(f[i], 1 + f[j]);
        }
        for (let j = i + 1; j < n; j++) {
            if (j - i > d || arr[j] >= arr[i]) {
                break;
            }
            f[i] = Math.max(f[i], 1 + f[j]);
        }
    }
    return Math.max(...f);
}
```

#### Rust

```rust
impl Solution {
    pub fn max_jumps(arr: Vec<i32>, d: i32) -> i32 {
        let n = arr.len();
        let d = d as usize;

        let mut idx: Vec<usize> = (0..n).collect();
        idx.sort_by_key(|&i| arr[i]);

        let mut f = vec![1; n];

        for &i in &idx {
            let mut j = i as i32 - 1;
            while j >= 0 {
                let k = j as usize;

                if i - k > d || arr[k] >= arr[i] {
                    break;
                }

                f[i] = f[i].max(1 + f[k]);
                j -= 1;
            }

            let mut j = i + 1;
            while j < n {
                if j - i > d || arr[j] >= arr[i] {
                    break;
                }

                f[i] = f[i].max(1 + f[j]);
                j += 1;
            }
        }

        *f.iter().max().unwrap()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
