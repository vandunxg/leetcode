---
comments: true
difficulty: Medium
rating: 1688
source: Weekly Contest 385 Q2
tags:
    - Trie
    - Array
    - Hash Table
    - String
---

<!-- problem:start -->

# [3043. Find the Length of the Longest Common Prefix](https://leetcode.com/problems/find-the-length-of-the-longest-common-prefix)

[中文文档](/solution/3000-3099/3043.Find%20the%20Length%20of%20the%20Longest%20Common%20Prefix/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <strong>dương</strong> <code>arr1</code> và <code>arr2</code>.</p>

<p><strong>Tiền tố</strong> của một số nguyên dương là một số nguyên được tạo thành từ một hoặc nhiều chữ số của nó, bắt đầu từ chữ số <strong>bên trái nhất</strong>. Ví dụ, <code>123</code> là tiền tố của số nguyên <code>12345</code>, còn <code>234</code> thì <strong>không phải</strong>.</p>

<p><strong>Tiền tố chung</strong> của hai số nguyên <code>a</code> và <code>b</code> là một số nguyên <code>c</code> sao cho <code>c</code> là tiền tố của cả <code>a</code> và <code>b</code>. Ví dụ, <code>5655359</code> và <code>56554</code> có các tiền tố chung là <code>565</code> và <code>5655</code>, còn <code>1223</code> và <code>43456</code> thì <strong>không</strong> có tiền tố chung.</p>

<p>Hãy tìm độ dài của <strong>tiền tố chung dài nhất</strong> giữa mọi cặp số nguyên <code>(x, y)</code> sao cho <code>x</code> thuộc <code>arr1</code> và <code>y</code> thuộc <code>arr2</code>.</p>

<p>Trả về <em>độ dài của tiền tố chung <strong>dài nhất</strong> trong mọi cặp</em>.<em> Nếu không tồn tại tiền tố chung nào</em>, <em>hãy trả về</em> <code>0</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> arr1 = [1,10,100], arr2 = [1000]
<strong>Output:</strong> 3
<strong>Giải thích:</strong> Có 3 cặp (arr1[i], arr2[j]):
- Tiền tố chung dài nhất của (1, 1000) là 1.
- Tiền tố chung dài nhất của (10, 1000) là 10.
- Tiền tố chung dài nhất của (100, 1000) là 100.
Tiền tố chung dài nhất là 100, có độ dài bằng 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> arr1 = [1,2,3], arr2 = [4,4,4]
<strong>Output:</strong> 0
<strong>Giải thích:</strong> Không tồn tại tiền tố chung nào cho bất kỳ cặp (arr1[i], arr2[j]) nào, vì vậy ta trả về 0.
Lưu ý rằng tiền tố chung giữa các phần tử trong cùng một mảng không được tính.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr1.length, arr2.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= arr1[i], arr2[i] &lt;= 10<sup>8</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Các số nguyên được đọc dưới dạng chuỗi thập phân; ta cần tìm tiền tố chung dài nhất giữa hai mảng. Vì $n \le 5 \times 10^4$, ta không thể so sánh mọi cặp.
>
> Mọi tiền tố của một số đều là các giá trị nhận được bằng cách liên tục chia số đó cho $10$. Sau khi lưu các tiền tố của $\textit{arr}_1$ vào một hash set, với mỗi giá trị trong $\textit{arr}_2$, ta tìm từ chính giá trị đó về các tiền tố ngắn hơn.
>
> Giá trị lớn nhất tìm thấy là số biểu diễn tiền tố dài nhất; số chữ số của nó chính là đáp án.

<!-- thinking:end -->

Ta có thể dùng một hash table để lưu tất cả tiền tố của các số trong `arr1`. Sau đó, ta duyệt qua mọi số $x$ trong `arr2`. Với mỗi số $x$, ta bắt đầu từ chữ số cao nhất và giảm dần, kiểm tra xem giá trị đó có tồn tại trong hash table hay không. Nếu có, ta đã tìm thấy một tiền tố chung và có thể cập nhật đáp án tương ứng.

Độ phức tạp thời gian là $O(m \times \log M + n \times \log N)$, độ phức tạp không gian là $O(m \times \log M)$. Trong đó, $m$ và $n$ lần lượt là độ dài của `arr1` và `arr2`, còn $M$ và $N$ lần lượt là giá trị lớn nhất trong `arr1` và `arr2`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestCommonPrefix(self, arr1: List[int], arr2: List[int]) -> int:
        s = set()
        for x in arr1:
            while x:
                s.add(x)
                x //= 10
        mx = 0
        for x in arr2:
            while x:
                if x in s:
                    mx = max(mx, x)
                    break
                x //= 10
        return len(str(mx)) if mx else 0
```

#### Java

```java
class Solution {
    public int longestCommonPrefix(int[] arr1, int[] arr2) {
        Set<Integer> s = new HashSet<>();
        for (int x : arr1) {
            for (; x > 0; x /= 10) {
                s.add(x);
            }
        }
        int mx = 0;
        for (int x : arr2) {
            for (; x > 0; x /= 10) {
                if (s.contains(x)) {
                    mx = Math.max(mx, x);
                    break;
                }
            }
        }
        return mx > 0 ? String.valueOf(mx).length() : 0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int longestCommonPrefix(vector<int>& arr1, vector<int>& arr2) {
        unordered_set<int> s;
        for (int x : arr1) {
            for (; x; x /= 10) {
                s.insert(x);
            }
        }
        int mx = 0;
        for (int x : arr2) {
            for (; x; x /= 10) {
                if (s.count(x)) {
                    mx = max(mx, x);
                    break;
                }
            }
        }
        return mx > 0 ? (int) log10(mx) + 1 : 0;
    }
};
```

#### Go

```go
func longestCommonPrefix(arr1 []int, arr2 []int) int {
	s := map[int]bool{}
	for _, x := range arr1 {
		for ; x > 0; x /= 10 {
			s[x] = true
		}
	}
	mx := 0
	for _, x := range arr2 {
		for ; x > 0; x /= 10 {
			if s[x] {
				mx = max(mx, x)
				break
			}
		}
	}
	if mx > 0 {
		return len(strconv.Itoa(mx))
	}
	return 0
}
```

#### TypeScript

```ts
function longestCommonPrefix(arr1: number[], arr2: number[]): number {
    const s: Set<number> = new Set<number>();
    for (let x of arr1) {
        for (; x; x = Math.floor(x / 10)) {
            s.add(x);
        }
    }
    let mx: number = 0;
    for (let x of arr2) {
        for (; x; x = Math.floor(x / 10)) {
            if (s.has(x)) {
                mx = Math.max(mx, x);
                break;
            }
        }
    }
    return mx > 0 ? Math.floor(Math.log10(mx)) + 1 : 0;
}
```

#### Rust

```rust
impl Solution {
    pub fn longest_common_prefix(arr1: Vec<i32>, arr2: Vec<i32>) -> i32 {
        let mut s = std::collections::HashSet::new();
        for x in arr1 {
            let mut y = x;
            while y > 0 {
                s.insert(y);
                y /= 10;
            }
        }
        let mut mx = 0;
        for x in arr2 {
            let mut y = x;
            while y > 0 {
                if s.contains(&y) {
                    mx = mx.max(y);
                    break;
                }
                y /= 10;
            }
        }
        if mx > 0 {
            (mx as f64).log10().floor() as i32 + 1
        } else {
            0
        }
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} arr1
 * @param {number[]} arr2
 * @return {number}
 */
var longestCommonPrefix = function (arr1, arr2) {
    const s = new Set();
    for (let x of arr1) {
        for (; x; x = Math.floor(x / 10)) {
            s.add(x);
        }
    }
    let mx = 0;
    for (let x of arr2) {
        for (; x; x = Math.floor(x / 10)) {
            if (s.has(x)) {
                mx = Math.max(mx, x);
                break;
            }
        }
    }
    return mx > 0 ? Math.floor(Math.log10(mx)) + 1 : 0;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
