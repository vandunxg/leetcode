---
comments: true
difficulty: Easy
tags:
    - Greedy
    - Array
---

<!-- problem:start -->

# [605. Can Place Flowers](https://leetcode.com/problems/can-place-flowers)

[中文文档](/solution/0600-0699/0605.Can%20Place%20Flowers/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có một luống hoa dài, trong đó một số ô đã được trồng hoa và một số ô còn trống. Tuy nhiên, không thể trồng hoa ở hai ô <strong>liền kề</strong>.</p>

<p>Cho một mảng số nguyên <code>flowerbed</code> chứa các giá trị <code>0</code>&#39;s và <code>1</code>&#39;s, trong đó <code>0</code> nghĩa là ô trống và <code>1</code> nghĩa là ô đã có hoa, cùng một số nguyên <code>n</code>. Trả về <code>true</code>&nbsp;<em>nếu</em> có thể trồng <code>n</code> <em>bông hoa mới trong</em> <code>flowerbed</code> <em>mà không vi phạm quy tắc không trồng hoa ở hai ô liền kề, và</em> <code>false</code> <em>trong trường hợp ngược lại</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> flowerbed = [1,0,0,0,1], n = 1
<strong>Đầu ra:</strong> true
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> flowerbed = [1,0,0,0,1], n = 2
<strong>Đầu ra:</strong> false
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= flowerbed.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>flowerbed[i]</code> là <code>0</code> hoặc <code>1</code>.</li>
	<li>Trong <code>flowerbed</code> không có hai bông hoa nào ở hai ô liền kề.</li>
	<li><code>0 &lt;= n &lt;= flowerbed.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy

<!-- thinking:start -->

> **Tư duy**
>
> Hoa không thể nằm ở hai ô liền kề, và ta chỉ cần biết có thể trồng đủ $n$ bông hay không. Với độ dài lên đến $2\times 10^4$, không thể liệt kê mọi tập hợp vị trí trồng.
>
> Trồng hoa ngay khi gặp một khoảng gồm ba số 0 sẽ không ngăn ta trồng thêm ở các ô phía sau. Thêm một số $0$ ở hai đầu, trồng theo chiến lược tham lam rồi kiểm tra xem $n$ còn lớn hơn $0$ hay không.

<!-- thinking:end -->

Ta duyệt trực tiếp mảng $flowerbed$. Với mỗi vị trí $i$, nếu $flowerbed[i]=0$ và các vị trí liền kề bên trái, bên phải cũng đều là $0$, ta có thể trồng hoa tại vị trí này; nếu không thì không thể. Cuối cùng, ta đếm số bông hoa có thể trồng. Nếu số lượng này không nhỏ hơn $n$, trả về $true$; ngược lại, trả về $false$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $flowerbed$. Ta chỉ cần duyệt mảng một lần. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canPlaceFlowers(self, flowerbed: List[int], n: int) -> bool:
        flowerbed = [0] + flowerbed + [0]
        for i in range(1, len(flowerbed) - 1):
            if sum(flowerbed[i - 1 : i + 2]) == 0:
                flowerbed[i] = 1
                n -= 1
        return n <= 0
```

#### Java

```java
class Solution {
    public boolean canPlaceFlowers(int[] flowerbed, int n) {
        int m = flowerbed.length;
        for (int i = 0; i < m; ++i) {
            int l = i == 0 ? 0 : flowerbed[i - 1];
            int r = i == m - 1 ? 0 : flowerbed[i + 1];
            if (l + flowerbed[i] + r == 0) {
                flowerbed[i] = 1;
                --n;
            }
        }
        return n <= 0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canPlaceFlowers(vector<int>& flowerbed, int n) {
        int m = flowerbed.size();
        for (int i = 0; i < m; ++i) {
            int l = i == 0 ? 0 : flowerbed[i - 1];
            int r = i == m - 1 ? 0 : flowerbed[i + 1];
            if (l + flowerbed[i] + r == 0) {
                flowerbed[i] = 1;
                --n;
            }
        }
        return n <= 0;
    }
};
```

#### Go

```go
func canPlaceFlowers(flowerbed []int, n int) bool {
	m := len(flowerbed)
	for i, v := range flowerbed {
		l, r := 0, 0
		if i > 0 {
			l = flowerbed[i-1]
		}
		if i < m-1 {
			r = flowerbed[i+1]
		}
		if l+v+r == 0 {
			flowerbed[i] = 1
			n--
		}
	}
	return n <= 0
}
```

#### TypeScript

```ts
function canPlaceFlowers(flowerbed: number[], n: number): boolean {
    const m = flowerbed.length;
    for (let i = 0; i < m; ++i) {
        const l = i === 0 ? 0 : flowerbed[i - 1];
        const r = i === m - 1 ? 0 : flowerbed[i + 1];
        if (l + flowerbed[i] + r === 0) {
            flowerbed[i] = 1;
            --n;
        }
    }
    return n <= 0;
}
```

#### Rust

```rust
impl Solution {
    pub fn can_place_flowers(flowerbed: Vec<i32>, n: i32) -> bool {
        let (mut flowers, mut cnt) = (vec![0], 0);
        flowers.append(&mut flowerbed.clone());
        flowers.push(0);

        for i in 1..flowers.len() - 1 {
            let (l, r) = (flowers[i - 1], flowers[i + 1]);
            if l + flowers[i] + r == 0 {
                flowers[i] = 1;
                cnt += 1;
            }
        }
        cnt >= n
    }
}
```

#### PHP

```php
class Solution {
    /**
     * @param Integer[] $flowerbed
     * @param Integer $n
     * @return Boolean
     */
    function canPlaceFlowers($flowerbed, $n) {
        array_push($flowerbed, 0);
        array_unshift($flowerbed, 0);
        for ($i = 1; $i < count($flowerbed) - 1; $i++) {
            if ($flowerbed[$i] === 0) {
                if ($flowerbed[$i - 1] === 0 && $flowerbed[$i + 1] === 0) {
                    $flowerbed[$i] = 1;
                    $n--;
                }
            }
        }
        return $n <= 0;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
