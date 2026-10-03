---
comments: true
difficulty: Medium
rating: 1952
source: Weekly Contest 225 Q2
tags:
    - Hash Table
    - String
    - Counting
    - Prefix Sum
---

<!-- problem:start -->

# [1737. Change Minimum Characters to Satisfy One of Three Conditions](https://leetcode.com/problems/change-minimum-characters-to-satisfy-one-of-three-conditions)

[中文文档](/solution/1700-1799/1737.Change%20Minimum%20Characters%20to%20Satisfy%20One%20of%20Three%20Conditions/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai chuỗi <code>a</code> và <code>b</code> chỉ gồm các chữ cái thường. Trong một thao tác, bạn có thể đổi bất kỳ ký tự nào trong <code>a</code> hoặc <code>b</code> thành <strong>bất kỳ chữ cái thường nào</strong>.</p>

<p>Mục tiêu là thỏa mãn <strong>một</strong> trong ba điều kiện sau:</p>

<ul>
	<li><strong>Mọi</strong> chữ cái trong <code>a</code> đều <strong>nhỏ hơn nghiêm ngặt</strong> <strong>mọi</strong> chữ cái trong <code>b</code> theo thứ tự bảng chữ cái.</li>
	<li><strong>Mọi</strong> chữ cái trong <code>b</code> đều <strong>nhỏ hơn nghiêm ngặt</strong> <strong>mọi</strong> chữ cái trong <code>a</code> theo thứ tự bảng chữ cái.</li>
	<li><strong>Cả</strong> <code>a</code> và <code>b</code> đều chỉ gồm <strong>một</strong> chữ cái phân biệt.</li>
</ul>

<p>Trả về <em><strong>số thao tác ít nhất</strong> cần thực hiện để đạt mục tiêu.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> a = &quot;aba&quot;, b = &quot;caa&quot;
<strong>Output:</strong> 2
<strong>Giải thích:</strong> Xét cách tốt nhất để thỏa mãn từng điều kiện:
1) Đổi b thành &quot;ccc&quot; trong 2 thao tác, khi đó mọi chữ cái trong a đều nhỏ hơn mọi chữ cái trong b.
2) Đổi a thành &quot;bbb&quot; và b thành &quot;aaa&quot; trong 3 thao tác, khi đó mọi chữ cái trong b đều nhỏ hơn mọi chữ cái trong a.
3) Đổi a và b thành &quot;aaa&quot; trong 2 thao tác, khi đó a và b chỉ gồm một chữ cái phân biệt.
Cách tốt nhất cần 2 thao tác (điều kiện 1 hoặc điều kiện 3).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> a = &quot;dabadd&quot;, b = &quot;cda&quot;
<strong>Output:</strong> 3
<strong>Giải thích:</strong> Cách tốt nhất là thỏa mãn điều kiện 1 bằng cách đổi b thành &quot;eee&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= a.length, b.length &lt;= 10<sup>5</sup></code></li>
	<li><code>a</code> và <code>b</code> chỉ gồm các chữ cái thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Có ba mục tiêu: mọi chữ cái của $a$ nhỏ hơn nghiêm ngặt mọi chữ cái của $b$, điều ngược lại, hoặc cả hai chuỗi chỉ gồm một chữ cái. Bảng chữ cái có $26$ chữ cái, nên ta có thể liệt kê vị trí chia hoặc chữ cái đích.
>
> Đếm tần suất. Với mục tiêu thứ ba, thử từng chữ cái $c$ và đếm các chữ cái khác $c$. Với hai mục tiêu đầu, thử vị trí chia $c$, rồi đổi một chuỗi thành các chữ cái nhỏ hơn $c$ và chuỗi kia thành các chữ cái không nhỏ hơn $c$.
>
> Đáp án là giá trị nhỏ nhất trong ba trường hợp.

<!-- thinking:end -->

Trước hết, ta đếm số lần xuất hiện của từng chữ cái trong hai chuỗi $a$ và $b$, lần lượt ký hiệu là $cnt_1$ và $cnt_2$.

Tiếp theo, xét điều kiện $3$, tức mọi chữ cái trong $a$ và $b$ đều giống nhau. Ta chỉ cần liệt kê chữ cái cuối cùng $c$, rồi đếm số chữ cái trong $a$ và $b$ khác $c$. Đây là số ký tự cần thay đổi.

Sau đó, xét điều kiện $1$ và $2$, tức mọi chữ cái trong $a$ nhỏ hơn mọi chữ cái trong $b$, hoặc mọi chữ cái trong $b$ nhỏ hơn mọi chữ cái trong $a$. Với điều kiện $1$, ta đổi tất cả ký tự trong chuỗi $a$ thành các chữ cái nhỏ hơn $c$, còn các ký tự trong chuỗi $b$ thành các chữ cái không nhỏ hơn $c$. Liệt kê $c$ để tìm đáp án nhỏ nhất. Điều kiện $2$ tương tự.

Đáp án cuối cùng là giá trị nhỏ nhất trong ba trường hợp trên.

Độ phức tạp thời gian là $O(m + n + C^2)$, trong đó $m$ và $n$ lần lượt là độ dài của $a$ và $b$, còn $C$ là kích thước tập ký tự. Trong bài này, $C = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minCharacters(self, a: str, b: str) -> int:
        def f(cnt1, cnt2):
            for i in range(1, 26):
                t = sum(cnt1[i:]) + sum(cnt2[:i])
                nonlocal ans
                ans = min(ans, t)

        m, n = len(a), len(b)
        cnt1 = [0] * 26
        cnt2 = [0] * 26
        for c in a:
            cnt1[ord(c) - ord('a')] += 1
        for c in b:
            cnt2[ord(c) - ord('a')] += 1
        ans = m + n
        for c1, c2 in zip(cnt1, cnt2):
            ans = min(ans, m + n - c1 - c2)
        f(cnt1, cnt2)
        f(cnt2, cnt1)
        return ans
```

#### Java

```java
class Solution {
    private int ans;

    public int minCharacters(String a, String b) {
        int m = a.length(), n = b.length();
        int[] cnt1 = new int[26];
        int[] cnt2 = new int[26];
        for (int i = 0; i < m; ++i) {
            ++cnt1[a.charAt(i) - 'a'];
        }
        for (int i = 0; i < n; ++i) {
            ++cnt2[b.charAt(i) - 'a'];
        }
        ans = m + n;
        for (int i = 0; i < 26; ++i) {
            ans = Math.min(ans, m + n - cnt1[i] - cnt2[i]);
        }
        f(cnt1, cnt2);
        f(cnt2, cnt1);
        return ans;
    }

    private void f(int[] cnt1, int[] cnt2) {
        for (int i = 1; i < 26; ++i) {
            int t = 0;
            for (int j = i; j < 26; ++j) {
                t += cnt1[j];
            }
            for (int j = 0; j < i; ++j) {
                t += cnt2[j];
            }
            ans = Math.min(ans, t);
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minCharacters(string a, string b) {
        int m = a.size(), n = b.size();
        vector<int> cnt1(26);
        vector<int> cnt2(26);
        for (char& c : a) ++cnt1[c - 'a'];
        for (char& c : b) ++cnt2[c - 'a'];
        int ans = m + n;
        for (int i = 0; i < 26; ++i) ans = min(ans, m + n - cnt1[i] - cnt2[i]);
        auto f = [&](vector<int>& cnt1, vector<int>& cnt2) {
            for (int i = 1; i < 26; ++i) {
                int t = 0;
                for (int j = i; j < 26; ++j) t += cnt1[j];
                for (int j = 0; j < i; ++j) t += cnt2[j];
                ans = min(ans, t);
            }
        };
        f(cnt1, cnt2);
        f(cnt2, cnt1);
        return ans;
    }
};
```

#### Go

```go
func minCharacters(a string, b string) int {
	cnt1 := [26]int{}
	cnt2 := [26]int{}
	for _, c := range a {
		cnt1[c-'a']++
	}
	for _, c := range b {
		cnt2[c-'a']++
	}
	m, n := len(a), len(b)
	ans := m + n
	for i := 0; i < 26; i++ {
		ans = min(ans, m+n-cnt1[i]-cnt2[i])
	}
	f := func(cnt1, cnt2 [26]int) {
		for i := 1; i < 26; i++ {
			t := 0
			for j := i; j < 26; j++ {
				t += cnt1[j]
			}
			for j := 0; j < i; j++ {
				t += cnt2[j]
			}
			ans = min(ans, t)
		}
	}
	f(cnt1, cnt2)
	f(cnt2, cnt1)
	return ans
}
```

#### TypeScript

```ts
function minCharacters(a: string, b: string): number {
    const m = a.length,
        n = b.length;
    let count1 = new Array(26).fill(0);
    let count2 = new Array(26).fill(0);
    const base = 'a'.charCodeAt(0);

    for (let char of a) {
        count1[char.charCodeAt(0) - base]++;
    }
    for (let char of b) {
        count2[char.charCodeAt(0) - base]++;
    }

    let pre1 = 0,
        pre2 = 0;
    let ans = m + n;
    for (let i = 0; i < 25; i++) {
        pre1 += count1[i];
        pre2 += count2[i];
        // case1， case2， case3
        ans = Math.min(ans, m - pre1 + pre2, pre1 + n - pre2, m + n - count1[i] - count2[i]);
    }
    ans = Math.min(ans, m + n - count1[25] - count2[25]);

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
