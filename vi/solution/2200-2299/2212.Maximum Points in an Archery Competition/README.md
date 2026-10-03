---
comments: true
difficulty: Medium
rating: 1868
source: Weekly Contest 285 Q3
tags:
    - Bit Manipulation
    - Array
    - Backtracking
    - Enumeration
---

<!-- problem:start -->

# [2212. Maximum Points in an Archery Competition](https://leetcode.com/problems/maximum-points-in-an-archery-competition)

[中文文档](/solution/2200-2299/2212.Maximum%20Points%20in%20an%20Archery%20Competition/README.md)

## Mô tả

<!-- description:start -->

<p>Alice và Bob là đối thủ trong một cuộc thi bắn cung. Cuộc thi có các quy tắc sau:</p>

<ol>
	<li>Alice bắn <code>numArrows</code> mũi tên trước, sau đó Bob bắn <code>numArrows</code> mũi tên.</li>
	<li>Điểm được tính như sau:
	<ol>
		<li>Bia có các vùng tính điểm nguyên từ <code>0</code> đến <code>11</code> <strong>bao gồm cả hai đầu</strong>.</li>
		<li>Với <strong>mỗi</strong> vùng của bia có điểm <code>k</code> (từ <code>0</code> đến <code>11</code>), giả sử Alice và Bob lần lượt bắn <code>a<sub>k</sub></code> và <code>b<sub>k</sub></code> mũi tên vào vùng đó. Nếu <code>a<sub>k</sub> &gt;= b<sub>k</sub></code>, Alice nhận <code>k</code> điểm. Nếu <code>a<sub>k</sub> &lt; b<sub>k</sub></code>, Bob nhận <code>k</code> điểm.</li>
		<li>Tuy nhiên, nếu <code>a<sub>k</sub> == b<sub>k</sub> == 0</code> thì <strong>không ai</strong> nhận được <code>k</code> điểm.</li>
	</ol>
	</li>
</ol>

<ul>
	<li>
	<p>Ví dụ, nếu Alice và Bob đều bắn <code>2</code> mũi tên vào vùng có điểm <code>11</code>, Alice sẽ nhận <code>11</code> điểm. Ngược lại, nếu Alice bắn <code>0</code> mũi tên vào vùng có điểm <code>11</code> còn Bob bắn <code>2</code> mũi tên vào cùng vùng đó, Bob sẽ nhận <code>11</code> điểm.</p>
	</li>
</ul>

<p>Bạn được cho số nguyên <code>numArrows</code> và mảng số nguyên <code>aliceArrows</code> có kích thước <code>12</code>, biểu diễn số mũi tên Alice bắn vào mỗi vùng tính điểm từ <code>0</code> đến <code>11</code>. Bob muốn <strong>tối đa hóa</strong> tổng số điểm có thể đạt được.</p>

<p>Trả về <em>mảng </em><code>bobArrows</code><em> biểu diễn số mũi tên Bob bắn vào <strong>từng</strong> vùng tính điểm từ </em><code>0</code><em> đến </em><code>11</code>. Tổng các giá trị trong <code>bobArrows</code> phải bằng <code>numArrows</code>.</p>

<p>Nếu có nhiều cách giúp Bob đạt được tổng điểm tối đa, hãy trả về <strong>bất kỳ</strong> cách nào trong số đó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2212.Maximum%20Points%20in%20an%20Archery%20Competition/images/ex1.jpg" style="width: 600px; height: 120px;" />
<pre>
<strong>Đầu vào:</strong> numArrows = 9, aliceArrows = [1,1,0,1,0,0,2,1,0,1,2,0]
<strong>Đầu ra:</strong> [0,0,0,0,1,1,0,0,1,2,3,1]
<strong>Giải thích:</strong> Bảng trên cho biết cách tính điểm của cuộc thi.
Bob đạt tổng cộng 4 + 5 + 8 + 9 + 10 + 11 = 47 điểm.
Có thể chứng minh rằng Bob không thể đạt được số điểm cao hơn 47.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2212.Maximum%20Points%20in%20an%20Archery%20Competition/images/ex2new.jpg" style="width: 600px; height: 117px;" />
<pre>
<strong>Đầu vào:</strong> numArrows = 3, aliceArrows = [0,0,1,0,0,0,0,0,0,0,0,2]
<strong>Đầu ra:</strong> [0,0,0,0,0,0,0,0,1,1,1,0]
<strong>Giải thích:</strong> Bảng trên cho biết cách tính điểm của cuộc thi.
Bob đạt tổng cộng 8 + 9 + 10 = 27 điểm.
Có thể chứng minh rằng Bob không thể đạt được số điểm cao hơn 27.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= numArrows &lt;= 10<sup>5</sup></code></li>
	<li><code>aliceArrows.length == bobArrows.length == 12</code></li>
	<li><code>0 &lt;= aliceArrows[i], bobArrows[i] &lt;= numArrows</code></li>
	<li><code>sum(aliceArrows[i]) == numArrows</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Bob phải phân bổ mũi tên vào $12$ vùng và chỉ ghi điểm ở một vùng khi bắn nhiều mũi tên hơn Alice. Việc tìm số mũi tên chính xác cho từng vùng là không khả thi vì $\textit{numArrows}$ có thể lên tới $10^5$. Chỉ có $12$ vùng, nên lựa chọn thực sự là những vùng nào Bob sẽ thắng.
>
> Để thắng vùng $i$, Bob cần $aliceArrows[i]+1$ mũi tên và nhận được $i$ điểm. Ta liệt kê một mask $12$ bit biểu diễn các vùng cần thắng, tính tổng chi phí và điểm, rồi giữ lại mask khả thi tốt nhất.
>
> Từ mask đó, ta dựng lại mảng số mũi tên và dồn các mũi tên còn thừa vào vùng $0$. Có $2^{12}$ mask nên việc liệt kê này rất nhẹ.

<!-- thinking:end -->

Vì chỉ có $12$ vùng, ta dùng liệt kê nhị phân để xác định những vùng mà $\textit{Bob}$ ghi điểm. Ta dùng biến $\textit{st}$ để biểu diễn phương án giúp $\textit{Bob}$ đạt điểm cao nhất, và $\textit{mx}$ để biểu diễn số điểm tối đa $\textit{Bob}$ đạt được.

Ta liệt kê các phương án ghi điểm của $\textit{Bob}$ trong khoảng $[1, 2^m)$, với $m$ là độ dài của $\textit{aliceArrows}$. Với mỗi phương án, ta tính điểm $\textit{s}$ và số mũi tên $\textit{cnt}$ của $\textit{Bob}$. Nếu $\textit{cnt} \leq \textit{numArrows}$ và $\textit{s} > \textit{mx}$, ta cập nhật $\textit{mx}$ và $\textit{st}$.

Sau đó, ta tính phương án ghi điểm của $\textit{Bob}$ dựa trên $\textit{st}$. Nếu còn mũi tên chưa phân bổ, ta đặt toàn bộ số mũi tên còn lại vào vùng đầu tiên, tức vùng có chỉ số $0$.

Độ phức tạp thời gian là $O(2^m \times m)$, với $m$ là độ dài của $\textit{aliceArrows}$. Không tính phần không gian của mảng kết quả, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumBobPoints(self, numArrows: int, aliceArrows: List[int]) -> List[int]:
        st = mx = 0
        m = len(aliceArrows)
        for mask in range(1, 1 << m):
            cnt = s = 0
            for i, x in enumerate(aliceArrows):
                if mask >> i & 1:
                    s += i
                    cnt += x + 1
            if cnt <= numArrows and s > mx:
                mx = s
                st = mask
        ans = [0] * m
        for i, x in enumerate(aliceArrows):
            if st >> i & 1:
                ans[i] = x + 1
                numArrows -= ans[i]
        ans[0] += numArrows
        return ans
```

#### Java

```java
class Solution {
    public int[] maximumBobPoints(int numArrows, int[] aliceArrows) {
        int st = 0, mx = 0;
        int m = aliceArrows.length;
        for (int mask = 1; mask < 1 << m; ++mask) {
            int cnt = 0, s = 0;
            for (int i = 0; i < m; ++i) {
                if ((mask >> i & 1) == 1) {
                    s += i;
                    cnt += aliceArrows[i] + 1;
                }
            }
            if (cnt <= numArrows && s > mx) {
                mx = s;
                st = mask;
            }
        }
        int[] ans = new int[m];
        for (int i = 0; i < m; ++i) {
            if ((st >> i & 1) == 1) {
                ans[i] = aliceArrows[i] + 1;
                numArrows -= ans[i];
            }
        }
        ans[0] += numArrows;
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> maximumBobPoints(int numArrows, vector<int>& aliceArrows) {
        int st = 0, mx = 0;
        int m = aliceArrows.size();
        for (int mask = 1; mask < 1 << m; ++mask) {
            int cnt = 0, s = 0;
            for (int i = 0; i < m; ++i) {
                if (mask >> i & 1) {
                    s += i;
                    cnt += aliceArrows[i] + 1;
                }
            }
            if (cnt <= numArrows && s > mx) {
                mx = s;
                st = mask;
            }
        }
        vector<int> ans(m);
        for (int i = 0; i < m; ++i) {
            if (st >> i & 1) {
                ans[i] = aliceArrows[i] + 1;
                numArrows -= ans[i];
            }
        }
        ans[0] += numArrows;
        return ans;
    }
};
```

#### Go

```go
func maximumBobPoints(numArrows int, aliceArrows []int) []int {
	st, mx := 0, 0
	m := len(aliceArrows)
	for mask := 1; mask < 1<<m; mask++ {
		cnt, s := 0, 0
		for i, x := range aliceArrows {
			if mask>>i&1 == 1 {
				s += i
				cnt += x + 1
			}
		}
		if cnt <= numArrows && s > mx {
			mx = s
			st = mask
		}
	}
	ans := make([]int, m)
	for i, x := range aliceArrows {
		if (st>>i)&1 == 1 {
			ans[i] = x + 1
			numArrows -= ans[i]
		}
	}
	ans[0] += numArrows
	return ans
}
```

#### TypeScript

```ts
function maximumBobPoints(numArrows: number, aliceArrows: number[]): number[] {
    let [st, mx] = [0, 0];
    const m = aliceArrows.length;
    for (let mask = 1; mask < 1 << m; mask++) {
        let [cnt, s] = [0, 0];
        for (let i = 0; i < m; i++) {
            if ((mask >> i) & 1) {
                cnt += aliceArrows[i] + 1;
                s += i;
            }
        }
        if (cnt <= numArrows && s > mx) {
            mx = s;
            st = mask;
        }
    }
    const ans: number[] = Array(m).fill(0);
    for (let i = 0; i < m; i++) {
        if ((st >> i) & 1) {
            ans[i] = aliceArrows[i] + 1;
            numArrows -= ans[i];
        }
    }
    ans[0] += numArrows;
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn maximum_bob_points(num_arrows: i32, alice_arrows: Vec<i32>) -> Vec<i32> {
        let mut st = 0;
        let mut mx = 0;
        let m = alice_arrows.len();
        for mask in 1..(1 << m) {
            let mut cnt = 0;
            let mut s = 0;
            for i in 0..m {
                if (mask >> i) & 1 == 1 {
                    s += i as i32;
                    cnt += alice_arrows[i] + 1;
                }
            }
            if cnt <= num_arrows && s > mx {
                mx = s;
                st = mask;
            }
        }
        let mut ans = vec![0; m];
        let mut num_arrows = num_arrows;
        for i in 0..m {
            if (st >> i) & 1 == 1 {
                ans[i] = alice_arrows[i] + 1;
                num_arrows -= ans[i];
            }
        }
        ans[0] += num_arrows;
        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {number} numArrows
 * @param {number[]} aliceArrows
 * @return {number[]}
 */
var maximumBobPoints = function (numArrows, aliceArrows) {
    let [st, mx] = [0, 0];
    const m = aliceArrows.length;
    for (let mask = 1; mask < 1 << m; mask++) {
        let [cnt, s] = [0, 0];
        for (let i = 0; i < m; i++) {
            if ((mask >> i) & 1) {
                cnt += aliceArrows[i] + 1;
                s += i;
            }
        }
        if (cnt <= numArrows && s > mx) {
            mx = s;
            st = mask;
        }
    }
    const ans = Array(m).fill(0);
    for (let i = 0; i < m; i++) {
        if ((st >> i) & 1) {
            ans[i] = aliceArrows[i] + 1;
            numArrows -= ans[i];
        }
    }
    ans[0] += numArrows;
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
