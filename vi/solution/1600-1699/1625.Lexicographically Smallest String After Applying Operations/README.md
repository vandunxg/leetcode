---
comments: true
difficulty: Medium
rating: 1992
source: Weekly Contest 211 Q2
tags:
    - Depth-First Search
    - Breadth-First Search
    - String
    - Enumeration
---

<!-- problem:start -->

# [1625. Lexicographically Smallest String After Applying Operations](https://leetcode.com/problems/lexicographically-smallest-string-after-applying-operations)

[中文文档](/solution/1600-1699/1625.Lexicographically%20Smallest%20String%20After%20Applying%20Operations/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code> có <strong>độ dài chẵn</strong>, chỉ gồm các chữ số từ <code>0</code> đến <code>9</code>, cùng hai số nguyên <code>a</code> và <code>b</code>.</p>

<p>Có thể áp dụng hai thao tác sau lên <code>s</code> nhiều lần và theo bất kỳ thứ tự nào:</p>

<ul>
	<li>Cộng <code>a</code> vào mọi chỉ số lẻ của <code>s</code> <strong>(đánh số từ 0)</strong>. Các chữ số vượt quá <code>9</code> sẽ quay lại <code>0</code>. Ví dụ, nếu <code>s = &quot;3456&quot;</code> và <code>a = 5</code>, <code>s</code> trở thành <code>&quot;3951&quot;</code>.</li>
	<li>Xoay <code>s</code> sang phải <code>b</code> vị trí. Ví dụ, nếu <code>s = &quot;3456&quot;</code> và <code>b = 1</code>, <code>s</code> trở thành <code>&quot;6345&quot;</code>.</li>
</ul>

<p>Trả về <em>chuỗi <strong>nhỏ nhất theo thứ tự từ điển</strong> có thể nhận được bằng cách áp dụng các thao tác trên nhiều lần lên</em> <code>s</code>.</p>

<p>Chuỗi <code>a</code> nhỏ hơn chuỗi <code>b</code> theo thứ tự từ điển (khi có cùng độ dài) nếu tại vị trí đầu tiên mà chúng khác nhau, ký tự của <code>a</code> đứng trước ký tự tương ứng của <code>b</code> trong bảng chữ cái. Ví dụ, <code>&quot;0158&quot;</code> nhỏ hơn <code>&quot;0190&quot;</code> vì vị trí khác nhau đầu tiên là ký tự thứ ba, và <code>&#39;5&#39;</code> đứng trước <code>&#39;9&#39;</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;5525&quot;, a = 9, b = 2
<strong>Output:</strong> &quot;2050&quot;
<strong>Explanation:</strong> Ta có thể áp dụng các thao tác sau:
Start:  &quot;5525&quot;
Rotate: &quot;2555&quot;
Add:    &quot;2454&quot;
Add:    &quot;2353&quot;
Rotate: &quot;5323&quot;
Add:    &quot;5222&quot;
Add:    &quot;5121&quot;
Rotate: &quot;2151&quot;
Add:    &quot;2050&quot;​​​​​
There is no way to obtain a string that is lexicographically smaller than &quot;2050&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;74&quot;, a = 5, b = 1
<strong>Output:</strong> &quot;24&quot;
<strong>Explanation:</strong> We can apply the following operations:
Start:  &quot;74&quot;
Rotate: &quot;47&quot;
​​​​​​​Add:    &quot;42&quot;
​​​​​​​Rotate: &quot;24&quot;​​​​​​​​​​​​
There is no way to obtain a string that is lexicographically smaller than &quot;24&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;0011&quot;, a = 4, b = 2
<strong>Output:</strong> &quot;0011&quot;
<strong>Explanation:</strong> Không có chuỗi thao tác nào tạo ra chuỗi nhỏ hơn &quot;0011&quot; theo thứ tự từ điển.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= s.length &lt;= 100</code></li>
	<li><code>s.length</code> là số chẵn.</li>
	<li><code>s</code> consists of digits from <code>0</code> to <code>9</code> only.</li>
	<li><code>1 &lt;= a &lt;= 9</code></li>
	<li><code>1 &lt;= b &lt;= s.length - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

Các tham số thao tác là <code>a</code> và <code>b</code>.

<!-- solution:start -->

### Lời giải 1: BFS

<!-- thinking:start -->

> **Tư duy**
>
> Hai thao tác cộng và xoay tạo thành một đồ thị có bậc ra bằng hai. Độ dài nhiều nhất là $100$ và bảng chữ cái chỉ gồm chữ số, nên tập trạng thái có thể đạt được đủ nhỏ để dùng BFS.
>
> Từ $s$, cộng $a$ (mod $10$) vào các chỉ số lẻ và xoay phải $b$ vị trí, khử trùng bằng một set và giữ chuỗi nhỏ nhất theo thứ tự từ điển đã gặp.

<!-- thinking:end -->

Vì quy mô dữ liệu của bài toán tương đối nhỏ, ta có thể dùng BFS để tìm kiếm vét cạn mọi trạng thái có thể, sau đó chọn trạng thái nhỏ nhất theo thứ tự từ điển.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findLexSmallestString(self, s: str, a: int, b: int) -> str:
        q = deque([s])
        vis = {s}
        ans = s
        while q:
            s = q.popleft()
            if ans > s:
                ans = s
            t1 = ''.join(
                [str((int(c) + a) % 10) if i & 1 else c for i, c in enumerate(s)]
            )
            t2 = s[-b:] + s[:-b]
            for t in (t1, t2):
                if t not in vis:
                    vis.add(t)
                    q.append(t)
        return ans
```

#### Java

```java
class Solution {
    public String findLexSmallestString(String s, int a, int b) {
        Deque<String> q = new ArrayDeque<>();
        q.offer(s);
        Set<String> vis = new HashSet<>();
        vis.add(s);
        String ans = s;
        int n = s.length();
        while (!q.isEmpty()) {
            s = q.poll();
            if (ans.compareTo(s) > 0) {
                ans = s;
            }
            char[] cs = s.toCharArray();
            for (int i = 1; i < n; i += 2) {
                cs[i] = (char) (((cs[i] - '0' + a) % 10) + '0');
            }
            String t1 = String.valueOf(cs);
            String t2 = s.substring(n - b) + s.substring(0, n - b);
            for (String t : List.of(t1, t2)) {
                if (vis.add(t)) {
                    q.offer(t);
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
    string findLexSmallestString(string s, int a, int b) {
        queue<string> q{{s}};
        unordered_set<string> vis{{s}};
        string ans = s;
        int n = s.size();
        while (!q.empty()) {
            s = q.front();
            q.pop();
            ans = min(ans, s);
            string t1 = s;
            for (int i = 1; i < n; i += 2) {
                t1[i] = (t1[i] - '0' + a) % 10 + '0';
            }
            string t2 = s.substr(n - b) + s.substr(0, n - b);
            for (auto& t : {t1, t2}) {
                if (!vis.count(t)) {
                    vis.insert(t);
                    q.emplace(t);
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findLexSmallestString(s string, a int, b int) string {
	q := []string{s}
	vis := map[string]bool{s: true}
	ans := s
	n := len(s)
	for len(q) > 0 {
		s = q[0]
		q = q[1:]
		if ans > s {
			ans = s
		}
		t1 := []byte(s)
		for i := 1; i < n; i += 2 {
			t1[i] = byte((int(t1[i]-'0')+a)%10 + '0')
		}
		t2 := s[n-b:] + s[:n-b]
		for _, t := range []string{string(t1), t2} {
			if !vis[t] {
				vis[t] = true
				q = append(q, t)
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function findLexSmallestString(s: string, a: number, b: number): string {
    const q: string[] = [s];
    const vis = new Set<string>([s]);
    let ans = s;
    let i = 0;
    while (i < q.length) {
        s = q[i++];
        if (ans > s) {
            ans = s;
        }
        const t1 = s
            .split('')
            .map((c, j) => (j & 1 ? String((Number(c) + a) % 10) : c))
            .join('');
        const t2 = s.slice(-b) + s.slice(0, -b);
        for (const t of [t1, t2]) {
            if (!vis.has(t)) {
                vis.add(t);
                q.push(t);
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 duyệt đồ thị và phải dùng queue cùng hash set. Phép cộng có chu kỳ $10$ và phép xoay có nhiều nhất $n$ trạng thái; khi $b$ chẵn, phép cộng không bao giờ tác động lên các chỉ số chẵn.
>
> Liệt kê nhiều nhất $n$ lần xoay và $10$ lần cộng trên các vị trí lẻ; nếu $b$ lẻ, lồng thêm $10$ lần cộng trên các vị trí chẵn, rồi lấy chuỗi nhỏ nhất.

<!-- thinking:end -->

Ta nhận thấy với phép cộng, một chữ số sẽ trở về trạng thái ban đầu sau nhiều nhất $10$ lần cộng; với phép xoay, chuỗi cũng trở về trạng thái ban đầu sau nhiều nhất $n$ lần xoay.

Do đó, phép xoay tạo ra nhiều nhất $n$ trạng thái. Nếu số vị trí xoay $b$ là chẵn, phép cộng chỉ tác động lên các chữ số ở chỉ số lẻ, tạo ra tổng cộng $n \times 10$ trạng thái; nếu $b$ lẻ, phép cộng tác động lên cả chữ số ở chỉ số lẻ và chẵn, tạo ra tổng cộng $n \times 10 \times 10$ trạng thái.

Vì vậy, ta có thể trực tiếp liệt kê mọi trạng thái chuỗi có thể và chọn trạng thái nhỏ nhất theo thứ tự từ điển.

Độ phức tạp thời gian là $O(n^2 \times 10^2)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findLexSmallestString(self, s: str, a: int, b: int) -> str:
        ans = s
        n = len(s)
        s = list(s)
        for _ in range(n):
            s = s[-b:] + s[:-b]
            for j in range(10):
                for k in range(1, n, 2):
                    s[k] = str((int(s[k]) + a) % 10)
                if b & 1:
                    for p in range(10):
                        for k in range(0, n, 2):
                            s[k] = str((int(s[k]) + a) % 10)
                        t = ''.join(s)
                        if ans > t:
                            ans = t
                else:
                    t = ''.join(s)
                    if ans > t:
                        ans = t
        return ans
```

#### Java

```java
class Solution {
    public String findLexSmallestString(String s, int a, int b) {
        int n = s.length();
        String ans = s;
        for (int i = 0; i < n; ++i) {
            s = s.substring(b) + s.substring(0, b);
            char[] cs = s.toCharArray();
            for (int j = 0; j < 10; ++j) {
                for (int k = 1; k < n; k += 2) {
                    cs[k] = (char) (((cs[k] - '0' + a) % 10) + '0');
                }
                if ((b & 1) == 1) {
                    for (int p = 0; p < 10; ++p) {
                        for (int k = 0; k < n; k += 2) {
                            cs[k] = (char) (((cs[k] - '0' + a) % 10) + '0');
                        }
                        s = String.valueOf(cs);
                        if (ans.compareTo(s) > 0) {
                            ans = s;
                        }
                    }
                } else {
                    s = String.valueOf(cs);
                    if (ans.compareTo(s) > 0) {
                        ans = s;
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
    string findLexSmallestString(string s, int a, int b) {
        int n = s.size();
        string ans = s;
        for (int i = 0; i < n; ++i) {
            s = s.substr(n - b) + s.substr(0, n - b);
            for (int j = 0; j < 10; ++j) {
                for (int k = 1; k < n; k += 2) {
                    s[k] = (s[k] - '0' + a) % 10 + '0';
                }
                if (b & 1) {
                    for (int p = 0; p < 10; ++p) {
                        for (int k = 0; k < n; k += 2) {
                            s[k] = (s[k] - '0' + a) % 10 + '0';
                        }
                        ans = min(ans, s);
                    }
                } else {
                    ans = min(ans, s);
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findLexSmallestString(s string, a int, b int) string {
	n := len(s)
	ans := s
	for _ = range s {
		s = s[n-b:] + s[:n-b]
		cs := []byte(s)
		for j := 0; j < 10; j++ {
			for k := 1; k < n; k += 2 {
				cs[k] = byte((int(cs[k]-'0')+a)%10 + '0')
			}
			if b&1 == 1 {
				for p := 0; p < 10; p++ {
					for k := 0; k < n; k += 2 {
						cs[k] = byte((int(cs[k]-'0')+a)%10 + '0')
					}
					s = string(cs)
					if ans > s {
						ans = s
					}
				}
			} else {
				s = string(cs)
				if ans > s {
					ans = s
				}
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function findLexSmallestString(s: string, a: number, b: number): string {
    let ans = s;
    const n = s.length;
    let arr = s.split('');
    for (let _ = 0; _ < n; _++) {
        arr = arr.slice(-b).concat(arr.slice(0, -b));
        for (let j = 0; j < 10; j++) {
            for (let k = 1; k < n; k += 2) {
                arr[k] = String((Number(arr[k]) + a) % 10);
            }
            if (b & 1) {
                for (let p = 0; p < 10; p++) {
                    for (let k = 0; k < n; k += 2) {
                        arr[k] = String((Number(arr[k]) + a) % 10);
                    }
                    const t = arr.join('');
                    if (ans > t) {
                        ans = t;
                    }
                }
            } else {
                const t = arr.join('');
                if (ans > t) {
                    ans = t;
                }
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
