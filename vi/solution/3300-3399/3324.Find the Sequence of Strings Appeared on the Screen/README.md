---
comments: true
difficulty: Medium
rating: 1293
source: Weekly Contest 420 Q1
tags:
    - String
    - Simulation
---

<!-- problem:start -->

# [3324. Find the Sequence of Strings Appeared on the Screen](https://leetcode.com/problems/find-the-sequence-of-strings-appeared-on-the-screen)

[中文文档](/solution/3300-3399/3324.Find%20the%20Sequence%20of%20Strings%20Appeared%20on%20the%20Screen/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>target</code>.</p>

<p>Alice sẽ gõ <code>target</code> trên máy tính bằng một bàn phím đặc biệt chỉ có <strong>hai</strong> phím:</p>

<ul>
    <li>Phím 1 nối thêm ký tự <code>&quot;a&quot;</code> vào chuỗi trên màn hình.</li>
    <li>Phím 2 thay đổi ký tự <strong>cuối cùng</strong> của chuỗi trên màn hình thành ký tự <strong>kế tiếp</strong> trong bảng chữ cái tiếng Anh. Ví dụ, <code>&quot;c&quot;</code> đổi thành <code>&quot;d&quot;</code> và <code>&quot;z&quot;</code> đổi thành <code>&quot;a&quot;</code>.</li>
</ul>

<p><strong>Lưu ý</strong> rằng ban đầu màn hình hiển thị một chuỗi <em>rỗng</em> <code>&quot;&quot;</code>, nên cô ấy <strong>chỉ có thể</strong> nhấn phím 1.</p>

<p>Hãy trả về danh sách <em>tất cả</em> các chuỗi xuất hiện trên màn hình khi Alice gõ <code>target</code>, theo thứ tự xuất hiện, bằng cách sử dụng số lần nhấn phím <strong>ít nhất</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">target = &quot;abc&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[&quot;a&quot;,&quot;aa&quot;,&quot;ab&quot;,&quot;aba&quot;,&quot;abb&quot;,&quot;abc&quot;]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi thao tác nhấn phím của Alice là:</p>

<ul>
    <li>Nhấn phím 1, chuỗi trên màn hình trở thành <code>&quot;a&quot;</code>.</li>
    <li>Nhấn phím 1, chuỗi trên màn hình trở thành <code>&quot;aa&quot;</code>.</li>
    <li>Nhấn phím 2, chuỗi trên màn hình trở thành <code>&quot;ab&quot;</code>.</li>
    <li>Nhấn phím 1, chuỗi trên màn hình trở thành <code>&quot;aba&quot;</code>.</li>
    <li>Nhấn phím 2, chuỗi trên màn hình trở thành <code>&quot;abb&quot;</code>.</li>
    <li>Nhấn phím 2, chuỗi trên màn hình trở thành <code>&quot;abc&quot;</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">target = &quot;he&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[&quot;a&quot;,&quot;b&quot;,&quot;c&quot;,&quot;d&quot;,&quot;e&quot;,&quot;f&quot;,&quot;g&quot;,&quot;h&quot;,&quot;ha&quot;,&quot;hb&quot;,&quot;hc&quot;,&quot;hd&quot;,&quot;he&quot;]</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= target.length &lt;= 400</code></li>
    <li><code>target</code> chỉ chứa các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Màn hình bắt đầu bằng một chuỗi rỗng; mỗi phím nối thêm một chữ cái hoặc tăng ký tự cuối lên một bậc. Với $|\textit{target}| \le 400$, ta có thể mô phỏng lần lượt từng chữ cái.
>
> Với mỗi ký tự trong target, ta nối thêm lần lượt $\texttt{a}$, rồi $\texttt{b}$, v.v. vào prefix hiện tại cho đến khi ký tự đó xuất hiện. Mọi chuỗi trung gian đều thuộc sequence cần trả về.
>
> Không cần tìm kiếm các thao tác nhấn phím: chỉ cần duyệt $\textit{ascii\_lowercase}$ đến ký tự đích là có thể ghi lại mọi trạng thái trên màn hình.

<!-- thinking:end -->

Ta có thể mô phỏng quá trình gõ của Alice, bắt đầu từ một chuỗi rỗng và cập nhật chuỗi sau mỗi lần nhấn phím cho đến khi thu được chuỗi đích.

Độ phức tạp thời gian là $O(n^2 \times |\Sigma|)$, trong đó $n$ là độ dài của chuỗi đích và $\Sigma$ là tập ký tự; trong bài này, đó là tập các chữ cái viết thường, nên $|\Sigma| = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def stringSequence(self, target: str) -> List[str]:
        ans = []
        for c in target:
            s = ans[-1] if ans else ""
            for a in ascii_lowercase:
                t = s + a
                ans.append(t)
                if a == c:
                    break
        return ans
```

#### Java

```java
class Solution {
    public List<String> stringSequence(String target) {
        List<String> ans = new ArrayList<>();
        for (char c : target.toCharArray()) {
            String s = ans.isEmpty() ? "" : ans.get(ans.size() - 1);
            for (char a = 'a'; a <= c; ++a) {
                String t = s + a;
                ans.add(t);
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
    vector<string> stringSequence(string target) {
        vector<string> ans;
        for (char c : target) {
            string s = ans.empty() ? "" : ans.back();
            for (char a = 'a'; a <= c; ++a) {
                string t = s + a;
                ans.push_back(t);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func stringSequence(target string) (ans []string) {
    for _, c := range target {
        s := ""
        if len(ans) > 0 {
            s = ans[len(ans)-1]
        }
        for a := 'a'; a <= c; a++ {
            t := s + string(a)
            ans = append(ans, t)
        }
    }
    return
}
```

#### TypeScript

```ts
function stringSequence(target: string): string[] {
    const ans: string[] = [];
    for (const c of target) {
        let s = ans.length > 0 ? ans[ans.length - 1] : '';
        for (let a = 'a'.charCodeAt(0); a <= c.charCodeAt(0); a++) {
            const t = s + String.fromCharCode(a);
            ans.push(t);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
