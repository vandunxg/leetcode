---
comments: true
difficulty: Hard
rating: 2499
source: Weekly Contest 278 Q4
tags:
    - Bit Manipulation
    - Union Find
    - Array
    - Hash Table
    - String
---

<!-- problem:start -->

# [2157. Groups of Strings](https://leetcode.com/problems/groups-of-strings)

[中文文档](/solution/2100-2199/2157.Groups%20of%20Strings/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng chuỗi <code>words</code> được đánh chỉ số từ <strong>0</strong>. Mỗi chuỗi chỉ gồm các <strong>chữ cái tiếng Anh viết thường</strong>. Không chữ cái nào xuất hiện quá một lần trong bất kỳ chuỗi nào của <code>words</code>.</p>

<p>Hai chuỗi <code>s1</code> và <code>s2</code> được gọi là <strong>kết nối</strong> nếu có thể thu được tập hợp các chữ cái của <code>s2</code> từ tập hợp các chữ cái của <code>s1</code> bằng <strong>một</strong> trong các phép toán sau:</p>

<ul>
	<li>Thêm đúng một chữ cái vào tập hợp các chữ cái của <code>s1</code>.</li>
	<li>Xóa đúng một chữ cái khỏi tập hợp các chữ cái của <code>s1</code>.</li>
	<li>Thay một chữ cái trong tập hợp các chữ cái của <code>s1</code> bằng một chữ cái bất kỳ, <strong>kể cả chính nó</strong>.</li>
</ul>

<p>Mảng <code>words</code> có thể được chia thành một hoặc nhiều <strong>nhóm</strong> không giao nhau. Một chuỗi thuộc về một nhóm nếu thỏa mãn <strong>một</strong> trong các điều kiện sau:</p>

<ul>
	<li>Nó được kết nối với <strong>ít nhất một</strong> chuỗi khác trong nhóm.</li>
	<li>Nó là chuỗi <strong>duy nhất</strong> trong nhóm.</li>
</ul>

<p>Lưu ý rằng các chuỗi trong <code>words</code> phải được nhóm sao cho một chuỗi thuộc một nhóm không thể được kết nối với một chuỗi thuộc bất kỳ nhóm nào khác. Có thể chứng minh rằng cách chia như vậy luôn là duy nhất.</p>

<p>Hãy trả về <em>một mảng</em> <code>ans</code> <em>có kích thước</em> <code>2</code> <em>trong đó:</em></p>

<ul>
	<li><code>ans[0]</code> <em>là <strong>số lượng nhóm lớn nhất</strong> mà mảng</em> <code>words</code> <em>có thể được chia thành, và</em></li>
	<li><code>ans[1]</code> <em>là <strong>kích thước của nhóm lớn nhất</strong>.</em></li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;a&quot;,&quot;b&quot;,&quot;ab&quot;,&quot;cde&quot;]
<strong>Đầu ra:</strong> [2,3]
<strong>Giải thích:</strong>
- Có thể dùng words[0] để thu được words[1] (bằng cách thay &#39;a&#39; bằng &#39;b&#39;) và words[2] (bằng cách thêm &#39;b&#39;). Vì vậy, words[0] được kết nối với words[1] và words[2].
- Có thể dùng words[1] để thu được words[0] (bằng cách thay &#39;b&#39; bằng &#39;a&#39;) và words[2] (bằng cách thêm &#39;a&#39;). Vì vậy, words[1] được kết nối với words[0] và words[2].
- Có thể dùng words[2] để thu được words[0] (bằng cách xóa &#39;b&#39;) và words[1] (bằng cách xóa &#39;a&#39;). Vì vậy, words[2] được kết nối với words[0] và words[1].
- words[3] không được kết nối với chuỗi nào trong words.
Do đó, words có thể được chia thành 2 nhóm [&quot;a&quot;,&quot;b&quot;,&quot;ab&quot;] và [&quot;cde&quot;]. Kích thước của nhóm lớn nhất là 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;a&quot;,&quot;ab&quot;,&quot;abc&quot;]
<strong>Đầu ra:</strong> [1,3]
<strong>Giải thích:</strong>
- words[0] được kết nối với words[1].
- words[1] được kết nối với words[0] và words[2].
- words[2] được kết nối với words[1].
Vì tất cả các chuỗi đều được kết nối với nhau, chúng phải được xếp vào cùng một nhóm.
Do đó, kích thước của nhóm lớn nhất là 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= words[i].length &lt;= 26</code></li>
	<li><code>words[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li>Không chữ cái nào xuất hiện quá một lần trong <code>words[i]</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Hai từ có liên hệ nếu có thể biến từ này thành từ kia bằng một lần thêm, xóa hoặc thay thế. Ta biểu diễn các từ bằng mask 26 bit; nếu so sánh từng cặp chuỗi để tìm các hàng xóm thì quá chậm. Với $n\le 2\times 10^4$, ta hợp nhất các mask.
>
> Với mỗi mask, ta hợp nhất mask đó với mọi mask khác đúng một bit (thêm/xóa) và mọi mask khác bằng cách “xóa một bit rồi bật một bit khác” (thay thế). Các mask trùng nhau đã thuộc cùng một thành phần và làm tăng kích thước của thành phần đó.
>
> Duyệt các mask hiện có, hợp nhất với các hàng xóm, đồng thời theo dõi số thành phần và kích thước lớn nhất.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def groupStrings(self, words: List[str]) -> List[int]:
        def find(x):
            if p[x] != x:
                p[x] = find(p[x])
            return p[x]

        def union(a, b):
            nonlocal mx, n
            if b not in p:
                return
            pa, pb = find(a), find(b)
            if pa == pb:
                return
            p[pa] = pb
            size[pb] += size[pa]
            mx = max(mx, size[pb])
            n -= 1

        p = {}
        size = Counter()
        n = len(words)
        mx = 0
        for word in words:
            x = 0
            for c in word:
                x |= 1 << (ord(c) - ord('a'))
            p[x] = x
            size[x] += 1
            mx = max(mx, size[x])
            if size[x] > 1:
                n -= 1
        for x in p.keys():
            for i in range(26):
                union(x, x ^ (1 << i))
                if (x >> i) & 1:
                    for j in range(26):
                        if ((x >> j) & 1) == 0:
                            union(x, x ^ (1 << i) | (1 << j))
        return [n, mx]
```

#### Java

```java
class Solution {
    private Map<Integer, Integer> p;
    private Map<Integer, Integer> size;
    private int mx;
    private int n;

    public int[] groupStrings(String[] words) {
        p = new HashMap<>();
        size = new HashMap<>();
        n = words.length;
        mx = 0;
        for (String word : words) {
            int x = 0;
            for (char c : word.toCharArray()) {
                x |= 1 << (c - 'a');
            }
            p.put(x, x);
            size.put(x, size.getOrDefault(x, 0) + 1);
            mx = Math.max(mx, size.get(x));
            if (size.get(x) > 1) {
                --n;
            }
        }
        for (int x : p.keySet()) {
            for (int i = 0; i < 26; ++i) {
                union(x, x ^ (1 << i));
                if (((x >> i) & 1) != 0) {
                    for (int j = 0; j < 26; ++j) {
                        if (((x >> j) & 1) == 0) {
                            union(x, x ^ (1 << i) | (1 << j));
                        }
                    }
                }
            }
        }
        return new int[] {n, mx};
    }

    private int find(int x) {
        if (p.get(x) != x) {
            p.put(x, find(p.get(x)));
        }
        return p.get(x);
    }

    private void union(int a, int b) {
        if (!p.containsKey(b)) {
            return;
        }
        int pa = find(a), pb = find(b);
        if (pa == pb) {
            return;
        }
        p.put(pa, pb);
        size.put(pb, size.get(pb) + size.get(pa));
        mx = Math.max(mx, size.get(pb));
        --n;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int mx, n;

    vector<int> groupStrings(vector<string>& words) {
        unordered_map<int, int> p;
        unordered_map<int, int> size;
        mx = 0;
        n = words.size();
        for (auto& word : words) {
            int x = 0;
            for (auto& c : word) x |= 1 << (c - 'a');
            p[x] = x;
            ++size[x];
            mx = max(mx, size[x]);
            if (size[x] > 1) --n;
        }
        for (auto& [x, _] : p) {
            for (int i = 0; i < 26; ++i) {
                unite(x, x ^ (1 << i), p, size);
                if ((x >> i) & 1) {
                    for (int j = 0; j < 26; ++j) {
                        if (((x >> j) & 1) == 0) unite(x, x ^ (1 << i) | (1 << j), p, size);
                    }
                }
            }
        }
        return {n, mx};
    }

    int find(int x, unordered_map<int, int>& p) {
        if (p[x] != x) p[x] = find(p[x], p);
        return p[x];
    }

    void unite(int a, int b, unordered_map<int, int>& p, unordered_map<int, int>& size) {
        if (!p.count(b)) return;
        int pa = find(a, p), pb = find(b, p);
        if (pa == pb) return;
        p[pa] = pb;
        size[pb] += size[pa];
        mx = max(mx, size[pb]);
        --n;
    }
};
```

#### Go

```go
func groupStrings(words []string) []int {
	p := map[int]int{}
	size := map[int]int{}
	mx, n := 0, len(words)
	var find func(int) int
	find = func(x int) int {
		if p[x] != x {
			p[x] = find(p[x])
		}
		return p[x]
	}
	union := func(a, b int) {
		if _, ok := p[b]; !ok {
			return
		}
		pa, pb := find(a), find(b)
		if pa == pb {
			return
		}
		p[pa] = pb
		size[pb] += size[pa]
		mx = max(mx, size[pb])
		n--
	}

	for _, word := range words {
		x := 0
		for _, c := range word {
			x |= 1 << (c - 'a')
		}
		p[x] = x
		size[x]++
		mx = max(mx, size[x])
		if size[x] > 1 {
			n--
		}
	}
	for x := range p {
		for i := 0; i < 26; i++ {
			union(x, x^(1<<i))
			if ((x >> i) & 1) != 0 {
				for j := 0; j < 26; j++ {
					if ((x >> j) & 1) == 0 {
						union(x, x^(1<<i)|(1<<j))
					}
				}
			}
		}
	}
	return []int{n, mx}
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
