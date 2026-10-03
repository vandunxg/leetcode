---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - Two Pointers
---

<!-- problem:start -->

# [1989. Maximum Number of People That Can Be Caught in Tag 🔒](https://leetcode.com/problems/maximum-number-of-people-that-can-be-caught-in-tag)

[中文文档](/solution/1900-1999/1989.Maximum%20Number%20of%20People%20That%20Can%20Be%20Caught%20in%20Tag/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn đang chơi trò đuổi bắt với bạn bè. Trong trò chơi này, mọi người được chia thành hai đội: những người là &quot;người đi bắt&quot; và những người không phải là &quot;người đi bắt&quot;. Những người là &quot;người đi bắt&quot; muốn bắt càng nhiều người không phải là &quot;người đi bắt&quot; càng tốt.</p>

<p>Cho một mảng số nguyên <code>team</code> <strong>được đánh chỉ số từ 0</strong>, chỉ chứa số 0 (biểu thị những người <strong>không phải</strong> là &quot;người đi bắt&quot;) và số 1 (biểu thị những người là &quot;người đi bắt&quot;), cùng một số nguyên <code>dist</code>. Một người là &quot;người đi bắt&quot; ở chỉ số <code>i</code> có thể bắt <strong>một</strong> người bất kỳ có chỉ số nằm trong đoạn <code>[i - dist, i + dist]</code> (<strong>bao gồm hai đầu mút</strong>) và người đó <strong>không phải</strong> là &quot;người đi bắt&quot;.</p>

<p>Hãy trả về <em>số lượng <strong>lớn nhất</strong> người mà những người là &quot;người đi bắt&quot; có thể bắt được</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> team = [0,1,0,1,0], dist = 3
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Người là &quot;người đi bắt&quot; ở chỉ số 1 có thể bắt những người trong đoạn [i-dist, i+dist] = [1-3, 1+3] = [-2, 4].
Họ có thể bắt người không phải là &quot;người đi bắt&quot; ở chỉ số 2.
Người là &quot;người đi bắt&quot; ở chỉ số 3 có thể bắt những người trong đoạn [i-dist, i+dist] = [3-3, 3+3] = [0, 6].
Họ có thể bắt người không phải là &quot;người đi bắt&quot; ở chỉ số 0.
Người không phải là &quot;người đi bắt&quot; ở chỉ số 4 sẽ không bị bắt vì những người ở chỉ số 1 và 3 đã bắt đủ một người.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> team = [1], dist = 1
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>
Không có người nào không phải là &quot;người đi bắt&quot; để bắt.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> team = [0], dist = 1
<strong>Đầu ra:</strong> 0
<strong>Giải thích:
</strong>Không có người nào là &quot;người đi bắt&quot; để bắt người khác.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= team.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= team[i] &lt;= 1</code></li>
	<li><code>1 &lt;= dist &lt;= team.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Two Pointers

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi người đi bắt chỉ bắt được nhiều nhất một người chưa bị bắt trong phạm vi $\textit{dist}$. Ghép bằng hai con trỏ từ trái sang phải là tối ưu.
>
> $i$ duyệt những người đi bắt còn $j$ duyệt người tiếp theo có thể bắt, tăng $j$ khi đó là người đi bắt hoặc ở quá xa về bên trái. Khi tìm được một cặp hợp lệ, ta tăng đáp án.
>
> Mỗi chỉ số được xét đúng một lần.

<!-- thinking:end -->

Ta có thể dùng hai con trỏ $i$ và $j$ lần lượt trỏ đến người đi bắt và người không phải là người đi bắt, ban đầu $i=0$, $j=0$.

Sau đó, ta duyệt mảng từ trái sang phải. Khi gặp một người đi bắt, tức là $team[i]=1$, nếu $j \lt n$ và $\textit{team}[j]=1$ hoặc $i - j \gt \textit{dist}$, ta lặp để di chuyển con trỏ $j$ sang phải. Điều này có nghĩa là ta cần tìm người đầu tiên không phải là người đi bắt sao cho khoảng cách giữa $i$ và $j$ không vượt quá $\textit{dist}$. Nếu tìm thấy người đó, ta di chuyển con trỏ $j$ sang phải một bước, cho biết người này đã bị bắt, đồng thời tăng đáp án lên một. Tiếp tục duyệt cho đến khi xử lý hết mảng.

Cuối cùng, trả về đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{team}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def catchMaximumAmountofPeople(self, team: List[int], dist: int) -> int:
        ans = j = 0
        n = len(team)
        for i, x in enumerate(team):
            if x:
                while j < n and (team[j] or i - j > dist):
                    j += 1
                if j < n and abs(i - j) <= dist:
                    ans += 1
                    j += 1
        return ans
```

#### Java

```java
class Solution {
    public int catchMaximumAmountofPeople(int[] team, int dist) {
        int ans = 0;
        int n = team.length;
        for (int i = 0, j = 0; i < n; ++i) {
            if (team[i] == 1) {
                while (j < n && (team[j] == 1 || i - j > dist)) {
                    ++j;
                }
                if (j < n && Math.abs(i - j) <= dist) {
                    ++ans;
                    ++j;
                }
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
    int catchMaximumAmountofPeople(vector<int>& team, int dist) {
        int ans = 0;
        int n = team.size();
        for (int i = 0, j = 0; i < n; ++i) {
            if (team[i]) {
                while (j < n && (team[j] || i - j > dist)) {
                    ++j;
                }
                if (j < n && abs(i - j) <= dist) {
                    ++ans;
                    ++j;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func catchMaximumAmountofPeople(team []int, dist int) (ans int) {
	n := len(team)
	for i, j := 0, 0; i < n; i++ {
		if team[i] == 1 {
			for j < n && (team[j] == 1 || i-j > dist) {
				j++
			}
			if j < n && abs(i-j) <= dist {
				ans++
				j++
			}
		}
	}
	return
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
