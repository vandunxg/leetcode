---
comments: true
difficulty: Medium
rating: 1563
source: Weekly Contest 417 Q2
tags:
    - Hash Table
    - String
    - Sliding Window
---

<!-- problem:start -->

# [3305. Count of Substrings Containing Every Vowel and K Consonants I](https://leetcode.com/problems/count-of-substrings-containing-every-vowel-and-k-consonants-i)

[中文文档](/solution/3300-3399/3305.Count%20of%20Substrings%20Containing%20Every%20Vowel%20and%20K%20Consonants%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>word</code> và một số nguyên <strong>không âm</strong> <code>k</code>.</p>

<p>Hãy trả về tổng số <span data-keyword="substring-nonempty">chuỗi con</span> của <code>word</code> chứa tất cả các nguyên âm (<code>&#39;a&#39;</code>, <code>&#39;e&#39;</code>, <code>&#39;i&#39;</code>, <code>&#39;o&#39;</code> và <code>&#39;u&#39;</code>) <strong>ít nhất</strong> một lần và có <strong>đúng</strong> <code>k</code> phụ âm.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">word = &quot;aeioqq&quot;, k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có chuỗi con nào chứa tất cả các nguyên âm.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">word = &quot;aeiou&quot;, k = 0</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi con duy nhất chứa tất cả các nguyên âm và không có phụ âm là <code>word[0..4]</code>, tức là <code>&quot;aeiou&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">word = &quot;</span>ieaouqqieaouqq<span class="example-io">&quot;, k = 1</span></p>

<p><strong>Đầu ra:</strong> 3</p>

<p><strong>Giải thích:</strong></p>

<p>Các chuỗi con chứa tất cả các nguyên âm và một phụ âm là:</p>

<ul>
	<li><code>word[0..5]</code>, tức là <code>&quot;ieaouq&quot;</code>.</li>
	<li><code>word[6..11]</code>, tức là <code>&quot;qieaou&quot;</code>.</li>
	<li><code>word[7..12]</code>, tức là <code>&quot;ieaouq&quot;</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>5 &lt;= word.length &lt;= 250</code></li>
	<li><code>word</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>0 &lt;= k &lt;= word.length - 5</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Biến đổi bài toán + Cửa sổ trượt

<!-- thinking:start -->

> **Tư duy**
>
> Với $|\textit{word}| \le 250$, ta có thể liệt kê các chuỗi con, nhưng một cửa sổ phải chứa tất cả các nguyên âm và đúng $k$ phụ âm thì không đơn điệu theo một ràng buộc duy nhất.
>
> Ta biến đổi “đúng $k$ phụ âm” thành $f(k)-f(k+1)$, trong đó $f(t)$ đếm các chuỗi con chứa cả năm nguyên âm và có ít nhất $t$ phụ âm. Dạng “ít nhất” cho phép con trỏ trái tiến lên mỗi khi cửa sổ hợp lệ.
>
> Khi đó, mọi chuỗi con kết thúc tại đầu phải hiện tại và bắt đầu trong $[0, l)$ đều hợp lệ, nên ta cộng $l$. Một map theo dõi năm nguyên âm; các phụ âm được đếm riêng bằng biến $x$.

<!-- thinking:end -->

Ta có thể biến đổi bài toán thành việc giải hai bài toán con sau:

1. Tìm tổng số chuỗi con mà mỗi nguyên âm xuất hiện ít nhất một lần và chứa ít nhất $k$ phụ âm, ký hiệu là $\textit{f}(k)$;
2. Tìm tổng số chuỗi con mà mỗi nguyên âm xuất hiện ít nhất một lần và chứa ít nhất $k + 1$ phụ âm, ký hiệu là $\textit{f}(k + 1)$.

Khi đó, đáp án là $\textit{f}(k) - \textit{f}(k + 1)$.

Do đó, ta thiết kế hàm $\textit{f}(k)$ để đếm tổng số chuỗi con mà mỗi nguyên âm xuất hiện ít nhất một lần và chứa ít nhất $k$ phụ âm.

Ta có thể dùng một bảng băm $\textit{cnt}$ để đếm số lần xuất hiện của mỗi nguyên âm, một biến $\textit{ans}$ để lưu đáp án, một biến $\textit{l}$ để ghi nhận biên trái của cửa sổ trượt, và một biến $\textit{x}$ để ghi nhận số phụ âm trong cửa sổ hiện tại.

Duyệt qua chuỗi. Nếu ký tự hiện tại là nguyên âm, thêm ký tự đó vào bảng băm $\textit{cnt}$; ngược lại, tăng $\textit{x}$ lên một. Nếu $\textit{x} \ge k$ và kích thước bảng băm $\textit{cnt}$ bằng $5$, nghĩa là cửa sổ hiện tại thỏa mãn các điều kiện. Khi đó, ta lặp để dịch biên trái cho đến khi cửa sổ không còn thỏa mãn các điều kiện. Lúc này, mọi chuỗi con kết thúc tại biên phải $\textit{r}$ và có biên trái thuộc đoạn $[0, .. \textit{l} - 1]$ đều thỏa mãn các điều kiện, tổng cộng có $\textit{l}$ chuỗi con. Ta cộng $\textit{l}$ vào đáp án. Tiếp tục duyệt đến cuối chuỗi, ta nhận được $\textit{f}(k)$.

Cuối cùng, ta trả về $\textit{f}(k) - \textit{f}(k + 1)$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi $\textit{word}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countOfSubstrings(self, word: str, k: int) -> int:
        def f(k: int) -> int:
            cnt = Counter()
            ans = l = x = 0
            for c in word:
                if c in "aeiou":
                    cnt[c] += 1
                else:
                    x += 1
                while x >= k and len(cnt) == 5:
                    d = word[l]
                    if d in "aeiou":
                        cnt[d] -= 1
                        if cnt[d] == 0:
                            cnt.pop(d)
                    else:
                        x -= 1
                    l += 1
                ans += l
            return ans

        return f(k) - f(k + 1)
```

#### Java

```java
class Solution {
    public int countOfSubstrings(String word, int k) {
        return f(word, k) - f(word, k + 1);
    }

    private int f(String word, int k) {
        int ans = 0;
        int l = 0, x = 0;
        Map<Character, Integer> cnt = new HashMap<>(5);
        for (char c : word.toCharArray()) {
            if (vowel(c)) {
                cnt.merge(c, 1, Integer::sum);
            } else {
                ++x;
            }
            while (x >= k && cnt.size() == 5) {
                char d = word.charAt(l++);
                if (vowel(d)) {
                    if (cnt.merge(d, -1, Integer::sum) == 0) {
                        cnt.remove(d);
                    }
                } else {
                    --x;
                }
            }
            ans += l;
        }
        return ans;
    }

    private boolean vowel(char c) {
        return c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u';
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countOfSubstrings(string word, int k) {
        auto f = [&](int k) -> int {
            int ans = 0;
            int l = 0, x = 0;
            unordered_map<char, int> cnt;
            auto vowel = [&](char c) -> bool {
                return c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u';
            };
            for (char c : word) {
                if (vowel(c)) {
                    cnt[c]++;
                } else {
                    ++x;
                }
                while (x >= k && cnt.size() == 5) {
                    char d = word[l++];
                    if (vowel(d)) {
                        if (--cnt[d] == 0) {
                            cnt.erase(d);
                        }
                    } else {
                        --x;
                    }
                }
                ans += l;
            }
            return ans;
        };

        return f(k) - f(k + 1);
    }
};
```

#### Go

```go
func countOfSubstrings(word string, k int) int {
	f := func(k int) int {
		var ans int = 0
		l, x := 0, 0
		cnt := make(map[rune]int)
		vowel := func(c rune) bool {
			return c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u'
		}
		for _, c := range word {
			if vowel(c) {
				cnt[c]++
			} else {
				x++
			}
			for x >= k && len(cnt) == 5 {
				d := rune(word[l])
				l++
				if vowel(d) {
					cnt[d]--
					if cnt[d] == 0 {
						delete(cnt, d)
					}
				} else {
					x--
				}
			}
			ans += l
		}
		return ans
	}

	return f(k) - f(k+1)
}
```

#### TypeScript

```ts
function countOfSubstrings(word: string, k: number): number {
    const f = (k: number): number => {
        let ans = 0;
        let l = 0,
            x = 0;
        const cnt = new Map<string, number>();

        const vowel = (c: string): boolean => {
            return c === 'a' || c === 'e' || c === 'i' || c === 'o' || c === 'u';
        };

        for (const c of word) {
            if (vowel(c)) {
                cnt.set(c, (cnt.get(c) || 0) + 1);
            } else {
                x++;
            }

            while (x >= k && cnt.size === 5) {
                const d = word[l++];
                if (vowel(d)) {
                    cnt.set(d, cnt.get(d)! - 1);
                    if (cnt.get(d) === 0) {
                        cnt.delete(d);
                    }
                } else {
                    x--;
                }
            }
            ans += l;
        }

        return ans;
    };

    return f(k) - f(k + 1);
}
```

#### Rust

```rust
impl Solution {
    pub fn count_of_substrings(word: String, k: i32) -> i32 {
        fn f(word: &Vec<char>, k: i32) -> i32 {
            let mut ans = 0;
            let mut l = 0;
            let mut x = 0;
            let mut cnt = std::collections::HashMap::new();

            let is_vowel = |c: char| matches!(c, 'a' | 'e' | 'i' | 'o' | 'u');

            for (r, &c) in word.iter().enumerate() {
                if is_vowel(c) {
                    *cnt.entry(c).or_insert(0) += 1;
                } else {
                    x += 1;
                }

                while x >= k && cnt.len() == 5 {
                    let d = word[l];
                    l += 1;
                    if is_vowel(d) {
                        let count = cnt.entry(d).or_insert(0);
                        *count -= 1;
                        if *count == 0 {
                            cnt.remove(&d);
                        }
                    } else {
                        x -= 1;
                    }
                }
                ans += l as i32;
            }
            ans
        }

        let chars: Vec<char> = word.chars().collect();
        f(&chars, k) - f(&chars, k + 1)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
