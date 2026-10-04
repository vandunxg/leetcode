---
comments: true
difficulty: Medium
rating: 1735
source: Biweekly Contest 150 Q2
tags:
    - Array
    - Binary Search
---

<!-- problem:start -->

# [3453. Separate Squares I](https://leetcode.com/problems/separate-squares-i)

[中文文档](/solution/3400-3499/3453.Separate%20Squares%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên 2D <code>squares</code>. Mỗi <code>squares[i] = [x<sub>i</sub>, y<sub>i</sub>, l<sub>i</sub>]</code> biểu diễn tọa độ điểm dưới cùng bên trái và độ dài cạnh của một hình vuông song song với trục x.</p>

<p>Hãy tìm giá trị tọa độ y <strong>nhỏ nhất</strong> của một đường ngang sao cho tổng diện tích các hình vuông phía trên đường này <em>bằng</em> tổng diện tích các hình vuông phía dưới đường này.</p>

<p>Các đáp án nằm trong phạm vi <code>10<sup>-5</sup></code> so với đáp án thực tế đều được chấp nhận.</p>

<p><strong>Lưu ý</strong>: Các hình vuông <strong>có thể</strong> chồng lấn. Các vùng chồng lấn được tính <strong>nhiều lần</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">squares = [[0,0,1],[2,2,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1.00000</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3400-3499/3453.Separate%20Squares%20I/images/4062example1drawio.png" style="width: 378px; height: 352px;" /></p>

<p>Mọi đường ngang nằm giữa <code>y = 1</code> và <code>y = 2</code> đều có 1 đơn vị diện tích phía trên và 1 đơn vị diện tích phía dưới. Lựa chọn thấp nhất là 1.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">squares = [[0,0,2],[1,1,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1.16667</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3400-3499/3453.Separate%20Squares%20I/images/4062example2drawio.png" style="width: 378px; height: 352px;" /></p>

<p>Diện tích là:</p>

<ul>
	<li>Phía dưới đường ngang: <code>7/6 * 2 (Red) + 1/6 (Blue) = 15/6 = 2.5</code>.</li>
	<li>Phía trên đường ngang: <code>5/6 * 2 (Red) + 5/6 (Blue) = 15/6 = 2.5</code>.</li>
</ul>

<p>Vì diện tích phía trên và phía dưới đường ngang bằng nhau, đầu ra là <code>7/6 = 1.16667</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= squares.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>squares[i] = [x<sub>i</sub>, y<sub>i</sub>, l<sub>i</sub>]</code></li>
	<li><code>squares[i].length == 3</code></li>
	<li><code>0 &lt;= x<sub>i</sub>, y<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= l<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
	<li>Tổng diện tích của tất cả các hình vuông không vượt quá <code>10<sup>12</sup></code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Một đường ngang phải chia đôi tổng diện tích của các hình vuông. Diện tích phía dưới đường tăng đơn điệu; tổng diện tích có thể lên đến $10^{12}$ và các tọa độ rất lớn, nên không thể duyệt từng độ cao.
>
> Tính đơn điệu trên tập số thực gợi ý việc dùng tìm kiếm nhị phân. Điều kiện cần kiểm tra là liệu diện tích nằm hoàn toàn phía dưới $y=y_1$ đã đạt một nửa tổng diện tích hay chưa.
>
> Một hình vuông có đáy nằm dưới $y_1$ đóng góp độ dài cạnh nhân với chiều sâu bị cắt. Ta tìm kiếm cho đến khi khoảng cách là $10^{-5}$ rồi trả về đầu mút bên phải.

<!-- thinking:end -->

Theo đề bài, chúng ta cần tìm một đường ngang sao cho tổng diện tích các hình vuông phía trên đường bằng tổng diện tích các hình vuông phía dưới đường. Khi tọa độ $y$ tăng, diện tích phía dưới đường tăng còn diện tích phía trên đường giảm, vì vậy chúng ta có thể dùng tìm kiếm nhị phân để tìm tọa độ $y$ của đường ngang này.

Ta đặt biên trái của tìm kiếm nhị phân là $l = 0$, biên phải là $r = \max(y_i + l_i)$, tức điểm cao nhất của tất cả các hình vuông. Sau đó, ta tính điểm giữa $mid = (l + r) / 2$ và tính diện tích phía dưới đường ngang này. Nếu diện tích lớn hơn hoặc bằng một nửa tổng diện tích, nghĩa là cần dịch biên phải $r$ xuống dưới; ngược lại, ta dịch biên trái $l$ lên trên. Ta lặp lại quá trình này cho đến khi hiệu giữa biên trái và biên phải nhỏ hơn một giá trị rất nhỏ, chẳng hạn $10^{-5}$.

Độ phức tạp thời gian là $O(n \log(MU))$, trong đó $n$ là số hình vuông, $M = 10^5$, và $U = \max(y_i + l_i)$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def separateSquares(self, squares: List[List[int]]) -> float:
        def check(y1: float) -> bool:
            t = 0
            for _, y, l in squares:
                if y < y1:
                    t += l * min(y1 - y, l)
            return t >= s / 2

        s = sum(a[2] * a[2] for a in squares)
        l, r = 0, max(a[1] + a[2] for a in squares)
        eps = 1e-5
        while r - l > eps:
            mid = (l + r) / 2
            if check(mid):
                r = mid
            else:
                l = mid
        return r
```

#### Java

```java
class Solution {
    private int[][] squares;
    private double s;

    private boolean check(double y1) {
        double t = 0.0;
        for (int[] a : squares) {
            int y = a[1];
            int l = a[2];
            if (y < y1) {
                t += (double) l * Math.min(y1 - y, l);
            }
        }
        return t >= s / 2.0;
    }

    public double separateSquares(int[][] squares) {
        this.squares = squares;
        s = 0.0;
        double l = 0.0;
        double r = 0.0;
        for (int[] a : squares) {
            s += (double) a[2] * a[2];
            r = Math.max(r, a[1] + a[2]);
        }

        double eps = 1e-5;
        while (r - l > eps) {
            double mid = (l + r) / 2.0;
            if (check(mid)) {
                r = mid;
            } else {
                l = mid;
            }
        }
        return r;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>>* squares;
    double s;

    bool check(double y1) {
        double t = 0.0;
        for (const auto& a : *squares) {
            int y = a[1];
            int l = a[2];
            if (y < y1) {
                t += (double) l * min(y1 - y, (double) l);
            }
        }
        return t >= s / 2.0;
    }

    double separateSquares(vector<vector<int>>& squares) {
        this->squares = &squares;
        s = 0.0;
        double l = 0.0;
        double r = 0.0;
        for (const auto& a : squares) {
            s += (double) a[2] * a[2];
            r = max(r, (double) a[1] + a[2]);
        }
        const double eps = 1e-5;
        while (r - l > eps) {
            double mid = (l + r) / 2.0;
            if (check(mid)) {
                r = mid;
            } else {
                l = mid;
            }
        }
        return r;
    }
};
```

#### Go

```go
func separateSquares(squares [][]int) float64 {
	s := 0.0
	check := func(y1 float64) bool {
		t := 0.0
		for _, a := range squares {
			y := a[1]
			l := a[2]
			if float64(y) < y1 {
				h := min(float64(l), y1-float64(y))
				t += float64(l) * h
			}
		}
		return t >= s/2.0
	}
	l, r := 0.0, 0.0
	for _, a := range squares {
		s += float64(a[2] * a[2])
		r = max(r, float64(a[1]+a[2]))
	}

	const eps = 1e-5
	for r-l > eps {
		mid := (l + r) / 2.0
		if check(mid) {
			r = mid
		} else {
			l = mid
		}
	}
	return r
}
```

#### TypeScript

```ts
function separateSquares(squares: number[][]): number {
    const check = (y1: number): boolean => {
        let t = 0;
        for (const [_, y, l] of squares) {
            if (y < y1) {
                t += l * Math.min(y1 - y, l);
            }
        }
        return t >= s / 2;
    };

    let s = 0;
    let l = 0;
    let r = 0;
    for (const a of squares) {
        s += a[2] * a[2];
        r = Math.max(r, a[1] + a[2]);
    }

    const eps = 1e-5;
    while (r - l > eps) {
        const mid = (l + r) / 2;
        if (check(mid)) {
            r = mid;
        } else {
            l = mid;
        }
    }
    return r;
}
```

#### Rust

```rust
impl Solution {
    pub fn separate_squares(squares: Vec<Vec<i32>>) -> f64 {
        let mut s: f64 = 0.0;

        let mut l: f64 = 0.0;
        let mut r: f64 = 0.0;

        for a in squares.iter() {
            let len = a[2] as f64;
            s += len * len;
            r = r.max((a[1] + a[2]) as f64);
        }

        let check = |y1: f64| -> bool {
            let mut t: f64 = 0.0;
            for a in squares.iter() {
                let y = a[1] as f64;
                let l = a[2] as f64;
                if y < y1 {
                    let h = l.min(y1 - y);
                    t += l * h;
                }
            }
            t >= s / 2.0
        };

        const EPS: f64 = 1e-5;
        while r - l > EPS {
            let mid = (l + r) / 2.0;
            if check(mid) {
                r = mid;
            } else {
                l = mid;
            }
        }
        r
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
