---
comments: true
difficulty: Hard
rating: 2233
source: Weekly Contest 152 Q4
tags:
    - Bit Manipulation
    - Trie
    - Array
    - Hash Table
    - String
---

<!-- problem:start -->

# [1178. Number of Valid Words for Each Puzzle](https://leetcode.com/problems/number-of-valid-words-for-each-puzzle)

[中文文档](/solution/1100-1199/1178.Number%20of%20Valid%20Words%20for%20Each%20Puzzle/README.md)

## Mô tả

<!-- description:start -->

Với chuỗi <code>puzzle</code> cho trước, một <code>word</code> được gọi là <em>hợp lệ</em> nếu thỏa mãn cả hai điều kiện sau:
<ul>
	<li><code>word</code> chứa chữ cái đầu tiên của <code>puzzle</code>.</li>
	<li>Mọi chữ cái trong <code>word</code> đều có trong <code>puzzle</code>.
	<ul>
		<li>Ví dụ, nếu puzzle là <code>&quot;abcdefg&quot;</code>, các từ hợp lệ là <code>&quot;faced&quot;</code>, <code>&quot;cabbage&quot;</code> và <code>&quot;baggage&quot;</code>, còn</li>
		<li>các từ không hợp lệ là <code>&quot;beefed&quot;</code> (không chứa <code>&#39;a&#39;</code>) và <code>&quot;based&quot;</code> (chứa <code>&#39;s&#39;</code>, ký tự không có trong puzzle).</li>
	</ul>
	</li>
</ul>
Trả về <em>mảng </em><code>answer</code><em>, trong đó </em><code>answer[i]</code><em> là số từ trong danh sách </em><code>words</code><em> hợp lệ đối với puzzle </em><code>puzzles[i]</code>.
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;aaaa&quot;,&quot;asas&quot;,&quot;able&quot;,&quot;ability&quot;,&quot;actt&quot;,&quot;actor&quot;,&quot;access&quot;], puzzles = [&quot;aboveyz&quot;,&quot;abrodyz&quot;,&quot;abslute&quot;,&quot;absoryz&quot;,&quot;actresz&quot;,&quot;gaswxyz&quot;]
<strong>Đầu ra:</strong> [1,1,3,2,4,0]
<strong>Giải thích:</strong> 
1 từ hợp lệ cho &quot;aboveyz&quot;: &quot;aaaa&quot; 
1 từ hợp lệ cho &quot;abrodyz&quot;: &quot;aaaa&quot;
3 từ hợp lệ cho &quot;abslute&quot;: &quot;aaaa&quot;, &quot;asas&quot;, &quot;able&quot;
2 từ hợp lệ cho &quot;absoryz&quot;: &quot;aaaa&quot;, &quot;asas&quot;
4 từ hợp lệ cho &quot;actresz&quot;: &quot;aaaa&quot;, &quot;asas&quot;, &quot;actt&quot;, &quot;access&quot;
Không có từ nào hợp lệ cho &quot;gaswxyz&quot; vì không từ nào trong danh sách chứa chữ cái &#39;g&#39;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;apple&quot;,&quot;pleas&quot;,&quot;please&quot;], puzzles = [&quot;aelwxyz&quot;,&quot;aelpxyz&quot;,&quot;aelpsxy&quot;,&quot;saelpxy&quot;,&quot;xaelpsy&quot;]
<strong>Đầu ra:</strong> [0,1,3,2,0]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 10<sup>5</sup></code></li>
	<li><code>4 &lt;= words[i].length &lt;= 50</code></li>
	<li><code>1 &lt;= puzzles.length &lt;= 10<sup>4</sup></code></li>
	<li><code>puzzles[i].length == 7</code></li>
	<li><code>words[i]</code> và <code>puzzles[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>puzzles[i] </code>không chứa ký tự lặp lại.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nén trạng thái + Hash Table + Liệt kê tập con

<!-- thinking:start -->

> **Tư duy**
>
> Tập ký tự của một từ hợp lệ phải là tập con của puzzle và chứa chữ cái đầu tiên của puzzle. Với bảng chữ cái nhỏ, các chuỗi ngắn có thể được nén thành bit mask. Đếm số từ ứng với mỗi mask, sau đó liệt kê $2^7$ tập con của từng puzzle và cộng số lượng của các mask chứa chữ cái đầu tiên. Cách này hiệu quả hơn việc kiểm tra từng từ với từng puzzle.

<!-- thinking:end -->

Theo mô tả bài toán, với mỗi puzzle $p$ trong mảng $puzzles$, ta cần đếm số từ $w$ chứa chữ cái đầu tiên của $p$ và mọi chữ cái trong $w$ đều có trong $p$.

Vì mỗi chữ cái lặp lại trong một từ chỉ cần tính một lần, ta có thể dùng phương pháp nén trạng thái nhị phân để chuyển mỗi từ $w$ thành số nhị phân $mask$. Bit thứ $i$ của $mask$ bằng $1$ khi và chỉ khi chữ cái thứ $i$ xuất hiện trong $w$. Ta dùng hash table $cnt$ để đếm số từ ứng với mỗi trạng thái đã nén.

Tiếp theo, ta duyệt mảng puzzle $puzzles$. Độ dài mỗi puzzle $p$ cố định bằng $7$, nên chỉ cần liệt kê các tập con của $p$. Nếu tập con chứa chữ cái đầu tiên của $p$, ta tra cứu số lượng tương ứng trong hash table rồi cộng vào đáp án của puzzle hiện tại.

Sau khi duyệt xong, ta thu được số từ hợp lệ tương ứng với từng puzzle trong mảng $puzzles$ và trả về mảng kết quả.

Độ phức tạp thời gian là $O(m \times |w| + n \times 2^{|p|})$, độ phức tạp không gian là $O(m)$. Trong đó, $m$ và $n$ lần lượt là số phần tử của mảng $words$ và $puzzles$; $|w|$ là độ dài lớn nhất của từ trong $words$, còn $|p|$ là độ dài của puzzle trong mảng $puzzles$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findNumOfValidWords(self, words: List[str], puzzles: List[str]) -> List[int]:
        cnt = Counter()
        for w in words:
            mask = 0
            for c in w:
                mask |= 1 << (ord(c) - ord("a"))
            cnt[mask] += 1

        ans = []
        for p in puzzles:
            mask = 0
            for c in p:
                mask |= 1 << (ord(c) - ord("a"))
            x, i, j = 0, ord(p[0]) - ord("a"), mask
            while j:
                if j >> i & 1:
                    x += cnt[j]
                j = (j - 1) & mask
            ans.append(x)
        return ans
```

#### Java

```java
class Solution {
    public List<Integer> findNumOfValidWords(String[] words, String[] puzzles) {
        Map<Integer, Integer> cnt = new HashMap<>(words.length);
        for (var w : words) {
            int mask = 0;
            for (int i = 0; i < w.length(); ++i) {
                mask |= 1 << (w.charAt(i) - 'a');
            }
            cnt.merge(mask, 1, Integer::sum);
        }
        List<Integer> ans = new ArrayList<>();
        for (var p : puzzles) {
            int mask = 0;
            for (int i = 0; i < p.length(); ++i) {
                mask |= 1 << (p.charAt(i) - 'a');
            }
            int x = 0;
            int i = p.charAt(0) - 'a';
            for (int j = mask; j > 0; j = (j - 1) & mask) {
                if ((j >> i & 1) == 1) {
                    x += cnt.getOrDefault(j, 0);
                }
            }
            ans.add(x);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> findNumOfValidWords(vector<string>& words, vector<string>& puzzles) {
        unordered_map<int, int> cnt;
        for (auto& w : words) {
            int mask = 0;
            for (char& c : w) {
                mask |= 1 << (c - 'a');
            }
            cnt[mask]++;
        }
        vector<int> ans;
        for (auto& p : puzzles) {
            int mask = 0;
            for (char& c : p) {
                mask |= 1 << (c - 'a');
            }
            int x = 0;
            int i = p[0] - 'a';
            for (int j = mask; j; j = (j - 1) & mask) {
                if (j >> i & 1) {
                    x += cnt[j];
                }
            }
            ans.push_back(x);
        }
        return ans;
    }
};
```

#### Go

```go
func findNumOfValidWords(words []string, puzzles []string) (ans []int) {
	cnt := map[int]int{}
	for _, w := range words {
		mask := 0
		for _, c := range w {
			mask |= 1 << (c - 'a')
		}
		cnt[mask]++
	}
	for _, p := range puzzles {
		mask := 0
		for _, c := range p {
			mask |= 1 << (c - 'a')
		}
		x, i := 0, p[0]-'a'
		for j := mask; j > 0; j = (j - 1) & mask {
			if j>>i&1 > 0 {
				x += cnt[j]
			}
		}
		ans = append(ans, x)
	}
	return
}
```

#### TypeScript

```ts
function findNumOfValidWords(words: string[], puzzles: string[]): number[] {
    const cnt: Map<number, number> = new Map();
    for (const w of words) {
        let mask = 0;
        for (const c of w) {
            mask |= 1 << (c.charCodeAt(0) - 97);
        }
        cnt.set(mask, (cnt.get(mask) || 0) + 1);
    }
    const ans: number[] = [];
    for (const p of puzzles) {
        let mask = 0;
        for (const c of p) {
            mask |= 1 << (c.charCodeAt(0) - 97);
        }
        let x = 0;
        const i = p.charCodeAt(0) - 97;
        for (let j = mask; j; j = (j - 1) & mask) {
            if ((j >> i) & 1) {
                x += cnt.get(j) || 0;
            }
        }
        ans.push(x);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
