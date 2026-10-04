---
comments: true
difficulty: Hard
tags:
    - Array
    - Two Pointers
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [2604. Minimum Time to Eat All Grains 🔒](https://leetcode.com/problems/minimum-time-to-eat-all-grains)

[中文文档](/solution/2600-2699/2604.Minimum%20Time%20to%20Eat%20All%20Grains/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> con gà mái và <code>m</code> hạt trên một đường thẳng. Bạn được cho vị trí ban đầu của các con gà mái và các hạt trong hai mảng số nguyên <code>hens</code> và <code>grains</code>, lần lượt có kích thước <code>n</code> và <code>m</code>.</p>

<p>Một con gà mái có thể ăn một hạt nếu chúng ở cùng vị trí. Thời gian cần thiết cho việc này không đáng kể. Một con gà mái cũng có thể ăn nhiều hạt.</p>

<p>Trong <code>1</code> giây, một con gà mái có thể di chuyển sang phải hoặc trái <code>1</code> đơn vị. Các con gà mái có thể di chuyển đồng thời và độc lập với nhau.</p>

<p>Trả về <em><strong>thời gian nhỏ nhất</strong> để ăn hết tất cả các hạt nếu các con gà mái hành động tối ưu.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> hens = [3,6,7], grains = [2,4,7,9]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Một cách để các con gà mái ăn hết tất cả các hạt trong 2 giây là:
- Con gà mái thứ nhất ăn hạt ở vị trí 2 trong 1 giây.
- Con gà mái thứ hai ăn hạt ở vị trí 4 trong 2 giây.
- Con gà mái thứ ba ăn các hạt ở vị trí 7 và 9 trong 2 giây.
Vì vậy, thời gian lâu nhất cần thiết là 2.
Có thể chứng minh rằng các con gà mái không thể ăn hết tất cả các hạt trong thời gian dưới 2 giây.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> hens = [4,6,109,111,213,215], grains = [5,110,214]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>
Một cách để các con gà mái ăn hết tất cả các hạt trong 1 giây là:
- Con gà mái thứ nhất ăn hạt ở vị trí 5 trong 1 giây.
- Con gà mái thứ tư ăn hạt ở vị trí 110 trong 1 giây.
- Con gà mái thứ sáu ăn hạt ở vị trí 214 trong 1 giây.
- Các con gà mái còn lại không di chuyển.
Vì vậy, thời gian lâu nhất cần thiết là 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= hens.length, grains.length &lt;= 2*10<sup>4</sup></code></li>
	<li><code>0 &lt;= hens[i], grains[j] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Nhiều con gà mái ăn các hạt đồng thời; thời gian của một con gà mái là độ dài của một hành trình có thể gập lại. Việc chia các đoạn hạt liên tiếp cho các con gà mái sẽ tăng nhanh theo số lượng con gà mái. Với $n,m \le 2\times 10^4$, không thể dùng cách tìm kiếm toàn bộ.
>
> Thời gian $t$ có tính đơn điệu: nếu có thể ăn hết mọi hạt trong $t$, thì thời gian lớn hơn cũng thực hiện được. Sau khi sắp xếp, các hạt bên trái nên được giao cho các con gà mái bên trái, vì vậy ta có thể dùng hai con trỏ để kiểm tra một giá trị $t$.
>
> Với mỗi con gà mái, ta tính chi phí của hành trình gập tùy theo việc hạt tiếp theo nằm bên trái hay bên phải, rồi tiếp tục ăn các hạt về bên phải cho đến khi hạt tiếp theo vượt quá $t$. Tìm kiếm nhị phân sẽ cho ra $t$ nhỏ nhất khả thi.

<!-- thinking:end -->

Trước hết, sắp xếp các con gà mái và các hạt theo vị trí từ trái sang phải. Sau đó dùng tìm kiếm nhị phân trên thời gian $t$ để tìm $t$ nhỏ nhất sao cho có thể ăn hết tất cả các hạt trong $t$ giây.

Với mỗi con gà mái, ta dùng con trỏ $j$ trỏ đến hạt bên trái nhất chưa được ăn, vị trí hiện tại của con gà mái là $x$ và vị trí của hạt là $y$. Có các trường hợp sau:

- Nếu $y \leq x$, ta đặt $d = x - y$. Nếu $d \gt t$, không thể ăn hạt hiện tại, nên trả về `false` ngay. Nếu không, di chuyển con trỏ $j$ sang phải cho đến khi $j=m$ hoặc $grains[j] \gt x$. Lúc này, ta cần kiểm tra xem con gà mái có thể ăn hạt mà $j$ đang trỏ đến hay không. Nếu có, tiếp tục di chuyển con trỏ $j$ sang phải cho đến khi $j=m$ hoặc $min(d, grains[j] - x) + grains[j] - y \gt t$.
- Nếu $y \lt x$, di chuyển con trỏ $j$ sang phải cho đến khi $j=m$ hoặc $grains[j] - x \gt t$.

Nếu $j=m$, nghĩa là tất cả các hạt đã được ăn, trả về `true`; ngược lại, trả về `false`.

Độ phức tạp thời gian là $O(n \times \log n + m \times \log m + (m + n) \times \log U)$, độ phức tạp không gian là $O(\log m + \log n)$. $n$ và $m$ lần lượt là số lượng con gà mái và số lượng hạt, còn $U$ là giá trị lớn nhất trong tất cả vị trí của các con gà mái và các hạt.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumTime(self, hens: List[int], grains: List[int]) -> int:
        def check(t):
            j = 0
            for x in hens:
                if j == m:
                    return True
                y = grains[j]
                if y <= x:
                    d = x - y
                    if d > t:
                        return False
                    while j < m and grains[j] <= x:
                        j += 1
                    while j < m and min(d, grains[j] - x) + grains[j] - y <= t:
                        j += 1
                else:
                    while j < m and grains[j] - x <= t:
                        j += 1
            return j == m

        hens.sort()
        grains.sort()
        m = len(grains)
        r = abs(hens[0] - grains[0]) + grains[-1] - grains[0] + 1
        return bisect_left(range(r), True, key=check)
```

#### Java

```java
class Solution {
    private int[] hens;
    private int[] grains;
    private int m;

    public int minimumTime(int[] hens, int[] grains) {
        m = grains.length;
        this.hens = hens;
        this.grains = grains;
        Arrays.sort(hens);
        Arrays.sort(grains);
        int l = 0;
        int r = Math.abs(hens[0] - grains[0]) + grains[m - 1] - grains[0];
        while (l < r) {
            int mid = (l + r) >> 1;
            if (check(mid)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }

    private boolean check(int t) {
        int j = 0;
        for (int x : hens) {
            if (j == m) {
                return true;
            }
            int y = grains[j];
            if (y <= x) {
                int d = x - y;
                if (d > t) {
                    return false;
                }
                while (j < m && grains[j] <= x) {
                    ++j;
                }
                while (j < m && Math.min(d, grains[j] - x) + grains[j] - y <= t) {
                    ++j;
                }
            } else {
                while (j < m && grains[j] - x <= t) {
                    ++j;
                }
            }
        }
        return j == m;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumTime(vector<int>& hens, vector<int>& grains) {
        int m = grains.size();
        sort(hens.begin(), hens.end());
        sort(grains.begin(), grains.end());
        int l = 0;
        int r = abs(hens[0] - grains[0]) + grains[m - 1] - grains[0];
        auto check = [&](int t) -> bool {
            int j = 0;
            for (int x : hens) {
                if (j == m) {
                    return true;
                }
                int y = grains[j];
                if (y <= x) {
                    int d = x - y;
                    if (d > t) {
                        return false;
                    }
                    while (j < m && grains[j] <= x) {
                        ++j;
                    }
                    while (j < m && min(d, grains[j] - x) + grains[j] - y <= t) {
                        ++j;
                    }
                } else {
                    while (j < m && grains[j] - x <= t) {
                        ++j;
                    }
                }
            }
            return j == m;
        };
        while (l < r) {
            int mid = (l + r) >> 1;
            if (check(mid)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
};
```

#### Go

```go
func minimumTime(hens []int, grains []int) int {
	sort.Ints(hens)
	sort.Ints(grains)
	m := len(grains)
	l, r := 0, abs(hens[0]-grains[0])+grains[m-1]-grains[0]
	check := func(t int) bool {
		j := 0
		for _, x := range hens {
			if j == m {
				return true
			}
			y := grains[j]
			if y <= x {
				d := x - y
				if d > t {
					return false
				}
				for j < m && grains[j] <= x {
					j++
				}
				for j < m && min(d, grains[j]-x)+grains[j]-y <= t {
					j++
				}
			} else {
				for j < m && grains[j]-x <= t {
					j++
				}
			}
		}
		return j == m
	}
	for l < r {
		mid := (l + r) >> 1
		if check(mid) {
			r = mid
		} else {
			l = mid + 1
		}
	}
	return l
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function minimumTime(hens: number[], grains: number[]): number {
    hens.sort((a, b) => a - b);
    grains.sort((a, b) => a - b);
    const m = grains.length;
    let l = 0;
    let r = Math.abs(hens[0] - grains[0]) + grains[m - 1] - grains[0] + 1;

    const check = (t: number): boolean => {
        let j = 0;
        for (const x of hens) {
            if (j === m) {
                return true;
            }
            const y = grains[j];
            if (y <= x) {
                const d = x - y;
                if (d > t) {
                    return false;
                }
                while (j < m && grains[j] <= x) {
                    ++j;
                }
                while (j < m && Math.min(d, grains[j] - x) + grains[j] - y <= t) {
                    ++j;
                }
            } else {
                while (j < m && grains[j] - x <= t) {
                    ++j;
                }
            }
        }
        return j === m;
    };

    while (l < r) {
        const mid = (l + r) >> 1;
        if (check(mid)) {
            r = mid;
        } else {
            l = mid + 1;
        }
    }
    return l;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
