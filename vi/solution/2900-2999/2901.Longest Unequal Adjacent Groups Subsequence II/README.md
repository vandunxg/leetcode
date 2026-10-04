---
comments: true
difficulty: Medium
rating: 1898
source: Biweekly Contest 115 Q3
tags:
    - Array
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [2901. Longest Unequal Adjacent Groups Subsequence II](https://leetcode.com/problems/longest-unequal-adjacent-groups-subsequence-ii)

[中文文档](/solution/2900-2999/2901.Longest%20Unequal%20Adjacent%20Groups%20Subsequence%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng chuỗi <code>words</code> và một mảng <code>groups</code>, cả hai đều có độ dài <code>n</code>.</p>

<p><strong>Khoảng cách Hamming</strong> giữa hai chuỗi có cùng độ dài là số vị trí mà các ký tự tương ứng <strong>khác nhau</strong>.</p>

<p>Bạn cần chọn <span data-keyword="subsequence-array">dãy con</span> <strong>dài nhất</strong> từ một mảng chỉ số <code>[0, 1, ..., n - 1]</code>, sao cho với dãy con <code>[i<sub>0</sub>, i<sub>1</sub>, ..., i<sub>k-1</sub>]</code> có độ dài <code>k</code>, các điều kiện sau được thỏa mãn:</p>

<ul>
	<li>Với các chỉ số <strong>kề nhau</strong> trong dãy con, các groups tương ứng phải <strong>khác nhau</strong>, tức là <code>groups[i<sub>j</sub>] != groups[i<sub>j+1</sub>]</code>, với mọi <code>j</code> sao cho <code>0 &lt; j + 1 &lt; k</code>.</li>
	<li><code>words[i<sub>j</sub>]</code> và <code>words[i<sub>j+1</sub>]</code> phải có <strong>cùng</strong> độ dài, đồng thời <strong>khoảng cách Hamming</strong> giữa chúng bằng <code>1</code>, với <code>0 &lt; j + 1 &lt; k</code>, cho mọi chỉ số trong dãy con.</li>
</ul>

<p>Trả về <em>một mảng chuỗi chứa các words tương ứng với các chỉ số <strong>(theo thứ tự)</strong> trong dãy con đã chọn</em>. Nếu có nhiều đáp án, trả về <em>bất kỳ đáp án nào</em>.</p>

<p><strong>Lưu ý:</strong> các chuỗi trong <code>words</code> có thể có độ dài <strong>khác nhau</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">words = [&quot;bab&quot;,&quot;dab&quot;,&quot;cab&quot;], groups = [1,2,2]</span></p>

<p><strong>Đầu ra: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">[&quot;bab&quot;,&quot;cab&quot;]</span></p>

<p><strong>Giải thích: </strong>Một dãy con có thể được chọn là <code>[0,2]</code>.</p>

<ul>
	<li><code>groups[0] != groups[2]</code></li>
	<li><code>words[0].length == words[2].length</code>, và khoảng cách Hamming giữa chúng bằng 1.</li>
</ul>

<p>Vì vậy, một đáp án hợp lệ là <code>[words[0],words[2]] = [&quot;bab&quot;,&quot;cab&quot;]</code>.</p>

<p>Một dãy con khác có thể được chọn là <code>[0,1]</code>.</p>

<ul>
	<li><code>groups[0] != groups[1]</code></li>
	<li><code>words[0].length == words[1].length</code>, và khoảng cách Hamming giữa chúng bằng <code>1</code>.</li>
</ul>

<p>Vì vậy, một đáp án hợp lệ khác là <code>[words[0],words[1]] = [&quot;bab&quot;,&quot;dab&quot;]</code>.</p>

<p>Có thể chứng minh rằng độ dài của dãy con chỉ số dài nhất thỏa mãn các điều kiện là <code>2</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">words = [&quot;a&quot;,&quot;b&quot;,&quot;c&quot;,&quot;d&quot;], groups = [1,2,3,4]</span></p>

<p><strong>Đầu ra: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">[&quot;a&quot;,&quot;b&quot;,&quot;c&quot;,&quot;d&quot;]</span></p>

<p><strong>Giải thích: </strong>Ta có thể chọn dãy con <code>[0,1,2,3]</code>.</p>

<p>Dãy con này thỏa mãn cả hai điều kiện.</p>

<p>Do đó, đáp án là <code>[words[0],words[1],words[2],words[3]] = [&quot;a&quot;,&quot;b&quot;,&quot;c&quot;,&quot;d&quot;]</code>.</p>

<p>Dãy con này có độ dài lớn nhất trong tất cả các dãy con chỉ số thỏa mãn các điều kiện.</p>

<p>Vì vậy, đây là đáp án duy nhất.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == words.length == groups.length &lt;= 1000</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 10</code></li>
	<li><code>1 &lt;= groups[i] &lt;= n</code></li>
	<li><code>words</code> gồm các chuỗi <strong>khác nhau</strong>.</li>
	<li><code>words[i]</code> gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Khác với bài trước, các từ kề nhau còn phải có cùng độ dài và khoảng cách Hamming chính xác bằng $1$, nên việc tham lam xen kẽ các group không phải lúc nào cũng khả thi. $n$ chỉ ở mức vài trăm và các từ ngắn, nên có thể chấp nhận $O(n^2 \cdot L)$ chuyển trạng thái.
>
> Gọi $f[i]$ là độ dài lớn nhất của dãy kết thúc tại $i$, còn $g[i]$ là phần tử trước đó. Với $j < i$, ta cập nhật bằng $f[j]+1$ chỉ khi các group khác nhau và $check$ đúng. Bắt đầu từ một chỉ số đạt cực đại toàn cục $mx$, lần theo $g$ rồi đảo ngược để khôi phục một dãy con.

<!-- thinking:end -->

Ta định nghĩa $f[i]$ là độ dài của dãy con dài nhất có các group kề nhau khác nhau và kết thúc bằng từ thứ $i$, còn $g[i]$ là chỉ số phần tử trước đó của dãy con dài nhất kết thúc bằng từ thứ $i$. Ban đầu, ta đặt $f[i] = 1$ và $g[i] = -1$.

Ngoài ra, ta định nghĩa biến $mx$ là độ dài của dãy con dài nhất có các group kề nhau khác nhau.

Ta duyệt $i$ và $j \in [0, i)$, nếu $groups[i] \neq groups[j]$, $f[i] \lt f[j] + 1$, và khoảng cách Hamming giữa $words[i]$ và $words[j]$ bằng $1$, thì cập nhật $f[i] = f[j] + 1$, $g[i] = j$, đồng thời cập nhật $mx = \max(mx, f[i])$.

Cuối cùng, ta tìm chỉ số $i$ tương ứng với giá trị lớn nhất trong mảng $f$, sau đó liên tục truy ngược từ $i$ cho đến khi tìm được $g[i] = -1$; các phần tử thu được tạo thành dãy con dài nhất có các group kề nhau khác nhau.

Độ phức tạp thời gian là $O(n^2 \times L)$, và độ phức tạp không gian là $O(n)$. Ở đây, $L$ là độ dài lớn nhất của một từ.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getWordsInLongestSubsequence(
        self, words: List[str], groups: List[int]
    ) -> List[str]:
        def check(s: str, t: str) -> bool:
            return len(s) == len(t) and sum(a != b for a, b in zip(s, t)) == 1

        n = len(groups)
        f = [1] * n
        g = [-1] * n
        mx = 1
        for i, x in enumerate(groups):
            for j, y in enumerate(groups[:i]):
                if x != y and f[i] < f[j] + 1 and check(words[i], words[j]):
                    f[i] = f[j] + 1
                    g[i] = j
                    mx = max(mx, f[i])
        ans = []
        for i in range(n):
            if f[i] == mx:
                j = i
                while j >= 0:
                    ans.append(words[j])
                    j = g[j]
                break
        return ans[::-1]
```

#### Java

```java
class Solution {
    public List<String> getWordsInLongestSubsequence(String[] words, int[] groups) {
        int n = groups.length;
        int[] f = new int[n];
        int[] g = new int[n];
        Arrays.fill(f, 1);
        Arrays.fill(g, -1);
        int mx = 1;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                if (groups[i] != groups[j] && f[i] < f[j] + 1 && check(words[i], words[j])) {
                    f[i] = f[j] + 1;
                    g[i] = j;
                    mx = Math.max(mx, f[i]);
                }
            }
        }
        List<String> ans = new ArrayList<>();
        for (int i = 0; i < n; ++i) {
            if (f[i] == mx) {
                for (int j = i; j >= 0; j = g[j]) {
                    ans.add(words[j]);
                }
                break;
            }
        }
        Collections.reverse(ans);
        return ans;
    }

    private boolean check(String s, String t) {
        if (s.length() != t.length()) {
            return false;
        }
        int cnt = 0;
        for (int i = 0; i < s.length(); ++i) {
            if (s.charAt(i) != t.charAt(i)) {
                ++cnt;
            }
        }
        return cnt == 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> getWordsInLongestSubsequence(vector<string>& words, vector<int>& groups) {
        auto check = [](string& s, string& t) {
            if (s.size() != t.size()) {
                return false;
            }
            int cnt = 0;
            for (int i = 0; i < s.size(); ++i) {
                cnt += s[i] != t[i];
            }
            return cnt == 1;
        };
        int n = groups.size();
        vector<int> f(n, 1);
        vector<int> g(n, -1);
        int mx = 1;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                if (groups[i] != groups[j] && f[i] < f[j] + 1 && check(words[i], words[j])) {
                    f[i] = f[j] + 1;
                    g[i] = j;
                    mx = max(mx, f[i]);
                }
            }
        }
        vector<string> ans;
        for (int i = 0; i < n; ++i) {
            if (f[i] == mx) {
                for (int j = i; ~j; j = g[j]) {
                    ans.emplace_back(words[j]);
                }
                break;
            }
        }
        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```

#### Go

```go
func getWordsInLongestSubsequence(words []string, groups []int) []string {
	check := func(s, t string) bool {
		if len(s) != len(t) {
			return false
		}
		cnt := 0
		for i := range s {
			if s[i] != t[i] {
				cnt++
			}
		}
		return cnt == 1
	}
	n := len(groups)
	f := make([]int, n)
	g := make([]int, n)
	for i := range f {
		f[i] = 1
		g[i] = -1
	}
	mx := 1
	for i, x := range groups {
		for j, y := range groups[:i] {
			if x != y && f[i] < f[j]+1 && check(words[i], words[j]) {
				f[i] = f[j] + 1
				g[i] = j
				if mx < f[i] {
					mx = f[i]
				}
			}
		}
	}
	ans := make([]string, 0, mx)
	for i, x := range f {
		if x == mx {
			for j := i; j >= 0; j = g[j] {
				ans = append(ans, words[j])
			}
			break
		}
	}
	slices.Reverse(ans)
	return ans
}
```

#### TypeScript

```ts
function getWordsInLongestSubsequence(words: string[], groups: number[]): string[] {
    const n = groups.length;
    const f: number[] = Array(n).fill(1);
    const g: number[] = Array(n).fill(-1);
    let mx = 1;
    const check = (s: string, t: string) => {
        if (s.length !== t.length) {
            return false;
        }
        let cnt = 0;
        for (let i = 0; i < s.length; ++i) {
            if (s[i] !== t[i]) {
                ++cnt;
            }
        }
        return cnt === 1;
    };
    for (let i = 0; i < n; ++i) {
        for (let j = 0; j < i; ++j) {
            if (groups[i] !== groups[j] && f[i] < f[j] + 1 && check(words[i], words[j])) {
                f[i] = f[j] + 1;
                g[i] = j;
                mx = Math.max(mx, f[i]);
            }
        }
    }
    const ans: string[] = [];
    for (let i = 0; i < n; ++i) {
        if (f[i] === mx) {
            for (let j = i; ~j; j = g[j]) {
                ans.push(words[j]);
            }
            break;
        }
    }
    return ans.reverse();
}
```

#### Rust

```rust
impl Solution {
    pub fn get_words_in_longest_subsequence(words: Vec<String>, groups: Vec<i32>) -> Vec<String> {
        fn check(s: &str, t: &str) -> bool {
            s.len() == t.len() && s.chars().zip(t.chars()).filter(|(a, b)| a != b).count() == 1
        }

        let n = groups.len();

        let mut f = vec![1; n];
        let mut g = vec![-1; n];

        let mut mx = 1;

        for i in 0..n {
            let x = groups[i] as usize;
            for j in 0..i {
                let y = groups[j] as usize;
                if x != y && f[i] < f[j] + 1 && check(&words[i], &words[j]) {
                    f[i] = f[j] + 1;
                    g[i] = j as i32;
                    mx = mx.max(f[i]);
                }
            }
        }

        let mut ans = vec![];
        let mut i = n - 1;

        while f[i] != mx {
            i -= 1;
        }

        let mut j = i as i32;
        while j >= 0 {
            ans.push(words[j as usize].clone());
            j = g[j as usize];
        }

        ans.reverse();
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
