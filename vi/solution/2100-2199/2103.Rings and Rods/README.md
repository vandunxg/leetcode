---
comments: true
difficulty: Easy
rating: 1257
source: Weekly Contest 271 Q1
tags:
    - Hash Table
    - String
---

<!-- problem:start -->

# [2103. Rings and Rods](https://leetcode.com/problems/rings-and-rods)

[中文文档](/solution/2100-2199/2103.Rings%20and%20Rods/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> chiếc vòng, mỗi chiếc có màu đỏ, xanh lá hoặc xanh dương. Các vòng được phân bố trên <strong>mười thanh</strong> được đánh số từ <code>0</code> đến <code>9</code>.</p>

<p>Cho chuỗi <code>rings</code> có độ dài <code>2n</code>, mô tả vị trí của <code>n</code> chiếc vòng trên các thanh. Cứ mỗi hai ký tự trong <code>rings</code> tạo thành một <strong>cặp màu-vị trí</strong> dùng để mô tả một chiếc vòng, trong đó:</p>

<ul>
	<li>Ký tự <strong>đầu tiên</strong> trong cặp thứ <code>i<sup>th</sup></code> biểu thị <strong>màu</strong> của chiếc vòng thứ <code>i<sup>th</sup></code> (<code>&#39;R&#39;</code>, <code>&#39;G&#39;</code>, <code>&#39;B&#39;</code>).</li>
	<li>Ký tự <strong>thứ hai</strong> trong cặp thứ <code>i<sup>th</sup></code> biểu thị <strong>thanh</strong> mà chiếc vòng thứ <code>i<sup>th</sup></code> được đặt lên (<code>&#39;0&#39;</code> đến <code>&#39;9&#39;</code>).</li>
</ul>

<p>Ví dụ, <code>&quot;R3G2B1&quot;</code> mô tả <code>n == 3</code> chiếc vòng: một vòng đỏ đặt trên thanh mang số 3, một vòng xanh lá đặt trên thanh mang số 2 và một vòng xanh dương đặt trên thanh mang số 1.</p>

<p>Hãy trả về <em>số thanh có đủ vòng thuộc <strong>cả ba màu</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2103.Rings%20and%20Rods/images/ex1final.png" style="width: 258px; height: 130px;" />
<pre>
<strong>Đầu vào:</strong> rings = &quot;B0B6G0R6R0R6G9&quot;
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>
- Thanh mang số 0 có 3 chiếc vòng với đủ cả ba màu: đỏ, xanh lá và xanh dương.
- Thanh mang số 6 có 3 chiếc vòng, nhưng chỉ có màu đỏ và xanh dương.
- Thanh mang số 9 chỉ có một vòng xanh lá.
Vì vậy, số thanh có đủ cả ba màu là 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2103.Rings%20and%20Rods/images/ex2final.png" style="width: 266px; height: 130px;" />
<pre>
<strong>Đầu vào:</strong> rings = &quot;B0R0G0R9R0B0G0&quot;
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>
- Thanh mang số 0 có 6 chiếc vòng với đủ cả ba màu: đỏ, xanh lá và xanh dương.
- Thanh mang số 9 chỉ có một vòng đỏ.
Vì vậy, số thanh có đủ cả ba màu là 1.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> rings = &quot;G4&quot;
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>
Chỉ có một chiếc vòng được cho. Do đó, không có thanh nào có đủ cả ba màu.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>rings.length == 2 * n</code></li>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>rings[i]</code> với <code>i</code> là <strong>chẵn</strong> là một trong các ký tự <code>&#39;R&#39;</code>, <code>&#39;G&#39;</code> hoặc <code>&#39;B&#39;</code> (<strong>đánh chỉ số từ 0</strong>).</li>
	<li><code>rings[i]</code> với <code>i</code> là <strong>lẻ</strong> là một chữ số từ <code>&#39;0&#39;</code> đến <code>&#39;9&#39;</code> (<strong>đánh chỉ số từ 0</strong>).</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi thanh, chỉ cần biết trên đó đã xuất hiện đủ màu đỏ, xanh lá và xanh dương hay chưa. Nếu duyệt riêng $rings$ cho từng thanh thì các cặp vẫn bị xử lý lặp lại; vì chuỗi gồm các cặp màu-chỉ số có độ dài chẵn nên ta có thể xử lý trong một lượt.
>
> Ba màu tương ứng với ba cờ độc lập và có thể lưu trong ba bit. Ánh xạ `'R','G','B'` lần lượt thành $1,2,4$ rồi OR vào thanh $j$ sẽ cho biết một thanh đã đủ màu chính xác khi mask của nó bằng $7$.
>
> Vì vậy, ta duy trì một mảng độ dài $10$ là $\textit{mask}$, đọc từng cặp ký tự trong $rings$ và đếm số phần tử bằng $7$.

<!-- thinking:end -->

Ta có thể dùng một mảng $mask$ có độ dài $10$ để biểu diễn trạng thái màu của các vòng trên mỗi thanh, trong đó $mask[i]$ biểu diễn trạng thái màu của các vòng trên thanh thứ $i$. Nếu thanh thứ $i$ có vòng đỏ, xanh lá và xanh dương, biểu diễn nhị phân của $mask[i]$ là $111$, tức là $mask[i] = 7$.

Ta duyệt chuỗi $rings$. Với mỗi cặp màu-vị trí $(c, j)$, trong đó $c$ biểu thị màu của vòng và $j$ biểu thị số của thanh chứa vòng đó, ta bật bit tương ứng trong $mask[j]$, tức là $mask[j] |= d[c]$, trong đó $d[c]$ biểu thị bit nhị phân tương ứng với màu $c$.

Cuối cùng, ta đếm số phần tử trong $mask$ bằng $7$; đây chính là số thanh có đủ vòng thuộc cả ba màu.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(|\Sigma|)$, trong đó $n$ biểu thị độ dài của chuỗi $rings$ và $|\Sigma|$ biểu thị kích thước của tập ký tự.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countPoints(self, rings: str) -> int:
        mask = [0] * 10
        d = {"R": 1, "G": 2, "B": 4}
        for i in range(0, len(rings), 2):
            c = rings[i]
            j = int(rings[i + 1])
            mask[j] |= d[c]
        return mask.count(7)
```

#### Java

```java
class Solution {
    public int countPoints(String rings) {
        int[] d = new int['Z'];
        d['R'] = 1;
        d['G'] = 2;
        d['B'] = 4;
        int[] mask = new int[10];
        for (int i = 0, n = rings.length(); i < n; i += 2) {
            int c = rings.charAt(i);
            int j = rings.charAt(i + 1) - '0';
            mask[j] |= d[c];
        }
        int ans = 0;
        for (int x : mask) {
            if (x == 7) {
                ++ans;
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
    int countPoints(string rings) {
        int d['Z']{['R'] = 1, ['G'] = 2, ['B'] = 4};
        int mask[10]{};
        for (int i = 0, n = rings.size(); i < n; i += 2) {
            int c = rings[i];
            int j = rings[i + 1] - '0';
            mask[j] |= d[c];
        }
        return count(mask, mask + 10, 7);
    }
};
```

#### Go

```go
func countPoints(rings string) (ans int) {
	d := ['Z']int{'R': 1, 'G': 2, 'B': 4}
	mask := [10]int{}
	for i, n := 0, len(rings); i < n; i += 2 {
		c := rings[i]
		j := int(rings[i+1] - '0')
		mask[j] |= d[c]
	}
	for _, x := range mask {
		if x == 7 {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function countPoints(rings: string): number {
    const idx = (c: string) => c.charCodeAt(0) - 'A'.charCodeAt(0);
    const d: number[] = Array(26).fill(0);
    d[idx('R')] = 1;
    d[idx('G')] = 2;
    d[idx('B')] = 4;
    const mask: number[] = Array(10).fill(0);
    for (let i = 0; i < rings.length; i += 2) {
        const c = rings[i];
        const j = rings[i + 1].charCodeAt(0) - '0'.charCodeAt(0);
        mask[j] |= d[idx(c)];
    }
    return mask.filter(x => x === 7).length;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_points(rings: String) -> i32 {
        let mut d: [i32; 90] = [0; 90];
        d['R' as usize] = 1;
        d['G' as usize] = 2;
        d['B' as usize] = 4;

        let mut mask: [i32; 10] = [0; 10];

        let cs: Vec<char> = rings.chars().collect();

        for i in (0..cs.len()).step_by(2) {
            let c = cs[i] as usize;
            let j = (cs[i + 1] as usize) - ('0' as usize);
            mask[j] |= d[c];
        }

        mask.iter().filter(|&&x| x == 7).count() as i32
    }
}
```

#### C

```c
int countPoints(char* rings) {
    int d['Z'];
    memset(d, 0, sizeof(d));
    d['R'] = 1;
    d['G'] = 2;
    d['B'] = 4;

    int mask[10];
    memset(mask, 0, sizeof(mask));

    for (int i = 0, n = strlen(rings); i < n; i += 2) {
        int c = rings[i];
        int j = rings[i + 1] - '0';
        mask[j] |= d[c];
    }

    int ans = 0;
    for (int i = 0; i < 10; i++) {
        if (mask[i] == 7) {
            ans++;
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Vét cạn

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 đã duyệt chuỗi một lần. Một cách trực tiếp hơn là liệt kê các thanh từ $0$ đến $9$ rồi tìm trong chuỗi xem trên thanh đó có `'B'`, `'R'` và `'G'` hay không.
>
> Mỗi lần tìm vẫn có độ phức tạp tuyến tính và số thanh chỉ là $10$, nên độ phức tạp không đổi; phần trình bày này ghi lại cách kiểm tra bằng vét cạn đó.

<!-- thinking:end -->

Liệt kê các thanh từ $0$ đến $9$, rồi dùng thao tác tìm chuỗi để kiểm tra xem thanh đó đã xuất hiện cùng màu xanh dương, đỏ và xanh lá hay chưa.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$, trong đó $n$ là độ dài của chuỗi $rings$.

<!-- tabs:start -->

#### TypeScript

```ts
function countPoints(rings: string): number {
    let c = 0;
    for (let i = 0; i <= 9; i++) {
        if (rings.includes('B' + i) && rings.includes('R' + i) && rings.includes('G' + i)) c++;
    }
    return c;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
