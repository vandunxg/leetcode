---
comments: true
difficulty: Easy
rating: 1192
source: Weekly Contest 187 Q1
tags:
    - Array
    - Hash Table
    - String
---

<!-- problem:start -->

# [1436. Destination City](https://leetcode.com/problems/destination-city)

[Tài liệu tiếng Trung](/solution/1400-1499/1436.Destination%20City/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>paths</code>, trong đó <code>paths[i] = [cityA<sub>i</sub>, cityB<sub>i</sub>]</code> nghĩa là tồn tại một đường đi trực tiếp từ <code>cityA<sub>i</sub></code> đến <code>cityB<sub>i</sub></code>. <em>Hãy trả về thành phố đích, tức là thành phố không có đường đi nào đến một thành phố khác.</em></p>

<p>Đảm bảo đồ thị các đường đi tạo thành một đường thẳng không có vòng lặp, do đó sẽ có đúng một thành phố đích.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> paths = [[&quot;London&quot;,&quot;New York&quot;],[&quot;New York&quot;,&quot;Lima&quot;],[&quot;Lima&quot;,&quot;Sao Paulo&quot;]]
<strong>Đầu ra:</strong> &quot;Sao Paulo&quot;
<strong>Giải thích:</strong> Bắt đầu từ thành phố &quot;London&quot;, bạn sẽ đến thành phố &quot;Sao Paulo&quot;, là thành phố đích. Chuyến đi của bạn là: &quot;London&quot; -&gt; &quot;New York&quot; -&gt; &quot;Lima&quot; -&gt; &quot;Sao Paulo&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> paths = [[&quot;B&quot;,&quot;C&quot;],[&quot;D&quot;,&quot;B&quot;],[&quot;C&quot;,&quot;A&quot;]]
<strong>Đầu ra:</strong> &quot;A&quot;
<strong>Giải thích:</strong> Tất cả các chuyến đi có thể là:&nbsp;
&quot;D&quot; -&gt; &quot;B&quot; -&gt; &quot;C&quot; -&gt; &quot;A&quot;.&nbsp;
&quot;B&quot; -&gt; &quot;C&quot; -&gt; &quot;A&quot;.&nbsp;
&quot;C&quot; -&gt; &quot;A&quot;.&nbsp;
&quot;A&quot;.&nbsp;
Rõ ràng thành phố đích là &quot;A&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> paths = [[&quot;A&quot;,&quot;Z&quot;]]
<strong>Đầu ra:</strong> &quot;Z&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= paths.length &lt;= 100</code></li>
	<li><code>paths[i].length == 2</code></li>
	<li><code>1 &lt;= cityA<sub>i</sub>.length, cityB<sub>i</sub>.length &lt;= 10</code></li>
	<li><code>cityA<sub>i</sub> != cityB<sub>i</sub></code></li>
	<li>Tất cả các chuỗi chỉ gồm chữ cái tiếng Anh viết thường, viết hoa và ký tự khoảng trắng.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Các đường đi tạo thành một chuỗi kết thúc tại thành phố có bậc ra bằng không. $n\le 100$. Đưa mọi thành phố bắt đầu vào một set, rồi trả về thành phố kết thúc duy nhất không nằm trong set.

<!-- thinking:end -->

Theo mô tả bài toán, thành phố đích sẽ không xuất hiện trong bất kỳ $\textit{cityA}$ nào. Vì vậy, trước tiên ta duyệt qua $\textit{paths}$ và đưa tất cả $\textit{cityA}$ vào một set $\textit{s}$. Sau đó, ta duyệt qua $\textit{paths}$ lần nữa để tìm $\textit{cityB}$ không nằm trong $\textit{s}$.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của $\textit{paths}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def destCity(self, paths: List[List[str]]) -> str:
        s = {a for a, _ in paths}
        return next(b for _, b in paths if b not in s)
```

#### Java

```java
class Solution {
    public String destCity(List<List<String>> paths) {
        Set<String> s = new HashSet<>();
        for (var p : paths) {
            s.add(p.get(0));
        }
        for (int i = 0;; ++i) {
            var b = paths.get(i).get(1);
            if (!s.contains(b)) {
                return b;
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    string destCity(vector<vector<string>>& paths) {
        unordered_set<string> s;
        for (auto& p : paths) {
            s.insert(p[0]);
        }
        for (int i = 0;; ++i) {
            auto b = paths[i][1];
            if (!s.contains(b)) {
                return b;
            }
        }
    }
};
```

#### Go

```go
func destCity(paths [][]string) string {
	s := map[string]bool{}
	for _, p := range paths {
		s[p[0]] = true
	}
	for _, p := range paths {
		if !s[p[1]] {
			return p[1]
		}
	}
	return ""
}
```

#### TypeScript

```ts
function destCity(paths: string[][]): string {
    const s = new Set<string>(paths.map(([a, _]) => a));
    return paths.find(([_, b]) => !s.has(b))![1];
}
```

#### Rust

```rust
use std::collections::HashSet;

impl Solution {
    pub fn dest_city(paths: Vec<Vec<String>>) -> String {
        let s = paths
            .iter()
            .map(|p| p[0].clone())
            .collect::<HashSet<String>>();
        paths.into_iter().find(|p| !s.contains(&p[1])).unwrap()[1].clone()
    }
}
```

#### JavaScript

```js
/**
 * @param {string[][]} paths
 * @return {string}
 */
var destCity = function (paths) {
    const s = new Set(paths.map(([a, _]) => a));
    return paths.find(([_, b]) => !s.has(b))[1];
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
