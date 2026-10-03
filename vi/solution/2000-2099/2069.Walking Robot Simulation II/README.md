---
comments: true
difficulty: Medium
rating: 1918
source: Biweekly Contest 65 Q2
tags:
    - Design
    - Simulation
---

<!-- problem:start -->

# [2069. Walking Robot Simulation II](https://leetcode.com/problems/walking-robot-simulation-ii)

[中文文档](/solution/2000-2099/2069.Walking%20Robot%20Simulation%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Trên mặt phẳng XY có một lưới kích thước <code>width x height</code>, với ô <strong>dưới cùng bên trái</strong> tại <code>(0, 0)</code> và ô <strong>trên cùng bên phải</strong> tại <code>(width - 1, height - 1)</code>. Lưới được căn theo bốn hướng chính (<code>&quot;North&quot;</code>, <code>&quot;East&quot;</code>, <code>&quot;South&quot;</code> và <code>&quot;West&quot;</code>). <strong>Ban đầu</strong>, robot ở ô <code>(0, 0)</code> và quay mặt về hướng <code>&quot;East&quot;</code>.</p>

<p>Robot có thể được hướng dẫn di chuyển một số <strong>bước</strong> cụ thể. Với mỗi bước, robot thực hiện các thao tác sau.</p>

<ol>
	<li>Cố gắng di chuyển <strong>về phía trước một</strong> ô theo hướng đang quay mặt.</li>
	<li>Nếu ô robot <strong>đang di chuyển đến</strong> nằm <strong>ngoài phạm vi</strong>, robot sẽ <strong>quay</strong> 90 độ <strong>ngược chiều kim đồng hồ</strong> rồi thử lại bước đó.</li>
</ol>

<p>Sau khi di chuyển đủ số bước được yêu cầu, robot dừng lại và chờ chỉ dẫn tiếp theo.</p>

<p>Hãy triển khai class <code>Robot</code>:</p>

<ul>
	<li><code>Robot(int width, int height)</code> Khởi tạo lưới kích thước <code>width x height</code>, với robot ở <code>(0, 0)</code> và quay mặt về hướng <code>&quot;East&quot;</code>.</li>
	<li><code>void step(int num)</code> Ra lệnh cho robot di chuyển về phía trước <code>num</code> bước.</li>
	<li><code>int[] getPos()</code> Trả về ô hiện tại của robot dưới dạng một mảng có độ dài 2, <code>[x, y]</code>.</li>
	<li><code>String getDir()</code> Trả về hướng hiện tại của robot, là <code>&quot;North&quot;</code>, <code>&quot;East&quot;</code>, <code>&quot;South&quot;</code> hoặc <code>&quot;West&quot;</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="example-1" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2069.Walking%20Robot%20Simulation%20II/images/example-1.png" style="width: 498px; height: 268px;" />
<pre>
<strong>Đầu vào</strong>
[&quot;Robot&quot;, &quot;step&quot;, &quot;step&quot;, &quot;getPos&quot;, &quot;getDir&quot;, &quot;step&quot;, &quot;step&quot;, &quot;step&quot;, &quot;getPos&quot;, &quot;getDir&quot;]
[[6, 3], [2], [2], [], [], [2], [1], [4], [], []]
<strong>Đầu ra</strong>
[null, null, null, [4, 0], &quot;East&quot;, null, null, null, [1, 2], &quot;West&quot;]

<strong>Giải thích</strong>
Robot robot = new Robot(6, 3); // Initialize the grid and the robot at (0, 0) facing East.
robot.step(2); // It moves two steps East to (2, 0), and faces East.
robot.step(2); // It moves two steps East to (4, 0), and faces East.
robot.getPos(); // return [4, 0]
robot.getDir(); // return &quot;East&quot;
robot.step(2); // It moves one step East to (5, 0), and faces East.
// Moving the next step East would be out of bounds, so it turns and faces North.
// Then, it moves one step North to (5, 1), and faces North.
robot.step(1); // It moves one step North to (5, 2), and faces <strong>North</strong> (not West).
robot.step(4); // Moving the next step North would be out of bounds, so it turns and faces West.
// Then, it moves four steps West to (1, 2), and faces West.
robot.getPos(); // return [1, 2]
robot.getDir(); // return &quot;West&quot;

</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= width, height &lt;= 100</code></li>
	<li><code>1 &lt;= num &lt;= 10<sup>5</sup></code></li>
	<li>Tối đa <strong>tổng cộng</strong> <code>10<sup>4</sup></code> lần gọi <code>step</code>, <code>getPos</code> và <code>getDir</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phân tích trường hợp

<!-- thinking:start -->

> **Tư duy**
>
> `num` và số lần gọi đều lớn, nên di chuyển từng ô một sẽ quá chậm. Robot chạy theo chu kỳ trên biên hình chữ nhật có độ dài $p=2(w+h-2)$. Vị trí lấy modulo $p$ sẽ nằm trên một trong bốn cạnh.
>
> Từ khoảng cách đó, ta ánh xạ sang tọa độ và hướng quay mặt. Trước khi di chuyển, robot quay mặt về East; sau khi di chuyển, tại gốc tọa độ, robot quay mặt về South (điểm kết thúc của cạnh cuối cùng).

<!-- thinking:end -->

Đặt $mx = width - 1$ và $my = height - 1$. Quỹ đạo của robot tạo thành biên của một hình chữ nhật, với $(0, 0)$ là góc dưới cùng bên trái và $(mx, my)$ là góc trên cùng bên phải. Ta có thể chia quỹ đạo của robot thành bốn đoạn:

1. Từ $(0, 0)$ di chuyển dọc theo chiều dương của trục $x$ đến $(mx, 0)$, khi đó robot quay mặt về hướng "East".
2. Từ $(mx, 0)$ di chuyển dọc theo chiều dương của trục $y$ đến $(mx, my)$, khi đó robot quay mặt về hướng "North".
3. Từ $(mx, my)$ di chuyển dọc theo chiều âm của trục $x$ đến $(0, my)$, khi đó robot quay mặt về hướng "West".
4. Từ $(0, my)$ di chuyển dọc theo chiều âm của trục $y$ đến $(0, 0)$, khi đó robot quay mặt về hướng "South".

Do đó, quỹ đạo của robot có thể được xem như một chu kỳ có độ dài $p = 2 \cdot mx + 2 \cdot my$. Với mỗi lần gọi `step(num)`, ta cộng `num` vào vị trí hiện tại của robot rồi lấy kết quả modulo $p$ để nhận được vị trí mới. Dựa vào vị trí mới, ta có thể xác định hướng và tọa độ của robot.

Lưu ý rằng nếu robot chưa từng di chuyển, hướng của robot phải là "East". Nếu robot đã di chuyển và vị trí của robot là $(0, 0)$, hướng của robot phải là "South".

Độ phức tạp thời gian là $O(1)$, và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Robot:

    def __init__(self, width: int, height: int):
        self.mx = width - 1
        self.my = height - 1
        self.p = 2 * self.mx + 2 * self.my
        self.cur = 0
        self.moved = False

    def step(self, num: int) -> None:
        self.moved = True
        self.cur = (self.cur + num) % self.p

    def getPos(self) -> List[int]:
        d = self.cur
        mx, my = self.mx, self.my
        if 0 <= d <= mx:
            return [d, 0]
        if mx < d <= mx + my:
            return [mx, d - mx]
        if mx + my < d <= 2 * mx + my:
            return [mx - (d - (mx + my)), my]
        return [0, my - (d - (2 * mx + my))]

    def getDir(self) -> str:
        d = self.cur
        mx, my = self.mx, self.my
        if not self.moved:
            return "East"
        if 1 <= d <= mx:
            return "East"
        elif mx < d <= mx + my:
            return "North"
        elif mx + my < d <= 2 * mx + my:
            return "West"
        return "South"


# Your Robot object will be instantiated and called as such:
# obj = Robot(width, height)
# obj.step(num)
# param_2 = obj.getPos()
# param_3 = obj.getDir()
```

#### Java

```java
class Robot {

    private int mx, my, p, cur;
    private boolean moved;

    public Robot(int width, int height) {
        this.mx = width - 1;
        this.my = height - 1;
        this.p = 2 * this.mx + 2 * this.my;
        this.cur = 0;
        this.moved = false;
    }

    public void step(int num) {
        this.moved = true;
        this.cur = (this.cur + num) % this.p;
    }

    public int[] getPos() {
        int d = this.cur;
        int mx = this.mx, my = this.my;

        if (0 <= d && d <= mx) {
            return new int[] {d, 0};
        }
        if (mx < d && d <= mx + my) {
            return new int[] {mx, d - mx};
        }
        if (mx + my < d && d <= 2 * mx + my) {
            return new int[] {mx - (d - (mx + my)), my};
        }
        return new int[] {0, my - (d - (2 * mx + my))};
    }

    public String getDir() {
        int d = this.cur;
        int mx = this.mx, my = this.my;

        if (!this.moved) {
            return "East";
        }
        if (1 <= d && d <= mx) {
            return "East";
        } else if (mx < d && d <= mx + my) {
            return "North";
        } else if (mx + my < d && d <= 2 * mx + my) {
            return "West";
        }
        return "South";
    }
}

/**
 * Your Robot object will be instantiated and called as such:
 * Robot obj = new Robot(width, height);
 * obj.step(num);
 * int[] param_2 = obj.getPos();
 * String param_3 = obj.getDir();
 */
```

#### C++

```cpp
class Robot {
public:
    int mx, my, p, cur;
    bool moved;

    Robot(int width, int height) {
        mx = width - 1;
        my = height - 1;
        p = 2 * mx + 2 * my;
        cur = 0;
        moved = false;
    }

    void step(int num) {
        moved = true;
        cur = (cur + num) % p;
    }

    vector<int> getPos() {
        int d = cur;
        int mx = this->mx, my = this->my;

        if (0 <= d && d <= mx) {
            return {d, 0};
        }
        if (mx < d && d <= mx + my) {
            return {mx, d - mx};
        }
        if (mx + my < d && d <= 2 * mx + my) {
            return {mx - (d - (mx + my)), my};
        }
        return {0, my - (d - (2 * mx + my))};
    }

    string getDir() {
        int d = cur;
        int mx = this->mx, my = this->my;

        if (!moved) {
            return "East";
        }
        if (1 <= d && d <= mx) {
            return "East";
        } else if (mx < d && d <= mx + my) {
            return "North";
        } else if (mx + my < d && d <= 2 * mx + my) {
            return "West";
        }
        return "South";
    }
};

/**
 * Your Robot object will be instantiated and called as such:
 * Robot* obj = new Robot(width, height);
 * obj->step(num);
 * vector<int> param_2 = obj->getPos();
 * string param_3 = obj->getDir();
 */
```

#### Go

```go
type Robot struct {
	mx, my, p, cur int
	moved          bool
}

func Constructor(width int, height int) Robot {
	mx := width - 1
	my := height - 1
	return Robot{
		mx:    mx,
		my:    my,
		p:     2*mx + 2*my,
		cur:   0,
		moved: false,
	}
}

func (this *Robot) Step(num int) {
	this.moved = true
	this.cur = (this.cur + num) % this.p
}

func (this *Robot) GetPos() []int {
	d := this.cur
	mx, my := this.mx, this.my

	if 0 <= d && d <= mx {
		return []int{d, 0}
	}
	if mx < d && d <= mx+my {
		return []int{mx, d - mx}
	}
	if mx+my < d && d <= 2*mx+my {
		return []int{mx - (d - (mx + my)), my}
	}
	return []int{0, my - (d - (2*mx + my))}
}

func (this *Robot) GetDir() string {
	d := this.cur
	mx, my := this.mx, this.my

	if !this.moved {
		return "East"
	}
	if 1 <= d && d <= mx {
		return "East"
	} else if mx < d && d <= mx+my {
		return "North"
	} else if mx+my < d && d <= 2*mx+my {
		return "West"
	}
	return "South"
}

/**
 * Your Robot object will be instantiated and called as such:
 * obj := Constructor(width, height);
 * obj.Step(num);
 * param_2 := obj.GetPos();
 * param_3 := obj.GetDir();
 */
```

#### TypeScript

```ts
class Robot {
    private mx: number;
    private my: number;
    private p: number;
    private cur: number;
    private moved: boolean;

    constructor(width: number, height: number) {
        this.mx = width - 1;
        this.my = height - 1;
        this.p = 2 * this.mx + 2 * this.my;
        this.cur = 0;
        this.moved = false;
    }

    step(num: number): void {
        this.moved = true;
        this.cur = (this.cur + num) % this.p;
    }

    getPos(): number[] {
        const d = this.cur;
        const mx = this.mx,
            my = this.my;

        if (0 <= d && d <= mx) {
            return [d, 0];
        }
        if (mx < d && d <= mx + my) {
            return [mx, d - mx];
        }
        if (mx + my < d && d <= 2 * mx + my) {
            return [mx - (d - (mx + my)), my];
        }
        return [0, my - (d - (2 * mx + my))];
    }

    getDir(): string {
        const d = this.cur;
        const mx = this.mx,
            my = this.my;

        if (!this.moved) {
            return 'East';
        }
        if (1 <= d && d <= mx) {
            return 'East';
        } else if (mx < d && d <= mx + my) {
            return 'North';
        } else if (mx + my < d && d <= 2 * mx + my) {
            return 'West';
        }
        return 'South';
    }
}

/**
 * Your Robot object will be instantiated and called as such:
 * var obj = new Robot(width, height)
 * obj.step(num)
 * var param_2 = obj.getPos()
 * var param_3 = obj.getDir()
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
