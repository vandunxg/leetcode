---
comments: true
difficulty: Medium
tags:
    - Union Find
    - Counting
    - Interactive
---

<!-- problem:start -->

# [2782. Number of Unique Categories 🔒](https://leetcode.com/problems/number-of-unique-categories)

[Tài liệu tiếng Trung](/solution/2700-2799/2782.Number%20of%20Unique%20Categories/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>n</code> và một đối tượng <code>categoryHandler</code> thuộc lớp <code>CategoryHandler</code>.</p>

<p>Có <code>n&nbsp;</code>phần tử, được đánh số từ <code>0</code> đến <code>n - 1</code>. Mỗi phần tử thuộc một category, và nhiệm vụ của bạn là tìm số lượng category khác nhau.</p>

<p>Lớp <code>CategoryHandler</code> chứa hàm sau đây, có thể hữu ích cho bạn:</p>

<ul>
	<li><code>boolean haveSameCategory(integer a, integer b)</code>: Trả về <code>true</code> nếu <code>a</code> và <code>b</code> thuộc cùng một category, và <code>false</code> nếu ngược lại. Ngoài ra, nếu <code>a</code> hoặc <code>b</code> không phải là một số hợp lệ (tức là lớn hơn hoặc bằng <code>n</code> hoặc nhỏ hơn <code>0</code>), hàm sẽ trả về <code>false</code>.</li>
</ul>

<p>Trả về <em>số lượng category khác nhau.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 6, categoryHandler = [1,1,2,2,3,3]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Có 6 phần tử trong ví dụ này. Hai phần tử đầu tiên thuộc category 1, hai phần tử tiếp theo thuộc category 2, và hai phần tử cuối cùng thuộc category 3. Vì vậy, có 3 category khác nhau.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 5, categoryHandler = [1,2,3,4,5]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Có 5 phần tử trong ví dụ này. Mỗi phần tử thuộc một category riêng. Vì vậy, có 5 category khác nhau.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3, categoryHandler = [1,1,1]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Có 3 phần tử trong ví dụ này. Tất cả đều thuộc cùng một category. Vì vậy, chỉ có 1 category khác nhau.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Việc xác định hai category có bằng nhau hay không chỉ có thể thực hiện thông qua $haveSameCategory(a,b)$; ta cần đếm số category khác nhau. Nếu coi mỗi chỉ số là một lớp riêng rồi gộp chúng, độ phức tạp sẽ là bậc hai, phù hợp với $n\le 100$.
>
> Cấu trúc disjoint-set union lưu các phần tử đã biết là bằng nhau: truy vấn mọi cặp và union khi kết quả là yes. Số root vẫn trỏ đến chính nó chính là đáp án.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
# Definition for a category handler.
# class CategoryHandler:
#     def haveSameCategory(self, a: int, b: int) -> bool:
#         pass
class Solution:
    def numberOfCategories(
        self, n: int, categoryHandler: Optional['CategoryHandler']
    ) -> int:
        def find(x: int) -> int:
            if p[x] != x:
                p[x] = find(p[x])
            return p[x]

        p = list(range(n))
        for a in range(n):
            for b in range(a + 1, n):
                if categoryHandler.haveSameCategory(a, b):
                    p[find(a)] = find(b)
        return sum(i == x for i, x in enumerate(p))
```

#### Java

```java
/**
 * Definition for a category handler.
 * class CategoryHandler {
 *     public CategoryHandler(int[] categories);
 *     public boolean haveSameCategory(int a, int b);
 * };
 */
class Solution {
    private int[] p;

    public int numberOfCategories(int n, CategoryHandler categoryHandler) {
        p = new int[n];
        for (int i = 0; i < n; ++i) {
            p[i] = i;
        }
        for (int a = 0; a < n; ++a) {
            for (int b = a + 1; b < n; ++b) {
                if (categoryHandler.haveSameCategory(a, b)) {
                    p[find(a)] = find(b);
                }
            }
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            if (i == p[i]) {
                ++ans;
            }
        }
        return ans;
    }

    private int find(int x) {
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }
}
```

#### C++

```cpp
/**
 * Definition for a category handler.
 * class CategoryHandler {
 * public:
 *     CategoryHandler(vector<int> categories);
 *     bool haveSameCategory(int a, int b);
 * };
 */
class Solution {
public:
    int numberOfCategories(int n, CategoryHandler* categoryHandler) {
        vector<int> p(n);
        iota(p.begin(), p.end(), 0);
        function<int(int)> find = [&](int x) {
            if (p[x] != x) {
                p[x] = find(p[x]);
            }
            return p[x];
        };
        for (int a = 0; a < n; ++a) {
            for (int b = a + 1; b < n; ++b) {
                if (categoryHandler->haveSameCategory(a, b)) {
                    p[find(a)] = find(b);
                }
            }
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            ans += i == p[i];
        }
        return ans;
    }
};
```

#### Go

```go
/**
 * Definition for a category handler.
 * type CategoryHandler interface {
 *  HaveSameCategory(int, int) bool
 * }
 */
func numberOfCategories(n int, categoryHandler CategoryHandler) (ans int) {
	p := make([]int, n)
	for i := range p {
		p[i] = i
	}
	var find func(int) int
	find = func(x int) int {
		if p[x] != x {
			p[x] = find(p[x])
		}
		return p[x]
	}
	for a := 0; a < n; a++ {
		for b := a + 1; b < n; b++ {
			if categoryHandler.HaveSameCategory(a, b) {
				p[find(a)] = find(b)
			}
		}
	}
	for i, x := range p {
		if i == x {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
/**
 * Definition for a category handler.
 * class CategoryHandler {
 *     constructor(categories: number[]);
 *     public haveSameCategory(a: number, b: number): boolean;
 * }
 */
function numberOfCategories(n: number, categoryHandler: CategoryHandler): number {
    const p: number[] = new Array(n).fill(0).map((_, i) => i);
    const find = (x: number): number => {
        if (p[x] !== x) {
            p[x] = find(p[x]);
        }
        return p[x];
    };
    for (let a = 0; a < n; ++a) {
        for (let b = a + 1; b < n; ++b) {
            if (categoryHandler.haveSameCategory(a, b)) {
                p[find(a)] = find(b);
            }
        }
    }
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        if (i === p[i]) {
            ++ans;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
