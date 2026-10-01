---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [17.11. Find Closest](https://leetcode.cn/problems/find-closest-lcci)

[中文文档](/lcci/17.11.Find%20Closest/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có một tệp văn bản lớn chứa các từ. Với hai từ bất kỳ, hãy tìm khoảng cách ngắn nhất (tính theo số từ) giữa chúng trong tệp. Nếu thao tác này sẽ được lặp lại nhiều lần trên cùng một tệp (nhưng với các cặp từ khác nhau), bạn có thể tối ưu lời giải không?</p>

<p><strong>Ví dụ: </strong></p>

<pre>

<strong>Đầu vào: </strong>words = [&quot;I&quot;,&quot;am&quot;,&quot;a&quot;,&quot;student&quot;,&quot;from&quot;,&quot;a&quot;,&quot;university&quot;,&quot;in&quot;,&quot;a&quot;,&quot;city&quot;], word1 = &quot;a&quot;, word2 = &quot;student&quot;

<strong>Đầu ra: </strong>1</pre>

<p>Lưu ý:</p>

<ul>
	<li><code>words.length &lt;= 100000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt một lượt

<!-- thinking:start -->

> **Tư duy**
>
> Khoảng cách gần nhất giữa hai từ. Ghép mọi chỉ số của mỗi từ có thể dẫn đến độ phức tạp bậc hai.
>
> Cặp gần nhất luôn sử dụng một lần xuất hiện của một từ và lần xuất hiện gần nhất của từ còn lại, nên chỉ cần duyệt một lượt.
>
> $i$ và $j$ lưu các chỉ số gần nhất; mỗi bước cập nhật $|i-j|$. Chỉ giữ lại lần xuất hiện gần nhất của mỗi từ.

<!-- thinking:end -->

Chúng ta dùng hai con trỏ $i$ và $j$ để ghi lại các lần xuất hiện gần nhất của hai từ $\textit{word1}$ và $\textit{word2}$ tương ứng. Ban đầu, $i = \infty$ và $j = -\infty$.

Tiếp theo, chúng ta duyệt toàn bộ tệp văn bản. Với mỗi từ $w$, nếu $w$ bằng $\textit{word1}$, chúng ta cập nhật $i = k$, trong đó $k$ là chỉ số của từ hiện tại; nếu $w$ bằng $\textit{word2}$, chúng ta cập nhật $j = k$. Sau đó, chúng ta cập nhật đáp án $ans = \min(ans, |i - j|)$.

Sau khi duyệt xong, chúng ta trả về đáp án $ans$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là số từ trong tệp văn bản. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findClosest(self, words: List[str], word1: str, word2: str) -> int:
        i, j = inf, -inf
        ans = inf
        for k, w in enumerate(words):
            if w == word1:
                i = k
            elif w == word2:
                j = k
            ans = min(ans, abs(i - j))
        return ans
```

#### Java

```java
class Solution {
    public int findClosest(String[] words, String word1, String word2) {
        final int inf = 1 << 29;
        int i = inf, j = -inf, ans = inf;
        for (int k = 0; k < words.length; ++k) {
            if (words[k].equals(word1)) {
                i = k;
            } else if (words[k].equals(word2)) {
                j = k;
            }
            ans = Math.min(ans, Math.abs(i - j));
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findClosest(vector<string>& words, string word1, string word2) {
        const int inf = 1 << 29;
        int i = inf, j = -inf;
        int ans = inf;
        for (int k = 0; k < words.size(); ++k) {
            if (words[k] == word1) {
                i = k;
            } else if (words[k] == word2) {
                j = k;
            }
            ans = min(ans, abs(i - j));
        }
        return ans;
    }
};
```

#### Go

```go
func findClosest(words []string, word1 string, word2 string) int {
	const inf int = 1 << 29
	i, j, ans := inf, -inf, inf
	for k, w := range words {
		if w == word1 {
			i = k
		} else if w == word2 {
			j = k
		}
		ans = min(ans, max(i-j, j-i))
	}
	return ans
}
```

#### TypeScript

```ts
function findClosest(words: string[], word1: string, word2: string): number {
    let [i, j, ans] = [Infinity, -Infinity, Infinity];
    for (let k = 0; k < words.length; ++k) {
        if (words[k] === word1) {
            i = k;
        } else if (words[k] === word2) {
            j = k;
        }
        ans = Math.min(ans, Math.abs(i - j));
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn find_closest(words: Vec<String>, word1: String, word2: String) -> i32 {
        let mut ans = i32::MAX;
        let mut i = -1;
        let mut j = -1;
        for (k, w) in words.iter().enumerate() {
            let k = k as i32;
            if w.eq(&word1) {
                i = k;
            } else if w.eq(&word2) {
                j = k;
            }
            if i != -1 && j != -1 {
                ans = ans.min((i - j).abs());
            }
        }
        ans
    }
}
```

#### Swift

```swift
class Solution {
    func findClosest(_ words: [String], _ word1: String, _ word2: String) -> Int {
        let inf = Int.max / 2
        var i = inf
        var j = -inf
        var ans = inf

        for (k, word) in words.enumerated() {
            if word == word1 {
                i = k
            } else if word == word2 {
                j = k
            }
            ans = min(ans, abs(i - j))
        }

        return ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Bảng băm + Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Duyệt một lượt phù hợp với một truy vấn; nhiều truy vấn trên cùng văn bản sẽ khiến toàn bộ văn bản bị duyệt lại.
>
> Tiền xử lý các danh sách chỉ số, rồi dùng hai con trỏ trên hai danh sách đã sắp xếp cho mỗi truy vấn. Thời gian phụ thuộc vào số lần xuất hiện.

<!-- thinking:end -->

Chúng ta có thể dùng một bảng băm $d$ để ghi lại vị trí của mỗi từ. Sau đó, với mỗi cặp $\textit{word1}$ và $\textit{word2}$, chúng ta có thể tìm khoảng cách ngắn nhất giữa chúng bằng phương pháp hai con trỏ.

Chúng ta duyệt toàn bộ tệp văn bản. Với mỗi từ $w$, chúng ta thêm chỉ số của $w$ vào $d[w]$.

Tiếp theo, chúng ta tìm các vị trí mà $\textit{word1}$ và $\textit{word2}$ xuất hiện trong tệp văn bản, lần lượt biểu diễn bằng $idx1$ và $idx2$. Sau đó, chúng ta dùng hai con trỏ $i$ và $j$ lần lượt trỏ đến $idx1$ và $idx2$, với giá trị ban đầu là $i = 0$, $j = 0$.

Tiếp theo, chúng ta duyệt $idx1$ và $idx2$. Mỗi lần, chúng ta cập nhật đáp án $ans = \min(ans, |idx1[i] - idx2[j]|)$, sau đó lùi con trỏ nhỏ hơn trong $i$ và $j$ lại một bước.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là số từ trong tệp văn bản.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findClosest(self, words: List[str], word1: str, word2: str) -> int:
        d = defaultdict(list)
        for i, w in enumerate(words):
            d[w].append(i)
        ans = inf
        idx1, idx2 = d[word1], d[word2]
        i, j, m, n = 0, 0, len(idx1), len(idx2)
        while i < m and j < n:
            ans = min(ans, abs(idx1[i] - idx2[j]))
            if idx1[i] < idx2[j]:
                i += 1
            else:
                j += 1
        return ans
```

#### Java

```java
class Solution {
    public int findClosest(String[] words, String word1, String word2) {
        Map<String, List<Integer>> d = new HashMap<>();
        for (int i = 0; i < words.length; ++i) {
            d.computeIfAbsent(words[i], k -> new ArrayList<>()).add(i);
        }
        List<Integer> idx1 = d.get(word1), idx2 = d.get(word2);
        int i = 0, j = 0, m = idx1.size(), n = idx2.size();
        int ans = 1 << 29;
        while (i < m && j < n) {
            int t = Math.abs(idx1.get(i) - idx2.get(j));
            ans = Math.min(ans, t);
            if (idx1.get(i) < idx2.get(j)) {
                ++i;
            } else {
                ++j;
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
    int findClosest(vector<string>& words, string word1, string word2) {
        unordered_map<string, vector<int>> d;
        for (int i = 0; i < words.size(); ++i) {
            d[words[i]].push_back(i);
        }
        vector<int> idx1 = d[word1], idx2 = d[word2];
        int i = 0, j = 0, m = idx1.size(), n = idx2.size();
        int ans = 1e5;
        while (i < m && j < n) {
            int t = abs(idx1[i] - idx2[j]);
            ans = min(ans, t);
            if (idx1[i] < idx2[j]) {
                ++i;
            } else {
                ++j;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findClosest(words []string, word1 string, word2 string) int {
	d := map[string][]int{}
	for i, w := range words {
		d[w] = append(d[w], i)
	}
	idx1, idx2 := d[word1], d[word2]
	i, j, m, n := 0, 0, len(idx1), len(idx2)
	ans := 1 << 30
	for i < m && j < n {
		t := max(idx1[i]-idx2[j], idx2[j]-idx1[i])
		if t < ans {
			ans = t
		}
		if idx1[i] < idx2[j] {
			i++
		} else {
			j++
		}
	}
	return ans
}
```

#### TypeScript

```ts
function findClosest(words: string[], word1: string, word2: string): number {
    const d: Map<string, number[]> = new Map();
    for (let i = 0; i < words.length; ++i) {
        if (!d.has(words[i])) {
            d.set(words[i], []);
        }
        d.get(words[i])!.push(i);
    }
    let [i, j] = [0, 0];
    let ans = Infinity;
    while (i < d.get(word1)!.length && j < d.get(word2)!.length) {
        ans = Math.min(ans, Math.abs(d.get(word1)![i] - d.get(word2)![j]));
        if (d.get(word1)![i] < d.get(word2)![j]) {
            ++i;
        } else {
            ++j;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
