---
comments: true
difficulty: Medium
rating: 1433
source: Weekly Contest 200 Q2
tags:
    - Array
    - Simulation
---

<!-- problem:start -->

# [1535. Find the Winner of an Array Game](https://leetcode.com/problems/find-the-winner-of-an-array-game)

[中文文档](/solution/1500-1599/1535.Find%20the%20Winner%20of%20an%20Array%20Game/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>arr</code> gồm các số nguyên <strong>khác nhau</strong> và một số nguyên <code>k</code>.</p>

<p>A game will be played between the first two elements of the array (i.e. <code>arr[0]</code> and <code>arr[1]</code>). In each round of the game, we compare <code>arr[0]</code> with <code>arr[1]</code>, the larger integer wins and remains at position <code>0</code>, and the smaller integer moves to the end of the array. The game ends when an integer wins <code>k</code> consecutive rounds.</p>

<p>Trả về <em>số nguyên sẽ thắng trò chơi</em>.</p>

<p><strong>Đảm bảo</strong> trò chơi sẽ có người thắng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [2,1,3,5,4,6,7], k = 2
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Hãy xem các vòng của trò chơi:
Round |       arr       | winner | win_count
  1   | [2,1,3,5,4,6,7] | 2      | 1
  2   | [2,3,5,4,6,7,1] | 3      | 1
  3   | [3,5,4,6,7,1,2] | 5      | 1
  4   | [5,4,6,7,1,2,3] | 5      | 2
So we can see that 4 rounds will be played and 5 is the winner because it wins 2 consecutive games.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [3,2,1], k = 10
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> 3 sẽ thắng liên tiếp 10 vòng đầu tiên.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= arr.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= arr[i] &lt;= 10<sup>6</sup></code></li>
	<li><code>arr</code> contains <strong>distinct</strong> integers.</li>
	<li><code>1 &lt;= k &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Suy luận nhanh

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi vòng so sánh hai giá trị đầu; giá trị lớn hơn thắng và trở lại đầu hàng. $k$ có thể bằng $10^9$, nên ta không thể mô phỏng từng ấy vòng.
>
> Dù ai thắng, trận tiếp theo luôn là giữa nhà vô địch hiện tại và phần tử chưa xét tiếp theo. Giá trị đầu tiên thắng liên tiếp $k$ lần là đáp án; nếu duyệt hết mà chưa có chuỗi thắng đó, người thắng phải là giá trị lớn nhất toàn cục, vì sau đó không thể thua.

<!-- thinking:end -->

Ta nhận thấy mỗi khi hai phần tử đầu mảng được so sánh, bất kể kết quả thế nào, lần so sánh tiếp theo luôn là giữa phần tử tiếp theo trong mảng và người thắng hiện tại. Vì vậy, nếu lặp $n-1$ lần, người thắng cuối cùng chắc chắn là phần tử lớn nhất trong mảng. Nếu không, phần tử đã thắng liên tiếp $k$ lần chính là người thắng cuối cùng.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng. Độ phức tạp không gian là $O(1)$.

Similar problems:

- [1535. Find the Winner of an Array Game](https://github.com/doocs/leetcode/blob/main/solution/3100-3199/3175.Find%20The%20First%20Player%20to%20win%20K%20Games%20in%20a%20Row/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getWinner(self, arr: List[int], k: int) -> int:
        mx = arr[0]
        cnt = 0
        for x in arr[1:]:
            if mx < x:
                mx = x
                cnt = 1
            else:
                cnt += 1
            if cnt == k:
                break
        return mx
```

#### Java

```java
class Solution {
    public int getWinner(int[] arr, int k) {
        int mx = arr[0];
        for (int i = 1, cnt = 0; i < arr.length; ++i) {
            if (mx < arr[i]) {
                mx = arr[i];
                cnt = 1;
            } else {
                ++cnt;
            }
            if (cnt == k) {
                break;
            }
        }
        return mx;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int getWinner(vector<int>& arr, int k) {
        int mx = arr[0];
        for (int i = 1, cnt = 0; i < arr.size(); ++i) {
            if (mx < arr[i]) {
                mx = arr[i];
                cnt = 1;
            } else {
                ++cnt;
            }
            if (cnt == k) {
                break;
            }
        }
        return mx;
    }
};
```

#### Go

```go
func getWinner(arr []int, k int) int {
	mx, cnt := arr[0], 0
	for _, x := range arr[1:] {
		if mx < x {
			mx = x
			cnt = 1
		} else {
			cnt++
		}
		if cnt == k {
			break
		}
	}
	return mx
}
```

#### TypeScript

```ts
function getWinner(arr: number[], k: number): number {
    let mx = arr[0];
    let cnt = 0;
    for (const x of arr.slice(1)) {
        if (mx < x) {
            mx = x;
            cnt = 1;
        } else {
            ++cnt;
        }
        if (cnt === k) {
            break;
        }
    }
    return mx;
}
```

#### C#

```cs
public class Solution {
    public int GetWinner(int[] arr, int k) {
        int maxElement = arr[0], count = 0;
        for (int i = 1; i < arr.Length; i++) {
            if (maxElement < arr[i]) {
                maxElement = arr[i];
                count = 1;
            } else {
                count++;
            }
            if (count == k) {
                break;
            }
        }
        return maxElement;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
