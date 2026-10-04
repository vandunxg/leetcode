---
comments: true
difficulty: Medium
rating: 1491
source: Weekly Contest 396 Q2
tags:
    - Hash Table
    - String
    - Counting
---

<!-- problem:start -->

# [3137. Minimum Number of Operations to Make Word K-Periodic](https://leetcode.com/problems/minimum-number-of-operations-to-make-word-k-periodic)

[Tài liệu tiếng Trung](/solution/3100-3199/3137.Minimum%20Number%20of%20Operations%20to%20Make%20Word%20K-Periodic/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>word</code> có độ dài <code>n</code> và một số nguyên <code>k</code> sao cho <code>k</code> là ước của <code>n</code>.</p>

<p>Trong một thao tác, bạn có thể chọn hai chỉ số <code>i</code> và <code>j</code> đều chia hết cho <code>k</code>, sau đó thay thế <span data-keyword="substring">substring</span> có độ dài <code>k</code> bắt đầu tại <code>i</code> bằng substring có độ dài <code>k</code> bắt đầu tại <code>j</code>. Cụ thể, thay thế substring <code>word[i..i + k - 1]</code> bằng substring <code>word[j..j + k - 1]</code>.<!-- notionvc: 49ac84f7-0724-452a-ab43-0c5e53f1db33 --></p>

<p>Trả về <em><strong>số thao tác nhỏ nhất</strong> cần thực hiện để biến</em> <code>word</code> <em>thành chuỗi <strong>k-tuần hoàn</strong></em>.</p>

<p>Ta gọi <code>word</code> là <strong>k-tuần hoàn</strong> nếu tồn tại một chuỗi <code>s</code> có độ dài <code>k</code> sao cho <code>word</code> có thể thu được bằng cách nối <code>s</code> một số lần tùy ý. Ví dụ, nếu <code>word == &ldquo;ababab&rdquo;</code> thì <code>word</code> là chuỗi 2-tuần hoàn với <code>s = &quot;ab&quot;</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
">word = &quot;leetcodeleet&quot;, k = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io" style="
font-family: Menlo,sans-serif;
font-size: 0.85rem;
">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta có thể thu được một chuỗi 4-tuần hoàn bằng cách chọn i = 4 và j = 0. Sau thao tác này, word trở thành &quot;leetleetleet&quot;.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
">word = &quot;</span>leetcoleet<span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
">&quot;, k = 2</span></p>

<p><strong>Đầu ra:</strong> 3</p>

<p><strong>Giải thích:</strong></p>

<p>Ta có thể thu được một chuỗi 2-tuần hoàn bằng cách thực hiện các thao tác trong bảng dưới đây.</p>

<table border="1" bordercolor="#ccc" cellpadding="5" cellspacing="0" height="146" style="border-collapse:collapse; text-align: center; vertical-align: middle;">
	<tbody>
		<tr>
			<th>i</th>
			<th>j</th>
			<th>word</th>
		</tr>
		<tr>
			<td style="padding: 5px 15px;">0</td>
			<td style="padding: 5px 15px;">2</td>
			<td style="padding: 5px 15px;">etetcoleet</td>
		</tr>
		<tr>
			<td style="padding: 5px 15px;">4</td>
			<td style="padding: 5px 15px;">0</td>
			<td style="padding: 5px 15px;">etetetleet</td>
		</tr>
		<tr>
			<td style="padding: 5px 15px;">6</td>
			<td style="padding: 5px 15px;">0</td>
			<td style="padding: 5px 15px;">etetetetet</td>
		</tr>
	</tbody>
</table>
</div>

<div id="gtx-trans" style="position: absolute; left: 107px; top: 238.5px;">
<div class="gtx-trans-icon">&nbsp;</div>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == word.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k &lt;= word.length</code></li>
	<li><code>k</code> là ước của <code>word.length</code>.</li>
	<li><code>word</code> chỉ bao gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác thay thế một khối có độ dài-$k$ bằng một khối khác. Chuỗi cần trở thành một chuỗi lặp lại cùng một khối. Việc thử mọi khối đích có độ phức tạp tỷ lệ với số khối.
>
> Số thao tác bằng số khối trừ đi tần suất của khối xuất hiện nhiều nhất. Chỉ cần đếm, không cần xây dựng lại chuỗi.
>
> Chia $word$ với bước $k$, lấy số lần xuất hiện lớn nhất, rồi trả về $n/k$ trừ đi giá trị lớn nhất đó.

<!-- thinking:end -->

Ta có thể chia chuỗi `word` thành các chuỗi con có độ dài $k$, sau đó đếm số lần xuất hiện của mỗi chuỗi con và cuối cùng trả về $n/k$ trừ đi số lần xuất hiện của chuỗi con xuất hiện nhiều nhất.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của chuỗi `word`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumOperationsToMakeKPeriodic(self, word: str, k: int) -> int:
        n = len(word)
        return n // k - max(Counter(word[i : i + k] for i in range(0, n, k)).values())
```

#### Java

```java
class Solution {
    public int minimumOperationsToMakeKPeriodic(String word, int k) {
        Map<String, Integer> cnt = new HashMap<>();
        int n = word.length();
        int mx = 0;
        for (int i = 0; i < n; i += k) {
            mx = Math.max(mx, cnt.merge(word.substring(i, i + k), 1, Integer::sum));
        }
        return n / k - mx;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumOperationsToMakeKPeriodic(string word, int k) {
        unordered_map<string, int> cnt;
        int n = word.size();
        int mx = 0;
        for (int i = 0; i < n; i += k) {
            mx = max(mx, ++cnt[word.substr(i, k)]);
        }
        return n / k - mx;
    }
};
```

#### Go

```go
func minimumOperationsToMakeKPeriodic(word string, k int) int {
	cnt := map[string]int{}
	n := len(word)
	mx := 0
	for i := 0; i < n; i += k {
		s := word[i : i+k]
		cnt[s]++
		mx = max(mx, cnt[s])
	}
	return n/k - mx
}
```

#### TypeScript

```ts
function minimumOperationsToMakeKPeriodic(word: string, k: number): number {
    const cnt: Map<string, number> = new Map();
    const n: number = word.length;
    let mx: number = 0;
    for (let i = 0; i < n; i += k) {
        const s = word.slice(i, i + k);
        cnt.set(s, (cnt.get(s) || 0) + 1);
        mx = Math.max(mx, cnt.get(s)!);
    }
    return Math.floor(n / k) - mx;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
