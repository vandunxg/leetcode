---
comments: true
difficulty: Easy
rating: 1277
source: Weekly Contest 171 Q1
tags:
    - Math
---

<!-- problem:start -->

# [1317. Convert Integer to the Sum of Two No-Zero Integers](https://leetcode.com/problems/convert-integer-to-the-sum-of-two-no-zero-integers)

[中文文档](/solution/1300-1399/1317.Convert%20Integer%20to%20the%20Sum%20of%20Two%20No-Zero%20Integers/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Số No-Zero</strong> là số nguyên dương có biểu diễn thập phân <strong>không chứa chữ số <code>0</code> nào</strong>.</p>

<p>Cho số nguyên <code>n</code>, hãy trả về <em>danh sách gồm hai số nguyên</em> <code>[a, b]</code> <em>thỏa mãn</em>:</p>

<ul>
	<li><code>a</code> và <code>b</code> là <strong>số No-Zero</strong>.</li>
	<li><code>a + b = n</code></li>
</ul>

<p>Bộ test được tạo sao cho luôn có ít nhất một nghiệm hợp lệ. Nếu có nhiều nghiệm, bạn có thể trả về bất kỳ nghiệm nào.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> n = 2
<strong>Output:</strong> [1,1]
<strong>Giải thích:</strong> Chọn a = 1 và b = 1.
Cả a và b đều là số No-Zero, đồng thời a + b = 2 = n.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> n = 11
<strong>Output:</strong> [2,9]
<strong>Giải thích:</strong> Chọn a = 2 và b = 9.
Cả a và b đều là số No-Zero, đồng thời a + b = 11 = n.
Lưu ý rằng các đáp án hợp lệ khác như [8, 3] cũng được chấp nhận.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt trực tiếp

<!-- thinking:start -->

> **Tư duy**
>
> Với $n \le 10^4$, luôn tồn tại cách tách $a+b=n$ sao cho cả hai số không chứa chữ số 0. Tăng dần $a$ từ $1$, đặt $b=n-a$, rồi kiểm tra chuỗi ghép từ biểu diễn thập phân của chúng có chứa ký tự `'0'` hay không. Cặp đầu tiên thỏa mãn là đáp án hợp lệ.

<!-- thinking:end -->

Bắt đầu từ $1$, ta duyệt các giá trị $a$ và đặt $b = n - a$. Với mỗi cặp $a, b$, chuyển chúng thành chuỗi rồi ghép lại để kiểm tra có chứa ký tự '0' hay không. Nếu không chứa '0', ta đã tìm được đáp án và trả về $[a, b]$.

Độ phức tạp thời gian là $O(n \times \log n)$, trong đó $n$ là số nguyên được cho trong đề bài. Độ phức tạp không gian là $O(\log n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getNoZeroIntegers(self, n: int) -> List[int]:
        for a in count(1):
            b = n - a
            if "0" not in f"{a}{b}":
                return [a, b]
```

#### Java

```java
class Solution {
    public int[] getNoZeroIntegers(int n) {
        for (int a = 1;; ++a) {
            int b = n - a;
            if (!(a + "" + b).contains("0")) {
                return new int[] {a, b};
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> getNoZeroIntegers(int n) {
        for (int a = 1;; ++a) {
            int b = n - a;
            if ((to_string(a) + to_string(b)).find('0') == -1) {
                return {a, b};
            }
        }
    }
};
```

#### Go

```go
func getNoZeroIntegers(n int) []int {
	for a := 1; ; a++ {
		b := n - a
		if !strings.Contains(strconv.Itoa(a)+strconv.Itoa(b), "0") {
			return []int{a, b}
		}
	}
}
```

#### TypeScript

```ts
function getNoZeroIntegers(n: number): number[] {
    for (let a = 1; ; ++a) {
        const b = n - a;
        if (!`${a}${b}`.includes('0')) {
            return [a, b];
        }
    }
}
```

#### Rust

```rust
impl Solution {
    pub fn get_no_zero_integers(n: i32) -> Vec<i32> {
        for a in 1..n {
            let b = n - a;
            if !a.to_string().contains('0') && !b.to_string().contains('0') {
                return vec![a, b];
            }
        }
        vec![]
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Duyệt trực tiếp (Cách khác)

<!-- thinking:start -->

> **Tư duy**
>
> Cách đầu tiên tạo chuỗi để phát hiện chữ số 0. Ta có thể tránh cấp phát chuỗi bằng cách kiểm tra phần dư khi chia $a$ và $b$ cho 10; cách này dùng thêm không gian hằng số và giữ nguyên thứ tự duyệt.

<!-- thinking:end -->

Ở Lời giải 1, ta chuyển $a$ và $b$ thành chuỗi rồi ghép lại để kiểm tra xem có chứa ký tự '0' hay không. Ở đây, ta có thể dùng hàm $f(x)$ để kiểm tra $x$ có chứa ký tự '0' không, sau đó duyệt trực tiếp các giá trị $a$ và kiểm tra cả $a$ lẫn $b = n - a$. Nếu cả hai đều không chứa ký tự '0', ta đã tìm được đáp án và trả về $[a, b]$.

Độ phức tạp thời gian là $O(n \times \log n)$, trong đó $n$ là số nguyên được cho trong đề bài. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getNoZeroIntegers(self, n: int) -> List[int]:
        def f(x: int) -> bool:
            while x:
                if x % 10 == 0:
                    return False
                x //= 10
            return True

        for a in count(1):
            b = n - a
            if f(a) and f(b):
                return [a, b]
```

#### Java

```java
class Solution {
    public int[] getNoZeroIntegers(int n) {
        for (int a = 1;; ++a) {
            int b = n - a;
            if (f(a) && f(b)) {
                return new int[] {a, b};
            }
        }
    }

    private boolean f(int x) {
        for (; x > 0; x /= 10) {
            if (x % 10 == 0) {
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
    vector<int> getNoZeroIntegers(int n) {
        auto f = [](int x) {
            for (; x; x /= 10) {
                if (x % 10 == 0) {
                    return false;
                }
            }
            return true;
        };
        for (int a = 1;; ++a) {
            int b = n - a;
            if (f(a) && f(b)) {
                return {a, b};
            }
        }
    }
};
```

#### Go

```go
func getNoZeroIntegers(n int) []int {
	f := func(x int) bool {
		for ; x > 0; x /= 10 {
			if x%10 == 0 {
				return false
			}
		}
		return true
	}
	for a := 1; ; a++ {
		b := n - a
		if f(a) && f(b) {
			return []int{a, b}
		}
	}
}
```

#### TypeScript

```ts
function getNoZeroIntegers(n: number): number[] {
    const f = (x: number): boolean => {
        for (; x; x = (x / 10) | 0) {
            if (x % 10 === 0) {
                return false;
            }
        }
        return true;
    };
    for (let a = 1; ; ++a) {
        const b = n - a;
        if (f(a) && f(b)) {
            return [a, b];
        }
    }
}
```

#### Rust

```rust
impl Solution {
    pub fn get_no_zero_integers(n: i32) -> Vec<i32> {
        fn f(mut x: i32) -> bool {
            while x > 0 {
                if x % 10 == 0 {
                    return false;
                }
                x /= 10;
            }
            true
        }

        for a in 1..n {
            let b = n - a;
            if f(a) && f(b) {
                return vec![a, b];
            }
        }
        vec![]
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
