---
comments: true
difficulty: Medium
rating: 1646
source: Biweekly Contest 20 Q3
tags:
    - Hash Table
    - String
    - Sliding Window
---

<!-- problem:start -->

# [1358. Number of Substrings Containing All Three Characters](https://leetcode.com/problems/number-of-substrings-containing-all-three-characters)

[中文文档](/solution/1300-1399/1358.Number%20of%20Substrings%20Containing%20All%20Three%20Characters/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code> chỉ gồm các ký tự <em>a</em>, <em>b</em> và <em>c</em>.</p>

<p>Trả về số chuỗi con có <b>ít nhất</b> một lần xuất hiện của cả ba ký tự <em>a</em>, <em>b</em> và <em>c</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abcabc&quot;
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong> Các chuỗi con chứa&nbsp;ít nhất&nbsp;một lần xuất hiện của cả ba ký tự&nbsp;<em>a</em>,&nbsp;<em>b</em>&nbsp;và&nbsp;<em>c</em> là &quot;abc&quot;, &quot;abca&quot;, &quot;abcab&quot;, &quot;abcabc&quot;, &quot;bca&quot;, &quot;bcab&quot;, &quot;bcabc&quot;, &quot;cab&quot;, &quot;cabc&quot; và &quot;abc&quot; (<strong>tính thêm một lần</strong>).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;aaacb&quot;
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Các chuỗi con chứa&nbsp;ít nhất&nbsp;một lần xuất hiện của cả ba ký tự&nbsp;<em>a</em>,&nbsp;<em>b</em>&nbsp;và&nbsp;<em>c</em> là &quot;aaacb&quot;, &quot;aacb&quot; và &quot;acb&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abc&quot;
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= s.length &lt;= 5 x 10<sup>4</sup></code></li>
	<li><code>s</code>&nbsp;chỉ gồm các ký tự <code>&#39;a&#39;</code>, <code>&#39;b&#39;</code> hoặc <code>&#39;c&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Một lượt duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các chuỗi con chứa $a$, $b$ và $c$. Vì $n \le 5 \times 10^4$, không thể duyệt mọi cặp đầu mút. Với đầu phải $i$, mọi đầu trái không lớn hơn vị trí xuất hiện gần nhất sớm nhất trong ba ký tự đều tạo thành chuỗi con hợp lệ. Theo dõi ba chỉ số này, tại mỗi $i$ ta cộng thêm $\min(d[a],d[b],d[c])+1$.

<!-- thinking:end -->

Ta dùng mảng $d$ có độ dài $3$ để lưu vị trí xuất hiện gần nhất của ba ký tự; ban đầu tất cả được gán bằng $-1$.

Ta duyệt chuỗi $s$. Tại vị trí hiện tại $i$, trước tiên cập nhật $d[s[i]]=i$. Khi đó, số chuỗi con hợp lệ là $\min(d[0], d[1], d[2]) + 1$; ta cộng giá trị này vào đáp án.

Độ phức tạp thời gian là $O(n)$, với $n$ là độ dài chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfSubstrings(self, s: str) -> int:
        d = {"a": -1, "b": -1, "c": -1}
        ans = 0
        for i, c in enumerate(s):
            d[c] = i
            ans += min(d["a"], d["b"], d["c"]) + 1
        return ans
```

#### Java

```java
class Solution {
    public int numberOfSubstrings(String s) {
        int[] d = new int[] {-1, -1, -1};
        int ans = 0;
        for (int i = 0; i < s.length(); ++i) {
            char c = s.charAt(i);
            d[c - 'a'] = i;
            ans += Math.min(d[0], Math.min(d[1], d[2])) + 1;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfSubstrings(string s) {
        int d[3] = {-1, -1, -1};
        int ans = 0;
        for (int i = 0; i < s.size(); ++i) {
            d[s[i] - 'a'] = i;
            ans += min(d[0], min(d[1], d[2])) + 1;
        }
        return ans;
    }
};
```

#### Go

```go
func numberOfSubstrings(s string) (ans int) {
	d := [3]int{-1, -1, -1}
	for i, c := range s {
		d[c-'a'] = i
		ans += min(d[0], min(d[1], d[2])) + 1
	}
	return
}
```

#### TypeScript

```ts
function numberOfSubstrings(s: string): number {
    const d: number[] = [-1, -1, -1];
    let ans = 0;

    for (let i = 0; i < s.length; i++) {
        const c = s.charCodeAt(i) - 97;
        d[c] = i;

        ans += Math.min(d[0], Math.min(d[1], d[2])) + 1;
    }

    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn number_of_substrings(s: String) -> i32 {
        let bytes = s.as_bytes();
        let mut d = [-1i32; 3];
        let mut ans: i32 = 0;

        for i in 0..bytes.len() {
            let c = (bytes[i] - b'a') as usize;
            d[c] = i as i32;

            let mn = d[0].min(d[1]).min(d[2]);
            ans += mn + 1;
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Sliding window

<!-- thinking:start -->

> **Tư duy**
>
> Cách đầu tiên dùng các chỉ số xuất hiện gần nhất. Ta cũng có thể dùng sliding window đếm tần suất: sau khi tăng $r$, thu hẹp $l$ trong khi cửa sổ vẫn chứa đủ cả ba ký tự, rồi cộng giá trị $l hiện tại vào đáp án vì đó là số đầu trái hợp lệ. Cả hai cách đều có độ phức tạp tuyến tính; sliding window không cần lưu vị trí xuất hiện gần nhất.

<!-- thinking:end -->

Ta có thể giải bài toán bằng sliding window. Duy trì cửa sổ $[l, r]$ và mảng $\textit{cnt}$ để lưu tần suất từng ký tự trong cửa sổ.

Duyệt chuỗi và liên tục dịch biên phải $r$ để thêm $s[r]$ vào cửa sổ. Nếu cửa sổ chứa ít nhất một ký tự $a$, $b$ và $c$, tiếp tục dịch biên trái $l$ sang phải cho đến khi cửa sổ không còn chứa đủ cả ba ký tự.

Lúc này, các chuỗi con kết thúc tại $r$ và chứa đủ $a$, $b$, $c$ có thể bắt đầu tại các chỉ số $0, 1, \ldots, l - 1$, tổng cộng có $l$ chuỗi con hợp lệ. Cộng số lượng này vào đáp án.

Độ phức tạp thời gian là $O(n)$, với $n$ là độ dài chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfSubstrings(self, s: str) -> int:
        ans = l = 0
        cnt = Counter()
        for r, c in enumerate(s):
            cnt[c] += 1
            while cnt['a'] and cnt['b'] and cnt['c']:
                cnt[s[l]] -= 1
                l += 1
            ans += l
        return ans
```

#### Java

```java
class Solution {
    public int numberOfSubstrings(String s) {
        int ans = 0, l = 0;
        int[] cnt = new int[3];
        for (int r = 0; r < s.length(); r++) {
            char c = s.charAt(r);
            cnt[c - 'a']++;
            while (cnt[0] > 0 && cnt[1] > 0 && cnt[2] > 0) {
                cnt[s.charAt(l) - 'a']--;
                l++;
            }
            ans += l;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfSubstrings(string s) {
        int ans = 0, l = 0;
        int cnt[3] = {0, 0, 0};
        for (int r = 0; r < (int) s.size(); r++) {
            cnt[s[r] - 'a']++;
            while (cnt[0] && cnt[1] && cnt[2]) {
                cnt[s[l] - 'a']--;
                l++;
            }
            ans += l;
        }
        return ans;
    }
};
```

#### Go

```go
func numberOfSubstrings(s string) int {
	ans, l := 0, 0
	cnt := [3]int{}

	for r := 0; r < len(s); r++ {
		cnt[s[r]-'a']++

		for cnt[0] > 0 && cnt[1] > 0 && cnt[2] > 0 {
			cnt[s[l]-'a']--
			l++
		}

		ans += l
	}

	return ans
}
```

#### TypeScript

```ts
function numberOfSubstrings(s: string): number {
    let ans = 0,
        l = 0;
    const cnt = [0, 0, 0];

    for (let r = 0; r < s.length; r++) {
        cnt[s.charCodeAt(r) - 97]++;

        while (cnt[0] > 0 && cnt[1] > 0 && cnt[2] > 0) {
            cnt[s.charCodeAt(l) - 97]--;
            l++;
        }

        ans += l;
    }

    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn number_of_substrings(s: String) -> i32 {
        let bytes = s.as_bytes();
        let mut ans = 0;
        let mut l = 0;
        let mut cnt = [0; 3];

        for r in 0..bytes.len() {
            cnt[(bytes[r] - b'a') as usize] += 1;

            while cnt[0] > 0 && cnt[1] > 0 && cnt[2] > 0 {
                cnt[(bytes[l] - b'a') as usize] -= 1;
                l += 1;
            }

            ans += l as i32;
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
