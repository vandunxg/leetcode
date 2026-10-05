---
comments: true
difficulty: Medium
rating: 1391
source: Weekly Contest 480 Q2
tags:
    - Two Pointers
    - String
    - Simulation
---

<!-- problem:start -->

# [3775. Reverse Words With Same Vowel Count](https://leetcode.com/problems/reverse-words-with-same-vowel-count)

[Tài liệu tiếng Trung](/solution/3700-3799/3775.Reverse%20Words%20With%20Same%20Vowel%20Count/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> gồm các từ tiếng Anh viết thường, mỗi từ được ngăn cách bởi một dấu cách.</p>

<p>Hãy xác định có bao nhiêu nguyên âm xuất hiện trong từ <strong>đầu tiên</strong>. Sau đó, đảo ngược mỗi từ tiếp theo có <strong>số lượng nguyên âm bằng nhau</strong>. Giữ nguyên tất cả các từ còn lại.</p>

<p>Trả về chuỗi kết quả.</p>

<p>Các nguyên âm là <code>&#39;a&#39;</code>, <code>&#39;e&#39;</code>, <code>&#39;i&#39;</code>, <code>&#39;o&#39;</code> và <code>&#39;u&#39;</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;cat and mice&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;cat dna mice&quot;</span></p>

<p><strong>Giải thích:</strong>​​​​​​​</p>

<ul>
	<li>Từ đầu tiên <code>&quot;cat&quot;</code> có 1 nguyên âm.</li>
	<li><code>&quot;and&quot;</code> có 1 nguyên âm, nên được đảo ngược thành <code>&quot;dna&quot;</code>.</li>
	<li><code>&quot;mice&quot;</code> có 2 nguyên âm, nên giữ nguyên.</li>
	<li>Do đó, chuỗi kết quả là <code>&quot;cat dna mice&quot;</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;book is nice&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;book is ecin&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Từ đầu tiên <code>&quot;book&quot;</code> có 2 nguyên âm.</li>
	<li><code>&quot;is&quot;</code> có 1 nguyên âm, nên giữ nguyên.</li>
	<li><code>&quot;nice&quot;</code> có 2 nguyên âm, nên được đảo ngược thành <code>&quot;ecin&quot;</code>.</li>
	<li>Do đó, chuỗi kết quả là <code>&quot;book is ecin&quot;</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;banana healthy&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;banana healthy&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Từ đầu tiên <code>&quot;banana&quot;</code> có 3 nguyên âm.</li>
	<li><code>&quot;healthy&quot;</code> có 2 nguyên âm, nên giữ nguyên.</li>
	<li>Do đó, chuỗi kết quả là <code>&quot;banana healthy&quot;</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường và dấu cách.</li>
	<li>Các từ trong <code>s</code> được ngăn cách bởi một dấu cách <strong>duy nhất</strong>.</li>
	<li><code>s</code> <strong>không</strong> chứa dấu cách ở đầu hoặc cuối.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Một từ phía sau được đảo ngược khi và chỉ khi nó có cùng số lượng nguyên âm với từ đầu tiên. Sau khi tách chuỗi, ta đếm số nguyên âm trong từ đầu tiên và áp dụng điều kiện đó cho từng từ tiếp theo trước khi nối lại.

<!-- thinking:end -->

Trước tiên, chúng ta tách chuỗi theo dấu cách thành danh sách từ $\textit{words}$. Sau đó, chúng ta tính số lượng nguyên âm $\textit{cnt}$ trong từ đầu tiên. Tiếp theo, chúng ta duyệt qua từng từ còn lại, tính số lượng nguyên âm của từ đó, và đảo ngược từ nếu số lượng này bằng $\textit{cnt}$. Cuối cùng, chúng ta nối lại danh sách từ đã xử lý thành một chuỗi và trả về chuỗi đó.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def reverseWords(self, s: str) -> str:
        def calc(w: str) -> int:
            return sum(c in "aeiou" for c in w)

        words = s.split()
        cnt = calc(words[0])
        ans = [words[0]]
        for w in words[1:]:
            if calc(w) == cnt:
                ans.append(w[::-1])
            else:
                ans.append(w)
        return " ".join(ans)
```

#### Java

```java
class Solution {
    public String reverseWords(String s) {
        String[] words = s.split("\\s+");

        int cnt = calc(words[0]);
        List<String> ans = new ArrayList<>();
        ans.add(words[0]);

        for (int i = 1; i < words.length; i++) {
            String w = words[i];
            if (calc(w) == cnt) {
                ans.add(new StringBuilder(w).reverse().toString());
            } else {
                ans.add(w);
            }
        }
        return String.join(" ", ans);
    }

    private int calc(String w) {
        int res = 0;
        for (char c : w.toCharArray()) {
            if (c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u') {
                res++;
            }
        }
        return res;
    }
}
```

#### C++

```cpp
class Solution {
public:
    string reverseWords(string s) {
        stringstream ss(s);
        string w;
        ss >> w;
        int cnt = calc(w);

        string ans = w;

        while (ss >> w) {
            ans.push_back(' ');
            if (calc(w) == cnt) {
                reverse(w.begin(), w.end());
            }
            ans += w;
        }

        return ans;
    }

private:
    int calc(const string& w) {
        return count_if(w.begin(), w.end(), [](char c) {
            return c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u';
        });
    }
};
```

#### Go

```go
func reverseWords(s string) string {
	words := strings.Fields(s)

	calc := func(w string) int {
		cnt := 0
		for _, c := range w {
			switch c {
			case 'a', 'e', 'i', 'o', 'u':
				cnt++
			}
		}
		return cnt
	}

	cnt := calc(words[0])
	var ans []string
	ans = append(ans, words[0])

	for i := 1; i < len(words); i++ {
		w := words[i]
		if calc(w) == cnt {
			b := []rune(w)
			for l, r := 0, len(b)-1; l < r; l, r = l+1, r-1 {
				b[l], b[r] = b[r], b[l]
			}
			w = string(b)
		}
		ans = append(ans, w)
	}

	return strings.Join(ans, " ")
}
```

#### TypeScript

```ts
function reverseWords(s: string): string {
    const words = s.split(/\s+/);

    const calc = (w: string): number => {
        let cnt = 0;
        for (const c of w) {
            if ('aeiou'.includes(c)) cnt++;
        }
        return cnt;
    };

    const cnt = calc(words[0]);
    const ans: string[] = [words[0]];

    for (let i = 1; i < words.length; i++) {
        let w = words[i];
        if (calc(w) === cnt) {
            w = w.split('').reverse().join('');
        }
        ans.push(w);
    }

    return ans.join(' ');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
