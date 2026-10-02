---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - Counting
    - Sorting
---

<!-- problem:start -->

# [945. Minimum Increment to Make Array Unique](https://leetcode.com/problems/minimum-increment-to-make-array-unique)

[中文文档](/solution/0900-0999/0945.Minimum%20Increment%20to%20Make%20Array%20Unique/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code>. Trong một lượt, bạn có thể chọn chỉ số <code>i</code> thỏa mãn <code>0 &lt;= i &lt; nums.length</code> rồi tăng <code>nums[i]</code> thêm <code>1</code>.</p>

<p>Hãy trả về <em>số lượt ít nhất để mọi giá trị trong </em><code>nums</code><em> đều </em><strong>khác nhau</strong>.</p>

<p>Dữ liệu kiểm thử được tạo sao cho đáp án nằm trong phạm vi số nguyên 32-bit.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,2]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Sau 1 lượt, mảng có thể là [1, 2, 3].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,2,1,2,1,7]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Sau 6 lượt, mảng có thể là [3, 4, 1, 2, 5, 7].
Có thể chứng minh rằng không thể làm cho mọi giá trị trong mảng khác nhau chỉ với tối đa 5 lượt.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Greedy

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lượt tăng một phần tử; ta cần làm cho mọi giá trị khác nhau với chi phí nhỏ nhất. Sau khi sắp xếp, mỗi $x$ được đặt vào vị trí trống tiếp theo $y$. Gán $y\leftarrow\max(y+1,x)$ rồi cộng $y-x$ vào số lượt.

<!-- thinking:end -->

Trước tiên, sắp xếp mảng $\textit{nums}$ và dùng biến $\textit{y}$ để lưu giá trị lớn nhất hiện tại; ban đầu $\textit{y} = -1$.

Sau đó, duyệt mảng $\textit{nums}$. Với mỗi phần tử $x$, cập nhật $y$ thành $\max(y + 1, x)$ rồi cộng số lượt $y - x$ vào kết quả.

Duyệt xong thì trả về kết quả.

Độ phức tạp thời gian là $O(n \log n)$ và độ phức tạp không gian là $O(\log n)$, trong đó $n$ là độ dài mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minIncrementForUnique(self, nums: List[int]) -> int:
        nums.sort()
        ans, y = 0, -1
        for x in nums:
            y = max(y + 1, x)
            ans += y - x
        return ans
```

#### Java

```java
class Solution {
    public int minIncrementForUnique(int[] nums) {
        Arrays.sort(nums);
        int ans = 0, y = -1;
        for (int x : nums) {
            y = Math.max(y + 1, x);
            ans += y - x;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minIncrementForUnique(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int ans = 0, y = -1;
        for (int x : nums) {
            y = max(y + 1, x);
            ans += y - x;
        }
        return ans;
    }
};
```

#### Go

```go
func minIncrementForUnique(nums []int) (ans int) {
	sort.Ints(nums)
	y := -1
	for _, x := range nums {
		y = max(y+1, x)
		ans += y - x
	}
	return
}
```

#### TypeScript

```ts
function minIncrementForUnique(nums: number[]): number {
    nums.sort((a, b) => a - b);
    let [ans, y] = [0, -1];
    for (const x of nums) {
        y = Math.max(y + 1, x);
        ans += y - x;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- source:start -->

### Lời giải 2: Đếm + Greedy

<!-- thinking:start -->

> **Tư duy**
>
> Sắp xếp tốn $O(n\log n)$. Với cận trên $m=\max+n$, ta có thể dùng mảng đếm: duyệt từ nhỏ đến lớn, chuyển $cnt[i]-1$ bản sao dư của $i$ sang $i+1$, chỉ cần quét đoạn một lần.

<!-- thinking:end -->

Theo đề bài, giá trị lớn nhất của mảng kết quả là $m = \max(\textit{nums}) + \textit{len}(\textit{nums})$. Ta có thể dùng mảng đếm $\textit{cnt}$ để ghi lại số lần xuất hiện của mỗi phần tử.

Sau đó, duyệt từ $0$ đến $m - 1$. Với mỗi giá trị $i$, nếu số lần xuất hiện $\textit{cnt}[i]$ lớn hơn $1$, chuyển $\textit{cnt}[i] - 1$ phần tử sang $i + 1$ và cộng số lượt cần thiết vào kết quả.

Duyệt xong thì trả về kết quả.

Độ phức tạp thời gian là $O(m)$ và độ phức tạp không gian là $O(m)$, trong đó $m$ bằng độ dài mảng $\textit{nums}$ cộng với giá trị lớn nhất trong mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minIncrementForUnique(self, nums: List[int]) -> int:
        m = max(nums) + len(nums)
        cnt = Counter(nums)
        ans = 0
        for i in range(m - 1):
            if (diff := cnt[i] - 1) > 0:
                cnt[i + 1] += diff
                ans += diff
        return ans
```

#### Java

```java
class Solution {
    public int minIncrementForUnique(int[] nums) {
        int m = Arrays.stream(nums).max().getAsInt() + nums.length;
        int[] cnt = new int[m];
        for (int x : nums) {
            ++cnt[x];
        }
        int ans = 0;
        for (int i = 0; i < m - 1; ++i) {
            int diff = cnt[i] - 1;
            if (diff > 0) {
                cnt[i + 1] += diff;
                ans += diff;
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
    int minIncrementForUnique(vector<int>& nums) {
        int m = *max_element(nums.begin(), nums.end()) + nums.size();
        int cnt[m];
        memset(cnt, 0, sizeof(cnt));
        for (int x : nums) {
            ++cnt[x];
        }
        int ans = 0;
        for (int i = 0; i < m - 1; ++i) {
            int diff = cnt[i] - 1;
            if (diff > 0) {
                cnt[i + 1] += diff;
                ans += diff;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minIncrementForUnique(nums []int) (ans int) {
	m := slices.Max(nums) + len(nums)
	cnt := make([]int, m)
	for _, x := range nums {
		cnt[x]++
	}
	for i := 0; i < m-1; i++ {
		if diff := cnt[i] - 1; diff > 0 {
			cnt[i+1] += diff
			ans += diff
		}
	}
	return ans
}
```

#### TypeScript

```ts
function minIncrementForUnique(nums: number[]): number {
    const m = Math.max(...nums) + nums.length;
    const cnt: number[] = Array(m).fill(0);
    for (const x of nums) {
        cnt[x]++;
    }
    let ans = 0;
    for (let i = 0; i < m - 1; ++i) {
        const diff = cnt[i] - 1;
        if (diff > 0) {
            cnt[i + 1] += diff;
            ans += diff;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
