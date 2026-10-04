---
comments: true
difficulty: Hard
rating: 2449
source: Weekly Contest 374 Q3
tags:
    - Hash Table
    - String
    - Sliding Window
---

<!-- problem:start -->

# [2953. Count Complete Substrings](https://leetcode.com/problems/count-complete-substrings)

[中文文档](/solution/2900-2999/2953.Count%20Complete%20Substrings/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>word</code> và một số nguyên <code>k</code>.</p>

<p>Một chuỗi con <code>s</code> của <code>word</code> được gọi là <strong>đầy đủ</strong> nếu:</p>

<ul>
	<li>Mỗi ký tự trong <code>s</code> xuất hiện <strong>chính xác</strong> <code>k</code> lần.</li>
	<li>Chênh lệch giữa hai ký tự liền kề không <strong>vượt quá</strong> <code>2</code>. Nghĩa là, với mọi hai ký tự liền kề <code>c1</code> và <code>c2</code> trong <code>s</code>, chênh lệch tuyệt đối giữa vị trí của chúng trong bảng chữ cái không <strong>vượt quá</strong> <code>2</code>.</li>
</ul>

<p>Trả về <em>số lượng chuỗi con <strong>đầy đủ</strong> của</em> <code>word</code>.</p>

<p><strong>Chuỗi con</strong> là một dãy ký tự liên tiếp <strong>không rỗng</strong> trong một chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;igigee&quot;, k = 2
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Các chuỗi con đầy đủ trong đó mỗi ký tự xuất hiện chính xác hai lần và chênh lệch giữa các ký tự liền kề không vượt quá 2 là: <u><strong>igig</strong></u>ee, igig<u><strong>ee</strong></u>, <u><strong>igigee</strong></u>.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;aaabbbccc&quot;, k = 3
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Các chuỗi con đầy đủ trong đó mỗi ký tự xuất hiện chính xác ba lần và chênh lệch giữa các ký tự liền kề không vượt quá 2 là: <strong><u>aaa</u></strong>bbbccc, aaa<u><strong>bbb</strong></u>ccc, aaabbb<u><strong>ccc</strong></u>, <strong><u>aaabbb</u></strong>ccc, aaa<u><strong>bbbccc</strong></u>, <u><strong>aaabbbccc</strong></u>.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= word.length &lt;= 10<sup>5</sup></code></li>
	<li><code>word</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>1 &lt;= k &lt;= word.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê loại ký tự + Cửa sổ trượt

<!-- thinking:start -->

> **Tư duy**
>
> Một chuỗi con đầy đủ có mỗi ký tự xuất hiện chính xác $k$ lần và các codepoint liền kề chênh lệch không quá $2$. Điều kiện thứ hai chia chuỗi thành các đoạn độc lập; đáp án là tổng kết quả trên từng đoạn. Một đoạn có nhiều nhất $26$ chữ cái, nên ta liệt kê số loại ký tự $i$ và trượt một cửa sổ có độ dài $i \cdot k$.
>
> $cnt$ và $freq$ lần lượt theo dõi số chữ cái xuất hiện bao nhiêu lần; ta tăng kết quả khi $freq[k]=i$. Cả việc chia đoạn và duyệt các cửa sổ đều có độ phức tạp tuyến tính với $n \le 10^5$.

<!-- thinking:end -->

Theo điều kiện 2 trong đề bài, ta nhận thấy trong một chuỗi đầy đủ, chênh lệch giữa hai ký tự liền kề không vượt quá 2. Vì vậy, ta duyệt chuỗi $word$ và dùng hai con trỏ để chia $word$ thành một số chuỗi con. Số loại ký tự trong các chuỗi con này không vượt quá 26, đồng thời chênh lệch giữa các ký tự liền kề không vượt quá 2. Tiếp theo, ta chỉ cần đếm số chuỗi con trong mỗi chuỗi con mà mỗi ký tự xuất hiện $k$ lần.

Ta định nghĩa hàm $f(s)$ để đếm số chuỗi con trong chuỗi $s$ mà mỗi ký tự xuất hiện $k$ lần. Vì số loại ký tự trong $s$ không vượt quá 26, ta có thể liệt kê từng số loại ký tự $i$, với $1 \le i \le 26$, khi đó độ dài của chuỗi con có $i$ loại ký tự là $l = i \times k$.

Ta có thể dùng một mảng hoặc hash table $cnt$ để duy trì số lần xuất hiện của mỗi ký tự trong cửa sổ trượt có độ dài $l$, và dùng một hash table khác là $freq$ để duy trì số lần xuất hiện của từng tần suất. Nếu $freq[k] = i$, nghĩa là có $i$ ký tự xuất hiện $k$ lần, thì ta đã tìm được một chuỗi con thỏa mãn điều kiện. Ta dùng hai con trỏ để duy trì cửa sổ trượt này. Mỗi lần di chuyển con trỏ phải, ta tăng số lần xuất hiện của ký tự mà con trỏ phải đang trỏ tới và cập nhật mảng $freq$; mỗi lần di chuyển con trỏ trái, ta giảm số lần xuất hiện của ký tự mà con trỏ trái đang trỏ tới và cập nhật mảng $freq$. Sau mỗi lần di chuyển con trỏ, ta kiểm tra xem $freq[k]$ có bằng $i$ hay không. Nếu bằng, nghĩa là ta đã tìm được một chuỗi con thỏa mãn điều kiện.

Độ phức tạp thời gian là $O(n \times |\Sigma|)$, độ phức tạp không gian là $O(|\Sigma|)$, trong đó $n$ là độ dài của chuỗi $word$ và $\Sigma$ là kích thước của tập ký tự. Trong bài toán này, tập ký tự là các chữ cái tiếng Anh viết thường, nên $|\Sigma| = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countCompleteSubstrings(self, word: str, k: int) -> int:
        def f(s: str) -> int:
            m = len(s)
            ans = 0
            for i in range(1, 27):
                l = i * k
                if l > m:
                    break
                cnt = Counter(s[:l])
                freq = Counter(cnt.values())
                ans += freq[k] == i
                for j in range(l, m):
                    freq[cnt[s[j]]] -= 1
                    cnt[s[j]] += 1
                    freq[cnt[s[j]]] += 1

                    freq[cnt[s[j - l]]] -= 1
                    cnt[s[j - l]] -= 1
                    freq[cnt[s[j - l]]] += 1

                    ans += freq[k] == i
            return ans

        n = len(word)
        ans = i = 0
        while i < n:
            j = i + 1
            while j < n and abs(ord(word[j]) - ord(word[j - 1])) <= 2:
                j += 1
            ans += f(word[i:j])
            i = j
        return ans
```

#### Java

```java
class Solution {
    public int countCompleteSubstrings(String word, int k) {
        int n = word.length();
        int ans = 0;
        for (int i = 0; i < n;) {
            int j = i + 1;
            while (j < n && Math.abs(word.charAt(j) - word.charAt(j - 1)) <= 2) {
                ++j;
            }
            ans += f(word.substring(i, j), k);
            i = j;
        }
        return ans;
    }

    private int f(String s, int k) {
        int m = s.length();
        int ans = 0;
        for (int i = 1; i <= 26 && i * k <= m; ++i) {
            int l = i * k;
            int[] cnt = new int[26];
            for (int j = 0; j < l; ++j) {
                ++cnt[s.charAt(j) - 'a'];
            }
            Map<Integer, Integer> freq = new HashMap<>();
            for (int x : cnt) {
                if (x > 0) {
                    freq.merge(x, 1, Integer::sum);
                }
            }
            if (freq.getOrDefault(k, 0) == i) {
                ++ans;
            }
            for (int j = l; j < m; ++j) {
                int a = s.charAt(j) - 'a';
                int b = s.charAt(j - l) - 'a';
                freq.merge(cnt[a], -1, Integer::sum);
                ++cnt[a];
                freq.merge(cnt[a], 1, Integer::sum);

                freq.merge(cnt[b], -1, Integer::sum);
                --cnt[b];
                freq.merge(cnt[b], 1, Integer::sum);
                if (freq.getOrDefault(k, 0) == i) {
                    ++ans;
                }
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countCompleteSubstrings(string word, int k) {
        int n = word.length();
        int ans = 0;
        auto f = [&](string s) {
            int m = s.length();
            int ans = 0;
            for (int i = 1; i <= 26 && i * k <= m; ++i) {
                int l = i * k;
                int cnt[26]{};
                for (int j = 0; j < l; ++j) {
                    ++cnt[s[j] - 'a'];
                }
                unordered_map<int, int> freq;
                for (int x : cnt) {
                    if (x > 0) {
                        freq[x]++;
                    }
                }
                if (freq[k] == i) {
                    ++ans;
                }
                for (int j = l; j < m; ++j) {
                    int a = s[j] - 'a';
                    int b = s[j - l] - 'a';
                    freq[cnt[a]]--;
                    cnt[a]++;
                    freq[cnt[a]]++;

                    freq[cnt[b]]--;
                    cnt[b]--;
                    freq[cnt[b]]++;

                    if (freq[k] == i) {
                        ++ans;
                    }
                }
            }
            return ans;
        };
        for (int i = 0; i < n;) {
            int j = i + 1;
            while (j < n && abs(word[j] - word[j - 1]) <= 2) {
                ++j;
            }
            ans += f(word.substr(i, j - i));
            i = j;
        }
        return ans;
    }
};
```

#### Go

```go
func countCompleteSubstrings(word string, k int) (ans int) {
	n := len(word)
	f := func(s string) (ans int) {
		m := len(s)
		for i := 1; i <= 26 && i*k <= m; i++ {
			l := i * k
			cnt := [26]int{}
			for j := 0; j < l; j++ {
				cnt[int(s[j]-'a')]++
			}
			freq := map[int]int{}
			for _, x := range cnt {
				if x > 0 {
					freq[x]++
				}
			}
			if freq[k] == i {
				ans++
			}
			for j := l; j < m; j++ {
				a := int(s[j] - 'a')
				b := int(s[j-l] - 'a')
				freq[cnt[a]]--
				cnt[a]++
				freq[cnt[a]]++

				freq[cnt[b]]--
				cnt[b]--
				freq[cnt[b]]++

				if freq[k] == i {
					ans++
				}
			}
		}
		return
	}
	for i := 0; i < n; {
		j := i + 1
		for j < n && abs(int(word[j])-int(word[j-1])) <= 2 {
			j++
		}
		ans += f(word[i:j])
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
function countCompleteSubstrings(word: string, k: number): number {
    const f = (s: string): number => {
        const m = s.length;
        let ans = 0;
        for (let i = 1; i <= 26 && i * k <= m; i++) {
            const l = i * k;
            const cnt: number[] = new Array(26).fill(0);
            for (let j = 0; j < l; j++) {
                cnt[s.charCodeAt(j) - 'a'.charCodeAt(0)]++;
            }
            const freq: { [key: number]: number } = {};
            for (const x of cnt) {
                if (x > 0) {
                    freq[x] = (freq[x] || 0) + 1;
                }
            }
            if (freq[k] === i) {
                ans++;
            }

            for (let j = l; j < m; j++) {
                const a = s.charCodeAt(j) - 'a'.charCodeAt(0);
                const b = s.charCodeAt(j - l) - 'a'.charCodeAt(0);

                freq[cnt[a]]--;
                cnt[a]++;
                freq[cnt[a]] = (freq[cnt[a]] || 0) + 1;

                freq[cnt[b]]--;
                cnt[b]--;
                freq[cnt[b]] = (freq[cnt[b]] || 0) + 1;

                if (freq[k] === i) {
                    ans++;
                }
            }
        }

        return ans;
    };

    let n = word.length;
    let ans = 0;
    for (let i = 0; i < n;) {
        let j = i + 1;
        while (j < n && Math.abs(word.charCodeAt(j) - word.charCodeAt(j - 1)) <= 2) {
            j++;
        }
        ans += f(word.substring(i, j));
        i = j;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
