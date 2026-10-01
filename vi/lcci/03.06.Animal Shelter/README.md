---
comments: true
difficulty: Easy
---

<!-- problem:start -->

# [03.06. Animal Shelter](https://leetcode.cn/problems/animal-shelter-lcci)

[中文文档](/lcci/03.06.Animal%20Shelter/README.md)

## Mô tả

<!-- description:start -->

<p>Trại động vật chỉ nhận chó và mèo, hoạt động nghiêm ngặt theo nguyên tắc &quot;first in, first out&quot;. Người nhận nuôi phải chọn con vật &quot;cũ nhất&quot; (dựa trên thời điểm đến) trong toàn bộ trại, hoặc chọn chó hay mèo (và sẽ nhận con vật cũ nhất thuộc loại đó). Họ không thể chọn một con vật cụ thể. Hãy tạo các cấu trúc dữ liệu để duy trì hệ thống này và cài đặt các thao tác như <code>enqueue</code>, <code>dequeueAny</code>, <code>dequeueDog</code> và <code>dequeueCat</code>. Bạn có thể sử dụng cấu trúc dữ liệu Linked list tích hợp sẵn.</p>

<p>Phương thức <code>enqueue</code> có tham số <code>animal</code>, trong đó <code>animal[0]</code> là số của con vật, còn <code>animal[1]</code> là loại con vật: 0 là mèo và 1 là chó.</p>

<p>Phương thức <code>dequeue*</code> trả về <code>[animal number, animal type]</code>; nếu không có con vật nào có thể nhận nuôi, trả về <code>[-1, -1]</code>.</p>

<p><strong>Ví dụ 1:</strong></p>

<pre>

<strong> Đầu vào</strong>: 

[&quot;AnimalShelf&quot;, &quot;enqueue&quot;, &quot;enqueue&quot;, &quot;dequeueCat&quot;, &quot;dequeueDog&quot;, &quot;dequeueAny&quot;]

[[], [[0, 0]], [[1, 0]], [], [], []]

<strong> Đầu ra</strong>: 

[null,null,null,[0,0],[-1,-1],[1,0]]

</pre>

<p><strong>Ví dụ 2:</strong></p>

<pre>

<strong> Đầu vào</strong>: 

[&quot;AnimalShelf&quot;, &quot;enqueue&quot;, &quot;enqueue&quot;, &quot;enqueue&quot;, &quot;dequeueDog&quot;, &quot;dequeueCat&quot;, &quot;dequeueAny&quot;]

[[], [[0, 0]], [[1, 0]], [[2, 1]], [], [], []]

<strong> Đầu ra</strong>: 

[null,null,null,null,[2,1],[0,0],[1,0]]

</pre>

<p><strong>Lưu ý:</strong></p>

<ol>
	<li>Số lượng con vật trong trại không vượt quá 20000.</li>
</ol>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mảng các queue

<!-- thinking:start -->

> **Tư duy**
>
> Việc nhận nuôi tuân theo thứ tự cũ nhất trước, với bộ lọc cho mèo, chó hoặc cả hai. Một queue không thể bỏ qua loài còn lại trong $O(1)$.
>
> Thứ tự của mèo và chó độc lập, nên chỉ cần hai queue; id tăng dần khiến con vật cũ hơn nằm ở front có giá trị nhỏ hơn.
>
> $q[0]$ và $q[1]$ lưu id của mèo và chó. `dequeueAny` so sánh hai front và dùng queue còn lại nếu một queue rỗng.

<!-- thinking:end -->

Ta định nghĩa một mảng $q$ có độ dài $2$ để lưu queue của mèo và chó.

Trong thao tác `enqueue`, giả sử số của con vật là $i$ và loại con vật là $j$, ta enqueue $i$ vào $q[j]$.

Trong thao tác `dequeueAny`, ta kiểm tra xem $q[0]$ có rỗng hay không, hoặc $q[1]$ không rỗng và $q[1][0] < q[0][0]$. Nếu đúng, ta gọi `dequeueDog`, nếu không thì gọi `dequeueCat`.

Trong thao tác `dequeueDog`, nếu $q[1]$ rỗng, ta trả về $[-1, -1]$; nếu không, ta trả về $[q[1].pop(), 1]$.

Trong thao tác `dequeueCat`, nếu $q[0]$ rỗng, ta trả về $[-1, -1]$; nếu không, ta trả về $[q[0].pop(), 0]$.

Độ phức tạp thời gian của các thao tác trên là $O(1)$, còn độ phức tạp không gian là $O(n)$, trong đó $n$ là số con vật trong trại.

<!-- tabs:start -->

#### Python3

```python
class AnimalShelf:

    def __init__(self):
        self.q = [deque(), deque()]

    def enqueue(self, animal: List[int]) -> None:
        i, j = animal
        self.q[j].append(i)

    def dequeueAny(self) -> List[int]:
        if not self.q[0] or (self.q[1] and self.q[1][0] < self.q[0][0]):
            return self.dequeueDog()
        return self.dequeueCat()

    def dequeueDog(self) -> List[int]:
        return [-1, -1] if not self.q[1] else [self.q[1].popleft(), 1]

    def dequeueCat(self) -> List[int]:
        return [-1, -1] if not self.q[0] else [self.q[0].popleft(), 0]


# Your AnimalShelf object will be instantiated and called as such:
# obj = AnimalShelf()
# obj.enqueue(animal)
# param_2 = obj.dequeueAny()
# param_3 = obj.dequeueDog()
# param_4 = obj.dequeueCat()
```

#### Java

```java
class AnimalShelf {
    private Deque<Integer>[] q = new Deque[2];

    public AnimalShelf() {
        Arrays.setAll(q, k -> new ArrayDeque<>());
    }

    public void enqueue(int[] animal) {
        q[animal[1]].offer(animal[0]);
    }

    public int[] dequeueAny() {
        if (q[0].isEmpty() || (!q[1].isEmpty() && q[1].peek() < q[0].peek())) {
            return dequeueDog();
        }
        return dequeueCat();
    }

    public int[] dequeueDog() {
        return q[1].isEmpty() ? new int[] {-1, -1} : new int[] {q[1].poll(), 1};
    }

    public int[] dequeueCat() {
        return q[0].isEmpty() ? new int[] {-1, -1} : new int[] {q[0].poll(), 0};
    }
}

/**
 * Your AnimalShelf object will be instantiated and called as such:
 * AnimalShelf obj = new AnimalShelf();
 * obj.enqueue(animal);
 * int[] param_2 = obj.dequeueAny();
 * int[] param_3 = obj.dequeueDog();
 * int[] param_4 = obj.dequeueCat();
 */
```

#### C++

```cpp
class AnimalShelf {
public:
    AnimalShelf() {
    }

    void enqueue(vector<int> animal) {
        q[animal[1]].push(animal[0]);
    }

    vector<int> dequeueAny() {
        if (q[0].empty() || (!q[1].empty() && q[1].front() < q[0].front())) {
            return dequeueDog();
        }
        return dequeueCat();
    }

    vector<int> dequeueDog() {
        if (q[1].empty()) {
            return {-1, -1};
        }
        int dog = q[1].front();
        q[1].pop();
        return {dog, 1};
    }

    vector<int> dequeueCat() {
        if (q[0].empty()) {
            return {-1, -1};
        }
        int cat = q[0].front();
        q[0].pop();
        return {cat, 0};
    }

private:
    queue<int> q[2];
};

/**
 * Your AnimalShelf object will be instantiated and called as such:
 * AnimalShelf* obj = new AnimalShelf();
 * obj->enqueue(animal);
 * vector<int> param_2 = obj->dequeueAny();
 * vector<int> param_3 = obj->dequeueDog();
 * vector<int> param_4 = obj->dequeueCat();
 */
```

#### Go

```go
type AnimalShelf struct {
	q [2][]int
}

func Constructor() AnimalShelf {
	return AnimalShelf{}
}

func (this *AnimalShelf) Enqueue(animal []int) {
	this.q[animal[1]] = append(this.q[animal[1]], animal[0])
}

func (this *AnimalShelf) DequeueAny() []int {
	if len(this.q[0]) == 0 || (len(this.q[1]) > 0 && this.q[0][0] > this.q[1][0]) {
		return this.DequeueDog()
	}
	return this.DequeueCat()
}

func (this *AnimalShelf) DequeueDog() []int {
	if len(this.q[1]) == 0 {
		return []int{-1, -1}
	}
	dog := this.q[1][0]
	this.q[1] = this.q[1][1:]
	return []int{dog, 1}
}

func (this *AnimalShelf) DequeueCat() []int {
	if len(this.q[0]) == 0 {
		return []int{-1, -1}
	}
	cat := this.q[0][0]
	this.q[0] = this.q[0][1:]
	return []int{cat, 0}
}

/**
 * Your AnimalShelf object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Enqueue(animal);
 * param_2 := obj.DequeueAny();
 * param_3 := obj.DequeueDog();
 * param_4 := obj.DequeueCat();
 */
```

#### TypeScript

```ts
class AnimalShelf {
    private q: number[][] = [[], []];
    constructor() {}

    enqueue(animal: number[]): void {
        const [i, j] = animal;
        this.q[j].push(i);
    }

    dequeueAny(): number[] {
        if (this.q[0].length === 0 || (this.q[1].length > 0 && this.q[0][0] > this.q[1][0])) {
            return this.dequeueDog();
        }
        return this.dequeueCat();
    }

    dequeueDog(): number[] {
        if (this.q[1].length === 0) {
            return [-1, -1];
        }
        return [this.q[1].shift()!, 1];
    }

    dequeueCat(): number[] {
        if (this.q[0].length === 0) {
            return [-1, -1];
        }
        return [this.q[0].shift()!, 0];
    }
}

/**
 * Your AnimalShelf object will be instantiated and called as such:
 * var obj = new AnimalShelf()
 * obj.enqueue(animal)
 * var param_2 = obj.dequeueAny()
 * var param_3 = obj.dequeueDog()
 * var param_4 = obj.dequeueCat()
 */
```

#### Rust

```rust
use std::collections::VecDeque;

struct AnimalShelf {
    q: [VecDeque<i32>; 2],
}

impl AnimalShelf {
    fn new() -> Self {
        AnimalShelf {
            q: [VecDeque::new(), VecDeque::new()],
        }
    }

    fn enqueue(&mut self, animal: Vec<i32>) {
        self.q[animal[1] as usize].push_back(animal[0]);
    }

    fn dequeue_any(&mut self) -> Vec<i32> {
        if self.q[0].is_empty()
            || (!self.q[1].is_empty() && self.q[1].front().unwrap() < self.q[0].front().unwrap())
        {
            self.dequeue_dog()
        } else {
            self.dequeue_cat()
        }
    }

    fn dequeue_dog(&mut self) -> Vec<i32> {
        if self.q[1].is_empty() {
            vec![-1, -1]
        } else {
            let dog = self.q[1].pop_front().unwrap();
            vec![dog, 1]
        }
    }

    fn dequeue_cat(&mut self) -> Vec<i32> {
        if self.q[0].is_empty() {
            vec![-1, -1]
        } else {
            let cat = self.q[0].pop_front().unwrap();
            vec![cat, 0]
        }
    }
}
```

#### Swift

```swift
class AnimalShelf {
    private var q: [[Int]] = Array(repeating: [], count: 2)

    init() {
    }

    func enqueue(_ animal: [Int]) {
        q[animal[1]].append(animal[0])
    }

    func dequeueAny() -> [Int] {
        if q[0].isEmpty || (!q[1].isEmpty && q[1].first! < q[0].first!) {
            return dequeueDog()
        }
        return dequeueCat()
    }

    func dequeueDog() -> [Int] {
        return q[1].isEmpty ? [-1, -1] : [q[1].removeFirst(), 1]
    }

    func dequeueCat() -> [Int] {
        return q[0].isEmpty ? [-1, -1] : [q[0].removeFirst(), 0]
    }
}

/**
 * Your AnimalShelf object will be instantiated and called as such:
 * let obj = new AnimalShelf();
 * obj.enqueue(animal);
 * let param_2 = obj.dequeueAny();
 * let param_3 = obj.dequeueDog();
 * let param_4 = obj.dequeueCat();
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
