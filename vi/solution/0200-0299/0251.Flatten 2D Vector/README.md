---
comments: true
difficulty: Medium
tags:
    - Design
    - Array
    - Two Pointers
    - Iterator
---

<!-- problem:start -->

# [251. Flatten 2D Vector 🔒](https://leetcode.com/problems/flatten-2d-vector)

[中文文档](/solution/0200-0299/0251.Flatten%202D%20Vector/README.md)

## Mô tả

<!-- description:start -->

<p>Thiết kế một iterator để làm phẳng vector 2D. Iterator cần hỗ trợ các thao tác <code>next</code> và <code>hasNext</code>.</p>

<p>Hãy triển khai class <code>Vector2D</code>:</p>

<ul>
	<li><code>Vector2D(int[][] vec)</code> khởi tạo object bằng vector 2D <code>vec</code>.</li>
	<li><code>next()</code> trả về phần tử tiếp theo trong vector 2D rồi đưa con trỏ tiến lên một bước. Bạn có thể giả định mọi lời gọi <code>next</code> đều hợp lệ.</li>
	<li><code>hasNext()</code> trả về <code>true</code> nếu vector vẫn còn phần tử, ngược lại trả về <code>false</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;Vector2D&quot;, &quot;next&quot;, &quot;next&quot;, &quot;next&quot;, &quot;hasNext&quot;, &quot;hasNext&quot;, &quot;next&quot;, &quot;hasNext&quot;]
[[[[1, 2], [3], [4]]], [], [], [], [], [], [], []]
<strong>Đầu ra</strong>
[null, 1, 2, 3, true, true, 4, false]

<strong>Giải thích</strong>
Vector2D vector2D = new Vector2D([[1, 2], [3], [4]]);
vector2D.next();    // return 1
vector2D.next();    // return 2
vector2D.next();    // return 3
vector2D.hasNext(); // return True
vector2D.hasNext(); // return True
vector2D.next();    // return 4
vector2D.hasNext(); // return False
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= vec.length &lt;= 200</code></li>
	<li><code>0 &lt;= vec[i].length &lt;= 500</code></li>
	<li><code>-500 &lt;= vec[i][j] &lt;= 500</code></li>
	<li>Sẽ có tối đa <code>10<sup>5</sup></code> lời gọi đến <code>next</code> và <code>hasNext</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Để tăng độ thử thách, hãy thử cài đặt chỉ bằng <a href="http://www.cplusplus.com/reference/iterator/iterator/" target="_blank">iterator trong C++</a> hoặc <a href="http://docs.oracle.com/javase/7/docs/api/java/util/Iterator.html" target="_blank">iterator trong Java</a>.</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Làm phẳng thành mảng 1D thì đơn giản, nhưng vừa tốn bộ nhớ vừa không phải iterator. Ta dùng chỉ số hàng và chỉ số cột để xác định giá trị tiếp theo.
>
> Hàm $forward$ bỏ qua các hàng rỗng để $(i,j)$ trỏ đến một phần tử có thật; $next$ đọc phần tử đó rồi tiến lên, còn $hasNext$ kiểm tra xem $i$ còn nằm trong phạm vi hay không.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Vector2D:
    def __init__(self, vec: List[List[int]]):
        self.i = 0
        self.j = 0
        self.vec = vec

    def next(self) -> int:
        self.forward()
        ans = self.vec[self.i][self.j]
        self.j += 1
        return ans

    def hasNext(self) -> bool:
        self.forward()
        return self.i < len(self.vec)

    def forward(self):
        while self.i < len(self.vec) and self.j >= len(self.vec[self.i]):
            self.i += 1
            self.j = 0


# Your Vector2D object will be instantiated and called as such:
# obj = Vector2D(vec)
# param_1 = obj.next()
# param_2 = obj.hasNext()
```

#### Java

```java
class Vector2D {
    private int i;
    private int j;
    private int[][] vec;

    public Vector2D(int[][] vec) {
        this.vec = vec;
    }

    public int next() {
        forward();
        return vec[i][j++];
    }

    public boolean hasNext() {
        forward();
        return i < vec.length;
    }

    private void forward() {
        while (i < vec.length && j >= vec[i].length) {
            ++i;
            j = 0;
        }
    }
}

/**
 * Your Vector2D object will be instantiated and called as such:
 * Vector2D obj = new Vector2D(vec);
 * int param_1 = obj.next();
 * boolean param_2 = obj.hasNext();
 */
```

#### C++

```cpp
class Vector2D {
public:
    Vector2D(vector<vector<int>>& vec) {
        this->vec = move(vec);
    }

    int next() {
        forward();
        return vec[i][j++];
    }

    bool hasNext() {
        forward();
        return i < vec.size();
    }

private:
    int i = 0;
    int j = 0;
    vector<vector<int>> vec;

    void forward() {
        while (i < vec.size() && j >= vec[i].size()) {
            ++i;
            j = 0;
        }
    }
};

/**
 * Your Vector2D object will be instantiated and called as such:
 * Vector2D* obj = new Vector2D(vec);
 * int param_1 = obj->next();
 * bool param_2 = obj->hasNext();
 */
```

#### Go

```go
type Vector2D struct {
	i, j int
	vec  [][]int
}

func Constructor(vec [][]int) Vector2D {
	return Vector2D{vec: vec}
}

func (this *Vector2D) Next() int {
	this.forward()
	ans := this.vec[this.i][this.j]
	this.j++
	return ans
}

func (this *Vector2D) HasNext() bool {
	this.forward()
	return this.i < len(this.vec)
}

func (this *Vector2D) forward() {
	for this.i < len(this.vec) && this.j >= len(this.vec[this.i]) {
		this.i++
		this.j = 0
	}
}

/**
 * Your Vector2D object will be instantiated and called as such:
 * obj := Constructor(vec);
 * param_1 := obj.Next();
 * param_2 := obj.HasNext();
 */
```

#### TypeScript

```ts
class Vector2D {
    i: number;
    j: number;
    vec: number[][];

    constructor(vec: number[][]) {
        this.i = 0;
        this.j = 0;
        this.vec = vec;
    }

    next(): number {
        this.forward();
        return this.vec[this.i][this.j++];
    }

    hasNext(): boolean {
        this.forward();
        return this.i < this.vec.length;
    }

    forward(): void {
        while (this.i < this.vec.length && this.j >= this.vec[this.i].length) {
            ++this.i;
            this.j = 0;
        }
    }
}

/**
 * Your Vector2D object will be instantiated and called as such:
 * var obj = new Vector2D(vec)
 * var param_1 = obj.next()
 * var param_2 = obj.hasNext()
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
