---
comments: true
difficulty: Medium
tags:
    - Design
    - Array
    - String
---

<!-- problem:start -->

# [271. Encode and Decode Strings 🔒](https://leetcode.com/problems/encode-and-decode-strings)

[中文文档](/solution/0200-0299/0271.Encode%20and%20Decode%20Strings/README.md)

## Mô tả

<!-- description:start -->

<p>Thiết kế thuật toán để mã hóa <b>một danh sách chuỗi</b> thành <b>một chuỗi</b>. Chuỗi đã mã hóa được gửi qua network rồi giải mã để khôi phục danh sách chuỗi ban đầu.</p>

<p>Máy 1 (bên gửi) có hàm:</p>

<pre>
string encode(vector&lt;string&gt; strs) {
  // ... your code
  return encoded_string;
}</pre>

Máy 2 (bên nhận) có hàm:

<pre>
vector&lt;string&gt; decode(string s) {
  //... your code
  return strs;
}
</pre>

<p>Máy 1 thực hiện:</p>

<pre>
string encoded_string = encode(strs);
</pre>

<p>còn Máy 2 thực hiện:</p>

<pre>
vector&lt;string&gt; strs2 = decode(encoded_string);
</pre>

<p><code>strs2</code> ở Máy 2 phải giống với <code>strs</code> ở Máy 1.</p>

<p>Hãy triển khai các method <code>encode</code> và <code>decode</code>.</p>

<p>Không được giải bài toán bằng các method serialize (chẳng hạn như <code>eval</code>).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> dummy_input = [&quot;Hello&quot;,&quot;World&quot;]
<strong>Đầu ra:</strong> [&quot;Hello&quot;,&quot;World&quot;]
<strong>Giải thích:</strong>
Máy 1:
Codec encoder = new Codec();
String msg = encoder.encode(strs);
Máy 1 ---msg---&gt; Máy 2

Máy 2:
Codec decoder = new Codec();
String[] strs = decoder.decode(msg);
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> dummy_input = [&quot;&quot;]
<strong>Đầu ra:</strong> [&quot;&quot;]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= strs.length &lt;= 200</code></li>
	<li><code>0 &lt;= strs[i].length &lt;= 200</code></li>
	<li><code>strs[i]</code> có thể chứa bất kỳ ký tự nào trong số <code>256</code> ký tự ASCII hợp lệ.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng: </strong>Bạn có thể viết thuật toán tổng quát hoạt động với mọi tập ký tự có thể có không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mã hóa độ dài chuỗi

<!-- thinking:start -->

> **Tư duy**
>
> Một delimiter đơn lẻ có thể xuất hiện bên trong chuỗi. Thêm độ dài có độ rộng cố định ở đầu giúp xác định ranh giới chuỗi một cách rõ ràng.
>
> Encode ghi độ dài gồm $4$ ký tự rồi đến payload; decode đọc độ dài đó và cắt chuỗi tương ứng.

<!-- thinking:end -->

Khi mã hóa, ta chuyển độ dài của chuỗi thành một chuỗi cố định gồm 4 chữ số, ghép với chính chuỗi đó rồi lần lượt nối vào chuỗi kết quả.

Khi giải mã, trước tiên ta lấy bốn chữ số đầu của chuỗi để biết độ dài, rồi cắt phần chuỗi tiếp theo theo độ dài đó. Lặp lại cho đến khi thu được danh sách chuỗi.

Độ phức tạp thời gian là $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class Codec:
    def encode(self, strs: List[str]) -> str:
        """Encodes a list of strings to a single string."""
        ans = []
        for s in strs:
            ans.append('{:4}'.format(len(s)) + s)
        return ''.join(ans)

    def decode(self, s: str) -> List[str]:
        """Decodes a single string to a list of strings."""
        ans = []
        i, n = 0, len(s)
        while i < n:
            size = int(s[i : i + 4])
            i += 4
            ans.append(s[i : i + size])
            i += size
        return ans


# Your Codec object will be instantiated and called as such:
# codec = Codec()
# codec.decode(codec.encode(strs))
```

#### Java

```java
public class Codec {

    // Encodes a list of strings to a single string.
    public String encode(List<String> strs) {
        StringBuilder ans = new StringBuilder();
        for (String s : strs) {
            ans.append((char) s.length()).append(s);
        }
        return ans.toString();
    }

    // Decodes a single string to a list of strings.
    public List<String> decode(String s) {
        List<String> ans = new ArrayList<>();
        int i = 0, n = s.length();
        while (i < n) {
            int size = s.charAt(i++);
            ans.add(s.substring(i, i + size));
            i += size;
        }
        return ans;
    }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.decode(codec.encode(strs));
```

#### C++

```cpp
class Codec {
public:
    // Encodes a list of strings to a single string.
    string encode(vector<string>& strs) {
        string ans;
        for (string s : strs) {
            int size = s.size();
            ans += string((const char*) &size, sizeof(size));
            ans += s;
        }
        return ans;
    }

    // Decodes a single string to a list of strings.
    vector<string> decode(string s) {
        vector<string> ans;
        int i = 0, n = s.size();
        int size = 0;
        while (i < n) {
            memcpy(&size, s.data() + i, sizeof(size));
            i += sizeof(size);
            ans.push_back(s.substr(i, size));
            i += size;
        }
        return ans;
    }
};

// Your Codec object will be instantiated and called as such:
// Codec codec;
// codec.decode(codec.encode(strs));
```

#### Go

```go
type Codec struct {
}

// Encodes a list of strings to a single string.
func (codec *Codec) Encode(strs []string) string {
	ans := &bytes.Buffer{}
	for _, s := range strs {
		t := fmt.Sprintf("%04d", len(s))
		ans.WriteString(t)
		ans.WriteString(s)
	}
	return ans.String()
}

// Decodes a single string to a list of strings.
func (codec *Codec) Decode(strs string) []string {
	ans := []string{}
	i, n := 0, len(strs)
	for i < n {
		t := strs[i : i+4]
		i += 4
		size, _ := strconv.Atoi(t)
		ans = append(ans, strs[i:i+size])
		i += size
	}
	return ans
}

// Your Codec object will be instantiated and called as such:
// var codec Codec
// codec.Decode(codec.Encode(strs));
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
