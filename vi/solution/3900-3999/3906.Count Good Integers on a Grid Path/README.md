---
comments: true
difficulty: Hard
rating: 2160
source: Weekly Contest 498 Q4
tags:
    - Dynamic Programming
---

<!-- problem:start -->

# [3906. Count Good Integers on a Grid Path](https://leetcode.com/problems/count-good-integers-on-a-grid-path)

[中文文档](/solution/3900-3999/3906.Count%20Good%20Integers%20on%20a%20Grid%20Path/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên <code>l</code> và <code>r</code>, cùng một chuỗi <code>directions</code> gồm <strong>đúng</strong> ba ký tự <code>&#39;D&#39;</code> và ba ký tự <code>&#39;R&#39;</code>.</p>

<p>Với mỗi số nguyên <code>x</code> trong đoạn <code>[l, r]</code> (tính cả hai đầu mút), thực hiện các bước sau:</p>

<ol>
	<li>Nếu <code>x</code> có ít hơn 16 chữ số, thêm các <strong>số 0 ở đầu</strong> bên trái để thu được một chuỗi gồm 16 chữ số.</li>
	<li>Đặt 16 chữ số vào một lưới <code>4 &times; 4</code> theo thứ tự <strong>theo hàng</strong> (4 chữ số đầu tiên tạo thành hàng đầu tiên từ trái sang phải, 4 chữ số tiếp theo tạo thành hàng thứ hai, v.v.).</li>
	<li>Bắt đầu tại ô <strong>trên cùng bên trái</strong> (<code>row = 0</code>, <code>column = 0</code>), lần lượt áp dụng 6 ký tự của <code>directions</code>:
	<ul>
		<li><code>&#39;D&#39;</code> tăng hàng lên 1.</li>
		<li><code>&#39;R&#39;</code> tăng cột lên 1.</li>
	</ul>
	</li>
	<li>Ghi lại dãy chữ số đi qua trên đường đi (bao gồm cả ô bắt đầu), tạo thành một dãy có độ dài 7.</li>
</ol>

<p>Số nguyên <code>x</code> được gọi là <strong>tốt</strong> nếu dãy thu được <strong>không giảm</strong>.</p>

<p>Trả về một số nguyên biểu thị số lượng số nguyên tốt trong đoạn <code>[l, r]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">l = 8, r = 10, directions = &quot;DDDRRR&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Lưới ứng với <code>x = 8</code>:</p>

<table style="border: 1px solid black;">
	<tbody>
		<tr style="background:none;">
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
		</tr>
		<tr style="background:none;">
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
		</tr>
		<tr style="background:none;">
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
		</tr>
		<tr style="background:none;">
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">8</td>
		</tr>
	</tbody>
</table>

<ul>
	<li>Đường đi: <code>(0,0) &rarr; (1,0) &rarr; (2,0) &rarr; (3,0) &rarr; (3,1) &rarr; (3,2) &rarr; (3,3)</code></li>
	<li>Dãy chữ số đi qua là <code>[0, 0, 0, 0, 0, 0, 8]</code>.</li>
	<li>Vì dãy chữ số đi qua không giảm, 8 là một số tốt.</li>
</ul>

<p>Lưới ứng với <code>x = 9</code>:</p>

<table style="border: 1px solid black;">
	<tbody>
		<tr style="background:none;">
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
		</tr>
		<tr style="background:none;">
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
		</tr>
		<tr style="background:none;">
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
		</tr>
		<tr style="background:none;">
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">9</td>
		</tr>
	</tbody>
</table>

<ul>
	<li>Dãy chữ số đi qua là <code>[0, 0, 0, 0, 0, 0, 9]</code>.</li>
	<li>Vì dãy chữ số đi qua không giảm, 9 là một số tốt.</li>
</ul>

<p>Lưới ứng với <code>x = 10</code>:</p>

<table style="border: 1px solid black;">
	<tbody>
		<tr style="background:none;">
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
		</tr>
		<tr style="background:none;">
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
		</tr>
		<tr style="background:none;">
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
		</tr>
		<tr style="background:none;">
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">0</td>
		</tr>
	</tbody>
</table>

<ul>
	<li>Dãy chữ số đi qua là <code>[0, 0, 0, 0, 0, 1, 0]</code>.</li>
	<li>Vì dãy chữ số đi qua không giảm, 10 không phải là số tốt.</li>
	<li>Do đó, chỉ 8 và 9 là số tốt, tổng cộng có 2 số nguyên tốt trong đoạn.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">l = 123456789, r = 123456790, directions = &quot;DDRRDR&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Lưới ứng với <code>x = 123456789</code>:</p>

<table style="border: 1px solid black;">
	<tbody>
		<tr style="background:none;">
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
		</tr>
		<tr style="background:none;">
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">1</td>
		</tr>
		<tr style="background:none;">
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">4</td>
			<td style="border: 1px solid black;">5</td>
		</tr>
		<tr style="background:none;">
			<td style="border: 1px solid black;">6</td>
			<td style="border: 1px solid black;">7</td>
			<td style="border: 1px solid black;">8</td>
			<td style="border: 1px solid black;">9</td>
		</tr>
	</tbody>
</table>

<ul>
	<li>Đường đi: <code>(0,0) &rarr; (1,0) &rarr; (2,0) &rarr; (2,1) &rarr; (2,2) &rarr; (3,2) &rarr; (3,3)</code></li>
	<li>Dãy chữ số đi qua là <code>[0, 0, 2, 3, 4, 8, 9]</code>.</li>
	<li>Vì dãy chữ số đi qua không giảm, 123456789 là một số tốt.</li>
</ul>

<p>Lưới ứng với <code>x = 123456790</code>:</p>

<table style="border: 1px solid black;">
	<tbody>
		<tr style="background:none;">
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
		</tr>
		<tr style="background:none;">
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">0</td>
			<td style="border: 1px solid black;">1</td>
		</tr>
		<tr style="background:none;">
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">4</td>
			<td style="border: 1px solid black;">5</td>
		</tr>
		<tr style="background:none;">
			<td style="border: 1px solid black;">6</td>
			<td style="border: 1px solid black;">7</td>
			<td style="border: 1px solid black;">9</td>
			<td style="border: 1px solid black;">0</td>
		</tr>
	</tbody>
</table>

<ul>
	<li>Dãy chữ số đi qua là <code>[0, 0, 2, 3, 4, 9, 0]</code>.</li>
	<li>Vì dãy chữ số đi qua không giảm, 123456790 không phải là số tốt.</li>
	<li>Do đó, chỉ 123456789 là số tốt, tổng cộng có 1 số nguyên tốt trong đoạn.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">l = 1288561398769758, r = 1288561398769758, directions = &quot;RRRDDD&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Lưới ứng với <code>x = 1288561398769758</code>:</p>

<table style="border: 1px solid black;">
	<tbody>
		<tr style="background:none;">
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">8</td>
			<td style="border: 1px solid black;">8</td>
		</tr>
		<tr style="background:none;">
			<td style="border: 1px solid black;">5</td>
			<td style="border: 1px solid black;">6</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">3</td>
		</tr>
		<tr style="background:none;">
			<td style="border: 1px solid black;">9</td>
			<td style="border: 1px solid black;">8</td>
			<td style="border: 1px solid black;">7</td>
			<td style="border: 1px solid black;">6</td>
		</tr>
		<tr style="background:none;">
			<td style="border: 1px solid black;">9</td>
			<td style="border: 1px solid black;">7</td>
			<td style="border: 1px solid black;">5</td>
			<td style="border: 1px solid black;">8</td>
		</tr>
	</tbody>
</table>

<ul>
	<li>Đường đi: <code>(0,0) &rarr; (0,1) &rarr; (0,2) &rarr; (0,3) &rarr; (1,3) &rarr; (2,3) &rarr; (3,3)</code></li>
	<li>Dãy chữ số đi qua là <code>[1, 2, 8, 8, 3, 6, 8]</code>.</li>
	<li>Vì dãy chữ số đi qua không giảm, 1288561398769758 không phải là số tốt.</li>
	<li>Không có số nào là số tốt, tổng cộng có 0 số nguyên tốt trong đoạn.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= l &lt;= r &lt;= 9 &times; 10<sup>15</sup></code></li>
	<li><code>directions.length == 6</code></li>
	<li><code>directions</code> gồm <strong>đúng</strong> ba ký tự <code>&#39;D&#39;</code> và ba ký tự <code>&#39;R&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Digit DP

<!-- thinking:start -->

> **Tư duy**
>
> Cận trên là $9\times 10^{15}$, nên không thể liệt kê các số. Sáu bước `D`/`R` tạo thành một đường đi trên lưới $4\times 4$; các chữ số tại những ô đã đi qua phải không giảm theo thứ tự đi.
>
> Đếm trên $[l,r]$ bằng cách lấy $[0,r]$ trừ $[0,l-1]$, sau đó chạy digit DP trên một chuỗi gồm 16 chữ số. Trạng thái $(\textit{pos},\textit{last},\textit{lim})$ lưu vị trí hiện tại, chữ số của ô key gần nhất và việc ta còn đang bám sát cận trên hay không.
>
> Các ô không phải key có thể bắt đầu từ $0$; một ô key phải có giá trị không nhỏ hơn $\textit{last}$. Những ô nào là key được tiền xử lý từ $\textit{directions}$ vào mảng Boolean $\textit{key}$.

<!-- thinking:end -->

Vì 6 ký tự trong $\textit{directions}$ xác định đường đi, ta có thể tiền xử lý một mảng Boolean $\textit{key}$ có độ dài 16, trong đó $\textit{key}[i]$ cho biết ô thứ $i$ được đi qua trên đường đi có phải là ô key hay không (tức là ô nằm trên đường đi). Ta có thể tính mảng $\textit{key}$ dựa trên $\textit{directions}$.

Tiếp theo, ta dùng digit DP để đếm số lượng số nguyên trong đoạn $[l, r]$ thỏa mãn điều kiện. Ta chuyển $r$ và $l - 1$ thành các chuỗi gồm $16$ chữ số $s$, sau đó dùng một hàm đệ quy để đếm số nguyên hợp lệ trong $[0, r]$, rồi trừ đi số lượng trong $[0, l - 1]$ để thu được đáp án trên $[l, r]$.

Ta định nghĩa hàm đệ quy $\textit{dfs}(pos, last, lim)$, trong đó $pos$ là vị trí chữ số hiện tại, $last$ là chữ số của ô key trước đó, còn $lim$ cho biết chữ số hiện tại có bị giới hạn bởi $s$ hay không (tức là tiền tố hiện tại có trùng với $s$ đến thời điểm này hay không).

Trong hàm đệ quy, trước hết ta kiểm tra xem đã xử lý hết các vị trí chưa; nếu rồi thì trả về 1. Nếu chưa, ta xác định đoạn chữ số cần thử ở vị trí hiện tại: nếu $\textit{key}[pos]$ là true, chữ số phải lớn hơn hoặc bằng $last$; nếu không, nó có thể bắt đầu từ 0. Cận trên là $s[pos]$ nếu $lim$ là true, hoặc 9 nếu không.

Ta liệt kê tất cả chữ số có thể chọn ở vị trí hiện tại, cập nhật $last$ thành chữ số hiện tại nếu đây là ô key, hoặc giữ nguyên nếu không phải. Đồng thời, ta cập nhật $lim$: nếu chữ số hiện tại bằng cận trên thì $lim$ vẫn là true; nếu không thì trở thành false. Ta cộng kết quả của tất cả các nhánh và trả về tổng.

Độ phức tạp thời gian là $O(D^2 \times \log r)$ và độ phức tạp không gian là $O(D \times \log r)$, trong đó $D = 10$ là miền giá trị của các chữ số và $\log r$ là số chữ số của $r$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countGoodIntegersOnPath(self, l: int, r: int, directions: str) -> int:
        key = [False] * 16
        row, col = 0, 0
        key[0] = True
        for c in directions:
            if c == "D":
                row += 1
            else:
                col += 1
            key[row * 4 + col] = True

        s = ""

        @cache
        def dfs(pos, last, lim):
            if pos == 16:
                return 1

            res = 0
            start = last if key[pos] else 0
            end = int(s[pos]) if lim else 9

            for i in range(start, end + 1):
                res += dfs(pos + 1, i if key[pos] else last, lim and (i == end))

            return res

        def calc(x):
            nonlocal s
            if x < 0:
                return 0
            s = str(x).zfill(16)
            dfs.cache_clear()
            return dfs(0, 0, True)

        return calc(r) - calc(l - 1)
```

#### Java

```java
class Solution {
    private boolean[] key;
    private long[][] f;
    private String s;

    public long countGoodIntegersOnPath(long l, long r, String directions) {
        key = new boolean[16];
        int row = 0, col = 0;
        key[0] = true;
        for (char c : directions.toCharArray()) {
            if (c == 'D') {
                row++;
            } else {
                col++;
            }
            key[row * 4 + col] = true;
        }

        return calc(r) - calc(l - 1);
    }

    private long dfs(int pos, int last, boolean lim) {
        if (pos == 16) {
            return 1;
        }
        if (!lim && f[pos][last] != -1) {
            return f[pos][last];
        }

        long res = 0;
        int start = key[pos] ? last : 0;
        int end = lim ? (s.charAt(pos) - '0') : 9;

        for (int i = start; i <= end; i++) {
            res += dfs(pos + 1, key[pos] ? i : last, lim && (i == end));
        }

        if (!lim) {
            f[pos][last] = res;
        }
        return res;
    }

    private long calc(long x) {
        if (x < 0) {
            return 0;
        }
        String t = String.valueOf(x);
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < 16 - t.length(); i++) {
            sb.append('0');
        }
        s = sb.append(t).toString();
        f = new long[16][10];
        for (long[] row : f) {
            Arrays.fill(row, -1);
        }
        return dfs(0, 0, true);
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long countGoodIntegersOnPath(long long l, long long r, string directions) {
        bool key[16];
        memset(key, 0, sizeof(key));
        int row = 0, col = 0;
        key[0] = true;
        for (char c : directions) {
            if (c == 'D') {
                row++;
            } else {
                col++;
            }
            key[row * 4 + col] = true;
        }

        long long f[16][10];
        string s;

        auto dfs = [&](this auto&& dfs, int pos, int last, bool lim) -> long long {
            if (pos == 16) {
                return 1;
            }
            if (!lim && f[pos][last] != -1) {
                return f[pos][last];
            }

            long long res = 0;
            int start = key[pos] ? last : 0;
            int end = lim ? (s[pos] - '0') : 9;

            for (int i = start; i <= end; i++) {
                res += dfs(pos + 1, key[pos] ? i : last, lim && (i == end));
            }

            if (!lim) {
                f[pos][last] = res;
            }
            return res;
        };

        auto calc = [&](long long x) {
            if (x < 0) {
                return 0LL;
            }
            string t = to_string(x);
            s = string(16 - t.length(), '0') + t;
            memset(f, -1, sizeof(f));
            return dfs(0, 0, true);
        };

        return calc(r) - calc(l - 1);
    }
};
```

#### Go

```go
func countGoodIntegersOnPath(l int64, r int64, directions string) int64 {
	key := make([]bool, 16)
	row, col := 0, 0
	key[0] = true
	for _, c := range directions {
		if c == 'D' {
			row++
		} else {
			col++
		}
		key[row*4+col] = true
	}

	var s string
	var f [16][10]int64

	var dfs func(int, int, bool) int64
	dfs = func(pos int, last int, lim bool) int64 {
		if pos == 16 {
			return 1
		}
		if !lim && f[pos][last] != -1 {
			return f[pos][last]
		}

		var res int64 = 0
		start := 0
		if key[pos] {
			start = last
		}
		end := 9
		if lim {
			end = int(s[pos] - '0')
		}

		for i := start; i <= end; i++ {
			nextLast := last
			if key[pos] {
				nextLast = i
			}
			res += dfs(pos+1, nextLast, lim && (i == end))
		}

		if !lim {
			f[pos][last] = res
		}
		return res
	}

	calc := func(x int64) int64 {
		if x < 0 {
			return 0
		}
		t := strconv.FormatInt(x, 10)
		s = fmt.Sprintf("%016s", t)
		for i := 0; i < 16; i++ {
			for j := 0; j < 10; j++ {
				f[i][j] = -1
			}
		}
		return dfs(0, 0, true)
	}

	return calc(r) - calc(l-1)
}
```

#### TypeScript

```ts
function countGoodIntegersOnPath(l: number, r: number, directions: string): number {
    const key = new Array(16).fill(false);
    let row = 0,
        col = 0;
    key[0] = true;
    for (const c of directions) {
        if (c === 'D') {
            row++;
        } else {
            col++;
        }
        key[row * 4 + col] = true;
    }

    let s: string;
    let f: number[][];

    const dfs = (pos: number, last: number, lim: boolean): number => {
        if (pos === 16) {
            return 1;
        }
        if (!lim && f[pos][last] !== -1) {
            return f[pos][last];
        }

        let res = 0;
        const start = key[pos] ? last : 0;
        const end = lim ? parseInt(s[pos]) : 9;

        for (let i = start; i <= end; i++) {
            res += dfs(pos + 1, key[pos] ? i : last, lim && i === end);
        }

        if (!lim) {
            f[pos][last] = res;
        }
        return res;
    };

    const calc = (x: number): number => {
        if (x < 0) {
            return 0;
        }
        s = x.toString().padStart(16, '0');
        f = Array.from({ length: 16 }, () => {
            return new Array(10).fill(-1);
        });
        return dfs(0, 0, true);
    };

    return calc(r) - calc(l - 1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
