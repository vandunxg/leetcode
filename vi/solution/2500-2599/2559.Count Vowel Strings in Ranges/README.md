---
comments: true
difficulty: Medium
rating: 1435
source: Weekly Contest 331 Q2
tags:
    - Array
    - String
    - Prefix Sum
---

<!-- problem:start -->

# [2559. Count Vowel Strings in Ranges](https://leetcode.com/problems/count-vowel-strings-in-ranges)

[中文文档](/solution/2500-2599/2559.Count%20Vowel%20Strings%20in%20Ranges/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng chuỗi <strong>được đánh chỉ số từ 0</strong> <code>words</code> và một mảng số nguyên 2 chiều <code>queries</code>.</p>

<p>Mỗi truy vấn <code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>]</code> yêu cầu tìm số chuỗi trong <code>words</code> có chỉ số từ <code>l<sub>i</sub></code> đến <code>r<sub>i</sub></code> (<strong>bao gồm cả hai đầu mút</strong>) bắt đầu và kết thúc bằng một nguyên âm.</p>

<p>Trả về <em>một mảng </em><code>ans</code><em> có kích thước </em><code>queries.length</code><em>, trong đó </em><code>ans[i]</code><em> là đáp án của truy vấn thứ </em><code>i</code><sup>th</sup><em>.</em></p>

<p><strong>Lưu ý</strong> rằng các chữ cái nguyên âm là <code>&#39;a&#39;</code>, <code>&#39;e&#39;</code>, <code>&#39;i&#39;</code>, <code>&#39;o&#39;</code> và <code>&#39;u&#39;</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;aba&quot;,&quot;bcb&quot;,&quot;ece&quot;,&quot;aa&quot;,&quot;e&quot;], queries = [[0,2],[1,4],[1,1]]
<strong>Đầu ra:</strong> [2,3,0]
<strong>Giải thích:</strong> Các chuỗi bắt đầu và kết thúc bằng một nguyên âm là &quot;aba&quot;, &quot;ece&quot;, &quot;aa&quot; và &quot;e&quot;.
Đáp án của truy vấn [0,2] là 2 (các chuỗi &quot;aba&quot; và &quot;ece&quot;).
Đáp án của truy vấn [1,4] là 3 (các chuỗi &quot;ece&quot;, &quot;aa&quot;, &quot;e&quot;).
Đáp án của truy vấn [1,1] là 0.
Ta trả về [2,3,0].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;a&quot;,&quot;e&quot;,&quot;i&quot;], queries = [[0,2],[0,1],[2,2]]
<strong>Đầu ra:</strong> [3,2,1]
<strong>Giải thích:</strong> Mọi chuỗi đều thỏa mãn điều kiện, nên ta trả về [3,2,1].</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= words[i].length &lt;= 40</code></li>
	<li><code>words[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>sum(words[i].length) &lt;= 3 * 10<sup>5</sup></code></li>
	<li><code>1 &lt;= queries.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= l<sub>i</sub> &lt;= r<sub>i</sub> &lt;&nbsp;words.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tiền xử lý + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi truy vấn yêu cầu đếm số chuỗi trong một đoạn bắt đầu và kết thúc bằng nguyên âm. Việc kiểm tra từng chuỗi cho mỗi truy vấn là quá chậm khi $n,q\le 10^5$.
>
> Thu thập các chỉ số thỏa mãn theo thứ tự. Khi đó, một truy vấn trở thành việc đếm các chỉ số nằm trong $[l,r]$, tức là lấy hiệu của hai lần tìm kiếm nhị phân.

<!-- thinking:end -->

Ta có thể tiền xử lý tất cả chỉ số của các chuỗi bắt đầu và kết thúc bằng nguyên âm, rồi lưu chúng theo thứ tự vào mảng $nums$.

Tiếp theo, ta duyệt qua từng truy vấn $(l, r)$ và dùng tìm kiếm nhị phân để tìm chỉ số đầu tiên $i$ trong $nums$ lớn hơn hoặc bằng $l$, cùng chỉ số đầu tiên $j$ lớn hơn $r$. Vì vậy, đáp án của truy vấn hiện tại là $j - i$.

Độ phức tạp thời gian là $O(n + m \times \log n)$, còn độ phức tạp không gian là $O(n)$. Trong đó, $n$ và $m$ lần lượt là độ dài của các mảng $words$ và $queries$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def vowelStrings(self, words: List[str], queries: List[List[int]]) -> List[int]:
        vowels = set("aeiou")
        nums = [i for i, w in enumerate(words) if w[0] in vowels and w[-1] in vowels]
        return [bisect_right(nums, r) - bisect_left(nums, l) for l, r in queries]
```

#### Java

```java
class Solution {
    private List<Integer> nums = new ArrayList<>();

    public int[] vowelStrings(String[] words, int[][] queries) {
        Set<Character> vowels = Set.of('a', 'e', 'i', 'o', 'u');
        for (int i = 0; i < words.length; ++i) {
            char a = words[i].charAt(0), b = words[i].charAt(words[i].length() - 1);
            if (vowels.contains(a) && vowels.contains(b)) {
                nums.add(i);
            }
        }
        int m = queries.length;
        int[] ans = new int[m];
        for (int i = 0; i < m; ++i) {
            int l = queries[i][0], r = queries[i][1];
            ans[i] = search(r + 1) - search(l);
        }
        return ans;
    }

    private int search(int x) {
        int l = 0, r = nums.size();
        while (l < r) {
            int mid = (l + r) >> 1;
            if (nums.get(mid) >= x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> vowelStrings(vector<string>& words, vector<vector<int>>& queries) {
        unordered_set<char> vowels = {'a', 'e', 'i', 'o', 'u'};
        vector<int> nums;
        for (int i = 0; i < words.size(); ++i) {
            char a = words[i][0], b = words[i].back();
            if (vowels.count(a) && vowels.count(b)) {
                nums.push_back(i);
            }
        }
        vector<int> ans;
        for (auto& q : queries) {
            int l = q[0], r = q[1];
            int cnt = upper_bound(nums.begin(), nums.end(), r) - lower_bound(nums.begin(), nums.end(), l);
            ans.push_back(cnt);
        }
        return ans;
    }
};
```

#### Go

```go
func vowelStrings(words []string, queries [][]int) []int {
	vowels := map[byte]bool{'a': true, 'e': true, 'i': true, 'o': true, 'u': true}
	nums := []int{}
	for i, w := range words {
		if vowels[w[0]] && vowels[w[len(w)-1]] {
			nums = append(nums, i)
		}
	}
	ans := make([]int, len(queries))
	for i, q := range queries {
		l, r := q[0], q[1]
		ans[i] = sort.SearchInts(nums, r+1) - sort.SearchInts(nums, l)
	}
	return ans
}
```

#### TypeScript

```ts
function vowelStrings(words: string[], queries: number[][]): number[] {
    const vowels = new Set(['a', 'e', 'i', 'o', 'u']);
    const nums: number[] = [];
    for (let i = 0; i < words.length; ++i) {
        if (vowels.has(words[i][0]) && vowels.has(words[i][words[i].length - 1])) {
            nums.push(i);
        }
    }
    const search = (x: number): number => {
        let l = 0,
            r = nums.length;
        while (l < r) {
            const mid = (l + r) >> 1;
            if (nums[mid] >= x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    };
    return queries.map(([l, r]) => search(r + 1) - search(l));
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Tổng tiền tố

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 vẫn tốn một thừa số logarit cho mỗi truy vấn. Một tổng tiền tố của chỉ báo $0/1$ cho chuỗi nguyên âm có thể trả lời mỗi đoạn bằng $s[r+1]-s[l]$ trong thời gian không đổi.

<!-- thinking:end -->

Ta có thể tạo một mảng tổng tiền tố $s$ có độ dài $n+1$, trong đó $s[i]$ biểu diễn số chuỗi bắt đầu và kết thúc bằng nguyên âm trong $i$ chuỗi đầu tiên của mảng $words$. Ban đầu, $s[0] = 0$.

Tiếp theo, ta duyệt qua mảng $words$. Nếu chuỗi hiện tại bắt đầu và kết thúc bằng nguyên âm, thì $s[i+1] = s[i] + 1$; ngược lại, $s[i+1] = s[i]$.

Cuối cùng, ta duyệt qua từng truy vấn $(l, r)$. Khi đó, đáp án của truy vấn hiện tại là $s[r+1] - s[l]$.

Độ phức tạp thời gian là $O(n + m)$, còn độ phức tạp không gian là $O(n)$. Trong đó, $n$ và $m$ lần lượt là độ dài của các mảng $words$ và $queries$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def vowelStrings(self, words: List[str], queries: List[List[int]]) -> List[int]:
        vowels = set("aeiou")
        s = list(
            accumulate(
                (int(w[0] in vowels and w[-1] in vowels) for w in words), initial=0
            )
        )
        return [s[r + 1] - s[l] for l, r in queries]
```

#### Java

```java
class Solution {
    public int[] vowelStrings(String[] words, int[][] queries) {
        Set<Character> vowels = Set.of('a', 'e', 'i', 'o', 'u');
        int n = words.length;
        int[] s = new int[n + 1];
        for (int i = 0; i < n; ++i) {
            char a = words[i].charAt(0), b = words[i].charAt(words[i].length() - 1);
            s[i + 1] = s[i] + (vowels.contains(a) && vowels.contains(b) ? 1 : 0);
        }
        int m = queries.length;
        int[] ans = new int[m];
        for (int i = 0; i < m; ++i) {
            int l = queries[i][0], r = queries[i][1];
            ans[i] = s[r + 1] - s[l];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> vowelStrings(vector<string>& words, vector<vector<int>>& queries) {
        unordered_set<char> vowels = {'a', 'e', 'i', 'o', 'u'};
        int n = words.size();
        int s[n + 1];
        s[0] = 0;
        for (int i = 0; i < n; ++i) {
            char a = words[i][0], b = words[i].back();
            s[i + 1] = s[i] + (vowels.count(a) && vowels.count(b));
        }
        vector<int> ans;
        for (auto& q : queries) {
            int l = q[0], r = q[1];
            ans.push_back(s[r + 1] - s[l]);
        }
        return ans;
    }
};
```

#### Go

```go
func vowelStrings(words []string, queries [][]int) []int {
	vowels := map[byte]bool{'a': true, 'e': true, 'i': true, 'o': true, 'u': true}
	n := len(words)
	s := make([]int, n+1)
	for i, w := range words {
		x := 0
		if vowels[w[0]] && vowels[w[len(w)-1]] {
			x = 1
		}
		s[i+1] = s[i] + x
	}
	ans := make([]int, len(queries))
	for i, q := range queries {
		l, r := q[0], q[1]
		ans[i] = s[r+1] - s[l]
	}
	return ans
}
```

#### TypeScript

```ts
function vowelStrings(words: string[], queries: number[][]): number[] {
    const vowels = new Set(['a', 'e', 'i', 'o', 'u']);
    const s = new Array(words.length + 1).fill(0);

    words.forEach((w, i) => {
        const x = +(vowels.has(w[0]) && vowels.has(w.at(-1)!));
        s[i + 1] = s[i] + x;
    });

    return queries.map(([l, r]) => s[r + 1] - s[l]);
}
```

#### JavaScript

```js
function vowelStrings(words, queries) {
    const vowels = new Set(['a', 'e', 'i', 'o', 'u']);
    const s = new Array(words.length + 1).fill(0);

    words.forEach((w, i) => {
        const x = +(vowels.has(w[0]) && vowels.has(w.at(-1)));
        s[i + 1] = s[i] + x;
    });

    return queries.map(([l, r]) => s[r + 1] - s[l]);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
