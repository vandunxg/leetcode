---
comments: true
difficulty: Easy
rating: 1367
source: Weekly Contest 325 Q1
tags:
    - Array
    - String
---

<!-- problem:start -->

# [2515. Shortest Distance to Target String in a Circular Array](https://leetcode.com/problems/shortest-distance-to-target-string-in-a-circular-array)

[中文文档](/solution/2500-2599/2515.Shortest%20Distance%20to%20Target%20String%20in%20a%20Circular%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng chuỗi <code>words</code> <strong>được đánh chỉ số từ 0</strong> và có tính <strong>vòng</strong>, cùng một chuỗi <code>target</code>. <strong>Mảng vòng</strong> là mảng mà phần tử cuối nối với phần tử đầu.</p>

<ul>
	<li>Cụ thể, phần tử tiếp theo của <code>words[i]</code> là <code>words[(i + 1) % n]</code> và phần tử trước đó của <code>words[i]</code> là <code>words[(i - 1 + n) % n]</code>, trong đó <code>n</code> là độ dài của <code>words</code>.</li>
</ul>

<p>Bắt đầu từ <code>startIndex</code>, mỗi lần di chuyển đến từ tiếp theo hoặc từ trước đó được tính là <code>1</code> bước.</p>

<p>Trả về <em><strong>khoảng cách ngắn nhất</strong> cần thiết để đến chuỗi</em> <code>target</code>. Nếu chuỗi <code>target</code> không tồn tại trong <code>words</code>, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;hello&quot;,&quot;i&quot;,&quot;am&quot;,&quot;leetcode&quot;,&quot;hello&quot;], target = &quot;hello&quot;, startIndex = 1
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Bắt đầu từ chỉ số 1, ta có thể đến &quot;hello&quot; bằng cách
- di chuyển 3 đơn vị sang phải để đến chỉ số 4.
- di chuyển 2 đơn vị sang trái để đến chỉ số 4.
- di chuyển 4 đơn vị sang phải để đến chỉ số 0.
- di chuyển 1 đơn vị sang trái để đến chỉ số 0.
Khoảng cách ngắn nhất để đến &quot;hello&quot; là 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;a&quot;,&quot;b&quot;,&quot;leetcode&quot;], target = &quot;leetcode&quot;, startIndex = 0
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Bắt đầu từ chỉ số 0, ta có thể đến &quot;leetcode&quot; bằng cách
- di chuyển 2 đơn vị sang phải để đến chỉ số 2.
- di chuyển 1 đơn vị sang trái để đến chỉ số 2.
Khoảng cách ngắn nhất để đến &quot;leetcode&quot; là 1.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;i&quot;,&quot;eat&quot;,&quot;leetcode&quot;], target = &quot;ate&quot;, startIndex = 0
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Vì &quot;ate&quot; không tồn tại trong <code>words</code>, ta trả về -1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 100</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 100</code></li>
	<li><code>words[i]</code> và <code>target</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>0 &lt;= startIndex &lt; words.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt một lần

<!-- thinking:start -->

> **Tư duy**
>
> Mảng có tính vòng; ta cần số bước ít nhất từ $\textit{startIndex}$ đến một chỉ số có từ bằng $\textit{target}$. Việc mở rộng theo cả hai hướng phù hợp với $n\le 100$, nhưng khoảng cách vòng đến mỗi lần xuất hiện có công thức đóng.
>
> Với mỗi chỉ số $i$ thỏa mãn $\textit{words}[i]=\textit{target}$, khoảng cách là $\min(|i-\textit{startIndex}|,\,n-|i-\textit{startIndex}|)$. Ta lấy giá trị nhỏ nhất, hoặc $-1$ nếu $\textit{target}$ không xuất hiện.

<!-- thinking:end -->

Ta duyệt mảng $\textit{words}$$,$ tìm các từ bằng $\textit{target}$ và tính khoảng cách $t$ từ chúng đến $\textit{startIndex}$. Khoảng cách ngắn nhất trong trường hợp này là $\min(t, n - t)$, nên ta chỉ cần liên tục cập nhật giá trị nhỏ nhất.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def closestTarget(self, words: List[str], target: str, startIndex: int) -> int:
        n = len(words)
        ans = n
        for i, w in enumerate(words):
            if w == target:
                t = abs(i - startIndex)
                ans = min(ans, t, n - t)
        return -1 if ans == n else ans
```

#### Java

```java
class Solution {
    public int closestTarget(String[] words, String target, int startIndex) {
        int n = words.length;
        int ans = n;
        for (int i = 0; i < n; i++) {
            if (words[i].equals(target)) {
                int t = Math.abs(i - startIndex);
                ans = Math.min(ans, Math.min(t, n - t));
            }
        }
        return ans == n ? -1 : ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int closestTarget(vector<string>& words, string target, int startIndex) {
        int n = words.size();
        int ans = n;
        for (int i = 0; i < n; i++) {
            if (words[i] == target) {
                int t = abs(i - startIndex);
                ans = min({ans, t, n - t});
            }
        }
        return ans == n ? -1 : ans;
    }
};
```

#### Go

```go
func closestTarget(words []string, target string, startIndex int) int {
	n := len(words)
	ans := n
	for i, w := range words {
		if w == target {
			t := i - startIndex
			if t < 0 {
				t = -t
			}
			ans = min(ans, t, n-t)
		}
	}
	if ans == n {
		return -1
	}
	return ans
}
```

#### TypeScript

```ts
function closestTarget(words: string[], target: string, startIndex: number): number {
    const n = words.length;
    let ans = n;
    for (let i = 0; i < n; i++) {
        if (words[i] === target) {
            const t = Math.abs(i - startIndex);
            ans = Math.min(ans, t, n - t);
        }
    }
    return ans === n ? -1 : ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn closest_target(words: Vec<String>, target: String, start_index: i32) -> i32 {
        let n = words.len() as i32;
        let mut ans = n;
        for (i, w) in words.iter().enumerate() {
            if w == &target {
                let t = (i as i32 - start_index).abs();
                ans = ans.min(t.min(n - t));
            }
        }
        if ans == n { -1 } else { ans }
    }
}
```

#### C

```c
#define min(a, b) ((a) < (b) ? (a) : (b))

int closestTarget(char** words, int wordsSize, char* target, int startIndex) {
    int n = wordsSize;
    int ans = n;

    for (int i = 0; i < n; i++) {
        if (strcmp(words[i], target) == 0) {
            int t = abs(i - startIndex);
            int dist = min(t, n - t);
            ans = min(ans, dist);
        }
    }

    return ans == n ? -1 : ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
