---
comments: true
difficulty: Hard
rating: 2214
source: Weekly Contest 244 Q4
tags:
    - Array
    - Binary Search
    - Prefix Sum
    - Sorting
---

<!-- problem:start -->

# [1889. Minimum Space Wasted From Packaging](https://leetcode.com/problems/minimum-space-wasted-from-packaging)

[中文文档](/solution/1800-1899/1889.Minimum%20Space%20Wasted%20From%20Packaging/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có <code>n</code> gói hàng cần đặt vào các hộp, <strong>mỗi hộp chứa một gói hàng</strong>. Có <code>m</code> nhà cung cấp, mỗi nhà cung cấp sản xuất các hộp có <strong>kích thước khác nhau</strong> với số lượng không giới hạn. Một gói hàng có thể đặt vào một hộp nếu kích thước của gói hàng <strong>nhỏ hơn hoặc bằng</strong> kích thước hộp.</p>

<p>Kích thước các gói hàng được cho bởi mảng số nguyên <code>packages</code>, trong đó <code>packages[i]</code> là <strong>kích thước</strong> của gói hàng thứ <code>i<sup>th</sup></code>. Các nhà cung cấp được cho bởi mảng số nguyên hai chiều <code>boxes</code>, trong đó <code>boxes[j]</code> là một mảng các <strong>kích thước hộp</strong> mà nhà cung cấp thứ <code>j<sup>th</sup></code> sản xuất.</p>

<p>Bạn muốn chọn <strong>một nhà cung cấp duy nhất</strong> và sử dụng các hộp của họ sao cho <strong>tổng không gian lãng phí</strong> là <strong>nhỏ nhất</strong>. Với mỗi gói hàng trong một hộp, không gian <strong>lãng phí</strong> được định nghĩa là <code>size of the box - size of the package</code>. <strong>Tổng không gian lãng phí</strong> là tổng không gian lãng phí trong <strong>tất cả các hộp</strong>.</p>

<ul>
	<li>Ví dụ, nếu cần đặt các gói hàng có kích thước <code>[2,3,5]</code> và nhà cung cấp cung cấp hộp kích thước <code>[4,8]</code>, ta có thể đặt các gói kích thước <code>2</code> và <code>3</code> vào hai hộp kích thước <code>4</code>, còn gói kích thước <code>5</code> vào một hộp kích thước <code>8</code>. Khi đó phần lãng phí là <code>(4-2) + (4-3) + (8-5) = 6</code>.</li>
</ul>

<p>Trả về <em><strong>tổng không gian lãng phí nhỏ nhất</strong> khi chọn nhà cung cấp <strong>tối ưu</strong>, hoặc </em><code>-1</code> <i>nếu <strong>không thể</strong> đặt tất cả gói hàng vào các hộp. </i>Vì đáp án có thể <strong>lớn</strong>, hãy trả về đáp án <strong>theo modulo </strong><code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> packages = [2,3,5], boxes = [[4,8],[2,8]]
<strong>Đầu ra:</strong> 6
<strong>Giải thích</strong>: Chọn nhà cung cấp thứ nhất là tối ưu, dùng hai hộp kích thước 4 và một hộp kích thước 8.
Tổng phần lãng phí là (4-2) + (4-3) + (8-5) = 6.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> packages = [2,3,5], boxes = [[1,4],[2,3],[3,4]]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không có hộp nào chứa được gói hàng kích thước 5.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> packages = [3,5,8,10,11,12], boxes = [[12],[11,9],[10,5,14]]
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong> Chọn nhà cung cấp thứ ba là tối ưu, dùng hai hộp kích thước 5, hai hộp kích thước 10 và hai hộp kích thước 14.
Tổng phần lãng phí là (5-3) + (5-5) + (10-8) + (10-10) + (14-11) + (14-12) = 9.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == packages.length</code></li>
	<li><code>m == boxes.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= m &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= packages[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= boxes[j].length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= boxes[j][k] &lt;= 10<sup>5</sup></code></li>
	<li><code>sum(boxes[j].length) &lt;= 10<sup>5</sup></code></li>
	<li>Các phần tử trong <code>boxes[j]</code> là <strong>phân biệt</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi nhà cung cấp đưa ra các kích thước hộp; mỗi hộp chứa nhiều nhất một gói hàng không lớn hơn hộp. Ta muốn tổng không gian lãng phí nhỏ nhất. Duyệt các gói hàng cho từng hộp là quá chậm.
>
> Sắp xếp các gói hàng và các hộp của từng nhà cung cấp. Tìm kiếm nhị phân giúp xác định nhóm gói hàng tiếp theo vừa với hộp hiện tại và cộng $b$ nhân với số lượng nhóm đó. Không gian lãng phí là tổng không gian hộp trừ tổng kích thước gói hàng; ta chọn nhà cung cấp tốt nhất.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minWastedSpace(self, packages: List[int], boxes: List[List[int]]) -> int:
        mod = 10**9 + 7
        ans = inf
        packages.sort()
        for box in boxes:
            box.sort()
            if packages[-1] > box[-1]:
                continue
            s = i = 0
            for b in box:
                j = bisect_right(packages, b, lo=i)
                s += (j - i) * b
                i = j
            ans = min(ans, s)
        if ans == inf:
            return -1
        return (ans - sum(packages)) % mod
```

#### Java

```java
class Solution {
    public int minWastedSpace(int[] packages, int[][] boxes) {
        int n = packages.length;
        final long inf = 1L << 62;
        Arrays.sort(packages);
        long ans = inf;
        for (var box : boxes) {
            Arrays.sort(box);
            if (packages[n - 1] > box[box.length - 1]) {
                continue;
            }
            long s = 0;
            int i = 0;
            for (int b : box) {
                int j = search(packages, b, i);
                s += 1L * (j - i) * b;
                i = j;
            }
            ans = Math.min(ans, s);
        }
        if (ans == inf) {
            return -1;
        }
        long s = 0;
        for (int p : packages) {
            s += p;
        }
        final int mod = (int) 1e9 + 7;
        return (int) ((ans - s) % mod);
    }

    private int search(int[] nums, int x, int l) {
        int r = nums.length;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (nums[mid] > x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minWastedSpace(vector<int>& packages, vector<vector<int>>& boxes) {
        int n = packages.size(), m = boxes.size();
        sort(packages.begin(), packages.end());
        const int mod = 1e9 + 7;
        const long long inf = 1LL << 62;
        long long ans = inf;
        for (auto& box : boxes) {
            sort(box.begin(), box.end());
            if (packages.back() > box.back()) {
                continue;
            }
            int i = 0;
            long long s = 0;
            for (auto& b : box) {
                int j = upper_bound(packages.begin() + i, packages.end(), b) - packages.begin();
                s += 1LL * (j - i) * b;
                i = j;
            }
            ans = min(ans, s);
        }
        return ans == inf ? -1 : (ans - accumulate(packages.begin(), packages.end(), 0LL)) % mod;
    }
};
```

#### Go

```go
func minWastedSpace(packages []int, boxes [][]int) int {
	n := len(packages)
	inf := 1 << 62
	sort.Ints(packages)
	ans := inf
	for _, box := range boxes {
		sort.Ints(box)
		if packages[n-1] > box[len(box)-1] {
			continue
		}
		s, i := 0, 0
		for _, b := range box {
			j := sort.SearchInts(packages[i:], b+1) + i
			s += (j - i) * b
			i = j
		}
		ans = min(ans, s)
	}
	if ans == inf {
		return -1
	}
	s := 0
	for _, p := range packages {
		s += p
	}
	const mod = 1e9 + 7
	return (ans - s) % mod
}
```

#### TypeScript

```ts
function minWastedSpace(packages: number[], boxes: number[][]): number {
    const n = packages.length;
    const inf = Infinity;
    packages.sort((a, b) => a - b);
    let ans = inf;
    for (const box of boxes) {
        box.sort((a, b) => a - b);
        if (packages[n - 1] > box[box.length - 1]) {
            continue;
        }
        let s = 0;
        let i = 0;
        for (const b of box) {
            const j = search(packages, b, i);
            s += (j - i) * b;
            i = j;
        }
        ans = Math.min(ans, s);
    }
    if (ans === inf) {
        return -1;
    }
    const s = packages.reduce((a, b) => a + b, 0);
    return (ans - s) % 1000000007;
}

function search(nums: number[], x: number, l: number): number {
    let r = nums.length;
    while (l < r) {
        const mid = (l + r) >> 1;
        if (nums[mid] > x) {
            r = mid;
        } else {
            l = mid + 1;
        }
    }
    return l;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
