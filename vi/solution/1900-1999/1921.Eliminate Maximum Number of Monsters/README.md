---
comments: true
difficulty: Medium
rating: 1527
source: Weekly Contest 248 Q2
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [1921. Eliminate Maximum Number of Monsters](https://leetcode.com/problems/eliminate-maximum-number-of-monsters)

[中文文档](/solution/1900-1999/1921.Eliminate%20Maximum%20Number%20of%20Monsters/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn đang chơi một trò chơi điện tử, trong đó bạn phải bảo vệ thành phố khỏi một nhóm gồm <code>n</code> quái vật. Bạn được cho một mảng số nguyên <code>dist</code> <strong>được đánh chỉ số từ 0</strong> có kích thước <code>n</code>, trong đó <code>dist[i]</code> là <strong>khoảng cách ban đầu</strong> tính bằng kilômét của quái vật thứ <code>i<sup>th</sup></code> đến thành phố.</p>

<p>Các quái vật tiến về phía thành phố với <strong>tốc độ</strong> không đổi. Tốc độ của mỗi quái vật được cho trong một mảng số nguyên <code>speed</code> có kích thước <code>n</code>, trong đó <code>speed[i]</code> là tốc độ tính bằng kilômét mỗi phút của quái vật thứ <code>i<sup>th</sup></code>.</p>

<p>Bạn có một vũ khí, sau khi nạp đầy có thể tiêu diệt <strong>một</strong> quái vật. Tuy nhiên, vũ khí cần <strong>một phút</strong> để nạp. Vũ khí được nạp đầy ngay từ đầu.</p>

<p>Bạn thua khi bất kỳ quái vật nào đến thành phố. Nếu một quái vật đến thành phố đúng thời điểm vũ khí được nạp đầy, điều đó được tính là <strong>thua</strong>, và trò chơi kết thúc trước khi bạn có thể sử dụng vũ khí.</p>

<p>Hãy trả về <em>số lượng quái vật <strong>lớn nhất</strong> mà bạn có thể tiêu diệt trước khi thua, hoặc </em><code>n</code><em> nếu bạn có thể tiêu diệt tất cả quái vật trước khi chúng đến thành phố.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> dist = [1,3,4], speed = [1,1,1]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
Ban đầu, khoảng cách của các quái vật là [1,3,4]. Bạn tiêu diệt quái vật đầu tiên.
Sau một phút, khoảng cách của các quái vật là [X,2,3]. Bạn tiêu diệt quái vật thứ hai.
Sau một phút, khoảng cách của các quái vật là [X,X,2]. Bạn tiêu diệt quái vật thứ ba.
Cả 3 quái vật đều có thể bị tiêu diệt.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> dist = [1,1,2,3], speed = [1,1,1,1]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>
Ban đầu, khoảng cách của các quái vật là [1,1,2,3]. Bạn tiêu diệt quái vật đầu tiên.
Sau một phút, khoảng cách của các quái vật là [X,0,1,2], nên bạn thua.
Bạn chỉ có thể tiêu diệt 1 quái vật.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> dist = [3,2,4], speed = [5,3,2]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>
Ban đầu, khoảng cách của các quái vật là [3,2,4]. Bạn tiêu diệt quái vật đầu tiên.
Sau một phút, khoảng cách của các quái vật là [X,0,2], nên bạn thua.
Bạn chỉ có thể tiêu diệt 1 quái vật.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == dist.length == speed.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= dist[i], speed[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi phút chỉ bắn được một phát, nên thứ tự tiêu diệt rất quan trọng. Mỗi quái vật có một thời điểm cuối cùng có thể bị tiêu diệt, được xác định bởi khoảng cách và tốc độ.
>
> Sau khi sắp xếp các thời hạn đó, phát bắn thứ $i$ diễn ra ở phút $i$. Nếu thời hạn thứ $i$ nhỏ hơn $i$, quái vật đó (và mọi quái vật sau nó) không thể bị tiêu diệt.
>
> $\lfloor(d-1)/s\rfloor$ là phút cuối cùng trước khi quái vật đến thành phố; duyệt tuyến tính sau khi sắp xếp sẽ cho ra số lượng cần tìm.

<!-- thinking:end -->

Ta dùng mảng $\textit{times}$ để lưu thời điểm muộn nhất mà mỗi quái vật có thể bị tiêu diệt. Với quái vật thứ $i$, thời điểm muộn nhất mà nó có thể bị tiêu diệt là:

$$\textit{times}[i] = \left\lfloor \frac{\textit{dist}[i]-1}{\textit{speed}[i]} \right\rfloor$$

Tiếp theo, ta sắp xếp mảng $\textit{times}$ theo thứ tự tăng dần.

Sau đó, ta duyệt qua mảng $\textit{times}$. Với quái vật thứ $i$, nếu $\textit{times}[i] \geq i$, điều đó có nghĩa là quái vật thứ $i$ có thể bị tiêu diệt. Ngược lại, quái vật thứ $i$ không thể bị tiêu diệt, và ta trả về $i$ ngay lập tức.

Nếu có thể tiêu diệt tất cả quái vật, ta trả về $n$.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def eliminateMaximum(self, dist: List[int], speed: List[int]) -> int:
        times = sorted((d - 1) // s for d, s in zip(dist, speed))
        for i, t in enumerate(times):
            if t < i:
                return i
        return len(times)
```

#### Java

```java
class Solution {
    public int eliminateMaximum(int[] dist, int[] speed) {
        int n = dist.length;
        int[] times = new int[n];
        for (int i = 0; i < n; ++i) {
            times[i] = (dist[i] - 1) / speed[i];
        }
        Arrays.sort(times);
        for (int i = 0; i < n; ++i) {
            if (times[i] < i) {
                return i;
            }
        }
        return n;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int eliminateMaximum(vector<int>& dist, vector<int>& speed) {
        int n = dist.size();
        vector<int> times;
        for (int i = 0; i < n; ++i) {
            times.push_back((dist[i] - 1) / speed[i]);
        }
        sort(times.begin(), times.end());
        for (int i = 0; i < n; ++i) {
            if (times[i] < i) {
                return i;
            }
        }
        return n;
    }
};
```

#### Go

```go
func eliminateMaximum(dist []int, speed []int) int {
	n := len(dist)
	times := make([]int, n)
	for i, d := range dist {
		times[i] = (d - 1) / speed[i]
	}
	sort.Ints(times)
	for i, t := range times {
		if t < i {
			return i
		}
	}
	return n
}
```

#### TypeScript

```ts
function eliminateMaximum(dist: number[], speed: number[]): number {
    const n = dist.length;
    const times: number[] = Array(n).fill(0);
    for (let i = 0; i < n; ++i) {
        times[i] = Math.floor((dist[i] - 1) / speed[i]);
    }
    times.sort((a, b) => a - b);
    for (let i = 0; i < n; ++i) {
        if (times[i] < i) {
            return i;
        }
    }
    return n;
}
```

#### JavaScript

```js
/**
 * @param {number[]} dist
 * @param {number[]} speed
 * @return {number}
 */
var eliminateMaximum = function (dist, speed) {
    const n = dist.length;
    const times = Array(n).fill(0);
    for (let i = 0; i < n; ++i) {
        times[i] = Math.floor((dist[i] - 1) / speed[i]);
    }
    times.sort((a, b) => a - b);
    for (let i = 0; i < n; ++i) {
        if (times[i] < i) {
            return i;
        }
    }
    return n;
};
```

#### C#

```cs
public class Solution {
    public int EliminateMaximum(int[] dist, int[] speed) {
        int n = dist.Length;
        int[] times = new int[n];
        for (int i = 0; i < n; ++i) {
            times[i] = (dist[i] - 1) / speed[i];
        }
        Array.Sort(times);
        for (int i = 0; i < n; ++i) {
            if (times[i] < i) {
                return i;
            }
        }
        return n;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
