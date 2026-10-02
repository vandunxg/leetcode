---
comments: true
difficulty: Easy
rating: 1284
source: Weekly Contest 223 Q1
tags:
    - Bit Manipulation
    - Array
---

<!-- problem:start -->

# [1720. Decode XORed Array](https://leetcode.com/problems/decode-xored-array)

[中文文档](/solution/1700-1799/1720.Decode%20XORed%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Có một mảng số nguyên <code>arr</code> <strong>ẩn</strong> gồm <code>n</code> số nguyên không âm.</p>

<p>Mảng này được mã hóa thành một mảng số nguyên khác <code>encoded</code> có độ dài <code>n - 1</code>, sao cho <code>encoded[i] = arr[i] XOR arr[i + 1]</code>. Ví dụ, nếu <code>arr = [1,0,2,1]</code> thì <code>encoded = [1,2,3]</code>.</p>

<p>Bạn được cho mảng <code>encoded</code> và một số nguyên <code>first</code>, là phần tử đầu tiên của <code>arr</code>, tức <code>arr[0]</code>.</p>

<p>Hãy trả về <em>mảng ban đầu</em> <code>arr</code>. Có thể chứng minh rằng đáp án tồn tại và duy nhất.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> encoded = [1,2,3], first = 1
<strong>Đầu ra:</strong> [1,0,2,1]
<strong>Giải thích:</strong> Nếu arr = [1,0,2,1] thì first = 1 và encoded = [1 XOR 0, 0 XOR 2, 2 XOR 1] = [1,2,3]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> encoded = [6,2,7,3], first = 4
<strong>Đầu ra:</strong> [4,2,0,7,4]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>encoded.length == n - 1</code></li>
	<li><code>0 &lt;= encoded[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= first &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> $\textit{encoded}[i]=\textit{arr}[i]\oplus\textit{arr}[i+1]$ và giá trị đầu tiên đã được cho. XOR hai vế với $\textit{arr}[i]$ sẽ khôi phục phần tử tiếp theo.
>
> Bắt đầu từ $\textit{first}$ và áp dụng $\textit{arr}[i+1]=\textit{arr}[i]\oplus\textit{encoded}[i]$ lần lượt trên mảng.

<!-- thinking:end -->

Dựa trên mô tả bài toán, ta có:

$$
\textit{encoded}[i] = \textit{arr}[i] \oplus \textit{arr}[i + 1]
$$

Nếu XOR hai vế của phương trình với $\textit{arr}[i]$, ta được:

$$
\textit{arr}[i] \oplus \textit{arr}[i] \oplus \textit{arr}[i + 1] = \textit{arr}[i] \oplus \textit{encoded}[i]
$$

Rút gọn ta được:

$$
\textit{arr}[i + 1] = \textit{arr}[i] \oplus \textit{encoded}[i]
$$

Theo phép biến đổi trên, ta có thể bắt đầu với $\textit{first}$ và lần lượt tính mọi phần tử của mảng $\textit{arr}$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def decode(self, encoded: List[int], first: int) -> List[int]:
        ans = [first]
        for x in encoded:
            ans.append(ans[-1] ^ x)
        return ans
```

#### Java

```java
class Solution {
    public int[] decode(int[] encoded, int first) {
        int n = encoded.length;
        int[] ans = new int[n + 1];
        ans[0] = first;
        for (int i = 0; i < n; ++i) {
            ans[i + 1] = ans[i] ^ encoded[i];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> decode(vector<int>& encoded, int first) {
        vector<int> ans = {{first}};
        for (int x : encoded) {
            ans.push_back(ans.back() ^ x);
        }
        return ans;
    }
};
```

#### Go

```go
func decode(encoded []int, first int) []int {
	ans := []int{first}
	for i, x := range encoded {
		ans = append(ans, ans[i]^x)
	}
	return ans
}
```

#### TypeScript

```ts
function decode(encoded: number[], first: number): number[] {
    const ans: number[] = [first];
    for (const x of encoded) {
        ans.push(ans.at(-1)! ^ x);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
