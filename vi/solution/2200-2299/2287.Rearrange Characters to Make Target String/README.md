---
comments: true
difficulty: Easy
rating: 1299
source: Weekly Contest 295 Q1
tags:
    - Hash Table
    - String
    - Counting
---

<!-- problem:start -->

# [2287. Rearrange Characters to Make Target String](https://leetcode.com/problems/rearrange-characters-to-make-target-string)

[中文文档](/solution/2200-2299/2287.Rearrange%20Characters%20to%20Make%20Target%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <strong>0-indexed</strong> là <code>s</code> và <code>target</code>. Bạn có thể lấy một số ký tự từ <code>s</code> rồi sắp xếp lại chúng để tạo thành các chuỗi mới.</p>

<p>Hãy trả về <em>số bản sao <strong>lớn nhất</strong> của </em><code>target</code><em> có thể tạo ra bằng cách lấy các ký tự từ </em><code>s</code><em> và sắp xếp lại chúng.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;ilovecodingonleetcode&quot;, target = &quot;code&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Để tạo bản sao đầu tiên của &quot;code&quot;, lấy các ký tự ở các chỉ số 4, 5, 6 và 7.
Để tạo bản sao thứ hai của &quot;code&quot;, lấy các ký tự ở các chỉ số 17, 18, 19 và 20.
Các chuỗi được tạo thành là &quot;ecod&quot; và &quot;code&quot;, cả hai đều có thể được sắp xếp lại thành &quot;code&quot;.
Ta có thể tạo nhiều nhất hai bản sao của &quot;code&quot;, nên trả về 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abcba&quot;, target = &quot;abc&quot;
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>
Ta có thể tạo một bản sao của &quot;abc&quot; bằng cách lấy các ký tự ở các chỉ số 0, 1 và 2.
Ta có thể tạo nhiều nhất một bản sao của &quot;abc&quot;, nên trả về 1.
Lưu ý rằng dù còn thừa một ký tự &#39;a&#39; và một ký tự &#39;b&#39; ở các chỉ số 3 và 4, ta không thể dùng lại ký tự &#39;c&#39; ở chỉ số 2, nên không thể tạo bản sao thứ hai của &quot;abc&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abbaccaddaeea&quot;, target = &quot;aaaaa&quot;
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>
Ta có thể tạo một bản sao của &quot;aaaaa&quot; bằng cách lấy các ký tự ở các chỉ số 0, 3, 6, 9 và 12.
Ta có thể tạo nhiều nhất một bản sao của &quot;aaaaa&quot;, nên trả về 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>1 &lt;= target.length &lt;= 10</code></li>
	<li><code>s</code> và <code>target</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Ghi chú:</strong> Bài này giống với bài <a href="https://leetcode.com/problems/maximum-number-of-balloons/description/" target="_blank">1189: Maximum Number of Balloons.</a></p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Ta tạo các bản sao của $target$ từ các ký tự của $s$ nhiều nhất có thể. Vì $|s|\le 100$, ký tự có tỷ lệ số lượng cung cấp trên nhu cầu nhỏ nhất sẽ quyết định giới hạn.
>
> Đếm số lần xuất hiện trong cả hai chuỗi rồi lấy $\min \lfloor cnt_1[c]/cnt_2[c] \rfloor$ trên các ký tự của $target$.

<!-- thinking:end -->

Ta đếm số lần xuất hiện của mỗi ký tự trong các chuỗi $\textit{s}$ và $\textit{target}$, lần lượt ký hiệu là $\textit{cnt1}$ và $\textit{cnt2}$. Với mỗi ký tự trong $\textit{target}$, ta tính số lần ký tự đó xuất hiện trong $\textit{cnt1}$ chia cho số lần xuất hiện trong $\textit{cnt2}$, rồi lấy giá trị nhỏ nhất.

Độ phức tạp thời gian là $O(n + m)$, còn độ phức tạp không gian là $O(|\Sigma|)$. Trong đó, $n$ và $m$ lần lượt là độ dài của các chuỗi $\textit{s}$ và $\textit{target}$. $|\Sigma|$ là kích thước của tập ký tự, bằng 26 trong bài này.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def rearrangeCharacters(self, s: str, target: str) -> int:
        cnt1 = Counter(s)
        cnt2 = Counter(target)
        return min(cnt1[c] // v for c, v in cnt2.items())
```

#### Java

```java
class Solution {
    public int rearrangeCharacters(String s, String target) {
        int[] cnt1 = new int[26];
        int[] cnt2 = new int[26];
        for (int i = 0; i < s.length(); ++i) {
            ++cnt1[s.charAt(i) - 'a'];
        }
        for (int i = 0; i < target.length(); ++i) {
            ++cnt2[target.charAt(i) - 'a'];
        }
        int ans = 100;
        for (int i = 0; i < 26; ++i) {
            if (cnt2[i] > 0) {
                ans = Math.min(ans, cnt1[i] / cnt2[i]);
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
    int rearrangeCharacters(string s, string target) {
        int cnt1[26]{};
        int cnt2[26]{};
        for (char& c : s) {
            ++cnt1[c - 'a'];
        }
        for (char& c : target) {
            ++cnt2[c - 'a'];
        }
        int ans = 100;
        for (int i = 0; i < 26; ++i) {
            if (cnt2[i]) {
                ans = min(ans, cnt1[i] / cnt2[i]);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func rearrangeCharacters(s string, target string) int {
	var cnt1, cnt2 [26]int
	for _, c := range s {
		cnt1[c-'a']++
	}
	for _, c := range target {
		cnt2[c-'a']++
	}
	ans := 100
	for i, v := range cnt2 {
		if v > 0 {
			ans = min(ans, cnt1[i]/v)
		}
	}
	return ans
}
```

#### TypeScript

```ts
function rearrangeCharacters(s: string, target: string): number {
    const idx = (s: string) => s.charCodeAt(0) - 97;
    const cnt1 = new Array(26).fill(0);
    const cnt2 = new Array(26).fill(0);
    for (const c of s) {
        ++cnt1[idx(c)];
    }
    for (const c of target) {
        ++cnt2[idx(c)];
    }
    let ans = 100;
    for (let i = 0; i < 26; ++i) {
        if (cnt2[i]) {
            ans = Math.min(ans, Math.floor(cnt1[i] / cnt2[i]));
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn rearrange_characters(s: String, target: String) -> i32 {
        let mut count1 = [0; 26];
        let mut count2 = [0; 26];
        for c in s.as_bytes() {
            count1[(c - b'a') as usize] += 1;
        }
        for c in target.as_bytes() {
            count2[(c - b'a') as usize] += 1;
        }
        let mut ans = i32::MAX;
        for i in 0..26 {
            if count2[i] != 0 {
                ans = ans.min(count1[i] / count2[i]);
            }
        }
        ans
    }
}
```

#### C

```c
#define min(a, b) (((a) < (b)) ? (a) : (b))

int rearrangeCharacters(char* s, char* target) {
    int count1[26] = {0};
    int count2[26] = {0};
    for (int i = 0; s[i]; i++) {
        count1[s[i] - 'a']++;
    }
    for (int i = 0; target[i]; i++) {
        count2[target[i] - 'a']++;
    }
    int ans = INT_MAX;
    for (int i = 0; i < 26; i++) {
        if (count2[i]) {
            ans = min(ans, count1[i] / count2[i]);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
