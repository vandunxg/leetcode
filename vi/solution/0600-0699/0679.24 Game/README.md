---
comments: true
difficulty: Hard
tags:
    - Array
    - Math
    - Backtracking
---

<!-- problem:start -->

# [679. 24 Game](https://leetcode.com/problems/24-game)

[中文文档](/solution/0600-0699/0679.24%20Game/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>cards</code> có độ dài <code>4</code>. Bạn có bốn lá bài, mỗi lá ghi một số trong khoảng <code>[1, 9]</code>. Hãy sắp xếp các số trên lá bài thành một biểu thức toán học sử dụng các phép toán <code>[&#39;+&#39;, &#39;-&#39;, &#39;*&#39;, &#39;/&#39;]</code> và dấu ngoặc <code>&#39;(&#39;</code>, <code>&#39;)&#39;</code> để được kết quả bằng 24.</p>

<p>Bạn phải tuân theo các quy tắc sau:</p>

<ul>
	<li>Phép chia <code>&#39;/&#39;</code> là phép chia thực, không phải phép chia số nguyên.

    <ul>
    	<li>Ví dụ, <code>4 / (1 - 2 / 3) = 4 / (1 / 3) = 12</code>.</li>
    </ul>
    </li>
    <li>Mỗi phép toán phải thực hiện trên hai số. Cụ thể, không được dùng <code>&#39;-&#39;</code> như toán tử một ngôi.
    <ul>
    	<li>Ví dụ, nếu <code>cards = [1, 1, 1, 1]</code>, biểu thức <code>&quot;-1 - 1 - 1 - 1&quot;</code> sẽ <strong>không được phép</strong>.</li>
    </ul>
    </li>
    <li>Bạn không được ghép các số lại với nhau.
    <ul>
    	<li>Ví dụ, nếu <code>cards = [1, 2, 1, 2]</code>, biểu thức <code>&quot;12 + 12&quot;</code> không hợp lệ.</li>
    </ul>
    </li>

</ul>

<p>Trả về <code>true</code> nếu có thể tạo được biểu thức cho kết quả bằng <code>24</code>, nếu không thì trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> cards = [4,1,8,7]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> (8-4) * (7-1) = 24
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> cards = [1,2,1,2]
<strong>Đầu ra:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>cards.length == 4</code></li>
	<li><code>1 &lt;= cards[i] &lt;= 9</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Cần dùng bốn lá bài cùng bốn phép toán để tạo ra $24$. Không gian trạng thái rất nhỏ.
>
> Chọn hai số, áp dụng một trong các phép toán $+,-,*,/$ rồi gọi đệ quy với danh sách ngắn hơn. Nếu danh sách chỉ còn một số và số đó cách $24$ ít hơn $10^{-6}$ thì thành công; bỏ qua phép chia cho 0.

<!-- thinking:end -->

Ta xây dựng hàm $dfs(nums)$, trong đó $nums$ biểu diễn dãy số hiện tại. Hàm trả về giá trị boolean cho biết có tồn tại cách sắp xếp các số trong dãy để tạo ra kết quả $24$ hay không.

Nếu độ dài của $nums$ bằng $1$, ta chỉ trả về $true$ khi số duy nhất đó bằng $24$; nếu không thì trả về $false$.

Trong trường hợp còn lại, ta lần lượt chọn hai số bất kỳ $a$ và $b$ trong $nums$ làm toán hạng trái và phải, đồng thời thử các toán tử $op$ đặt giữa chúng. Kết quả của $a\ op\ b$ có thể được dùng làm một phần tử trong dãy số mới. Ta thêm kết quả đó vào dãy mới, xóa $a$ và $b$ khỏi $nums$, rồi gọi đệ quy hàm $dfs$. Nếu hàm trả về $true$, nghĩa là ta đã tìm được cách sắp xếp để dãy số cho kết quả $24$, và ta trả về $true$.

Nếu không trường hợp nào đã thử trả về $true$, ta trả về $false$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def judgePoint24(self, cards: List[int]) -> bool:
        def dfs(nums: List[float]):
            n = len(nums)
            if n == 1:
                if abs(nums[0] - 24) < 1e-6:
                    return True
                return False
            ok = False
            for i in range(n):
                for j in range(n):
                    if i != j:
                        nxt = [nums[k] for k in range(n) if k != i and k != j]
                        for op in ops:
                            match op:
                                case "/":
                                    if nums[j] == 0:
                                        continue
                                    ok |= dfs(nxt + [nums[i] / nums[j]])
                                case "*":
                                    ok |= dfs(nxt + [nums[i] * nums[j]])
                                case "+":
                                    ok |= dfs(nxt + [nums[i] + nums[j]])
                                case "-":
                                    ok |= dfs(nxt + [nums[i] - nums[j]])
                            if ok:
                                return True
            return ok

        ops = ("+", "-", "*", "/")
        nums = [float(x) for x in cards]
        return dfs(nums)
```

#### Java

```java
class Solution {
    private final char[] ops = {'+', '-', '*', '/'};

    public boolean judgePoint24(int[] cards) {
        List<Double> nums = new ArrayList<>();
        for (int num : cards) {
            nums.add((double) num);
        }
        return dfs(nums);
    }

    private boolean dfs(List<Double> nums) {
        int n = nums.size();
        if (n == 1) {
            return Math.abs(nums.get(0) - 24) < 1e-6;
        }
        boolean ok = false;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (i != j) {
                    List<Double> nxt = new ArrayList<>();
                    for (int k = 0; k < n; ++k) {
                        if (k != i && k != j) {
                            nxt.add(nums.get(k));
                        }
                    }
                    for (char op : ops) {
                        switch (op) {
                            case '/' -> {
                                if (nums.get(j) == 0) {
                                    continue;
                                }
                                nxt.add(nums.get(i) / nums.get(j));
                            }
                            case '*' -> {
                                nxt.add(nums.get(i) * nums.get(j));
                            }
                            case '+' -> {
                                nxt.add(nums.get(i) + nums.get(j));
                            }
                            case '-' -> {
                                nxt.add(nums.get(i) - nums.get(j));
                            }
                        }
                        ok |= dfs(nxt);
                        if (ok) {
                            return true;
                        }
                        nxt.remove(nxt.size() - 1);
                    }
                }
            }
        }
        return ok;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool judgePoint24(vector<int>& cards) {
        vector<double> nums;
        for (int num : cards) {
            nums.push_back(static_cast<double>(num));
        }
        return dfs(nums);
    }

private:
    const char ops[4] = {'+', '-', '*', '/'};

    bool dfs(vector<double>& nums) {
        int n = nums.size();
        if (n == 1) {
            return abs(nums[0] - 24) < 1e-6;
        }
        bool ok = false;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (i != j) {
                    vector<double> nxt;
                    for (int k = 0; k < n; ++k) {
                        if (k != i && k != j) {
                            nxt.push_back(nums[k]);
                        }
                    }
                    for (char op : ops) {
                        switch (op) {
                        case '/':
                            if (nums[j] == 0) {
                                continue;
                            }
                            nxt.push_back(nums[i] / nums[j]);
                            break;
                        case '*':
                            nxt.push_back(nums[i] * nums[j]);
                            break;
                        case '+':
                            nxt.push_back(nums[i] + nums[j]);
                            break;
                        case '-':
                            nxt.push_back(nums[i] - nums[j]);
                            break;
                        }
                        ok |= dfs(nxt);
                        if (ok) {
                            return true;
                        }
                        nxt.pop_back();
                    }
                }
            }
        }
        return ok;
    }
};
```

#### Go

```go
func judgePoint24(cards []int) bool {
	ops := [4]rune{'+', '-', '*', '/'}
	nums := make([]float64, len(cards))
	for i, num := range cards {
		nums[i] = float64(num)
	}
	var dfs func([]float64) bool
	dfs = func(nums []float64) bool {
		n := len(nums)
		if n == 1 {
			return math.Abs(nums[0]-24) < 1e-6
		}
		ok := false
		for i := 0; i < n; i++ {
			for j := 0; j < n; j++ {
				if i != j {
					var nxt []float64
					for k := 0; k < n; k++ {
						if k != i && k != j {
							nxt = append(nxt, nums[k])
						}
					}
					for _, op := range ops {
						switch op {
						case '/':
							if nums[j] == 0 {
								continue
							}
							nxt = append(nxt, nums[i]/nums[j])
						case '*':
							nxt = append(nxt, nums[i]*nums[j])
						case '+':
							nxt = append(nxt, nums[i]+nums[j])
						case '-':
							nxt = append(nxt, nums[i]-nums[j])
						}
						ok = ok || dfs(nxt)
						if ok {
							return true
						}
						nxt = nxt[:len(nxt)-1]
					}
				}
			}
		}
		return ok
	}

	return dfs(nums)
}
```

#### TypeScript

```ts
function judgePoint24(cards: number[]): boolean {
    const ops: string[] = ['+', '-', '*', '/'];
    const dfs = (nums: number[]): boolean => {
        const n: number = nums.length;
        if (n === 1) {
            return Math.abs(nums[0] - 24) < 1e-6;
        }
        let ok: boolean = false;
        for (let i = 0; i < n; i++) {
            for (let j = 0; j < n; j++) {
                if (i !== j) {
                    const nxt: number[] = [];
                    for (let k = 0; k < n; k++) {
                        if (k !== i && k !== j) {
                            nxt.push(nums[k]);
                        }
                    }
                    for (const op of ops) {
                        switch (op) {
                            case '/':
                                if (nums[j] === 0) {
                                    continue;
                                }
                                nxt.push(nums[i] / nums[j]);
                                break;
                            case '*':
                                nxt.push(nums[i] * nums[j]);
                                break;
                            case '+':
                                nxt.push(nums[i] + nums[j]);
                                break;
                            case '-':
                                nxt.push(nums[i] - nums[j]);
                                break;
                        }
                        ok = ok || dfs(nxt);
                        if (ok) {
                            return true;
                        }
                        nxt.pop();
                    }
                }
            }
        }
        return ok;
    };

    return dfs(cards);
}
```

#### Rust

```rust
impl Solution {
    pub fn judge_point24(cards: Vec<i32>) -> bool {
        fn dfs(nums: Vec<f64>) -> bool {
            let n = nums.len();
            if n == 1 {
                return (nums[0] - 24.0).abs() < 1e-6;
            }
            for i in 0..n {
                for j in 0..n {
                    if i == j {
                        continue;
                    }
                    let mut nxt = Vec::new();
                    for k in 0..n {
                        if k != i && k != j {
                            nxt.push(nums[k]);
                        }
                    }
                    for op in 0..4 {
                        let mut nxt2 = nxt.clone();
                        match op {
                            0 => {
                                nxt2.push(nums[i] + nums[j]);
                            }
                            1 => {
                                nxt2.push(nums[i] - nums[j]);
                            }
                            2 => {
                                nxt2.push(nums[i] * nums[j]);
                            }
                            3 => {
                                if nums[j].abs() < 1e-6 {
                                    continue;
                                }
                                nxt2.push(nums[i] / nums[j]);
                            }
                            _ => {}
                        }
                        if dfs(nxt2) {
                            return true;
                        }
                    }
                }
            }
            false
        }

        let nums: Vec<f64> = cards.into_iter().map(|x| x as f64).collect();
        dfs(nums)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
