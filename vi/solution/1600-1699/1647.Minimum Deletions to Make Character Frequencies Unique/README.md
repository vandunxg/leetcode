---
comments: true
difficulty: Medium
rating: 1509
source: Weekly Contest 214 Q2
tags:
    - Greedy
    - Hash Table
    - String
    - Sorting
---

<!-- problem:start -->

# [1647. Minimum Deletions to Make Character Frequencies Unique](https://leetcode.com/problems/minimum-deletions-to-make-character-frequencies-unique)

[中文文档](/solution/1600-1699/1647.Minimum%20Deletions%20to%20Make%20Character%20Frequencies%20Unique/README.md)

## Mô tả

<!-- description:start -->

<p>Chuỗi <code>s</code> được gọi là <strong>tốt</strong> nếu không có hai ký tự khác nhau trong <code>s</code> có cùng <strong>tần suất</strong>.</p>

<p>Cho chuỗi <code>s</code>, hãy trả về <em>số ký tự <strong>ít nhất</strong> cần xóa để biến </em><code>s</code><em> thành chuỗi <strong>tốt</strong>.</em></p>

<p><strong>Tần suất</strong> của một ký tự trong chuỗi là số lần ký tự đó xuất hiện. Ví dụ, trong chuỗi <code>&quot;aab&quot;</code>, <strong>tần suất</strong> của <code>&#39;a&#39;</code> là <code>2</code>, còn <strong>tần suất</strong> của <code>&#39;b&#39;</code> là <code>1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;aab&quot;
<strong>Output:</strong> 0
<strong>Explanation:</strong> <code>s</code> đã là chuỗi tốt.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;aaabbbcc&quot;
<strong>Output:</strong> 2
<strong>Explanation:</strong> Có thể xóa hai ký tự &#39;b&#39; để được chuỗi tốt &quot;aaabcc&quot;.
Một cách khác là xóa một ký tự &#39;b&#39; và một ký tự &#39;c&#39; để được chuỗi tốt &quot;aaabbc&quot;.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;ceabaacb&quot;
<strong>Output:</strong> 2
<strong>Explanation:</strong> Có thể xóa cả hai ký tự &#39;c&#39; để được chuỗi tốt &quot;eabaab&quot;.
Lưu ý rằng ta chỉ quan tâm đến các ký tự còn lại trong chuỗi sau cùng (tức là bỏ qua tần suất bằng 0).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code>&nbsp;chỉ chứa các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mảng + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Các tần suất phải trở nên khác nhau và ta chỉ được xóa ký tự. Với $26$ chữ cái, sắp xếp tần suất giảm dần để mỗi giá trị chiếm nhiều nhất một vị trí nguyên.
>
> Duy trì cận trên tự do tiếp theo $\textit{pre}$. Nếu $v \ge \textit{pre}$, xóa giảm xuống $\textit{pre}-1$ (hoặc xóa hết khi $\textit{pre}$ bằng $0$).
>
> Nếu không, giữ $v$ và đặt $\textit{pre}=v$.

<!-- thinking:end -->

Đầu tiên, ta dùng mảng $\textit{cnt}$ độ dài $26$ để đếm số lần xuất hiện của mỗi chữ cái trong chuỗi $s$.

Sau đó, ta sắp xếp mảng $\textit{cnt}$ theo thứ tự giảm dần. Ta định nghĩa biến $\textit{pre}$ để lưu số lần xuất hiện hiện tại của chữ cái.

Tiếp theo, ta duyệt từng phần tử $v$ trong mảng $\textit{cnt}$. Nếu $\textit{pre}$ hiện tại bằng $0$, ta cộng trực tiếp $v$ vào đáp án. Ngược lại, nếu $v \geq \textit{pre}$, ta cộng $v - \textit{pre} + 1$ vào đáp án và giảm $\textit{pre}$ đi $1$. Nếu không, ta cập nhật trực tiếp $\textit{pre}$ thành $v$. Sau đó tiếp tục với phần tử kế tiếp.

Sau khi duyệt xong, ta trả về đáp án.

Độ phức tạp thời gian là $O(n + |\Sigma| \times \log |\Sigma|)$, và độ phức tạp không gian là $O(|\Sigma|)$. Trong đó, $n$ là độ dài chuỗi $s$, còn $|\Sigma|$ là kích thước bảng chữ cái. Trong bài này, $|\Sigma| = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minDeletions(self, s: str) -> int:
        cnt = Counter(s)
        ans, pre = 0, inf
        for v in sorted(cnt.values(), reverse=True):
            if pre == 0:
                ans += v
            elif v >= pre:
                ans += v - pre + 1
                pre -= 1
            else:
                pre = v
        return ans
```

#### Java

```java
class Solution {
    public int minDeletions(String s) {
        int[] cnt = new int[26];
        for (int i = 0; i < s.length(); ++i) {
            ++cnt[s.charAt(i) - 'a'];
        }
        Arrays.sort(cnt);
        int ans = 0, pre = 1 << 30;
        for (int i = 25; i >= 0; --i) {
            int v = cnt[i];
            if (pre == 0) {
                ans += v;
            } else if (v >= pre) {
                ans += v - pre + 1;
                --pre;
            } else {
                pre = v;
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
    int minDeletions(string s) {
        vector<int> cnt(26);
        for (char& c : s) ++cnt[c - 'a'];
        sort(cnt.rbegin(), cnt.rend());
        int ans = 0, pre = 1 << 30;
        for (int& v : cnt) {
            if (pre == 0) {
                ans += v;
            } else if (v >= pre) {
                ans += v - pre + 1;
                --pre;
            } else {
                pre = v;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minDeletions(s string) (ans int) {
	cnt := make([]int, 26)
	for _, c := range s {
		cnt[c-'a']++
	}
	sort.Sort(sort.Reverse(sort.IntSlice(cnt)))
	pre := 1 << 30
	for _, v := range cnt {
		if pre == 0 {
			ans += v
		} else if v >= pre {
			ans += v - pre + 1
			pre--
		} else {
			pre = v
		}
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Tham lam (Giảm các phần tử kề nhau)

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 áp đặt một cận trên toàn cục. Tương đương, sau khi sắp xếp, ta buộc các tần suất kề nhau giảm dần: khi phần tử sau chưa nhỏ hơn, giảm nó và đếm số lần xóa.
>
> Cách cài đặt này đảm bảo “các phần tử kề nhau khác nhau” và có cùng độ phức tạp.

<!-- thinking:end -->

Đếm tần suất rồi sắp xếp theo thứ tự giảm dần. Duyệt các tần suất kề nhau, giảm tần suất hiện tại cho đến khi nó nhỏ hơn nghiêm ngặt tần suất trước đó.

Độ phức tạp thời gian là $O(n + |\Sigma| \times \log |\Sigma| + n)$, và độ phức tạp không gian là $O(|\Sigma|)$, trong đó $n$ là độ dài của $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minDeletions(self, s: str) -> int:
        cnt = Counter(s)
        vals = sorted(cnt.values(), reverse=True)
        ans = 0
        for i in range(1, len(vals)):
            while vals[i] >= vals[i - 1] and vals[i] > 0:
                vals[i] -= 1
                ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int minDeletions(String s) {
        int[] cnt = new int[26];
        for (int i = 0; i < s.length(); ++i) {
            ++cnt[s.charAt(i) - 'a'];
        }
        Arrays.sort(cnt);
        int ans = 0;
        for (int i = 24; i >= 0; --i) {
            while (cnt[i] >= cnt[i + 1] && cnt[i] > 0) {
                --cnt[i];
                ++ans;
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
    int minDeletions(string s) {
        vector<int> cnt(26);
        for (char& c : s) ++cnt[c - 'a'];
        sort(cnt.rbegin(), cnt.rend());
        int ans = 0;
        for (int i = 1; i < 26; ++i) {
            while (cnt[i] >= cnt[i - 1] && cnt[i] > 0) {
                --cnt[i];
                ++ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minDeletions(s string) (ans int) {
	cnt := make([]int, 26)
	for _, c := range s {
		cnt[c-'a']++
	}
	sort.Sort(sort.Reverse(sort.IntSlice(cnt)))
	for i := 1; i < 26; i++ {
		for cnt[i] >= cnt[i-1] && cnt[i] > 0 {
			cnt[i]--
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function minDeletions(s: string): number {
    let map = {};
    for (let c of s) {
        map[c] = (map[c] || 0) + 1;
    }
    let ans = 0;
    let vals: number[] = Object.values(map);
    vals.sort((a, b) => a - b);
    for (let i = 1; i < vals.length; ++i) {
        while (vals[i] > 0 && i != vals.indexOf(vals[i])) {
            --vals[i];
            ++ans;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    #[allow(dead_code)]
    pub fn min_deletions(s: String) -> i32 {
        let mut cnt = vec![0; 26];
        let mut ans = 0;

        for c in s.chars() {
            cnt[((c as u8) - ('a' as u8)) as usize] += 1;
        }

        cnt.sort_by(|&lhs, &rhs| rhs.cmp(&lhs));

        for i in 1..26 {
            while cnt[i] >= cnt[i - 1] && cnt[i] > 0 {
                cnt[i] -= 1;
                ans += 1;
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
