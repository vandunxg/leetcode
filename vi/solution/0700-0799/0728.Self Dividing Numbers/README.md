---
comments: true
difficulty: Easy
tags:
    - Math
---

<!-- problem:start -->

# [728. Self Dividing Numbers](https://leetcode.com/problems/self-dividing-numbers)

[中文文档](/solution/0700-0799/0728.Self%20Dividing%20Numbers/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Số tự chia hết</strong> là số chia hết cho từng chữ số của chính nó.</p>

<ul>
	<li>Ví dụ, <code>128</code> là <strong>số tự chia hết</strong> vì <code>128 % 1 == 0</code>, <code>128 % 2 == 0</code> và <code>128 % 8 == 0</code>.</li>
</ul>

<p><strong>Số tự chia hết</strong> không được chứa chữ số 0.</p>

<p>Cho hai số nguyên <code>left</code> và <code>right</code>, hãy trả về <em>danh sách tất cả <strong>số tự chia hết</strong> trong khoảng</em> <code>[left, right]</code> (bao gồm cả hai đầu mút).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> left = 1, right = 22
<strong>Đầu ra:</strong> [1,2,3,4,5,6,7,8,9,11,12,15,22]
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> left = 47, right = 85
<strong>Đầu ra:</strong> [48,55,66,77]
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= left &lt;= right &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Khoảng cần xét dài tối đa $10^4$, và kiểm tra một số với các chữ số của nó chỉ cần vài phép chia. Có thể duyệt toàn bộ khoảng.
>
> Chữ số 0 không thể làm số chia nên số chứa chữ số 0 bị loại ngay; các số còn lại phải chia hết cho từng chữ số của mình.
>
> Duyệt các chữ số của $y=x$; loại số nếu gặp chữ số $0$ hoặc phép chia lấy dư khác $0$, nếu không thì giữ lại $x$.

<!-- thinking:end -->

Ta định nghĩa hàm $\textit{check}(x)$ để xác định $x$ có phải là số tự chia hết hay không. Ý tưởng triển khai như sau:

Ta dùng $y$ để lưu giá trị của $x$, rồi liên tục chia $y$ cho $10$ đến khi $y$ bằng $0$. Trong quá trình này, ta kiểm tra chữ số cuối của $y$ có bằng $0$ hay $x$ không chia hết cho chữ số cuối đó. Nếu một trong hai điều kiện đúng, $x$ không phải số tự chia hết và ta trả về $\text{false}$. Nếu duyệt hết các chữ số mà không gặp trường hợp này, ta trả về $\text{true}$.

Cuối cùng, ta duyệt mọi số trong khoảng $[\textit{left}, \textit{right}]$ và gọi $\textit{check}(x)$ cho từng số. Nếu hàm trả về $\text{true}$, ta thêm số đó vào mảng kết quả.

Độ phức tạp thời gian là $O(n \times \log_{10} M)$, trong đó $n$ là số phần tử trong khoảng $[\textit{left}, \textit{right}]$, còn $M = \textit{right}$ là giá trị lớn nhất trong khoảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def selfDividingNumbers(self, left: int, right: int) -> List[int]:
        def check(x: int) -> bool:
            y = x
            while y:
                if y % 10 == 0 or x % (y % 10):
                    return False
                y //= 10
            return True

        return [x for x in range(left, right + 1) if check(x)]
```

#### Java

```java
class Solution {
    public List<Integer> selfDividingNumbers(int left, int right) {
        List<Integer> ans = new ArrayList<>();
        for (int x = left; x <= right; ++x) {
            if (check(x)) {
                ans.add(x);
            }
        }
        return ans;
    }

    private boolean check(int x) {
        for (int y = x; y > 0; y /= 10) {
            if (y % 10 == 0 || x % (y % 10) != 0) {
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
    vector<int> selfDividingNumbers(int left, int right) {
        auto check = [&](int x) -> bool {
            for (int y = x; y; y /= 10) {
                if (y % 10 == 0 || x % (y % 10)) {
                    return false;
                }
            }
            return true;
        };
        vector<int> ans;
        for (int x = left; x <= right; ++x) {
            if (check(x)) {
                ans.push_back(x);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func selfDividingNumbers(left int, right int) (ans []int) {
	check := func(x int) bool {
		for y := x; y > 0; y /= 10 {
			if y%10 == 0 || x%(y%10) != 0 {
				return false
			}
		}
		return true
	}
	for x := left; x <= right; x++ {
		if check(x) {
			ans = append(ans, x)
		}
	}
	return
}
```

#### TypeScript

```ts
function selfDividingNumbers(left: number, right: number): number[] {
    const check = (x: number): boolean => {
        for (let y = x; y; y = Math.floor(y / 10)) {
            if (y % 10 === 0 || x % (y % 10) !== 0) {
                return false;
            }
        }
        return true;
    };
    return Array.from({ length: right - left + 1 }, (_, i) => i + left).filter(check);
}
```

#### Rust

```rust
impl Solution {
    pub fn self_dividing_numbers(left: i32, right: i32) -> Vec<i32> {
        fn check(x: i32) -> bool {
            let mut y = x;
            while y > 0 {
                if y % 10 == 0 || x % (y % 10) != 0 {
                    return false;
                }
                y /= 10;
            }
            true
        }

        (left..=right).filter(|&x| check(x)).collect()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
