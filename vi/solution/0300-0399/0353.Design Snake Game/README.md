---
comments: true
difficulty: Medium
tags:
    - Design
    - Queue
    - Array
    - Hash Table
    - Simulation
---

<!-- problem:start -->

# [353. Design Snake Game 🔒](https://leetcode.com/problems/design-snake-game)

[中文文档](/solution/0300-0399/0353.Design%20Snake%20Game/README.md)

## Mô tả

<!-- description:start -->

<p>Hãy thiết kế trò chơi <a href="https://en.wikipedia.org/wiki/Snake_(video_game)" target="_blank">Snake</a> chạy trên thiết bị có màn hình kích thước <code>height x width</code>. Nếu chưa quen trò chơi này, bạn có thể <a href="http://patorjk.com/games/snake/" target="_blank">chơi thử trực tuyến</a>.</p>

<p>Ban đầu, rắn nằm ở góc trên bên trái <code>(0, 0)</code> và có độ dài <code>1</code> đơn vị.</p>

<p>Bạn được cho mảng <code>food</code>, trong đó <code>food[i] = (r<sub>i</sub>, c<sub>i</sub>)</code> là vị trí hàng và cột của một miếng thức ăn mà rắn có thể ăn. Khi rắn ăn thức ăn, độ dài của rắn và điểm số đều tăng thêm <code>1</code>.</p>

<p>Các miếng thức ăn xuất hiện lần lượt trên màn hình; miếng thứ hai chỉ xuất hiện sau khi rắn ăn miếng thứ nhất.</p>

<p>Khi thức ăn xuất hiện, đảm bảo vị trí đó không bị rắn chiếm.</p>

<p>Trò chơi kết thúc nếu rắn đi ra ngoài giới hạn (đâm vào tường), hoặc sau khi di chuyển, đầu rắn đi vào ô mà thân rắn đang chiếm <strong>sau</strong> nước đi đó (ví dụ, rắn dài 4 không thể tự đâm vào thân mình).</p>

<p>Hãy triển khai class <code>SnakeGame</code>:</p>

<ul>
	<li><code>SnakeGame(int width, int height, int[][] food)</code> Khởi tạo object với màn hình kích thước <code>height x width</code> và các vị trí trong <code>food</code>.</li>
	<li><code>int move(String direction)</code> Trả về điểm số sau khi rắn di chuyển một bước theo hướng <code>direction</code>. Nếu trò chơi kết thúc, trả về <code>-1</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0353.Design%20Snake%20Game/images/snake.jpg" style="width: 800px; height: 302px;" />
<pre>
<strong>Đầu vào</strong>
[&quot;SnakeGame&quot;, &quot;move&quot;, &quot;move&quot;, &quot;move&quot;, &quot;move&quot;, &quot;move&quot;, &quot;move&quot;]
[[3, 2, [[1, 2], [0, 1]]], [&quot;R&quot;], [&quot;D&quot;], [&quot;R&quot;], [&quot;U&quot;], [&quot;L&quot;], [&quot;U&quot;]]
<strong>Đầu ra</strong>
[null, 0, 0, 1, 1, 2, -1]

<strong>Giải thích</strong>
SnakeGame snakeGame = new SnakeGame(3, 2, [[1, 2], [0, 1]]);
snakeGame.move(&quot;R&quot;); // return 0
snakeGame.move(&quot;D&quot;); // return 0
snakeGame.move(&quot;R&quot;); // return 1, snake eats the first piece of food. The second piece of food appears at (0, 1).
snakeGame.move(&quot;U&quot;); // return 1
snakeGame.move(&quot;L&quot;); // return 2, snake eats the second food. No more food appears.
snakeGame.move(&quot;U&quot;); // return -1, game over because snake collides with border
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= width, height &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= food.length &lt;= 50</code></li>
	<li><code>food[i].length == 2</code></li>
	<li><code>0 &lt;= r<sub>i</sub> &lt; height</code></li>
	<li><code>0 &lt;= c<sub>i</sub> &lt; width</code></li>
	<li><code>direction.length == 1</code></li>
	<li><code>direction</code> là <code>&#39;U&#39;</code>, <code>&#39;D&#39;</code>, <code>&#39;L&#39;</code> hoặc <code>&#39;R&#39;</code>.</li>
	<li>Sẽ có tối đa <code>10<sup>4</sup></code> lần gọi <code>move</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mô phỏng chuyển động của rắn: đi ra ngoài giới hạn hoặc cắn vào thân sẽ kết thúc trò chơi; ăn thức ăn làm thân dài ra. Thân rắn được lưu bằng queue vì thường xuyên cập nhật đầu và đuôi.
>
> Dùng deque lưu thân rắn (đầu ở phía trước) và set để kiểm tra va chạm. Tính vị trí đầu mới: trả về $-1$ nếu ra ngoài giới hạn; nếu gặp thức ăn thì tăng điểm và giữ nguyên đuôi, nếu không thì pop đuôi. Sau đó, nếu đầu mới va vào thân thì kết thúc; ngược lại, thêm đầu mới vào deque. Xóa đuôi trước giúp ô đuôi cũ được xem là trống.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class SnakeGame:
    def __init__(self, width: int, height: int, food: List[List[int]]):
        self.m = height
        self.n = width
        self.food = food
        self.score = 0
        self.idx = 0
        self.q = deque([(0, 0)])
        self.vis = {(0, 0)}

    def move(self, direction: str) -> int:
        i, j = self.q[0]
        x, y = i, j
        match direction:
            case "U":
                x -= 1
            case "D":
                x += 1
            case "L":
                y -= 1
            case "R":
                y += 1
        if x < 0 or x >= self.m or y < 0 or y >= self.n:
            return -1
        if (
            self.idx < len(self.food)
            and x == self.food[self.idx][0]
            and y == self.food[self.idx][1]
        ):
            self.score += 1
            self.idx += 1
        else:
            self.vis.remove(self.q.pop())
        if (x, y) in self.vis:
            return -1
        self.q.appendleft((x, y))
        self.vis.add((x, y))
        return self.score


# Your SnakeGame object will be instantiated and called as such:
# obj = SnakeGame(width, height, food)
# param_1 = obj.move(direction)
```

#### Java

```java
class SnakeGame {
    private int m;
    private int n;
    private int[][] food;
    private int score;
    private int idx;
    private Deque<Integer> q = new ArrayDeque<>();
    private Set<Integer> vis = new HashSet<>();

    public SnakeGame(int width, int height, int[][] food) {
        m = height;
        n = width;
        this.food = food;
        q.offer(0);
        vis.add(0);
    }

    public int move(String direction) {
        int p = q.peekFirst();
        int i = p / n, j = p % n;
        int x = i, y = j;
        if ("U".equals(direction)) {
            --x;
        } else if ("D".equals(direction)) {
            ++x;
        } else if ("L".equals(direction)) {
            --y;
        } else {
            ++y;
        }
        if (x < 0 || x >= m || y < 0 || y >= n) {
            return -1;
        }
        if (idx < food.length && x == food[idx][0] && y == food[idx][1]) {
            ++score;
            ++idx;
        } else {
            int t = q.pollLast();
            vis.remove(t);
        }
        int cur = f(x, y);
        if (vis.contains(cur)) {
            return -1;
        }
        q.offerFirst(cur);
        vis.add(cur);
        return score;
    }

    private int f(int i, int j) {
        return i * n + j;
    }
}

/**
 * Your SnakeGame object will be instantiated and called as such:
 * SnakeGame obj = new SnakeGame(width, height, food);
 * int param_1 = obj.move(direction);
 */
```

#### C++

```cpp
class SnakeGame {
public:
    SnakeGame(int width, int height, vector<vector<int>>& food) {
        m = height;
        n = width;
        this->food = food;
        score = 0;
        idx = 0;
        q.push_back(0);
        vis.insert(0);
    }

    int move(string direction) {
        int p = q.front();
        int i = p / n, j = p % n;
        int x = i, y = j;
        if (direction == "U") {
            --x;
        } else if (direction == "D") {
            ++x;
        } else if (direction == "L") {
            --y;
        } else {
            ++y;
        }
        if (x < 0 || x >= m || y < 0 || y >= n) {
            return -1;
        }
        if (idx < food.size() && x == food[idx][0] && y == food[idx][1]) {
            ++score;
            ++idx;
        } else {
            int tail = q.back();
            q.pop_back();
            vis.erase(tail);
        }
        int cur = f(x, y);
        if (vis.count(cur)) {
            return -1;
        }
        q.push_front(cur);
        vis.insert(cur);
        return score;
    }

private:
    int m;
    int n;
    vector<vector<int>> food;
    int score;
    int idx;
    deque<int> q;
    unordered_set<int> vis;

    int f(int i, int j) {
        return i * n + j;
    }
};

/**
 * Your SnakeGame object will be instantiated and called as such:
 * SnakeGame* obj = new SnakeGame(width, height, food);
 * int param_1 = obj->move(direction);
 */
```

#### Go

```go
type SnakeGame struct {
	m     int
	n     int
	food  [][]int
	score int
	idx   int
	q     []int
	vis   map[int]bool
}

func Constructor(width int, height int, food [][]int) SnakeGame {
	return SnakeGame{height, width, food, 0, 0, []int{0}, map[int]bool{}}
}

func (this *SnakeGame) Move(direction string) int {
	f := func(i, j int) int {
		return i*this.n + j
	}
	p := this.q[0]
	i, j := p/this.n, p%this.n
	x, y := i, j
	if direction == "U" {
		x--
	} else if direction == "D" {
		x++
	} else if direction == "L" {
		y--
	} else {
		y++
	}
	if x < 0 || x >= this.m || y < 0 || y >= this.n {
		return -1
	}
	if this.idx < len(this.food) && x == this.food[this.idx][0] && y == this.food[this.idx][1] {
		this.score++
		this.idx++
	} else {
		t := this.q[len(this.q)-1]
		this.q = this.q[:len(this.q)-1]
		this.vis[t] = false
	}
	cur := f(x, y)
	if this.vis[cur] {
		return -1
	}
	this.q = append([]int{cur}, this.q...)
	this.vis[cur] = true
	return this.score
}

/**
 * Your SnakeGame object will be instantiated and called as such:
 * obj := Constructor(width, height, food);
 * param_1 := obj.Move(direction);
 */
```

#### TypeScript

```ts
class SnakeGame {
    private m: number;
    private n: number;
    private food: number[][];
    private score: number;
    private idx: number;
    private q: number[];
    private vis: Set<number>;

    constructor(width: number, height: number, food: number[][]) {
        this.m = height;
        this.n = width;
        this.food = food;
        this.score = 0;
        this.idx = 0;
        this.q = [0];
        this.vis = new Set([0]);
    }

    move(direction: string): number {
        const p = this.q[0];
        const i = Math.floor(p / this.n);
        const j = p % this.n;
        let x = i;
        let y = j;
        if (direction === 'U') {
            --x;
        } else if (direction === 'D') {
            ++x;
        } else if (direction === 'L') {
            --y;
        } else {
            ++y;
        }
        if (x < 0 || x >= this.m || y < 0 || y >= this.n) {
            return -1;
        }
        if (
            this.idx < this.food.length &&
            x === this.food[this.idx][0] &&
            y === this.food[this.idx][1]
        ) {
            ++this.score;
            ++this.idx;
        } else {
            const t = this.q.pop()!;
            this.vis.delete(t);
        }
        const cur = x * this.n + y;
        if (this.vis.has(cur)) {
            return -1;
        }
        this.q.unshift(cur);
        this.vis.add(cur);
        return this.score;
    }
}

/**
 * Your SnakeGame object will be instantiated and called as such:
 * var obj = new SnakeGame(width, height, food)
 * var param_1 = obj.move(direction)
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
