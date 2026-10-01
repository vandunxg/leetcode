---
comments: true
difficulty: Medium
tags:
    - Stack
    - Tree
    - String
    - Binary Tree
---

<!-- problem:start -->

# [331. Verify Preorder Serialization of a Binary Tree](https://leetcode.com/problems/verify-preorder-serialization-of-a-binary-tree)

[中文文档](/solution/0300-0399/0331.Verify%20Preorder%20Serialization%20of%20a%20Binary%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Một cách serialize cây nhị phân là dùng <strong>preorder traversal</strong>. Khi gặp node không null, ta ghi lại giá trị của node. Nếu đó là node null, ta ghi một giá trị sentinel như <code>&#39;#&#39;</code>.</p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0331.Verify%20Preorder%20Serialization%20of%20a%20Binary%20Tree/images/pre-tree.jpg" style="width: 362px; height: 293px;" />
<p>Ví dụ, cây nhị phân trên có thể được serialize thành chuỗi <code>&quot;9,3,4,#,#,1,#,#,2,#,6,#,#&quot;</code>, trong đó <code>&#39;#&#39;</code> biểu thị node null.</p>

<p>Cho chuỗi <code>preorder</code> gồm các giá trị phân tách bằng dấu phẩy. Trả về <code>true</code> nếu chuỗi là serialization hợp lệ của phép duyệt preorder trên cây nhị phân.</p>

<p><strong>Đảm bảo</strong> mỗi giá trị trong chuỗi, được phân tách bằng dấu phẩy, là số nguyên hoặc ký tự <code>&#39;#&#39;</code> biểu thị null pointer.</p>

<p>Bạn có thể giả định định dạng đầu vào luôn hợp lệ.</p>

<ul>
	<li>Ví dụ, chuỗi không bao giờ có hai dấu phẩy liên tiếp như <code>&quot;1,,3&quot;</code>.</li>
</ul>

<p><strong>Lưu ý:&nbsp;</strong>Bạn không được phép dựng lại cây.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> preorder = "9,3,4,#,#,1,#,#,2,#,6,#,#"
<strong>Đầu ra:</strong> true
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> preorder = "1,#"
<strong>Đầu ra:</strong> false
</pre><p><strong class="example">Ví dụ 3:</strong></p>
<pre><strong>Đầu vào:</strong> preorder = "9,#,#,1"
<strong>Đầu ra:</strong> false
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= preorder.length &lt;= 10<sup>4</sup></code></li>
	<li><code>preorder</code> gồm các số nguyên trong khoảng <code>[0, 100]</code> và ký tự <code>&#39;#&#39;</code>, được phân tách bằng dấu phẩy <code>&#39;,&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Stack

<!-- thinking:start -->

> **Tư duy**
>
> Kiểm tra xem chuỗi preorder phân tách bằng dấu phẩy có biểu diễn một cây nhị phân hợp lệ hay không. Mỗi node có hai node con; node null được ký hiệu là `#`. Có thể đếm slot, hoặc rút gọn trực tiếp bằng stack.
>
> Mẫu `value # #` biểu diễn một subtree lá hoàn chỉnh và được rút gọn thành một `#`. Sau khi rút gọn hết, nếu serialization hợp lệ thì chỉ còn lại một `#`. Nếu một node không null không có đủ hai node con, các token sẽ còn dư.

<!-- thinking:end -->

Ta tách chuỗi `preorder` thành mảng theo dấu phẩy rồi duyệt mảng. Nếu gặp hai ký tự `'#'` liên tiếp và phần tử thứ ba không phải `'#'`, ta thay ba phần tử này bằng một `'#'`. Tiếp tục như vậy cho đến khi duyệt hết mảng.

Cuối cùng, ta kiểm tra xem độ dài mảng có bằng $1$ và phần tử duy nhất trong mảng có phải là `'#'` hay không.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài chuỗi `preorder`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isValidSerialization(self, preorder: str) -> bool:
        stk = []
        for c in preorder.split(","):
            stk.append(c)
            while len(stk) > 2 and stk[-1] == stk[-2] == "#" and stk[-3] != "#":
                stk = stk[:-3]
                stk.append("#")
        return len(stk) == 1 and stk[0] == "#"
```

#### Java

```java
class Solution {
    public boolean isValidSerialization(String preorder) {
        List<String> stk = new ArrayList<>();
        for (String s : preorder.split(",")) {
            stk.add(s);
            while (stk.size() >= 3 && stk.get(stk.size() - 1).equals("#")
                && stk.get(stk.size() - 2).equals("#") && !stk.get(stk.size() - 3).equals("#")) {
                stk.remove(stk.size() - 1);
                stk.remove(stk.size() - 1);
                stk.remove(stk.size() - 1);
                stk.add("#");
            }
        }
        return stk.size() == 1 && stk.get(0).equals("#");
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isValidSerialization(string preorder) {
        vector<string> stk;
        stringstream ss(preorder);
        string s;
        while (getline(ss, s, ',')) {
            stk.push_back(s);
            while (stk.size() >= 3 && stk[stk.size() - 1] == "#" && stk[stk.size() - 2] == "#" && stk[stk.size() - 3] != "#") {
                stk.pop_back();
                stk.pop_back();
                stk.pop_back();
                stk.push_back("#");
            }
        }
        return stk.size() == 1 && stk[0] == "#";
    }
};
```

#### Go

```go
func isValidSerialization(preorder string) bool {
	stk := []string{}
	for _, s := range strings.Split(preorder, ",") {
		stk = append(stk, s)
		for len(stk) >= 3 && stk[len(stk)-1] == "#" && stk[len(stk)-2] == "#" && stk[len(stk)-3] != "#" {
			stk = stk[:len(stk)-3]
			stk = append(stk, "#")
		}
	}
	return len(stk) == 1 && stk[0] == "#"
}
```

#### TypeScript

```ts
function isValidSerialization(preorder: string): boolean {
    const stk: string[] = [];
    for (const s of preorder.split(',')) {
        stk.push(s);
        while (stk.length >= 3 && stk.at(-1) === '#' && stk.at(-2) === '#' && stk.at(-3) !== '#') {
            stk.splice(-3, 3, '#');
        }
    }
    return stk.length === 1 && stk[0] === '#';
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
