---
comments: true
difficulty: Easy
rating: 1199
source: Biweekly Contest 7 Q1
tags:
    - Hash Table
    - String
---

<!-- problem:start -->

# [1165. Single-Row Keyboard 🔒](https://leetcode.com/problems/single-row-keyboard)

[中文文档](/solution/1100-1199/1165.Single-Row%20Keyboard/README.md)

## Mô tả

<!-- description:start -->

<p>Có một bàn phím đặc biệt với <strong>tất cả phím nằm trên cùng một hàng</strong>.</p>

<p>Cho chuỗi <code>keyboard</code> có độ dài <code>26</code> biểu thị cách sắp xếp các phím trên bàn phím (đánh chỉ số từ <code>0</code> đến <code>25</code>). Ban đầu, ngón tay của bạn ở chỉ số <code>0</code>. Để gõ một ký tự, bạn cần di chuyển ngón tay đến chỉ số của ký tự đó. Thời gian di chuyển ngón tay từ chỉ số <code>i</code> đến chỉ số <code>j</code> là <code>|i - j|</code>.</p>

<p>Bạn muốn gõ chuỗi <code>word</code>. Hãy viết hàm tính thời gian cần thiết để gõ chuỗi bằng một ngón tay.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> keyboard = &quot;abcdefghijklmnopqrstuvwxyz&quot;, word = &quot;cba&quot;
<strong>Đầu ra:</strong> 4
<strong>Giải thích: </strong>Chỉ số di chuyển từ 0 đến 2 để gõ &#39;c&#39;, sau đó đến 1 để gõ &#39;b&#39;, rồi quay lại 0 để gõ &#39;a&#39;.
Tổng thời gian = 2 + 1 + 1 = 4. 
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> keyboard = &quot;pqrstuvwxyzabcdefghijklmno&quot;, word = &quot;leetcode&quot;
<strong>Đầu ra:</strong> 73
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>keyboard.length == 26</code></li>
	<li><code>keyboard</code> chứa mỗi chữ cái tiếng Anh viết thường đúng một lần, theo thứ tự bất kỳ.</li>
	<li><code>1 &lt;= word.length &lt;= 10<sup>4</sup></code></li>
	<li><code>word[i]</code> là một chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table hoặc mảng

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lần di chuyển tốn chi phí bằng độ chênh lệch tuyệt đối giữa hai chỉ số trên bàn phím. Ánh xạ các ký tự sang vị trí tương ứng, bắt đầu ở chỉ số $0$, cộng $|\textit{pos}[c]-i|$ vào tổng rồi di chuyển ngón tay. Không cần quét lại chuỗi keyboard cho từng ký tự.

<!-- thinking:end -->

Ta có thể dùng hash table hoặc mảng $pos$ độ dài $26$ để lưu vị trí của từng ký tự trên bàn phím, trong đó $pos[c]$ là vị trí của ký tự $c$.

Sau đó, ta duyệt chuỗi $word$ và dùng biến $i$ để ghi lại vị trí hiện tại của ngón tay, ban đầu $i = 0$. Với mỗi ký tự $c$, ta tìm vị trí $j$ của nó trên bàn phím, cộng $|i - j|$ vào đáp án rồi cập nhật $i = j$. Tiếp tục với các ký tự còn lại cho đến hết chuỗi $word$.

Sau khi duyệt hết chuỗi $word$, ta thu được đáp án.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(C)$. Trong đó, $n$ là độ dài chuỗi $word$, còn $C$ là kích thước của tập ký tự. Ở bài này, $C = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def calculateTime(self, keyboard: str, word: str) -> int:
        pos = {c: i for i, c in enumerate(keyboard)}
        ans = i = 0
        for c in word:
            ans += abs(pos[c] - i)
            i = pos[c]
        return ans
```

#### Java

```java
class Solution {
    public int calculateTime(String keyboard, String word) {
        int[] pos = new int[26];
        for (int i = 0; i < 26; ++i) {
            pos[keyboard.charAt(i) - 'a'] = i;
        }
        int ans = 0, i = 0;
        for (int k = 0; k < word.length(); ++k) {
            int j = pos[word.charAt(k) - 'a'];
            ans += Math.abs(i - j);
            i = j;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int calculateTime(string keyboard, string word) {
        int pos[26];
        for (int i = 0; i < 26; ++i) {
            pos[keyboard[i] - 'a'] = i;
        }
        int ans = 0, i = 0;
        for (char& c : word) {
            int j = pos[c - 'a'];
            ans += abs(i - j);
            i = j;
        }
        return ans;
    }
};
```

#### Go

```go
func calculateTime(keyboard string, word string) (ans int) {
	pos := [26]int{}
	for i, c := range keyboard {
		pos[c-'a'] = i
	}
	i := 0
	for _, c := range word {
		j := pos[c-'a']
		ans += abs(i - j)
		i = j
	}
	return
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function calculateTime(keyboard: string, word: string): number {
    const pos: number[] = Array(26).fill(0);
    for (let i = 0; i < 26; ++i) {
        pos[keyboard.charCodeAt(i) - 97] = i;
    }
    let ans = 0;
    let i = 0;
    for (const c of word) {
        const j = pos[c.charCodeAt(0) - 97];
        ans += Math.abs(i - j);
        i = j;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
