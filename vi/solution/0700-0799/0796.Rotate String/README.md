---
comments: true
difficulty: Easy
tags:
    - String
    - String Matching
---

<!-- problem:start -->

# [796. Rotate String](https://leetcode.com/problems/rotate-string)

[中文文档](/solution/0700-0799/0796.Rotate%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi <code>s</code> và <code>goal</code>, trả về <code>true</code> <em>khi và chỉ khi</em> <code>s</code> <em>có thể trở thành</em> <code>goal</code> <em>sau một số lần <strong>dịch chuyển</strong></em> <code>s</code>.</p>

<p>Một lần <strong>dịch chuyển</strong> <code>s</code> là đưa ký tự ngoài cùng bên trái của <code>s</code> đến vị trí ngoài cùng bên phải.</p>

<ul>
	<li>Ví dụ, nếu <code>s = &quot;abcde&quot;</code>, sau một lần dịch chuyển, chuỗi sẽ thành <code>&quot;bcdea&quot;</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> s = "abcde", goal = "cdeab"
<strong>Đầu ra:</strong> true
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> s = "abcde", goal = "abced"
<strong>Đầu ra:</strong> false
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length, goal.length &lt;= 100</code></li>
	<li><code>s</code> và <code>goal</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> $goal$ có phải là phép xoay vòng của $s$ không? Nếu độ dài khác nhau thì kết quả là false; nếu bằng nhau, $goal$ phải là chuỗi con của $s+s$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def rotateString(self, s: str, goal: str) -> bool:
        return len(s) == len(goal) and goal in s + s
```

#### Java

```java
class Solution {
    public boolean rotateString(String s, String goal) {
        return s.length() == goal.length() && (s + s).contains(goal);
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool rotateString(string s, string goal) {
        return s.size() == goal.size() && strstr((s + s).data(), goal.data());
    }
};
```

#### Go

```go
func rotateString(s string, goal string) bool {
	return len(s) == len(goal) && strings.Contains(s+s, goal)
}
```

#### TypeScript

```ts
function rotateString(s: string, goal: string): boolean {
    return s.length === goal.length && (goal + goal).includes(s);
}
```

#### Rust

```rust
impl Solution {
    pub fn rotate_string(s: String, goal: String) -> bool {
        s.len() == goal.len() && (s.clone() + &s).contains(&goal)
    }
}
```

#### PHP

```php
class Solution {
    /**
     * @param String $s
     * @param String $goal
     * @return Boolean
     */
    function rotateString($s, $goal) {
        return strlen($goal) === strlen($s) && strpos($s . $s, $goal) !== false;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
