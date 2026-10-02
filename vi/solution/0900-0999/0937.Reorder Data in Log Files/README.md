---
comments: true
difficulty: Medium
tags:
    - Array
    - String
    - Sorting
---

<!-- problem:start -->

# [937. Reorder Data in Log Files](https://leetcode.com/problems/reorder-data-in-log-files)

[中文文档](/solution/0900-0999/0937.Reorder%20Data%20in%20Log%20Files/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>logs</code>. Mỗi log là một chuỗi gồm các từ được phân tách bằng dấu cách, trong đó từ đầu tiên là <strong>identifier</strong>.</p>

<p>Có hai loại log:</p>

<ul>
	<li><b>Letter-log</b>: Tất cả các từ (ngoại trừ identifier) chỉ gồm chữ cái tiếng Anh viết thường.</li>
	<li><strong>Digit-log</strong>: Tất cả các từ (ngoại trừ identifier) chỉ gồm chữ số.</li>
</ul>

<p>Sắp xếp lại các log sao cho:</p>

<ol>
	<li><strong>Letter-log</strong> đứng trước tất cả <strong>digit-log</strong>.</li>
	<li><strong>Letter-log</strong> được sắp xếp theo thứ tự từ điển dựa trên nội dung. Nếu nội dung giống nhau thì sắp xếp theo thứ tự từ điển dựa trên identifier.</li>
	<li><strong>Digit-log</strong> giữ nguyên thứ tự tương đối ban đầu.</li>
</ol>

<p>Trả về <em>thứ tự cuối cùng của các log</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> logs = [&quot;dig1 8 1 5 1&quot;,&quot;let1 art can&quot;,&quot;dig2 3 6&quot;,&quot;let2 own kit dig&quot;,&quot;let3 art zero&quot;]
<strong>Đầu ra:</strong> [&quot;let1 art can&quot;,&quot;let3 art zero&quot;,&quot;let2 own kit dig&quot;,&quot;dig1 8 1 5 1&quot;,&quot;dig2 3 6&quot;]
<strong>Giải thích:</strong>
Nội dung của các letter-log đều khác nhau nên thứ tự của chúng là &quot;art can&quot;, &quot;art zero&quot;, &quot;own kit dig&quot;.
Các digit-log giữ thứ tự tương đối là &quot;dig1 8 1 5 1&quot;, &quot;dig2 3 6&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> logs = [&quot;a1 9 2 3 1&quot;,&quot;g1 act car&quot;,&quot;zo4 4 7&quot;,&quot;ab1 off key dog&quot;,&quot;a8 act zoo&quot;]
<strong>Đầu ra:</strong> [&quot;g1 act car&quot;,&quot;a8 act zoo&quot;,&quot;ab1 off key dog&quot;,&quot;a1 9 2 3 1&quot;,&quot;zo4 4 7&quot;]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= logs.length &lt;= 100</code></li>
	<li><code>3 &lt;= logs[i].length &lt;= 100</code></li>
	<li>Tất cả token trong <code>logs[i]</code> được phân tách bằng đúng một dấu cách.</li>
	<li>Đảm bảo <code>logs[i]</code> có một identifier và ít nhất một từ đứng sau identifier.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp tùy chỉnh

<!-- thinking:start -->

> **Tư duy**
>
> Letter-log được sắp xếp theo nội dung rồi đến identifier; digit-log giữ nguyên thứ tự tương đối và nằm cuối. Có thể thực hiện chỉ với một lần sắp xếp ổn định theo key: letter-log dùng $(0,\textit{content},\textit{id})$, còn digit-log dùng $(1,)$.

<!-- thinking:end -->

Ta có thể dùng cách sắp xếp tùy chỉnh để chia log thành hai nhóm: letter-log và digit-log.

Với letter-log, ta sắp xếp theo yêu cầu của đề bài: trước tiên theo nội dung, sau đó theo identifier.

Với digit-log, ta chỉ cần giữ nguyên thứ tự tương đối ban đầu.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số lượng log.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def reorderLogFiles(self, logs: List[str]) -> List[str]:
        def f(log: str):
            id_, rest = log.split(" ", 1)
            return (0, rest, id_) if rest[0].isalpha() else (1,)

        return sorted(logs, key=f)
```

#### Java

```java
class Solution {
    public String[] reorderLogFiles(String[] logs) {
        Arrays.sort(logs, (log1, log2) -> {
            String[] split1 = log1.split(" ", 2);
            String[] split2 = log2.split(" ", 2);

            boolean isLetter1 = Character.isLetter(split1[1].charAt(0));
            boolean isLetter2 = Character.isLetter(split2[1].charAt(0));

            if (isLetter1 && isLetter2) {
                int cmp = split1[1].compareTo(split2[1]);
                if (cmp != 0) {
                    return cmp;
                }
                return split1[0].compareTo(split2[0]);
            }

            return isLetter1 ? -1 : (isLetter2 ? 1 : 0);
        });

        return logs;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> reorderLogFiles(vector<string>& logs) {
        stable_sort(logs.begin(), logs.end(), [](const string& log1, const string& log2) {
            int idx1 = log1.find(' ');
            int idx2 = log2.find(' ');
            string id1 = log1.substr(0, idx1);
            string id2 = log2.substr(0, idx2);
            string content1 = log1.substr(idx1 + 1);
            string content2 = log2.substr(idx2 + 1);

            bool isLetter1 = isalpha(content1[0]);
            bool isLetter2 = isalpha(content2[0]);

            if (isLetter1 && isLetter2) {
                if (content1 != content2) {
                    return content1 < content2;
                }
                return id1 < id2;
            }

            return isLetter1 > isLetter2;
        });

        return logs;
    }
};
```

#### Go

```go
func reorderLogFiles(logs []string) []string {
	sort.SliceStable(logs, func(i, j int) bool {
		log1, log2 := logs[i], logs[j]
		idx1 := strings.IndexByte(log1, ' ')
		idx2 := strings.IndexByte(log2, ' ')
		id1, content1 := log1[:idx1], log1[idx1+1:]
		id2, content2 := log2[:idx2], log2[idx2+1:]

		isLetter1 := 'a' <= content1[0] && content1[0] <= 'z'
		isLetter2 := 'a' <= content2[0] && content2[0] <= 'z'

		if isLetter1 && isLetter2 {
			if content1 != content2 {
				return content1 < content2
			}
			return id1 < id2
		}

		return isLetter1 && !isLetter2
	})

	return logs
}
```

#### TypeScript

```ts
function reorderLogFiles(logs: string[]): string[] {
    return logs.sort((log1, log2) => {
        const [id1, content1] = log1.split(/ (.+)/);
        const [id2, content2] = log2.split(/ (.+)/);

        const isLetter1 = isNaN(Number(content1[0]));
        const isLetter2 = isNaN(Number(content2[0]));

        if (isLetter1 && isLetter2) {
            const cmp = content1.localeCompare(content2);
            if (cmp !== 0) {
                return cmp;
            }
            return id1.localeCompare(id2);
        }

        return isLetter1 ? -1 : isLetter2 ? 1 : 0;
    });
}
```

#### Rust

```rust
use std::cmp::Ordering;

impl Solution {
    pub fn reorder_log_files(logs: Vec<String>) -> Vec<String> {
        let mut logs = logs;

        logs.sort_by(|log1, log2| {
            let split1: Vec<&str> = log1.splitn(2, ' ').collect();
            let split2: Vec<&str> = log2.splitn(2, ' ').collect();

            let is_letter1 = split1[1].chars().next().unwrap().is_alphabetic();
            let is_letter2 = split2[1].chars().next().unwrap().is_alphabetic();

            if is_letter1 && is_letter2 {
                let cmp = split1[1].cmp(split2[1]);
                if cmp != Ordering::Equal {
                    return cmp;
                }
                return split1[0].cmp(split2[0]);
            }

            if is_letter1 {
                Ordering::Less
            } else if is_letter2 {
                Ordering::Greater
            } else {
                Ordering::Equal
            }
        });

        logs
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
