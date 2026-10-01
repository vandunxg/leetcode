---
comments: true
difficulty: Medium
tags:
    - Stack
    - Depth-First Search
    - String
---

<!-- problem:start -->

# [388. Longest Absolute File Path](https://leetcode.com/problems/longest-absolute-file-path)

[中文文档](/solution/0300-0399/0388.Longest%20Absolute%20File%20Path/README.md)

## Mô tả

<!-- description:start -->

<p>Giả sử ta có một hệ thống file lưu cả file lẫn thư mục. Hình dưới đây minh họa một ví dụ:</p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0388.Longest%20Absolute%20File%20Path/images/mdir.jpg" style="width: 681px; height: 322px;" /></p>

<p>Ở đây, <code>dir</code> là thư mục duy nhất ở root. <code>dir</code> chứa hai thư mục con <code>subdir1</code> và <code>subdir2</code>. <code>subdir1</code> chứa file <code>file1.ext</code> và thư mục con <code>subsubdir1</code>. <code>subdir2</code> chứa thư mục con <code>subsubdir2</code>, bên trong có file <code>file2.ext</code>.</p>

<p>Dạng văn bản của cấu trúc này như sau (ký hiệu ⟶ biểu thị ký tự tab):</p>

<pre>
dir
⟶ subdir1
⟶ ⟶ file1.ext
⟶ ⟶ subsubdir1
⟶ subdir2
⟶ ⟶ subsubdir2
⟶ ⟶ ⟶ file2.ext
</pre>

<p>Nếu biểu diễn cấu trúc này trong code, ta có chuỗi <code>&quot;dir\n\tsubdir1\n\t\tfile1.ext\n\t\tsubsubdir1\n\tsubdir2\n\t\tsubsubdir2\n\t\t\tfile2.ext&quot;</code>. Lưu ý, <code>&#39;\n&#39;</code> là ký tự xuống dòng và <code>&#39;\t&#39;</code> là ký tự tab.</p>

<p>Mỗi file và thư mục có một <strong>đường dẫn tuyệt đối</strong> duy nhất trong hệ thống file. Đường dẫn này liệt kê các thư mục cần đi qua để tới file hoặc thư mục đó, nối với nhau bằng <code>&#39;/&#39;s</code>. Trong ví dụ trên, <strong>đường dẫn tuyệt đối</strong> tới <code>file2.ext</code> là <code>&quot;dir/subdir2/subsubdir2/file2.ext&quot;</code>. Tên mỗi thư mục gồm chữ cái, chữ số và/hoặc dấu cách. Tên mỗi file có dạng <code>name.extension</code>, trong đó <code>name</code> và <code>extension</code> gồm chữ cái, chữ số và/hoặc dấu cách.</p>

<p>Cho chuỗi <code>input</code> biểu diễn hệ thống file theo định dạng trên. Hãy trả về <em>độ dài của <strong>đường dẫn tuyệt đối dài nhất</strong> tới một <strong>file</strong> trong hệ thống file được mô hình hóa</em>. Nếu hệ thống không có file nào, trả về <code>0</code>.</p>

<p><strong>Lưu ý</strong>, các test được tạo sao cho hệ thống file hợp lệ và tên của mọi file, thư mục đều có độ dài khác 0.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0388.Longest%20Absolute%20File%20Path/images/dir1.jpg" style="width: 401px; height: 202px;" />
<pre>
<strong>Đầu vào:</strong> input = &quot;dir\n\tsubdir1\n\tsubdir2\n\t\tfile.ext&quot;
<strong>Đầu ra:</strong> 20
<strong>Giải thích:</strong> Chỉ có một file, với đường dẫn tuyệt đối &quot;dir/subdir2/file.ext&quot; có độ dài 20.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0388.Longest%20Absolute%20File%20Path/images/dir2.jpg" style="width: 641px; height: 322px;" />
<pre>
<strong>Đầu vào:</strong> input = &quot;dir\n\tsubdir1\n\t\tfile1.ext\n\t\tsubsubdir1\n\tsubdir2\n\t\tsubsubdir2\n\t\t\tfile2.ext&quot;
<strong>Đầu ra:</strong> 32
<strong>Giải thích:</strong> Có hai file:
&quot;dir/subdir1/file1.ext&quot; có độ dài 21
&quot;dir/subdir2/subsubdir2/file2.ext&quot; có độ dài 32.
Ta trả về 32 vì đây là đường dẫn tuyệt đối dài nhất tới một file.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> input = &quot;a&quot;
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có file nào, chỉ có một thư mục tên &quot;a&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= input.length &lt;= 10<sup>4</sup></code></li>
	<li><code>input</code> có thể chứa chữ cái tiếng Anh viết thường hoặc viết hoa, ký tự xuống dòng <code>&#39;\n&#39;</code>, ký tự tab <code>&#39;\t&#39;</code>, dấu chấm <code>&#39;.&#39;</code>, dấu cách <code>&#39; &#39;</code> và chữ số.</li>
	<li>Tên của mọi file và thư mục đều có độ dài <strong>dương</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Cấu trúc là một cây được thụt lề bằng tab; cần tìm đường dẫn file tuyệt đối dài nhất. Mô phỏng thao tác `cd` theo từng dòng giúp không phải dựng cây.
>
> Stack lưu độ dài đường dẫn ở từng depth. Pop các mục khi mức thụt lề hiện tại nông hơn. Với thư mục, push độ dài đường dẫn cha cộng một dấu phân cách và độ dài tên thư mục; với file, cập nhật đáp án. Dấu chấm cho biết một mục là file.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def lengthLongestPath(self, input: str) -> int:
        i, n = 0, len(input)
        ans = 0
        stk = []
        while i < n:
            ident = 0
            while input[i] == '\t':
                ident += 1
                i += 1

            cur, isFile = 0, False
            while i < n and input[i] != '\n':
                cur += 1
                if input[i] == '.':
                    isFile = True
                i += 1
            i += 1

            # popd
            while len(stk) > 0 and len(stk) > ident:
                stk.pop()

            if len(stk) > 0:
                cur += stk[-1] + 1

            # pushd
            if not isFile:
                stk.append(cur)
                continue

            ans = max(ans, cur)

        return ans
```

#### Java

```java
class Solution {
    public int lengthLongestPath(String input) {
        int i = 0;
        int n = input.length();
        int ans = 0;
        Deque<Integer> stack = new ArrayDeque<>();
        while (i < n) {
            int ident = 0;
            for (; input.charAt(i) == '\t'; i++) {
                ident++;
            }

            int cur = 0;
            boolean isFile = false;
            for (; i < n && input.charAt(i) != '\n'; i++) {
                cur++;
                if (input.charAt(i) == '.') {
                    isFile = true;
                }
            }
            i++;

            // popd
            while (!stack.isEmpty() && stack.size() > ident) {
                stack.pop();
            }

            if (stack.size() > 0) {
                cur += stack.peek() + 1;
            }

            // pushd
            if (!isFile) {
                stack.push(cur);
                continue;
            }

            ans = Math.max(ans, cur);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int lengthLongestPath(string input) {
        int i = 0, n = input.size();
        int ans = 0;
        stack<int> stk;
        while (i < n) {
            int ident = 0;
            for (; input[i] == '\t'; ++i) {
                ++ident;
            }

            int cur = 0;
            bool isFile = false;
            for (; i < n && input[i] != '\n'; ++i) {
                ++cur;
                if (input[i] == '.') {
                    isFile = true;
                }
            }
            ++i;

            // popd
            while (!stk.empty() && stk.size() > ident) {
                stk.pop();
            }

            if (stk.size() > 0) {
                cur += stk.top() + 1;
            }

            // pushd
            if (!isFile) {
                stk.push(cur);
                continue;
            }

            ans = max(ans, cur);
        }
        return ans;
    }
};
```

#### Go

```go
func lengthLongestPath(input string) int {
	i, n := 0, len(input)
	ans := 0
	var stk []int
	for i < n {
		ident := 0
		for ; input[i] == '\t'; i++ {
			ident++
		}

		cur, isFile := 0, false
		for ; i < n && input[i] != '\n'; i++ {
			cur++
			if input[i] == '.' {
				isFile = true
			}
		}
		i++

		// popd
		for len(stk) > 0 && len(stk) > ident {
			stk = stk[:len(stk)-1]
		}

		if len(stk) > 0 {
			cur += stk[len(stk)-1] + 1
		}

		// pushd
		if !isFile {
			stk = append(stk, cur)
			continue
		}

		ans = max(ans, cur)
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
