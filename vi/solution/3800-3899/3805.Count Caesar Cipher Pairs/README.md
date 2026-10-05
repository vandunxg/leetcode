---
comments: true
difficulty: Medium
rating: 1624
source: Weekly Contest 484 Q3
tags:
    - Array
    - Hash Table
    - Math
    - String
    - Counting
---

<!-- problem:start -->

# [3805. Count Caesar Cipher Pairs](https://leetcode.com/problems/count-caesar-cipher-pairs)

[中文文档](/solution/3800-3899/3805.Count%20Caesar%20Cipher%20Pairs/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <code>words</code> gồm <code>n</code> chuỗi. Mỗi chuỗi có độ dài <code>m</code> và chỉ chứa các chữ cái tiếng Anh viết thường.</p>

<p>Hai chuỗi <code>s</code> và <code>t</code> được gọi là <strong>tương tự</strong> nếu ta có thể thực hiện thao tác sau một số lần bất kỳ (có thể bằng không) để <code>s</code> và <code>t</code> trở nên <strong>bằng nhau</strong>.</p>

<ul>
	<li>Chọn <code>s</code> hoặc <code>t</code>.</li>
	<li>Thay thế <strong>mọi</strong> chữ cái trong chuỗi đã chọn bằng chữ cái tiếp theo trong bảng chữ cái theo cách tuần hoàn. Chữ cái tiếp theo sau <code>&#39;z&#39;</code> là <code>&#39;a&#39;</code>.</li>
</ul>

<p>Hãy đếm số cặp chỉ số <code>(i, j)</code> thỏa mãn:</p>

<ul>
	<li><code>i &lt; j</code></li>
	<li><code>words[i]</code> và <code>words[j]</code> là <strong>tương tự</strong>.</li>
</ul>

<p>Trả về một số nguyên biểu thị số lượng cặp như vậy.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">words = [&quot;fusion&quot;,&quot;layout&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>words[0] = &quot;fusion&quot;</code> và <code>words[1] = &quot;layout&quot;</code> là hai chuỗi tương tự vì ta có thể thực hiện thao tác trên <code>&quot;fusion&quot;</code> 6 lần. Chuỗi <code>&quot;fusion&quot;</code> thay đổi như sau.</p>

<ul>
	<li><code>&quot;fusion&quot;</code></li>
	<li><code>&quot;gvtjpo&quot;</code></li>
	<li><code>&quot;hwukqp&quot;</code></li>
	<li><code>&quot;ixvlrq&quot;</code></li>
	<li><code>&quot;jywmsr&quot;</code></li>
	<li><code>&quot;kzxnts&quot;</code></li>
	<li><code>&quot;layout&quot;</code></li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">words = [&quot;ab&quot;,&quot;aa&quot;,&quot;za&quot;,&quot;aa&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>words[0] = &quot;ab&quot;</code> và <code>words[2] = &quot;za&quot;</code> là hai chuỗi tương tự. <code>words[1] = &quot;aa&quot;</code> và <code>words[3] = &quot;aa&quot;</code> là hai chuỗi tương tự.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == words.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= m == words[i].length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= n * m &lt;= 10<sup>5</sup></code></li>
	<li><code>words[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Biến đổi chuỗi + Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Hai chuỗi tương tự nếu một phép dịch Caesar tuần hoàn có thể biến chúng thành bằng nhau. Điều kiện $n \cdot m \le 10^5$ khiến việc kiểm tra từng cặp bằng phép dịch là không khả thi.
>
> Các chuỗi trong cùng một lớp chỉ khác nhau bởi một offset chung. Dịch mỗi chuỗi sao cho ký tự đầu tiên trở thành $\texttt{z}$ sẽ đưa cả lớp về cùng một chuỗi chuẩn.
>
> Ta chuẩn hóa mỗi chuỗi đúng một lần rồi đếm; mỗi lớp có v phần tử đóng góp $\binom{v}{2}$ cặp.
>
> Chỉ cần dùng một hash map với dạng chuẩn làm key, sau đó tính tổng các giá trị nhị thức nói trên.

<!-- thinking:end -->

Ta có thể biến đổi mỗi chuỗi về một dạng thống nhất. Cụ thể, ta chuyển ký tự đầu tiên của chuỗi thành `'z'`, sau đó biến đổi các ký tự còn lại trong chuỗi với cùng một offset. Nhờ đó, mọi chuỗi tương tự sẽ được chuyển thành cùng một dạng. Ta dùng một hash table $\textit{cnt}$ để ghi nhận số lần xuất hiện của mỗi chuỗi sau biến đổi.

Cuối cùng, ta duyệt qua hash table, tính số tổ hợp $\frac{v(v-1)}{2}$ với số lần xuất hiện $v$ của mỗi chuỗi, rồi cộng vào đáp án.

Độ phức tạp thời gian là $O(n \times m)$ và độ phức tạp không gian là $O(n \times m)$, trong đó $n$ là độ dài của mảng chuỗi và $m$ là độ dài của các chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countPairs(self, words: List[str]) -> int:
        cnt = defaultdict(int)
        for s in words:
            t = list(s)
            k = ord("z") - ord(t[0])
            for i in range(1, len(t)):
                t[i] = chr((ord(t[i]) - ord("a") + k) % 26 + ord("a"))
            t[0] = "z"
            cnt["".join(t)] += 1
        return sum(v * (v - 1) // 2 for v in cnt.values())
```

#### Java

```java
class Solution {
    public long countPairs(String[] words) {
        Map<String, Integer> cnt = new HashMap<>();
        long ans = 0;
        for (String s : words) {
            char[] t = s.toCharArray();
            int k = 'z' - t[0];
            for (int i = 1; i < t.length; i++) {
                t[i] = (char)('a' + (t[i] - 'a' + k) % 26);
            }
            t[0] = 'z';
            cnt.merge(new String(t), 1, Integer::sum);
        }
        for (int v : cnt.values()) {
            ans += 1L * v * (v - 1) / 2;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long countPairs(vector<string>& words) {
        unordered_map<string, int> cnt;
        long long ans = 0;
        for (auto& s : words) {
            string t = s;
            int k = 'z' - t[0];
            for (int i = 1; i < t.size(); i++) {
                t[i] = 'a' + (t[i] - 'a' + k) % 26;
            }
            t[0] = 'z';
            cnt[t]++;
        }
        for (auto& [key, v] : cnt) {
            ans += 1LL * v * (v - 1) / 2;
        }
        return ans;
    }
};
```

#### Go

```go
func countPairs(words []string) int64 {
	cnt := make(map[string]int)
	var ans int64 = 0
	for _, s := range words {
		t := []rune(s)
		k := 'z' - t[0]
		for i := 1; i < len(t); i++ {
			t[i] = 'a' + (t[i]-'a'+k)%26
		}
		t[0] = 'z'
		key := string(t)
		cnt[key]++
	}
	for _, v := range cnt {
		ans += int64(v) * int64(v-1) / 2
	}
	return ans
}
```

#### TypeScript

```ts
function countPairs(words: string[]): number {
    const cnt = new Map<string, number>();
    let ans = 0;
    for (const s of words) {
        const t = s.split('');
        const k = 'z'.charCodeAt(0) - t[0].charCodeAt(0);
        for (let i = 1; i < t.length; i++) {
            t[i] = String.fromCharCode(97 + ((t[i].charCodeAt(0) - 97 + k) % 26));
        }
        t[0] = 'z';
        const key = t.join('');
        cnt.set(key, (cnt.get(key) || 0) + 1);
    }
    for (const v of cnt.values()) {
        ans += (v * (v - 1)) / 2;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
