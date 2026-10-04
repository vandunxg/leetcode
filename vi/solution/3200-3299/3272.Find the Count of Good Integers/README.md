---
comments: true
difficulty: Hard
rating: 2382
source: Biweekly Contest 138 Q3
tags:
    - Hash Table
    - Math
    - Combinatorics
    - Enumeration
---

<!-- problem:start -->

# [3272. Find the Count of Good Integers](https://leetcode.com/problems/find-the-count-of-good-integers)

[中文文档](/solution/3200-3299/3272.Find%20the%20Count%20of%20Good%20Integers/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai số nguyên <strong>dương</strong> <code>n</code> và <code>k</code>.</p>

<p>Một số nguyên <code>x</code> được gọi là <strong>k-palindromic</strong> nếu:</p>

<ul>
	<li><code>x</code> là một <span data-keyword="palindrome-integer">palindrome</span>.</li>
	<li><code>x</code> chia hết cho <code>k</code>.</li>
</ul>

<p>Một số nguyên được gọi là <strong>good</strong> nếu các chữ số của nó có thể được <em>sắp xếp lại</em> để tạo thành một số nguyên <strong>k-palindromic</strong>. Ví dụ, với <code>k = 2</code>, 2020 có thể được sắp xếp lại thành số nguyên <em>k-palindromic</em> 2002, còn 1010 thì không thể được sắp xếp lại thành một số nguyên <em>k-palindromic</em>.</p>

<p>Hãy trả về số lượng số nguyên <strong>good</strong> có <code>n</code> chữ số.</p>

<p><strong>Lưu ý</strong> rằng <em>bất kỳ</em> số nguyên nào cũng <strong>không được</strong> có số 0 ở đầu, <strong>dù là</strong> trước <strong>hay</strong> sau khi sắp xếp lại. Ví dụ, 1010 <em>không thể</em> được sắp xếp lại để tạo thành 101.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, k = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">27</span></p>

<p><strong>Giải thích:</strong></p>

<p><em>Một vài</em> số nguyên good là:</p>

<ul>
	<li>551 vì nó có thể được sắp xếp lại thành 515.</li>
	<li>525 vì bản thân nó đã là k-palindromic.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 1, k = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Hai số nguyên good là 4 và 8.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5, k = 6</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2468</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10</code></li>
	<li><code>1 &lt;= k &lt;= 9</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê + Tổ hợp

<!-- thinking:start -->

> **Tư duy**
>
> Một số nguyên good là một số có $n$ chữ số với các chữ số giống với một palindrome nào đó chia hết cho $k$. Vì $n\le 10$, không thể liệt kê tất cả các số có $n$ chữ số; một palindrome được xác định bởi nửa đầu của nó, nên chỉ cần khoảng $10^{\lceil n/2\rceil}$ số.
>
> Xây dựng từng palindrome từ nửa đầu; nếu nó chia hết cho $k$, cộng số hoán vị của đa tập chữ số đó (chữ số đầu khác 0), dùng chuỗi chữ số đã sắp xếp làm khóa để đánh dấu. Giai thừa cho ta $\frac{(n-x_0)(n-1)!}{\prod x_i!}$.

<!-- thinking:end -->

Ta có thể liệt kê tất cả các số palindrome có độ dài $n$ và kiểm tra xem chúng có phải là số $k$-palindromic hay không. Do tính chất của số palindrome, ta chỉ cần liệt kê nửa đầu các chữ số, sau đó đảo ngược và nối vào để tạo thành số hoàn chỉnh.

Độ dài của nửa đầu các chữ số là $\lfloor \frac{n - 1}{2} \rfloor$, nên phạm vi của nửa đầu là $[10^{\lfloor \frac{n - 1}{2} \rfloor}, 10^{\lfloor \frac{n - 1}{2} \rfloor + 1})$. Ta có thể đảo ngược nửa đầu rồi nối vào để tạo thành số palindrome có độ dài $n$. Lưu ý rằng nếu $n$ là số lẻ, chữ số ở giữa cần được xử lý đặc biệt.

Tiếp theo, ta kiểm tra xem số palindrome có phải là số $k$-palindromic hay không. Nếu đúng, ta đếm tất cả các hoán vị phân biệt của các chữ số trong số đó. Để tránh trùng lặp, ta có thể dùng một set $\textit{vis}$ để lưu hoán vị nhỏ nhất của mỗi số palindrome đã được xử lý. Nếu hoán vị nhỏ nhất của số palindrome hiện tại đã có trong set, ta bỏ qua nó. Nếu chưa, ta tính số hoán vị phân biệt của số palindrome và cộng vào kết quả.

Ta có thể dùng một mảng $\textit{cnt}$ để đếm số lần xuất hiện của mỗi chữ số, sau đó dùng tổ hợp để tính số hoán vị. Cụ thể, nếu chữ số $0$ xuất hiện $x_0$ lần, chữ số $1$ xuất hiện $x_1$ lần, ..., chữ số $9$ xuất hiện $x_9$ lần, số hoán vị của số palindrome là:

$$
\frac{(n - x_0) \cdot (n - 1)!}{x_0! \cdot x_1! \cdots x_9!}
$$

Ở đây, $(n - x_0)$ biểu thị số lựa chọn cho chữ số đầu tiên (không tính $0$), $(n - 1)!$ biểu thị số hoán vị của các chữ số còn lại, và ta chia cho giai thừa số lần xuất hiện của mỗi chữ số để loại bỏ các hoán vị trùng lặp.

Cuối cùng, ta cộng tất cả số lượng hoán vị để thu được kết quả.

Độ phức tạp thời gian là $O(10^m \times n \times \log n)$, và độ phức tạp không gian là $O(10^m \times n)$, trong đó $m = \lfloor \frac{n - 1}{2} \rfloor$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countGoodIntegers(self, n: int, k: int) -> int:
        fac = [factorial(i) for i in range(n + 1)]
        ans = 0
        vis = set()
        base = 10 ** ((n - 1) // 2)
        for i in range(base, base * 10):
            s = str(i)
            s += s[::-1][n % 2 :]
            if int(s) % k:
                continue
            t = "".join(sorted(s))
            if t in vis:
                continue
            vis.add(t)
            cnt = Counter(t)
            res = (n - cnt["0"]) * fac[n - 1]
            for x in cnt.values():
                res //= fac[x]
            ans += res
        return ans
```

#### Java

```java
class Solution {
    public long countGoodIntegers(int n, int k) {
        long[] fac = new long[n + 1];
        fac[0] = 1;
        for (int i = 1; i <= n; i++) {
            fac[i] = fac[i - 1] * i;
        }

        long ans = 0;
        Set<String> vis = new HashSet<>();
        int base = (int) Math.pow(10, (n - 1) / 2);

        for (int i = base; i < base * 10; i++) {
            String s = String.valueOf(i);
            StringBuilder sb = new StringBuilder(s).reverse();
            s += sb.substring(n % 2);
            if (Long.parseLong(s) % k != 0) {
                continue;
            }

            char[] arr = s.toCharArray();
            Arrays.sort(arr);
            String t = new String(arr);
            if (vis.contains(t)) {
                continue;
            }
            vis.add(t);
            int[] cnt = new int[10];
            for (char c : arr) {
                cnt[c - '0']++;
            }

            long res = (n - cnt[0]) * fac[n - 1];
            for (int x : cnt) {
                res /= fac[x];
            }
            ans += res;
        }

        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long countGoodIntegers(int n, int k) {
        vector<long long> fac(n + 1, 1);
        for (int i = 1; i <= n; ++i) {
            fac[i] = fac[i - 1] * i;
        }

        long long ans = 0;
        unordered_set<string> vis;
        int base = pow(10, (n - 1) / 2);

        for (int i = base; i < base * 10; ++i) {
            string s = to_string(i);
            string rev = s;
            reverse(rev.begin(), rev.end());
            s += rev.substr(n % 2);
            if (stoll(s) % k) {
                continue;
            }
            string t = s;
            sort(t.begin(), t.end());
            if (vis.count(t)) {
                continue;
            }
            vis.insert(t);
            vector<int> cnt(10);
            for (char c : t) {
                cnt[c - '0']++;
            }
            long long res = (n - cnt[0]) * fac[n - 1];
            for (int x : cnt) {
                res /= fac[x];
            }
            ans += res;
        }
        return ans;
    }
};
```

#### Go

```go
func factorial(n int) []int64 {
	fac := make([]int64, n+1)
	fac[0] = 1
	for i := 1; i <= n; i++ {
		fac[i] = fac[i-1] * int64(i)
	}
	return fac
}

func countGoodIntegers(n int, k int) (ans int64) {
	fac := factorial(n)
	vis := make(map[string]bool)
	base := int(math.Pow(10, float64((n-1)/2)))

	for i := base; i < base*10; i++ {
		s := strconv.Itoa(i)
		rev := reverseString(s)
		s += rev[n%2:]
		num, _ := strconv.ParseInt(s, 10, 64)
		if num%int64(k) != 0 {
			continue
		}
		bs := []byte(s)
		slices.Sort(bs)
		t := string(bs)

		if vis[t] {
			continue
		}
		vis[t] = true
		cnt := make([]int, 10)
		for _, c := range t {
			cnt[c-'0']++
		}
		res := (int64(n) - int64(cnt[0])) * fac[n-1]
		for _, x := range cnt {
			res /= fac[x]
		}
		ans += res
	}

	return
}

func reverseString(s string) string {
	t := []byte(s)
	for i, j := 0, len(t)-1; i < j; i, j = i+1, j-1 {
		t[i], t[j] = t[j], t[i]
	}
	return string(t)
}
```

#### TypeScript

```ts
function countGoodIntegers(n: number, k: number): number {
    const fac = factorial(n);
    let ans = 0;
    const vis = new Set<string>();
    const base = Math.pow(10, Math.floor((n - 1) / 2));

    for (let i = base; i < base * 10; i++) {
        let s = `${i}`;
        const rev = reverseString(s);
        if (n % 2 === 1) {
            s += rev.substring(1);
        } else {
            s += rev;
        }

        if (+s % k !== 0) {
            continue;
        }

        const bs = Array.from(s).sort();
        const t = bs.join('');

        if (vis.has(t)) {
            continue;
        }

        vis.add(t);

        const cnt = Array(10).fill(0);
        for (const c of t) {
            cnt[+c]++;
        }

        let res = (n - cnt[0]) * fac[n - 1];
        for (const x of cnt) {
            res /= fac[x];
        }
        ans += res;
    }

    return ans;
}

function factorial(n: number): number[] {
    const fac = Array(n + 1).fill(1);
    for (let i = 1; i <= n; i++) {
        fac[i] = fac[i - 1] * i;
    }
    return fac;
}

function reverseString(s: string): string {
    return s.split('').reverse().join('');
}
```

#### Rust

```rust
impl Solution {
    pub fn count_good_integers(n: i32, k: i32) -> i64 {
        use std::collections::HashSet;
        let n = n as usize;
        let k = k as i64;
        let mut fac = vec![1_i64; n + 1];
        for i in 1..=n {
            fac[i] = fac[i - 1] * i as i64;
        }

        let mut ans = 0;
        let mut vis = HashSet::new();
        let base = 10_i64.pow(((n - 1) / 2) as u32);

        for i in base..base * 10 {
            let s = i.to_string();
            let rev: String = s.chars().rev().collect();
            let full_s = if n % 2 == 0 {
                format!("{}{}", s, rev)
            } else {
                format!("{}{}", s, &rev[1..])
            };

            let num: i64 = full_s.parse().unwrap();
            if num % k != 0 {
                continue;
            }

            let mut arr: Vec<char> = full_s.chars().collect();
            arr.sort_unstable();
            let t: String = arr.iter().collect();
            if vis.contains(&t) {
                continue;
            }
            vis.insert(t);

            let mut cnt = vec![0; 10];
            for c in arr {
                cnt[c as usize - '0' as usize] += 1;
            }

            let mut res = (n - cnt[0]) as i64 * fac[n - 1];
            for &x in &cnt {
                if x > 0 {
                    res /= fac[x];
                }
            }
            ans += res;
        }

        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {number} n
 * @param {number} k
 * @return {number}
 */
var countGoodIntegers = function (n, k) {
    const fac = factorial(n);
    let ans = 0;
    const vis = new Set();
    const base = Math.pow(10, Math.floor((n - 1) / 2));

    for (let i = base; i < base * 10; i++) {
        let s = String(i);
        const rev = reverseString(s);
        if (n % 2 === 1) {
            s += rev.substring(1);
        } else {
            s += rev;
        }

        if (parseInt(s, 10) % k !== 0) {
            continue;
        }

        const bs = Array.from(s).sort();
        const t = bs.join('');

        if (vis.has(t)) {
            continue;
        }

        vis.add(t);

        const cnt = Array(10).fill(0);
        for (const c of t) {
            cnt[parseInt(c, 10)]++;
        }

        let res = (n - cnt[0]) * fac[n - 1];
        for (const x of cnt) {
            res /= fac[x];
        }
        ans += res;
    }

    return ans;
};

function factorial(n) {
    const fac = Array(n + 1).fill(1);
    for (let i = 1; i <= n; i++) {
        fac[i] = fac[i - 1] * i;
    }
    return fac;
}

function reverseString(s) {
    return s.split('').reverse().join('');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
