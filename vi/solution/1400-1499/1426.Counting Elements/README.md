---
comments: true
difficulty: Easy
tags:
    - Array
    - Hash Table
---

<!-- problem:start -->

# [1426. Counting Elements 🔒](https://leetcode.com/problems/counting-elements)

[中文文档](/solution/1400-1499/1426.Counting%20Elements/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>arr</code>, hãy đếm số phần tử <code>x</code> sao cho <code>x + 1</code> cũng có trong <code>arr</code>. Nếu các phần tử trong <code>arr</code> bị trùng, hãy đếm chúng riêng biệt.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> arr = [1,2,3]
<strong>Output:</strong> 2
<strong>Giải thích:</strong> 1 và 2 được đếm vì 2 và 3 có trong arr.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> arr = [1,1,3,3,5,5,7,7]
<strong>Output:</strong> 0
<strong>Giải thích:</strong> Không có số nào được đếm vì 2, 4, 6 và 8 không có trong arr.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 1000</code></li>
	<li><code>0 &lt;= arr[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> $n\le 1000$ cho phép duyệt tìm $x+1$ với từng phần tử. Sau khi đếm, với mỗi $x$ đã xuất hiện, ta cộng tần suất của nó nếu $x+1$ cũng xuất hiện.

<!-- thinking:end -->

Ta có thể dùng hash table hoặc mảng $cnt$ để ghi lại tần suất của mỗi số trong mảng $arr$. Sau đó, ta duyệt từng số $x$ trong $cnt$. Nếu $x+1$ cũng tồn tại trong $cnt$, ta cộng $cnt[x]$ vào đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $arr$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countElements(self, arr: List[int]) -> int:
        cnt = Counter(arr)
        return sum(v for x, v in cnt.items() if cnt[x + 1])
```

#### Java

```java
class Solution {
    public int countElements(int[] arr) {
        int[] cnt = new int[1001];
        for (int x : arr) {
            ++cnt[x];
        }
        int ans = 0;
        for (int x = 0; x < 1000; ++x) {
            if (cnt[x + 1] > 0) {
                ans += cnt[x];
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
    int countElements(vector<int>& arr) {
        int cnt[1001]{};
        for (int x : arr) {
            ++cnt[x];
        }
        int ans = 0;
        for (int x = 0; x < 1000; ++x) {
            if (cnt[x + 1]) {
                ans += cnt[x];
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countElements(arr []int) (ans int) {
	mx := slices.Max(arr)
	cnt := make([]int, mx+1)
	for _, x := range arr {
		cnt[x]++
	}
	for x := 0; x < mx; x++ {
		if cnt[x+1] > 0 {
			ans += cnt[x]
		}
	}
	return
}
```

#### TypeScript

```ts
function countElements(arr: number[]): number {
    const mx = Math.max(...arr);
    const cnt = Array(mx + 1).fill(0);
    for (const x of arr) {
        ++cnt[x];
    }
    let ans = 0;
    for (let i = 0; i < mx; ++i) {
        if (cnt[i + 1] > 0) {
            ans += cnt[i];
        }
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn count_elements(arr: Vec<i32>) -> i32 {
        let mut cnt = HashMap::new();
        for &num in &arr {
            *cnt.entry(num).or_insert(0) += 1;
        }
        cnt.iter()
            .filter(|(&x, _)| cnt.contains_key(&(x + 1)))
            .map(|(_, &v)| v)
            .sum()
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} arr
 * @return {number}
 */
var countElements = function (arr) {
    const mx = Math.max(...arr);
    const cnt = Array(mx + 1).fill(0);
    for (const x of arr) {
        ++cnt[x];
    }
    let ans = 0;
    for (let i = 0; i < mx; ++i) {
        if (cnt[i + 1] > 0) {
            ans += cnt[i];
        }
    }
    return ans;
};
```

#### PHP

```php
class Solution {
    /**
     * @param Integer[] $arr
     * @return Integer
     */
    function countElements($arr) {
        $cnt = array_count_values($arr);
        $ans = 0;
        foreach ($cnt as $x => $v) {
            if (isset($cnt[$x + 1])) {
                $ans += $v;
            }
        }
        return $ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
