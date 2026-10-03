---
comments: true
difficulty: Easy
rating: 1241
source: Biweekly Contest 83 Q1
tags:
    - Array
    - Hash Table
    - Counting
---

<!-- problem:start -->

# [2347. Best Poker Hand](https://leetcode.com/problems/best-poker-hand)

[中文文档](/solution/2300-2399/2347.Best%20Poker%20Hand/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>ranks</code> và một mảng ký tự <code>suits</code>. Bạn có <code>5</code> lá bài, trong đó lá bài thứ <code>i<sup>th</sup></code> có giá trị là <code>ranks[i]</code> và chất là <code>suits[i]</code>.</p>

<p>Dưới đây là các loại <strong>bộ bài poker</strong> có thể tạo được, theo thứ tự từ mạnh nhất đến yếu nhất:</p>

<ol>
	<li><code>&quot;Flush&quot;</code>: Năm lá bài cùng chất.</li>
	<li><code>&quot;Three of a Kind&quot;</code>: Ba lá bài có cùng giá trị.</li>
	<li><code>&quot;Pair&quot;</code>: Hai lá bài có cùng giá trị.</li>
	<li><code>&quot;High Card&quot;</code>: Bất kỳ một lá bài nào.</li>
</ol>

<p>Trả về <em>một chuỗi biểu thị loại <strong>bộ bài poker</strong> <strong>mạnh nhất</strong> có thể tạo được từ các lá bài đã cho.</em></p>

<p><strong>Lưu ý</strong> rằng các giá trị trả về <strong>phân biệt chữ hoa chữ thường</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> ranks = [13,2,3,1,9], suits = [&quot;a&quot;,&quot;a&quot;,&quot;a&quot;,&quot;a&quot;,&quot;a&quot;]
<strong>Đầu ra:</strong> &quot;Flush&quot;
<strong>Giải thích:</strong> Bộ bài gồm cả 5 lá bài có cùng chất, nên ta có một bộ &quot;Flush&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> ranks = [4,4,2,4,4], suits = [&quot;d&quot;,&quot;a&quot;,&quot;a&quot;,&quot;b&quot;,&quot;c&quot;]
<strong>Đầu ra:</strong> &quot;Three of a Kind&quot;
<strong>Giải thích:</strong> Các lá bài thứ nhất, thứ hai và thứ tư có cùng giá trị, nên ta có một bộ &quot;Three of a Kind&quot;.
Lưu ý rằng ta cũng có thể tạo một bộ &quot;Pair&quot;, nhưng &quot;Three of a Kind&quot; là bộ mạnh hơn.
Cũng lưu ý rằng các lá bài khác cũng có thể được dùng để tạo bộ &quot;Three of a Kind&quot;.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> ranks = [10,10,2,12,9], suits = [&quot;a&quot;,&quot;b&quot;,&quot;c&quot;,&quot;a&quot;,&quot;d&quot;]
<strong>Đầu ra:</strong> &quot;Pair&quot;
<strong>Giải thích:</strong> Lá bài thứ nhất và thứ hai có cùng giá trị, nên ta có một bộ &quot;Pair&quot;.
Lưu ý rằng ta không thể tạo bộ &quot;Flush&quot; hoặc &quot;Three of a Kind&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>ranks.length == suits.length == 5</code></li>
	<li><code>1 &lt;= ranks[i] &lt;= 13</code></li>
	<li><code>&#39;a&#39; &lt;= suits[i] &lt;= &#39;d&#39;</code></li>
	<li>Không có hai lá bài nào có cùng giá trị và chất.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Với năm lá bài, các khả năng là Flush, Three of a Kind, Pair hoặc High Card, theo thứ tự đó. Số lá bài rất ít, nên chỉ cần đếm là đủ.
>
> Trước tiên kiểm tra xem các lá bài có cùng chất hay không, sau đó đếm tần suất xuất hiện của các giá trị để kiểm tra có giá trị nào xuất hiện $\ge 3$ hoặc đúng $2$ lần hay không. Nếu không, đó là High Card.

<!-- thinking:end -->

Trước tiên, ta duyệt mảng $\textit{suits}$ để kiểm tra xem các phần tử liền kề có bằng nhau hay không. Nếu có, ta trả về `"Flush"`.

Tiếp theo, ta dùng một hash table hoặc mảng $\textit{cnt}$ để đếm số lần xuất hiện của từng lá bài:

- Nếu có lá bài xuất hiện $3$ lần, trả về `"Three of a Kind"`;
- Nếu không, nếu có lá bài xuất hiện $2$ lần, trả về `"Pair"`;
- Nếu không, trả về `"High Card"`.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $\textit{ranks}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def bestHand(self, ranks: List[int], suits: List[str]) -> str:
        # if len(set(suits)) == 1:
        if all(a == b for a, b in pairwise(suits)):
            return 'Flush'
        cnt = Counter(ranks)
        if any(v >= 3 for v in cnt.values()):
            return 'Three of a Kind'
        if any(v == 2 for v in cnt.values()):
            return 'Pair'
        return 'High Card'
```

#### Java

```java
class Solution {
    public String bestHand(int[] ranks, char[] suits) {
        boolean flush = true;
        for (int i = 1; i < 5 && flush; ++i) {
            flush = suits[i] == suits[i - 1];
        }
        if (flush) {
            return "Flush";
        }
        int[] cnt = new int[14];
        boolean pair = false;
        for (int x : ranks) {
            if (++cnt[x] == 3) {
                return "Three of a Kind";
            }
            pair = pair || cnt[x] == 2;
        }
        return pair ? "Pair" : "High Card";
    }
}
```

#### C++

```cpp
class Solution {
public:
    string bestHand(vector<int>& ranks, vector<char>& suits) {
        bool flush = true;
        for (int i = 1; i < 5 && flush; ++i) {
            flush = suits[i] == suits[i - 1];
        }
        if (flush) {
            return "Flush";
        }
        int cnt[14]{};
        bool pair = false;
        for (int& x : ranks) {
            if (++cnt[x] == 3) {
                return "Three of a Kind";
            }
            pair |= cnt[x] == 2;
        }
        return pair ? "Pair" : "High Card";
    }
};
```

#### Go

```go
func bestHand(ranks []int, suits []byte) string {
	flush := true
	for i := 1; i < 5 && flush; i++ {
		flush = suits[i] == suits[i-1]
	}
	if flush {
		return "Flush"
	}
	cnt := [14]int{}
	pair := false
	for _, x := range ranks {
		cnt[x]++
		if cnt[x] == 3 {
			return "Three of a Kind"
		}
		pair = pair || cnt[x] == 2
	}
	if pair {
		return "Pair"
	}
	return "High Card"
}
```

#### TypeScript

```ts
function bestHand(ranks: number[], suits: string[]): string {
    if (suits.every(v => v === suits[0])) {
        return 'Flush';
    }
    const count = new Array(14).fill(0);
    let isPair = false;
    for (const v of ranks) {
        if (++count[v] === 3) {
            return 'Three of a Kind';
        }
        isPair = isPair || count[v] === 2;
    }
    if (isPair) {
        return 'Pair';
    }
    return 'High Card';
}
```

#### Rust

```rust
impl Solution {
    pub fn best_hand(ranks: Vec<i32>, suits: Vec<char>) -> String {
        if suits.iter().all(|v| *v == suits[0]) {
            return "Flush".to_string();
        }
        let mut count = [0; 14];
        let mut is_pair = false;
        for &v in ranks.iter() {
            let i = v as usize;
            count[i] += 1;
            if count[i] == 3 {
                return "Three of a Kind".to_string();
            }
            is_pair = is_pair || count[i] == 2;
        }
        (if is_pair { "Pair" } else { "High Card" }).to_string()
    }
}
```

#### C

```c
char* bestHand(int* ranks, int ranksSize, char* suits, int suitsSize) {
    bool isFlush = true;
    for (int i = 1; i < suitsSize; i++) {
        if (suits[0] != suits[i]) {
            isFlush = false;
            break;
        }
    }
    if (isFlush) {
        return "Flush";
    }
    int count[14] = {0};
    bool isPair = false;
    for (int i = 0; i < ranksSize; i++) {
        if (++count[ranks[i]] == 3) {
            return "Three of a Kind";
        }
        isPair = isPair || count[ranks[i]] == 2;
    }
    if (isPair) {
        return "Pair";
    }
    return "High Card";
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
