---
comments: true
difficulty: Easy
tags:
    - Stack
    - Array
    - Simulation
---

<!-- problem:start -->

# [682. Baseball Game](https://leetcode.com/problems/baseball-game)

[中文文档](/solution/0600-0699/0682.Baseball%20Game/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn đang ghi điểm cho một trận bóng chày với luật chơi đặc biệt. Khi bắt đầu trận đấu, bảng điểm chưa có mục nào.</p>

<p>Cho danh sách chuỗi <code>operations</code>, trong đó <code>operations[i]</code> là thao tác thứ <code>i<sup>th</sup></code> cần áp dụng lên bảng điểm và thuộc một trong các loại sau:</p>

<ul>
	<li>Một số nguyên <code>x</code>.

    <ul>
    	<li>Ghi thêm điểm mới là <code>x</code>.</li>
    </ul>
    </li>
    <li><code>&#39;+&#39;</code>.
    <ul>
    	<li>Ghi thêm điểm mới bằng tổng của hai điểm trước đó.</li>
    </ul>
    </li>
    <li><code>&#39;D&#39;</code>.
    <ul>
    	<li>Ghi thêm điểm mới bằng hai lần điểm trước đó.</li>
    </ul>
    </li>
    <li><code>&#39;C&#39;</code>.
    <ul>
    	<li>Hủy điểm trước đó và xóa điểm này khỏi bảng điểm.</li>
    </ul>
    </li>

</ul>

<p>Hãy trả về <em>tổng tất cả điểm trên bảng sau khi áp dụng toàn bộ thao tác</em>.</p>

<p>Dữ liệu kiểm thử được tạo sao cho kết quả và mọi phép tính trung gian đều nằm trong phạm vi số nguyên <strong>32-bit</strong>, đồng thời mọi thao tác đều hợp lệ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> ops = [&quot;5&quot;,&quot;2&quot;,&quot;C&quot;,&quot;D&quot;,&quot;+&quot;]
<strong>Đầu ra:</strong> 30
<strong>Giải thích:</strong>
&quot;5&quot; - Thêm 5 vào bảng điểm, lúc này bảng là [5].
&quot;2&quot; - Thêm 2 vào bảng điểm, lúc này bảng là [5, 2].
&quot;C&quot; - Hủy và xóa điểm trước đó, lúc này bảng là [5].
&quot;D&quot; - Thêm 2 * 5 = 10 vào bảng điểm, lúc này bảng là [5, 10].
&quot;+&quot; - Thêm 5 + 10 = 15 vào bảng điểm, lúc này bảng là [5, 10, 15].
Tổng điểm là 5 + 10 + 15 = 30.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> ops = [&quot;5&quot;,&quot;-2&quot;,&quot;4&quot;,&quot;C&quot;,&quot;D&quot;,&quot;9&quot;,&quot;+&quot;,&quot;+&quot;]
<strong>Đầu ra:</strong> 27
<strong>Giải thích:</strong>
&quot;5&quot; - Thêm 5 vào bảng điểm, lúc này bảng là [5].
&quot;-2&quot; - Thêm -2 vào bảng điểm, lúc này bảng là [5, -2].
&quot;4&quot; - Thêm 4 vào bảng điểm, lúc này bảng là [5, -2, 4].
&quot;C&quot; - Hủy và xóa điểm trước đó, lúc này bảng là [5, -2].
&quot;D&quot; - Thêm 2 * -2 = -4 vào bảng điểm, lúc này bảng là [5, -2, -4].
&quot;9&quot; - Thêm 9 vào bảng điểm, lúc này bảng là [5, -2, -4, 9].
&quot;+&quot; - Thêm -4 + 9 = 5 vào bảng điểm, lúc này bảng là [5, -2, -4, 9, 5].
&quot;+&quot; - Thêm 9 + 5 = 14 vào bảng điểm, lúc này bảng là [5, -2, -4, 9, 5, 14].
Tổng điểm là 5 + -2 + -4 + 9 + 5 + 14 = 27.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> ops = [&quot;1&quot;,&quot;C&quot;]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>
&quot;1&quot; - Thêm 1 vào bảng điểm, lúc này bảng là [1].
&quot;C&quot; - Hủy và xóa điểm trước đó, lúc này bảng là [].
Vì bảng điểm rỗng nên tổng điểm là 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= operations.length &lt;= 1000</code></li>
	<li><code>operations[i]</code> is <code>&quot;C&quot;</code>, <code>&quot;D&quot;</code>, <code>&quot;+&quot;</code>, or a string representing an integer in the range <code>[-3 * 10<sup>4</sup>, 3 * 10<sup>4</sup>]</code>.</li>
	<li>Với thao tác <code>&quot;+&quot;</code>, bảng điểm luôn có ít nhất hai điểm trước đó.</li>
	<li>Với thao tác <code>&quot;C&quot;</code> và <code>&quot;D&quot;</code>, bảng điểm luôn có ít nhất một điểm trước đó.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Stack + Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác tham chiếu đến điểm gần nhất, hai điểm gần nhất hoặc hủy điểm trước đó, nên cần giữ các giá trị gần nhất để truy cập.
>
> Dùng stack để lưu các lượt ghi điểm: `+` cộng hai phần tử trên cùng, `D` nhân đôi phần tử trên cùng, `C` pop phần tử đó, còn số nguyên thì push vào stack. Cuối cùng tính tổng các phần tử trong stack.

<!-- thinking:end -->

Ta có thể dùng stack để mô phỏng quá trình này.

Duyệt $\textit{operations}$ và xử lý từng thao tác:

- Nếu là `+`, cộng hai phần tử trên cùng của stack rồi push kết quả vào stack;
- Nếu là `D`, nhân đôi phần tử trên cùng của stack rồi push kết quả vào stack;
- Nếu là `C`, pop phần tử trên cùng của stack;
- Nếu là số, push số đó vào stack.

Cuối cùng, cộng tất cả phần tử trong stack để thu được đáp án.

Độ phức tạp thời gian và không gian đều là $O(n)$. Ở đây, $n$ là độ dài của $\textit{operations}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def calPoints(self, operations: List[str]) -> int:
        stk = []
        for op in operations:
            if op == "+":
                stk.append(stk[-1] + stk[-2])
            elif op == "D":
                stk.append(stk[-1] << 1)
            elif op == "C":
                stk.pop()
            else:
                stk.append(int(op))
        return sum(stk)
```

#### Java

```java
class Solution {
    public int calPoints(String[] operations) {
        Deque<Integer> stk = new ArrayDeque<>();
        for (String op : operations) {
            if ("+".equals(op)) {
                int a = stk.pop();
                int b = stk.peek();
                stk.push(a);
                stk.push(a + b);
            } else if ("D".equals(op)) {
                stk.push(stk.peek() << 1);
            } else if ("C".equals(op)) {
                stk.pop();
            } else {
                stk.push(Integer.valueOf(op));
            }
        }
        return stk.stream().mapToInt(Integer::intValue).sum();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int calPoints(vector<string>& operations) {
        vector<int> stk;
        for (auto& op : operations) {
            int n = stk.size();
            if (op == "+") {
                stk.push_back(stk[n - 1] + stk[n - 2]);
            } else if (op == "D") {
                stk.push_back(stk[n - 1] << 1);
            } else if (op == "C") {
                stk.pop_back();
            } else {
                stk.push_back(stoi(op));
            }
        }
        return accumulate(stk.begin(), stk.end(), 0);
    }
};
```

#### Go

```go
func calPoints(operations []string) (ans int) {
	var stk []int
	for _, op := range operations {
		n := len(stk)
		switch op {
		case "+":
			stk = append(stk, stk[n-1]+stk[n-2])
		case "D":
			stk = append(stk, stk[n-1]*2)
		case "C":
			stk = stk[:n-1]
		default:
			num, _ := strconv.Atoi(op)
			stk = append(stk, num)
		}
	}
	for _, x := range stk {
		ans += x
	}
	return
}
```

#### TypeScript

```ts
function calPoints(operations: string[]): number {
    const stk: number[] = [];
    for (const op of operations) {
        if (op === '+') {
            stk.push(stk.at(-1)! + stk.at(-2)!);
        } else if (op === 'D') {
            stk.push(stk.at(-1)! << 1);
        } else if (op === 'C') {
            stk.pop();
        } else {
            stk.push(+op);
        }
    }
    return stk.reduce((a, b) => a + b, 0);
}
```

#### Rust

```rust
impl Solution {
    pub fn cal_points(operations: Vec<String>) -> i32 {
        let mut stk = vec![];
        for op in operations {
            match op.as_str() {
                "+" => {
                    let n = stk.len();
                    stk.push(stk[n - 1] + stk[n - 2]);
                }
                "D" => {
                    stk.push(stk.last().unwrap() * 2);
                }
                "C" => {
                    stk.pop();
                }
                n => {
                    stk.push(n.parse::<i32>().unwrap());
                }
            }
        }
        stk.into_iter().sum()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
