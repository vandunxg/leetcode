---
comments: true
difficulty: Medium
rating: 1604
source: Biweekly Contest 88 Q2
tags:
    - Union Find
    - Design
    - Binary Indexed Tree
    - Segment Tree
    - Hash Table
    - Binary Search
    - Ordered Set
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2424. Longest Uploaded Prefix](https://leetcode.com/problems/longest-uploaded-prefix)

[中文文档](/solution/2400-2499/2424.Longest%20Uploaded%20Prefix/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một stream gồm <code>n</code> video, mỗi video được biểu diễn bằng một số <strong>khác nhau</strong> từ <code>1</code> đến <code>n</code> mà bạn cần &quot;upload&quot; lên server. Bạn cần triển khai một data structure để tính độ dài của <strong>prefix đã upload dài nhất</strong> tại các thời điểm khác nhau trong quá trình upload.</p>

<p>Ta xem <code>i</code> là một prefix đã upload nếu tất cả video trong khoảng từ <code>1</code> đến <code>i</code> (<strong>bao gồm cả hai đầu</strong>) đều đã được upload lên server. Prefix đã upload dài nhất là giá trị <strong>lớn nhất</strong> của <code>i</code> thỏa mãn định nghĩa này.<br />
<br />
Hãy triển khai class <code>LUPrefix </code>:</p>

<ul>
	<li><code>LUPrefix(int n)</code> Khởi tạo đối tượng cho một stream gồm <code>n</code> video.</li>
	<li><code>void upload(int video)</code> Upload <code>video</code> lên server.</li>
	<li><code>int longest()</code> Trả về độ dài của <strong>prefix đã upload dài nhất</strong> được định nghĩa ở trên.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;LUPrefix&quot;, &quot;upload&quot;, &quot;longest&quot;, &quot;upload&quot;, &quot;longest&quot;, &quot;upload&quot;, &quot;longest&quot;]
[[4], [3], [], [1], [], [2], []]
<strong>Đầu ra</strong>
[null, null, 0, null, 1, null, 3]

<strong>Giải thích</strong>
LUPrefix server = new LUPrefix(4);   // Initialize a stream of 4 videos.
server.upload(3);                    // Upload video 3.
server.longest();                    // Since video 1 has not been uploaded yet, there is no prefix.
                                     // So, we return 0.
server.upload(1);                    // Upload video 1.
server.longest();                    // The prefix [1] is the longest uploaded prefix, so we return 1.
server.upload(2);                    // Upload video 2.
server.longest();                    // The prefix [1,2,3] is the longest uploaded prefix, so we return 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= video &lt;= n</code></li>
	<li>Tất cả giá trị của <code>video</code> đều <strong>khác nhau</strong>.</li>
	<li>Có <strong>tổng cộng</strong> nhiều nhất <code>2 * 10<sup>5</sup></code> lời gọi đến <code>upload</code> và <code>longest</code>.</li>
	<li>Sẽ có ít nhất một lời gọi đến <code>longest</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Có tối đa $2\times 10^5$ lần upload và truy vấn, nên không thể quét $[1,n]$ trong mỗi lần. Prefix dài nhất là giá trị $r$ lớn nhất sao cho tất cả các video từ $1..r$ đều đã được upload; chỉ việc upload $r+1$ mới có thể mở rộng prefix này.
>
> Lưu các id đã upload trong một set và duy trì $r$. Sau mỗi lần upload, tăng $r$ khi $r+1$ đang có trong set. Mỗi id chỉ làm $r$ tăng nhiều nhất một lần, nên tổng phần việc phát sinh là tuyến tính.

<!-- thinking:end -->

Ta dùng một biến $r$ để ghi nhận prefix các video đã upload dài nhất hiện tại, cùng một mảng hoặc hash table $s$ để lưu các video đã được upload.

Mỗi khi một video được upload, ta đặt `s[video]` thành `true`, sau đó lặp để kiểm tra `s[r + 1]` có phải `true` hay không. Nếu đúng, ta cập nhật $r$.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là tổng số video.

<!-- tabs:start -->

#### Python3

```python
class LUPrefix:
    def __init__(self, n: int):
        self.r = 0
        self.s = set()

    def upload(self, video: int) -> None:
        self.s.add(video)
        while self.r + 1 in self.s:
            self.r += 1

    def longest(self) -> int:
        return self.r


# Your LUPrefix object will be instantiated and called as such:
# obj = LUPrefix(n)
# obj.upload(video)
# param_2 = obj.longest()
```

#### Java

```java
class LUPrefix {
    private int r;
    private Set<Integer> s = new HashSet<>();

    public LUPrefix(int n) {
    }

    public void upload(int video) {
        s.add(video);
        while (s.contains(r + 1)) {
            ++r;
        }
    }

    public int longest() {
        return r;
    }
}

/**
 * Your LUPrefix object will be instantiated and called as such:
 * LUPrefix obj = new LUPrefix(n);
 * obj.upload(video);
 * int param_2 = obj.longest();
 */
```

#### C++

```cpp
class LUPrefix {
public:
    LUPrefix(int n) {
    }

    void upload(int video) {
        s.insert(video);
        while (s.count(r + 1)) {
            ++r;
        }
    }

    int longest() {
        return r;
    }

private:
    int r = 0;
    unordered_set<int> s;
};

/**
 * Your LUPrefix object will be instantiated and called as such:
 * LUPrefix* obj = new LUPrefix(n);
 * obj->upload(video);
 * int param_2 = obj->longest();
 */
```

#### Go

```go
type LUPrefix struct {
	r int
	s []bool
}

func Constructor(n int) LUPrefix {
	return LUPrefix{0, make([]bool, n+1)}
}

func (this *LUPrefix) Upload(video int) {
	this.s[video] = true
	for this.r+1 < len(this.s) && this.s[this.r+1] {
		this.r++
	}
}

func (this *LUPrefix) Longest() int {
	return this.r
}

/**
 * Your LUPrefix object will be instantiated and called as such:
 * obj := Constructor(n);
 * obj.Upload(video);
 * param_2 := obj.Longest();
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
