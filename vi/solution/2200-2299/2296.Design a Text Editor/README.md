---
comments: true
difficulty: Hard
rating: 1911
source: Weekly Contest 296 Q4
tags:
    - Stack
    - Design
    - Linked List
    - String
    - Doubly-Linked List
    - Simulation
---

<!-- problem:start -->

# [2296. Design a Text Editor](https://leetcode.com/problems/design-a-text-editor)

[中文文档](/solution/2200-2299/2296.Design%20a%20Text%20Editor/README.md)

## Mô tả

<!-- description:start -->

<p>Thiết kế một trình soạn thảo văn bản có con trỏ với các thao tác sau:</p>

<ul>
	<li><strong>Thêm</strong> văn bản tại vị trí con trỏ.</li>
	<li><strong>Xóa</strong> văn bản tại vị trí con trỏ (mô phỏng phím backspace).</li>
	<li><strong>Di chuyển</strong> con trỏ sang trái hoặc phải.</li>
</ul>

<p>Khi xóa văn bản, chỉ các ký tự ở bên trái con trỏ bị xóa. Con trỏ luôn nằm trong phần văn bản thực tế và không thể di chuyển vượt qua phần văn bản đó. Cụ thể hơn, luôn có <code>0 &lt;= cursor.position &lt;= currentText.length</code>.</p>

<p>Hãy triển khai lớp <code>TextEditor</code>:</p>

<ul>
	<li><code>TextEditor()</code> Khởi tạo đối tượng với văn bản rỗng.</li>
	<li><code>void addText(string text)</code> Thêm <code>text</code> vào vị trí con trỏ. Sau thao tác, con trỏ nằm bên phải <code>text</code>.</li>
	<li><code>int deleteText(int k)</code> Xóa <code>k</code> ký tự ở bên trái con trỏ. Trả về số ký tự thực sự đã xóa.</li>
	<li><code>string cursorLeft(int k)</code> Di chuyển con trỏ sang trái <code>k</code> lần. Trả về <code>min(10, len)</code> ký tự cuối cùng ở bên trái con trỏ, trong đó <code>len</code> là số ký tự ở bên trái con trỏ.</li>
	<li><code>string cursorRight(int k)</code> Di chuyển con trỏ sang phải <code>k</code> lần. Trả về <code>min(10, len)</code> ký tự cuối cùng ở bên trái con trỏ, trong đó <code>len</code> là số ký tự ở bên trái con trỏ.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;TextEditor&quot;, &quot;addText&quot;, &quot;deleteText&quot;, &quot;addText&quot;, &quot;cursorRight&quot;, &quot;cursorLeft&quot;, &quot;deleteText&quot;, &quot;cursorLeft&quot;, &quot;cursorRight&quot;]
[[], [&quot;leetcode&quot;], [4], [&quot;practice&quot;], [3], [8], [10], [2], [6]]
<strong>Đầu ra</strong>
[null, null, 4, null, &quot;etpractice&quot;, &quot;leet&quot;, 4, &quot;&quot;, &quot;practi&quot;]

<strong>Giải thích</strong>
TextEditor textEditor = new TextEditor(); // The current text is &quot;|&quot;. (The &#39;|&#39; character represents the cursor)
textEditor.addText(&quot;leetcode&quot;); // The current text is &quot;leetcode|&quot;.
textEditor.deleteText(4); // return 4
                           // The current text is &quot;leet|&quot;.
                           // 4 characters were deleted.
textEditor.addText(&quot;practice&quot;); // The current text is &quot;leetpractice|&quot;.
textEditor.cursorRight(3); // return &quot;etpractice&quot;
                            // The current text is &quot;leetpractice|&quot;.
                            // The cursor cannot be moved beyond the actual text and thus did not move.
                            // &quot;etpractice&quot; is the last 10 characters to the left of the cursor.
textEditor.cursorLeft(8); // return &quot;leet&quot;
                           // The current text is &quot;leet|practice&quot;.
                           // &quot;leet&quot; is the last min(10, 4) = 4 characters to the left of the cursor.
textEditor.deleteText(10); // return 4
                            // The current text is &quot;|practice&quot;.
                            // Only 4 characters were deleted.
textEditor.cursorLeft(2); // return &quot;&quot;
                           // The current text is &quot;|practice&quot;.
                           // The cursor cannot be moved beyond the actual text and thus did not move.
                           // &quot;&quot; is the last min(10, 0) = 0 characters to the left of the cursor.
textEditor.cursorRight(6); // return &quot;practi&quot;
                            // The current text is &quot;practi|ce&quot;.
                            // &quot;practi&quot; is the last min(10, 6) = 6 characters to the left of the cursor.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= text.length, k &lt;= 40</code></li>
	<li><code>text</code> chỉ bao gồm các chữ cái tiếng Anh viết thường.</li>
	<li>Tổng số lần gọi <strong>tất cả</strong> các phương thức <code>addText</code>, <code>deleteText</code>, <code>cursorLeft</code> và <code>cursorRight</code> không vượt quá <code>2 * 10<sup>4</sup></code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Bạn có thể tìm được lời giải có độ phức tạp thời gian <code>O(k)</code> cho mỗi lần gọi không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai stack trái và phải

<!-- thinking:start -->

> **Tư duy**
>
> Trình soạn thảo cần chèn, xóa và di chuyển con trỏ; có $2\times 10^4$ lần gọi và mỗi đoạn văn bản đều ngắn. Nếu nối lại toàn bộ chuỗi sau mỗi lần di chuyển, ta sẽ phải sao chép quá nhiều dữ liệu. Con trỏ chia văn bản thành hai stack, cho phép chỉnh sửa tại vị trí phân chia với chi phí khấu hao $O(1)$.
>
> $\textit{left}$ là phần bên trái con trỏ, còn $\textit{right}$ là phần bên phải con trỏ (đỉnh stack hướng về phía con trỏ). Thao tác chèn và xóa chỉ tác động lên $\textit{left}$; thao tác di chuyển chuyển tối đa $k$ ký tự giữa hai stack. Kết quả trả về là mười ký tự cuối của $\textit{left}$.

<!-- thinking:end -->

Ta có thể sử dụng hai stack, $\textit{left}$ và $\textit{right}$, trong đó stack $\textit{left}$ lưu các ký tự ở bên trái con trỏ, còn stack $\textit{right}$ lưu các ký tự ở bên phải con trỏ.

- Khi gọi phương thức $\text{addText}$, lần lượt đưa các ký tự trong $\text{text}$ vào stack $\text{left}$ từng ký tự một. Độ phức tạp thời gian là $O(|\text{text}|)$.
- Khi gọi phương thức $\text{deleteText}$, lấy các ký tự ra khỏi stack $\text{left}$ tối đa $k$ lần. Độ phức tạp thời gian là $O(k)$.
- Khi gọi phương thức $\text{cursorLeft}$, lấy các ký tự ra khỏi stack $\text{left}$ tối đa $k$ lần, sau đó lần lượt đưa các ký tự đã lấy vào stack $\text{right}$, cuối cùng trả về tối đa 10 ký tự từ stack $\text{left}$. Độ phức tạp thời gian là $O(k)$.
- Khi gọi phương thức $\text{cursorRight}$, lấy các ký tự ra khỏi stack $\text{right}$ tối đa $k$ lần, sau đó lần lượt đưa các ký tự đã lấy vào stack $\text{left}$, cuối cùng trả về tối đa 10 ký tự từ stack $\text{left}$. Độ phức tạp thời gian là $O(k)$.

<!-- tabs:start -->

#### Python3

```python
class TextEditor:
    def __init__(self):
        self.left = []
        self.right = []

    def addText(self, text: str) -> None:
        self.left.extend(list(text))

    def deleteText(self, k: int) -> int:
        k = min(k, len(self.left))
        for _ in range(k):
            self.left.pop()
        return k

    def cursorLeft(self, k: int) -> str:
        k = min(k, len(self.left))
        for _ in range(k):
            self.right.append(self.left.pop())
        return ''.join(self.left[-10:])

    def cursorRight(self, k: int) -> str:
        k = min(k, len(self.right))
        for _ in range(k):
            self.left.append(self.right.pop())
        return ''.join(self.left[-10:])


# Your TextEditor object will be instantiated and called as such:
# obj = TextEditor()
# obj.addText(text)
# param_2 = obj.deleteText(k)
# param_3 = obj.cursorLeft(k)
# param_4 = obj.cursorRight(k)
```

#### Java

```java
class TextEditor {
    private StringBuilder left = new StringBuilder();
    private StringBuilder right = new StringBuilder();

    public TextEditor() {
    }

    public void addText(String text) {
        left.append(text);
    }

    public int deleteText(int k) {
        k = Math.min(k, left.length());
        left.setLength(left.length() - k);
        return k;
    }

    public String cursorLeft(int k) {
        k = Math.min(k, left.length());
        for (int i = 0; i < k; ++i) {
            right.append(left.charAt(left.length() - 1));
            left.deleteCharAt(left.length() - 1);
        }
        return left.substring(Math.max(left.length() - 10, 0));
    }

    public String cursorRight(int k) {
        k = Math.min(k, right.length());
        for (int i = 0; i < k; ++i) {
            left.append(right.charAt(right.length() - 1));
            right.deleteCharAt(right.length() - 1);
        }
        return left.substring(Math.max(left.length() - 10, 0));
    }
}

/**
 * Your TextEditor object will be instantiated and called as such:
 * TextEditor obj = new TextEditor();
 * obj.addText(text);
 * int param_2 = obj.deleteText(k);
 * String param_3 = obj.cursorLeft(k);
 * String param_4 = obj.cursorRight(k);
 */
```

#### C++

```cpp
class TextEditor {
public:
    TextEditor() {
    }

    void addText(string text) {
        left += text;
    }

    int deleteText(int k) {
        k = min(k, (int) left.size());
        left.resize(left.size() - k);
        return k;
    }

    string cursorLeft(int k) {
        k = min(k, (int) left.size());
        while (k--) {
            right += left.back();
            left.pop_back();
        }
        return left.substr(max(0, (int) left.size() - 10));
    }

    string cursorRight(int k) {
        k = min(k, (int) right.size());
        while (k--) {
            left += right.back();
            right.pop_back();
        }
        return left.substr(max(0, (int) left.size() - 10));
    }

private:
    string left, right;
};

/**
 * Your TextEditor object will be instantiated and called as such:
 * TextEditor* obj = new TextEditor();
 * obj->addText(text);
 * int param_2 = obj->deleteText(k);
 * string param_3 = obj->cursorLeft(k);
 * string param_4 = obj->cursorRight(k);
 */
```

#### Go

```go
type TextEditor struct {
	left, right []byte
}

func Constructor() TextEditor {
	return TextEditor{}
}

func (this *TextEditor) AddText(text string) {
	this.left = append(this.left, text...)
}

func (this *TextEditor) DeleteText(k int) int {
	k = min(k, len(this.left))
	if k < len(this.left) {
		this.left = this.left[:len(this.left)-k]
	} else {
		this.left = []byte{}
	}
	return k
}

func (this *TextEditor) CursorLeft(k int) string {
	k = min(k, len(this.left))
	for ; k > 0; k-- {
		this.right = append(this.right, this.left[len(this.left)-1])
		this.left = this.left[:len(this.left)-1]
	}
	return string(this.left[max(len(this.left)-10, 0):])
}

func (this *TextEditor) CursorRight(k int) string {
	k = min(k, len(this.right))
	for ; k > 0; k-- {
		this.left = append(this.left, this.right[len(this.right)-1])
		this.right = this.right[:len(this.right)-1]
	}
	return string(this.left[max(len(this.left)-10, 0):])
}

/**
 * Your TextEditor object will be instantiated and called as such:
 * obj := Constructor();
 * obj.AddText(text);
 * param_2 := obj.DeleteText(k);
 * param_3 := obj.CursorLeft(k);
 * param_4 := obj.CursorRight(k);
 */
```

#### TypeScript

```ts
class TextEditor {
    private left: string[];
    private right: string[];

    constructor() {
        this.left = [];
        this.right = [];
    }

    addText(text: string): void {
        this.left.push(...text);
    }

    deleteText(k: number): number {
        k = Math.min(k, this.left.length);
        for (let i = 0; i < k; i++) {
            this.left.pop();
        }
        return k;
    }

    cursorLeft(k: number): string {
        k = Math.min(k, this.left.length);
        for (let i = 0; i < k; i++) {
            this.right.push(this.left.pop()!);
        }
        return this.left.slice(-10).join('');
    }

    cursorRight(k: number): string {
        k = Math.min(k, this.right.length);
        for (let i = 0; i < k; i++) {
            this.left.push(this.right.pop()!);
        }
        return this.left.slice(-10).join('');
    }
}

/**
 * Your TextEditor object will be instantiated and called as such:
 * var obj = new TextEditor()
 * obj.addText(text)
 * var param_2 = obj.deleteText(k)
 * var param_3 = obj.cursorLeft(k)
 * var param_4 = obj.cursorRight(k)
 */
```

#### Rust

```rust
struct TextEditor {
    left: String,
    right: String,
}

impl TextEditor {
    fn new() -> Self {
        TextEditor {
            left: String::new(),
            right: String::new(),
        }
    }

    fn add_text(&mut self, text: String) {
        self.left.push_str(&text);
    }

    fn delete_text(&mut self, k: i32) -> i32 {
        let k = k.min(self.left.len() as i32) as usize;
        self.left.truncate(self.left.len() - k);
        k as i32
    }

    fn cursor_left(&mut self, k: i32) -> String {
        let k = k.min(self.left.len() as i32) as usize;
        for _ in 0..k {
            if let Some(c) = self.left.pop() {
                self.right.push(c);
            }
        }
        self.get_last_10_chars()
    }

    fn cursor_right(&mut self, k: i32) -> String {
        let k = k.min(self.right.len() as i32) as usize;
        for _ in 0..k {
            if let Some(c) = self.right.pop() {
                self.left.push(c);
            }
        }
        self.get_last_10_chars()
    }

    fn get_last_10_chars(&self) -> String {
        let len = self.left.len();
        self.left[len.saturating_sub(10)..].to_string()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
