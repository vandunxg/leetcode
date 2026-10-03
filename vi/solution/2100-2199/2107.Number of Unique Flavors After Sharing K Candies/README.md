---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - Sliding Window
---

<!-- problem:start -->

# [2107. Number of Unique Flavors After Sharing K Candies 🔒](https://leetcode.com/problems/number-of-unique-flavors-after-sharing-k-candies)

[中文文档](/solution/2100-2199/2107.Number%20of%20Unique%20Flavors%20After%20Sharing%20K%20Candies/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <strong>0-indexed</strong> <code>candies</code>, trong đó <code>candies[i]</code> biểu thị hương vị của viên kẹo thứ <code>i<sup>th</sup></code>. Mẹ muốn bạn chia số kẹo này cho em gái bằng cách đưa cho em <code>k</code> viên kẹo <strong>liên tiếp</strong>, nhưng bạn muốn giữ lại nhiều hương vị kẹo nhất có thể.</p>

<p>Hãy trả về <em>số lượng <strong>lớn nhất</strong> các hương vị kẹo <strong>khác nhau</strong> mà bạn có thể giữ lại sau khi chia kẹo</em><em> cho em gái.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> candies = [1,<u>2,2,3</u>,4,3], k = 3
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
Đưa cho em các viên kẹo trong phạm vi [1, 3] (bao gồm cả hai đầu) với các hương vị [2,2,3].
Bạn có thể ăn các viên kẹo có hương vị [1,4,3].
Có 3 hương vị khác nhau, vì vậy trả về 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> candies = [2,2,2,<u>2,3</u>,3], k = 2
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Đưa cho em các viên kẹo trong phạm vi [3, 4] (bao gồm cả hai đầu) với các hương vị [2,3].
Bạn có thể ăn các viên kẹo có hương vị [2,2,2,3].
Có 2 hương vị khác nhau, vì vậy trả về 2.
Lưu ý rằng bạn cũng có thể chia các viên kẹo có hương vị [2,2] và ăn các viên kẹo có hương vị [2,2,3,3].
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> candies = [2,4,5], k = 0
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
Bạn không cần đưa cho em viên kẹo nào.
Bạn có thể ăn các viên kẹo có hương vị [2,4,5].
Có 3 hương vị khác nhau, vì vậy trả về 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= candies.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= candies[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= k &lt;= candies.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sliding Window + Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> $k$ viên kẹo được chia tạo thành một đoạn liên tiếp; ta giữ lại phần bù của đoạn đó. Việc xây dựng lại tập hợp hương vị cho từng cửa sổ tốn $O(n)$ mỗi lần và $O(n^2)$ tổng cộng, không đáp ứng được $n\le 10^5$.
>
> Độ dài cửa sổ là cố định, nên mỗi lần dịch cửa sổ, phần bù chỉ thay đổi bởi một viên kẹo ở mỗi đầu. Một frequency map của các hương vị nằm ngoài cửa sổ có kích thước bằng số hương vị khác nhau mà ta giữ lại.
>
> Khởi tạo map bằng $\textit{candies}[k:]$, trượt mọi cửa sổ có độ dài $k$, cập nhật cả hai đầu rồi ghi nhận kích thước lớn nhất của map.

<!-- thinking:end -->

Ta có thể duy trì một cửa sổ trượt có kích thước $k$, trong đó các viên kẹo bên ngoài cửa sổ là phần dành cho ta, còn $k$ viên kẹo bên trong cửa sổ được chia cho em gái và mẹ. Ta có thể dùng một hash table $cnt$ để ghi lại các hương vị của những viên kẹo bên ngoài cửa sổ và số lượng tương ứng của chúng.

Ban đầu, hash table $cnt$ lưu các hương vị của những viên kẹo từ $candies[k]$ đến $candies[n-1]$ cùng số lượng tương ứng. Khi đó, số lượng hương vị kẹo là kích thước của hash table $cnt$, tức là $ans = cnt.size()$.

Tiếp theo, ta duyệt các viên kẹo trong phạm vi $[k,..n-1]$, thêm viên kẹo hiện tại $candies[i]$ vào cửa sổ và đưa viên kẹo $candies[i-k]$ ở bên trái cửa sổ ra ngoài. Sau đó, ta cập nhật hash table $cnt$ và cập nhật số lượng hương vị kẹo $ans$ thành $max(ans, cnt.size())$.

Sau khi duyệt qua tất cả các viên kẹo, ta sẽ nhận được số lượng hương vị kẹo khác nhau lớn nhất có thể giữ lại.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là số lượng viên kẹo.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def shareCandies(self, candies: List[int], k: int) -> int:
        cnt = Counter(candies[k:])
        ans = len(cnt)
        for i in range(k, len(candies)):
            cnt[candies[i - k]] += 1
            cnt[candies[i]] -= 1
            if cnt[candies[i]] == 0:
                cnt.pop(candies[i])
            ans = max(ans, len(cnt))
        return ans
```

#### Java

```java
class Solution {
    public int shareCandies(int[] candies, int k) {
        Map<Integer, Integer> cnt = new HashMap<>();
        int n = candies.length;
        for (int i = k; i < n; ++i) {
            cnt.merge(candies[i], 1, Integer::sum);
        }
        int ans = cnt.size();
        for (int i = k; i < n; ++i) {
            cnt.merge(candies[i - k], 1, Integer::sum);
            if (cnt.merge(candies[i], -1, Integer::sum) == 0) {
                cnt.remove(candies[i]);
            }
            ans = Math.max(ans, cnt.size());
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int shareCandies(vector<int>& candies, int k) {
        unordered_map<int, int> cnt;
        int n = candies.size();
        for (int i = k; i < n; ++i) {
            ++cnt[candies[i]];
        }
        int ans = cnt.size();
        for (int i = k; i < n; ++i) {
            ++cnt[candies[i - k]];
            if (--cnt[candies[i]] == 0) {
                cnt.erase(candies[i]);
            }
            ans = max(ans, (int) cnt.size());
        }
        return ans;
    }
};
```

#### Go

```go
func shareCandies(candies []int, k int) (ans int) {
	cnt := map[int]int{}
	for _, c := range candies[k:] {
		cnt[c]++
	}
	ans = len(cnt)
	for i := k; i < len(candies); i++ {
		cnt[candies[i-k]]++
		cnt[candies[i]]--
		if cnt[candies[i]] == 0 {
			delete(cnt, candies[i])
		}
		ans = max(ans, len(cnt))
	}
	return
}
```

#### TypeScript

```ts
function shareCandies(candies: number[], k: number): number {
    const cnt: Map<number, number> = new Map();
    for (const x of candies.slice(k)) {
        cnt.set(x, (cnt.get(x) || 0) + 1);
    }
    let ans = cnt.size;
    for (let i = k; i < candies.length; ++i) {
        cnt.set(candies[i - k], (cnt.get(candies[i - k]) || 0) + 1);
        cnt.set(candies[i], (cnt.get(candies[i]) || 0) - 1);
        if (cnt.get(candies[i]) === 0) {
            cnt.delete(candies[i]);
        }
        ans = Math.max(ans, cnt.size);
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn share_candies(candies: Vec<i32>, k: i32) -> i32 {
        let mut cnt = HashMap::new();
        let n = candies.len();

        for i in k as usize..n {
            *cnt.entry(candies[i]).or_insert(0) += 1;
        }

        let mut ans = cnt.len() as i32;

        for i in k as usize..n {
            *cnt.entry(candies[i - (k as usize)]).or_insert(0) += 1;
            if let Some(x) = cnt.get_mut(&candies[i]) {
                *x -= 1;
                if *x == 0 {
                    cnt.remove(&candies[i]);
                }
            }

            ans = ans.max(cnt.len() as i32);
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
