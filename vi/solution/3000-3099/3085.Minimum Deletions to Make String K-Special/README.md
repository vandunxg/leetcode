---
comments: true
difficulty: Medium
rating: 1764
source: Weekly Contest 389 Q3
tags:
    - Greedy
    - Hash Table
    - String
    - Counting
    - Sorting
---

<!-- problem:start -->

# [3085. Minimum Deletions to Make String K-Special](https://leetcode.com/problems/minimum-deletions-to-make-string-k-special)

[中文文档](/solution/3000-3099/3085.Minimum%20Deletions%20to%20Make%20String%20K-Special/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>word</code> và một số nguyên <code>k</code>.</p>

<p>Ta gọi <code>word</code> là <strong>k-special</strong> nếu <code>|freq(word[i]) - freq(word[j])| &lt;= k</code> với mọi chỉ số <code>i</code> và <code>j</code> trong chuỗi.</p>

<p>Ở đây, <code>freq(x)</code> là <span data-keyword="frequency-letter">tần suất</span> của ký tự <code>x</code> trong <code>word</code>, còn <code>|y|</code> là giá trị tuyệt đối của <code>y</code>.</p>

<p>Trả về <em><strong>số lượng ký tự nhỏ nhất</strong> cần xóa để biến</em> <code>word</code> <strong><em>k-special</em></strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">word = &quot;aabcaba&quot;, k = 0</span></p>

<p><strong>Đầu ra: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">3</span></p>

<p><strong>Giải thích:</strong> Ta có thể biến <code>word</code> thành <code>0</code>-special bằng cách xóa <code>2</code> lần xuất hiện của <code>&quot;a&quot;</code> và <code>1</code> lần xuất hiện của <code>&quot;c&quot;</code>. Khi đó, <code>word</code> trở thành <code>&quot;baba&quot;</code>, với <code>freq(&#39;a&#39;) == freq(&#39;b&#39;) == 2</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">word = &quot;dabdcbdcdcd&quot;, k = 2</span></p>

<p><strong>Đầu ra: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">2</span></p>

<p><strong>Giải thích:</strong> Ta có thể biến <code>word</code> thành <code>2</code>-special bằng cách xóa <code>1</code> lần xuất hiện của <code>&quot;a&quot;</code> và <code>1</code> lần xuất hiện của <code>&quot;d&quot;</code>. Khi đó, <code>word</code> trở thành &quot;bdcbdcdcd&quot;, với <code>freq(&#39;b&#39;) == 2</code>, <code>freq(&#39;c&#39;) == 3</code> và <code>freq(&#39;d&#39;) == 4</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">word = &quot;aaabaaa&quot;, k = 2</span></p>

<p><strong>Đầu ra: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">1</span></p>

<p><strong>Giải thích:</strong> Ta có thể biến <code>word</code> thành <code>2</code>-special bằng cách xóa <code>1</code> lần xuất hiện của <code>&quot;b&quot;</code>. Khi đó, <code>word</code> trở thành <code>&quot;aaaaaa&quot;</code>, với tần suất của mỗi chữ cái đều là <code>6</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= word.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= k &lt;= 10<sup>5</sup></code></li>
	<li><code>word</code> chỉ bao gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Mọi cặp tần suất còn lại có thể chênh lệch nhiều nhất là $k$. Ta chỉ được xóa ký tự, $n \le 10^5$, và chỉ có $26$ chữ cái.
>
> Các tần suất được giữ lại nằm trong một khoảng $[v,v+k]$. Các chữ cái có tần suất nhỏ hơn $v$ sẽ bị xóa hoàn toàn; các chữ cái có tần suất lớn hơn $v+k$ sẽ bị giảm xuống còn $v+k$.
>
> Ta liệt kê tần suất nhỏ nhất được giữ lại $v$ và lấy tổng nhỏ nhất trong $26$ số đếm đó.

<!-- thinking:end -->

Trước tiên, ta đếm số lần xuất hiện của mỗi ký tự trong chuỗi và đưa tất cả số đếm vào một mảng $nums$. Vì chuỗi chỉ chứa các chữ cái viết thường nên độ dài của mảng $nums$ không vượt quá $26$.

Tiếp theo, ta liệt kê tần suất nhỏ nhất $v$ của các ký tự trong chuỗi $K$-special trong khoảng $[0,..n]$, rồi dùng hàm $f(v)$ để tính số lần xóa nhỏ nhất cần thiết nhằm điều chỉnh tần suất của mọi ký tự về $v$. Giá trị nhỏ nhất của mọi $f(v)$ là đáp án.

Cách tính hàm $f(v)$ như sau:

Duyệt từng phần tử $x$ trong mảng $nums$. Nếu $x < v$, điều đó có nghĩa là ta cần xóa toàn bộ các ký tự có tần suất $x$, nên số lần xóa là $x$. Nếu $x > v + k$, ta cần điều chỉnh tất cả ký tự có tần suất $x$ về $v + k$, nên số lần xóa là $x - v - k$. Tổng của tất cả số lần xóa là giá trị của $f(v)$.

Độ phức tạp thời gian là $O(n \times |\Sigma|)$, và độ phức tạp không gian là $O(|\Sigma|)$. Trong đó, $n$ là độ dài chuỗi, còn $|\Sigma|$ là kích thước của tập ký tự. Trong bài toán này, $|\Sigma| = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumDeletions(self, word: str, k: int) -> int:
        def f(v: int) -> int:
            ans = 0
            for x in nums:
                if x < v:
                    ans += x
                elif x > v + k:
                    ans += x - v - k
            return ans

        nums = Counter(word).values()
        return min(f(v) for v in range(len(word) + 1))
```

#### Java

```java
class Solution {
    private List<Integer> nums = new ArrayList<>();

    public int minimumDeletions(String word, int k) {
        int[] freq = new int[26];
        int n = word.length();
        for (int i = 0; i < n; ++i) {
            ++freq[word.charAt(i) - 'a'];
        }
        for (int v : freq) {
            if (v > 0) {
                nums.add(v);
            }
        }
        int ans = n;
        for (int i = 0; i <= n; ++i) {
            ans = Math.min(ans, f(i, k));
        }
        return ans;
    }

    private int f(int v, int k) {
        int ans = 0;
        for (int x : nums) {
            if (x < v) {
                ans += x;
            } else if (x > v + k) {
                ans += x - v - k;
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
    int minimumDeletions(string word, int k) {
        int freq[26]{};
        for (char& c : word) {
            ++freq[c - 'a'];
        }
        vector<int> nums;
        for (int v : freq) {
            if (v) {
                nums.push_back(v);
            }
        }
        int n = word.size();
        int ans = n;
        auto f = [&](int v) {
            int ans = 0;
            for (int x : nums) {
                if (x < v) {
                    ans += x;
                } else if (x > v + k) {
                    ans += x - v - k;
                }
            }
            return ans;
        };
        for (int i = 0; i <= n; ++i) {
            ans = min(ans, f(i));
        }
        return ans;
    }
};
```

#### Go

```go
func minimumDeletions(word string, k int) int {
	freq := [26]int{}
	for _, c := range word {
		freq[c-'a']++
	}
	nums := []int{}
	for _, v := range freq {
		if v > 0 {
			nums = append(nums, v)
		}
	}
	f := func(v int) int {
		ans := 0
		for _, x := range nums {
			if x < v {
				ans += x
			} else if x > v+k {
				ans += x - v - k
			}
		}
		return ans
	}
	ans := len(word)
	for i := 0; i <= len(word); i++ {
		ans = min(ans, f(i))
	}
	return ans
}
```

#### TypeScript

```ts
function minimumDeletions(word: string, k: number): number {
    const freq: number[] = Array(26).fill(0);
    for (const ch of word) {
        ++freq[ch.charCodeAt(0) - 97];
    }
    const nums = freq.filter(x => x > 0);
    const f = (v: number): number => {
        let ans = 0;
        for (const x of nums) {
            if (x < v) {
                ans += x;
            } else if (x > v + k) {
                ans += x - v - k;
            }
        }
        return ans;
    };
    return Math.min(...Array.from({ length: word.length + 1 }, (_, i) => f(i)));
}
```

#### Rust

```rust
impl Solution {
    pub fn minimum_deletions(word: String, k: i32) -> i32 {
        let mut freq = [0; 26];
        for c in word.chars() {
            freq[(c as u8 - b'a') as usize] += 1;
        }
        let mut nums = vec![];
        for &v in freq.iter() {
            if v > 0 {
                nums.push(v);
            }
        }
        let n = word.len() as i32;
        let mut ans = n;
        for i in 0..=n {
            let mut cur = 0;
            for &x in nums.iter() {
                if x < i {
                    cur += x;
                } else if x > i + k {
                    cur += x - i - k;
                }
            }
            ans = ans.min(cur);
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
