---
comments: true
difficulty: Medium
rating: 1774
source: Weekly Contest 160 Q2
tags:
    - Bit Manipulation
    - Math
    - Backtracking
---

<!-- problem:start -->

# [1238. Circular Permutation in Binary Representation](https://leetcode.com/problems/circular-permutation-in-binary-representation)

[中文文档](/solution/1200-1299/1238.Circular%20Permutation%20in%20Binary%20Representation/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên <code>n</code> và <code>start</code>. Nhiệm vụ của bạn là trả về <strong>bất kỳ</strong> hoán vị <code>p</code> nào của <code>(0,1,2.....,2^n -1) </code>thỏa mãn:</p>

<ul>
	<li><code>p[0] = start</code></li>
	<li><code>p[i]</code> và <code>p[i+1]</code> chỉ khác nhau đúng một bit trong biểu diễn nhị phân.</li>
	<li><code>p[0]</code> và <code>p[2^n -1]</code> cũng phải chỉ khác nhau đúng một bit trong biểu diễn nhị phân.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2, start = 3
<strong>Đầu ra:</strong> [3,2,0,1]
<strong>Giải thích:</strong> Biểu diễn nhị phân của hoán vị là (11,10,00,01). 
Mọi cặp phần tử kề nhau chỉ khác một bit. Một hoán vị hợp lệ khác là [3,1,0,2]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3, start = 2
<strong>Đầu ra:</strong> [2,6,7,5,4,0,1,3]
<strong>Giải thích:</strong> Biểu diễn nhị phân của hoán vị là (010,110,111,101,100,000,001,011).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 16</code></li>
	<li><code>0 &lt;= start&nbsp;&lt;&nbsp;2 ^ n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Chuyển mã nhị phân sang Gray code

<!-- thinking:start -->

> **Tư duy**
>
> Các giá trị kề nhau (kể cả cặp cuối và đầu khi quay vòng) chỉ khác nhau một bit; đó là đặc trưng của Gray code. Biểu thức $i\oplus(i\gg 1)$ tạo ra một chu trình Gray trên các giá trị từ $0$ đến $2^n-1$. Vì $n \le 16$, ta có thể liệt kê mọi mã.
>
> Sau khi tạo dãy, ta tìm vị trí của $start$ rồi xoay dãy để đưa nó lên đầu; tính kề nhau theo vòng vẫn được bảo toàn.

<!-- thinking:end -->

Quan sát hoán vị trong đề bài, ta thấy biểu diễn nhị phân của hai số bất kỳ đứng cạnh nhau (kể cả số đầu và số cuối) chỉ khác nhau một bit. Đây là đặc điểm của Gray code, một phương pháp mã hóa thường gặp trong kỹ thuật.

Quy tắc chuyển mã nhị phân sang Gray code là giữ nguyên bit cao nhất; bit cao thứ hai của Gray code bằng XOR giữa hai bit cao nhất của mã nhị phân. Các bit còn lại của Gray code được tạo theo cách tương tự.

Giả sử số nhị phân được biểu diễn là $B_{n-1}B_{n-2}...B_2B_1B_0$, còn biểu diễn Gray code là $G_{n-1}G_{n-2}...G_2G_1G_0$. Bit cao nhất được giữ nguyên, nên $G_{n-1} = B_{n-1}$; các bit còn lại được tính theo $G_i = B_{i+1} \oplus B_{i}$, với $i=0,1,2..,n-2$.

Vì vậy, với số nguyên $x$, ta có thể dùng hàm $gray(x)$ để lấy Gray code tương ứng:

```java
int gray(x) {
    return x ^ (x >> 1);
}
```

Ta có thể chuyển trực tiếp các số nguyên từ $0$ đến $2^n - 1$ thành mảng Gray code tương ứng, sau đó tìm vị trí của $start$ trong mảng. Xoay mảng từ vị trí đó để $start$ đứng đầu sẽ thu được hoán vị cần tìm.

Độ phức tạp thời gian và không gian đều là $O(2^n)$, trong đó $n$ là số nguyên được cho trong đề bài.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def circularPermutation(self, n: int, start: int) -> List[int]:
        g = [i ^ (i >> 1) for i in range(1 << n)]
        j = g.index(start)
        return g[j:] + g[:j]
```

#### Java

```java
class Solution {
    public List<Integer> circularPermutation(int n, int start) {
        int[] g = new int[1 << n];
        int j = 0;
        for (int i = 0; i < 1 << n; ++i) {
            g[i] = i ^ (i >> 1);
            if (g[i] == start) {
                j = i;
            }
        }
        List<Integer> ans = new ArrayList<>();
        for (int i = j; i < j + (1 << n); ++i) {
            ans.add(g[i % (1 << n)]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> circularPermutation(int n, int start) {
        int g[1 << n];
        int j = 0;
        for (int i = 0; i < 1 << n; ++i) {
            g[i] = i ^ (i >> 1);
            if (g[i] == start) {
                j = i;
            }
        }
        vector<int> ans;
        for (int i = j; i < j + (1 << n); ++i) {
            ans.push_back(g[i % (1 << n)]);
        }
        return ans;
    }
};
```

#### Go

```go
func circularPermutation(n int, start int) []int {
	g := make([]int, 1<<n)
	j := 0
	for i := range g {
		g[i] = i ^ (i >> 1)
		if g[i] == start {
			j = i
		}
	}
	return append(g[j:], g[:j]...)
}
```

#### TypeScript

```ts
function circularPermutation(n: number, start: number): number[] {
    const ans: number[] = [];
    for (let i = 0; i < 1 << n; ++i) {
        ans.push(i ^ (i >> 1) ^ start);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Tối ưu phép chuyển đổi

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 tạo dãy rồi mới xoay. Các giá trị $gray(i)\oplus start$ vẫn chỉ khác nhau một bit giữa hai phần tử kề nhau, đồng thời bằng $start$ khi $i=0$. Vì vậy, ta có thể ánh xạ trực tiếp từng $i$ và bỏ qua bước tìm vị trí rồi nối mảng.

<!-- thinking:end -->

Vì $gray(0) = 0$, nên $gray(0) \oplus start = start$. Mặt khác, $gray(i)$ chỉ khác $gray(i-1)$ một bit, do đó $gray(i) \oplus start$ cũng chỉ khác $gray(i-1) \oplus start$ một bit.

Do đó, ta có thể chuyển trực tiếp các số nguyên từ $0$ đến $2^n - 1$ thành các giá trị $gray(i) \oplus start$ tương ứng để tạo hoán vị Gray code bắt đầu bằng $start$.

Độ phức tạp thời gian là $O(2^n)$, trong đó $n$ là số nguyên được cho trong đề bài. Nếu không tính phần không gian dùng cho đáp án, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def circularPermutation(self, n: int, start: int) -> List[int]:
        return [i ^ (i >> 1) ^ start for i in range(1 << n)]
```

#### Java

```java
class Solution {
    public List<Integer> circularPermutation(int n, int start) {
        List<Integer> ans = new ArrayList<>();
        for (int i = 0; i < 1 << n; ++i) {
            ans.add(i ^ (i >> 1) ^ start);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> circularPermutation(int n, int start) {
        vector<int> ans(1 << n);
        for (int i = 0; i < 1 << n; ++i) {
            ans[i] = i ^ (i >> 1) ^ start;
        }
        return ans;
    }
};
```

#### Go

```go
func circularPermutation(n int, start int) (ans []int) {
	for i := 0; i < 1<<n; i++ {
		ans = append(ans, i^(i>>1)^start)
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
