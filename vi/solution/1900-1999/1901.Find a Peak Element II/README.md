---
comments: true
difficulty: Medium
tags:
    - Array
    - Binary Search
    - Matrix
---

<!-- problem:start -->

# [1901. Find a Peak Element II](https://leetcode.com/problems/find-a-peak-element-ii)

[中文文档](/solution/1900-1999/1901.Find%20a%20Peak%20Element%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Một phần tử <strong>cực đại</strong> trong lưới 2D là phần tử <strong>lớn hơn nghiêm ngặt</strong> tất cả các phần tử <strong>kề</strong> với nó ở bên trái, bên phải, phía trên và phía dưới.</p>

<p>Cho ma trận <code>mat</code> kích thước <code>m x n</code>, <strong>được đánh chỉ số từ 0</strong>, trong đó <strong>không có hai ô kề nhau nào bằng nhau</strong>, hãy tìm <strong>bất kỳ</strong> phần tử cực đại <code>mat[i][j]</code> và trả về <em>mảng có độ dài 2</em> <code>[i,j]</code>.</p>

<p>Có thể giả sử toàn bộ ma trận được bao quanh bởi một <strong>biên ngoài</strong>, trong đó giá trị của mỗi ô là <code>-1</code>.</p>

<p>Bạn phải viết thuật toán có thời gian chạy <code>O(m log(n))</code> hoặc <code>O(n log(m))</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1901.Find%20a%20Peak%20Element%20II/images/1.png" style="width: 206px; height: 209px;" /></p>

<pre>
<strong>Đầu vào:</strong> mat = [[1,4],[3,2]]
<strong>Đầu ra:</strong> [0,1]
<strong>Giải thích:</strong>&nbsp;Cả 3 và 4 đều là phần tử cực đại, nên [1,0] và [0,1] đều là đáp án hợp lệ.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1901.Find%20a%20Peak%20Element%20II/images/3.png" style="width: 254px; height: 257px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> mat = [[10,20,15],[21,30,14],[7,16,32]]
<strong>Đầu ra:</strong> [1,1]
<strong>Giải thích:</strong>&nbsp;Cả 30 và 32 đều là phần tử cực đại, nên [1,1] và [2,2] đều là đáp án hợp lệ.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == mat.length</code></li>
	<li><code>n == mat[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 500</code></li>
	<li><code>1 &lt;= mat[i][j] &lt;= 10<sup>5</sup></code></li>
	<li>Không có hai ô kề nhau nào bằng nhau.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Duyệt từng ô và so sánh với bốn ô kề tìm được một cực đại trong $O(mn)$, nhưng giới hạn yêu cầu là $O(n\log m)$ hoặc $O(m\log n)$.
>
> Gọi $mat[i][j]$ là phần tử lớn nhất của hàng $i$. So sánh nó với $mat[i+1][j]$ cho biết nửa nào chắc chắn chứa một cực đại: nếu giảm thì giữ nửa trên, nếu tăng thì giữ nửa dưới. Nếu nửa đó không có cực đại, phần tử lớn nhất ở hàng biên sẽ mâu thuẫn với giá trị biên $-1$ bên ngoài ma trận.
>
> Vì vậy, ta tìm kiếm nhị phân chỉ số hàng; mỗi lần lấy cột $j$ chứa phần tử lớn nhất của hàng giữa và thu hẹp theo phép so sánh theo chiều dọc, đạt thời gian $O(n\log m)$.

<!-- thinking:end -->

Gọi $m$ và $n$ lần lượt là số hàng và số cột của ma trận.

Bài toán yêu cầu tìm một cực đại với độ phức tạp thời gian $O(m \times \log n)$ hoặc $O(n \times \log m)$. Vì vậy, ta có thể cân nhắc sử dụng tìm kiếm nhị phân.

Ta xét giá trị lớn nhất của hàng thứ $i$ và gọi chỉ số của nó là $j$.

Nếu $mat[i][j] > mat[i + 1][j]$, chắc chắn có một cực đại trong các hàng $[0,..i]$. Ta chỉ cần tìm giá trị lớn nhất trong các hàng này. Tương tự, nếu $mat[i][j] < mat[i + 1][j]$, chắc chắn có một cực đại trong các hàng $[i + 1,..m - 1]$. Ta chỉ cần tìm giá trị lớn nhất trong các hàng này.

Tại sao phương pháp trên đúng? Ta có thể chứng minh bằng phản chứng.

Nếu $mat[i][j] > mat[i + 1][j]$, giả sử không có cực đại nào trong các hàng $[0,..i]$. Khi đó $mat[i][j]$ không phải là cực đại. Vì $mat[i][j]$ là giá trị lớn nhất của hàng thứ $i$ và $mat[i][j] > mat[i + 1][j]$, nên $mat[i][j] < mat[i - 1][j]$. Ta tiếp tục xét từ hàng thứ $(i - 1)$ trở lên, và giá trị lớn nhất của mỗi hàng nhỏ hơn giá trị lớn nhất của hàng trước đó. Khi duyệt đến $i = 0$, vì mọi phần tử trong ma trận đều là số nguyên dương và các ô bao quanh ma trận có giá trị $-1$, nên ở hàng thứ 0, giá trị lớn nhất của hàng đó lớn hơn tất cả các phần tử kề nó. Do đó, giá trị lớn nhất của hàng thứ 0 là một cực đại, mâu thuẫn với giả sử ban đầu. Vậy phải có một cực đại trong các hàng $[0,..i]$.

Với trường hợp $mat[i][j] < mat[i + 1][j]$, ta có thể chứng minh tương tự rằng phải có một cực đại trong các hàng $[i + 1,..m - 1]$.

Vì vậy, ta có thể sử dụng tìm kiếm nhị phân để tìm cực đại.

Ta tìm kiếm nhị phân trên các hàng của ma trận, ban đầu với biên tìm kiếm $l = 0$, $r = m - 1$. Mỗi lần, ta tìm hàng giữa $mid$ và tìm chỉ số $j$ của giá trị lớn nhất trong hàng này. Nếu $mat[mid][j] > mat[mid + 1][j]$, ta tìm cực đại trong các hàng $[0,..mid]$, tức là cập nhật $r = mid$. Ngược lại, ta tìm cực đại trong các hàng $[mid + 1,..m - 1]$, tức là cập nhật $l = mid + 1$. Khi $l = r$, ta tìm được vị trí $[l, j_l]$ của cực đại, trong đó $j_l$ là chỉ số của giá trị lớn nhất trong hàng thứ $l$.

Độ phức tạp thời gian là $O(n \times \log m)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột của ma trận. Tìm kiếm nhị phân có độ phức tạp $O(\log m)$, và mỗi bước tìm kiếm, ta cần duyệt tất cả phần tử của hàng $mid$, với độ phức tạp $O(n)$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findPeakGrid(self, mat: List[List[int]]) -> List[int]:
        l, r = 0, len(mat) - 1
        while l < r:
            mid = (l + r) >> 1
            j = mat[mid].index(max(mat[mid]))
            if mat[mid][j] > mat[mid + 1][j]:
                r = mid
            else:
                l = mid + 1
        return [l, mat[l].index(max(mat[l]))]
```

#### Java

```java
class Solution {
    public int[] findPeakGrid(int[][] mat) {
        int l = 0, r = mat.length - 1;
        int n = mat[0].length;
        while (l < r) {
            int mid = (l + r) >> 1;
            int j = maxPos(mat[mid]);
            if (mat[mid][j] > mat[mid + 1][j]) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return new int[] {l, maxPos(mat[l])};
    }

    private int maxPos(int[] arr) {
        int j = 0;
        for (int i = 1; i < arr.length; ++i) {
            if (arr[j] < arr[i]) {
                j = i;
            }
        }
        return j;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> findPeakGrid(vector<vector<int>>& mat) {
        int l = 0, r = mat.size() - 1;
        while (l < r) {
            int mid = (l + r) >> 1;
            int j = distance(mat[mid].begin(), max_element(mat[mid].begin(), mat[mid].end()));
            if (mat[mid][j] > mat[mid + 1][j]) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        int j = distance(mat[l].begin(), max_element(mat[l].begin(), mat[l].end()));
        return {l, j};
    }
};
```

#### Go

```go
func findPeakGrid(mat [][]int) []int {
	maxPos := func(arr []int) int {
		j := 0
		for i := 1; i < len(arr); i++ {
			if arr[i] > arr[j] {
				j = i
			}
		}
		return j
	}
	l, r := 0, len(mat)-1
	for l < r {
		mid := (l + r) >> 1
		j := maxPos(mat[mid])
		if mat[mid][j] > mat[mid+1][j] {
			r = mid
		} else {
			l = mid + 1
		}
	}
	return []int{l, maxPos(mat[l])}
}
```

#### TypeScript

```ts
function findPeakGrid(mat: number[][]): number[] {
    let [l, r] = [0, mat.length - 1];
    while (l < r) {
        const mid = (l + r) >> 1;
        const j = mat[mid].indexOf(Math.max(...mat[mid]));
        if (mat[mid][j] > mat[mid + 1][j]) {
            r = mid;
        } else {
            l = mid + 1;
        }
    }
    return [l, mat[l].indexOf(Math.max(...mat[l]))];
}
```

#### Rust

```rust
impl Solution {
    pub fn find_peak_grid(mat: Vec<Vec<i32>>) -> Vec<i32> {
        let mut l: usize = 0;
        let mut r: usize = mat.len() - 1;
        while l < r {
            let mid: usize = (l + r) >> 1;
            let j: usize = mat[mid]
                .iter()
                .position(|&x| x == *mat[mid].iter().max().unwrap())
                .unwrap();
            if mat[mid][j] > mat[mid + 1][j] {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        let j: usize = mat[l]
            .iter()
            .position(|&x| x == *mat[l].iter().max().unwrap())
            .unwrap();
        vec![l as i32, j as i32]
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
