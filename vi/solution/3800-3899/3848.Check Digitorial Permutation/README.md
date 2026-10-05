---
comments: true
difficulty: Medium
rating: 1420
source: Weekly Contest 490 Q2
tags:
    - Math
    - Counting
---

<!-- problem:start -->

# [3848. Check Digitorial Permutation](https://leetcode.com/problems/check-digitorial-permutation)

[中文文档](/solution/3800-3899/3848.Check%20Digitorial%20Permutation/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code>.</p>

<p>Một số được gọi là <strong>digitorial</strong> nếu tổng <strong>giai thừa</strong> của các chữ số <strong>bằng</strong> chính số đó.</p>

<p>Hãy xác định xem <strong>có bất kỳ hoán vị nào</strong> của <code>n</code> (bao gồm cả thứ tự ban đầu) tạo thành một số <strong>digitorial</strong> hay không.</p>

<p>Trả về <code>true</code> nếu tồn tại một <strong>hoán vị</strong> như vậy, nếu không thì trả về <code>false</code>.</p>

<p><strong>Lưu ý</strong>:</p>

<ul>
	<li><strong>Giai thừa</strong> của một số nguyên không âm <code>x</code>, ký hiệu là <code>x!</code>, là <strong>tích</strong> của tất cả các số nguyên dương <strong>nhỏ hơn hoặc bằng</strong> <code>x</code>, và <code>0! = 1</code>.</li>
	<li><strong>Hoán vị</strong> là cách sắp xếp lại tất cả các chữ số của một số, nhưng <strong>không</strong> được bắt đầu bằng số 0. Mọi cách sắp xếp bắt đầu bằng số 0 đều không hợp lệ.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 145</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<p>Bản thân số 145 là digitorial vì <code>1! + 4! + 5! = 1 + 24 + 120 = 145</code>. Vì vậy, đáp án là <code>true</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 10</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong>​​​​​​​</p>

<p>10 không phải là digitorial vì <code>1! + 0! = 2</code> không bằng 10, còn hoán vị <code>&quot;01&quot;</code> không hợp lệ vì bắt đầu bằng số 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần kiểm tra xem có hoán vị nào của $n$ không bắt đầu bằng 0 bằng với tổng giai thừa các chữ số của nó hay không. Điều kiện $n \le 10^9$ không cho phép liệt kê các hoán vị.
>
> Tổng giai thừa chỉ phụ thuộc vào đa tập chữ số. Sau khi tính tổng đó là $x$, ta so sánh đa tập chữ số của $x$ và $n$.
>
> Ta tính trước giai thừa của các số từ $0..9$, cộng giai thừa các chữ số của $n$, rồi sắp xếp hai chuỗi thập phân.
>
> Hai chuỗi bằng nhau nghĩa là tồn tại một hoán vị của cùng các chữ số; cách viết bắt đầu bằng 0 sẽ không thể khớp với tập chữ số của $n$ khi biểu diễn dưới dạng một số.

<!-- thinking:end -->

Theo mô tả đề bài, dù sắp xếp lại các chữ số của số $n$ theo cách nào, tổng các giai thừa của các chữ số của số digitorial vẫn không đổi. Do đó, ta chỉ cần tính tổng giai thừa của từng chữ số trong số $n$, rồi kiểm tra xem các chữ số của tổng này có thể được hoán vị để giống với các chữ số của $n$ hay không.

Độ phức tạp thời gian là $O(\log n)$, trong đó $n$ là số nguyên được cho trong đề bài. Độ phức tạp không gian là $O(d)$, trong đó $d = 10$ là độ dài của mảng tiền xử lý giai thừa.

<!-- tabs:start -->

#### Python3

```python
@cache
def f(x: int) -> int:
    if x < 2:
        return 1
    return x * f(x - 1)

class Solution:
    def isDigitorialPermutation(self, n: int) -> bool:
        x, y = 0, n
        while y:
            x += f(y % 10)
            y //= 10
        return sorted(str(x)) == sorted(str(n))
```

#### Java

```java
class Solution {
    private static final int[] f = new int[10];

    static {
        f[0] = 1;
        for (int i = 1; i < 10; i++) {
            f[i] = f[i - 1] * i;
        }
    }

    public boolean isDigitorialPermutation(int n) {
        int x = 0;
        int y = n;

        while (y > 0) {
            x += f[y % 10];
            y /= 10;
        }

        char[] a = String.valueOf(x).toCharArray();
        char[] b = String.valueOf(n).toCharArray();

        Arrays.sort(a);
        Arrays.sort(b);

        return Arrays.equals(a, b);
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isDigitorialPermutation(int n) {
        static int f[10];
        static bool initialized = false;

        if (!initialized) {
            f[0] = 1;
            for (int i = 1; i < 10; i++) {
                f[i] = f[i - 1] * i;
            }
            initialized = true;
        }

        int x = 0;
        int y = n;

        while (y > 0) {
            x += f[y % 10];
            y /= 10;
        }

        string a = to_string(x);
        string b = to_string(n);

        sort(a.begin(), a.end());
        sort(b.begin(), b.end());

        return a == b;
    }
};
```

#### Go

```go
func isDigitorialPermutation(n int) bool {
	f := make([]int, 10)
	f[0] = 1
	for i := 1; i < 10; i++ {
		f[i] = f[i-1] * i
	}

	x := 0
	y := n

	for y > 0 {
		x += f[y%10]
		y /= 10
	}

	a := []byte(strconv.Itoa(x))
	b := []byte(strconv.Itoa(n))

	sort.Slice(a, func(i, j int) bool { return a[i] < a[j] })
	sort.Slice(b, func(i, j int) bool { return b[i] < b[j] })

	return string(a) == string(b)
}
```

#### TypeScript

```ts
function isDigitorialPermutation(n: number): boolean {
    const f: number[] = new Array(10);
    f[0] = 1;
    for (let i = 1; i < 10; i++) {
        f[i] = f[i - 1] * i;
    }

    let x = 0;
    let y = n;

    while (y > 0) {
        x += f[y % 10];
        y = Math.floor(y / 10);
    }

    const a = x.toString().split('').sort().join('');
    const b = n.toString().split('').sort().join('');

    return a === b;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
