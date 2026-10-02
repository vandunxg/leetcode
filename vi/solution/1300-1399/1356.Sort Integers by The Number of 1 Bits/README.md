---
comments: true
difficulty: Easy
rating: 1257
source: Biweekly Contest 20 Q1
tags:
    - Bit Manipulation
    - Array
    - Counting
    - Sorting
---

<!-- problem:start -->

# [1356. Sort Integers by The Number of 1 Bits](https://leetcode.com/problems/sort-integers-by-the-number-of-1-bits)

[中文文档](/solution/1300-1399/1356.Sort%20Integers%20by%20The%20Number%20of%201%20Bits/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>arr</code>. Hãy sắp xếp các số trong mảng theo thứ tự tăng dần dựa trên số bit <code>1</code> trong biểu diễn nhị phân; nếu hai hay nhiều số có cùng số bit <code>1</code>, hãy sắp xếp chúng theo giá trị tăng dần.</p>

<p>Trả về <em>mảng sau khi sắp xếp</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> arr = [0,1,2,3,4,5,6,7,8]
<strong>Output:</strong> [0,1,2,4,8,3,5,6,7]
<strong>Giải thích:</strong> [0] là số nguyên duy nhất có 0 bit 1.
[1,2,4,8] đều có 1 bit 1.
[3,5,6] có 2 bit 1.
[7] có 3 bit 1.
Mảng sau khi sắp xếp theo số bit 1 là [0,1,2,4,8,3,5,6,7].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> arr = [1024,512,256,128,64,32,16,8,4,2,1]
<strong>Output:</strong> [1,2,4,8,16,32,64,128,256,512,1024]
<strong>Giải thích:</strong> Tất cả số nguyên đều có 1 bit 1 trong biểu diễn nhị phân, nên chỉ cần sắp xếp chúng theo giá trị tăng dần.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 500</code></li>
	<li><code>0 &lt;= arr[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp tùy chỉnh

<!-- thinking:start -->

> **Tư duy**
>
> Sắp xếp theo số bit 1, rồi theo giá trị số nguyên. Key $(x.\mathrm{bit\_count}(), x)$ gộp cả hai tiêu chí vào một phép so sánh.

<!-- thinking:end -->

Ta sắp xếp mảng $arr$ theo yêu cầu của đề bài: số lượng $1$ trong biểu diễn nhị phân tăng dần. Nếu nhiều số có cùng số lượng bit $1$, ta sắp xếp chúng theo giá trị số tăng dần.

Độ phức tạp thời gian là $O(n \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài mảng $arr$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sortByBits(self, arr: List[int]) -> List[int]:
        return sorted(arr, key=lambda x: (x.bit_count(), x))
```

#### Java

```java
class Solution {
    public int[] sortByBits(int[] arr) {
        int n = arr.length;
        for (int i = 0; i < n; ++i) {
            arr[i] += Integer.bitCount(arr[i]) * 100000;
        }
        Arrays.sort(arr);
        for (int i = 0; i < n; ++i) {
            arr[i] %= 100000;
        }
        return arr;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> sortByBits(vector<int>& arr) {
        for (int& x : arr) {
            x += __builtin_popcount(x) * 100000;
        }
        ranges::sort(arr);
        for (int& x : arr) {
            x %= 100000;
        }
        return arr;
    }
};
```

#### Go

```go
func sortByBits(arr []int) []int {
	for i, v := range arr {
		arr[i] += bits.OnesCount(uint(v)) * 100000
	}
	sort.Ints(arr)
	for i := range arr {
		arr[i] %= 100000
	}
	return arr
}
```

#### TypeScript

```ts
function sortByBits(arr: number[]): number[] {
    const n = arr.length;

    for (let i = 0; i < n; ++i) {
        arr[i] += bitCount(arr[i]) * 100000;
    }

    arr.sort((a, b) => a - b);

    for (let i = 0; i < n; ++i) {
        arr[i] %= 100000;
    }

    return arr;
}

function bitCount(i: number): number {
    i = i - ((i >>> 1) & 0x55555555);
    i = (i & 0x33333333) + ((i >>> 2) & 0x33333333);
    i = (i + (i >>> 4)) & 0x0f0f0f0f;
    i = i + (i >>> 8);
    i = i + (i >>> 16);
    return i & 0x3f;
}
```

#### Rust

```rust
impl Solution {
    pub fn sort_by_bits(mut arr: Vec<i32>) -> Vec<i32> {
        let n = arr.len();

        for i in 0..n {
            arr[i] += arr[i].count_ones() as i32 * 100000;
        }

        arr.sort();

        for i in 0..n {
            arr[i] %= 100000;
        }

        arr
    }
}
```

#### C

```c
static int bitCount(int x) {
    int cnt = 0;
    while (x) {
        x &= (x - 1);
        ++cnt;
    }
    return cnt;
}

static int cmp(const void* a, const void* b) {
    int x = *(const int*) a;
    int y = *(const int*) b;

    int cx = bitCount(x);
    int cy = bitCount(y);

    if (cx != cy) {
        return cx - cy;
    }
    return x - y;
}

/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* sortByBits(int* arr, int arrSize, int* returnSize) {
    *returnSize = arrSize;

    int* res = (int*) malloc(sizeof(int) * arrSize);
    if (!res) {
        return NULL;
    }

    for (int i = 0; i < arrSize; ++i) {
        res[i] = arr[i];
    }

    qsort(res, arrSize, sizeof(int), cmp);

    return res;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
