---
comments: true
difficulty: Easy
rating: 1118
source: Weekly Contest 182 Q1
tags:
    - Array
    - Hash Table
    - Counting
---

<!-- problem:start -->

# [1394. Find Lucky Integer in an Array](https://leetcode.com/problems/find-lucky-integer-in-an-array)

[中文文档](/solution/1300-1399/1394.Find%20Lucky%20Integer%20in%20an%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>arr</code>, một <strong>số may mắn</strong> là số có tần suất xuất hiện trong mảng bằng chính giá trị của nó.</p>

<p>Trả về <em><strong>số may mắn</strong> lớn nhất trong mảng</em>. Nếu không có <strong>số may mắn</strong> nào, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [2,2,3,4]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Số may mắn duy nhất trong mảng là 2 vì frequency[2] == 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1,2,2,3,3,3]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> 1, 2 và 3 đều là số may mắn; hãy trả về số lớn nhất trong các số đó.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [2,2,2,3,3]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Mảng không có số may mắn nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 500</code></li>
	<li><code>1 &lt;= arr[i] &lt;= 500</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Số may mắn xuất hiện đúng số lần bằng giá trị của nó; ta cần tìm số lớn nhất. Sau khi đếm, lấy giá trị $x$ lớn nhất thỏa mãn $x=v$, hoặc trả về $-1$ nếu không có số nào.

<!-- thinking:end -->

Ta có thể dùng hash table hoặc mảng $\textit{cnt}$ để đếm số lần xuất hiện của từng số trong $\textit{arr}$. Sau đó, duyệt $\textit{cnt}$ để tìm $x$ lớn nhất sao cho $\textit{cnt}[x] = x$. Nếu không có $x$ nào như vậy, trả về $-1$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của $\textit{arr}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findLucky(self, arr: List[int]) -> int:
        cnt = Counter(arr)
        return max((x for x, v in cnt.items() if x == v), default=-1)
```

#### Java

```java
class Solution {
    public int findLucky(int[] arr) {
        int[] cnt = new int[501];
        for (int x : arr) {
            ++cnt[x];
        }
        for (int x = cnt.length - 1; x > 0; --x) {
            if (x == cnt[x]) {
                return x;
            }
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findLucky(vector<int>& arr) {
        int cnt[501]{};
        for (int x : arr) {
            ++cnt[x];
        }
        for (int x = 500; x; --x) {
            if (x == cnt[x]) {
                return x;
            }
        }
        return -1;
    }
};
```

#### Go

```go
func findLucky(arr []int) int {
	cnt := [501]int{}
	for _, x := range arr {
		cnt[x]++
	}
	for x := len(cnt) - 1; x > 0; x-- {
		if x == cnt[x] {
			return x
		}
	}
	return -1
}
```

#### TypeScript

```ts
function findLucky(arr: number[]): number {
    const cnt: number[] = Array(501).fill(0);
    for (const x of arr) {
        ++cnt[x];
    }
    for (let x = cnt.length - 1; x; --x) {
        if (x === cnt[x]) {
            return x;
        }
    }
    return -1;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn find_lucky(arr: Vec<i32>) -> i32 {
        let mut cnt = HashMap::new();
        arr.iter().for_each(|&x| *cnt.entry(x).or_insert(0) += 1);
        cnt.iter()
            .filter(|(&x, &v)| x == v)
            .map(|(&x, _)| x)
            .max()
            .unwrap_or(-1)
    }
}
```

#### PHP

```php
class Solution {
    /**
     * @param Integer[] $arr
     * @return Integer
     */
    function findLucky($arr) {
        $cnt = array_fill(0, 501, 0);
        foreach ($arr as $x) {
            $cnt[$x]++;
        }
        for ($x = 500; $x > 0; $x--) {
            if ($cnt[$x] === $x) {
                return $x;
            }
        }
        return -1;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
