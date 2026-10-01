---
comments: true
difficulty: Medium
tags:
    - Design
    - Queue
    - Array
    - Hash Table
    - Linked List
---

<!-- problem:start -->

# [379. Design Phone Directory 🔒](https://leetcode.com/problems/design-phone-directory)

[中文文档](/solution/0300-0399/0379.Design%20Phone%20Directory/README.md)

## Mô tả

<!-- description:start -->

<p>Thiết kế một danh bạ điện thoại ban đầu có <code>maxNumbers</code> ô trống để lưu số điện thoại. Danh bạ cần lưu số, kiểm tra một ô có trống hay không và giải phóng một ô được chỉ định.</p>

<p>Hãy triển khai class <code>PhoneDirectory</code>:</p>

<ul>
	<li><code>PhoneDirectory(int maxNumbers)</code> Khởi tạo danh bạ điện thoại với <code>maxNumbers</code> ô khả dụng.</li>
	<li><code>int get()</code> Trả về một số chưa được cấp cho ai. Trả về <code>-1</code> nếu không còn số khả dụng.</li>
	<li><code>bool check(int number)</code> Trả về <code>true</code> nếu ô <code>number</code> còn khả dụng, ngược lại trả về <code>false</code>.</li>
	<li><code>void release(int number)</code> Thu hồi hoặc giải phóng ô <code>number</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;PhoneDirectory&quot;, &quot;get&quot;, &quot;get&quot;, &quot;check&quot;, &quot;get&quot;, &quot;check&quot;, &quot;release&quot;, &quot;check&quot;]
[[3], [], [], [2], [], [2], [2], [2]]
<strong>Đầu ra</strong>
[null, 0, 1, true, 2, false, null, true]

<strong>Giải thích</strong>
PhoneDirectory phoneDirectory = new PhoneDirectory(3);
phoneDirectory.get();      // It can return any available phone number. Here we assume it returns 0.
phoneDirectory.get();      // Assume it returns 1.
phoneDirectory.check(2);   // The number 2 is available, so return true.
phoneDirectory.get();      // It returns 2, the only number that is left.
phoneDirectory.check(2);   // The number 2 is no longer available, so return false.
phoneDirectory.release(2); // Release number 2 back to the pool.
phoneDirectory.check(2);   // Number 2 is available again, return true.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= maxNumbers &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= number &lt; maxNumbers</code></li>
	<li>Có tối đa <code>2 * 10<sup>4</sup></code> lần gọi các phương thức <code>get</code>, <code>check</code> và <code>release</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần một pool số để cấp phát, kiểm tra số còn trống và giải phóng số, tất cả với thời gian kỳ vọng hằng số. Duyệt mảng để tìm một ô trống sẽ chậm.
>
> Dùng hash set lưu các số chưa được cấp phát. `get` lấy ra một phần tử bất kỳ, `check` kiểm tra phần tử có trong set hay không, còn `release` thêm phần tử trở lại. Nếu set rỗng thì trả về $-1$.

<!-- thinking:end -->

Ta có thể dùng hash set `available` để lưu các số điện thoại chưa được cấp phát. Ban đầu, hash set chứa `[0, 1, 2, ..., maxNumbers - 1]`.

Khi gọi phương thức `get`, ta lấy một số điện thoại chưa được cấp phát từ `available`. Nếu `available` rỗng, ta trả về `-1`. Độ phức tạp thời gian là $O(1)$.

Khi gọi phương thức `check`, ta chỉ cần kiểm tra `number` có nằm trong `available` hay không. Độ phức tạp thời gian là $O(1)$.

Khi gọi phương thức `release`, ta thêm `number` vào `available`. Độ phức tạp thời gian là $O(1)$.

Độ phức tạp không gian là $O(n)$, trong đó $n$ là giá trị của `maxNumbers`.

<!-- tabs:start -->

#### Python3

```python
class PhoneDirectory:

    def __init__(self, maxNumbers: int):
        self.available = set(range(maxNumbers))

    def get(self) -> int:
        if not self.available:
            return -1
        return self.available.pop()

    def check(self, number: int) -> bool:
        return number in self.available

    def release(self, number: int) -> None:
        self.available.add(number)


# Your PhoneDirectory object will be instantiated and called as such:
# obj = PhoneDirectory(maxNumbers)
# param_1 = obj.get()
# param_2 = obj.check(number)
# obj.release(number)
```

#### Java

```java
class PhoneDirectory {
    private Set<Integer> available = new HashSet<>();

    public PhoneDirectory(int maxNumbers) {
        for (int i = 0; i < maxNumbers; ++i) {
            available.add(i);
        }
    }

    public int get() {
        if (available.isEmpty()) {
            return -1;
        }
        int x = available.iterator().next();
        available.remove(x);
        return x;
    }

    public boolean check(int number) {
        return available.contains(number);
    }

    public void release(int number) {
        available.add(number);
    }
}

/**
 * Your PhoneDirectory object will be instantiated and called as such:
 * PhoneDirectory obj = new PhoneDirectory(maxNumbers);
 * int param_1 = obj.get();
 * boolean param_2 = obj.check(number);
 * obj.release(number);
 */
```

#### C++

```cpp
class PhoneDirectory {
public:
    PhoneDirectory(int maxNumbers) {
        for (int i = 0; i < maxNumbers; ++i) {
            available.insert(i);
        }
    }

    int get() {
        if (available.empty()) {
            return -1;
        }
        int x = *available.begin();
        available.erase(x);
        return x;
    }

    bool check(int number) {
        return available.contains(number);
    }

    void release(int number) {
        available.insert(number);
    }

private:
    unordered_set<int> available;
};

/**
 * Your PhoneDirectory object will be instantiated and called as such:
 * PhoneDirectory* obj = new PhoneDirectory(maxNumbers);
 * int param_1 = obj->get();
 * bool param_2 = obj->check(number);
 * obj->release(number);
 */
```

#### Go

```go
type PhoneDirectory struct {
	available map[int]bool
}

func Constructor(maxNumbers int) PhoneDirectory {
	available := make(map[int]bool)
	for i := 0; i < maxNumbers; i++ {
		available[i] = true
	}
	return PhoneDirectory{available}
}

func (this *PhoneDirectory) Get() int {
	for k := range this.available {
		delete(this.available, k)
		return k
	}
	return -1
}

func (this *PhoneDirectory) Check(number int) bool {
	_, ok := this.available[number]
	return ok
}

func (this *PhoneDirectory) Release(number int) {
	this.available[number] = true
}

/**
 * Your PhoneDirectory object will be instantiated and called as such:
 * obj := Constructor(maxNumbers);
 * param_1 := obj.Get();
 * param_2 := obj.Check(number);
 * obj.Release(number);
 */
```

#### TypeScript

```ts
class PhoneDirectory {
    private available: Set<number> = new Set();

    constructor(maxNumbers: number) {
        for (let i = 0; i < maxNumbers; ++i) {
            this.available.add(i);
        }
    }

    get(): number {
        const [x] = this.available;
        if (x === undefined) {
            return -1;
        }
        this.available.delete(x);
        return x;
    }

    check(number: number): boolean {
        return this.available.has(number);
    }

    release(number: number): void {
        this.available.add(number);
    }
}

/**
 * Your PhoneDirectory object will be instantiated and called as such:
 * var obj = new PhoneDirectory(maxNumbers)
 * var param_1 = obj.get()
 * var param_2 = obj.check(number)
 * obj.release(number)
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
