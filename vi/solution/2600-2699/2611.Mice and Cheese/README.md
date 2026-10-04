---
comments: true
difficulty: Medium
rating: 1663
source: Weekly Contest 339 Q3
tags:
    - Greedy
    - Array
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2611. Mice and Cheese](https://leetcode.com/problems/mice-and-cheese)

[中文文档](/solution/2600-2699/2611.Mice%20and%20Cheese/README.md)

## Mô tả

<!-- description:start -->

<p>Có hai con chuột và <code>n</code> loại phô mai khác nhau, mỗi loại phô mai phải được ăn bởi đúng một con chuột.</p>

<p>Điểm của loại phô mai có chỉ số <code>i</code> (đánh chỉ số từ <strong>0</strong>) là:</p>

<ul>
	<li><code>reward1[i]</code> nếu chuột thứ nhất ăn.</li>
	<li><code>reward2[i]</code> nếu chuột thứ hai ăn.</li>
</ul>

<p>Cho hai mảng số nguyên dương <code>reward1</code>, <code>reward2</code> và một số nguyên không âm <code>k</code>.</p>

<p>Hãy trả về <em><strong>số điểm lớn nhất</strong> mà hai con chuột có thể đạt được nếu chuột thứ nhất ăn chính xác </em><code>k</code><em> loại phô mai.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> reward1 = [1,1,3,4], reward2 = [4,4,1,1], k = 2
<strong>Đầu ra:</strong> 15
<strong>Giải thích:</strong> Trong ví dụ này, chuột thứ nhất ăn loại phô mai thứ 2<sup>nd</sup> (đánh chỉ số từ 0) và thứ 3<sup>rd</sup>, còn chuột thứ hai ăn loại thứ 0<sup>th</sup> và thứ 1<sup>st</sup>.
Tổng số điểm là 4 + 4 + 3 + 4 = 15.
Có thể chứng minh rằng 15 là tổng số điểm lớn nhất mà hai con chuột có thể đạt được.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> reward1 = [1,1], reward2 = [1,1], k = 2
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Trong ví dụ này, chuột thứ nhất ăn loại phô mai thứ 0<sup>th</sup> (đánh chỉ số từ 0) và thứ 1<sup>st</sup>, còn chuột thứ hai không ăn loại phô mai nào.
Tổng số điểm là 1 + 1 = 2.
Có thể chứng minh rằng 2 là tổng số điểm lớn nhất mà hai con chuột có thể đạt được.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == reward1.length == reward2.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= reward1[i],&nbsp;reward2[i] &lt;= 1000</code></li>
	<li><code>0 &lt;= k &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Chuột thứ nhất phải ăn chính xác $k$ miếng phô mai. Việc chọn một tập con gồm $k$ phần tử là không khả thi khi $n \le 10^5$.
>
> Ban đầu, cho chuột thứ hai ăn tất cả các miếng phô mai, sau đó chuyển $k$ miếng sang cho chuột thứ nhất; độ thay đổi là $reward1[i]-reward2[i]$. Nên chuyển những miếng có độ thay đổi lớn hơn.
>
> Sắp xếp các chỉ số theo độ chênh lệch đó theo thứ tự giảm dần; $k$ chỉ số đầu tiên dùng $reward1$, các chỉ số còn lại dùng $reward2$.

<!-- thinking:end -->

Trước tiên, ta có thể cho chuột thứ hai ăn tất cả phô mai. Sau đó, xét việc đưa $k$ miếng phô mai cho chuột thứ nhất. Ta nên chọn $k$ miếng phô mai này như thế nào? Rõ ràng, nếu chuyển miếng phô mai thứ $i$ từ chuột thứ hai sang chuột thứ nhất, số điểm thay đổi là $reward1[i] - reward2[i]$. Ta muốn độ thay đổi này lớn nhất có thể để tổng số điểm được tối đa hóa.

Do đó, ta sắp xếp phô mai theo thứ tự giảm dần của `reward1[i] - reward2[i]`. $k$ miếng phô mai đầu tiên sẽ do chuột thứ nhất ăn, còn số phô mai còn lại sẽ do chuột thứ hai ăn để đạt số điểm lớn nhất.

Độ phức tạp thời gian là $O(n \times \log n)$, độ phức tạp không gian là $O(n)$. Trong đó $n$ là số loại phô mai.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def miceAndCheese(self, reward1: List[int], reward2: List[int], k: int) -> int:
        n = len(reward1)
        idx = sorted(range(n), key=lambda i: reward1[i] - reward2[i], reverse=True)
        return sum(reward1[i] for i in idx[:k]) + sum(reward2[i] for i in idx[k:])
```

#### Java

```java
class Solution {
    public int miceAndCheese(int[] reward1, int[] reward2, int k) {
        int n = reward1.length;
        Integer[] idx = new Integer[n];
        Arrays.setAll(idx, i -> i);
        Arrays.sort(idx, (i, j) -> reward1[j] - reward2[j] - (reward1[i] - reward2[i]));
        int ans = 0;
        for (int i = 0; i < k; ++i) {
            ans += reward1[idx[i]];
        }
        for (int i = k; i < n; ++i) {
            ans += reward2[idx[i]];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int miceAndCheese(vector<int>& reward1, vector<int>& reward2, int k) {
        int n = reward1.size();
        vector<int> idx(n);
        iota(idx.begin(), idx.end(), 0);
        sort(idx.begin(), idx.end(), [&](int i, int j) { return reward1[j] - reward2[j] < reward1[i] - reward2[i]; });
        int ans = 0;
        for (int i = 0; i < k; ++i) {
            ans += reward1[idx[i]];
        }
        for (int i = k; i < n; ++i) {
            ans += reward2[idx[i]];
        }
        return ans;
    }
};
```

#### Go

```go
func miceAndCheese(reward1 []int, reward2 []int, k int) (ans int) {
	n := len(reward1)
	idx := make([]int, n)
	for i := range idx {
		idx[i] = i
	}
	sort.Slice(idx, func(i, j int) bool {
		i, j = idx[i], idx[j]
		return reward1[j]-reward2[j] < reward1[i]-reward2[i]
	})
	for i := 0; i < k; i++ {
		ans += reward1[idx[i]]
	}
	for i := k; i < n; i++ {
		ans += reward2[idx[i]]
	}
	return
}
```

#### TypeScript

```ts
function miceAndCheese(reward1: number[], reward2: number[], k: number): number {
    const n = reward1.length;
    const idx = Array.from({ length: n }, (_, i) => i);
    idx.sort((i, j) => reward1[j] - reward2[j] - (reward1[i] - reward2[i]));
    let ans = 0;
    for (let i = 0; i < k; ++i) {
        ans += reward1[idx[i]];
    }
    for (let i = k; i < n; ++i) {
        ans += reward2[idx[i]];
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 sắp xếp một mảng chỉ số. Ghi các độ chênh lệch vào $reward1$ và sắp xếp ngay trên mảng này sẽ cho $\sum reward2$ cộng với $k$ độ chênh lệch lớn nhất, không cần ánh xạ chỉ số.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def miceAndCheese(self, reward1: List[int], reward2: List[int], k: int) -> int:
        for i, x in enumerate(reward2):
            reward1[i] -= x
        reward1.sort(reverse=True)
        return sum(reward2) + sum(reward1[:k])
```

#### Java

```java
class Solution {
    public int miceAndCheese(int[] reward1, int[] reward2, int k) {
        int ans = 0;
        int n = reward1.length;
        for (int i = 0; i < n; ++i) {
            ans += reward2[i];
            reward1[i] -= reward2[i];
        }
        Arrays.sort(reward1);
        for (int i = 0; i < k; ++i) {
            ans += reward1[n - i - 1];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int miceAndCheese(vector<int>& reward1, vector<int>& reward2, int k) {
        int n = reward1.size();
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            ans += reward2[i];
            reward1[i] -= reward2[i];
        }
        sort(reward1.rbegin(), reward1.rend());
        ans += accumulate(reward1.begin(), reward1.begin() + k, 0);
        return ans;
    }
};
```

#### Go

```go
func miceAndCheese(reward1 []int, reward2 []int, k int) (ans int) {
	for i, x := range reward2 {
		ans += x
		reward1[i] -= x
	}
	sort.Ints(reward1)
	n := len(reward1)
	for i := 0; i < k; i++ {
		ans += reward1[n-i-1]
	}
	return
}
```

#### TypeScript

```ts
function miceAndCheese(reward1: number[], reward2: number[], k: number): number {
    const n = reward1.length;
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        ans += reward2[i];
        reward1[i] -= reward2[i];
    }
    reward1.sort((a, b) => b - a);
    for (let i = 0; i < k; ++i) {
        ans += reward1[i];
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
