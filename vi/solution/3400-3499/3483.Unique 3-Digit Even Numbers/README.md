---
comments: true
difficulty: Easy
rating: 1323
source: Biweekly Contest 152 Q1
tags:
    - Recursion
    - Array
    - Hash Table
    - Enumeration
---

<!-- problem:start -->

# [3483. Unique 3-Digit Even Numbers](https://leetcode.com/problems/unique-3-digit-even-numbers)

[中文文档](/solution/3400-3499/3483.Unique%203-Digit%20Even%20Numbers/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng các chữ số có tên là <code>digits</code>. Nhiệm vụ của bạn là xác định số lượng số chẵn có ba chữ số <strong>khác nhau</strong> có thể được tạo từ các chữ số này.</p>

<p><strong>Lưu ý</strong>: Mỗi <em>bản sao</em> của một chữ số chỉ có thể được sử dụng <strong>một lần trong mỗi số</strong>, và <strong>không được có số 0 ở đầu</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">digits = [1,2,3,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">12</span></p>

<p><strong>Giải thích:</strong> 12 số chẵn có ba chữ số khác nhau có thể tạo được là 124, 132, 134, 142, 214, 234, 312, 314, 324, 342, 412 và 432. Lưu ý rằng không thể tạo 222 vì chỉ có 1 bản sao của chữ số 2.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">digits = [0,2,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong> Hai số chẵn có ba chữ số duy nhất có thể tạo được là 202 và 220. Lưu ý rằng chữ số 2 có thể được sử dụng hai lần vì nó xuất hiện hai lần trong mảng.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">digits = [6,6,6]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong> Chỉ có thể tạo 666.</p>
</div>

<p><strong class="example">Ví dụ 4:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">digits = [1,3,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong> Không thể tạo số chẵn nào có ba chữ số.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>3 &lt;= digits.length &lt;= 10</code></li>
    <li><code>0 &lt;= digits[i] &lt;= 9</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Set + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Ta tạo các số chẵn có ba chữ số từ nhiều nhất $10$ chữ số với các chỉ số khác nhau, rồi đếm các giá trị khác nhau. $n^3\le 10^3$, nên ba vòng lặp là đủ.
>
> Chữ số hàng đơn vị phải chẵn, chữ số hàng trăm không thể là $0$, và ba chỉ số phải khác nhau. Một hash set loại bỏ các bộ ba chỉ số khác nhau nhưng tạo ra cùng một số.
>
> Liệt kê chữ số hàng đơn vị chẵn $a$, sau đó chọn hàng chục $b$ và hàng trăm $c$, rồi thêm $100c+10b+a$ vào hash set.

<!-- thinking:end -->

Chúng ta sử dụng một hash set $\textit{s}$ để lưu tất cả các số chẵn có ba chữ số khác nhau, sau đó liệt kê mọi số chẵn có ba chữ số có thể tạo được để thêm vào hash set.

Cuối cùng, chúng ta trả về kích thước của hash set.

Độ phức tạp thời gian là $O(n^3)$, và độ phức tạp không gian là $O(n^3)$. Trong đó $n$ là độ dài của mảng $\textit{digits}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def totalNumbers(self, digits: List[int]) -> int:
        s = set()
        for i, a in enumerate(digits):
            if a & 1:
                continue
            for j, b in enumerate(digits):
                if i == j:
                    continue
                for k, c in enumerate(digits):
                    if c == 0 or k in (i, j):
                        continue
                    s.add(c * 100 + b * 10 + a)
        return len(s)
```

#### Java

```java
class Solution {
    public int totalNumbers(int[] digits) {
        Set<Integer> s = new HashSet<>();
        int n = digits.length;
        for (int i = 0; i < n; ++i) {
            if (digits[i] % 2 == 1) {
                continue;
            }
            for (int j = 0; j < n; ++j) {
                if (i == j) {
                    continue;
                }
                for (int k = 0; k < n; ++k) {
                    if (digits[k] == 0 || k == i || k == j) {
                        continue;
                    }
                    s.add(digits[k] * 100 + digits[j] * 10 + digits[i]);
                }
            }
        }
        return s.size();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int totalNumbers(vector<int>& digits) {
        unordered_set<int> s;
        int n = digits.size();
        for (int i = 0; i < n; ++i) {
            if (digits[i] % 2 == 1) {
                continue;
            }
            for (int j = 0; j < n; ++j) {
                if (i == j) {
                    continue;
                }
                for (int k = 0; k < n; ++k) {
                    if (digits[k] == 0 || k == i || k == j) {
                        continue;
                    }
                    s.insert(digits[k] * 100 + digits[j] * 10 + digits[i]);
                }
            }
        }
        return s.size();
    }
};
```

#### Go

```go
func totalNumbers(digits []int) int {
    s := make(map[int]struct{})
    for i, a := range digits {
        if a%2 == 1 {
            continue
        }
        for j, b := range digits {
            if i == j {
                continue
            }
            for k, c := range digits {
                if c == 0 || k == i || k == j {
                    continue
                }
                s[c*100+b*10+a] = struct{}{}
            }
        }
    }
    return len(s)
}
```

#### TypeScript

```ts
function totalNumbers(digits: number[]): number {
    const s = new Set<number>();
    const n = digits.length;
    for (let i = 0; i < n; ++i) {
        if (digits[i] % 2 === 1) {
            continue;
        }
        for (let j = 0; j < n; ++j) {
            if (i === j) {
                continue;
            }
            for (let k = 0; k < n; ++k) {
                if (digits[k] === 0 || k === i || k === j) {
                    continue;
                }
                s.add(digits[k] * 100 + digits[j] * 10 + digits[i]);
            }
        }
    }
    return s.size;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
