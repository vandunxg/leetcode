---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Math
---

<!-- problem:start -->

# [670. Maximum Swap](https://leetcode.com/problems/maximum-swap)

[中文文档](/solution/0600-0699/0670.Maximum%20Swap/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên <code>num</code>. Bạn có thể đổi chỗ hai chữ số tối đa một lần để tạo ra số lớn nhất có thể.</p>

<p>Trả về <em>số lớn nhất có thể tạo được</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 2736
<strong>Đầu ra:</strong> 7236
<strong>Giải thích:</strong> Đổi chỗ chữ số 2 và chữ số 7.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 9973
<strong>Đầu ra:</strong> 9973
<strong>Giải thích:</strong> Không cần đổi chỗ.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= num &lt;= 10<sup>8</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thuật toán tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Đổi chỗ hai chữ số tối đa một lần để tạo số lớn nhất. Không cần thử mọi cặp.
>
> Duyệt từ phải sang trái, ghi lại chỉ số của chữ số lớn nhất ở bên phải (bao gồm cả vị trí hiện tại). Sau đó duyệt từ trái sang phải và đổi chỗ tại vị trí đầu tiên thỏa $s[i]<s[d[i]]$. Cách này làm tăng chữ số ở hàng cao nhất có thể.

<!-- thinking:end -->

Đầu tiên, chuyển số thành chuỗi $s$. Sau đó, duyệt chuỗi $s$ từ phải sang trái, dùng mảng hoặc hash table $d$ để ghi lại vị trí của chữ số lớn nhất ở bên phải mỗi chữ số (có thể là chính vị trí của chữ số đó).

Tiếp theo, duyệt $d$ từ trái sang phải. Nếu $s[i] < s[d[i]]$, đổi chỗ hai chữ số đó rồi kết thúc quá trình duyệt.

Cuối cùng, chuyển chuỗi $s$ trở lại thành số; đó là đáp án.

Độ phức tạp thời gian là $O(\log M)$ và độ phức tạp không gian là $O(\log M)$, trong đó $M$ là độ lớn của số $num$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumSwap(self, num: int) -> int:
        s = list(str(num))
        n = len(s)
        d = list(range(n))
        for i in range(n - 2, -1, -1):
            if s[i] <= s[d[i + 1]]:
                d[i] = d[i + 1]
        for i, j in enumerate(d):
            if s[i] < s[j]:
                s[i], s[j] = s[j], s[i]
                break
        return int(''.join(s))
```

#### Java

```java
class Solution {
    public int maximumSwap(int num) {
        char[] s = String.valueOf(num).toCharArray();
        int n = s.length;
        int[] d = new int[n];
        for (int i = 0; i < n; ++i) {
            d[i] = i;
        }
        for (int i = n - 2; i >= 0; --i) {
            if (s[i] <= s[d[i + 1]]) {
                d[i] = d[i + 1];
            }
        }
        for (int i = 0; i < n; ++i) {
            int j = d[i];
            if (s[i] < s[j]) {
                char t = s[i];
                s[i] = s[j];
                s[j] = t;
                break;
            }
        }
        return Integer.parseInt(String.valueOf(s));
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumSwap(int num) {
        string s = to_string(num);
        int n = s.size();
        vector<int> d(n);
        iota(d.begin(), d.end(), 0);
        for (int i = n - 2; ~i; --i) {
            if (s[i] <= s[d[i + 1]]) {
                d[i] = d[i + 1];
            }
        }
        for (int i = 0; i < n; ++i) {
            int j = d[i];
            if (s[i] < s[j]) {
                swap(s[i], s[j]);
                break;
            }
        }
        return stoi(s);
    }
};
```

#### Go

```go
func maximumSwap(num int) int {
	s := []byte(strconv.Itoa(num))
	n := len(s)
	d := make([]int, n)
	for i := range d {
		d[i] = i
	}
	for i := n - 2; i >= 0; i-- {
		if s[i] <= s[d[i+1]] {
			d[i] = d[i+1]
		}
	}
	for i, j := range d {
		if s[i] < s[j] {
			s[i], s[j] = s[j], s[i]
			break
		}
	}
	ans, _ := strconv.Atoi(string(s))
	return ans
}
```

#### TypeScript

```ts
function maximumSwap(num: number): number {
    const list = new Array();
    while (num !== 0) {
        list.push(num % 10);
        num = Math.floor(num / 10);
    }
    const n = list.length;
    const idx = new Array();
    for (let i = 0, j = 0; i < n; i++) {
        if (list[i] > list[j]) {
            j = i;
        }
        idx.push(j);
    }
    for (let i = n - 1; i >= 0; i--) {
        if (list[idx[i]] !== list[i]) {
            [list[idx[i]], list[i]] = [list[i], list[idx[i]]];
            break;
        }
    }
    let res = 0;
    for (let i = n - 1; i >= 0; i--) {
        res = res * 10 + list[i];
    }
    return res;
}
```

#### Rust

```rust
impl Solution {
    pub fn maximum_swap(mut num: i32) -> i32 {
        let mut list = {
            let mut res = Vec::new();
            while num != 0 {
                res.push(num % 10);
                num /= 10;
            }
            res
        };
        let n = list.len();
        let idx = {
            let mut i = 0;
            (0..n)
                .map(|j| {
                    if list[j] > list[i] {
                        i = j;
                    }
                    i
                })
                .collect::<Vec<usize>>()
        };
        for i in (0..n).rev() {
            if list[i] != list[idx[i]] {
                list.swap(i, idx[i]);
                break;
            }
        }
        let mut res = 0;
        for i in list.iter().rev() {
            res = res * 10 + i;
        }
        res
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Tham lam tối ưu bộ nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 lưu mảng chỉ số chữ số lớn nhất bên phải. Chỉ cần một lượt duyệt từ phải sang trái là có thể theo dõi chữ số lớn nhất đã gặp và vị trí đổi chỗ có lợi nhất ở bên trái, không cần mảng phụ.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function maximumSwap(num: number): number {
    const ans = [...String(num)];
    let [min, max, maybeMax, n] = [-1, -1, -1, ans.length];

    for (let i = n - 1; i >= 0; i--) {
        if (ans[i] > (ans[maybeMax] ?? -1)) maybeMax = i;
        if (i < maybeMax && ans[i] < ans[maybeMax]) {
            [min, max] = [i, maybeMax];
        }
    }

    if (~min && ~max && min < max) {
        [ans[min], ans[max]] = [ans[max], ans[min]];
    }

    return +ans.join('');
}
```

#### JavaScript

```js
function maximumSwap(num) {
    const ans = [...String(num)];
    let [min, max, maybeMax, n] = [-1, -1, -1, ans.length];

    for (let i = n - 1; i >= 0; i--) {
        if (ans[i] > (ans[maybeMax] ?? -1)) maybeMax = i;
        if (i < maybeMax && ans[i] < ans[maybeMax]) {
            [min, max] = [i, maybeMax];
        }
    }

    if (~min && ~max && min < max) {
        [ans[min], ans[max]] = [ans[max], ans[min]];
    }

    return +ans.join('');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
