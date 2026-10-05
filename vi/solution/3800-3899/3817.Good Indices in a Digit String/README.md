---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [3817. Good Indices in a Digit String 🔒](https://leetcode.com/problems/good-indices-in-a-digit-string)

[中文文档](/solution/3800-3899/3817.Good%20Indices%20in%20a%20Digit%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> chỉ gồm các chữ số.</p>

<p>Một chỉ số <code>i</code> được gọi là <strong>tốt</strong> nếu tồn tại một <span data-keyword="substring-nonempty">chuỗi con</span> của <code>s</code> kết thúc tại chỉ số <code>i</code> và bằng biểu diễn thập phân của <code>i</code>.</p>

<p>Hãy trả về một mảng số nguyên gồm tất cả các chỉ số tốt theo <strong>thứ tự tăng dần</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;0234567890112&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0,11,12]</span></p>

<p><strong>Giải thích:​​​​​​​</strong></p>

<ul>
	<li>
	<p>Tại chỉ số 0, biểu diễn thập phân của chỉ số là <code>&quot;0&quot;</code>. Chuỗi con <code>s[0]</code> là <code>&quot;0&quot;</code>, trùng khớp, nên chỉ số <code>0</code> là chỉ số tốt.</p>
	</li>
	<li>
	<p>Tại chỉ số 11, biểu diễn thập phân là <code>&quot;11&quot;</code>. Chuỗi con <code>s[10..11]</code> là <code>&quot;11&quot;</code>, trùng khớp, nên chỉ số <code>11</code> là chỉ số tốt.</p>
	</li>
	<li>
	<p>Tại chỉ số 12, biểu diễn thập phân là <code>&quot;12&quot;</code>. Chuỗi con <code>s[11..12]</code> là <code>&quot;12&quot;</code>, trùng khớp, nên chỉ số <code>12</code> là chỉ số tốt.</p>
	</li>
</ul>

<p>Không có chỉ số nào khác có chuỗi con kết thúc tại nó bằng với biểu diễn thập phân của chỉ số đó. Vì vậy, đáp án là <code>[0, 11, 12]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;01234&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0,1,2,3,4]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Với mọi chỉ số <code>i</code> từ 0 đến 4, biểu diễn thập phân của <code>i</code> chỉ gồm một chữ số, và chuỗi con <code>s[i]</code> trùng khớp với chữ số đó.</p>

<p>Vì vậy, tồn tại một chuỗi con hợp lệ kết thúc tại mỗi chỉ số, nên tất cả các chỉ số đều là chỉ số tốt.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;12345&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có chỉ số nào có chuỗi con kết thúc tại nó trùng khớp với biểu diễn thập phân của chỉ số đó.</p>

<p>Vì vậy, không có chỉ số tốt nào và kết quả là một mảng rỗng.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ số từ <code>&#39;0&#39;</code> đến <code>&#39;9&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ số $i$ là chỉ số tốt khi và chỉ khi có một chuỗi con kết thúc tại $i$ bằng biểu diễn thập phân của $i$. $|s| \le 10^5$, nhưng biểu diễn đó có độ dài không quá $6$.
>
> Với mỗi $i$, ta chỉ cần so sánh hậu tố có độ dài $|\mathrm{str}(i)|$; các chuỗi con khác không thể trùng khớp.
>
> Ta duyệt qua từng chỉ số và kiểm tra $s[i+1-k:i+1]$ với $\mathrm{str}(i)$.
>
> Tổng số phép so sánh là tuyến tính theo $n$.

<!-- thinking:end -->

Ta nhận thấy độ dài lớn nhất của chuỗi $s$ là $10^5$, còn độ dài biểu diễn thập phân của chỉ số $i$ nhiều nhất là $6$ (vì biểu diễn thập phân của $10^5$ là $100000$, có độ dài $6$). Do đó, với mỗi chỉ số $i$, ta chỉ cần kiểm tra xem chuỗi con tương ứng với biểu diễn thập phân của nó có bằng biểu diễn đó hay không.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$. Độ phức tạp không gian là $O(1)$, không tính phần không gian cần cho đáp án.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def goodIndices(self, s: str) -> List[int]:
        ans = []
        for i in range(len(s)):
            t = str(i)
            k = len(t)
            if s[i + 1 - k : i + 1] == t:
                ans.append(i)
        return ans
```

#### Java

```java
class Solution {
    public List<Integer> goodIndices(String s) {
        List<Integer> ans = new ArrayList<>();
        for (int i = 0; i < s.length(); i++) {
            String t = String.valueOf(i);
            int k = t.length();
            if (s.substring(i + 1 - k, i + 1).equals(t)) {
                ans.add(i);
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
    vector<int> goodIndices(string s) {
        vector<int> ans;
        for (int i = 0; i < s.size(); i++) {
            string t = to_string(i);
            int k = t.size();
            if (s.substr(i + 1 - k, k) == t) {
                ans.push_back(i);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func goodIndices(s string) (ans []int) {
	for i := range s {
		t := strconv.Itoa(i)
		k := len(t)
		if s[i+1-k:i+1] == t {
			ans = append(ans, i)
		}
	}
	return
}
```

#### TypeScript

```ts
function goodIndices(s: string): number[] {
    const ans: number[] = [];
    for (let i = 0; i < s.length; i++) {
        const t = String(i);
        const k = t.length;
        if (s.slice(i + 1 - k, i + 1) === t) {
            ans.push(i);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
