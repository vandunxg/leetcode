---
comments: true
difficulty: Easy
rating: 1249
source: Weekly Contest 379 Q1
tags:
    - Array
---

<!-- problem:start -->

# [3000. Maximum Area of Longest Diagonal Rectangle](https://leetcode.com/problems/maximum-area-of-longest-diagonal-rectangle)

[中文文档](/solution/3000-3099/3000.Maximum%20Area%20of%20Longest%20Diagonal%20Rectangle/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên 2D <strong>đánh chỉ số từ 0</strong> <code>dimensions</code>.</p>

<p>Với mọi chỉ số <code>i</code>, <code>0 &lt;= i &lt; dimensions.length</code>, <code>dimensions[i][0]</code> biểu diễn chiều dài và <code>dimensions[i][1]</code> biểu diễn chiều rộng của hình chữ nhật<span style="font-size: 13.3333px;"> <code>i</code></span>.</p>

<p>Trả về <em><strong>diện tích</strong> của hình chữ nhật có đường chéo <strong>dài nhất</strong>. Nếu có nhiều hình chữ nhật có đường chéo dài nhất, trả về diện tích của hình chữ nhật có <strong>diện tích lớn nhất</strong>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> dimensions = [[9,3],[8,6]]
<strong>Đầu ra:</strong> 48
<strong>Giải thích:</strong>
Với index = 0, length = 9 và width = 3. Diagonal length = sqrt(9 * 9 + 3 * 3) = sqrt(90) &asymp;<!-- notionvc: 882cf44c-3b17-428e-9c65-9940810216f1 --> 9.487.
Với index = 1, length = 8 và width = 6. Diagonal length = sqrt(8 * 8 + 6 * 6) = sqrt(100) = 10.
Vì vậy, hình chữ nhật tại index 1 có đường chéo dài hơn, do đó ta trả về area = 8 * 6 = 48.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> dimensions = [[3,4],[4,3]]
<strong>Đầu ra:</strong> 12
<strong>Giải thích:</strong> Độ dài đường chéo của cả hai hình chữ nhật đều bằng 5, nên diện tích lớn nhất là 12.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= dimensions.length &lt;= 100</code></li>
	<li><code><font face="monospace">dimensions[i].length == 2</font></code></li>
	<li><code><font face="monospace">1 &lt;= dimensions[i][0], dimensions[i][1] &lt;= 100</font></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Có nhiều nhất $100$ hình chữ nhật, nên chỉ cần duyệt tuyến tính qua các đường chéo và diện tích là đủ. Việc so sánh căn bậc hai của các đường chéo có thể gây sai số số thực.
>
> Độ dài đường chéo được xác định bởi $l^2+w^2$, còn diện tích là $l \times w$. Tổng bình phương lớn hơn nghiêm ngặt tương ứng với đường chéo dài nhất; nếu hai tổng bằng nhau thì so sánh diện tích.
>
> Vì vậy, ta duy trì tổng bình phương tốt nhất $\textit{mx}$ và diện tích tương ứng $\textit{ans}$. Khi tổng bình phương tăng, ta thay thế diện tích; khi bằng nhau, ta chọn diện tích lớn hơn.

<!-- thinking:end -->

Theo định lý Pythagore, bình phương độ dài đường chéo của một hình chữ nhật là $l^2 + w^2$, trong đó $l$ và $w$ lần lượt là chiều dài và chiều rộng của hình chữ nhật.

Ta có thể duyệt qua tất cả các hình chữ nhật, tính bình phương độ dài đường chéo của từng hình, đồng thời lưu lại độ dài đường chéo lớn nhất và diện tích tương ứng.

Sau khi duyệt xong, ta trả về diện tích lớn nhất đã ghi nhận.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là số hình chữ nhật. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def areaOfMaxDiagonal(self, dimensions: List[List[int]]) -> int:
        ans = mx = 0
        for l, w in dimensions:
            t = l**2 + w**2
            if mx < t:
                mx = t
                ans = l * w
            elif mx == t:
                ans = max(ans, l * w)
        return ans
```

#### Java

```java
class Solution {
    public int areaOfMaxDiagonal(int[][] dimensions) {
        int ans = 0, mx = 0;
        for (var d : dimensions) {
            int l = d[0], w = d[1];
            int t = l * l + w * w;
            if (mx < t) {
                mx = t;
                ans = l * w;
            } else if (mx == t) {
                ans = Math.max(ans, l * w);
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int areaOfMaxDiagonal(vector<vector<int>>& dimensions) {
        int ans = 0, mx = 0;
        for (auto& d : dimensions) {
            int l = d[0], w = d[1];
            int t = l * l + w * w;
            if (mx < t) {
                mx = t;
                ans = l * w;
            } else if (mx == t) {
                ans = max(ans, l * w);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func areaOfMaxDiagonal(dimensions [][]int) (ans int) {
	mx := 0
	for _, d := range dimensions {
		l, w := d[0], d[1]
		t := l*l + w*w
		if mx < t {
			mx = t
			ans = l * w
		} else if mx == t {
			ans = max(ans, l*w)
		}
	}
	return
}
```

#### TypeScript

```ts
function areaOfMaxDiagonal(dimensions: number[][]): number {
    let [ans, mx] = [0, 0];
    for (const [l, w] of dimensions) {
        const t = l * l + w * w;
        if (mx < t) {
            mx = t;
            ans = l * w;
        } else if (mx === t) {
            ans = Math.max(ans, l * w);
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn area_of_max_diagonal(dimensions: Vec<Vec<i32>>) -> i32 {
        let mut ans = 0;
        let mut mx = 0;
        for d in dimensions {
            let l = d[0];
            let w = d[1];
            let t = l * l + w * w;
            if mx < t {
                mx = t;
                ans = l * w;
            } else if mx == t {
                ans = ans.max(l * w);
            }
        }
        ans
    }
}
```

#### C#

```cs
public class Solution {
    public int AreaOfMaxDiagonal(int[][] dimensions) {
        int ans = 0, mx = 0;
        foreach (var d in dimensions) {
            int l = d[0], w = d[1];
            int t = l * l + w * w;
            if (mx < t) {
                mx = t;
                ans = l * w;
            } else if (mx == t) {
                ans = Math.Max(ans, l * w);
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
