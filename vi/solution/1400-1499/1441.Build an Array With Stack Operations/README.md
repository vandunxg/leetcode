---
comments: true
difficulty: Medium
rating: 1180
source: Weekly Contest 188 Q1
tags:
    - Stack
    - Array
    - Simulation
---

<!-- problem:start -->

# [1441. Build an Array With Stack Operations](https://leetcode.com/problems/build-an-array-with-stack-operations)

[中文文档](/solution/1400-1499/1441.Build%20an%20Array%20With%20Stack%20Operations/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>target</code> và một số nguyên <code>n</code>.</p>

<p>Bạn có một stack rỗng với hai thao tác sau:</p>

<ul>
	<li><strong><code>&quot;Push&quot;</code></strong>: đẩy một số nguyên lên đỉnh stack.</li>
	<li><strong><code>&quot;Pop&quot;</code></strong>: xóa số nguyên trên đỉnh stack.</li>
</ul>

<p>Bạn cũng có một stream gồm các số nguyên trong khoảng <code>[1, n]</code>.</p>

<p>Sử dụng hai thao tác trên stack để tạo ra stack mà các số trong đó (từ đáy lên đỉnh) bằng với <code>target</code>. Bạn cần tuân theo các quy tắc sau:</p>

<ul>
	<li>Nếu stream số nguyên chưa rỗng, lấy số nguyên tiếp theo từ stream và đẩy nó lên đỉnh stack.</li>
	<li>Nếu stack chưa rỗng, lấy số nguyên trên đỉnh stack ra.</li>
	<li>Nếu tại bất kỳ thời điểm nào, các phần tử trong stack (từ đáy lên đỉnh) bằng với <code>target</code>, không đọc thêm số nguyên từ stream và không thực hiện thêm thao tác nào trên stack.</li>
</ul>

<p>Trả về <em>các thao tác trên stack cần thiết để tạo ra </em><code>target</code> theo các quy tắc trên. Nếu có nhiều đáp án hợp lệ, trả về <strong>bất kỳ đáp án nào</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> target = [1,3], n = 3
<strong>Đầu ra:</strong> [&quot;Push&quot;,&quot;Push&quot;,&quot;Pop&quot;,&quot;Push&quot;]
<strong>Giải thích:</strong> Ban đầu stack s rỗng. Phần tử cuối cùng là đỉnh stack.
Đọc 1 từ stream và đẩy nó vào stack. s = [1].
Đọc 2 từ stream và đẩy nó vào stack. s = [1,2].
Lấy số nguyên trên đỉnh stack ra. s = [1].
Đọc 3 từ stream và đẩy nó vào stack. s = [1,3].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> target = [1,2,3], n = 3
<strong>Đầu ra:</strong> [&quot;Push&quot;,&quot;Push&quot;,&quot;Push&quot;]
<strong>Giải thích:</strong> Ban đầu stack s rỗng. Phần tử cuối cùng là đỉnh stack.
Đọc 1 từ stream và đẩy nó vào stack. s = [1].
Đọc 2 từ stream và đẩy nó vào stack. s = [1,2].
Đọc 3 từ stream và đẩy nó vào stack. s = [1,2,3].
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> target = [1,2], n = 4
<strong>Đầu ra:</strong> [&quot;Push&quot;,&quot;Push&quot;]
<strong>Giải thích:</strong> Ban đầu stack s rỗng. Phần tử cuối cùng là đỉnh stack.
Đọc 1 từ stream và đẩy nó vào stack. s = [1].
Đọc 2 từ stream và đẩy nó vào stack. s = [1,2].
Vì stack (từ đáy lên đỉnh) bằng với target, chúng ta dừng các thao tác trên stack.
Các đáp án đọc số nguyên 3 từ stream không được chấp nhận.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= target.length &lt;= 100</code></li>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>1 &lt;= target[i] &lt;= n</code></li>
	<li><code>target</code> tăng nghiêm ngặt.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Stream là $1,2,\ldots,n$ và `target` tăng nghiêm ngặt. Các số không có trong `target` cần `Push` rồi `Pop`; các số có trong `target` chỉ cần `Push`.
>
> Một con trỏ $\textit{cur}$ là giá trị tiếp theo trong stream. Với mỗi $x$ trong target, thêm Push/Pop cho khoảng trống, sau đó thêm Push cho $x$. $n\le 100$.

<!-- thinking:end -->

Ta định nghĩa biến $\textit{cur}$ biểu diễn số hiện tại cần đọc, ban đầu đặt $\textit{cur} = 1$, và sử dụng mảng $\textit{ans}$ để lưu đáp án.

Tiếp theo, ta duyệt qua từng số $x$ trong mảng $\textit{target}$:

- Nếu $\textit{cur} < x$, ta lần lượt thêm $\textit{Push}$ và $\textit{Pop}$ vào đáp án cho đến khi $\textit{cur} = x$;
- Sau đó ta thêm $\textit{Push}$ vào đáp án, tương ứng với việc đọc số $x$;
- Tiếp đó, ta tăng $\textit{cur}$ và tiếp tục xử lý số tiếp theo.

Sau khi duyệt xong, ta trả về mảng đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{target}$. Không tính phần bộ nhớ của mảng đáp án, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def buildArray(self, target: List[int], n: int) -> List[str]:
        ans = []
        cur = 1
        for x in target:
            while cur < x:
                ans.extend(["Push", "Pop"])
                cur += 1
            ans.append("Push")
            cur += 1
        return ans
```

#### Java

```java
class Solution {
    public List<String> buildArray(int[] target, int n) {
        List<String> ans = new ArrayList<>();
        int cur = 1;
        for (int x : target) {
            while (cur < x) {
                ans.addAll(List.of("Push", "Pop"));
                ++cur;
            }
            ans.add("Push");
            ++cur;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> buildArray(vector<int>& target, int n) {
        vector<string> ans;
        int cur = 1;
        for (int x : target) {
            while (cur < x) {
                ans.push_back("Push");
                ans.push_back("Pop");
                ++cur;
            }
            ans.push_back("Push");
            ++cur;
        }
        return ans;
    }
};
```

#### Go

```go
func buildArray(target []int, n int) (ans []string) {
	cur := 1
	for _, x := range target {
		for ; cur < x; cur++ {
			ans = append(ans, "Push", "Pop")
		}
		ans = append(ans, "Push")
		cur++
	}
	return
}
```

#### TypeScript

```ts
function buildArray(target: number[], n: number): string[] {
    const ans: string[] = [];
    let cur: number = 1;
    for (const x of target) {
        for (; cur < x; ++cur) {
            ans.push('Push', 'Pop');
        }
        ans.push('Push');
        ++cur;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn build_array(target: Vec<i32>, n: i32) -> Vec<String> {
        let mut ans = Vec::new();
        let mut cur = 1;
        for &x in &target {
            while cur < x {
                ans.push("Push".to_string());
                ans.push("Pop".to_string());
                cur += 1;
            }
            ans.push("Push".to_string());
            cur += 1;
        }
        ans
    }
}
```

#### C

```c
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
char** buildArray(int* target, int targetSize, int n, int* returnSize) {
    char** ans = (char**) malloc(sizeof(char*) * (2 * n));
    *returnSize = 0;
    int cur = 1;
    for (int i = 0; i < targetSize; i++) {
        while (cur < target[i]) {
            ans[(*returnSize)++] = "Push";
            ans[(*returnSize)++] = "Pop";
            cur++;
        }
        ans[(*returnSize)++] = "Push";
        cur++;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
