---
comments: true
difficulty: Medium
rating: 1533
source: Weekly Contest 381 Q3
tags:
    - Greedy
    - Hash Table
    - String
    - Counting
    - Sorting
---

<!-- problem:start -->

# [3016. Minimum Number of Pushes to Type Word II](https://leetcode.com/problems/minimum-number-of-pushes-to-type-word-ii)

[中文文档](/solution/3000-3099/3016.Minimum%20Number%20of%20Pushes%20to%20Type%20Word%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>word</code> chứa các chữ cái tiếng Anh viết thường.</p>

<p>Các phím trên bàn phím điện thoại được ánh xạ tới các tập hợp chữ cái tiếng Anh viết thường <strong>không trùng nhau</strong>, có thể dùng để tạo thành các từ bằng cách nhấn phím. Ví dụ, phím <code>2</code> được ánh xạ tới <code>[&quot;a&quot;,&quot;b&quot;,&quot;c&quot;]</code>, ta cần nhấn phím một lần để nhập <code>&quot;a&quot;</code>, hai lần để nhập <code>&quot;b&quot;</code> và ba lần để nhập <code>&quot;c&quot;</code> <em>.</em></p>

<p>Có thể ánh xạ lại các phím từ <code>2</code> đến <code>9</code> tới các tập hợp chữ cái <strong>không trùng nhau</strong>. Các phím có thể được ánh xạ tới số lượng chữ cái <strong>bất kỳ</strong>, nhưng mỗi chữ cái <strong>phải</strong> được ánh xạ tới <strong>chính xác</strong> một phím. Hãy tìm <strong>số lần nhấn</strong> tối thiểu cần dùng để nhập chuỗi <code>word</code>.</p>

<p>Trả về <em><strong>số lần nhấn</strong> tối thiểu cần thiết để nhập </em><code>word</code> <em>sau khi ánh xạ lại các phím</em>.</p>

<p>Dưới đây là một ví dụ về cách ánh xạ chữ cái tới các phím trên bàn phím điện thoại. Lưu ý rằng <code>1</code>, <code>*</code>, <code>#</code> và <code>0</code> <strong>không</strong> được ánh xạ tới chữ cái nào.</p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3016.Minimum%20Number%20of%20Pushes%20to%20Type%20Word%20II/images/keypaddesc.png" style="width: 329px; height: 313px;" />
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3016.Minimum%20Number%20of%20Pushes%20to%20Type%20Word%20II/images/keypadv1e1.png" style="width: 329px; height: 313px;" />
<pre>
<strong>Đầu vào:</strong> word = &quot;abcde&quot;
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Bàn phím được ánh xạ lại như trong hình cho chi phí nhỏ nhất.
&quot;a&quot; -&gt; nhấn phím 2 một lần
&quot;b&quot; -&gt; nhấn phím 3 một lần
&quot;c&quot; -&gt; nhấn phím 4 một lần
&quot;d&quot; -&gt; nhấn phím 5 một lần
&quot;e&quot; -&gt; nhấn phím 6 một lần
Tổng chi phí là 1 + 1 + 1 + 1 + 1 = 5.
Có thể chứng minh rằng không có cách ánh xạ nào khác cho chi phí thấp hơn.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3016.Minimum%20Number%20of%20Pushes%20to%20Type%20Word%20II/images/edited.png" style="width: 329px; height: 313px;" />
<pre>
<strong>Đầu vào:</strong> word = &quot;xyzxyzxyzxyz&quot;
<strong>Đầu ra:</strong> 12
<strong>Giải thích:</strong> Bàn phím được ánh xạ lại như trong hình cho chi phí nhỏ nhất.
&quot;x&quot; -&gt; nhấn phím 2 một lần
&quot;y&quot; -&gt; nhấn phím 3 một lần
&quot;z&quot; -&gt; nhấn phím 4 một lần
Tổng chi phí là 1 * 4 + 1 * 4 + 1 * 4 = 12
Có thể chứng minh rằng không có cách ánh xạ nào khác cho chi phí thấp hơn.
Lưu ý rằng phím 9 không được ánh xạ tới chữ cái nào: không cần ánh xạ chữ cái tới mọi phím, nhưng phải ánh xạ tất cả các chữ cái.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3016.Minimum%20Number%20of%20Pushes%20to%20Type%20Word%20II/images/keypadv2.png" style="width: 329px; height: 313px;" />
<pre>
<strong>Đầu vào:</strong> word = &quot;aabbccddeeffgghhiiiiii&quot;
<strong>Đầu ra:</strong> 24
<strong>Giải thích:</strong> Bàn phím được ánh xạ lại như trong hình cho chi phí nhỏ nhất.
&quot;a&quot; -&gt; nhấn phím 2 một lần
&quot;b&quot; -&gt; nhấn phím 3 một lần
&quot;c&quot; -&gt; nhấn phím 4 một lần
&quot;d&quot; -&gt; nhấn phím 5 một lần
&quot;e&quot; -&gt; nhấn phím 6 một lần
&quot;f&quot; -&gt; nhấn phím 7 một lần
&quot;g&quot; -&gt; nhấn phím 8 một lần
&quot;h&quot; -&gt; nhấn phím 9 hai lần
&quot;i&quot; -&gt; nhấn phím 9 một lần
Tổng chi phí là 1 * 2 + 1 * 2 + 1 * 2 + 1 * 2 + 1 * 2 + 1 * 2 + 1 * 2 + 2 * 2 + 6 * 1 = 24.
Có thể chứng minh rằng không có cách ánh xạ nào khác cho chi phí thấp hơn.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= word.length &lt;= 10<sup>5</sup></code></li>
	<li><code>word</code> chỉ chứa các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thuật toán tham lam + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Khác với phần I, các chữ cái có thể lặp lại và $n \le 10^5$. Mỗi chữ cái chiếm một phím, còn chi phí của nó bằng tần suất nhân với thứ hạng số lần nhấn của phím đó.
>
> Các chữ cái có tần suất cao nên được đặt ở những thứ hạng nhỏ hơn. Sau khi sắp xếp $26$ tần suất theo thứ tự giảm dần, chữ cái ở vị trí $i$ có thứ hạng $\lfloor i/8 \rfloor + 1$.
>
> Đáp án là tổng có trọng số theo công thức trên.

<!-- thinking:end -->

Ta dùng hash table hoặc mảng $cnt$ để đếm số lần xuất hiện của mỗi chữ cái trong chuỗi $word$. Sau đó, ta sắp xếp các chữ cái theo số lần xuất hiện giảm dần, rồi nhóm mỗi $8$ chữ cái vào cùng một nhóm và gán mỗi nhóm cho một trong $8$ phím.

Độ phức tạp thời gian là $O(n + |\Sigma| \times \log |\Sigma|)$, và độ phức tạp không gian là $O(|\Sigma|)$. Trong đó, $n$ là độ dài của chuỗi $word$, còn $\Sigma$ là tập hợp các chữ cái xuất hiện trong chuỗi $word$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumPushes(self, word: str) -> int:
        cnt = Counter(word)
        ans = 0
        for i, x in enumerate(sorted(cnt.values(), reverse=True)):
            ans += (i // 8 + 1) * x
        return ans
```

#### Java

```java
class Solution {
    public int minimumPushes(String word) {
        int[] cnt = new int[26];
        for (int i = 0; i < word.length(); ++i) {
            ++cnt[word.charAt(i) - 'a'];
        }
        Arrays.sort(cnt);
        int ans = 0;
        for (int i = 0; i < 26; ++i) {
            ans += (i / 8 + 1) * cnt[26 - i - 1];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumPushes(string word) {
        vector<int> cnt(26);
        for (char& c : word) {
            ++cnt[c - 'a'];
        }
        sort(cnt.rbegin(), cnt.rend());
        int ans = 0;
        for (int i = 0; i < 26; ++i) {
            ans += (i / 8 + 1) * cnt[i];
        }
        return ans;
    }
};
```

#### Go

```go
func minimumPushes(word string) (ans int) {
	cnt := make([]int, 26)
	for _, c := range word {
		cnt[c-'a']++
	}
	sort.Ints(cnt)
	for i := 0; i < 26; i++ {
		ans += (i/8 + 1) * cnt[26-i-1]
	}
	return
}
```

#### TypeScript

```ts
function minimumPushes(word: string): number {
    const cnt: number[] = Array(26).fill(0);
    for (const c of word) {
        ++cnt[c.charCodeAt(0) - 'a'.charCodeAt(0)];
    }
    cnt.sort((a, b) => b - a);
    let ans = 0;
    for (let i = 0; i < 26; ++i) {
        ans += (((i / 8) | 0) + 1) * cnt[i];
    }
    return ans;
}
```

#### JavaScript

```js
function minimumPushes(word) {
    const cnt = Array(26).fill(0);
    for (const c of word) {
        ++cnt[c.charCodeAt(0) - 'a'.charCodeAt(0)];
    }
    cnt.sort((a, b) => b - a);
    let ans = 0;
    for (let i = 0; i < 26; ++i) {
        ans += (((i / 8) | 0) + 1) * cnt[i];
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
