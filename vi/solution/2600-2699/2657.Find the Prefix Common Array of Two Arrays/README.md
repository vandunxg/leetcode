---
comments: true
difficulty: Medium
rating: 1304
source: Biweekly Contest 103 Q2
tags:
    - Bit Manipulation
    - Array
    - Hash Table
---

<!-- problem:start -->

# [2657. Find the Prefix Common Array of Two Arrays](https://leetcode.com/problems/find-the-prefix-common-array-of-two-arrays)

[中文文档](/solution/2600-2699/2657.Find%20the%20Prefix%20Common%20Array%20of%20Two%20Arrays/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai <strong>mảng được đánh chỉ số từ 0, là các </strong>hoán vị<strong> số nguyên </strong><code>A</code> và <code>B</code> có độ dài <code>n</code>.</p>

<p><strong>Mảng chung tiền tố</strong> của <code>A</code> và <code>B</code> là một mảng <code>C</code> sao cho <code>C[i]</code> bằng số lượng các số xuất hiện tại hoặc trước chỉ số <code>i</code> trong cả <code>A</code> và <code>B</code>.</p>

<p>Trả về <em><strong>mảng chung tiền tố</strong> của </em><code>A</code><em> và </em><code>B</code>.</p>

<p>Một dãy gồm <code>n</code> số nguyên được gọi là một&nbsp;<strong>hoán vị</strong> nếu nó chứa tất cả các số nguyên từ <code>1</code> đến <code>n</code> đúng một lần.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> A = [1,3,2,4], B = [3,1,2,4]
<strong>Đầu ra:</strong> [0,2,3,4]
<strong>Giải thích:</strong> Với i = 0: không có số nào chung, nên C[0] = 0.
Với i = 1: 1 và 3 xuất hiện trong cả A và B, nên C[1] = 2.
Với i = 2: 1, 2 và 3 xuất hiện trong cả A và B, nên C[2] = 3.
Với i = 3: 1, 2, 3 và 4 xuất hiện trong cả A và B, nên C[3] = 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> A = [2,3,1], B = [3,1,2]
<strong>Đầu ra:</strong> [0,1,3]
<strong>Giải thích:</strong> Với i = 0: không có số nào chung, nên C[0] = 0.
Với i = 1: chỉ có 3 xuất hiện trong cả A và B, nên C[1] = 1.
Với i = 2: 1, 2 và 3 xuất hiện trong cả A và B, nên C[2] = 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= A.length == B.length == n &lt;= 50</code></li>
	<li><code>1 &lt;= A[i], B[i] &lt;= n</code></li>
	<li><code>It is guaranteed that A and B are both a permutation of n integers.</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần kích thước phần giao của hai tiền tố trong các hoán vị. Việc xây dựng lại các set ở mỗi bước vẫn phù hợp với $n \le 50$, nhưng mỗi tiền tố chỉ tăng thêm một phần tử.
>
> Đếm số lần xuất hiện trong $A$ và $B$, sau đó tính tổng $\min$ trên $[1,n]$ để thu được số lượng phần tử chung hiện tại.

<!-- thinking:end -->

Ta có thể sử dụng hai mảng $cnt1$ và $cnt2$ để ghi nhận số lần xuất hiện của mỗi phần tử trong hai mảng $A$ và $B$, đồng thời sử dụng mảng $ans$ để lưu đáp án.

Duyệt qua hai mảng $A$ và $B$, tăng số lần xuất hiện của $A[i]$ trong $cnt1$ và tăng số lần xuất hiện của $B[i]$ trong $cnt2$. Sau đó, duyệt qua $j \in [1,n]$, tính số lần xuất hiện nhỏ hơn giữa mỗi phần tử $j$ trong $cnt1$ và $cnt2$, rồi cộng dồn vào $ans[i]$.

Sau khi duyệt xong, trả về mảng đáp án $ans$.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của hai mảng $A$ và $B$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findThePrefixCommonArray(self, A: List[int], B: List[int]) -> List[int]:
        ans = []
        cnt1 = Counter()
        cnt2 = Counter()
        for a, b in zip(A, B):
            cnt1[a] += 1
            cnt2[b] += 1
            t = sum(min(v, cnt2[x]) for x, v in cnt1.items())
            ans.append(t)
        return ans
```

#### Java

```java
class Solution {
    public int[] findThePrefixCommonArray(int[] A, int[] B) {
        int n = A.length;
        int[] ans = new int[n];
        int[] cnt1 = new int[n + 1];
        int[] cnt2 = new int[n + 1];
        for (int i = 0; i < n; ++i) {
            ++cnt1[A[i]];
            ++cnt2[B[i]];
            for (int j = 1; j <= n; ++j) {
                ans[i] += Math.min(cnt1[j], cnt2[j]);
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
    vector<int> findThePrefixCommonArray(vector<int>& A, vector<int>& B) {
        int n = A.size();
        vector<int> ans(n);
        vector<int> cnt1(n + 1), cnt2(n + 1);
        for (int i = 0; i < n; ++i) {
            ++cnt1[A[i]];
            ++cnt2[B[i]];
            for (int j = 1; j <= n; ++j) {
                ans[i] += min(cnt1[j], cnt2[j]);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findThePrefixCommonArray(A []int, B []int) []int {
	n := len(A)
	cnt1 := make([]int, n+1)
	cnt2 := make([]int, n+1)
	ans := make([]int, n)
	for i, a := range A {
		b := B[i]
		cnt1[a]++
		cnt2[b]++
		for j := 1; j <= n; j++ {
			ans[i] += min(cnt1[j], cnt2[j])
		}
	}
	return ans
}
```

#### TypeScript

```ts
function findThePrefixCommonArray(A: number[], B: number[]): number[] {
    const n = A.length;
    const cnt1: number[] = Array(n + 1).fill(0);
    const cnt2: number[] = Array(n + 1).fill(0);
    const ans: number[] = Array(n).fill(0);
    for (let i = 0; i < n; ++i) {
        ++cnt1[A[i]];
        ++cnt2[B[i]];
        for (let j = 1; j <= n; ++j) {
            ans[i] += Math.min(cnt1[j], cnt2[j]);
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn find_the_prefix_common_array(a: Vec<i32>, b: Vec<i32>) -> Vec<i32> {
        let n = a.len();
        let mut ans = vec![0; n];
        let mut cnt1 = vec![0; n + 1];
        let mut cnt2 = vec![0; n + 1];
        for i in 0..n {
            cnt1[a[i] as usize] += 1;
            cnt2[b[i] as usize] += 1;
            for j in 1..=n {
                ans[i] += std::cmp::min(cnt1[j], cnt2[j]);
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Phép toán bit (phép toán XOR)

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 quét toàn bộ miền giá trị ở mỗi bước. Mỗi giá trị xuất hiện đúng một lần trong mỗi mảng, vì vậy việc XOR đảo cờ $vis$ sẽ tăng số lượng phần tử chung khi giá trị được gặp lần thứ hai, cho phép cập nhật $O(1)$.

<!-- thinking:end -->

Ta có thể sử dụng một mảng $vis$ có độ dài $n+1$ để ghi nhận trạng thái xuất hiện của mỗi phần tử trong hai mảng $A$ và $B$, với giá trị ban đầu của mảng $vis$ là $1$. Ngoài ra, ta sử dụng biến $s$ để ghi nhận số lượng phần tử chung hiện tại.

Tiếp theo, ta duyệt qua hai mảng $A$ và $B$, cập nhật $vis[A[i]] = vis[A[i]] \oplus 1$ và cập nhật $vis[B[i]] = vis[B[i]] \oplus 1$, trong đó $\oplus$ biểu diễn phép toán XOR.

Nếu tại vị trí hiện tại, phần tử $A[i]$ đã xuất hiện hai lần (tức là đã xuất hiện trong cả hai mảng $A$ và $B$), thì giá trị của $vis[A[i]]$ sẽ là $1$, và ta tăng $s$. Tương tự, nếu phần tử $B[i]$ đã xuất hiện hai lần, thì giá trị của $vis[B[i]]$ sẽ là $1$, và ta tăng $s$. Sau đó, thêm giá trị của $s$ vào mảng đáp án $ans$.

Sau khi duyệt xong, trả về mảng đáp án $ans$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của hai mảng $A$ và $B$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findThePrefixCommonArray(self, A: List[int], B: List[int]) -> List[int]:
        ans = []
        vis = [1] * (len(A) + 1)
        s = 0
        for a, b in zip(A, B):
            vis[a] ^= 1
            s += vis[a]
            vis[b] ^= 1
            s += vis[b]
            ans.append(s)
        return ans
```

#### Java

```java
class Solution {
    public int[] findThePrefixCommonArray(int[] A, int[] B) {
        int n = A.length;
        int[] ans = new int[n];
        int[] vis = new int[n + 1];
        Arrays.fill(vis, 1);
        int s = 0;
        for (int i = 0; i < n; ++i) {
            vis[A[i]] ^= 1;
            s += vis[A[i]];
            vis[B[i]] ^= 1;
            s += vis[B[i]];
            ans[i] = s;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> findThePrefixCommonArray(vector<int>& A, vector<int>& B) {
        int n = A.size();
        vector<int> ans;
        vector<int> vis(n + 1, 1);
        int s = 0;
        for (int i = 0; i < n; ++i) {
            vis[A[i]] ^= 1;
            s += vis[A[i]];
            vis[B[i]] ^= 1;
            s += vis[B[i]];
            ans.push_back(s);
        }
        return ans;
    }
};
```

#### Go

```go
func findThePrefixCommonArray(A []int, B []int) (ans []int) {
	vis := make([]int, len(A)+1)
	for i := range vis {
		vis[i] = 1
	}
	s := 0
	for i, a := range A {
		b := B[i]
		vis[a] ^= 1
		s += vis[a]
		vis[b] ^= 1
		s += vis[b]
		ans = append(ans, s)
	}
	return
}
```

#### TypeScript

```ts
function findThePrefixCommonArray(A: number[], B: number[]): number[] {
    const n = A.length;
    const vis: number[] = Array(n + 1).fill(1);
    const ans: number[] = [];
    let s = 0;
    for (let i = 0; i < n; ++i) {
        const [a, b] = [A[i], B[i]];
        vis[a] ^= 1;
        s += vis[a];
        vis[b] ^= 1;
        s += vis[b];
        ans.push(s);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn find_the_prefix_common_array(a: Vec<i32>, b: Vec<i32>) -> Vec<i32> {
        let n = a.len();
        let mut ans = vec![0; n];
        let mut vis = vec![1; n + 1];
        let mut s = 0;
        for i in 0..n {
            vis[a[i] as usize] ^= 1;
            s += vis[a[i] as usize];
            vis[b[i] as usize] ^= 1;
            s += vis[b[i] as usize];
            ans[i] = s;
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3: Phép thao tác bit (tối ưu bộ nhớ)

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 2 vẫn sử dụng một mảng tuyến tính. Với $n \le 50$, hai bitset số nguyên là đủ: OR trên tiền tố cộng với `bit_count(x & y)` cho ta kích thước phần giao với lượng bộ nhớ bổ sung hằng số.

<!-- thinking:end -->

Vì các phần tử của hai mảng $A$ và $B$ nằm trong khoảng $[1, n]$ và không vượt quá $50$, ta có thể sử dụng một số nguyên $x$ và một số nguyên $y$ để biểu diễn lần lượt trạng thái xuất hiện của mỗi phần tử trong hai mảng $A$ và $B$. Cụ thể, ta sử dụng bit thứ $i$ của số nguyên $x$ để cho biết phần tử $i$ đã xuất hiện trong mảng $A$ hay chưa, và bit thứ $i$ của số nguyên $y$ để cho biết phần tử $i$ đã xuất hiện trong mảng $B$ hay chưa.

Độ phức tạp thời gian của lời giải này là $O(n)$, trong đó $n$ là độ dài của hai mảng $A$ và $B$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findThePrefixCommonArray(self, A: List[int], B: List[int]) -> List[int]:
        ans = []
        x = y = 0
        for a, b in zip(A, B):
            x |= 1 << a
            y |= 1 << b
            ans.append((x & y).bit_count())
        return ans
```

#### Java

```java
class Solution {
    public int[] findThePrefixCommonArray(int[] A, int[] B) {
        int n = A.length;
        int[] ans = new int[n];
        long x = 0, y = 0;
        for (int i = 0; i < n; i++) {
            x |= 1L << A[i];
            y |= 1L << B[i];
            ans[i] = Long.bitCount(x & y);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> findThePrefixCommonArray(vector<int>& A, vector<int>& B) {
        int n = A.size();
        vector<int> ans(n);
        long long x = 0, y = 0;
        for (int i = 0; i < n; ++i) {
            x |= (1LL << A[i]);
            y |= (1LL << B[i]);
            ans[i] = __builtin_popcountll(x & y);
        }
        return ans;
    }
};
```

#### Go

```go
func findThePrefixCommonArray(A []int, B []int) []int {
	n := len(A)
	ans := make([]int, n)
	var x, y int
	for i := 0; i < n; i++ {
		x |= 1 << A[i]
		y |= 1 << B[i]
		ans[i] = bits.OnesCount(uint(x & y))
	}
	return ans
}
```

#### TypeScript

```ts
function findThePrefixCommonArray(A: number[], B: number[]): number[] {
    const n = A.length;
    const ans: number[] = [];
    let [x, y] = [0n, 0n];
    for (let i = 0; i < n; i++) {
        x |= 1n << BigInt(A[i]);
        y |= 1n << BigInt(B[i]);
        ans.push(bitCount64(x & y));
    }
    return ans;
}

function bitCount64(i: bigint): number {
    i = i - ((i >> 1n) & 0x5555555555555555n);
    i = (i & 0x3333333333333333n) + ((i >> 2n) & 0x3333333333333333n);
    i = (i + (i >> 4n)) & 0x0f0f0f0f0f0f0f0fn;
    i = i + (i >> 8n);
    i = i + (i >> 16n);
    i = i + (i >> 32n);
    return Number(i & 0x7fn);
}
```

#### Rust

```rust
impl Solution {
    pub fn find_the_prefix_common_array(a: Vec<i32>, b: Vec<i32>) -> Vec<i32> {
        let mut ans = Vec::with_capacity(a.len());
        let (mut x, mut y): (u64, u64) = (0, 0);
        for (&a_val, &b_val) in a.iter().zip(b.iter()) {
            x |= 1 << a_val;
            y |= 1 << b_val;
            ans.push((x & y).count_ones() as i32);
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
