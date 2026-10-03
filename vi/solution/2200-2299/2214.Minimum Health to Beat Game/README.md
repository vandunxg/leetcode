---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
---

<!-- problem:start -->

# [2214. Minimum Health to Beat Game 🔒](https://leetcode.com/problems/minimum-health-to-beat-game)

[中文文档](/solution/2200-2299/2214.Minimum%20Health%20to%20Beat%20Game/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn đang chơi một trò chơi gồm <code>n</code> màn, được đánh số từ <code>0</code> đến <code>n - 1</code>. Bạn được cho một mảng số nguyên <code>damage</code> <strong>đánh chỉ số từ 0</strong>, trong đó <code>damage[i]</code> là lượng máu bạn mất để hoàn thành màn thứ <code>i<sup>th</sup></code>.</p>

<p>Bạn cũng được cho một số nguyên <code>armor</code>. Bạn có thể sử dụng khả năng của giáp <strong>nhiều nhất một lần</strong> trong trò chơi, ở <strong>bất kỳ</strong> màn nào, để giảm <strong>nhiều nhất</strong> <code>armor</code> sát thương.</p>

<p>Bạn phải hoàn thành các màn theo thứ tự và máu phải <strong>luôn lớn hơn</strong> <code>0</code> để vượt qua trò chơi.</p>

<p>Hãy trả về <em>lượng máu <strong>nhỏ nhất</strong> bạn cần có lúc bắt đầu để vượt qua trò chơi.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> damage = [2,7,4,3], armor = 4
<strong>Đầu ra:</strong> 13
<strong>Giải thích:</strong> Một cách tối ưu để vượt qua trò chơi khi bắt đầu với 13 máu là:
Ở vòng 1, nhận 2 sát thương. Bạn còn 13 - 2 = 11 máu.
Ở vòng 2, nhận 7 sát thương. Bạn còn 11 - 7 = 4 máu.
Ở vòng 3, dùng giáp để chặn 4 sát thương. Bạn còn 4 - 0 = 4 máu.
Ở vòng 4, nhận 3 sát thương. Bạn còn 4 - 3 = 1 máu.
Lưu ý rằng 13 là lượng máu nhỏ nhất bạn cần có lúc bắt đầu để vượt qua trò chơi.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> damage = [2,5,3,4], armor = 7
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong> Một cách tối ưu để vượt qua trò chơi khi bắt đầu với 10 máu là:
Ở vòng 1, nhận 2 sát thương. Bạn còn 10 - 2 = 8 máu.
Ở vòng 2, dùng giáp để chặn 5 sát thương. Bạn còn 8 - 0 = 8 máu.
Ở vòng 3, nhận 3 sát thương. Bạn còn 8 - 3 = 5 máu.
Ở vòng 4, nhận 4 sát thương. Bạn còn 5 - 4 = 1 máu.
Lưu ý rằng 10 là lượng máu nhỏ nhất bạn cần có lúc bắt đầu để vượt qua trò chơi.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> damage = [3,3,3], armor = 0
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong> Một cách tối ưu để vượt qua trò chơi khi bắt đầu với 10 máu là:
Ở vòng 1, nhận 3 sát thương. Bạn còn 10 - 3 = 7 máu.
Ở vòng 2, nhận 3 sát thương. Bạn còn 7 - 3 = 4 máu.
Ở vòng 3, nhận 3 sát thương. Bạn còn 4 - 3 = 1 máu.
Lưu ý rằng bạn không sử dụng khả năng của giáp.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == damage.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= damage[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= armor &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Sát thương nhận theo thứ tự và máu phải luôn dương sau mỗi vòng. Giáp có thể dùng một lần, thay sát thương của vòng đó bằng $\max(0, d-\textit{armor})$. Thử mọi vòng có độ phức tạp $O(n)$ và vẫn đạt yêu cầu, nhưng lựa chọn này thực ra chỉ cần một phép so sánh.
>
> Không dùng giáp, lượng máu cần có là tổng sát thương cộng một. Giáp không thể chặn nhiều hơn giá trị của chính nó hoặc sát thương của vòng được chọn, nên lượng sát thương giảm được là $\min(\max(\textit{damage}), \textit{armor})$. Hãy dùng giáp cho đòn đánh mạnh nhất.

<!-- thinking:end -->

Ta có thể tham lam bằng cách chọn dùng khả năng của giáp ở vòng có sát thương lớn nhất. Gọi sát thương lớn nhất là $\textit{mx}$, khi đó ta có thể tránh $\min(\textit{mx}, \textit{armor})$ sát thương. Vì vậy, lượng máu nhỏ nhất cần có là $\sum(\textit{damage}) - \min(\textit{mx}, \textit{armor}) + 1$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{damage}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumHealth(self, damage: List[int], armor: int) -> int:
        return sum(damage) - min(max(damage), armor) + 1
```

#### Java

```java
class Solution {
    public long minimumHealth(int[] damage, int armor) {
        long s = 0;
        int mx = damage[0];
        for (int v : damage) {
            s += v;
            mx = Math.max(mx, v);
        }
        return s - Math.min(mx, armor) + 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minimumHealth(vector<int>& damage, int armor) {
        long long s = 0;
        int mx = damage[0];
        for (int& v : damage) {
            s += v;
            mx = max(mx, v);
        }
        return s - min(mx, armor) + 1;
    }
};
```

#### Go

```go
func minimumHealth(damage []int, armor int) int64 {
	var s int64
	var mx int
	for _, v := range damage {
		s += int64(v)
		mx = max(mx, v)
	}
	return s - int64(min(mx, armor)) + 1
}
```

#### TypeScript

```ts
function minimumHealth(damage: number[], armor: number): number {
    let s = 0;
    let mx = 0;
    for (const v of damage) {
        mx = Math.max(mx, v);
        s += v;
    }
    return s - Math.min(mx, armor) + 1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
