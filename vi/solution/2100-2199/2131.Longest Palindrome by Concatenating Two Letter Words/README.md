---
comments: true
difficulty: Medium
rating: 1556
source: Biweekly Contest 69 Q3
tags:
    - Greedy
    - Array
    - Hash Table
    - String
    - Counting
---

<!-- problem:start -->

# [2131. Longest Palindrome by Concatenating Two Letter Words](https://leetcode.com/problems/longest-palindrome-by-concatenating-two-letter-words)

[中文文档](/solution/2100-2199/2131.Longest%20Palindrome%20by%20Concatenating%20Two%20Letter%20Words/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng chuỗi <code>words</code>. Mỗi phần tử của <code>words</code> gồm <strong>hai</strong> chữ cái tiếng Anh viết thường.</p>

<p>Hãy tạo một <strong>palindrome dài nhất có thể</strong> bằng cách chọn một số phần tử trong <code>words</code> và nối chúng theo <strong>bất kỳ thứ tự nào</strong>. Mỗi phần tử được chọn <strong>nhiều nhất một lần</strong>.</p>

<p>Trả về <em><strong>độ dài</strong> của palindrome dài nhất có thể tạo ra</em>. Nếu không thể tạo palindrome nào, trả về <code>0</code>.</p>

<p><strong>Palindrome</strong> là một chuỗi đọc xuôi hay đọc ngược đều giống nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;lc&quot;,&quot;cl&quot;,&quot;gg&quot;]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Một palindrome dài nhất là &quot;lc&quot; + &quot;gg&quot; + &quot;cl&quot; = &quot;lcggcl&quot;, có độ dài 6.
Lưu ý rằng &quot;clgglc&quot; cũng là một palindrome dài nhất có thể tạo ra.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;ab&quot;,&quot;ty&quot;,&quot;yt&quot;,&quot;lc&quot;,&quot;cl&quot;,&quot;ab&quot;]
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong> Một palindrome dài nhất là &quot;ty&quot; + &quot;lc&quot; + &quot;cl&quot; + &quot;yt&quot; = &quot;tylcclyt&quot;, có độ dài 8.
Lưu ý rằng &quot;lcyttycl&quot; cũng là một palindrome dài nhất có thể tạo ra.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;cc&quot;,&quot;ll&quot;,&quot;xx&quot;]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Một palindrome dài nhất là &quot;cc&quot;, có độ dài 2.
Lưu ý rằng &quot;ll&quot; cũng là một palindrome khác có thể tạo ra, và &quot;xx&quot; cũng vậy.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 10<sup>5</sup></code></li>
	<li><code>words[i].length == 2</code></li>
	<li><code>words[i]</code> gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Một palindrome được tạo bằng cách ghép một từ với từ đảo ngược của nó, đồng thời có nhiều nhất một từ dạng `aa` ở chính giữa. Việc tìm kiếm thứ tự nối là quá lớn; ta chỉ cần quan tâm đến tần suất xuất hiện và các từ đảo ngược.
>
> Sau khi đếm, `ab` ghép với `ba` được $\min$ lần, mỗi cặp đóng góp $4$ ký tự; các từ có hai chữ cái giống nhau trước tiên đóng góp theo số lượng chẵn, và một bản sao lẻ còn lại có thể nằm ở chính giữa.
>
> Ta cộng độ dài các cặp từ từ counter rồi cộng thêm $2$ nếu còn từ đối xứng. Các từ đối nhau được tính bằng $\min(v,\textit{cnt}[k[::-1]])$ ở cả hai phía, cho tổng cộng $2\min\times 2$.

<!-- thinking:end -->

Trước tiên, ta dùng một hash table $\textit{cnt}$ để đếm số lần xuất hiện của mỗi từ.

Duyệt qua từng từ $k$ và số lần xuất hiện $v$ tương ứng trong $\textit{cnt}$:

- Nếu hai chữ cái trong $k$ giống nhau, ta có thể nối $\left \lfloor \frac{v}{2} \right \rfloor \times 2$ bản sao của $k$ vào đầu và cuối palindrome. Nếu còn lại một $k$, ta tạm thời lưu nó vào $x$.
- Nếu hai chữ cái trong $k$ khác nhau, ta cần tìm một từ $k'$ sao cho hai chữ cái trong $k'$ là đảo ngược của $k$, tức là $k' = k[1] + k[0]$. Nếu tồn tại $k'$, ta có thể nối $\min(v, \textit{cnt}[k'])$ bản sao của $k$ vào đầu và cuối palindrome.

Sau khi duyệt xong, nếu $x$ không rỗng, ta cũng có thể đặt một từ ở chính giữa palindrome.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$, trong đó $n$ là số lượng từ.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestPalindrome(self, words: List[str]) -> int:
        cnt = Counter(words)
        ans = x = 0
        for k, v in cnt.items():
            if k[0] == k[1]:
                x += v & 1
                ans += v // 2 * 2 * 2
            else:
                ans += min(v, cnt[k[::-1]]) * 2
        ans += 2 if x else 0
        return ans
```

#### Java

```java
class Solution {
    public int longestPalindrome(String[] words) {
        Map<String, Integer> cnt = new HashMap<>();
        for (var w : words) {
            cnt.merge(w, 1, Integer::sum);
        }
        int ans = 0, x = 0;
        for (var e : cnt.entrySet()) {
            var k = e.getKey();
            var rk = new StringBuilder(k).reverse().toString();
            int v = e.getValue();
            if (k.charAt(0) == k.charAt(1)) {
                x += v & 1;
                ans += v / 2 * 2 * 2;
            } else {
                ans += Math.min(v, cnt.getOrDefault(rk, 0)) * 2;
            }
        }
        ans += x > 0 ? 2 : 0;
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int longestPalindrome(vector<string>& words) {
        unordered_map<string, int> cnt;
        for (auto& w : words) cnt[w]++;
        int ans = 0, x = 0;
        for (auto& [k, v] : cnt) {
            string rk = k;
            reverse(rk.begin(), rk.end());
            if (k[0] == k[1]) {
                x += v & 1;
                ans += v / 2 * 2 * 2;
            } else if (cnt.count(rk)) {
                ans += min(v, cnt[rk]) * 2;
            }
        }
        ans += x ? 2 : 0;
        return ans;
    }
};
```

#### Go

```go
func longestPalindrome(words []string) int {
	cnt := map[string]int{}
	for _, w := range words {
		cnt[w]++
	}
	ans, x := 0, 0
	for k, v := range cnt {
		if k[0] == k[1] {
			x += v & 1
			ans += v / 2 * 2 * 2
		} else {
			rk := string([]byte{k[1], k[0]})
			if y, ok := cnt[rk]; ok {
				ans += min(v, y) * 2
			}
		}
	}
	if x > 0 {
		ans += 2
	}
	return ans
}
```

#### TypeScript

```ts
function longestPalindrome(words: string[]): number {
    const cnt = new Map<string, number>();
    for (const w of words) cnt.set(w, (cnt.get(w) || 0) + 1);
    let [ans, x] = [0, 0];
    for (const [k, v] of cnt.entries()) {
        if (k[0] === k[1]) {
            x += v & 1;
            ans += Math.floor(v / 2) * 2 * 2;
        } else {
            ans += Math.min(v, cnt.get(k[1] + k[0]) || 0) * 2;
        }
    }
    ans += x ? 2 : 0;
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
