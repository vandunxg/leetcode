---
comments: true
difficulty: Easy
rating: 1363
source: Weekly Contest 479 Q1
tags:
    - Array
    - Sorting
---

<!-- problem:start -->

# [3769. Sort Integers by Binary Reflection](https://leetcode.com/problems/sort-integers-by-binary-reflection)

[中文文档](/solution/3700-3799/3769.Sort%20Integers%20by%20Binary%20Reflection/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p><strong>Phép phản chiếu nhị phân</strong> của một số nguyên <strong>dương</strong> được định nghĩa là số nhận được bằng cách đảo ngược thứ tự các chữ số <strong>nhị phân</strong> của nó (bỏ qua các số 0 ở đầu), rồi diễn giải số nhị phân thu được thành số thập phân.</p>

<p>Hãy sắp xếp mảng theo thứ tự <strong>tăng dần</strong> dựa trên phép phản chiếu nhị phân của từng phần tử. Nếu hai số khác nhau có cùng phép phản chiếu nhị phân, số <strong>nhỏ hơn</strong> trong hai số ban đầu phải được đặt trước.</p>

<p>Trả về mảng sau khi sắp xếp.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,5,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[4,4,5]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các phép phản chiếu nhị phân là:</p>

<ul>
	<li>4 -&gt; (nhị phân) <code>100</code> -&gt; (đảo ngược) <code>001</code> -&gt; 1</li>
	<li>5 -&gt; (nhị phân) <code>101</code> -&gt; (đảo ngược) <code>101</code> -&gt; 5</li>
	<li>4 -&gt; (nhị phân) <code>100</code> -&gt; (đảo ngược) <code>001</code> -&gt; 1</li>
</ul>
Sắp xếp theo các giá trị phản chiếu cho ta <code>[4, 4, 5]</code>.</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,6,5,8]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[8,3,6,5]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các phép phản chiếu nhị phân là:</p>

<ul>
	<li>3 -&gt; (nhị phân) <code>11</code> -&gt; (đảo ngược) <code>11</code> -&gt; 3</li>
	<li>6 -&gt; (nhị phân) <code>110</code> -&gt; (đảo ngược) <code>011</code> -&gt; 3</li>
	<li>5 -&gt; (nhị phân) <code>101</code> -&gt; (đảo ngược) <code>101</code> -&gt; 5</li>
	<li>8 -&gt; (nhị phân) <code>1000</code> -&gt; (đảo ngược) <code>0001</code> -&gt; 1</li>
</ul>
Sắp xếp theo các giá trị phản chiếu cho ta <code>[8, 3, 6, 5]</code>.<br />
Lưu ý rằng 3 và 6 có cùng phép phản chiếu, nên chúng được sắp xếp theo thứ tự tăng dần của giá trị ban đầu.</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp tùy chỉnh

<!-- thinking:start -->

> **Tư duy**
>
> Khóa sắp xếp là phép đảo ngược nhị phân, bỏ qua các số 0 ở đầu, với giá trị ban đầu làm tiêu chí phụ khi bằng nhau. Vì $n\le 100$, ta lấy lần lượt bit thấp nhất của mỗi số nguyên để tạo phép phản chiếu rồi sắp xếp theo $(f(x),x)$.

<!-- thinking:end -->

Ta định nghĩa hàm $f(x)$ để tính giá trị phép phản chiếu nhị phân của số nguyên $x$. Cụ thể, ta liên tục lấy bit thấp nhất của $x$ và thêm nó vào cuối kết quả $y$ cho đến khi $x$ trở thành $0$.

Sau đó, ta sắp xếp mảng $\textit{nums}$ với khóa sắp xếp là tuple $(f(x), x)$ gồm giá trị phép phản chiếu nhị phân và giá trị ban đầu của mỗi phần tử. Điều này đảm bảo rằng khi hai phần tử có cùng giá trị phép phản chiếu nhị phân, phần tử có giá trị ban đầu nhỏ hơn sẽ được đặt trước.

Cuối cùng, ta trả về mảng đã sắp xếp.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(\log n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sortByReflection(self, nums: List[int]) -> List[int]:
        def f(x: int) -> int:
            y = 0
            while x:
                y = y << 1 | (x & 1)
                x >>= 1
            return y

        nums.sort(key=lambda x: (f(x), x))
        return nums
```

#### Java

```java
class Solution {
    public int[] sortByReflection(int[] nums) {
        int n = nums.length;
        Integer[] a = new Integer[n];
        Arrays.setAll(a, i -> nums[i]);

        Arrays.sort(a, (u, v) -> {
            int fu = f(u);
            int fv = f(v);
            if (fu != fv) {
                return Integer.compare(fu, fv);
            }
            return Integer.compare(u, v);
        });

        for (int i = 0; i < n; i++) nums[i] = a[i];
        return nums;
    }

    private int f(int x) {
        int y = 0;
        while (x != 0) {
            y = (y << 1) | (x & 1);
            x >>= 1;
        }
        return y;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> sortByReflection(vector<int>& nums) {
        auto f = [](int x) {
            int y = 0;
            while (x) {
                y = (y << 1) | (x & 1);
                x >>= 1;
            }
            return y;
        };

        sort(nums.begin(), nums.end(), [&](int a, int b) {
            int fa = f(a);
            int fb = f(b);
            if (fa != fb) {
                return fa < fb;
            }
            return a < b;
        });

        return nums;
    }
};
```

#### Go

```go
func sortByReflection(nums []int) []int {
	f := func(x int) int {
		y := 0
		for x != 0 {
			y = (y << 1) | (x & 1)
			x >>= 1
		}
		return y
	}

	sort.Slice(nums, func(i, j int) bool {
		fi := f(nums[i])
		fj := f(nums[j])
		if fi != fj {
			return fi < fj
		}
		return nums[i] < nums[j]
	})

	return nums
}
```

#### TypeScript

```ts
function sortByReflection(nums: number[]): number[] {
    const f = (x: number): number => {
        let y = 0;
        for (; x; x >>= 1) {
            y = (y << 1) | (x & 1);
        }
        return y;
    };

    nums.sort((a, b) => {
        const fa = f(a);
        const fb = f(b);
        if (fa !== fb) {
            return fa - fb;
        }
        return a - b;
    });

    return nums;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
