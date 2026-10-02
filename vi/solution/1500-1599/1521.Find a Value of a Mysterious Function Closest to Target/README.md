---
comments: true
difficulty: Hard
rating: 2383
source: Weekly Contest 198 Q4
tags:
    - Bit Manipulation
    - Segment Tree
    - Array
    - Binary Search
    - Sparse Table
---

<!-- problem:start -->

# [1521. Find a Value of a Mysterious Function Closest to Target](https://leetcode.com/problems/find-a-value-of-a-mysterious-function-closest-to-target)

[中文文档](/solution/1500-1599/1521.Find%20a%20Value%20of%20a%20Mysterious%20Function%20Closest%20to%20Target/README.md)

## Mô tả

<!-- description:start -->

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1521.Find%20a%20Value%20of%20a%20Mysterious%20Function%20Closest%20to%20Target/images/change.png" style="width: 635px; height: 312px;" /></p>

<p>Winston được cho hàm bí ẩn <code>func</code> ở trên. Anh ấy có mảng số nguyên <code>arr</code> và số nguyên <code>target</code>, đồng thời muốn tìm các giá trị <code>l</code> và <code>r</code> để giá trị <code>|func(arr, l, r) - target|</code> nhỏ nhất có thể.</p>

<p>Trả về <em>giá trị nhỏ nhất có thể</em> của <code>|func(arr, l, r) - target|</code>.</p>

<p>Lưu ý rằng <code>func</code> được gọi với các giá trị <code>l</code> và <code>r</code> thỏa mãn <code>0 &lt;= l, r &lt; arr.length</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> arr = [9,12,3,7,15], target = 5
<strong>Output:</strong> 2
<strong>Giải thích:</strong> Khi gọi func với mọi cặp [l,r] = [[0,0],[1,1],[2,2],[3,3],[4,4],[0,1],[1,2],[2,3],[3,4],[0,2],[1,3],[2,4],[0,3],[1,4],[0,4]], Winston nhận được các kết quả [9,12,3,7,15,8,0,3,7,0,0,3,0,0,0]. Các giá trị gần 5 nhất là 7 và 3, nên hiệu nhỏ nhất là 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> arr = [1000000,1000000,1000000], target = 1
<strong>Output:</strong> 999999
<strong>Giải thích:</strong> Winston gọi func với mọi giá trị có thể của [l,r] và luôn nhận được 1000000, nên hiệu nhỏ nhất là 999999.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> arr = [1,2,4,8,16], target = 0
<strong>Output:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= arr[i] &lt;= 10<sup>6</sup></code></li>
	<li><code>0 &lt;= target &lt;= 10<sup>7</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> $func$ là phép AND bitwise của một mảng con; ta muốn kết quả gần $target$ nhất có thể. Có $O(n^2)$ mảng con và $n\le 10^5$, nên không thể tạo tất cả chúng.
>
> Khi đầu trái dịch sang trái với đầu phải cố định, kết quả AND đơn điệu và chỉ nhận nhiều nhất $O(\log A)$ giá trị khác nhau, vì mỗi lần thay đổi sẽ xóa ít nhất một bit. Một set lưu mọi kết quả AND kết thúc tại chỉ số hiện tại, được suy ra từ set trước bằng cách AND với $arr[i]$, đồng thời ta theo dõi độ lệch nhỏ nhất so với $target$.

<!-- thinking:end -->

Theo mô tả bài toán, ta biết hàm $func(arr, l, r)$ thực chất là kết quả AND bitwise của các phần tử trong mảng $arr$ từ chỉ số $l$ đến $r$, tức là $arr[l] \& arr[l + 1] \& \cdots \& arr[r]$.

Nếu cố định đầu phải $r$, miền của đầu trái $l$ là $[0, r]$. Vì kết quả AND bitwise giảm đơn điệu khi $l$ giảm, và giá trị của $arr[i]$ không vượt quá $10^6$, trong đoạn $[0, r]$ có nhiều nhất $20$ giá trị khác nhau. Vì vậy, ta dùng một set để duy trì mọi giá trị của $arr[l] \& arr[l + 1] \& \cdots \& arr[r]$. Khi duyệt từ $r$ đến $r+1$, giá trị có đầu phải $r+1$ được tạo bằng cách AND từng giá trị trong set với $arr[r + 1]$, đồng thời thêm chính $arr[r + 1]$. Sau đó, chỉ cần duyệt các giá trị trong set và AND với $arr[r]$ để thu được mọi giá trị có đầu phải $r$. Lấy hiệu mỗi giá trị với $target$ rồi tính trị tuyệt đối để có độ lệch giữa mỗi giá trị và $target$; giá trị nhỏ nhất là đáp án.

Độ phức tạp thời gian là $O(n \times \log M)$ và độ phức tạp không gian là $O(\log M)$. Ở đây, $n$ và $M$ lần lượt là độ dài mảng $arr$ và giá trị lớn nhất trong mảng $arr$.

Similar problems:

- [3171. Find Subarray With Bitwise AND Closest to K](https://github.com/doocs/leetcode/blob/main/solution/3100-3199/3171.Find%20Subarray%20With%20Bitwise%20AND%20Closest%20to%20K/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def closestToTarget(self, arr: List[int], target: int) -> int:
        ans = abs(arr[0] - target)
        s = {arr[0]}
        for x in arr:
            s = {x & y for y in s} | {x}
            ans = min(ans, min(abs(y - target) for y in s))
        return ans
```

#### Java

```java
class Solution {
    public int closestToTarget(int[] arr, int target) {
        int ans = Math.abs(arr[0] - target);
        Set<Integer> pre = new HashSet<>();
        pre.add(arr[0]);
        for (int x : arr) {
            Set<Integer> cur = new HashSet<>();
            for (int y : pre) {
                cur.add(x & y);
            }
            cur.add(x);
            for (int y : cur) {
                ans = Math.min(ans, Math.abs(y - target));
            }
            pre = cur;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int closestToTarget(vector<int>& arr, int target) {
        int ans = abs(arr[0] - target);
        unordered_set<int> pre;
        pre.insert(arr[0]);
        for (int x : arr) {
            unordered_set<int> cur;
            cur.insert(x);
            for (int y : pre) {
                cur.insert(x & y);
            }
            for (int y : cur) {
                ans = min(ans, abs(y - target));
            }
            pre = move(cur);
        }
        return ans;
    }
};
```

#### Go

```go
func closestToTarget(arr []int, target int) int {
	ans := abs(arr[0] - target)
	pre := map[int]bool{arr[0]: true}
	for _, x := range arr {
		cur := map[int]bool{x: true}
		for y := range pre {
			cur[x&y] = true
		}
		for y := range cur {
			ans = min(ans, abs(y-target))
		}
		pre = cur
	}
	return ans
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function closestToTarget(arr: number[], target: number): number {
    let ans = Math.abs(arr[0] - target);
    let pre = new Set<number>();
    pre.add(arr[0]);
    for (const x of arr) {
        const cur = new Set<number>();
        cur.add(x);
        for (const y of pre) {
            cur.add(x & y);
        }
        for (const y of cur) {
            ans = Math.min(ans, Math.abs(y - target));
        }
        pre = cur;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
