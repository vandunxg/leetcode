---
comments: true
difficulty: Easy
rating: 1297
source: Weekly Contest 208 Q1
tags:
    - Stack
    - Array
    - String
---

<!-- problem:start -->

# [1598. Crawler Log Folder](https://leetcode.com/problems/crawler-log-folder)

[中文文档](/solution/1500-1599/1598.Crawler%20Log%20Folder/README.md)

## Mô tả

<!-- description:start -->

<p>Hệ thống tệp Leetcode ghi log mỗi khi người dùng thực hiện thao tác <em>đổi thư mục</em>.</p>

<p>Các thao tác được mô tả như sau:</p>

<ul>
	<li><code>&quot;../&quot;</code> : Move to the parent folder of the current folder. (If you are already in the main folder, <strong>remain in the same folder</strong>).</li>
	<li><code>&quot;./&quot;</code> : Remain in the same folder.</li>
	<li><code>&quot;x/&quot;</code> : Move to the child folder named <code>x</code> (This folder is <strong>guaranteed to always exist</strong>).</li>
</ul>

<p>Cho danh sách chuỗi <code>logs</code>, trong đó <code>logs[i]</code> là thao tác người dùng thực hiện ở bước thứ <code>i<sup>th</sup></code>.</p>

<p>Hệ thống tệp bắt đầu ở thư mục chính, sau đó thực hiện các thao tác trong <code>logs</code>.</p>

<p>Trả về <em>số thao tác tối thiểu cần thực hiện để quay lại thư mục chính sau các thao tác đổi thư mục</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1598.Crawler%20Log%20Folder/images/sample_11_1957.png" style="width: 775px; height: 151px;" /></p>

<pre>
<strong>Input:</strong> logs = [&quot;d1/&quot;,&quot;d2/&quot;,&quot;../&quot;,&quot;d21/&quot;,&quot;./&quot;]
<strong>Output:</strong> 2
<strong>Giải thích: </strong>Dùng thao tác đổi thư mục &quot;../&quot; 2 lần để quay lại thư mục chính.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1598.Crawler%20Log%20Folder/images/sample_22_1957.png" style="width: 600px; height: 270px;" /></p>

<pre>
<strong>Input:</strong> logs = [&quot;d1/&quot;,&quot;d2/&quot;,&quot;./&quot;,&quot;d3/&quot;,&quot;../&quot;,&quot;d31/&quot;]
<strong>Output:</strong> 3
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> logs = [&quot;d1/&quot;,&quot;../&quot;,&quot;../&quot;,&quot;../&quot;]
<strong>Output:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= logs.length &lt;= 10<sup>3</sup></code></li>
	<li><code>2 &lt;= logs[i].length &lt;= 10</code></li>
	<li><code>logs[i]</code> contains lowercase English letters, digits, <code>&#39;.&#39;</code>, and <code>&#39;/&#39;</code>.</li>
	<li><code>logs[i]</code> follows the format described in the statement.</li>
	<li>Folder names consist of lowercase English letters and digits.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Log có thể đi vào thư mục, đứng yên hoặc đi lên một cấp; ta cần ít bước $\texttt{../}$ nhất để về root. Có nhiều nhất $10^3$ log, nên chỉ cần theo dõi depth, không cần stack đường dẫn thật.
>
> Tên thư mục thật tăng depth; $\texttt{../}$ giảm depth nhưng không dưới 0. Depth cuối cùng là số lần đi lên còn cần thực hiện.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, logs: List[str]) -> int:
        ans = 0
        for v in logs:
            if v == "../":
                ans = max(0, ans - 1)
            elif v[0] != ".":
                ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int minOperations(String[] logs) {
        int ans = 0;
        for (var v : logs) {
            if ("../".equals(v)) {
                ans = Math.max(0, ans - 1);
            } else if (v.charAt(0) != '.') {
                ++ans;
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
    int minOperations(vector<string>& logs) {
        int ans = 0;
        for (auto& v : logs) {
            if (v == "../") {
                ans = max(0, ans - 1);
            } else if (v[0] != '.') {
                ++ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minOperations(logs []string) int {
	ans := 0
	for _, v := range logs {
		if v == "../" {
			if ans > 0 {
				ans--
			}
		} else if v[0] != '.' {
			ans++
		}
	}
	return ans
}
```

#### TypeScript

```ts
function minOperations(logs: string[]): number {
    let ans = 0;
    for (const x of logs) {
        if (x === '../') {
            ans && ans--;
        } else if (x !== './') {
            ans++;
        }
    }
    return ans;
}
```

#### JavaScript

```js
function minOperations(logs) {
    let ans = 0;
    for (const x of logs) {
        if (x === '../') {
            ans && ans--;
        } else if (x !== './') {
            ans++;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_operations(logs: Vec<String>) -> i32 {
        let mut depth = 0;
        for log in logs.iter() {
            if log == "../" {
                depth = (0).max(depth - 1);
            } else if log != "./" {
                depth += 1;
            }
        }
        depth
    }
}
```

#### C

```c
#define max(a, b) (((a) > (b)) ? (a) : (b))

int minOperations(char** logs, int logsSize) {
    int depth = 0;
    for (int i = 0; i < logsSize; i++) {
        char* log = logs[i];
        if (!strcmp(log, "../")) {
            depth = max(0, depth - 1);
        } else if (strcmp(log, "./")) {
            depth++;
        }
    }
    return depth;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
