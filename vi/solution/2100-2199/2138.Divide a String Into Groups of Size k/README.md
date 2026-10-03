---
comments: true
difficulty: Easy
rating: 1273
source: Weekly Contest 276 Q1
tags:
    - String
    - Simulation
---

<!-- problem:start -->

# [2138. Divide a String Into Groups of Size k](https://leetcode.com/problems/divide-a-string-into-groups-of-size-k)

[中文文档](/solution/2100-2199/2138.Divide%20a%20String%20Into%20Groups%20of%20Size%20k/README.md)

## Mô tả

<!-- description:start -->

<p>Một chuỗi <code>s</code> có thể được chia thành các nhóm có kích thước <code>k</code> theo quy trình sau:</p>

<ul>
	<li>Nhóm đầu tiên gồm <code>k</code> ký tự đầu tiên của chuỗi, nhóm thứ hai gồm <code>k</code> ký tự tiếp theo, và tiếp tục như vậy. Mỗi phần tử chỉ có thể thuộc về <strong>chính xác một</strong> nhóm.</li>
	<li>Đối với nhóm cuối, nếu chuỗi còn lại <strong>không đủ</strong> <code>k</code> ký tự, ta sử dụng một ký tự <code>fill</code> để hoàn thành nhóm.</li>
</ul>

<p>Lưu ý rằng việc chia được thực hiện sao cho sau khi loại bỏ ký tự <code>fill</code> khỏi nhóm cuối (nếu có) và nối tất cả các nhóm theo thứ tự, chuỗi thu được phải là <code>s</code>.</p>

<p>Cho chuỗi <code>s</code>, kích thước mỗi nhóm <code>k</code> và ký tự <code>fill</code>, hãy trả về <em>một mảng chuỗi biểu diễn <strong>các nhóm mà</strong> </em><code>s</code><em> đã được chia thành theo quy trình trên</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abcdefghi&quot;, k = 3, fill = &quot;x&quot;
<strong>Đầu ra:</strong> [&quot;abc&quot;,&quot;def&quot;,&quot;ghi&quot;]
<strong>Giải thích:</strong>
3 ký tự đầu tiên &quot;abc&quot; tạo thành nhóm đầu tiên.
3 ký tự tiếp theo &quot;def&quot; tạo thành nhóm thứ hai.
3 ký tự cuối cùng &quot;ghi&quot; tạo thành nhóm thứ ba.
Vì tất cả các nhóm đều có thể được lấp đầy hoàn toàn bằng các ký tự trong chuỗi, nên ta không cần sử dụng fill.
Do đó, các nhóm được tạo thành là &quot;abc&quot;, &quot;def&quot; và &quot;ghi&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;abcdefghij&quot;, k = 3, fill = &quot;x&quot;
<strong>Đầu ra:</strong> [&quot;abc&quot;,&quot;def&quot;,&quot;ghi&quot;,&quot;jxx&quot;]
<strong>Giải thích:</strong>
Tương tự ví dụ trước, ta tạo ba nhóm đầu tiên là &quot;abc&quot;, &quot;def&quot; và &quot;ghi&quot;.
Với nhóm cuối, ta chỉ có thể sử dụng ký tự &#39;j&#39; từ chuỗi. Để hoàn thành nhóm này, ta thêm &#39;x&#39; hai lần.
Do đó, bốn nhóm được tạo thành là &quot;abc&quot;, &quot;def&quot;, &quot;ghi&quot; và &quot;jxx&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>1 &lt;= k &lt;= 100</code></li>
	<li><code>fill</code> là một chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Chia $s$ thành các khối có độ dài $k$ và thêm $\textit{fill}$ vào khối cuối. Quy tắc này khá trực tiếp.
>
> Lấy các lát với bước nhảy $k$ và dùng $\texttt{ljust}$ cho từng lát.
>
> Số nhóm xấp xỉ $n/k$.

<!-- thinking:end -->

Ta có thể mô phỏng trực tiếp quy trình được mô tả trong đề bài, chia chuỗi $s$ thành các nhóm có độ dài $k$. Với nhóm cuối, nếu nhóm có ít hơn $k$ ký tự, ta sử dụng ký tự $\textit{fill}$ để thêm vào cho đủ.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def divideString(self, s: str, k: int, fill: str) -> List[str]:
        return [s[i : i + k].ljust(k, fill) for i in range(0, len(s), k)]
```

#### Java

```java
class Solution {
    public String[] divideString(String s, int k, char fill) {
        int n = s.length();
        String[] ans = new String[(n + k - 1) / k];
        if (n % k != 0) {
            s += String.valueOf(fill).repeat(k - n % k);
        }
        for (int i = 0; i < ans.length; ++i) {
            ans[i] = s.substring(i * k, (i + 1) * k);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> divideString(string s, int k, char fill) {
        int n = s.size();
        if (n % k) {
            s += string(k - n % k, fill);
        }
        vector<string> ans;
        for (int i = 0; i < s.size() / k; ++i) {
            ans.push_back(s.substr(i * k, k));
        }
        return ans;
    }
};
```

#### Go

```go
func divideString(s string, k int, fill byte) (ans []string) {
	n := len(s)
	if n%k != 0 {
		s += strings.Repeat(string(fill), k-n%k)
	}
	for i := 0; i < len(s)/k; i++ {
		ans = append(ans, s[i*k:(i+1)*k])
	}
	return
}
```

#### TypeScript

```ts
function divideString(s: string, k: number, fill: string): string[] {
    const ans: string[] = [];
    for (let i = 0; i < s.length; i += k) {
        ans.push(s.slice(i, i + k).padEnd(k, fill));
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
