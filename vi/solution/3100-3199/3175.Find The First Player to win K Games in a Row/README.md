---
comments: true
difficulty: Medium
rating: 1488
source: Biweekly Contest 132 Q2
tags:
    - Array
    - Simulation
---

<!-- problem:start -->

# [3175. Find The First Player to win K Games in a Row](https://leetcode.com/problems/find-the-first-player-to-win-k-games-in-a-row)

[Tài liệu tiếng Trung](/solution/3100-3199/3175.Find%20The%20First%20Player%20to%20win%20K%20Games%20in%20a%20Row/README.md)

## Mô tả

<!-- description:start -->

<p>Một cuộc thi gồm <code>n</code> người chơi được đánh số từ <code>0</code> đến <code>n - 1</code>.</p>

<p>Bạn được cho một mảng số nguyên <code>skills</code> có kích thước <code>n</code> và một số nguyên <strong>dương</strong> <code>k</code>, trong đó <code>skills[i]</code> là mức kỹ năng của người chơi <code>i</code>. Tất cả các số nguyên trong <code>skills</code> đều <strong>khác nhau</strong>.</p>

<p>Tất cả người chơi đứng trong một hàng đợi theo thứ tự từ người chơi <code>0</code> đến người chơi <code>n - 1</code>.</p>

<p>Quá trình thi đấu diễn ra như sau:</p>

<ul>
	<li>Hai người chơi đầu tiên trong hàng đợi thi đấu một trận, và người có mức kỹ năng <strong>cao hơn</strong> thắng.</li>
	<li>Sau trận đấu, người thắng vẫn ở đầu hàng đợi, còn người thua đi xuống cuối hàng đợi.</li>
</ul>

<p>Người thắng cuộc là người chơi <strong>đầu tiên</strong> thắng <code>k</code> trận <strong>liên tiếp</strong>.</p>

<p>Trả về chỉ số ban đầu của người chơi <em>chiến thắng</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">skills = [4,2,6,3,9], k = 2</span></p>

<p><strong>Đầu ra:</strong> 2</p>

<p><strong>Giải thích:</strong></p>

<p>Ban đầu, hàng đợi người chơi là <code>[0,1,2,3,4]</code>. Quá trình diễn ra như sau:</p>

<ul>
	<li>Người chơi 0 và 1 thi đấu. Vì kỹ năng của người chơi 0 cao hơn người chơi 1 nên người chơi 0 thắng. Hàng đợi trở thành <code>[0,2,3,4,1]</code>.</li>
	<li>Người chơi 0 và 2 thi đấu. Vì kỹ năng của người chơi 2 cao hơn người chơi 0 nên người chơi 2 thắng. Hàng đợi trở thành <code>[2,3,4,1,0]</code>.</li>
	<li>Người chơi 2 và 3 thi đấu. Vì kỹ năng của người chơi 2 cao hơn người chơi 3 nên người chơi 2 thắng. Hàng đợi trở thành <code>[2,4,1,0,3]</code>.</li>
</ul>

<p>Người chơi 2 đã thắng <code>k = 2</code> trận liên tiếp, nên người thắng là người chơi 2.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">skills = [2,5,4], k = 3</span></p>

<p><strong>Đầu ra:</strong> 1</p>

<p><strong>Giải thích:</strong></p>

<p>Ban đầu, hàng đợi người chơi là <code>[0,1,2]</code>. Quá trình diễn ra như sau:</p>

<ul>
	<li>Người chơi 0 và 1 thi đấu. Vì kỹ năng của người chơi 1 cao hơn người chơi 0 nên người chơi 1 thắng. Hàng đợi trở thành <code>[1,2,0]</code>.</li>
	<li>Người chơi 1 và 2 thi đấu. Vì kỹ năng của người chơi 1 cao hơn người chơi 2 nên người chơi 1 thắng. Hàng đợi trở thành <code>[1,0,2]</code>.</li>
	<li>Người chơi 1 và 0 thi đấu. Vì kỹ năng của người chơi 1 cao hơn người chơi 0 nên người chơi 1 thắng. Hàng đợi trở thành <code>[1,2,0]</code>.</li>
</ul>

<p>Người chơi 1 đã thắng <code>k = 3</code> trận liên tiếp, nên người thắng là người chơi 1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == skills.length</code></li>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= skills[i] &lt;= 10<sup>6</sup></code></li>
	<li>Tất cả các số nguyên trong <code>skills</code> đều khác nhau.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Suy luận nhanh

<!-- thinking:start -->

> **Tư duy**
>
> Hai người chơi ở đầu hàng đợi so tài, người thua đi xuống cuối hàng. Mô phỏng trực tiếp có thể cần $O(n+k)$ bước khi $k$ rất lớn.
>
> Một người chơi đã thua sẽ không quay lại trước khi gặp một người chơi mạnh hơn chưa xuất hiện. Sau $n-1$ chiến thắng liên tiếp, người chơi hiện tại là người có kỹ năng cao nhất toàn cục, vì vậy có thể giới hạn $k$ ở $n-1$.
>
> Duy trì chỉ số của nhà vô địch $i$ và số trận thắng liên tiếp $cnt$, đặt lại khi gặp đối thủ mạnh hơn. Dừng khi $cnt=k$ hoặc đã duyệt hết mảng.

<!-- thinking:end -->

Ta nhận thấy mỗi khi hai phần tử đầu tiên của mảng được so sánh, bất kể kết quả thế nào, lần so sánh tiếp theo luôn là giữa phần tử tiếp theo trong mảng và người thắng hiện tại. Vì vậy, nếu lặp $n-1$ lần, người thắng cuối cùng chắc chắn là phần tử lớn nhất trong mảng. Nếu không, phần tử đã thắng liên tiếp $k$ lần chính là người thắng cuối cùng.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

Bài toán tương tự:

- [1535. Find the Winner of an Array Game](https://github.com/doocs/leetcode/blob/main/solution/1500-1599/1535.Find%20the%20Winner%20of%20an%20Array%20Game/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findWinningPlayer(self, skills: List[int], k: int) -> int:
        n = len(skills)
        k = min(k, n - 1)
        i = cnt = 0
        for j in range(1, n):
            if skills[i] < skills[j]:
                i = j
                cnt = 1
            else:
                cnt += 1
            if cnt == k:
                break
        return i
```

#### Java

```java
class Solution {
    public int findWinningPlayer(int[] skills, int k) {
        int n = skills.length;
        k = Math.min(k, n - 1);
        int i = 0, cnt = 0;
        for (int j = 1; j < n; ++j) {
            if (skills[i] < skills[j]) {
                i = j;
                cnt = 1;
            } else {
                ++cnt;
            }
            if (cnt == k) {
                break;
            }
        }
        return i;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findWinningPlayer(vector<int>& skills, int k) {
        int n = skills.size();
        k = min(k, n - 1);
        int i = 0, cnt = 0;
        for (int j = 1; j < n; ++j) {
            if (skills[i] < skills[j]) {
                i = j;
                cnt = 1;
            } else {
                ++cnt;
            }
            if (cnt == k) {
                break;
            }
        }
        return i;
    }
};
```

#### Go

```go
func findWinningPlayer(skills []int, k int) int {
	n := len(skills)
	k = min(k, n-1)
	i, cnt := 0, 0
	for j := 1; j < n; j++ {
		if skills[i] < skills[j] {
			i = j
			cnt = 1
		} else {
			cnt++
		}
		if cnt == k {
			break
		}
	}
	return i
}
```

#### TypeScript

```ts
function findWinningPlayer(skills: number[], k: number): number {
    const n = skills.length;
    k = Math.min(k, n - 1);
    let [i, cnt] = [0, 0];
    for (let j = 1; j < n; ++j) {
        if (skills[i] < skills[j]) {
            i = j;
            cnt = 1;
        } else {
            ++cnt;
        }
        if (cnt === k) {
            break;
        }
    }
    return i;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
