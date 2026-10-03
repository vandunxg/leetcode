---
comments: true
difficulty: Medium
rating: 1320
source: Weekly Contest 268 Q2
tags:
    - Array
    - Simulation
---

<!-- problem:start -->

# [2079. Watering Plants](https://leetcode.com/problems/watering-plants)

[中文文档](/solution/2000-2099/2079.Watering%20Plants/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn muốn tưới <code>n</code> cây trong vườn bằng một bình tưới. Các cây được xếp thành một hàng và được đánh số từ <code>0</code> đến <code>n - 1</code> từ trái sang phải, trong đó cây thứ <code>i<sup>th</sup></code> nằm tại <code>x = i</code>. Có một con sông tại <code>x = -1</code>, nơi bạn có thể lấy thêm nước cho bình tưới.</p>

<p>Mỗi cây cần một lượng nước cụ thể. Bạn sẽ tưới cây theo cách sau:</p>

<ul>
	<li>Tưới cây theo thứ tự từ trái sang phải.</li>
	<li>Sau khi tưới cây hiện tại, nếu lượng nước còn lại không đủ để tưới <strong>hoàn toàn</strong> cây tiếp theo, hãy quay lại sông để đổ đầy bình tưới.</li>
	<li><strong>Không được</strong> đổ thêm nước sớm.</li>
</ul>

<p>Ban đầu, bạn đang ở sông (tức là <code>x = -1</code>). Mất <strong>một bước</strong> để di chuyển <strong>một đơn vị</strong> trên trục x.</p>

<p>Cho một mảng số nguyên <strong>đánh chỉ số từ 0</strong> <code>plants</code> gồm <code>n</code> phần tử, trong đó <code>plants[i]</code> là lượng nước cần cho cây thứ <code>i<sup>th</sup></code>, và một số nguyên <code>capacity</code> biểu thị sức chứa của bình tưới, hãy trả về <em><strong>số bước</strong> cần thiết để tưới tất cả các cây</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> plants = [2,2,3,3], capacity = 5
<strong>Đầu ra:</strong> 14
<strong>Giải thích:</strong> Bắt đầu tại sông với bình tưới đầy nước:
- Đi đến cây 0 (1 bước) và tưới cây. Bình tưới còn 3 đơn vị nước.
- Đi đến cây 1 (1 bước) và tưới cây. Bình tưới còn 1 đơn vị nước.
- Vì không thể tưới hoàn toàn cây 2, quay lại sông để đổ đầy bình (2 bước).
- Đi đến cây 2 (3 bước) và tưới cây. Bình tưới còn 2 đơn vị nước.
- Vì không thể tưới hoàn toàn cây 3, quay lại sông để đổ đầy bình (3 bước).
- Đi đến cây 3 (4 bước) và tưới cây.
Số bước cần thiết = 1 + 1 + 2 + 3 + 3 + 4 = 14.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> plants = [1,1,1,4,2,3], capacity = 4
<strong>Đầu ra:</strong> 30
<strong>Giải thích:</strong> Bắt đầu tại sông với bình tưới đầy nước:
- Tưới các cây 0, 1 và 2 (3 bước). Quay lại sông (3 bước).
- Tưới cây 3 (4 bước). Quay lại sông (4 bước).
- Tưới cây 4 (5 bước). Quay lại sông (5 bước).
- Tưới cây 5 (6 bước).
Số bước cần thiết = 3 + 3 + 4 + 4 + 5 + 5 + 6 = 30.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> plants = [7,7,7,7,7,7,7], capacity = 8
<strong>Đầu ra:</strong> 49
<strong>Giải thích:</strong> Bạn phải đổ đầy bình trước khi tưới từng cây.
Số bước cần thiết = 1 + 1 + 2 + 2 + 3 + 3 + 4 + 4 + 5 + 5 + 6 + 6 + 7 = 49.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == plants.length</code></li>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
	<li><code>1 &lt;= plants[i] &lt;= 10<sup>6</sup></code></li>
	<li><code>max(plants[i]) &lt;= capacity &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Sức chứa luôn lớn hơn hoặc bằng nhu cầu của từng cây; ta tưới cây theo thứ tự từ sông. Khi bình không đủ nước cho cây $i$, ta đổ đầy bình, mất $2i+1$ bước.
>
> Theo dõi lượng nước còn lại: nếu đủ thì trừ đi và tiến một bước, nếu không thì cộng $2i+1$ vào số bước và đặt lại lượng nước còn lại thành $capacity-p$.

<!-- thinking:end -->

Ta có thể mô phỏng quá trình tưới cây. Ta dùng biến $\textit{water}$ để biểu diễn lượng nước hiện tại trong bình tưới, ban đầu $\textit{water} = \textit{capacity}$.

Ta duyệt qua các cây. Với mỗi cây:

- Nếu lượng nước hiện tại trong bình đủ để tưới cây này, ta tiến lên một bước, tưới cây và cập nhật $\textit{water} = \textit{water} - \textit{plants}[i]$.
- Nếu không, ta cần quay lại sông để đổ đầy bình, đi ngược lại đến vị trí hiện tại, sau đó tiến lên một bước. Số bước cần đi là $i \times 2 + 1$. Tiếp theo, ta tưới cây này và cập nhật $\textit{water} = \textit{capacity} - \textit{plants}[i]$.

Cuối cùng, trả về tổng số bước.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là số cây. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def wateringPlants(self, plants: List[int], capacity: int) -> int:
        ans, water = 0, capacity
        for i, p in enumerate(plants):
            if water >= p:
                water -= p
                ans += 1
            else:
                water = capacity - p
                ans += i * 2 + 1
        return ans
```

#### Java

```java
class Solution {
    public int wateringPlants(int[] plants, int capacity) {
        int ans = 0, water = capacity;
        for (int i = 0; i < plants.length; ++i) {
            if (water >= plants[i]) {
                water -= plants[i];
                ans += 1;
            } else {
                water = capacity - plants[i];
                ans += i * 2 + 1;
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int wateringPlants(vector<int>& plants, int capacity) {
        int ans = 0, water = capacity;
        for (int i = 0; i < plants.size(); ++i) {
            if (water >= plants[i]) {
                water -= plants[i];
                ans += 1;
            } else {
                water = capacity - plants[i];
                ans += i * 2 + 1;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func wateringPlants(plants []int, capacity int) (ans int) {
	water := capacity
	for i, p := range plants {
		if water >= p {
			water -= p
			ans++
		} else {
			water = capacity - p
			ans += i*2 + 1
		}
	}
	return
}
```

#### TypeScript

```ts
function wateringPlants(plants: number[], capacity: number): number {
    let [ans, water] = [0, capacity];
    for (let i = 0; i < plants.length; ++i) {
        if (water >= plants[i]) {
            water -= plants[i];
            ++ans;
        } else {
            water = capacity - plants[i];
            ans += i * 2 + 1;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn watering_plants(plants: Vec<i32>, capacity: i32) -> i32 {
        let mut ans = 0;
        let mut water = capacity;
        for (i, &p) in plants.iter().enumerate() {
            if water >= p {
                water -= p;
                ans += 1;
            } else {
                water = capacity - p;
                ans += (i as i32) * 2 + 1;
            }
        }
        ans
    }
}
```

#### C

```c
int wateringPlants(int* plants, int plantsSize, int capacity) {
    int ans = 0, water = capacity;
    for (int i = 0; i < plantsSize; ++i) {
        if (water >= plants[i]) {
            water -= plants[i];
            ans += 1;
        } else {
            water = capacity - plants[i];
            ans += i * 2 + 1;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
