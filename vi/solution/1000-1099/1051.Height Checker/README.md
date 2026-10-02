---
comments: true
difficulty: Easy
rating: 1303
source: Weekly Contest 138 Q1
tags:
    - Array
    - Bubble Sort
    - Counting Sort
    - Sorting
---

<!-- problem:start -->

# [1051. Height Checker](https://leetcode.com/problems/height-checker)

[中文文档](/solution/1000-1099/1051.Height%20Checker/README.md)

## Mô tả

<!-- description:start -->

<p>Một trường học muốn chụp ảnh thường niên cho tất cả học sinh. Học sinh được yêu cầu xếp thành một hàng theo thứ tự chiều cao <strong>không giảm</strong>. Gọi thứ tự này là mảng số nguyên <code>expected</code>, trong đó <code>expected[i]</code> là chiều cao dự kiến của học sinh thứ <code>i<sup>th</sup></code> trong hàng.</p>

<p>Cho mảng số nguyên <code>heights</code> biểu diễn <strong>thứ tự hiện tại</strong> của học sinh. Mỗi <code>heights[i]</code> là chiều cao của học sinh thứ <code>i<sup>th</sup></code> trong hàng (đánh chỉ số từ <strong>0</strong>).</p>

<p>Hãy trả về <em><strong>số chỉ số</strong> mà tại đó </em><code>heights[i] != expected[i]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> heights = [1,1,4,2,1,3]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> 
heights:  [1,1,<u>4</u>,2,<u>1</u>,<u>3</u>]
expected: [1,1,<u>1</u>,2,<u>3</u>,<u>4</u>]
Các chỉ số 2, 4 và 5 không khớp.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> heights = [5,1,2,3,4]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong>
heights:  [<u>5</u>,<u>1</u>,<u>2</u>,<u>3</u>,<u>4</u>]
expected: [<u>1</u>,<u>2</u>,<u>3</u>,<u>4</u>,<u>5</u>]
Không có chỉ số nào khớp.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> heights = [1,2,3,4,5]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>
heights:  [1,2,3,4,5]
expected: [1,2,3,4,5]
Tất cả chỉ số đều khớp.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= heights.length &lt;= 100</code></li>
	<li><code>1 &lt;= heights[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Số học sinh đứng sai vị trí chính là số vị trí mà mảng khác với phiên bản đã được sắp xếp không giảm. Vì $n\le 100$, ta chỉ cần sắp xếp rồi so sánh.
>
> So sánh lần lượt bản sao đã sắp xếp $\textit{expected}$ với $\textit{heights}$ rồi đếm số vị trí không khớp.

<!-- thinking:end -->

Trước tiên, sắp xếp chiều cao của học sinh, sau đó so sánh với thứ tự chiều cao ban đầu và đếm các vị trí khác nhau.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, với $n$ là số học sinh.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def heightChecker(self, heights: List[int]) -> int:
        expected = sorted(heights)
        return sum(a != b for a, b in zip(heights, expected))
```

#### Java

```java
class Solution {
    public int heightChecker(int[] heights) {
        int[] expected = heights.clone();
        Arrays.sort(expected);
        int ans = 0;
        for (int i = 0; i < heights.length; ++i) {
            if (heights[i] != expected[i]) {
                ++ans;
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
    int heightChecker(vector<int>& heights) {
        vector<int> expected = heights;
        sort(expected.begin(), expected.end());
        int ans = 0;
        for (int i = 0; i < heights.size(); ++i) {
            ans += heights[i] != expected[i];
        }
        return ans;
    }
};
```

#### Go

```go
func heightChecker(heights []int) (ans int) {
	expected := slices.Clone(heights)
	sort.Ints(expected)
	for i, v := range heights {
		if v != expected[i] {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function heightChecker(heights: number[]): number {
    const expected = [...heights].sort((a, b) => a - b);
    return heights.reduce((acc, cur, i) => acc + (cur !== expected[i] ? 1 : 0), 0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Counting sort

<!-- thinking:start -->

> **Tư duy**
>
> Comparison sort có độ phức tạp $O(n\log n)$ và cần thêm một mảng. Chiều cao nằm trong khoảng $1..100$, nên counting sort có thể tạo thứ tự dự kiến trong thời gian tuyến tính.
>
> Đếm số lần xuất hiện của từng chiều cao, sau đó lần lượt xét chiều cao từ $1$ đến $100$ để so sánh với mảng ban đầu và đếm số vị trí không khớp.

<!-- thinking:end -->

Vì chiều cao của học sinh không vượt quá $100$, ta có thể dùng counting sort. Ở đây, dùng mảng $cnt$ có độ dài $101$ để đếm số lần xuất hiện của mỗi chiều cao $h_i$.

Độ phức tạp thời gian là $O(n + M)$ và độ phức tạp không gian là $O(M)$, với $n$ là số học sinh và $M$ là chiều cao lớn nhất. Trong bài này, $M = 101$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def heightChecker(self, heights: List[int]) -> int:
        cnt = [0] * 101
        for h in heights:
            cnt[h] += 1
        ans = i = 0
        for j in range(1, 101):
            while cnt[j]:
                cnt[j] -= 1
                if heights[i] != j:
                    ans += 1
                i += 1
        return ans
```

#### Java

```java
class Solution {
    public int heightChecker(int[] heights) {
        int[] cnt = new int[101];
        for (int h : heights) {
            ++cnt[h];
        }
        int ans = 0;
        for (int i = 0, j = 0; i < 101; ++i) {
            while (cnt[i] > 0) {
                --cnt[i];
                if (heights[j++] != i) {
                    ++ans;
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
    int heightChecker(vector<int>& heights) {
        vector<int> cnt(101);
        for (int& h : heights) ++cnt[h];
        int ans = 0;
        for (int i = 0, j = 0; i < 101; ++i) {
            while (cnt[i]) {
                --cnt[i];
                if (heights[j++] != i) ++ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func heightChecker(heights []int) int {
	cnt := make([]int, 101)
	for _, h := range heights {
		cnt[h]++
	}
	ans := 0
	for i, j := 0, 0; i < 101; i++ {
		for cnt[i] > 0 {
			cnt[i]--
			if heights[j] != i {
				ans++
			}
			j++
		}
	}
	return ans
}
```

#### TypeScript

```ts
function heightChecker(heights: number[]): number {
    const cnt = Array(101).fill(0);
    for (const i of heights) {
        cnt[i]++;
    }
    let ans = 0;
    for (let j = 1, i = 0; j < 101; j++) {
        while (cnt[j]--) {
            if (heights[i++] !== j) {
                ans++;
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
