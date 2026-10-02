---
comments: true
difficulty: Medium
rating: 1405
source: Weekly Contest 203 Q2
tags:
    - Greedy
    - Array
    - Math
    - Game Theory
    - Sorting
---

<!-- problem:start -->

# [1561. Maximum Number of Coins You Can Get](https://leetcode.com/problems/maximum-number-of-coins-you-can-get)

[中文文档](/solution/1500-1599/1561.Maximum%20Number%20of%20Coins%20You%20Can%20Get/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>3n</code> đống xu với kích thước khác nhau, bạn và bạn bè sẽ lấy các đống xu như sau:</p>

<ul>
	<li>Ở mỗi bước, bạn chọn <strong>bất kỳ </strong><code>3</code> đống xu nào (không nhất thiết liên tiếp).</li>
	<li>Trong ba đống đã chọn, Alice lấy đống có nhiều xu nhất.</li>
	<li>Bạn lấy đống có nhiều xu thứ hai.</li>
	<li>Bob, bạn của bạn, lấy đống còn lại.</li>
	<li>Lặp lại cho đến khi không còn đống xu nào.</li>
</ul>

<p>Cho mảng số nguyên <code>piles</code>, trong đó <code>piles[i]</code> là số xu trong đống thứ <code>i<sup>th</sup></code>.</p>

<p>Trả về số xu lớn nhất bạn có thể nhận được.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> piles = [2,4,1,2,7,8]
<strong>Output:</strong> 9
<strong>Giải thích: </strong>Chọn bộ ba (2, 7, 8), Alice lấy đống có 8 xu, bạn lấy đống có <strong>7</strong> xu và Bob lấy đống còn lại.
Chọn bộ ba (1, 2, 4), Alice lấy đống có 4 xu, bạn lấy đống có <strong>2</strong> xu và Bob lấy đống còn lại.
Số xu lớn nhất bạn có thể nhận là: 7 + 2 = 9.
Nếu chọn cách sắp xếp (1, <strong>2</strong>, 8), (2, <strong>4</strong>, 7), bạn chỉ nhận được 2 + 4 = 6 xu, không tối ưu.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> piles = [2,4,5]
<strong>Output:</strong> 4
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> piles = [9,8,7,6,5,1,2,3,4]
<strong>Output:</strong> 18
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= piles.length &lt;= 10<sup>5</sup></code></li>
	<li><code>piles.length % 3 == 0</code></li>
	<li><code>1 &lt;= piles[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi vòng, ba người lần lượt lấy một đống: Alice lấy đống lớn nhất hiện tại, ta lấy đống lớn thứ hai, Bob lấy đống nhỏ nhất. $n$ có thể đạt $10^5$, nên không thể mô phỏng từng phép so sánh.
>
> Sau khi sắp xếp, Bob luôn nhận một phần ba nhỏ nhất. Các đống còn lại lần lượt thuộc về Alice và ta; các đống của ta là cách một phần tử, bắt đầu từ chỉ số $n/3$. Cộng chúng lại.

<!-- thinking:end -->

Để tối đa hóa số xu nhận được, ta tham lam cho Bob lấy $n$ đống nhỏ nhất. Mỗi lần, Alice lấy đống lớn nhất, sau đó ta lấy đống lớn thứ hai, cứ như vậy cho đến khi không còn xu.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(\log n)$. Ở đây, $n$ là số đống xu.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxCoins(self, piles: List[int]) -> int:
        piles.sort()
        return sum(piles[len(piles) // 3 :][::2])
```

#### Java

```java
class Solution {
    public int maxCoins(int[] piles) {
        Arrays.sort(piles);
        int ans = 0;
        for (int i = piles.length / 3; i < piles.length; i += 2) {
            ans += piles[i];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxCoins(vector<int>& piles) {
        ranges::sort(piles);
        int ans = 0;
        for (int i = piles.size() / 3; i < piles.size(); i += 2) {
            ans += piles[i];
        }
        return ans;
    }
};
```

#### Go

```go
func maxCoins(piles []int) (ans int) {
	sort.Ints(piles)
	for i := len(piles) / 3; i < len(piles); i += 2 {
		ans += piles[i]
	}
	return
}
```

#### TypeScript

```ts
function maxCoins(piles: number[]): number {
    piles.sort((a, b) => a - b);
    let ans = 0;
    for (let i = piles.length / 3; i < piles.length; i += 2) {
        ans += piles[i];
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_coins(mut piles: Vec<i32>) -> i32 {
        piles.sort();
        let mut ans = 0;
        for i in (piles.len() / 3..piles.len()).step_by(2) {
            ans += piles[i];
        }
        ans
    }
}
```

#### C

```c
int compare(const void* a, const void* b) {
    return (*(int*) a - *(int*) b);
}

int maxCoins(int* piles, int pilesSize) {
    qsort(piles, pilesSize, sizeof(int), compare);
    int ans = 0;
    for (int i = pilesSize / 3; i < pilesSize; i += 2) {
        ans += piles[i];
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
