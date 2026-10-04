---
comments: true
difficulty: Easy
rating: 1324
source: Weekly Contest 343 Q1
tags:
    - Array
    - Simulation
---

<!-- problem:start -->

# [2660. Determine the Winner of a Bowling Game](https://leetcode.com/problems/determine-the-winner-of-a-bowling-game)

[中文文档](/solution/2600-2699/2660.Determine%20the%20Winner%20of%20a%20Bowling%20Game/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng số nguyên <strong>được đánh chỉ số từ 0</strong> là <code><font face="monospace">player1</font></code> và <code>player2</code>, lần lượt biểu diễn số pin mà người chơi 1 và người chơi 2 hạ được trong một ván bowling.</p>

<p>Ván bowling gồm <code>n</code> lượt, và số pin trong mỗi lượt luôn là 10.</p>

<p>Giả sử một người chơi hạ được <code>x<sub>i</sub></code> pin ở lượt thứ i<sup>th</sup>. Giá trị của lượt thứ i<sup>th</sup> đối với người chơi đó là:</p>

<ul>
	<li><code>2x<sub>i</sub></code> nếu người chơi hạ được 10 pin <b>ở lượt thứ (i - 1)<sup>th</sup> hoặc (i - 2)<sup>th</sup></b>.</li>
	<li>Nếu không, giá trị của lượt đó là <code>x<sub>i</sub></code>.</li>
</ul>

<p><strong>Điểm số</strong> của người chơi là tổng giá trị của <code>n</code> lượt.</p>

<p>Hãy trả về</p>

<ul>
	<li>1 nếu điểm của người chơi 1 lớn hơn điểm của người chơi 2,</li>
	<li>2 nếu điểm của người chơi 2 lớn hơn điểm của người chơi 1, và</li>
	<li>0 nếu hai người hòa.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">player1 = [5,10,3,2], player2 = [6,5,7,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Điểm của người chơi 1 là 5 + 10 + 2*3 + 2*2 = 25.</p>

<p>Điểm của người chơi 2 là 6 + 5 + 7 + 3 = 21.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">player1 = [3,5,7,6], player2 = [8,10,10,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Điểm của người chơi 1 là 3 + 5 + 7 + 6 = 21.</p>

<p>Điểm của người chơi 2 là 8 + 10 + 2*10 + 2*2 = 42.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">player1 = [2,3], player2 = [4,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Điểm của người chơi 1 là 2 + 3 = 5.</p>

<p>Điểm của người chơi 2 là 4 + 1 = 5.</p>
</div>

<p><strong class="example">Ví dụ 4:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">player1 = [1,1,1,10,10,10,10], player2 = [10,10,10,10,1,1,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Điểm của người chơi 1 là 1 + 1 + 1 + 10 + 2*10 + 2*10 + 2*10 = 73.</p>

<p>Điểm của người chơi 2 là 10 + 2*10 + 2*10 + 2*10 + 2*1 + 2*1 + 1 = 75.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == player1.length == player2.length</code></li>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
	<li><code>0 &lt;= player1[i], player2[i] &lt;= 10</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Điểm số chỉ phụ thuộc vào việc có xuất hiện một số $10$ trong hai lượt trước đó hay không. Vì độ dài mảng tối đa là $1000$, ta cộng dồn điểm của cả hai người chơi theo quy tắc này rồi so sánh.

<!-- thinking:end -->

Ta có thể định nghĩa một hàm $f(arr)$ để tính điểm của hai người chơi, lần lượt là $a$ và $b$, sau đó trả về đáp án dựa trên quan hệ giữa $a$ và $b$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isWinner(self, player1: List[int], player2: List[int]) -> int:
        def f(arr: List[int]) -> int:
            s = 0
            for i, x in enumerate(arr):
                k = 2 if (i and arr[i - 1] == 10) or (i > 1 and arr[i - 2] == 10) else 1
                s += k * x
            return s

        a, b = f(player1), f(player2)
        return 1 if a > b else (2 if b > a else 0)
```

#### Java

```java
class Solution {
    public int isWinner(int[] player1, int[] player2) {
        int a = f(player1), b = f(player2);
        return a > b ? 1 : b > a ? 2 : 0;
    }

    private int f(int[] arr) {
        int s = 0;
        for (int i = 0; i < arr.length; ++i) {
            int k = (i > 0 && arr[i - 1] == 10) || (i > 1 && arr[i - 2] == 10) ? 2 : 1;
            s += k * arr[i];
        }
        return s;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int isWinner(vector<int>& player1, vector<int>& player2) {
        auto f = [](vector<int>& arr) {
            int s = 0;
            for (int i = 0, n = arr.size(); i < n; ++i) {
                int k = (i && arr[i - 1] == 10) || (i > 1 && arr[i - 2] == 10) ? 2 : 1;
                s += k * arr[i];
            }
            return s;
        };
        int a = f(player1), b = f(player2);
        return a > b ? 1 : (b > a ? 2 : 0);
    }
};
```

#### Go

```go
func isWinner(player1 []int, player2 []int) int {
	f := func(arr []int) int {
		s := 0
		for i, x := range arr {
			k := 1
			if (i > 0 && arr[i-1] == 10) || (i > 1 && arr[i-2] == 10) {
				k = 2
			}
			s += k * x
		}
		return s
	}
	a, b := f(player1), f(player2)
	if a > b {
		return 1
	}
	if b > a {
		return 2
	}
	return 0
}
```

#### TypeScript

```ts
function isWinner(player1: number[], player2: number[]): number {
    const f = (arr: number[]): number => {
        let s = 0;
        for (let i = 0; i < arr.length; ++i) {
            s += arr[i];
            if ((i && arr[i - 1] === 10) || (i > 1 && arr[i - 2] === 10)) {
                s += arr[i];
            }
        }
        return s;
    };
    const a = f(player1);
    const b = f(player2);
    return a > b ? 1 : a < b ? 2 : 0;
}
```

#### Rust

```rust
impl Solution {
    pub fn is_winner(player1: Vec<i32>, player2: Vec<i32>) -> i32 {
        let f = |arr: &Vec<i32>| -> i32 {
            let mut s = 0;
            for i in 0..arr.len() {
                let mut k = 1;
                if (i > 0 && arr[i - 1] == 10) || (i > 1 && arr[i - 2] == 10) {
                    k = 2;
                }
                s += k * arr[i];
            }
            s
        };

        let a = f(&player1);
        let b = f(&player2);
        if a > b {
            1
        } else if a < b {
            2
        } else {
            0
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
