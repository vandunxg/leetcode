---
comments: true
difficulty: Medium
rating: 1377
source: Weekly Contest 128 Q2
tags:
    - Array
    - Hash Table
    - Counting
---

<!-- problem:start -->

# [1010. Pairs of Songs With Total Durations Divisible by 60](https://leetcode.com/problems/pairs-of-songs-with-total-durations-divisible-by-60)

[中文文档](/solution/1000-1099/1010.Pairs%20of%20Songs%20With%20Total%20Durations%20Divisible%20by%2060/README.md)

## Mô tả

<!-- description:start -->

<p>Cho danh sách bài hát, trong đó bài thứ <code>i</code> có thời lượng <code>time[i]</code> giây.</p>

<p>Hãy trả về số cặp bài hát có tổng thời lượng tính bằng giây chia hết cho <code>60</code>. Cụ thể, cần đếm số cặp chỉ số <code>i</code>, <code>j</code> thỏa mãn <code>i &lt; j</code> và <code>(time[i] + time[j]) % 60 == 0</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> time = [30,20,150,100,40]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Có ba cặp có tổng thời lượng chia hết cho 60:
(time[0] = 30, time[2] = 150): tổng thời lượng là 180
(time[1] = 20, time[3] = 100): tổng thời lượng là 120
(time[1] = 20, time[4] = 40): tổng thời lượng là 60
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> time = [60,60,60]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Cả ba cặp đều có tổng thời lượng bằng 120, chia hết cho 60.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= time.length &lt;= 6 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= time[i] &lt;= 500</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học + Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Kiểm tra mọi cặp để tìm tổng chia hết cho $60$ có độ phức tạp bậc hai theo $n\le 6\times 10^4$. Việc ghép cặp chỉ phụ thuộc vào số dư khi chia cho $60$.
>
> Nếu $x=a\bmod 60$, số dư cần ghép với nó là $y=(60-x)\bmod 60$. Đếm số lần số dư $y$ đã xuất hiện giúp ta cộng các cặp hợp lệ khi xử lý từng bài hát.
>
> Trong một lượt duyệt, ta tra cứu rồi cập nhật bảng kích thước $60$, nhờ đó một bài hát không bao giờ được ghép với chính nó. Thời gian chạy tuyến tính theo $n$.

<!-- thinking:end -->

Nếu tổng của một cặp $(a, b)$ chia hết cho $60$, tức $(a + b) \bmod 60 = 0$, thì $(a \bmod 60 + b \bmod 60) \bmod 60 = 0$. Đặt $x = a \bmod 60$ và $y = b \bmod 60$, ta có $(x + y) \bmod 60 = 0$, suy ra $y = (60 - x) \bmod 60$.

Vì vậy, ta duyệt danh sách bài hát và dùng mảng $cnt$ có độ dài $60$ để đếm số lần xuất hiện của mỗi số dư $x$. Với $x$ hiện tại, nếu mảng $cnt$ đã ghi nhận số dư $y = (60 - x) \bmod 60$, ta cộng $cnt[y]$ vào kết quả. Sau đó tăng số đếm của $x$ trong $cnt$ thêm $1$. Tiếp tục cho đến khi duyệt hết danh sách.

Sau khi duyệt xong, ta có số cặp bài hát thỏa mãn điều kiện.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(C)$, trong đó $n$ là độ dài danh sách bài hát, còn $C$ là số lượng số dư có thể có; ở đây $C = 60$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numPairsDivisibleBy60(self, time: List[int]) -> int:
        cnt = Counter()
        ans = 0
        for x in time:
            x %= 60
            y = (60 - x) % 60
            ans += cnt[y]
            cnt[x] += 1
        return ans
```

#### Java

```java
class Solution {
    public int numPairsDivisibleBy60(int[] time) {
        int[] cnt = new int[60];
        int ans = 0;
        for (int x : time) {
            x %= 60;
            int y = (60 - x) % 60;
            ans += cnt[y];
            ++cnt[x];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numPairsDivisibleBy60(vector<int>& time) {
        int cnt[60]{};
        int ans = 0;
        for (int x : time) {
            x %= 60;
            int y = (60 - x) % 60;
            ans += cnt[y];
            ++cnt[x];
        }
        return ans;
    }
};
```

#### Go

```go
func numPairsDivisibleBy60(time []int) (ans int) {
	cnt := [60]int{}
	for _, x := range time {
		x %= 60
		y := (60 - x) % 60
		ans += cnt[y]
		cnt[x]++
	}
	return
}
```

#### TypeScript

```ts
function numPairsDivisibleBy60(time: number[]): number {
    const cnt: number[] = new Array(60).fill(0);
    let ans: number = 0;
    for (let x of time) {
        x %= 60;
        const y = (60 - x) % 60;
        ans += cnt[y];
        ++cnt[x];
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn num_pairs_divisible_by60(time: Vec<i32>) -> i32 {
        let mut cnt = [0i32; 60];
        let mut ans: i32 = 0;
        for mut x in time {
            x %= 60;
            let y = (60 - x) % 60;
            ans += cnt[y as usize];
            cnt[x as usize] += 1;
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
