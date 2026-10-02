---
comments: true
difficulty: Medium
rating: 1453
source: Weekly Contest 192 Q3
tags:
    - Stack
    - Design
    - Array
    - Linked List
    - Data Stream
    - Doubly-Linked List
---

<!-- problem:start -->

# [1472. Design Browser History](https://leetcode.com/problems/design-browser-history)

[中文文档](/solution/1400-1499/1472.Design%20Browser%20History/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có một <strong>trình duyệt</strong> với một tab, bắt đầu tại <code>homepage</code>. Bạn có thể truy cập một <code>url</code> khác, quay lại trong lịch sử một số <code>steps</code> hoặc tiến tới trong lịch sử một số <code>steps</code>.</p>

<p>Hãy triển khai lớp <code>BrowserHistory</code>:</p>

<ul>
    <li><code>BrowserHistory(string homepage)</code> Khởi tạo đối tượng với <code>homepage</code>&nbsp;của trình duyệt.</li>
    <li><code>void visit(string url)</code>&nbsp;Truy cập&nbsp;<code>url</code> từ trang hiện tại. Xóa toàn bộ lịch sử phía trước.</li>
    <li><code>string back(int steps)</code>&nbsp;Quay lại <code>steps</code> bước trong lịch sử. Nếu trong lịch sử chỉ có thể quay lại <code>x</code> bước và <code>steps &gt; x</code>, bạn chỉ quay lại <code>x</code> bước. Trả về <code>url</code>&nbsp;hiện tại sau khi quay lại trong lịch sử <strong>nhiều nhất</strong> <code>steps</code> bước.</li>
    <li><code>string forward(int steps)</code>&nbsp;Tiến tới <code>steps</code> bước trong lịch sử. Nếu trong lịch sử chỉ có thể tiến tới <code>x</code> bước và <code>steps &gt; x</code>, bạn chỉ tiến tới&nbsp;<code>x</code> bước. Trả về <code>url</code>&nbsp;hiện tại sau khi tiến tới trong lịch sử <strong>nhiều nhất</strong> <code>steps</code> bước.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<pre>
<b>Đầu vào:</b>
[&quot;BrowserHistory&quot;,&quot;visit&quot;,&quot;visit&quot;,&quot;visit&quot;,&quot;back&quot;,&quot;back&quot;,&quot;forward&quot;,&quot;visit&quot;,&quot;forward&quot;,&quot;back&quot;,&quot;back&quot;]
[[&quot;leetcode.com&quot;],[&quot;google.com&quot;],[&quot;facebook.com&quot;],[&quot;youtube.com&quot;],[1],[1],[1],[&quot;linkedin.com&quot;],[2],[2],[7]]
<b>Đầu ra:</b>
[null,null,null,null,&quot;facebook.com&quot;,&quot;google.com&quot;,&quot;facebook.com&quot;,null,&quot;linkedin.com&quot;,&quot;google.com&quot;,&quot;leetcode.com&quot;]

<b>Giải thích:</b>
BrowserHistory browserHistory = new BrowserHistory(&quot;leetcode.com&quot;);
browserHistory.visit(&quot;google.com&quot;);       // You are in &quot;leetcode.com&quot;. Visit &quot;google.com&quot;
browserHistory.visit(&quot;facebook.com&quot;);     // You are in &quot;google.com&quot;. Visit &quot;facebook.com&quot;
browserHistory.visit(&quot;youtube.com&quot;);      // You are in &quot;facebook.com&quot;. Visit &quot;youtube.com&quot;
browserHistory.back(1);                   // You are in &quot;youtube.com&quot;, move back to &quot;facebook.com&quot; return &quot;facebook.com&quot;
browserHistory.back(1);                   // You are in &quot;facebook.com&quot;, move back to &quot;google.com&quot; return &quot;google.com&quot;
browserHistory.forward(1);                // You are in &quot;google.com&quot;, move forward to &quot;facebook.com&quot; return &quot;facebook.com&quot;
browserHistory.visit(&quot;linkedin.com&quot;);     // You are in &quot;facebook.com&quot;. Visit &quot;linkedin.com&quot;
browserHistory.forward(2);                // You are in &quot;linkedin.com&quot;, you cannot move forward any steps.
browserHistory.back(2);                   // You are in &quot;linkedin.com&quot;, move back two steps to &quot;facebook.com&quot; then to &quot;google.com&quot;. return &quot;google.com&quot;
browserHistory.back(7);                   // You are in &quot;google.com&quot;, you can move back only one step to &quot;leetcode.com&quot;. return &quot;leetcode.com&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= homepage.length &lt;= 20</code></li>
    <li><code>1 &lt;= url.length &lt;= 20</code></li>
    <li><code>1 &lt;= steps &lt;= 100</code></li>
    <li><code>homepage</code> và <code>url</code> chỉ gồm &#39;.&#39; hoặc các chữ cái tiếng Anh viết thường.</li>
    <li>Sẽ có nhiều nhất <code>5000</code>&nbsp;lần gọi đến <code>visit</code>, <code>back</code> và <code>forward</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai stack

<!-- thinking:start -->

> **Tư duy**
>
> `visit` loại bỏ lịch sử phía trước; `back`/`forward` di chuyển trên một dòng thời gian. $stk1$ lưu đường đi đến trang hiện tại, còn $stk2$ lưu các trang phía trước. `visit` đẩy vào $stk1$ và xóa $stk2$; `back` lấy phần tử ra và đẩy vào $stk2$, còn `forward` lấy phần tử ra theo chiều ngược lại.

<!-- thinking:end -->

Ta có thể sử dụng hai stack, $\textit{stk1}$ và $\textit{stk2}$, lần lượt để lưu các trang phía sau và phía trước. Ban đầu, $\textit{stk1}$ chứa $\textit{homepage}$, còn $\textit{stk2}$ rỗng.

Khi gọi $\text{visit}(url)$, ta thêm $\textit{url}$ vào $\textit{stk1}$ và xóa $\textit{stk2}$. Độ phức tạp thời gian là $O(1)$.

Khi gọi $\text{back}(steps)$, ta lấy phần tử trên cùng của $\textit{stk1}$ ra và đẩy vào $\textit{stk2}$. Lặp lại thao tác này $steps$ lần cho đến khi độ dài của $\textit{stk1}$ bằng $1$ hoặc $steps$ bằng $0$. Cuối cùng, trả về phần tử trên cùng của $\textit{stk1}$. Độ phức tạp thời gian là $O(\textit{steps})$.

Khi gọi $\text{forward}(steps)$, ta lấy phần tử trên cùng của $\textit{stk2}$ ra và đẩy vào $\textit{stk1}$. Lặp lại thao tác này $steps$ lần cho đến khi $\textit{stk2}$ rỗng hoặc $steps$ bằng $0$. Cuối cùng, trả về phần tử trên cùng của $\textit{stk1}$. Độ phức tạp thời gian là $O(\textit{steps})$.

Độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài lịch sử duyệt web.

<!-- tabs:start -->

#### Python3

```python
class BrowserHistory:
    def __init__(self, homepage: str):
        self.stk1 = []
        self.stk2 = []
        self.visit(homepage)

    def visit(self, url: str) -> None:
        self.stk1.append(url)
        self.stk2.clear()

    def back(self, steps: int) -> str:
        while steps and len(self.stk1) > 1:
            self.stk2.append(self.stk1.pop())
            steps -= 1
        return self.stk1[-1]

    def forward(self, steps: int) -> str:
        while steps and self.stk2:
            self.stk1.append(self.stk2.pop())
            steps -= 1
        return self.stk1[-1]


# Your BrowserHistory object will be instantiated and called as such:
# obj = BrowserHistory(homepage)
# obj.visit(url)
# param_2 = obj.back(steps)
# param_3 = obj.forward(steps)
```

#### Java

```java
class BrowserHistory {
    private Deque<String> stk1 = new ArrayDeque<>();
    private Deque<String> stk2 = new ArrayDeque<>();

    public BrowserHistory(String homepage) {
        visit(homepage);
    }

    public void visit(String url) {
        stk1.push(url);
        stk2.clear();
    }

    public String back(int steps) {
        for (; steps > 0 && stk1.size() > 1; --steps) {
            stk2.push(stk1.pop());
        }
        return stk1.peek();
    }

    public String forward(int steps) {
        for (; steps > 0 && !stk2.isEmpty(); --steps) {
            stk1.push(stk2.pop());
        }
        return stk1.peek();
    }
}

/**
 * Your BrowserHistory object will be instantiated and called as such:
 * BrowserHistory obj = new BrowserHistory(homepage);
 * obj.visit(url);
 * String param_2 = obj.back(steps);
 * String param_3 = obj.forward(steps);
 */
```

#### C++

```cpp
class BrowserHistory {
public:
    stack<string> stk1;
    stack<string> stk2;

    BrowserHistory(string homepage) {
        visit(homepage);
    }

    void visit(string url) {
        stk1.push(url);
        stk2 = stack<string>();
    }

    string back(int steps) {
        for (; steps && stk1.size() > 1; --steps) {
            stk2.push(stk1.top());
            stk1.pop();
        }
        return stk1.top();
    }

    string forward(int steps) {
        for (; steps && !stk2.empty(); --steps) {
            stk1.push(stk2.top());
            stk2.pop();
        }
        return stk1.top();
    }
};

/**
 * Your BrowserHistory object will be instantiated and called as such:
 * BrowserHistory* obj = new BrowserHistory(homepage);
 * obj->visit(url);
 * string param_2 = obj->back(steps);
 * string param_3 = obj->forward(steps);
 */
```

#### Go

```go
type BrowserHistory struct {
    stk1 []string
    stk2 []string
}

func Constructor(homepage string) BrowserHistory {
    t := BrowserHistory{[]string{}, []string{}}
    t.Visit(homepage)
    return t
}

func (this *BrowserHistory) Visit(url string) {
    this.stk1 = append(this.stk1, url)
    this.stk2 = []string{}
}

func (this *BrowserHistory) Back(steps int) string {
    for i := 0; i < steps && len(this.stk1) > 1; i++ {
        this.stk2 = append(this.stk2, this.stk1[len(this.stk1)-1])
        this.stk1 = this.stk1[:len(this.stk1)-1]
    }
    return this.stk1[len(this.stk1)-1]
}

func (this *BrowserHistory) Forward(steps int) string {
    for i := 0; i < steps && len(this.stk2) > 0; i++ {
        this.stk1 = append(this.stk1, this.stk2[len(this.stk2)-1])
        this.stk2 = this.stk2[:len(this.stk2)-1]
    }
    return this.stk1[len(this.stk1)-1]
}

/**
 * Your BrowserHistory object will be instantiated and called as such:
 * obj := Constructor(homepage);
 * obj.Visit(url);
 * param_2 := obj.Back(steps);
 * param_3 := obj.Forward(steps);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
