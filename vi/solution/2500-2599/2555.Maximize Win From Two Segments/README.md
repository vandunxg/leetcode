---
comments: true
difficulty: Medium
rating: 2080
source: Biweekly Contest 97 Q3
tags:
    - Array
    - Binary Search
    - Sliding Window
---

<!-- problem:start -->

# [2555. Maximize Win From Two Segments](https://leetcode.com/problems/maximize-win-from-two-segments)

[中文文档](/solution/2500-2599/2555.Maximize%20Win%20From%20Two%20Segments/README.md)

## Mô tả

<!-- description:start -->

<p>Có một số phần thưởng trên <strong>trục X</strong>. Cho một mảng số nguyên <code>prizePositions</code> được <strong>sắp xếp theo thứ tự không giảm</strong>, trong đó <code>prizePositions[i]</code> là vị trí của phần thưởng thứ <code>i<sup>th</sup></code>. Có thể có nhiều phần thưởng ở cùng một vị trí trên trục. Bạn cũng được cho một số nguyên <code>k</code>.</p>

<p>Bạn được phép chọn hai đoạn thẳng có các đầu mút là số nguyên. Độ dài của mỗi đoạn phải bằng <code>k</code>. Bạn sẽ thu thập tất cả phần thưởng có vị trí nằm trong ít nhất một trong hai đoạn đã chọn, bao gồm cả hai đầu mút của các đoạn. Hai đoạn đã chọn có thể giao nhau.</p>

<ul>
	<li>Ví dụ, nếu <code>k = 2</code>, bạn có thể chọn hai đoạn <code>[1, 3]</code> và <code>[2, 4]</code>, khi đó bạn sẽ nhận được mọi phần thưởng <font face="monospace">i</font> thỏa mãn <code>1 &lt;= prizePositions[i] &lt;= 3</code> hoặc <code>2 &lt;= prizePositions[i] &lt;= 4</code>.</li>
</ul>

<p><em>Hãy trả về <strong>số lượng phần thưởng tối đa bạn có thể nhận được nếu chọn hai đoạn một cách tối ưu</strong>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> prizePositions = [1,1,2,2,3,3,5], k = 2
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Trong ví dụ này, bạn có thể nhận cả 7 phần thưởng bằng cách chọn hai đoạn [1, 3] và [3, 5].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> prizePositions = [1,2,3,4], k = 0
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Trong ví dụ này, một <strong>cách chọn</strong> hai đoạn là <code>[3, 3]</code> và <code>[4, 4],</code> khi đó bạn có thể nhận được <code>2</code> phần thưởng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= prizePositions.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= prizePositions[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= k &lt;= 10<sup>9</sup> </code></li>
	<li><code>prizePositions</code> được sắp xếp theo thứ tự không giảm.</li>
</ul>

<p>&nbsp;</p>
<style type="text/css">.spoilerbutton {display:block; border:dashed; padding: 0px 0px; margin:10px 0px; font-size:150%; font-weight: bold; color:#000000; background-color:cyan; outline:0;
}
.spoiler {overflow:hidden;}
.spoiler > div {-webkit-transition: all 0s ease;-moz-transition: margin 0s ease;-o-transition: all 0s ease;transition: margin 0s ease;}
.spoilerbutton[value="Show Message"] + .spoiler > div {margin-top:-500%;}
.spoilerbutton[value="Hide Message"] + .spoiler {padding:5px;}
</style>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Hai đoạn có độ dài không quá $k$ có thể bao phủ các vị trí phần thưởng và chúng có thể giao nhau. Việc liệt kê cả hai đầu mút là quá chậm.
>
> Các vị trí đã được sắp xếp. Cố định đầu phải của đoạn thứ hai tại phần thưởng $i$; tìm kiếm nhị phân cho biết phần thưởng ngoài cùng bên trái vẫn nằm trong đoạn có độ dài $k$, nên đoạn này bao phủ $i-j$ phần thưởng. Đoạn thứ nhất phải nằm hoàn toàn trong $j$ phần thưởng đầu tiên, mà số phần thưởng tối đa có thể nhận được là $f[j]$ trong quy hoạch động tiền tố. Khi duyệt, $f[i]$ lưu số phần thưởng tối đa của một đoạn có đầu phải không vượt quá $i$.

<!-- thinking:end -->

Ta định nghĩa $f[i]$ là số phần thưởng tối đa có thể nhận được khi chọn một đoạn có độ dài $k$ từ $i$ phần thưởng đầu tiên. Ban đầu, $f[0] = 0$. Ta định nghĩa biến kết quả là $ans = 0$.

Tiếp theo, ta duyệt qua vị trí $x$ của từng phần thưởng và dùng tìm kiếm nhị phân để tìm chỉ số phần thưởng nhỏ nhất $j$ sao cho $prizePositions[j] \geq x - k$. Khi đó, ta cập nhật kết quả $ans = \max(ans, f[j] + i - j)$ và cập nhật $f[i] = \max(f[i - 1], i - j)$.

Cuối cùng, ta trả về $ans$.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(n)$. Trong đó $n$ là số lượng phần thưởng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximizeWin(self, prizePositions: List[int], k: int) -> int:
        n = len(prizePositions)
        f = [0] * (n + 1)
        ans = 0
        for i, x in enumerate(prizePositions, 1):
            j = bisect_left(prizePositions, x - k)
            ans = max(ans, f[j] + i - j)
            f[i] = max(f[i - 1], i - j)
        return ans
```

#### Java

```java
class Solution {
    public int maximizeWin(int[] prizePositions, int k) {
        int n = prizePositions.length;
        int[] f = new int[n + 1];
        int ans = 0;
        for (int i = 1; i <= n; ++i) {
            int x = prizePositions[i - 1];
            int j = search(prizePositions, x - k);
            ans = Math.max(ans, f[j] + i - j);
            f[i] = Math.max(f[i - 1], i - j);
        }
        return ans;
    }

    private int search(int[] nums, int x) {
        int left = 0, right = nums.length;
        while (left < right) {
            int mid = (left + right) >> 1;
            if (nums[mid] >= x) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximizeWin(vector<int>& prizePositions, int k) {
        int n = prizePositions.size();
        vector<int> f(n + 1);
        int ans = 0;
        for (int i = 1; i <= n; ++i) {
            int x = prizePositions[i - 1];
            int j = lower_bound(prizePositions.begin(), prizePositions.end(), x - k) - prizePositions.begin();
            ans = max(ans, f[j] + i - j);
            f[i] = max(f[i - 1], i - j);
        }
        return ans;
    }
};
```

#### Go

```go
func maximizeWin(prizePositions []int, k int) (ans int) {
	n := len(prizePositions)
	f := make([]int, n+1)
	for i, x := range prizePositions {
		j := sort.Search(n, func(h int) bool { return prizePositions[h] >= x-k })
		ans = max(ans, f[j]+i-j+1)
		f[i+1] = max(f[i], i-j+1)
	}
	return
}
```

#### TypeScript

```ts
function maximizeWin(prizePositions: number[], k: number): number {
    const n = prizePositions.length;
    const f: number[] = Array(n + 1).fill(0);
    let ans = 0;
    const search = (x: number): number => {
        let left = 0;
        let right = n;
        while (left < right) {
            const mid = (left + right) >> 1;
            if (prizePositions[mid] >= x) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    };
    for (let i = 1; i <= n; ++i) {
        const x = prizePositions[i - 1];
        const j = search(x - k);
        ans = Math.max(ans, f[j] + i - j);
        f[i] = Math.max(f[i - 1], i - j);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
