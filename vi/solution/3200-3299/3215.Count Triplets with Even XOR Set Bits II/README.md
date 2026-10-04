---
comments: true
difficulty: Medium
tags:
    - Bit Manipulation
    - Array
---

<!-- problem:start -->

# [3215. Count Triplets with Even XOR Set Bits II 🔒](https://leetcode.com/problems/count-triplets-with-even-xor-set-bits-ii)

[中文文档](/solution/3200-3299/3215.Count%20Triplets%20with%20Even%20XOR%20Set%20Bits%20II/README.md)

## Mô tả

<!-- description:start -->

Cho ba mảng số nguyên <code>a</code>, <code>b</code> và <code>c</code>, hãy trả về số bộ ba <code>(a[i], b[j], c[k])</code> sao cho phép <code>XOR</code> theo bit giữa các phần tử trong mỗi bộ ba có <strong>số</strong> <span data-keyword="set-bit">bit 1</span> là số chẵn.
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">a = [1], b = [2], c = [3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Bộ ba duy nhất là <code>(a[0], b[0], c[0])</code> và <code>XOR</code> của chúng là: <code>1 XOR 2 XOR 3 = 00<sub>2</sub></code>.</p>
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
	<li><code>1 &lt;= a.length, b.length, c.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= a[i], b[i], c[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bit Manipulation

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm các bộ ba sao cho XOR của chúng có số bit 1 là số chẵn. Với $n\le 10^5$, không thể duyệt toàn bộ các bộ ba. Tính chẵn lẻ của số bit 1 trong XOR chính là tính chẵn lẻ của ba số bit 1 tương ứng.
>
> Vì vậy, mỗi mảng chỉ cần quy về hai số đếm, tương ứng với popcount modulo $2$ bằng 0 và 1. Ta duyệt ba tính chẵn lẻ này; nếu tổng của chúng là số chẵn thì cộng tích của ba số đếm. Sau một lượt duyệt tuyến tính, phần kết hợp chỉ mất $O(1)$.

<!-- thinking:end -->

Với hai số nguyên, tính chẵn lẻ của số lượng $1$s trong kết quả XOR phụ thuộc vào tính chẵn lẻ của số lượng $1$s trong biểu diễn nhị phân của hai số đó.

Ta có thể dùng ba mảng `cnt1`, `cnt2`, `cnt3` để ghi lại tính chẵn lẻ của số lượng $1$s trong biểu diễn nhị phân của từng số thuộc các mảng `a`, `b`, `c` tương ứng.

Sau đó, ta duyệt tính chẵn lẻ của số lượng $1$s trong biểu diễn nhị phân của từng số thuộc ba mảng trong đoạn $[0, 1]$. Nếu tổng tính chẵn lẻ của số lượng $1$s trong biểu diễn nhị phân của ba số là số chẵn, thì số lượng $1$s trong kết quả XOR của ba số đó cũng là số chẵn. Khi đó, ta nhân số lượng cách chọn ba số này rồi cộng vào đáp án.

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
    public long tripletCount(int[] a, int[] b, int[] c) {
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
        long ans = 0;
        for (int i = 0; i < 2; ++i) {
            for (int j = 0; j < 2; ++j) {
                for (int k = 0; k < 2; ++k) {
                    if ((i + j + k) % 2 == 0) {
                        ans += 1L * cnt1[i] * cnt2[j] * cnt3[k];
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
    long long tripletCount(vector<int>& a, vector<int>& b, vector<int>& c) {
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
        long long ans = 0;
        for (int i = 0; i < 2; ++i) {
            for (int j = 0; j < 2; ++j) {
                for (int k = 0; k < 2; ++k) {
                    if ((i + j + k) % 2 == 0) {
                        ans += 1LL * cnt1[i] * cnt2[j] * cnt3[k];
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
func tripletCount(a []int, b []int, c []int) (ans int64) {
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
					ans += int64(cnt1[i] * cnt2[j] * cnt3[k])
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
