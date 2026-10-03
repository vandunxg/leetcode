---
comments: true
difficulty: Medium
rating: 1366
source: Weekly Contest 314 Q2
tags:
    - Bit Manipulation
    - Array
---

<!-- problem:start -->

# [2433. Find The Original Array of Prefix Xor](https://leetcode.com/problems/find-the-original-array-of-prefix-xor)

[中文文档](/solution/2400-2499/2433.Find%20The%20Original%20Array%20of%20Prefix%20Xor/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <strong>số nguyên</strong> <code>pref</code> có kích thước <code>n</code>. Hãy tìm và trả về <em>mảng </em><code>arr</code><em> có kích thước </em><code>n</code><em> thỏa mãn</em>:</p>

<ul>
	<li><code>pref[i] = arr[0] ^ arr[1] ^ ... ^ arr[i]</code>.</li>
</ul>

<p>Lưu ý rằng <code>^</code> biểu thị phép <strong>XOR theo bit</strong>.</p>

<p>Có thể chứng minh rằng đáp án là <strong>duy nhất</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> pref = [5,2,0,3,1]
<strong>Đầu ra:</strong> [5,7,2,3,2]
<strong>Giải thích:</strong> Từ mảng [5,7,2,3,2], ta có:
- pref[0] = 5.
- pref[1] = 5 ^ 7 = 2.
- pref[2] = 5 ^ 7 ^ 2 = 0.
- pref[3] = 5 ^ 7 ^ 2 ^ 3 = 3.
- pref[4] = 5 ^ 7 ^ 2 ^ 3 ^ 2 = 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> pref = [13]
<strong>Đầu ra:</strong> [13]
<strong>Giải thích:</strong> Ta có pref[0] = arr[0] = 13.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= pref.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= pref[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bit Manipulation

<!-- thinking:start -->

> **Tư duy**
>
> $pref[i]$ là phép XOR của $arr[0..i]$. Khi đó $pref[i]\oplus pref[i-1]=arr[i]$ với $pref[-1]=0$, nên mảng ban đầu chính là phép XOR giữa các prefix liền kề. Chỉ cần duyệt mảng một lần với $n\le 10^5$.

<!-- thinking:end -->

Theo đề bài, ta có phương trình thứ nhất:

$$
pref[i]=arr[0] \oplus arr[1] \oplus \cdots \oplus arr[i]
$$

Do đó, ta cũng có phương trình thứ hai:

$$
pref[i-1]=arr[0] \oplus arr[1] \oplus \cdots \oplus arr[i-1]
$$

Ta thực hiện phép XOR theo bit trên phương trình thứ nhất và thứ hai, thu được:

$$
pref[i] \oplus pref[i-1]=arr[i]
$$

Điều đó có nghĩa là mỗi phần tử trong mảng đáp án được tạo ra bằng cách thực hiện phép XOR theo bit trên hai phần tử liền kề trong mảng XOR tiền tố.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng XOR tiền tố. Không tính phần bộ nhớ dành cho mảng đáp án, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findArray(self, pref: List[int]) -> List[int]:
        return [a ^ b for a, b in pairwise([0] + pref)]
```

#### Java

```java
class Solution {
    public int[] findArray(int[] pref) {
        int n = pref.length;
        int[] ans = new int[n];
        ans[0] = pref[0];
        for (int i = 1; i < n; ++i) {
            ans[i] = pref[i - 1] ^ pref[i];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> findArray(vector<int>& pref) {
        int n = pref.size();
        vector<int> ans = {pref[0]};
        for (int i = 1; i < n; ++i) {
            ans.push_back(pref[i - 1] ^ pref[i]);
        }
        return ans;
    }
};
```

#### Go

```go
func findArray(pref []int) []int {
	n := len(pref)
	ans := []int{pref[0]}
	for i := 1; i < n; i++ {
		ans = append(ans, pref[i-1]^pref[i])
	}
	return ans
}
```

#### TypeScript

```ts
function findArray(pref: number[]): number[] {
    let ans = pref.slice();
    for (let i = 1; i < pref.length; i++) {
        ans[i] = pref[i - 1] ^ pref[i];
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn find_array(pref: Vec<i32>) -> Vec<i32> {
        let n = pref.len();
        let mut res = vec![0; n];
        res[0] = pref[0];
        for i in 1..n {
            res[i] = pref[i] ^ pref[i - 1];
        }
        res
    }
}
```

#### C

```c
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* findArray(int* pref, int prefSize, int* returnSize) {
    int* res = (int*) malloc(sizeof(int) * prefSize);
    res[0] = pref[0];
    for (int i = 1; i < prefSize; i++) {
        res[i] = pref[i - 1] ^ pref[i];
    }
    *returnSize = prefSize;
    return res;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
