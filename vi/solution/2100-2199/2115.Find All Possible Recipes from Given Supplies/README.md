---
comments: true
difficulty: Medium
rating: 1678
source: Biweekly Contest 68 Q2
tags:
    - Graph
    - Topological Sort
    - Array
    - Hash Table
    - String
    - Directed Acyclic Graph
---

<!-- problem:start -->

# [2115. Find All Possible Recipes from Given Supplies](https://leetcode.com/problems/find-all-possible-recipes-from-given-supplies)

[中文文档](/solution/2100-2199/2115.Find%20All%20Possible%20Recipes%20from%20Given%20Supplies/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có thông tin về <code>n</code> công thức khác nhau. Cho một mảng chuỗi <code>recipes</code> và một mảng chuỗi 2 chiều <code>ingredients</code>. Công thức thứ <code>i<sup>th</sup></code> có tên là <code>recipes[i]</code>, và bạn có thể <strong>tạo</strong> công thức đó nếu có <strong>tất cả</strong> nguyên liệu cần thiết trong <code>ingredients[i]</code>. Một công thức cũng có thể là nguyên liệu cho <strong>các </strong>công thức khác, nghĩa là <code>ingredients[i]</code> có thể chứa một chuỗi xuất hiện trong <code>recipes</code>.</p>

<p>Bạn cũng được cho một mảng chuỗi <code>supplies</code> chứa tất cả nguyên liệu hiện có ban đầu, và bạn có nguồn cung vô hạn cho tất cả các nguyên liệu này.</p>

<p>Trả về <em>danh sách tất cả các công thức mà bạn có thể tạo ra. </em>Bạn có thể trả về đáp án theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Lưu ý rằng hai công thức có thể chứa lẫn nhau trong danh sách nguyên liệu.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> recipes = [&quot;bread&quot;], ingredients = [[&quot;yeast&quot;,&quot;flour&quot;]], supplies = [&quot;yeast&quot;,&quot;flour&quot;,&quot;corn&quot;]
<strong>Đầu ra:</strong> [&quot;bread&quot;]
<strong>Giải thích:</strong>
Ta có thể tạo &quot;bread&quot; vì có các nguyên liệu &quot;yeast&quot; và &quot;flour&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> recipes = [&quot;bread&quot;,&quot;sandwich&quot;], ingredients = [[&quot;yeast&quot;,&quot;flour&quot;],[&quot;bread&quot;,&quot;meat&quot;]], supplies = [&quot;yeast&quot;,&quot;flour&quot;,&quot;meat&quot;]
<strong>Đầu ra:</strong> [&quot;bread&quot;,&quot;sandwich&quot;]
<strong>Giải thích:</strong>
Ta có thể tạo &quot;bread&quot; vì có các nguyên liệu &quot;yeast&quot; và &quot;flour&quot;.
Ta có thể tạo &quot;sandwich&quot; vì có nguyên liệu &quot;meat&quot; và có thể tạo nguyên liệu &quot;bread&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> recipes = [&quot;bread&quot;,&quot;sandwich&quot;,&quot;burger&quot;], ingredients = [[&quot;yeast&quot;,&quot;flour&quot;],[&quot;bread&quot;,&quot;meat&quot;],[&quot;sandwich&quot;,&quot;meat&quot;,&quot;bread&quot;]], supplies = [&quot;yeast&quot;,&quot;flour&quot;,&quot;meat&quot;]
<strong>Đầu ra:</strong> [&quot;bread&quot;,&quot;sandwich&quot;,&quot;burger&quot;]
<strong>Giải thích:</strong>
Ta có thể tạo &quot;bread&quot; vì có các nguyên liệu &quot;yeast&quot; và &quot;flour&quot;.
Ta có thể tạo &quot;sandwich&quot; vì có nguyên liệu &quot;meat&quot; và có thể tạo nguyên liệu &quot;bread&quot;.
Ta có thể tạo &quot;burger&quot; vì có nguyên liệu &quot;meat&quot; và có thể tạo các nguyên liệu &quot;bread&quot; và &quot;sandwich&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == recipes.length == ingredients.length</code></li>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>1 &lt;= ingredients[i].length, supplies.length &lt;= 100</code></li>
	<li><code>1 &lt;= recipes[i].length, ingredients[i][j].length, supplies[k].length &lt;= 10</code></li>
	<li><code>recipes[i], ingredients[i][j]</code> và <code>supplies[k]</code> chỉ chứa các chữ cái tiếng Anh viết thường.</li>
	<li>Tất cả giá trị của <code>recipes</code> và <code>supplies</code> khi gộp lại đều là duy nhất.</li>
	<li>Mỗi <code>ingredients[i]</code> không chứa giá trị trùng lặp.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một công thức có thể được tạo khi mọi nguyên liệu của nó có sẵn từ nguồn cung ban đầu hoặc từ các công thức đã tạo, tương ứng với một đồ thị phụ thuộc có hướng. Việc kiểm tra lại nguyên liệu của từng công thức sẽ lặp lại công việc và xử lý không đúng các chuỗi phụ thuộc.
>
> Với mỗi nguyên liệu, ta trỏ đến các công thức cần nguyên liệu đó, đồng thời đặt bậc vào bằng số nguyên liệu. Các nguồn cung là các đỉnh nguồn; khi bậc vào của một công thức giảm về 0, ta có thể tạo công thức đó và dùng nó để mở khóa các công thức khác.
>
> Ta xây dựng đồ thị này và chạy thuật toán Kahn bắt đầu từ $\textit{supplies}$, thêm một công thức vào đáp án khi bậc vào của nó giảm về 0.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findAllRecipes(
        self, recipes: List[str], ingredients: List[List[str]], supplies: List[str]
    ) -> List[str]:
        g = defaultdict(list)
        indeg = defaultdict(int)
        for a, b in zip(recipes, ingredients):
            for v in b:
                g[v].append(a)
            indeg[a] += len(b)
        q = supplies
        ans = []
        for i in q:
            for j in g[i]:
                indeg[j] -= 1
                if indeg[j] == 0:
                    ans.append(j)
                    q.append(j)
        return ans
```

#### Java

```java
class Solution {
    public List<String> findAllRecipes(
        String[] recipes, List<List<String>> ingredients, String[] supplies) {
        Map<String, List<String>> g = new HashMap<>();
        Map<String, Integer> indeg = new HashMap<>();
        for (int i = 0; i < recipes.length; ++i) {
            for (String v : ingredients.get(i)) {
                g.computeIfAbsent(v, k -> new ArrayList<>()).add(recipes[i]);
            }
            indeg.put(recipes[i], ingredients.get(i).size());
        }
        Deque<String> q = new ArrayDeque<>();
        for (String s : supplies) {
            q.offer(s);
        }
        List<String> ans = new ArrayList<>();
        while (!q.isEmpty()) {
            for (int n = q.size(); n > 0; --n) {
                String i = q.pollFirst();
                for (String j : g.getOrDefault(i, Collections.emptyList())) {
                    indeg.put(j, indeg.get(j) - 1);
                    if (indeg.get(j) == 0) {
                        ans.add(j);
                        q.offer(j);
                    }
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
    vector<string> findAllRecipes(vector<string>& recipes, vector<vector<string>>& ingredients, vector<string>& supplies) {
        unordered_map<string, vector<string>> g;
        unordered_map<string, int> indeg;
        for (int i = 0; i < recipes.size(); ++i) {
            for (auto& v : ingredients[i]) {
                g[v].push_back(recipes[i]);
            }
            indeg[recipes[i]] = ingredients[i].size();
        }
        queue<string> q;
        for (auto& s : supplies) {
            q.push(s);
        }
        vector<string> ans;
        while (!q.empty()) {
            for (int n = q.size(); n; --n) {
                auto i = q.front();
                q.pop();
                for (auto j : g[i]) {
                    if (--indeg[j] == 0) {
                        ans.push_back(j);
                        q.push(j);
                    }
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findAllRecipes(recipes []string, ingredients [][]string, supplies []string) []string {
	g := map[string][]string{}
	indeg := map[string]int{}
	for i, a := range recipes {
		for _, b := range ingredients[i] {
			g[b] = append(g[b], a)
		}
		indeg[a] = len(ingredients[i])
	}
	q := []string{}
	for _, s := range supplies {
		q = append(q, s)
	}
	ans := []string{}
	for len(q) > 0 {
		for n := len(q); n > 0; n-- {
			i := q[0]
			q = q[1:]
			for _, j := range g[i] {
				indeg[j]--
				if indeg[j] == 0 {
					ans = append(ans, j)
					q = append(q, j)
				}
			}
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
