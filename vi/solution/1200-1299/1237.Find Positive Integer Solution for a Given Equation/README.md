---
comments: true
difficulty: Medium
rating: 1404
source: Weekly Contest 160 Q1
tags:
    - Math
    - Two Pointers
    - Binary Search
    - Interactive
---

<!-- problem:start -->

# [1237. Find Positive Integer Solution for a Given Equation](https://leetcode.com/problems/find-positive-integer-solution-for-a-given-equation)

[中文文档](/solution/1200-1299/1237.Find%20Positive%20Integer%20Solution%20for%20a%20Given%20Equation/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hàm có thể gọi <code>f(x, y)</code> với <strong>công thức bị ẩn</strong> và giá trị <code>z</code>, hãy suy ra công thức và trả về <em>mọi cặp số nguyên dương </em><code>x</code><em> và </em><code>y</code><em> sao cho </em><code>f(x,y) == z</code>. Có thể trả các cặp theo bất kỳ thứ tự nào.</p>

<p>Dù công thức chính xác bị ẩn, hàm số tăng đơn điệu, tức là:</p>

<ul>
	<li><code>f(x, y) &lt; f(x + 1, y)</code></li>
	<li><code>f(x, y) &lt; f(x, y + 1)</code></li>
</ul>

<p>Interface của hàm được định nghĩa như sau:</p>

<pre>
interface CustomFunction {
public:
  // Returns some positive integer f(x, y) for two positive integers x and y based on a formula.
  int f(int x, int y);
};
</pre>

<p>Lời giải của bạn sẽ được chấm như sau:</p>

<ul>
	<li>Judge có danh sách gồm <code>9</code> cách cài đặt ẩn của <code>CustomFunction</code>, cùng với cách tạo <strong>đáp án chuẩn</strong> gồm mọi cặp hợp lệ ứng với một giá trị <code>z</code>.</li>
	<li>Judge nhận hai input: <code>function_id</code> (để xác định cách cài đặt nào sẽ được dùng để kiểm tra code của bạn) và giá trị đích <code>z</code>.</li>
	<li>Judge sẽ gọi hàm <code>findSolution</code> của bạn và so sánh kết quả với <strong>đáp án chuẩn</strong>.</li>
	<li>Nếu kết quả khớp với <strong>đáp án chuẩn</strong>, lời giải của bạn sẽ được <code>Accepted</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> function_id = 1, z = 5
<strong>Đầu ra:</strong> [[1,4],[2,3],[3,2],[4,1]]
<strong>Giải thích:</strong> Công thức ẩn của function_id = 1 là f(x, y) = x + y.
Các giá trị nguyên dương sau của x và y khiến f(x, y) bằng 5:
x=1, y=4 -&gt; f(1, 4) = 1 + 4 = 5.
x=2, y=3 -&gt; f(2, 3) = 2 + 3 = 5.
x=3, y=2 -&gt; f(3, 2) = 3 + 2 = 5.
x=4, y=1 -&gt; f(4, 1) = 4 + 1 = 5.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> function_id = 2, z = 5
<strong>Đầu ra:</strong> [[1,5],[5,1]]
<strong>Giải thích:</strong> Công thức ẩn của function_id = 2 là f(x, y) = x * y.
Các giá trị nguyên dương sau của x và y khiến f(x, y) bằng 5:
x=1, y=5 -&gt; f(1, 5) = 1 * 5 = 5.
x=5, y=1 -&gt; f(5, 1) = 5 * 1 = 5.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= function_id &lt;= 9</code></li>
	<li><code>1 &lt;= z &lt;= 100</code></li>
	<li>Đảm bảo các nghiệm của <code>f(x, y) == z</code> nằm trong phạm vi <code>1 &lt;= x, y &lt;= 1000</code>.</li>
	<li>Cũng đảm bảo rằng <code>f(x, y)</code> nằm trong phạm vi số nguyên có dấu 32 bit khi <code>1 &lt;= x, y &lt;= 1000</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> $f$ tăng nghiêm ngặt theo cả hai tham số, $z \le 100$ và nghiệm nằm trong khoảng $1\ldots 1000$. Duyệt tuyến tính hai vòng cần tối đa $10^6$ lần gọi; cách này vẫn chạy được nhưng có thể tối ưu hơn. Với $x$ cố định, $f(x,\cdot)$ đơn điệu nên có thể tìm kiếm nhị phân $y$.
>
> Ta tìm kiếm $y$ cho từng $x$ và ghi lại các cặp thỏa mãn. Tính đơn điệu giúp vòng lặp bên trong có độ phức tạp logarithmic.

<!-- thinking:end -->

Theo đề bài, hàm $f(x, y)$ tăng đơn điệu. Vì vậy, ta có thể duyệt $x$, rồi tìm kiếm nhị phân $y$ trong $[1,...z]$ để tìm giá trị sao cho $f(x, y) = z$. Nếu tìm thấy, thêm $(x, y)$ vào đáp án.

Độ phức tạp thời gian là $O(n \log n)$, trong đó $n$ là giá trị của $z$; độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
"""
   This is the custom function interface.
   You should not implement it, or speculate about its implementation
   class CustomFunction:
       # Returns f(x, y) for any given positive integers x and y.
       # Note that f(x, y) is increasing with respect to both x and y.
       # i.e. f(x, y) < f(x + 1, y), f(x, y) < f(x, y + 1)
       def f(self, x, y):

"""


class Solution:
    def findSolution(self, customfunction: "CustomFunction", z: int) -> List[List[int]]:
        ans = []
        for x in range(1, z + 1):
            y = 1 + bisect_left(
                range(1, z + 1), z, key=lambda y: customfunction.f(x, y)
            )
            if customfunction.f(x, y) == z:
                ans.append([x, y])
        return ans
```

#### Java

```java
/*
 * // This is the custom function interface.
 * // You should not implement it, or speculate about its implementation
 * class CustomFunction {
 *     // Returns f(x, y) for any given positive integers x and y.
 *     // Note that f(x, y) is increasing with respect to both x and y.
 *     // i.e. f(x, y) < f(x + 1, y), f(x, y) < f(x, y + 1)
 *     public int f(int x, int y);
 * };
 */

class Solution {
    public List<List<Integer>> findSolution(CustomFunction customfunction, int z) {
        List<List<Integer>> ans = new ArrayList<>();
        for (int x = 1; x <= 1000; ++x) {
            int l = 1, r = 1000;
            while (l < r) {
                int mid = (l + r) >> 1;
                if (customfunction.f(x, mid) >= z) {
                    r = mid;
                } else {
                    l = mid + 1;
                }
            }
            if (customfunction.f(x, l) == z) {
                ans.add(Arrays.asList(x, l));
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
/*
 * // This is the custom function interface.
 * // You should not implement it, or speculate about its implementation
 * class CustomFunction {
 * public:
 *     // Returns f(x, y) for any given positive integers x and y.
 *     // Note that f(x, y) is increasing with respect to both x and y.
 *     // i.e. f(x, y) < f(x + 1, y), f(x, y) < f(x, y + 1)
 *     int f(int x, int y);
 * };
 */

class Solution {
public:
    vector<vector<int>> findSolution(CustomFunction& customfunction, int z) {
        vector<vector<int>> ans;
        for (int x = 1; x <= 1000; ++x) {
            int l = 1, r = 1000;
            while (l < r) {
                int mid = (l + r) >> 1;
                if (customfunction.f(x, mid) >= z) {
                    r = mid;
                } else {
                    l = mid + 1;
                }
            }
            if (customfunction.f(x, l) == z) {
                ans.push_back({x, l});
            }
        }
        return ans;
    }
};
```

#### Go

```go
/**
 * This is the declaration of customFunction API.
 * @param  x    int
 * @param  x    int
 * @return 	    Returns f(x, y) for any given positive integers x and y.
 *			    Note that f(x, y) is increasing with respect to both x and y.
 *              i.e. f(x, y) < f(x + 1, y), f(x, y) < f(x, y + 1)
 */

func findSolution(customFunction func(int, int) int, z int) (ans [][]int) {
	for x := 1; x <= 1000; x++ {
		y := 1 + sort.Search(999, func(y int) bool { return customFunction(x, y+1) >= z })
		if customFunction(x, y) == z {
			ans = append(ans, []int{x, y})
		}
	}
	return
}
```

#### TypeScript

```ts
/**
 * // This is the CustomFunction's API interface.
 * // You should not implement it, or speculate about its implementation
 * class CustomFunction {
 *      f(x: number, y: number): number {}
 * }
 */

function findSolution(customfunction: CustomFunction, z: number): number[][] {
    const ans: number[][] = [];
    for (let x = 1; x <= 1000; ++x) {
        let l = 1;
        let r = 1000;
        while (l < r) {
            const mid = (l + r) >> 1;
            if (customfunction.f(x, mid) >= z) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        if (customfunction.f(x, l) == z) {
            ans.push([x, l]);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Two Pointers

<!-- thinking:start -->

> **Tư duy**
>
> Tìm kiếm nhị phân cho từng $x$ vẫn tốn thêm hệ số log. Bắt đầu tại $(1,1000)$: nếu giá trị nhỏ hơn mục tiêu thì chỉ có thể tăng $x$ (giảm $y$ sẽ khiến giá trị còn nhỏ hơn); nếu giá trị lớn hơn mục tiêu thì chỉ có thể giảm $y$. Khi bằng mục tiêu, ghi nhận cặp đó rồi dịch cả hai con trỏ. Số lần gọi là $O(z)$ và cách làm tận dụng tính đơn điệu theo cả hai chiều.

<!-- thinking:end -->

Ta định nghĩa hai con trỏ $x$ và $y$, ban đầu $x = 1$, $y = z$.

- Nếu $f(x, y) = z$, ta thêm $(x, y)$ vào đáp án, sau đó $x \leftarrow x + 1$, $y \leftarrow y - 1$;
- Nếu $f(x, y) \lt z$, thì với mọi $y' \lt y$, ta có $f(x, y') \lt f(x, y) \lt z$. Vì vậy, không thể giảm $y$; ta chỉ có thể tăng $x$, tức $x \leftarrow x + 1$;
- Nếu $f(x, y) \gt z$, thì với mọi $x' \gt x$, ta có $f(x', y) \gt f(x, y) \gt z$. Vì vậy, không thể tăng $x$; ta chỉ có thể giảm $y$, tức $y \leftarrow y - 1$.

Khi vòng lặp kết thúc, trả về đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là giá trị của $z$; độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
"""
   This is the custom function interface.
   You should not implement it, or speculate about its implementation
   class CustomFunction:
       # Returns f(x, y) for any given positive integers x and y.
       # Note that f(x, y) is increasing with respect to both x and y.
       # i.e. f(x, y) < f(x + 1, y), f(x, y) < f(x, y + 1)
       def f(self, x, y):

"""


class Solution:
    def findSolution(self, customfunction: "CustomFunction", z: int) -> List[List[int]]:
        ans = []
        x, y = 1, 1000
        while x <= 1000 and y:
            t = customfunction.f(x, y)
            if t < z:
                x += 1
            elif t > z:
                y -= 1
            else:
                ans.append([x, y])
                x, y = x + 1, y - 1
        return ans
```

#### Java

```java
/*
 * // This is the custom function interface.
 * // You should not implement it, or speculate about its implementation
 * class CustomFunction {
 *     // Returns f(x, y) for any given positive integers x and y.
 *     // Note that f(x, y) is increasing with respect to both x and y.
 *     // i.e. f(x, y) < f(x + 1, y), f(x, y) < f(x, y + 1)
 *     public int f(int x, int y);
 * };
 */

class Solution {
    public List<List<Integer>> findSolution(CustomFunction customfunction, int z) {
        List<List<Integer>> ans = new ArrayList<>();
        int x = 1, y = 1000;
        while (x <= 1000 && y > 0) {
            int t = customfunction.f(x, y);
            if (t < z) {
                x++;
            } else if (t > z) {
                y--;
            } else {
                ans.add(Arrays.asList(x++, y--));
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
/*
 * // This is the custom function interface.
 * // You should not implement it, or speculate about its implementation
 * class CustomFunction {
 * public:
 *     // Returns f(x, y) for any given positive integers x and y.
 *     // Note that f(x, y) is increasing with respect to both x and y.
 *     // i.e. f(x, y) < f(x + 1, y), f(x, y) < f(x, y + 1)
 *     int f(int x, int y);
 * };
 */

class Solution {
public:
    vector<vector<int>> findSolution(CustomFunction& customfunction, int z) {
        vector<vector<int>> ans;
        int x = 1, y = 1000;
        while (x <= 1000 && y) {
            int t = customfunction.f(x, y);
            if (t < z) {
                x++;
            } else if (t > z) {
                y--;
            } else {
                ans.push_back({x++, y--});
            }
        }
        return ans;
    }
};
```

#### Go

```go
/**
 * This is the declaration of customFunction API.
 * @param  x    int
 * @param  x    int
 * @return 	    Returns f(x, y) for any given positive integers x and y.
 *			    Note that f(x, y) is increasing with respect to both x and y.
 *              i.e. f(x, y) < f(x + 1, y), f(x, y) < f(x, y + 1)
 */

func findSolution(customFunction func(int, int) int, z int) (ans [][]int) {
	x, y := 1, 1000
	for x <= 1000 && y > 0 {
		t := customFunction(x, y)
		if t < z {
			x++
		} else if t > z {
			y--
		} else {
			ans = append(ans, []int{x, y})
			x, y = x+1, y-1
		}
	}
	return
}
```

#### TypeScript

```ts
/**
 * // This is the CustomFunction's API interface.
 * // You should not implement it, or speculate about its implementation
 * class CustomFunction {
 *      f(x: number, y: number): number {}
 * }
 */

function findSolution(customfunction: CustomFunction, z: number): number[][] {
    let x = 1;
    let y = 1000;
    const ans: number[][] = [];
    while (x <= 1000 && y) {
        const t = customfunction.f(x, y);
        if (t < z) {
            ++x;
        } else if (t > z) {
            --y;
        } else {
            ans.push([x--, y--]);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
