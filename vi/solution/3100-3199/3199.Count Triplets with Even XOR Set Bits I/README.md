---
comments: true
difficulty: Easy
tags:
    - Bit Manipulation
    - Array
---

<!-- problem:start -->

# [3199. Count Triplets with Even XOR Set Bits I 🔒](https://leetcode.com/problems/count-triplets-with-even-xor-set-bits-i)

[Tài liệu tiếng Trung](/solution/3100-3199/3199.Count%20Triplets%20with%20Even%20XOR%20Set%20Bits%20I/README.md)

## Mô tả

<!-- description:start -->

Cho ba mảng số nguyên <code>a</code>, <code>b</code> và <code>c</code>, hãy trả về số lượng bộ ba <code>(a[i], b[j], c[k])</code> sao cho phép <code>XOR</code> theo bit của các phần tử trong mỗi bộ ba có <strong>số lượng chẵn</strong> <span data-keyword="set-bit">bit 1</span>.
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">a = [1], b = [2], c = [3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Bộ ba duy nhất là <code>(a[0], b[0], c[0])</code> và phép <code>XOR</code> của chúng là: <code>1 XOR 2 XOR 3 = 00<sub>2</sub></code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">a = [1,1], b = [2,3], c = [1,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Xét bốn bộ ba sau:</p>

<ul>
    <li><code>(a[0], b[1], c[0])</code>: <code>1 XOR 3 XOR 1 = 011<sub>2</sub></code></li>
    <li><code>(a[1], b[1], c[0])</code>: <code>1 XOR 3 XOR 1 = 011<sub>2</sub></code></li>
    <li><code>(a[0], b[0], c[1])</code>: <code>1 XOR 2 XOR 5 = 110<sub>2</sub></code></li>
    <li><code>(a[1], b[0], c[1])</code>: <code>1 XOR 2 XOR 5 = 110<sub>2</sub></code></li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= a.length, b.length, c.length &lt;= 100</code></li>
    <li><code>0 &lt;= a[i], b[i], c[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các bộ ba có XOR với số bit 1 chẵn. Duyệt ba vòng lặp có độ phức tạp $O(n^3)$.
>
> Tính chẵn lẻ của số bit 1 trong XOR chính là tính chẵn lẻ của số bit 1 riêng lẻ, nên mỗi mảng có thể rút gọn thành hai nhóm.
>
> Đếm $bit\_count\bmod 2$ trong $a,b,c$, sau đó cộng $cnt1[i]cnt2[j]cnt3[k]$ với mọi $i+j+k$ chẵn.

<!-- thinking:end -->

Với hai số nguyên, tính chẵn lẻ của số lượng bit $1$ trong kết quả XOR phụ thuộc vào tính chẵn lẻ của số lượng bit $1$ trong biểu diễn nhị phân của hai số nguyên đó.

Ta có thể sử dụng ba mảng `cnt1`, `cnt2`, `cnt3` để ghi nhận tính chẵn lẻ của số lượng bit $1$ trong biểu diễn nhị phân của từng số thuộc các mảng `a`, `b`, `c` tương ứng.

Sau đó, ta duyệt tính chẵn lẻ của số lượng bit $1$ trong biểu diễn nhị phân của từng số thuộc ba mảng trong phạm vi $[0, 1]$. Nếu tổng tính chẵn lẻ của số lượng bit $1$ trong biểu diễn nhị phân của ba số là chẵn, thì số lượng bit $1$ trong kết quả XOR của ba số này cũng chẵn. Khi đó, ta nhân số lượng của ba nhóm tương ứng rồi cộng vào đáp án.

Cuối cùng, trả về đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của các mảng `a`, `b`, `c`. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def tripletCount(self, a: List[int], b: List[int], c: List[int]) -> int:
        cnt1 = Counter(x.bit_count() & 1 for x in a)
        cnt2 = Counter(x.bit_count() & 1 for x in b)
        cnt3 = Counter(x.bit_count() & 1 for x in c)
        ans = 0
        for i in range(2):
            for j in range(2):
                for k in range(2):
                    if (i + j + k) & 1 ^ 1:
                        ans += cnt1[i] * cnt2[j] * cnt3[k]
        return ans
```

#### Java

```java
class Solution {
    public int tripletCount(int[] a, int[] b, int[] c) {
        int[] cnt1 = new int[2];
        int[] cnt2 = new int[2];
        int[] cnt3 = new int[2];
        for (int x : a) {
            ++cnt1[Integer.bitCount(x) & 1];
        }
        for (int x : b) {
            ++cnt2[Integer.bitCount(x) & 1];
        }
        for (int x : c) {
            ++cnt3[Integer.bitCount(x) & 1];
        }
        int ans = 0;
        for (int i = 0; i < 2; ++i) {
            for (int j = 0; j < 2; ++j) {
                for (int k = 0; k < 2; ++k) {
                    if ((i + j + k) % 2 == 0) {
                        ans += cnt1[i] * cnt2[j] * cnt3[k];
                    }
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
    int tripletCount(vector<int>& a, vector<int>& b, vector<int>& c) {
        int cnt1[2]{};
        int cnt2[2]{};
        int cnt3[2]{};
        for (int x : a) {
            ++cnt1[__builtin_popcount(x) & 1];
        }
        for (int x : b) {
            ++cnt2[__builtin_popcount(x) & 1];
        }
        for (int x : c) {
            ++cnt3[__builtin_popcount(x) & 1];
        }
        int ans = 0;
        for (int i = 0; i < 2; ++i) {
            for (int j = 0; j < 2; ++j) {
                for (int k = 0; k < 2; ++k) {
                    if ((i + j + k) % 2 == 0) {
                        ans += cnt1[i] * cnt2[j] * cnt3[k];
                    }
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func tripletCount(a []int, b []int, c []int) (ans int) {
    cnt1 := [2]int{}
    cnt2 := [2]int{}
    cnt3 := [2]int{}
    for _, x := range a {
        cnt1[bits.OnesCount(uint(x))%2]++
    }
    for _, x := range b {
        cnt2[bits.OnesCount(uint(x))%2]++
    }
    for _, x := range c {
        cnt3[bits.OnesCount(uint(x))%2]++
    }
    for i := 0; i < 2; i++ {
        for j := 0; j < 2; j++ {
            for k := 0; k < 2; k++ {
                if (i+j+k)%2 == 0 {
                    ans += cnt1[i] * cnt2[j] * cnt3[k]
                }
            }
        }
    }
    return
}
```

#### TypeScript

```ts
function tripletCount(a: number[], b: number[], c: number[]): number {
    const cnt1: [number, number] = [0, 0];
    const cnt2: [number, number] = [0, 0];
    const cnt3: [number, number] = [0, 0];
    for (const x of a) {
        ++cnt1[bitCount(x) & 1];
    }
    for (const x of b) {
        ++cnt2[bitCount(x) & 1];
    }
    for (const x of c) {
        ++cnt3[bitCount(x) & 1];
    }
    let ans = 0;
    for (let i = 0; i < 2; ++i) {
        for (let j = 0; j < 2; ++j) {
            for (let k = 0; k < 2; ++k) {
                if ((i + j + k) % 2 === 0) {
                    ans += cnt1[i] * cnt2[j] * cnt3[k];
                }
            }
        }
    }
    return ans;
}

function bitCount(i: number): number {
    i = i - ((i >>> 1) & 0x55555555);
    i = (i & 0x33333333) + ((i >>> 2) & 0x33333333);
    i = (i + (i >>> 4)) & 0x0f0f0f0f;
    i = i + (i >>> 8);
    i = i + (i >>> 16);
    return i & 0x3f;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
