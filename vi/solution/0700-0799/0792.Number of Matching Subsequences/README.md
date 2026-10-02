---
comments: true
difficulty: Medium
tags:
    - Trie
    - Array
    - Hash Table
    - String
    - Binary Search
    - Dynamic Programming
    - Sorting
---

<!-- problem:start -->

# [792. Number of Matching Subsequences](https://leetcode.com/problems/number-of-matching-subsequences)

[中文文档](/solution/0700-0799/0792.Number%20of%20Matching%20Subsequences/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code> và mảng chuỗi <code>words</code>, hãy trả về <em>số lượng phần tử </em><code>words[i]</code><em> là dãy con của </em><code>s</code>.</p>

<p><strong>Dãy con</strong> của một chuỗi là chuỗi mới được tạo từ chuỗi ban đầu bằng cách xóa một số ký tự (có thể không xóa ký tự nào) mà không làm thay đổi thứ tự tương đối của các ký tự còn lại.</p>

<ul>
	<li>Ví dụ, <code>&quot;ace&quot;</code> là dãy con của <code>&quot;abcde&quot;</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abcde&quot;, words = [&quot;a&quot;,&quot;bb&quot;,&quot;acd&quot;,&quot;ace&quot;]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Có ba chuỗi trong words là dãy con của s: &quot;a&quot;, &quot;acd&quot;, &quot;ace&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;dsahjpjauf&quot;, words = [&quot;ahjpjau&quot;,&quot;ja&quot;,&quot;ahbwzgqnuk&quot;,&quot;tnmlanowax&quot;]
<strong>Đầu ra:</strong> 2
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= words.length &lt;= 5000</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 50</code></li>
	<li><code>s</code> và <code>words[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phân nhóm

<!-- thinking:start -->

> **Tư duy**
>
> $|s|\le 5\times 10^4$; nếu quét lại $s$ bằng một con trỏ cho từng từ thì sẽ đọc lại chuỗi nhiều lần. Hãy nhóm các từ theo ký tự tiếp theo mà chúng cần; khi duyệt $s$, chỉ cập nhật nhóm tương ứng.
>
> Từ nào đã khớp hoàn toàn thì được tính vào kết quả; nếu chưa, phần còn lại được chuyển sang nhóm theo ký tự đầu mới. Mỗi ký tự của mỗi từ chỉ được xử lý một lần.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numMatchingSubseq(self, s: str, words: List[str]) -> int:
        d = defaultdict(deque)
        for w in words:
            d[w[0]].append(w)
        ans = 0
        for c in s:
            for _ in range(len(d[c])):
                t = d[c].popleft()
                if len(t) == 1:
                    ans += 1
                else:
                    d[t[1]].append(t[1:])
        return ans
```

#### Java

```java
class Solution {
    public int numMatchingSubseq(String s, String[] words) {
        Deque<String>[] d = new Deque[26];
        Arrays.setAll(d, k -> new ArrayDeque<>());
        for (String w : words) {
            d[w.charAt(0) - 'a'].add(w);
        }
        int ans = 0;
        for (char c : s.toCharArray()) {
            var q = d[c - 'a'];
            for (int k = q.size(); k > 0; --k) {
                String t = q.pollFirst();
                if (t.length() == 1) {
                    ++ans;
                } else {
                    d[t.charAt(1) - 'a'].offer(t.substring(1));
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
    int numMatchingSubseq(string s, vector<string>& words) {
        vector<queue<string>> d(26);
        for (auto& w : words) d[w[0] - 'a'].emplace(w);
        int ans = 0;
        for (char& c : s) {
            auto& q = d[c - 'a'];
            for (int k = q.size(); k; --k) {
                auto t = q.front();
                q.pop();
                if (t.size() == 1)
                    ++ans;
                else
                    d[t[1] - 'a'].emplace(t.substr(1));
            }
        }
        return ans;
    }
};
```

#### Go

```go
func numMatchingSubseq(s string, words []string) (ans int) {
	d := [26][]string{}
	for _, w := range words {
		d[w[0]-'a'] = append(d[w[0]-'a'], w)
	}
	for _, c := range s {
		q := d[c-'a']
		d[c-'a'] = nil
		for _, t := range q {
			if len(t) == 1 {
				ans++
			} else {
				d[t[1]-'a'] = append(d[t[1]-'a'], t[1:])
			}
		}
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Phân nhóm (Con trỏ chỉ số)

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 tạo chuỗi con cho phần còn lại. Thay vào đó, lưu $(word\_index, matched\_len)$; vẫn dùng các nhóm tương tự nhưng giảm số lần cấp phát bộ nhớ.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numMatchingSubseq(self, s: str, words: List[str]) -> int:
        d = defaultdict(deque)
        for i, w in enumerate(words):
            d[w[0]].append((i, 0))
        ans = 0
        for c in s:
            for _ in range(len(d[c])):
                i, j = d[c].popleft()
                j += 1
                if j == len(words[i]):
                    ans += 1
                else:
                    d[words[i][j]].append((i, j))
        return ans
```

#### Java

```java
class Solution {
    public int numMatchingSubseq(String s, String[] words) {
        Deque<int[]>[] d = new Deque[26];
        Arrays.setAll(d, k -> new ArrayDeque<>());
        for (int i = 0; i < words.length; ++i) {
            d[words[i].charAt(0) - 'a'].offer(new int[] {i, 0});
        }
        int ans = 0;
        for (char c : s.toCharArray()) {
            var q = d[c - 'a'];
            for (int t = q.size(); t > 0; --t) {
                var p = q.pollFirst();
                int i = p[0], j = p[1] + 1;
                if (j == words[i].length()) {
                    ++ans;
                } else {
                    d[words[i].charAt(j) - 'a'].offer(new int[] {i, j});
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
    int numMatchingSubseq(string s, vector<string>& words) {
        vector<queue<pair<int, int>>> d(26);
        for (int i = 0; i < words.size(); ++i) d[words[i][0] - 'a'].emplace(i, 0);
        int ans = 0;
        for (char& c : s) {
            auto& q = d[c - 'a'];
            for (int t = q.size(); t; --t) {
                auto [i, j] = q.front();
                q.pop();
                if (++j == words[i].size())
                    ++ans;
                else
                    d[words[i][j] - 'a'].emplace(i, j);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func numMatchingSubseq(s string, words []string) (ans int) {
	type pair struct{ i, j int }
	d := [26][]pair{}
	for i, w := range words {
		d[w[0]-'a'] = append(d[w[0]-'a'], pair{i, 0})
	}
	for _, c := range s {
		q := d[c-'a']
		d[c-'a'] = nil
		for _, p := range q {
			i, j := p.i, p.j+1
			if j == len(words[i]) {
				ans++
			} else {
				d[words[i][j]-'a'] = append(d[words[i][j]-'a'], pair{i, j})
			}
		}
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Cách phân nhóm giữ các từ đang xử lý trong các nhóm. Một cách khác là lưu mọi chỉ số xuất hiện của từng ký tự trong $s$, rồi tìm kiếm nhị phân vị trí tiếp theo cho mỗi ký tự của $w$.
>
> Mỗi lần kiểm tra tốn $O(|w|\log n)$ và không cần dùng queue.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numMatchingSubseq(self, s: str, words: List[str]) -> int:
        def check(w):
            i = -1
            for c in w:
                j = bisect_right(d[c], i)
                if j == len(d[c]):
                    return False
                i = d[c][j]
            return True

        d = defaultdict(list)
        for i, c in enumerate(s):
            d[c].append(i)
        return sum(check(w) for w in words)
```

#### Java

```java
class Solution {
    private List<Integer>[] d = new List[26];

    public int numMatchingSubseq(String s, String[] words) {
        Arrays.setAll(d, k -> new ArrayList<>());
        for (int i = 0; i < s.length(); ++i) {
            d[s.charAt(i) - 'a'].add(i);
        }
        int ans = 0;
        for (String w : words) {
            if (check(w)) {
                ++ans;
            }
        }
        return ans;
    }

    private boolean check(String w) {
        int i = -1;
        for (int k = 0; k < w.length(); ++k) {
            int c = w.charAt(k) - 'a';
            int j = search(d[c], i);
            if (j == d[c].size()) {
                return false;
            }
            i = d[c].get(j);
        }
        return true;
    }

    private int search(List<Integer> t, int x) {
        int left = 0, right = t.size();
        while (left < right) {
            int mid = (left + right) >> 1;
            if (t.get(mid) > x) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numMatchingSubseq(string s, vector<string>& words) {
        vector<vector<int>> d(26);
        for (int i = 0; i < s.size(); ++i) d[s[i] - 'a'].emplace_back(i);
        int ans = 0;
        auto check = [&](string& w) {
            int i = -1;
            for (char& c : w) {
                auto& t = d[c - 'a'];
                int j = upper_bound(t.begin(), t.end(), i) - t.begin();
                if (j == t.size()) return false;
                i = t[j];
            }
            return true;
        };
        for (auto& w : words) ans += check(w);
        return ans;
    }
};
```

#### Go

```go
func numMatchingSubseq(s string, words []string) (ans int) {
	d := [26][]int{}
	for i, c := range s {
		d[c-'a'] = append(d[c-'a'], i)
	}
	check := func(w string) bool {
		i := -1
		for _, c := range w {
			t := d[c-'a']
			j := sort.SearchInts(t, i+1)
			if j == len(t) {
				return false
			}
			i = t[j]
		}
		return true
	}
	for _, w := range words {
		if check(w) {
			ans++
		}
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
