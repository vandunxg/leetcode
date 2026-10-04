---
comments: true
difficulty: Medium
rating: 2172
source: Weekly Contest 366 Q3
tags:
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [2896. Apply Operations to Make Two Strings Equal](https://leetcode.com/problems/apply-operations-to-make-two-strings-equal)

[中文文档](/solution/2800-2899/2896.Apply%20Operations%20to%20Make%20Two%20Strings%20Equal/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi nhị phân <strong>đánh chỉ số từ 0</strong> <code>s1</code> và <code>s2</code>, cả hai đều có độ dài <code>n</code>, cùng một số nguyên dương <code>x</code>.</p>

<p>Bạn có thể thực hiện bất kỳ thao tác nào sau đây trên chuỗi <code>s1</code> <strong>bao nhiêu lần tùy ý</strong>:</p>

<ul>
	<li>Chọn hai chỉ số <code>i</code> và <code>j</code>, sau đó đảo cả <code>s1[i]</code> và <code>s1[j]</code>. Chi phí của thao tác này là <code>x</code>.</li>
	<li>Chọn một chỉ số <code>i</code> sao cho <code>i &lt; n - 1</code>, sau đó đảo cả <code>s1[i]</code> và <code>s1[i + 1]</code>. Chi phí của thao tác này là <code>1</code>.</li>
</ul>

<p>Trả về <em>chi phí <strong>nhỏ nhất</strong> cần thiết để biến </em><code>s1</code><em> và </em><code>s2</code><em> thành hai chuỗi bằng nhau, hoặc trả về </em><code>-1</code><em> nếu không thể.</em></p>

<p><strong>Lưu ý</strong> rằng đảo một ký tự nghĩa là chuyển ký tự đó từ <code>0</code> thành <code>1</code> hoặc ngược lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s1 = &quot;1100011000&quot;, s2 = &quot;0101001010&quot;, x = 2
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Ta có thể thực hiện các thao tác sau:
- Chọn i = 3 và thực hiện thao tác thứ hai. Chuỗi thu được là s1 = &quot;110<u><strong>11</strong></u>11000&quot;.
- Chọn i = 4 và thực hiện thao tác thứ hai. Chuỗi thu được là s1 = &quot;1101<strong><u>00</u></strong>1000&quot;.
- Chọn i = 0 và j = 8, sau đó thực hiện thao tác thứ nhất. Chuỗi thu được là s1 = &quot;<u><strong>0</strong></u>1010010<u><strong>1</strong></u>0&quot; = s2.
Tổng chi phí là 1 + 1 + 2 = 4. Có thể chứng minh đây là chi phí nhỏ nhất có thể.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s1 = &quot;10110&quot;, s2 = &quot;00011&quot;, x = 4
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không thể biến hai chuỗi thành hai chuỗi bằng nhau.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == s1.length == s2.length</code></li>
	<li><code>1 &lt;= n, x &lt;= 500</code></li>
	<li><code>s1</code> và <code>s2</code> chỉ gồm các ký tự <code>&#39;0&#39;</code> và <code>&#39;1&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Memoization

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác đảo hai bit, nên không thể xử lý số lượng vị trí khác nhau là số lẻ. Ta thu thập các chỉ số bị khác nhau và để $dfs(i,j)$ chọn phương án tốt nhất: ghép hai đầu mút với chi phí $x$, hoặc ghép hai chỉ số ngoài cùng bên trái / bên phải với chi phí bằng khoảng cách.

<!-- thinking:end -->

Ta nhận thấy mỗi thao tác đều đảo hai ký tự, nên nếu số ký tự khác nhau giữa hai chuỗi là số lẻ thì không thể biến chúng thành bằng nhau; khi đó ta trả về $-1$. Ngược lại, ta lưu các chỉ số mà hai chuỗi khác nhau vào mảng $idx$, với $m$ là độ dài của $idx$.

Tiếp theo, ta xây dựng hàm $dfs(i, j)$, biểu diễn chi phí nhỏ nhất để đảo các ký tự tại $idx[i..j]$. Đáp án là $dfs(0, m - 1)$.

Quá trình tính hàm $dfs(i, j)$ như sau:

Nếu $i > j$, ta không cần thực hiện thao tác nào và trả về $0$.

Ngược lại, ta xét hai đầu mút của đoạn $[i, j]$:

- Nếu thực hiện thao tác thứ nhất trên đầu mút $i$, vì chi phí $x$ là cố định nên lựa chọn tối ưu là đảo $idx[i]$ và $idx[j]$, sau đó đệ quy tính $dfs(i + 1, j - 1)$, với tổng chi phí là $dfs(i + 1, j - 1) + x$.
- Nếu thực hiện thao tác thứ hai trên đầu mút $i$, ta cần đảo tất cả ký tự trong $[idx[i]..idx[i + 1]]$, sau đó đệ quy tính $dfs(i + 2, j)$, với tổng chi phí là $dfs(i + 2, j) + idx[i + 1] - idx[i]$.
- Nếu thực hiện thao tác thứ hai trên đầu mút $j$, ta cần đảo tất cả ký tự trong $[idx[j - 1]..idx[j]]$, sau đó đệ quy tính $dfs(i, j - 2)$, với tổng chi phí là $dfs(i, j - 2) + idx[j] - idx[j - 1]$.

Ta lấy giá trị nhỏ nhất trong ba phương án trên làm giá trị của $dfs(i, j)$.

Để tránh tính toán lặp lại, ta có thể dùng memoization để lưu kết quả trả về của $dfs(i, j)$ trong mảng hai chiều $f$. Nếu $f[i][j]$ khác $-1$, nghĩa là ta đã tính giá trị này, nên có thể trả về trực tiếp $f[i][j]$.

Độ phức tạp thời gian là $O(n^2)$, độ phức tạp không gian là $O(n^2)$. Trong đó, $n$ là độ dài của các chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, s1: str, s2: str, x: int) -> int:
        @cache
        def dfs(i: int, j: int) -> int:
            if i > j:
                return 0
            a = dfs(i + 1, j - 1) + x
            b = dfs(i + 2, j) + idx[i + 1] - idx[i]
            c = dfs(i, j - 2) + idx[j] - idx[j - 1]
            return min(a, b, c)

        n = len(s1)
        idx = [i for i in range(n) if s1[i] != s2[i]]
        m = len(idx)
        if m & 1:
            return -1
        return dfs(0, m - 1)
```

#### Java

```java
class Solution {
    private List<Integer> idx = new ArrayList<>();
    private Integer[][] f;
    private int x;

    public int minOperations(String s1, String s2, int x) {
        int n = s1.length();
        for (int i = 0; i < n; ++i) {
            if (s1.charAt(i) != s2.charAt(i)) {
                idx.add(i);
            }
        }
        int m = idx.size();
        if (m % 2 == 1) {
            return -1;
        }
        this.x = x;
        f = new Integer[m][m];
        return dfs(0, m - 1);
    }

    private int dfs(int i, int j) {
        if (i > j) {
            return 0;
        }
        if (f[i][j] != null) {
            return f[i][j];
        }
        f[i][j] = dfs(i + 1, j - 1) + x;
        f[i][j] = Math.min(f[i][j], dfs(i + 2, j) + idx.get(i + 1) - idx.get(i));
        f[i][j] = Math.min(f[i][j], dfs(i, j - 2) + idx.get(j) - idx.get(j - 1));
        return f[i][j];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minOperations(string s1, string s2, int x) {
        vector<int> idx;
        for (int i = 0; i < s1.size(); ++i) {
            if (s1[i] != s2[i]) {
                idx.push_back(i);
            }
        }
        int m = idx.size();
        if (m & 1) {
            return -1;
        }
        if (m == 0) {
            return 0;
        }
        int f[m][m];
        memset(f, -1, sizeof(f));
        function<int(int, int)> dfs = [&](int i, int j) {
            if (i > j) {
                return 0;
            }
            if (f[i][j] != -1) {
                return f[i][j];
            }
            f[i][j] = min({dfs(i + 1, j - 1) + x, dfs(i + 2, j) + idx[i + 1] - idx[i], dfs(i, j - 2) + idx[j] - idx[j - 1]});
            return f[i][j];
        };
        return dfs(0, m - 1);
    }
};
```

#### Go

```go
func minOperations(s1 string, s2 string, x int) int {
	idx := []int{}
	for i := range s1 {
		if s1[i] != s2[i] {
			idx = append(idx, i)
		}
	}
	m := len(idx)
	if m&1 == 1 {
		return -1
	}
	f := make([][]int, m)
	for i := range f {
		f[i] = make([]int, m)
		for j := range f[i] {
			f[i][j] = -1
		}
	}
	var dfs func(i, j int) int
	dfs = func(i, j int) int {
		if i > j {
			return 0
		}
		if f[i][j] != -1 {
			return f[i][j]
		}
		f[i][j] = dfs(i+1, j-1) + x
		f[i][j] = min(f[i][j], dfs(i+2, j)+idx[i+1]-idx[i])
		f[i][j] = min(f[i][j], dfs(i, j-2)+idx[j]-idx[j-1])
		return f[i][j]
	}
	return dfs(0, m-1)
}
```

#### TypeScript

```ts
function minOperations(s1: string, s2: string, x: number): number {
    const idx: number[] = [];
    for (let i = 0; i < s1.length; ++i) {
        if (s1[i] !== s2[i]) {
            idx.push(i);
        }
    }
    const m = idx.length;
    if (m % 2 === 1) {
        return -1;
    }
    if (m === 0) {
        return 0;
    }
    const f: number[][] = Array.from({ length: m }, () => Array.from({ length: m }, () => -1));
    const dfs = (i: number, j: number): number => {
        if (i > j) {
            return 0;
        }
        if (f[i][j] !== -1) {
            return f[i][j];
        }
        f[i][j] = dfs(i + 1, j - 1) + x;
        f[i][j] = Math.min(f[i][j], dfs(i + 2, j) + idx[i + 1] - idx[i]);
        f[i][j] = Math.min(f[i][j], dfs(i, j - 2) + idx[j] - idx[j - 1]);
        return f[i][j];
    };
    return dfs(0, m - 1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Memoization trên các đoạn có độ phức tạp $O(m^2)$. Quy hoạch động từ trái sang phải với số lượng trạng thái không đổi có thể tính cùng giá trị nhỏ nhất chỉ trong một lượt.

<!-- thinking:end -->

Ta duy trì một vài trạng thái tuyến tính khi quét từ trái sang phải.

<!-- tabs:start -->

#### Java

```java
class Solution {
    public int minOperations(String s1, String s2, int x) {
        int n = s1.length();
        int inf = 50_000;
        int one = inf, two = inf, last = inf;
        int done = 0;
        for (int i = 0; i < n; i++) {
            if (s1.charAt(i) == s2.charAt(i)) {
                one = Math.min(one, last);
                last = last + 1;
                two = two + 1;
                continue;
            }
            if (done < n) {
                one = Math.min(two + 1, done + x);
                last = Math.min(two + x, done);
                done = two = inf;
                continue;
            }
            done = Math.min(one + x, last + 1);
            two = one;
            one = last = inf;
            continue;
        }
        return done == inf ? -1 : done;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
