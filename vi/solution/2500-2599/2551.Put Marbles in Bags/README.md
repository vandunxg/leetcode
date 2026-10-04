---
comments: true
difficulty: Hard
rating: 2042
source: Weekly Contest 330 Q3
tags:
    - Greedy
    - Array
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2551. Put Marbles in Bags](https://leetcode.com/problems/put-marbles-in-bags)

[Tài liệu tiếng Trung](/solution/2500-2599/2551.Put%20Marbles%20in%20Bags/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có <code>k</code> túi. Cho một mảng số nguyên <strong>đánh chỉ số từ 0</strong> <code>weights</code>, trong đó <code>weights[i]</code> là trọng lượng của viên bi thứ <code>i<sup>th</sup></code>. Bạn cũng được cho số nguyên <code>k.</code></p>

<p>Hãy chia các viên bi vào <code>k</code> túi theo các quy tắc sau:</p>

<ul>
	<li>Không túi nào được rỗng.</li>
	<li>Nếu viên bi thứ <code>i<sup>th</sup></code> và viên bi thứ <code>j<sup>th</sup></code> nằm trong cùng một túi, thì tất cả các viên bi có chỉ số nằm giữa chỉ số của viên bi thứ <code>i<sup>th</sup></code> và viên bi thứ <code>j<sup>th</sup></code> cũng phải nằm trong túi đó.</li>
	<li>Nếu một túi gồm tất cả các viên bi có chỉ số từ <code>i</code> đến <code>j</code>, bao gồm cả hai đầu mút, thì chi phí của túi đó là <code>weights[i] + weights[j]</code>.</li>
</ul>

<p><strong>Điểm số</strong> sau khi phân phối các viên bi là tổng chi phí của tất cả <code>k</code> túi.</p>

<p>Trả về <em><strong>hiệu</strong> giữa <strong>điểm số lớn nhất</strong> và <strong>điểm số nhỏ nhất</strong> trong tất cả các cách phân phối các viên bi</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> weights = [1,3,5,1], k = 2
<strong>Output:</strong> 4
<strong>Explanation:</strong>
Cách phân phối [1],[3,5,1] cho điểm số nhỏ nhất là (1+1) + (3+1) = 6.
Cách phân phối [1,3],[5,1] cho điểm số lớn nhất là (1+3) + (5+1) = 10.
Vì vậy, ta trả về hiệu 10 - 6 = 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> weights = [1, 3], k = 2
<strong>Output:</strong> 0
<strong>Explanation:</strong> Chỉ có thể phân phối [1],[3].
Vì điểm số lớn nhất và nhỏ nhất bằng nhau, ta trả về 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= weights.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= weights[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Biến đổi bài toán + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Chia dãy viên bi thành $k$ túi liên tiếp; chi phí của một túi là tổng trọng lượng ở hai đầu. Ta cần tính điểm số lớn nhất trừ điểm số nhỏ nhất. Trọng lượng ở phần tử đầu và cuối luôn được tính; mỗi lần chia thêm một cặp $w_i+w_{i+1}$.
>
> Các vị trí chia không ảnh hưởng lẫn nhau. Sắp xếp tất cả các tổng của hai phần tử kề nhau; hiệu giữa $k-1$ tổng lớn nhất và $k-1$ tổng nhỏ nhất chính là đáp án sau khi các đầu mút chung được triệt tiêu.

<!-- thinking:end -->

Ta có thể biến đổi bài toán thành: chia mảng `weights` thành $k$ mảng con liên tiếp, tức là cần tìm $k-1$ vị trí chia, trong đó chi phí của mỗi vị trí chia là tổng của phần tử bên trái và bên phải vị trí đó. Hiệu giữa tổng chi phí của $k-1$ vị trí chia lớn nhất và $k-1$ vị trí chia nhỏ nhất chính là đáp án.

Do đó, ta duyệt mảng `weights` và biến đổi nó thành một mảng `arr` có độ dài $n-1$, trong đó `arr[i] = weights[i] + weights[i+1]`. Sau đó, ta sắp xếp mảng `arr`, rồi tính hiệu giữa tổng chi phí của $k-1$ vị trí chia lớn nhất và $k-1$ vị trí chia nhỏ nhất.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng `weights`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def putMarbles(self, weights: List[int], k: int) -> int:
        arr = sorted(a + b for a, b in pairwise(weights))
        return sum(arr[len(arr) - k + 1 :]) - sum(arr[: k - 1])
```

#### Java

```java
class Solution {
    public long putMarbles(int[] weights, int k) {
        int n = weights.length;
        int[] arr = new int[n - 1];
        for (int i = 0; i < n - 1; ++i) {
            arr[i] = weights[i] + weights[i + 1];
        }
        Arrays.sort(arr);
        long ans = 0;
        for (int i = 0; i < k - 1; ++i) {
            ans -= arr[i];
            ans += arr[n - 2 - i];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long putMarbles(vector<int>& weights, int k) {
        int n = weights.size();
        vector<int> arr(n - 1);
        for (int i = 0; i < n - 1; ++i) {
            arr[i] = weights[i] + weights[i + 1];
        }
        sort(arr.begin(), arr.end());
        long long ans = 0;
        for (int i = 0; i < k - 1; ++i) {
            ans -= arr[i];
            ans += arr[n - 2 - i];
        }
        return ans;
    }
};
```

#### Go

```go
func putMarbles(weights []int, k int) (ans int64) {
	n := len(weights)
	arr := make([]int, n-1)
	for i, w := range weights[:n-1] {
		arr[i] = w + weights[i+1]
	}
	sort.Ints(arr)
	for i := 0; i < k-1; i++ {
		ans += int64(arr[n-2-i] - arr[i])
	}
	return
}
```

#### TypeScript

```ts
function putMarbles(weights: number[], k: number): number {
    const n = weights.length;
    const arr: number[] = [];
    for (let i = 0; i < n - 1; ++i) {
        arr.push(weights[i] + weights[i + 1]);
    }
    arr.sort((a, b) => a - b);
    let ans = 0;
    for (let i = 0; i < k - 1; ++i) {
        ans += arr[n - i - 2] - arr[i];
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
