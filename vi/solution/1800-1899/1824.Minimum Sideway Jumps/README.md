---
comments: true
difficulty: Medium
rating: 1778
source: Weekly Contest 236 Q3
tags:
    - Greedy
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [1824. Minimum Sideway Jumps](https://leetcode.com/problems/minimum-sideway-jumps)

[中文文档](/solution/1800-1899/1824.Minimum%20Sideway%20Jumps/README.md)

## Mô tả

<!-- description:start -->

<p>Có một con đường <strong>3 làn</strong> dài <code>n</code>, gồm <code>n + 1</code> <strong>điểm</strong> được đánh số từ <code>0</code> đến <code>n</code>. Một con ếch <strong>bắt đầu</strong> tại điểm <code>0</code> ở <strong>làn thứ hai</strong> và<strong> </strong>muốn nhảy đến điểm <code>n</code>. Tuy nhiên, trên đường đi có thể có chướng ngại vật.</p>

<p>Cho mảng <code>obstacles</code> có độ dài <code>n + 1</code>, trong đó mỗi <code>obstacles[i]</code> (<strong>nằm trong khoảng từ 0 đến 3</strong>) mô tả một chướng ngại vật trên làn <code>obstacles[i]</code> tại điểm <code>i</code>. Nếu <code>obstacles[i] == 0</code>, tại điểm <code>i</code> không có chướng ngại vật. Tại mỗi điểm có <strong>nhiều nhất một</strong> chướng ngại vật trong 3 làn.</p>

<ul>
<li>Ví dụ, nếu <code>obstacles[2] == 1</code>, thì có chướng ngại vật trên làn 1 tại điểm 2.</li>
</ul>

<p>Ếch chỉ có thể đi từ điểm <code>i</code> đến điểm <code>i + 1</code> trên cùng làn nếu không có chướng ngại vật trên làn đó tại điểm <code>i + 1</code>. Để tránh chướng ngại vật, ếch cũng có thể thực hiện một <strong>cú nhảy ngang</strong> sang <strong>làn khác</strong> (kể cả làn không liền kề) tại <strong>cùng</strong> điểm đó nếu làn mới không có chướng ngại vật.</p>

<ul>
<li>Ví dụ, ếch có thể nhảy từ làn 3 tại điểm 3 sang làn 1 tại điểm 3.</li>
</ul>

<p>Trả về <em><strong>số cú nhảy ngang ít nhất</strong> mà ếch cần để đến <strong>bất kỳ làn nào</strong> tại điểm n, bắt đầu từ làn <code>2</code> tại điểm 0</em>.</p>

<p><strong>Lưu ý:</strong> Không có chướng ngại vật tại các điểm <code>0</code> và <code>n</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1800-1899/1824.Minimum%20Sideway%20Jumps/images/ic234-q3-ex1.png" style="width: 500px; height: 244px;" />
<pre>
<strong>Đầu vào:</strong> obstacles = [0,1,2,3,0]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Lời giải tối ưu được biểu diễn bằng các mũi tên ở trên. Có 2 cú nhảy ngang (mũi tên đỏ).
Lưu ý rằng ếch chỉ có thể vượt qua chướng ngại vật khi thực hiện cú nhảy ngang (như tại điểm 2).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1800-1899/1824.Minimum%20Sideway%20Jumps/images/ic234-q3-ex2.png" style="width: 500px; height: 196px;" />
<pre>
<strong>Đầu vào:</strong> obstacles = [0,1,1,3,3,0]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có chướng ngại vật trên làn 2. Không cần cú nhảy ngang nào.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1800-1899/1824.Minimum%20Sideway%20Jumps/images/ic234-q3-ex3.png" style="width: 500px; height: 196px;" />
<pre>
<strong>Đầu vào:</strong> obstacles = [0,2,1,0,3,0]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Lời giải tối ưu được biểu diễn bằng các mũi tên ở trên. Có 2 cú nhảy ngang.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>obstacles.length == n + 1</code></li>
	<li><code>1 &lt;= n &lt;= 5 * 10<sup>5</sup></code></li>
	<li><code>0 &lt;= obstacles[i] &lt;= 3</code></li>
	<li><code>obstacles[0] == obstacles[n] == 0</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Có ba làn với các chướng ngại vật và ta chỉ được nhảy ngang tại cùng một điểm. Tìm mọi đường đi mà không ghi nhớ sẽ lặp lại các trạng thái, tổng cộng $O(n)$ trạng thái.
>
> Gọi $f[j]$ là số cú nhảy ngang ít nhất để ở làn $j$ tại điểm hiện tại. Làn bị chặn được gán vô cực; nếu không, ta có thể giữ nguyên làn hoặc nhảy từ làn có chi phí nhỏ nhất tại điểm đó. Một mảng độ dài $3$ được cập nhật lần lượt từ đầu đến cuối.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ là số bước nhảy ngang ít nhất để ếch đến điểm thứ $i$ và ở làn thứ $j$ (chỉ số bắt đầu từ $0$).

Lưu ý rằng ếch bắt đầu ở làn thứ hai (chỉ số trong đề bắt đầu từ $1$), nên $f[0][1]$ bằng $0$, còn $f[0][0]$ và $f[0][2]$ đều bằng $1$. Đáp án là $min(f[n][0], f[n][1], f[n][2])$.

Với mỗi vị trí $i$ từ $1$ đến $n$, ta có thể duyệt qua làn hiện tại $j$ của ếch. Nếu $obstacles[i] = j + 1$, nghĩa là có chướng ngại vật trên làn thứ $j$, và giá trị của $f[i][j]$ là vô cực. Nếu không, ếch có thể không nhảy, khi đó giá trị của $f[i][j]$ bằng $f[i - 1][j]$, hoặc nhảy ngang từ làn khác, khi đó $f[i][j] = min(f[i][j], min(f[i][0], f[i][1], f[i][2]) + 1)$.

Trong cài đặt, ta có thể tối ưu chiều thứ nhất của không gian và chỉ cần duy trì một mảng $f$ độ dài $3$.

Độ phức tạp thời gian là $O(n)$, với $n$ là độ dài của mảng $obstacles$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minSideJumps(self, obstacles: List[int]) -> int:
        f = [1, 0, 1]
        for v in obstacles[1:]:
            for j in range(3):
                if v == j + 1:
                    f[j] = inf
                    break
            x = min(f) + 1
            for j in range(3):
                if v != j + 1:
                    f[j] = min(f[j], x)
        return min(f)
```

#### Java

```java
class Solution {
    public int minSideJumps(int[] obstacles) {
        final int inf = 1 << 30;
        int[] f = {1, 0, 1};
        for (int i = 1; i < obstacles.length; ++i) {
            for (int j = 0; j < 3; ++j) {
                if (obstacles[i] == j + 1) {
                    f[j] = inf;
                    break;
                }
            }
            int x = Math.min(f[0], Math.min(f[1], f[2])) + 1;
            for (int j = 0; j < 3; ++j) {
                if (obstacles[i] != j + 1) {
                    f[j] = Math.min(f[j], x);
                }
            }
        }
        return Math.min(f[0], Math.min(f[1], f[2]));
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minSideJumps(vector<int>& obstacles) {
        const int inf = 1 << 30;
        int f[3] = {1, 0, 1};
        for (int i = 1; i < obstacles.size(); ++i) {
            for (int j = 0; j < 3; ++j) {
                if (obstacles[i] == j + 1) {
                    f[j] = inf;
                    break;
                }
            }
            int x = min({f[0], f[1], f[2]}) + 1;
            for (int j = 0; j < 3; ++j) {
                if (obstacles[i] != j + 1) {
                    f[j] = min(f[j], x);
                }
            }
        }
        return min({f[0], f[1], f[2]});
    }
};
```

#### Go

```go
func minSideJumps(obstacles []int) int {
	f := [3]int{1, 0, 1}
	const inf = 1 << 30
	for _, v := range obstacles[1:] {
		for j := 0; j < 3; j++ {
			if v == j+1 {
				f[j] = inf
				break
			}
		}
		x := min(f[0], min(f[1], f[2])) + 1
		for j := 0; j < 3; j++ {
			if v != j+1 {
				f[j] = min(f[j], x)
			}
		}
	}
	return min(f[0], min(f[1], f[2]))
}
```

#### TypeScript

```ts
function minSideJumps(obstacles: number[]): number {
    const inf = 1 << 30;
    const f = [1, 0, 1];
    for (let i = 1; i < obstacles.length; ++i) {
        for (let j = 0; j < 3; ++j) {
            if (obstacles[i] == j + 1) {
                f[j] = inf;
                break;
            }
        }
        const x = Math.min(...f) + 1;
        for (let j = 0; j < 3; ++j) {
            if (obstacles[i] != j + 1) {
                f[j] = Math.min(f[j], x);
            }
        }
    }
    return Math.min(...f);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
