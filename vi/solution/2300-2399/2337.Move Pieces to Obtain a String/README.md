---
comments: true
difficulty: Medium
rating: 1693
source: Weekly Contest 301 Q3
tags:
    - Two Pointers
    - String
---

<!-- problem:start -->

# [2337. Move Pieces to Obtain a String](https://leetcode.com/problems/move-pieces-to-obtain-a-string)

[中文文档](/solution/2300-2399/2337.Move%20Pieces%20to%20Obtain%20a%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>start</code> và <code>target</code>, cả hai đều có độ dài <code>n</code>. Mỗi chuỗi <strong>chỉ</strong> bao gồm các ký tự <code>&#39;L&#39;</code>, <code>&#39;R&#39;</code> và <code>&#39;_&#39;</code>, trong đó:</p>

<ul>
	<li>Các ký tự <code>&#39;L&#39;</code> và <code>&#39;R&#39;</code> đại diện cho các quân cờ; quân <code>&#39;L&#39;</code> chỉ có thể di chuyển sang <strong>trái</strong> nếu ngay bên trái nó có một ô <strong>trống</strong>, còn quân <code>&#39;R&#39;</code> chỉ có thể di chuyển sang <strong>phải</strong> nếu ngay bên phải nó có một ô <strong>trống</strong>.</li>
	<li>Ký tự <code>&#39;_&#39;</code> đại diện cho một ô trống có thể được chiếm bởi <strong>bất kỳ</strong> quân <code>&#39;L&#39;</code> hoặc <code>&#39;R&#39;</code> nào.</li>
</ul>

<p>Trả về <code>true</code> <em>nếu có thể thu được chuỗi</em> <code>target</code><em> bằng cách di chuyển các quân cờ trong chuỗi </em><code>start</code><em> <strong>bất kỳ</strong> số lần nào</em>. Nếu không, trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> start = &quot;_L__R__R_&quot;, target = &quot;L______RR&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Ta có thể thu được chuỗi target từ start bằng cách thực hiện các bước di chuyển sau:
- Di chuyển quân cờ đầu tiên một bước sang trái, khi đó start trở thành &quot;<strong>L</strong>___R__R_&quot;.
- Di chuyển quân cờ cuối cùng một bước sang phải, khi đó start trở thành &quot;L___R___<strong>R</strong>&quot;.
- Di chuyển quân cờ thứ hai ba bước sang phải, khi đó start trở thành &quot;L______<strong>R</strong>R&quot;.
Vì có thể thu được chuỗi target từ start, ta trả về true.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> start = &quot;R_L_&quot;, target = &quot;__LR&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Quân &#39;R&#39; trong chuỗi start có thể di chuyển một bước sang phải để thu được &quot;_<strong>R</strong>L_&quot;.
Sau đó, không còn quân cờ nào có thể di chuyển, nên không thể thu được chuỗi target từ start.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> start = &quot;_R&quot;, target = &quot;R_&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Quân cờ trong chuỗi start chỉ có thể di chuyển sang phải, nên không thể thu được chuỗi target từ start.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == start.length == target.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>start</code> và <code>target</code> chỉ gồm các ký tự <code>&#39;L&#39;</code>, <code>&#39;R&#39;</code> và <code>&#39;_&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> `L` chỉ di chuyển sang trái, `R` chỉ di chuyển sang phải và chúng không thể vượt qua nhau. $n \le 10^5$ loại trừ việc mô phỏng từng bước. Sau khi loại bỏ các ô trống, dãy ký tự phải giống nhau.
>
> Trích xuất các ký tự khác `_` cùng với chỉ số của chúng. Nếu các ký tự không khớp thì trả về false; quân `L` không thể bắt đầu ở bên phải vị trí đích, còn quân `R` không thể bắt đầu ở bên trái vị trí đích.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canChange(self, start: str, target: str) -> bool:
        a = [(v, i) for i, v in enumerate(start) if v != '_']
        b = [(v, i) for i, v in enumerate(target) if v != '_']
        if len(a) != len(b):
            return False
        for (c, i), (d, j) in zip(a, b):
            if c != d:
                return False
            if c == 'L' and i < j:
                return False
            if c == 'R' and i > j:
                return False
        return True
```

#### Java

```java
class Solution {
    public boolean canChange(String start, String target) {
        List<int[]> a = f(start);
        List<int[]> b = f(target);
        if (a.size() != b.size()) {
            return false;
        }
        for (int i = 0; i < a.size(); ++i) {
            int[] x = a.get(i);
            int[] y = b.get(i);
            if (x[0] != y[0]) {
                return false;
            }
            if (x[0] == 1 && x[1] < y[1]) {
                return false;
            }
            if (x[0] == 2 && x[1] > y[1]) {
                return false;
            }
        }
        return true;
    }

    private List<int[]> f(String s) {
        List<int[]> res = new ArrayList<>();
        for (int i = 0; i < s.length(); ++i) {
            if (s.charAt(i) == 'L') {
                res.add(new int[] {1, i});
            } else if (s.charAt(i) == 'R') {
                res.add(new int[] {2, i});
            }
        }
        return res;
    }
}
```

#### C++

```cpp
using pii = pair<int, int>;

class Solution {
public:
    bool canChange(string start, string target) {
        auto a = f(start);
        auto b = f(target);
        if (a.size() != b.size()) return false;
        for (int i = 0; i < a.size(); ++i) {
            auto x = a[i], y = b[i];
            if (x.first != y.first) return false;
            if (x.first == 1 && x.second < y.second) return false;
            if (x.first == 2 && x.second > y.second) return false;
        }
        return true;
    }

    vector<pair<int, int>> f(string s) {
        vector<pii> res;
        for (int i = 0; i < s.size(); ++i) {
            if (s[i] == 'L')
                res.push_back({1, i});
            else if (s[i] == 'R')
                res.push_back({2, i});
        }
        return res;
    }
};
```

#### Go

```go
func canChange(start string, target string) bool {
	f := func(s string) [][]int {
		res := [][]int{}
		for i, c := range s {
			if c == 'L' {
				res = append(res, []int{1, i})
			} else if c == 'R' {
				res = append(res, []int{2, i})
			}
		}
		return res
	}

	a, b := f(start), f(target)
	if len(a) != len(b) {
		return false
	}
	for i, x := range a {
		y := b[i]
		if x[0] != y[0] {
			return false
		}
		if x[0] == 1 && x[1] < y[1] {
			return false
		}
		if x[0] == 2 && x[1] > y[1] {
			return false
		}
	}
	return true
}
```

#### TypeScript

```ts
function canChange(start: string, target: string): boolean {
    if (
        [...start].filter(c => c !== '_').join('') !== [...target].filter(c => c !== '_').join('')
    ) {
        return false;
    }
    const n = start.length;
    let i = 0;
    let j = 0;
    while (i < n || j < n) {
        while (start[i] === '_') {
            i++;
        }
        while (target[j] === '_') {
            j++;
        }
        if (start[i] === 'R') {
            if (i > j) {
                return false;
            }
        }
        if (start[i] === 'L') {
            if (i < j) {
                return false;
            }
        }
        i++;
        j++;
    }
    return true;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 tạo ra hai danh sách vị trí. Hai con trỏ sẽ bỏ qua `_` trực tiếp trên các chuỗi ban đầu và áp dụng cùng các phép kiểm tra chỉ số, nhờ đó không cần tạo thêm mảng.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canChange(self, start: str, target: str) -> bool:
        n = len(start)
        i = j = 0
        while 1:
            while i < n and start[i] == '_':
                i += 1
            while j < n and target[j] == '_':
                j += 1
            if i >= n and j >= n:
                return True
            if i >= n or j >= n or start[i] != target[j]:
                return False
            if start[i] == 'L' and i < j:
                return False
            if start[i] == 'R' and i > j:
                return False
            i, j = i + 1, j + 1
```

#### Java

```java
class Solution {
    public boolean canChange(String start, String target) {
        int n = start.length();
        int i = 0, j = 0;
        while (true) {
            while (i < n && start.charAt(i) == '_') {
                ++i;
            }
            while (j < n && target.charAt(j) == '_') {
                ++j;
            }
            if (i == n && j == n) {
                return true;
            }
            if (i == n || j == n || start.charAt(i) != target.charAt(j)) {
                return false;
            }
            if (start.charAt(i) == 'L' && i < j || start.charAt(i) == 'R' && i > j) {
                return false;
            }
            ++i;
            ++j;
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canChange(string start, string target) {
        int n = start.size();
        int i = 0, j = 0;
        while (true) {
            while (i < n && start[i] == '_') ++i;
            while (j < n && target[j] == '_') ++j;
            if (i == n && j == n) return true;
            if (i == n || j == n || start[i] != target[j]) return false;
            if (start[i] == 'L' && i < j) return false;
            if (start[i] == 'R' && i > j) return false;
            ++i;
            ++j;
        }
    }
};
```

#### Go

```go
func canChange(start string, target string) bool {
	n := len(start)
	i, j := 0, 0
	for {
		for i < n && start[i] == '_' {
			i++
		}
		for j < n && target[j] == '_' {
			j++
		}
		if i == n && j == n {
			return true
		}
		if i == n || j == n || start[i] != target[j] {
			return false
		}
		if start[i] == 'L' && i < j {
			return false
		}
		if start[i] == 'R' && i > j {
			return false
		}
		i, j = i+1, j+1
	}
}
```

#### TypeScript

```ts
function canChange(start: string, target: string): boolean {
    const n = start.length;
    let [i, j] = [0, 0];
    while (1) {
        while (i < n && start[i] === '_') {
            ++i;
        }
        while (j < n && target[j] === '_') {
            ++j;
        }
        if (i === n && j === n) {
            return true;
        }
        if (i === n || j === n || start[i] !== target[j]) {
            return false;
        }
        if ((start[i] === 'L' && i < j) || (start[i] === 'R' && i > j)) {
            return false;
        }
        ++i;
        ++j;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
