---
comments: true
difficulty: Easy
rating: 1360
source: Weekly Contest 301 Q1
tags:
    - Greedy
    - Array
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2335. Minimum Amount of Time to Fill Cups](https://leetcode.com/problems/minimum-amount-of-time-to-fill-cups)

[中文文档](/solution/2300-2399/2335.Minimum%20Amount%20of%20Time%20to%20Fill%20Cups/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có một máy cấp nước có thể cấp nước lạnh, ấm và nóng. Mỗi giây, bạn có thể đổ đầy <code>2</code> cốc chứa các loại nước <strong>khác nhau</strong>, hoặc <code>1</code> cốc chứa bất kỳ loại nước nào.</p>

<p>Cho một mảng số nguyên <strong>có chỉ số bắt đầu từ 0</strong> <code>amount</code> có độ dài <code>3</code>, trong đó <code>amount[0]</code>, <code>amount[1]</code> và <code>amount[2]</code> lần lượt biểu thị số cốc nước lạnh, ấm và nóng cần đổ đầy. Hãy trả về <em><strong>số giây tối thiểu</strong> cần thiết để đổ đầy tất cả các cốc</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> amount = [1,4,2]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Một cách đổ đầy các cốc là:
Giây 1: Đổ đầy một cốc nước lạnh và một cốc nước ấm.
Giây 2: Đổ đầy một cốc nước ấm và một cốc nước nóng.
Giây 3: Đổ đầy một cốc nước ấm và một cốc nước nóng.
Giây 4: Đổ đầy một cốc nước ấm.
Có thể chứng minh rằng 4 là số giây tối thiểu cần thiết.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> amount = [5,4,4]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Một cách đổ đầy các cốc là:
Giây 1: Đổ đầy một cốc nước lạnh và một cốc nước nóng.
Giây 2: Đổ đầy một cốc nước lạnh và một cốc nước ấm.
Giây 3: Đổ đầy một cốc nước lạnh và một cốc nước ấm.
Giây 4: Đổ đầy một cốc nước ấm và một cốc nước nóng.
Giây 5: Đổ đầy một cốc nước lạnh và một cốc nước nóng.
Giây 6: Đổ đầy một cốc nước lạnh và một cốc nước ấm.
Giây 7: Đổ đầy một cốc nước nóng.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> amount = [5,0,0]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Mỗi giây, ta đổ đầy một cốc nước lạnh.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>amount.length == 3</code></li>
	<li><code>0 &lt;= amount[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi giây ta đổ đầy hai cốc khác loại hoặc một cốc. Tổng số cốc không quá $300$, nên ta có thể liên tục giảm hai giá trị lớn nhất hiện tại.
>
> Sắp xếp, giảm hai giá trị lớn nhất (hoặc chỉ một giá trị nếu giá trị thứ hai đã là $0$), rồi lặp lại cho đến khi tất cả bằng 0. Mỗi giây xử lý được nhiều nhu cầu nhất có thể.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def fillCups(self, amount: List[int]) -> int:
        ans = 0
        while sum(amount):
            amount.sort()
            ans += 1
            amount[2] -= 1
            amount[1] = max(0, amount[1] - 1)
        return ans
```

#### Java

```java
class Solution {
    public int fillCups(int[] amount) {
        int ans = 0;
        while (amount[0] + amount[1] + amount[2] > 0) {
            Arrays.sort(amount);
            ++ans;
            amount[2]--;
            amount[1] = Math.max(0, amount[1] - 1);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int fillCups(vector<int>& amount) {
        int ans = 0;
        while (amount[0] + amount[1] + amount[2]) {
            sort(amount.begin(), amount.end());
            ++ans;
            amount[2]--;
            amount[1] = max(0, amount[1] - 1);
        }
        return ans;
    }
};
```

#### Go

```go
func fillCups(amount []int) int {
	ans := 0
	for amount[0]+amount[1]+amount[2] > 0 {
		sort.Ints(amount)
		ans++
		amount[2]--
		if amount[1] > 0 {
			amount[1]--
		}
	}
	return ans
}
```

#### TypeScript

```ts
function fillCups(amount: number[]): number {
    amount.sort((a, b) => a - b);
    let [a, b, c] = amount;
    let diff = a + b - c;
    if (diff <= 0) return c;
    else return Math.floor((diff + 1) / 2) + c;
}
```

#### Rust

```rust
impl Solution {
    pub fn fill_cups(mut amount: Vec<i32>) -> i32 {
        amount.sort();
        let dif = amount[0] + amount[1] - amount[2];
        if dif <= 0 {
            return amount[2];
        }
        (dif + 1) / 2 + amount[2]
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 mô phỏng từng giây. Sau khi sắp xếp $a \le b \le c$, ta có công thức đóng: nếu $a+b \le c$, hai lượng nhỏ hơn sẽ được xử lý trong $c$ giây; ngược lại, ta luôn ghép hai cốc, và đáp án là $\lfloor (a+b+c+1)/2 \rfloor$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def fillCups(self, amount: List[int]) -> int:
        amount.sort()
        if amount[0] + amount[1] <= amount[2]:
            return amount[2]
        return (sum(amount) + 1) // 2
```

#### Java

```java
class Solution {
    public int fillCups(int[] amount) {
        Arrays.sort(amount);
        if (amount[0] + amount[1] <= amount[2]) {
            return amount[2];
        }
        return (amount[0] + amount[1] + amount[2] + 1) / 2;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int fillCups(vector<int>& amount) {
        sort(amount.begin(), amount.end());
        if (amount[0] + amount[1] <= amount[2]) {
            return amount[2];
        }
        return (amount[0] + amount[1] + amount[2] + 1) / 2;
    }
};
```

#### Go

```go
func fillCups(amount []int) int {
	sort.Ints(amount)
	if amount[0]+amount[1] <= amount[2] {
		return amount[2]
	}
	return (amount[0] + amount[1] + amount[2] + 1) / 2
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
