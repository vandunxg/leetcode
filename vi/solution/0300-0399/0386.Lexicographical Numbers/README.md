---
comments: true
difficulty: Medium
tags:
    - Depth-First Search
    - Trie
---

<!-- problem:start -->

# [386. Lexicographical Numbers](https://leetcode.com/problems/lexicographical-numbers)

[中文文档](/solution/0300-0399/0386.Lexicographical%20Numbers/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên <code>n</code>, hãy trả về tất cả các số trong khoảng <code>[1, n]</code> được sắp xếp theo thứ tự từ điển.</p>

<p>Bạn cần viết thuật toán chạy trong thời gian <code>O(n)</code> và sử dụng <code>O(1)</code> bộ nhớ phụ.&nbsp;</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> n = 13
<strong>Đầu ra:</strong> [1,10,11,12,13,2,3,4,5,6,7,8,9]
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> n = 2
<strong>Đầu ra:</strong> [1,2]
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 5 * 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt lặp

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần liệt kê $[1,n]$ theo thứ tự từ điển. Sắp xếp các chuỗi sẽ tốn $O(n\log n\cdot \log n)$. Thứ tự preorder trên trie 10 nhánh chính là thứ tự cần tìm.
>
> Bắt đầu tại $1$: chuyển đến $v\times 10$ nếu giá trị này không vượt quá $n$; nếu không, lùi lại bằng cách chia $v$ cho $10$ khi chữ số cuối là $9$ hoặc $v+1>n$, rồi tăng $v$. Sau $n$ lượt, mỗi giá trị được đưa vào kết quả đúng một lần.

<!-- thinking:end -->

Đầu tiên, ta khai báo biến $v$ và khởi tạo bằng $1$. Sau đó, lặp từ $1$, mỗi lượt thêm $v$ vào mảng kết quả. Nếu $v \times 10 \leq n$, cập nhật $v$ thành $v \times 10$; nếu không, khi $v \bmod 10 = 9$ hoặc $v + 1 > n$, ta liên tục chia $v$ cho $10$. Khi vòng lặp này kết thúc, tăng $v$ lên $1$. Tiếp tục cho đến khi đã thêm đủ $n$ số vào mảng kết quả.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là số nguyên đầu vào. Không tính phần bộ nhớ của mảng kết quả, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def lexicalOrder(self, n: int) -> List[int]:
        ans = []
        v = 1
        for _ in range(n):
            ans.append(v)
            if v * 10 <= n:
                v *= 10
            else:
                while v % 10 == 9 or v + 1 > n:
                    v //= 10
                v += 1
        return ans
```

#### Java

```java
class Solution {
    public List<Integer> lexicalOrder(int n) {
        List<Integer> ans = new ArrayList<>(n);
        int v = 1;
        for (int i = 0; i < n; ++i) {
            ans.add(v);
            if (v * 10 <= n) {
                v *= 10;
            } else {
                while (v % 10 == 9 || v + 1 > n) {
                    v /= 10;
                }
                ++v;
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
    vector<int> lexicalOrder(int n) {
        vector<int> ans;
        int v = 1;
        for (int i = 0; i < n; ++i) {
            ans.push_back(v);
            if (v * 10 <= n) {
                v *= 10;
            } else {
                while (v % 10 == 9 || v + 1 > n) {
                    v /= 10;
                }
                ++v;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func lexicalOrder(n int) (ans []int) {
	v := 1
	for i := 0; i < n; i++ {
		ans = append(ans, v)
		if v*10 <= n {
			v *= 10
		} else {
			for v%10 == 9 || v+1 > n {
				v /= 10
			}
			v++
		}
	}
	return
}
```

#### TypeScript

```ts
function lexicalOrder(n: number): number[] {
    const ans: number[] = [];
    let v = 1;
    for (let i = 0; i < n; ++i) {
        ans.push(v);
        if (v * 10 <= n) {
            v *= 10;
        } else {
            while (v % 10 === 9 || v === n) {
                v = Math.floor(v / 10);
            }
            ++v;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn lexical_order(n: i32) -> Vec<i32> {
        let mut ans = Vec::with_capacity(n as usize);
        let mut v = 1;
        for _ in 0..n {
            ans.push(v);
            if v * 10 <= n {
                v *= 10;
            } else {
                while v % 10 == 9 || v + 1 > n {
                    v /= 10;
                }
                v += 1;
            }
        }
        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {number} n
 * @return {number[]}
 */
var lexicalOrder = function (n) {
    const ans = [];
    let v = 1;
    for (let i = 0; i < n; ++i) {
        ans.push(v);
        if (v * 10 <= n) {
            v *= 10;
        } else {
            while (v % 10 === 9 || v === n) {
                v = Math.floor(v / 10);
            }
            ++v;
        }
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
