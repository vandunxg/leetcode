---
comments: true
difficulty: Medium
tags:
    - Hash Table
    - Math
    - Enumeration
---

<!-- problem:start -->

# [970. Powerful Integers](https://leetcode.com/problems/powerful-integers)

[中文文档](/solution/0900-0999/0970.Powerful%20Integers/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ba số nguyên <code>x</code>, <code>y</code> và <code>bound</code>, hãy trả về <em>danh sách tất cả các <strong>số mạnh</strong> có giá trị nhỏ hơn hoặc bằng</em> <code>bound</code>.</p>

<p>Một số nguyên được gọi là <strong>số mạnh</strong> nếu có thể biểu diễn dưới dạng <code>x<sup>i</sup> + y<sup>j</sup></code> với một số nguyên <code>i &gt;= 0</code> và một số nguyên <code>j &gt;= 0</code>.</p>

<p>Bạn có thể trả về kết quả theo <strong>bất kỳ thứ tự nào</strong>. Mỗi giá trị trong kết quả chỉ được xuất hiện <strong>tối đa một lần</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> x = 2, y = 3, bound = 10
<strong>Đầu ra:</strong> [2,3,4,5,7,9,10]
<strong>Giải thích:</strong>
2 = 2<sup>0</sup> + 3<sup>0</sup>
3 = 2<sup>1</sup> + 3<sup>0</sup>
4 = 2<sup>0</sup> + 3<sup>1</sup>
5 = 2<sup>1</sup> + 3<sup>1</sup>
7 = 2<sup>2</sup> + 3<sup>1</sup>
9 = 2<sup>3</sup> + 3<sup>0</sup>
10 = 2<sup>0</sup> + 3<sup>2</sup>
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> x = 3, y = 5, bound = 15
<strong>Đầu ra:</strong> [2,4,6,8,10,14]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= x, y &lt;= 100</code></li>
	<li><code>0 &lt;= bound &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Một số mạnh có dạng $x^i+y^j\le bound$. Vì $bound\le 10^6$, với cơ số từ $2$ trở lên, số mũ tối đa chỉ khoảng $20$. Các vòng lặp lồng nhau liệt kê các lũy thừa rồi thêm tổng vào set; nếu $x=1$ hoặc $y=1$, lũy thừa không tăng nên vòng lặp tương ứng chỉ chạy một lần.

<!-- thinking:end -->

Theo mô tả đề bài, một số mạnh có thể biểu diễn dưới dạng $x^i + y^j$, trong đó $i \geq 0$, $j \geq 0$.

Đề bài yêu cầu tìm tất cả số mạnh không vượt quá $bound$. Ta thấy $bound$ không lớn hơn $10^6$ và $2^{20} = 1048576 \gt 10^6$. Vì vậy, nếu $x \geq 2$, thì $i$ tối đa là $20$ để $x^i + y^j \leq bound$. Tương tự, nếu $y \geq 2$, thì $j$ tối đa là $20$.

Do đó, ta có thể dùng hai vòng lặp để liệt kê mọi giá trị $x^i$ và $y^j$, lần lượt ký hiệu là $a$ và $b$. Với mỗi cặp thỏa mãn $a + b \leq bound$, $a + b$ là một số mạnh. Ta dùng hash table để lưu các số mạnh thỏa điều kiện, rồi chuyển các phần tử trong hash table thành danh sách kết quả và trả về.

> Lưu ý, nếu $x=1$ hoặc $y=1$, giá trị $a$ hoặc $b$ luôn bằng $1$, vì vậy vòng lặp tương ứng chỉ cần chạy một lần rồi thoát.

Độ phức tạp thời gian là $O(\log^2 bound)$ và độ phức tạp không gian là $O(\log^2 bound)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def powerfulIntegers(self, x: int, y: int, bound: int) -> List[int]:
        ans = set()
        a = 1
        while a <= bound:
            b = 1
            while a + b <= bound:
                ans.add(a + b)
                b *= y
                if y == 1:
                    break
            if x == 1:
                break
            a *= x
        return list(ans)
```

#### Java

```java
class Solution {
    public List<Integer> powerfulIntegers(int x, int y, int bound) {
        Set<Integer> ans = new HashSet<>();
        for (int a = 1; a <= bound; a *= x) {
            for (int b = 1; a + b <= bound; b *= y) {
                ans.add(a + b);
                if (y == 1) {
                    break;
                }
            }
            if (x == 1) {
                break;
            }
        }
        return new ArrayList<>(ans);
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> powerfulIntegers(int x, int y, int bound) {
        unordered_set<int> ans;
        for (int a = 1; a <= bound; a *= x) {
            for (int b = 1; a + b <= bound; b *= y) {
                ans.insert(a + b);
                if (y == 1) {
                    break;
                }
            }
            if (x == 1) {
                break;
            }
        }
        return vector<int>(ans.begin(), ans.end());
    }
};
```

#### Go

```go
func powerfulIntegers(x int, y int, bound int) (ans []int) {
	s := map[int]struct{}{}
	for a := 1; a <= bound; a *= x {
		for b := 1; a+b <= bound; b *= y {
			s[a+b] = struct{}{}
			if y == 1 {
				break
			}
		}
		if x == 1 {
			break
		}
	}
	for x := range s {
		ans = append(ans, x)
	}
	return ans
}
```

#### TypeScript

```ts
function powerfulIntegers(x: number, y: number, bound: number): number[] {
    const ans = new Set<number>();
    for (let a = 1; a <= bound; a *= x) {
        for (let b = 1; a + b <= bound; b *= y) {
            ans.add(a + b);
            if (y === 1) {
                break;
            }
        }
        if (x === 1) {
            break;
        }
    }
    return Array.from(ans);
}
```

#### JavaScript

```js
/**
 * @param {number} x
 * @param {number} y
 * @param {number} bound
 * @return {number[]}
 */
var powerfulIntegers = function (x, y, bound) {
    const ans = new Set();
    for (let a = 1; a <= bound; a *= x) {
        for (let b = 1; a + b <= bound; b *= y) {
            ans.add(a + b);
            if (y === 1) {
                break;
            }
        }
        if (x === 1) {
            break;
        }
    }
    return [...ans];
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
