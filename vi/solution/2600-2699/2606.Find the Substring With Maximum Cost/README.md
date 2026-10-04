---
comments: true
difficulty: Medium
rating: 1422
source: Biweekly Contest 101 Q2
tags:
    - Array
    - Hash Table
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [2606. Find the Substring With Maximum Cost](https://leetcode.com/problems/find-the-substring-with-maximum-cost)

[Tài liệu tiếng Trung](/solution/2600-2699/2606.Find%20the%20Substring%20With%20Maximum%20Cost/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code>, một chuỗi <code>chars</code> gồm các ký tự <strong>khác nhau</strong> và một mảng số nguyên <code>vals</code> có cùng độ dài với <code>chars</code>.</p>

<p><strong>Chi phí của chuỗi con</strong> là tổng giá trị của từng ký tự trong chuỗi con đó. Chi phí của chuỗi rỗng được xem là <code>0</code>.</p>

<p><strong>Giá trị của ký tự</strong> được xác định như sau:</p>

<ul>
	<li>Nếu ký tự không có trong chuỗi <code>chars</code>, giá trị của nó là vị trí tương ứng trong bảng chữ cái, tính từ <strong>1</strong>.

    <ul>
    <li>Ví dụ, giá trị của <code>&#39;a&#39;</code> là <code>1</code>, giá trị của <code>&#39;b&#39;</code> là <code>2</code>, và tiếp tục như vậy. Giá trị của <code>&#39;z&#39;</code> là <code>26</code>.</li>
    </ul>
    </li>
    <li>Ngược lại, giả sử <code>i</code> là chỉ số tại đó ký tự xuất hiện trong chuỗi <code>chars</code>, khi đó giá trị của ký tự là <code>vals[i]</code>.</li>

</ul>

<p>Trả về <em>chi phí lớn nhất trong tất cả các chuỗi con của chuỗi</em> <code>s</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;adaa&quot;, chars = &quot;d&quot;, vals = [-1000]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Giá trị của các ký tự &quot;a&quot; và &quot;d&quot; lần lượt là 1 và -1000.
Chuỗi con có chi phí lớn nhất là &quot;aa&quot; và chi phí của nó là 1 + 1 = 2.
Có thể chứng minh rằng 2 là chi phí lớn nhất.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abc&quot;, chars = &quot;abc&quot;, vals = [-1,-1,-1]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Giá trị của các ký tự &quot;a&quot;, &quot;b&quot; và &quot;c&quot; lần lượt là -1, -1 và -1.
Chuỗi con có chi phí lớn nhất là chuỗi rỗng &quot;&quot; và chi phí của nó là 0.
Có thể chứng minh rằng 0 là chi phí lớn nhất.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>1 &lt;= chars.length &lt;= 26</code></li>
	<li><code>chars</code> chỉ gồm các chữ cái tiếng Anh viết thường <strong>khác nhau</strong>.</li>
	<li><code>vals.length == chars.length</code></li>
	<li><code>-1000 &lt;= vals[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tổng tiền tố + Duy trì tổng tiền tố nhỏ nhất

<!-- thinking:start -->

> **Tư duy**
>
> Chi phí của một chuỗi con là tổng trên một đoạn; chuỗi rỗng có chi phí $0$. Việc duyệt tất cả các cặp đầu mút sẽ quá chậm với $n \le 10^5$.
>
> Với một đầu mút phải cố định, chi phí tốt nhất là tổng tiền tố hiện tại trừ đi tổng tiền tố nhỏ nhất đã gặp (bao gồm tổng tiền tố rỗng $0$). Một lần duyệt là đủ để duy trì tổng đang chạy $tot$ và giá trị nhỏ nhất $mi$, sau đó cập nhật đáp án bằng $tot-mi$.
>
> Các giá trị được lưu vào một map; những chữ cái không xuất hiện trong danh sách sẽ dùng chỉ số mặc định trong bảng chữ cái.

<!-- thinking:end -->

Theo mô tả bài toán, chúng ta duyệt từng ký tự $c$ trong chuỗi $s$, lấy giá trị tương ứng $v$, rồi cập nhật tổng tiền tố hiện tại $tot=tot+v$. Khi đó, chi phí của chuỗi con có chi phí lớn nhất kết thúc tại $c$ là $tot$ trừ đi tổng tiền tố nhỏ nhất $mi$, tức là $tot-mi$. Chúng ta cập nhật đáp án $ans=max(ans,tot-mi)$ và duy trì tổng tiền tố nhỏ nhất $mi=min(mi,tot)$.

Sau khi duyệt xong, trả về đáp án $ans$.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(C)$. Trong đó $n$ là độ dài của chuỗi $s$; còn $C$ là kích thước của tập ký tự, bằng $26$ trong bài toán này.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumCostSubstring(self, s: str, chars: str, vals: List[int]) -> int:
        d = {c: v for c, v in zip(chars, vals)}
        ans = tot = mi = 0
        for c in s:
            v = d.get(c, ord(c) - ord('a') + 1)
            tot += v
            ans = max(ans, tot - mi)
            mi = min(mi, tot)
        return ans
```

#### Java

```java
class Solution {
    public int maximumCostSubstring(String s, String chars, int[] vals) {
        int[] d = new int[26];
        for (int i = 0; i < d.length; ++i) {
            d[i] = i + 1;
        }
        int m = chars.length();
        for (int i = 0; i < m; ++i) {
            d[chars.charAt(i) - 'a'] = vals[i];
        }
        int ans = 0, tot = 0, mi = 0;
        int n = s.length();
        for (int i = 0; i < n; ++i) {
            int v = d[s.charAt(i) - 'a'];
            tot += v;
            ans = Math.max(ans, tot - mi);
            mi = Math.min(mi, tot);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumCostSubstring(string s, string chars, vector<int>& vals) {
        vector<int> d(26);
        iota(d.begin(), d.end(), 1);
        int m = chars.size();
        for (int i = 0; i < m; ++i) {
            d[chars[i] - 'a'] = vals[i];
        }
        int ans = 0, tot = 0, mi = 0;
        for (char& c : s) {
            int v = d[c - 'a'];
            tot += v;
            ans = max(ans, tot - mi);
            mi = min(mi, tot);
        }
        return ans;
    }
};
```

#### Go

```go
func maximumCostSubstring(s string, chars string, vals []int) (ans int) {
	d := [26]int{}
	for i := range d {
		d[i] = i + 1
	}
	for i, c := range chars {
		d[c-'a'] = vals[i]
	}
	tot, mi := 0, 0
	for _, c := range s {
		v := d[c-'a']
		tot += v
		ans = max(ans, tot-mi)
		mi = min(mi, tot)
	}
	return
}
```

#### TypeScript

```ts
function maximumCostSubstring(s: string, chars: string, vals: number[]): number {
    const d: number[] = Array.from({ length: 26 }, (_, i) => i + 1);
    for (let i = 0; i < chars.length; ++i) {
        d[chars.charCodeAt(i) - 97] = vals[i];
    }
    let ans = 0;
    let tot = 0;
    let mi = 0;
    for (const c of s) {
        tot += d[c.charCodeAt(0) - 97];
        ans = Math.max(ans, tot - mi);
        mi = Math.min(mi, tot);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Chuyển thành bài toán tổng lớn nhất của mảng con

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 duy trì tổng tiền tố nhỏ nhất trên toàn cục. Cùng một bài toán có thể xem là bài toán mảng con có tổng lớn nhất: nếu tổng đang chạy âm thì nên loại bỏ nó trước ký tự tiếp theo.
>
> Công thức Kadane $f=\max(f,0)+v$ loại bỏ mảng tổng tiền tố; việc khởi tạo đáp án bằng $0$ vẫn bao quát trường hợp chuỗi rỗng.

<!-- thinking:end -->

Chúng ta có thể xem giá trị $v$ của mỗi ký tự $c$ là một số nguyên, vì vậy bài toán thực chất là giải bài toán tổng lớn nhất của mảng con.

Chúng ta dùng biến $f$ để duy trì chi phí của chuỗi con có chi phí lớn nhất kết thúc tại ký tự hiện tại $c$. Mỗi khi duyệt đến một ký tự $c$, chúng ta cập nhật $f=max(f, 0) + v$. Sau đó, chúng ta cập nhật đáp án $ans=max(ans,f)$.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(C)$. Trong đó $n$ là độ dài của chuỗi $s$; còn $C$ là kích thước của tập ký tự, bằng $26$ trong bài toán này.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumCostSubstring(self, s: str, chars: str, vals: List[int]) -> int:
        d = {c: v for c, v in zip(chars, vals)}
        ans = f = 0
        for c in s:
            v = d.get(c, ord(c) - ord('a') + 1)
            f = max(f, 0) + v
            ans = max(ans, f)
        return ans
```

#### Java

```java
class Solution {
    public int maximumCostSubstring(String s, String chars, int[] vals) {
        int[] d = new int[26];
        for (int i = 0; i < d.length; ++i) {
            d[i] = i + 1;
        }
        int m = chars.length();
        for (int i = 0; i < m; ++i) {
            d[chars.charAt(i) - 'a'] = vals[i];
        }
        int ans = 0, f = 0;
        int n = s.length();
        for (int i = 0; i < n; ++i) {
            int v = d[s.charAt(i) - 'a'];
            f = Math.max(f, 0) + v;
            ans = Math.max(ans, f);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumCostSubstring(string s, string chars, vector<int>& vals) {
        vector<int> d(26);
        iota(d.begin(), d.end(), 1);
        int m = chars.size();
        for (int i = 0; i < m; ++i) {
            d[chars[i] - 'a'] = vals[i];
        }
        int ans = 0, f = 0;
        for (char& c : s) {
            int v = d[c - 'a'];
            f = max(f, 0) + v;
            ans = max(ans, f);
        }
        return ans;
    }
};
```

#### Go

```go
func maximumCostSubstring(s string, chars string, vals []int) (ans int) {
	d := [26]int{}
	for i := range d {
		d[i] = i + 1
	}
	for i, c := range chars {
		d[c-'a'] = vals[i]
	}
	f := 0
	for _, c := range s {
		v := d[c-'a']
		f = max(f, 0) + v
		ans = max(ans, f)
	}
	return
}
```

#### TypeScript

```ts
function maximumCostSubstring(s: string, chars: string, vals: number[]): number {
    const d: number[] = Array.from({ length: 26 }, (_, i) => i + 1);
    for (let i = 0; i < chars.length; ++i) {
        d[chars.charCodeAt(i) - 97] = vals[i];
    }
    let ans = 0;
    let f = 0;
    for (const c of s) {
        f = Math.max(f, 0) + d[c.charCodeAt(0) - 97];
        ans = Math.max(ans, f);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
