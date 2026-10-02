---
comments: true
difficulty: Easy
rating: 1508
source: Weekly Contest 195 Q1
tags:
    - Hash Table
    - String
---

<!-- problem:start -->

# [1496. Path Crossing](https://leetcode.com/problems/path-crossing)

[中文文档](/solution/1400-1499/1496.Path%20Crossing/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>path</code>, trong đó <code>path[i] = &#39;N&#39;</code>, <code>&#39;S&#39;</code>, <code>&#39;E&#39;</code> hoặc <code>&#39;W&#39;</code>, lần lượt biểu thị việc di chuyển một đơn vị về phía bắc, nam, đông hoặc tây. Bạn bắt đầu từ gốc tọa độ <code>(0, 0)</code> trên mặt phẳng 2D và đi theo đường đi được chỉ định bởi <code>path</code>.</p>

<p>Trả về <code>true</code> <em>nếu đường đi tự cắt chính nó tại bất kỳ thời điểm nào, tức là nếu tại một thời điểm bạn đi qua một vị trí đã từng ghé thăm</em>. Nếu không, trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1496.Path%20Crossing/images/screen-shot-2020-06-10-at-123929-pm.png" style="width: 400px; height: 358px;" />
<pre>
<strong>Đầu vào:</strong> path = &quot;NES&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Lưu ý rằng đường đi không đi qua bất kỳ điểm nào quá một lần.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1496.Path%20Crossing/images/screen-shot-2020-06-10-at-123843-pm.png" style="width: 400px; height: 339px;" />
<pre>
<strong>Đầu vào:</strong> path = &quot;NESWW&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Lưu ý rằng đường đi đi qua gốc tọa độ hai lần.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= path.length &lt;= 10<sup>4</sup></code></li>
	<li><code>path[i]</code> là một trong các ký tự <code>&#39;N&#39;</code>, <code>&#39;S&#39;</code>, <code>&#39;E&#39;</code> hoặc <code>&#39;W&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> $|path|\le 10^4$. Duyệt theo đường đi và lưu lại các ô đã đi qua. Nếu một bước dừng tại ô đã được lưu, đường đi đã tự cắt chính nó.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isPathCrossing(self, path: str) -> bool:
        i = j = 0
        vis = {(0, 0)}
        for c in path:
            match c:
                case 'N':
                    i -= 1
                case 'S':
                    i += 1
                case 'E':
                    j += 1
                case 'W':
                    j -= 1
            if (i, j) in vis:
                return True
            vis.add((i, j))
        return False
```

#### Java

```java
class Solution {
    public boolean isPathCrossing(String path) {
        int i = 0, j = 0;
        Set<Integer> vis = new HashSet<>();
        vis.add(0);
        for (int k = 0, n = path.length(); k < n; ++k) {
            switch (path.charAt(k)) {
                case 'N' -> --i;
                case 'S' -> ++i;
                case 'E' -> ++j;
                case 'W' -> --j;
            }
            int t = i * 20000 + j;
            if (!vis.add(t)) {
                return true;
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isPathCrossing(string path) {
        int i = 0, j = 0;
        unordered_set<int> s{{0}};
        for (char& c : path) {
            if (c == 'N') {
                --i;
            } else if (c == 'S') {
                ++i;
            } else if (c == 'E') {
                ++j;
            } else {
                --j;
            }
            int t = i * 20000 + j;
            if (s.count(t)) {
                return true;
            }
            s.insert(t);
        }
        return false;
    }
};
```

#### Go

```go
func isPathCrossing(path string) bool {
	i, j := 0, 0
	vis := map[int]bool{0: true}
	for _, c := range path {
		switch c {
		case 'N':
			i--
		case 'S':
			i++
		case 'E':
			j++
		case 'W':
			j--
		}
		if vis[i*20000+j] {
			return true
		}
		vis[i*20000+j] = true
	}
	return false
}
```

#### TypeScript

```ts
function isPathCrossing(path: string): boolean {
    let [i, j] = [0, 0];
    const vis: Set<number> = new Set();
    vis.add(0);
    for (const c of path) {
        if (c === 'N') {
            --i;
        } else if (c === 'S') {
            ++i;
        } else if (c === 'E') {
            ++j;
        } else if (c === 'W') {
            --j;
        }
        const t = i * 20000 + j;
        if (vis.has(t)) {
            return true;
        }
        vis.add(t);
    }
    return false;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
