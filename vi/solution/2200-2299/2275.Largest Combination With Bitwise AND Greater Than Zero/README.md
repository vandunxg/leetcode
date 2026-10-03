---
comments: true
difficulty: Medium
rating: 1642
source: Weekly Contest 293 Q3
tags:
    - Bit Manipulation
    - Array
    - Hash Table
    - Counting
---

<!-- problem:start -->

# [2275. Largest Combination With Bitwise AND Greater Than Zero](https://leetcode.com/problems/largest-combination-with-bitwise-and-greater-than-zero)

[中文文档](/solution/2200-2299/2275.Largest%20Combination%20With%20Bitwise%20AND%20Greater%20Than%20Zero/README.md)

## Mô tả

<!-- description:start -->

<p>Phép <strong>AND theo bit</strong> của một mảng <code>nums</code> là phép AND theo bit của tất cả số nguyên trong <code>nums</code>.</p>

<ul>
	<li>Ví dụ, với <code>nums = [1, 5, 3]</code>, phép AND theo bit là <code>1 &amp; 5 &amp; 3 = 1</code>.</li>
	<li>Tương tự, với <code>nums = [7]</code>, phép AND theo bit là <code>7</code>.</li>
</ul>

<p>Cho một mảng các số nguyên dương <code>candidates</code>. Hãy tính phép <strong>AND theo bit</strong> cho mọi <strong>tổ hợp</strong> có thể của các phần tử trong mảng <code>candidates</code>.</p>

<p>Trả về <em>kích thước của <strong>tổ hợp lớn nhất</strong> trong </em><code>candidates</code><em> có phép AND theo bit <strong>lớn hơn</strong> </em><code>0</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> candidates = [16,17,71,62,12,24,14]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Tổ hợp [16,17,62,24] có phép AND theo bit bằng 16 &amp; 17 &amp; 62 &amp; 24 = 16 &gt; 0.
Kích thước của tổ hợp là 4.
Có thể chứng minh rằng không có tổ hợp nào có kích thước lớn hơn 4 mà phép AND theo bit lớn hơn 0.
Lưu ý rằng có thể có nhiều tổ hợp đạt kích thước lớn nhất.
Ví dụ, tổ hợp [62,12,24,14] có phép AND theo bit bằng 62 &amp; 12 &amp; 24 &amp; 14 = 8 &gt; 0.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> candidates = [8,8]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Tổ hợp lớn nhất [8,8] có phép AND theo bit bằng 8 &amp; 8 = 8 &gt; 0.
Kích thước của tổ hợp là 2, nên ta trả về 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= candidates.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= candidates[i] &lt;= 10<sup>7</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Một tập con có phép AND dương khi và chỉ khi có một bit bằng $1$ trong mọi số được chọn. Vì vậy, tập con lớn nhất là số lượng lớn nhất các số có cùng một bit. Với $n \le 10^5$, ta không thể liệt kê tất cả các tập con.
>
> Với mỗi bit $i$, ta đếm có bao nhiêu giá trị $x$ có bit đó được bật, rồi lấy giá trị lớn nhất. Số bit cần xét phụ thuộc vào giá trị lớn nhất.

<!-- thinking:end -->

Bài toán yêu cầu tìm độ dài lớn nhất của một tổ hợp các số sao cho kết quả AND theo bit lớn hơn $0$. Điều này có nghĩa là phải có một bit nhị phân nào đó bằng $1$ trong tất cả các số. Do đó, ta có thể duyệt qua từng bit nhị phân và đếm số lượng bit $1$ ở vị trí đó trong tất cả các số. Cuối cùng, ta lấy số lượng lớn nhất.

Độ phức tạp thời gian là $O(n \times \log M)$, trong đó $n$ và $M$ lần lượt là độ dài của mảng $\textit{candidates}$ và giá trị lớn nhất trong mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def largestCombination(self, candidates: List[int]) -> int:
        ans = 0
        for i in range(max(candidates).bit_length()):
            ans = max(ans, sum(x >> i & 1 for x in candidates))
        return ans
```

#### Java

```java
class Solution {
    public int largestCombination(int[] candidates) {
        int mx = Arrays.stream(candidates).max().getAsInt();
        int m = Integer.SIZE - Integer.numberOfLeadingZeros(mx);
        int ans = 0;
        for (int i = 0; i < m; ++i) {
            int cnt = 0;
            for (int x : candidates) {
                cnt += x >> i & 1;
            }
            ans = Math.max(ans, cnt);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int largestCombination(vector<int>& candidates) {
        int mx = *max_element(candidates.begin(), candidates.end());
        int m = 32 - __builtin_clz(mx);
        int ans = 0;
        for (int i = 0; i < m; ++i) {
            int cnt = 0;
            for (int x : candidates) {
                cnt += x >> i & 1;
            }
            ans = max(ans, cnt);
        }
        return ans;
    }
};
```

#### Go

```go
func largestCombination(candidates []int) (ans int) {
	mx := slices.Max(candidates)
	m := bits.Len(uint(mx))
	for i := 0; i < m; i++ {
		cnt := 0
		for _, x := range candidates {
			cnt += (x >> i) & 1
		}
		ans = max(ans, cnt)
	}
	return
}
```

#### TypeScript

```ts
function largestCombination(candidates: number[]): number {
    const mx = Math.max(...candidates);
    const m = mx.toString(2).length;
    let ans = 0;
    for (let i = 0; i < m; ++i) {
        let cnt = 0;
        for (const x of candidates) {
            cnt += (x >> i) & 1;
        }
        ans = Math.max(ans, cnt);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
