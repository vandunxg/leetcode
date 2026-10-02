---
comments: true
difficulty: Medium
tags:
    - Recursion
    - String
    - Simulation
---

<!-- problem:start -->

# [544. Output Contest Matches 🔒](https://leetcode.com/problems/output-contest-matches)

[中文文档](/solution/0500-0599/0544.Output%20Contest%20Matches/README.md)

## Mô tả

<!-- description:start -->

<p>Trong vòng playoff NBA, ta thường xếp đội mạnh hơn đấu với đội yếu hơn, chẳng hạn đội hạng <code>1</code> gặp đội hạng <code>n<sup>th</sup></code>. Đây là cách hay để giải đấu hấp dẫn hơn.</p>

<p>Cho <code>n</code> đội, hãy trả về <em>lịch đấu cuối cùng dưới dạng chuỗi</em>.</p>

<p><code>n</code> đội được đánh số từ <code>1</code> đến <code>n</code>, tương ứng với thứ hạng ban đầu (đội hạng <code>1</code> mạnh nhất, đội hạng <code>n</code> yếu nhất).</p>

<p>Ta dùng dấu ngoặc đơn <code>&#39;(&#39;</code>, <code>&#39;)&#39;</code> và dấu phẩy <code>&#39;,&#39;</code> để biểu diễn các cặp đấu. Dấu ngoặc dùng để nhóm một cặp, còn dấu phẩy dùng để phân tách hai đội. Ở mỗi vòng, luôn ghép đội mạnh hơn với đội yếu hơn.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 4
<strong>Đầu ra:</strong> &quot;((1,4),(2,3))&quot;
<strong>Giải thích:</strong>
Ở vòng đầu tiên, ta ghép đội 1 với đội 4, đội 2 với đội 3 để mỗi cặp gồm một đội mạnh và một đội yếu.
Ta thu được (1, 4),(2, 3).
Ở vòng thứ hai, đội thắng trong các cặp (1, 4) và (2, 3) tiếp tục đấu để tìm đội vô địch. Vì vậy, ta thêm một cặp ngoặc bao quanh chúng.
Kết quả cuối cùng là ((1,4),(2,3)).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 8
<strong>Đầu ra:</strong> &quot;(((1,8),(4,5)),((2,7),(3,6)))&quot;
<strong>Giải thích:</strong>
Vòng đầu tiên: (1, 8),(2, 7),(3, 6),(4, 5)
Vòng thứ hai: ((1, 8),(4, 5)),((2, 7),(3, 6))
Vòng thứ ba: (((1, 8),(4, 5)),((2, 7),(3, 6)))
Vì vòng thứ ba tìm ra đội vô địch, ta cần trả về kết quả (((1,8),(4,5)),((2,7),(3,6))).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == 2<sup>x</sup></code>, trong đó <code>x</code> nằm trong khoảng <code>[1, 12]</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Các cặp đấu lần lượt là $1$ với $n$, $2$ với $n-1$, v.v.; các vòng được lồng nhau theo cách đệ quy. Vì $n$ là lũy thừa của hai nên có thể mô phỏng trực tiếp.
>
> Lưu các đội hiện tại (hoặc các chuỗi đã tạo). Ở mỗi vòng, ghép đội ở vị trí $i$ với đội ở vị trí $n-1-i$ thành `(a,b)` rồi ghi vào nửa đầu mảng. Giảm một nửa độ dài cho đến khi còn lại một chuỗi.

<!-- thinking:end -->

Ta dùng mảng $s$ có độ dài $n$ để lưu mã của từng đội, rồi mô phỏng quá trình thi đấu.

Ở mỗi vòng, ta ghép từng cặp trong $n$ phần tử đầu của mảng $s$, rồi lưu mã của các đội thắng vào $n/2$ vị trí đầu tiên của mảng. Sau đó, giảm một nửa $n$ và tiếp tục vòng tiếp theo cho đến khi $n = 1$. Khi đó, phần tử đầu tiên của mảng $s$ là sơ đồ đấu cuối cùng.

Độ phức tạp thời gian là $O(n \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số đội.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findContestMatch(self, n: int) -> str:
        s = [str(i + 1) for i in range(n)]
        while n > 1:
            for i in range(n >> 1):
                s[i] = f"({s[i]},{s[n - i - 1]})"
            n >>= 1
        return s[0]
```

#### Java

```java
class Solution {
    public String findContestMatch(int n) {
        String[] s = new String[n];
        for (int i = 0; i < n; ++i) {
            s[i] = String.valueOf(i + 1);
        }
        for (; n > 1; n >>= 1) {
            for (int i = 0; i < n >> 1; ++i) {
                s[i] = String.format("(%s,%s)", s[i], s[n - i - 1]);
            }
        }
        return s[0];
    }
}
```

#### C++

```cpp
class Solution {
public:
    string findContestMatch(int n) {
        vector<string> s(n);
        for (int i = 0; i < n; ++i) {
            s[i] = to_string(i + 1);
        }
        for (; n > 1; n >>= 1) {
            for (int i = 0; i < n >> 1; ++i) {
                s[i] = "(" + s[i] + "," + s[n - i - 1] + ")";
            }
        }
        return s[0];
    }
};
```

#### Go

```go
func findContestMatch(n int) string {
	s := make([]string, n)
	for i := 0; i < n; i++ {
		s[i] = strconv.Itoa(i + 1)
	}
	for ; n > 1; n >>= 1 {
		for i := 0; i < n>>1; i++ {
			s[i] = fmt.Sprintf("(%s,%s)", s[i], s[n-i-1])
		}
	}
	return s[0]
}
```

#### TypeScript

```ts
function findContestMatch(n: number): string {
    const s: string[] = Array.from({ length: n }, (_, i) => (i + 1).toString());
    for (; n > 1; n >>= 1) {
        for (let i = 0; i < n >> 1; ++i) {
            s[i] = `(${s[i]},${s[n - i - 1]})`;
        }
    }
    return s[0];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
