---
comments: true
difficulty: Medium
rating: 1381
source: Biweekly Contest 87 Q2
tags:
    - Greedy
    - Array
    - Two Pointers
    - Sorting
---

<!-- problem:start -->

# [2410. Maximum Matching of Players With Trainers](https://leetcode.com/problems/maximum-matching-of-players-with-trainers)

[中文文档](/solution/2400-2499/2410.Maximum%20Matching%20of%20Players%20With%20Trainers/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <strong>0-indexed</strong> <code>players</code>, trong đó <code>players[i]</code> biểu thị <strong>khả năng</strong> của người chơi thứ <code>i<sup>th</sup></code>. Bạn cũng được cho một mảng số nguyên <strong>0-indexed</strong> <code>trainers</code>, trong đó <code>trainers[j]</code> biểu thị <strong>năng lực huấn luyện </strong> của huấn luyện viên thứ <code>j<sup>th</sup></code>.</p>

<p>Người chơi thứ <code>i<sup>th</sup></code> có thể <strong>ghép</strong> với huấn luyện viên thứ <code>j<sup>th</sup></code> nếu khả năng của người chơi <strong>nhỏ hơn hoặc bằng</strong> năng lực huấn luyện của huấn luyện viên. Ngoài ra, người chơi thứ <code>i<sup>th</sup></code> chỉ có thể được ghép với nhiều nhất một huấn luyện viên, và huấn luyện viên thứ <code>j<sup>th</sup></code> chỉ có thể được ghép với nhiều nhất một người chơi.</p>

<p>Hãy trả về <em><strong>số lượng ghép cặp lớn nhất</strong> giữa </em><code>players</code><em> và </em><code>trainers</code><em> thỏa mãn các điều kiện trên.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> players = [4,7,9], trainers = [8,2,5,8]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Một trong các cách tạo ra hai cặp ghép là:
- players[0] có thể được ghép với trainers[0] vì 4 &lt;= 8.
- players[1] có thể được ghép với trainers[3] vì 7 &lt;= 8.
Có thể chứng minh rằng 2 là số lượng cặp ghép lớn nhất có thể tạo ra.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> players = [1,1,1], trainers = [10]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>
Huấn luyện viên có thể được ghép với bất kỳ một trong 3 người chơi.
Mỗi người chơi chỉ có thể được ghép với một huấn luyện viên, nên đáp án lớn nhất là 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= players.length, trainers.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= players[i], trainers[j] &lt;= 10<sup>9</sup></code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Lưu ý:</strong> Bài này giống với <a href="https://leetcode.com/problems/assign-cookies/description/" target="_blank">445: Assign Cookies.</a></p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Two Pointers

<!-- thinking:start -->

> **Tư duy**
>
> Việc liệt kê mọi cách ghép cặp không phù hợp với $n,m\le 10^5$. Mỗi người chơi nên nhận huấn luyện viên yếu nhất nhưng vẫn có thể huấn luyện mình, để dành các huấn luyện viên mạnh hơn cho những người chơi tiếp theo.
>
> Sắp xếp cả hai mảng rồi duyệt bằng hai con trỏ: bỏ qua các huấn luyện viên yếu hơn người chơi hiện tại, sau đó thực hiện một cặp ghép. Con trỏ của huấn luyện viên không bao giờ di chuyển ngược, nên chi phí chủ yếu nằm ở bước sắp xếp.

<!-- thinking:end -->

Theo mô tả bài toán, mỗi người chơi nên được ghép với huấn luyện viên có giá trị năng lực gần nhất. Vì vậy, ta có thể sắp xếp giá trị khả năng của người chơi và năng lực của huấn luyện viên, sau đó sử dụng phương pháp hai con trỏ để ghép cặp.

Ta sử dụng hai con trỏ $i$ và $j$ lần lượt trỏ đến mảng người chơi và huấn luyện viên, ban đầu đều trỏ đến đầu mảng. Sau đó, ta lần lượt duyệt qua các giá trị khả năng của người chơi. Nếu giá trị năng lực của huấn luyện viên hiện tại nhỏ hơn giá trị khả năng của người chơi hiện tại, ta di chuyển con trỏ của huấn luyện viên sang phải một vị trí cho đến khi tìm được một huấn luyện viên có giá trị năng lực lớn hơn hoặc bằng người chơi hiện tại. Nếu không tìm được huấn luyện viên như vậy, nghĩa là người chơi hiện tại không thể được ghép với bất kỳ huấn luyện viên nào, và ta trả về chỉ số của người chơi hiện tại. Nếu không, ta ghép người chơi hiện tại với huấn luyện viên đó, rồi di chuyển cả hai con trỏ sang phải một vị trí. Tiếp tục quá trình này cho đến khi duyệt hết tất cả người chơi.

Nếu duyệt hết tất cả người chơi, nghĩa là mọi người chơi đều có thể được ghép với huấn luyện viên, và ta trả về số lượng người chơi.

Độ phức tạp thời gian là $O(m \times \log m + n \times \log n)$, và độ phức tạp không gian là $O(\max(\log m, \log n))$. Trong đó, $m$ và $n$ lần lượt là số lượng người chơi và huấn luyện viên.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def matchPlayersAndTrainers(self, players: List[int], trainers: List[int]) -> int:
        players.sort()
        trainers.sort()
        j, n = 0, len(trainers)
        for i, p in enumerate(players):
            while j < n and trainers[j] < p:
                j += 1
            if j == n:
                return i
            j += 1
        return len(players)
```

#### Java

```java
class Solution {
    public int matchPlayersAndTrainers(int[] players, int[] trainers) {
        Arrays.sort(players);
        Arrays.sort(trainers);
        int m = players.length, n = trainers.length;
        for (int i = 0, j = 0; i < m; ++i, ++j) {
            while (j < n && trainers[j] < players[i]) {
                ++j;
            }
            if (j == n) {
                return i;
            }
        }
        return m;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int matchPlayersAndTrainers(vector<int>& players, vector<int>& trainers) {
        ranges::sort(players);
        ranges::sort(trainers);
        int m = players.size(), n = trainers.size();
        for (int i = 0, j = 0; i < m; ++i, ++j) {
            while (j < n && trainers[j] < players[i]) {
                ++j;
            }
            if (j == n) {
                return i;
            }
        }
        return m;
    }
};
```

#### Go

```go
func matchPlayersAndTrainers(players []int, trainers []int) int {
	sort.Ints(players)
	sort.Ints(trainers)
	m, n := len(players), len(trainers)
	for i, j := 0, 0; i < m; i, j = i+1, j+1 {
		for j < n && trainers[j] < players[i] {
			j++
		}
		if j == n {
			return i
		}
	}
	return m
}
```

#### TypeScript

```ts
function matchPlayersAndTrainers(players: number[], trainers: number[]): number {
    players.sort((a, b) => a - b);
    trainers.sort((a, b) => a - b);
    const [m, n] = [players.length, trainers.length];
    for (let i = 0, j = 0; i < m; ++i, ++j) {
        while (j < n && trainers[j] < players[i]) {
            ++j;
        }
        if (j === n) {
            return i;
        }
    }
    return m;
}
```

#### Rust

```rust
impl Solution {
    pub fn match_players_and_trainers(mut players: Vec<i32>, mut trainers: Vec<i32>) -> i32 {
        players.sort();
        trainers.sort();
        let mut j = 0;
        let n = trainers.len();
        for (i, &p) in players.iter().enumerate() {
            while j < n && trainers[j] < p {
                j += 1;
            }
            if j == n {
                return i as i32;
            }
            j += 1;
        }
        players.len() as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
