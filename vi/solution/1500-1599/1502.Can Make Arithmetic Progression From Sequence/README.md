---
comments: true
difficulty: Easy
rating: 1154
source: Weekly Contest 196 Q1
tags:
    - Array
    - Sorting
---

<!-- problem:start -->

# [1502. Can Make Arithmetic Progression From Sequence](https://leetcode.com/problems/can-make-arithmetic-progression-from-sequence)

[中文文档](/solution/1500-1599/1502.Can%20Make%20Arithmetic%20Progression%20From%20Sequence/README.md)

## Mô tả

<!-- description:start -->

<p>Một dãy số được gọi là <strong>cấp số cộng</strong> nếu hiệu giữa mọi cặp phần tử liên tiếp đều bằng nhau.</p>

<p>Cho một mảng số <code>arr</code>, hãy trả về <code>true</code> <em>nếu có thể sắp xếp lại mảng để tạo thành một <strong>cấp số cộng</strong>. Ngược lại, trả về</em> <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [3,5,1]
<strong>Đầu ra:</strong> true
<strong>Giải thích: </strong>Ta có thể sắp xếp lại các phần tử thành [1,3,5] hoặc [5,3,1], với hiệu lần lượt là 2 và -2 giữa mỗi cặp phần tử liên tiếp.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1,2,4]
<strong>Đầu ra:</strong> false
<strong>Giải thích: </strong>Không có cách nào sắp xếp lại các phần tử để tạo thành một cấp số cộng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= arr.length &lt;= 1000</code></li>
	<li><code>-10<sup>6</sup> &lt;= arr[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Để quyết định liệu mảng có thể được sắp xếp lại thành một cấp số cộng hay không, nếu thử mọi hoán vị thì số trường hợp là $n!$, không thể thực hiện được ngay cả khi $n$ khoảng một nghìn. Sau khi sắp xếp hợp lệ, hiệu giữa mọi cặp phần tử kề nhau đều bằng cùng một giá trị $d$, nên chỉ cần xét một thứ tự chuẩn.
>
> Việc sắp xếp tạo ra thứ tự đó: công sai phải là khoảng cách giữa các giá trị liên tiếp trong mảng đã sắp xếp. Sau đó, kiểm tra xem mọi cặp phần tử kề nhau có cùng hiệu với hiệu đầu tiên hay không bằng một lần duyệt tuyến tính, và chi phí sắp xếp vẫn đủ nhỏ với $n$ đã cho.

<!-- thinking:end -->

Trước tiên, ta có thể sắp xếp mảng $\textit{arr}$, sau đó duyệt mảng và kiểm tra xem hiệu giữa các phần tử kề nhau có bằng nhau hay không.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(\log n)$. Trong đó, $n$ là độ dài của mảng $\textit{arr}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canMakeArithmeticProgression(self, arr: List[int]) -> bool:
        arr.sort()
        d = arr[1] - arr[0]
        return all(b - a == d for a, b in pairwise(arr))
```

#### Java

```java
class Solution {
    public boolean canMakeArithmeticProgression(int[] arr) {
        Arrays.sort(arr);
        int d = arr[1] - arr[0];
        for (int i = 2; i < arr.length; ++i) {
            if (arr[i] - arr[i - 1] != d) {
                return false;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canMakeArithmeticProgression(vector<int>& arr) {
        sort(arr.begin(), arr.end());
        int d = arr[1] - arr[0];
        for (int i = 2; i < arr.size(); i++) {
            if (arr[i] - arr[i - 1] != d) {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func canMakeArithmeticProgression(arr []int) bool {
	sort.Ints(arr)
	d := arr[1] - arr[0]
	for i := 2; i < len(arr); i++ {
		if arr[i]-arr[i-1] != d {
			return false
		}
	}
	return true
}
```

#### TypeScript

```ts
function canMakeArithmeticProgression(arr: number[]): boolean {
    arr.sort((a, b) => a - b);
    const n = arr.length;
    const d = arr[1] - arr[0];
    for (let i = 2; i < n; i++) {
        if (arr[i] - arr[i - 1] !== d) {
            return false;
        }
    }
    return true;
}
```

#### Rust

```rust
impl Solution {
    pub fn can_make_arithmetic_progression(mut arr: Vec<i32>) -> bool {
        arr.sort();
        let n = arr.len();
        let d = arr[1] - arr[0];
        for i in 2..n {
            if arr[i] - arr[i - 1] != d {
                return false;
            }
        }
        true
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} arr
 * @return {boolean}
 */
var canMakeArithmeticProgression = function (arr) {
    arr.sort((a, b) => a - b);
    const n = arr.length;
    const d = arr[1] - arr[0];
    for (let i = 2; i < n; i++) {
        if (arr[i] - arr[i - 1] !== d) {
            return false;
        }
    }
    return true;
};
```

#### C

```c
int cmp(const void* a, const void* b) {
    return *(int*) a - *(int*) b;
}

bool canMakeArithmeticProgression(int* arr, int arrSize) {
    qsort(arr, arrSize, sizeof(int), cmp);
    int d = arr[1] - arr[0];
    for (int i = 2; i < arrSize; i++) {
        if (arr[i] - arr[i - 1] != d) {
            return 0;
        }
    }
    return 1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Hash Table + Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 tốn $O(n\log n)$ cho việc sắp xếp. Nếu một cấp số cộng tồn tại, công sai được xác định bởi giá trị nhỏ nhất $a$ và lớn nhất $b$ theo công thức $d=(b-a)/(n-1)$, và kết quả này phải là số nguyên. Sau khi đưa các giá trị vào một hash set, ta chỉ cần kiểm tra xem $a, a+d, \ldots, a+(n-1)d$ có xuất hiện đầy đủ hay không, với thời gian tuyến tính.

<!-- thinking:end -->

Trước tiên, ta tìm giá trị nhỏ nhất $a$ và giá trị lớn nhất $b$ trong mảng $\textit{arr}$. Nếu mảng $\textit{arr}$ có thể được sắp xếp lại thành một cấp số cộng, thì công sai $d = \frac{b - a}{n - 1}$ phải là một số nguyên.

Ta có thể dùng một hash table để ghi lại tất cả phần tử trong mảng $\textit{arr}$, sau đó duyệt $i \in [0, n)$ và kiểm tra xem $a + d \times i$ có nằm trong hash table hay không. Nếu không, điều đó có nghĩa là mảng $\textit{arr}$ không thể được sắp xếp lại thành một cấp số cộng, và ta trả về `false`. Nếu duyệt hết mảng, ta trả về `true`.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $\textit{arr}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canMakeArithmeticProgression(self, arr: List[int]) -> bool:
        a = min(arr)
        b = max(arr)
        n = len(arr)
        if (b - a) % (n - 1):
            return False
        d = (b - a) // (n - 1)
        s = set(arr)
        return all(a + d * i in s for i in range(n))
```

#### Java

```java
class Solution {
    public boolean canMakeArithmeticProgression(int[] arr) {
        int n = arr.length;
        int a = arr[0], b = arr[0];
        Set<Integer> s = new HashSet<>();
        for (int x : arr) {
            a = Math.min(a, x);
            b = Math.max(b, x);
            s.add(x);
        }
        if ((b - a) % (n - 1) != 0) {
            return false;
        }
        int d = (b - a) / (n - 1);
        for (int i = 0; i < n; ++i) {
            if (!s.contains(a + d * i)) {
                return false;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canMakeArithmeticProgression(vector<int>& arr) {
        auto [a, b] = minmax_element(arr.begin(), arr.end());
        int n = arr.size();
        if ((*b - *a) % (n - 1) != 0) {
            return false;
        }
        int d = (*b - *a) / (n - 1);
        unordered_set<int> s(arr.begin(), arr.end());
        for (int i = 0; i < n; ++i) {
            if (!s.count(*a + d * i)) {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func canMakeArithmeticProgression(arr []int) bool {
	a, b := slices.Min(arr), slices.Max(arr)
	n := len(arr)
	if (b-a)%(n-1) != 0 {
		return false
	}
	d := (b - a) / (n - 1)
	s := map[int]bool{}
	for _, x := range arr {
		s[x] = true
	}
	for i := 0; i < n; i++ {
		if !s[a+i*d] {
			return false
		}
	}
	return true
}
```

#### TypeScript

```ts
function canMakeArithmeticProgression(arr: number[]): boolean {
    const n = arr.length;
    const a = Math.min(...arr);
    const b = Math.max(...arr);

    if ((b - a) % (n - 1) !== 0) {
        return false;
    }

    const d = (b - a) / (n - 1);
    const s = new Set(arr);

    for (let i = 0; i < n; ++i) {
        if (!s.has(a + d * i)) {
            return false;
        }
    }
    return true;
}
```

#### Rust

```rust
impl Solution {
    pub fn can_make_arithmetic_progression(arr: Vec<i32>) -> bool {
        let n = arr.len();
        let a = *arr.iter().min().unwrap();
        let b = *arr.iter().max().unwrap();

        if (b - a) % (n as i32 - 1) != 0 {
            return false;
        }

        let d = (b - a) / (n as i32 - 1);
        let s: std::collections::HashSet<_> = arr.into_iter().collect();

        for i in 0..n {
            if !s.contains(&(a + d * i as i32)) {
                return false;
            }
        }
        true
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} arr
 * @return {boolean}
 */
var canMakeArithmeticProgression = function (arr) {
    const n = arr.length;
    const a = Math.min(...arr);
    const b = Math.max(...arr);

    if ((b - a) % (n - 1) !== 0) {
        return false;
    }

    const d = (b - a) / (n - 1);
    const s = new Set(arr);

    for (let i = 0; i < n; ++i) {
        if (!s.has(a + d * i)) {
            return false;
        }
    }
    return true;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
