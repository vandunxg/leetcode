---
comments: true
difficulty: Hard
---

<!-- problem:start -->

# [08.13. Pile Box](https://leetcode.cn/problems/pile-box-lcci)

[中文文档](/lcci/08.13.Pile%20Box/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có một chồng gồm n chiếc hộp, với chiều rộng wi, chiều cao hi và chiều sâu di. Các hộp không thể xoay và chỉ có thể xếp chồng lên nhau nếu mỗi hộp trong chồng đều lớn hơn nghiêm ngặt hộp bên trên về chiều rộng, chiều cao và chiều sâu. Hãy triển khai một method để tính chiều cao lớn nhất có thể của chồng hộp. Chiều cao của một chồng là tổng chiều cao của từng hộp.</p>
<p>Đầu vào sử dụng <code>[wi, di, hi]</code>&nbsp;để biểu diễn mỗi hộp.</p>
<p><strong>Ví dụ 1:</strong></p>
<pre>

<strong> Đầu vào</strong>: box = [[1, 1, 1], [2, 2, 2], [3, 3, 3]]

<strong> Đầu ra</strong>: 6

</pre>
<p><strong>Ví dụ 2:</strong></p>
<pre>

<strong> Đầu vào</strong>: box = [[1, 1, 1], [2, 3, 4], [2, 6, 7], [3, 4, 5]]

<strong> Đầu ra</strong>: 10

</pre>
<p><strong>Lưu ý:</strong></p>
<ol>
	<li><code>box.length &lt;= 3000</code></li>
</ol>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Một hộp chỉ có thể đặt trên một hộp lớn hơn nghiêm ngặt ở cả ba chiều. Việc tìm kiếm thứ tự xếp chồng sẽ có số trường hợp tăng theo giai thừa.
>
> Sắp xếp theo chiều rộng đưa bài toán về việc tìm dãy tăng trong không gian 3 chiều, có thể giải bằng quy hoạch động kiểu LIS với độ phức tạp $O(n^2)$. Với các chiều rộng bằng nhau, sắp xếp chiều sâu theo thứ tự giảm dần để chúng không bị xem là có thể xếp chồng lên nhau.
>
> $f[i]$ là chiều cao tốt nhất khi hộp $i$ nằm dưới cùng; các hộp đứng trước có chiều sâu và chiều cao nhỏ hơn sẽ cập nhật giá trị này, sau đó cộng chiều cao của $box[i]$ vào.

<!-- thinking:end -->

Trước tiên, chúng ta sắp xếp các hộp theo chiều rộng tăng dần và chiều sâu giảm dần, sau đó dùng quy hoạch động để giải bài toán.

Ta định nghĩa $f[i]$ là chiều cao lớn nhất khi hộp thứ $i$ nằm dưới cùng. Với $f[i]$, ta duyệt các $j \in [0, i)$; nếu $box[j][1] < box[i][1]$ và $box[j][2] < box[i][2]$, ta có thể đặt hộp thứ $j$ lên trên hộp thứ $i$, khi đó $f[i] = \max\{f[i], f[j]\}$. Cuối cùng, ta cộng chiều cao của hộp thứ $i$ vào $f[i]$ để nhận được giá trị cuối cùng của $f[i]$.

Đáp án là giá trị lớn nhất trong $f$.

Độ phức tạp thời gian là $O(n^2)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là số lượng hộp.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def pileBox(self, box: List[List[int]]) -> int:
        box.sort(key=lambda x: (x[0], -x[1]))
        n = len(box)
        f = [0] * n
        for i in range(n):
            for j in range(i):
                if box[j][1] < box[i][1] and box[j][2] < box[i][2]:
                    f[i] = max(f[i], f[j])
            f[i] += box[i][2]
        return max(f)
```

#### Java

```java
class Solution {
    public int pileBox(int[][] box) {
        Arrays.sort(box, (a, b) -> a[0] == b[0] ? b[1] - a[1] : a[0] - b[0]);
        int n = box.length;
        int[] f = new int[n];
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                if (box[j][1] < box[i][1] && box[j][2] < box[i][2]) {
                    f[i] = Math.max(f[i], f[j]);
                }
            }
            f[i] += box[i][2];
            ans = Math.max(ans, f[i]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int pileBox(vector<vector<int>>& box) {
        sort(box.begin(), box.end(), [](const vector<int>& a, const vector<int>& b) {
            return a[0] < b[0] || (a[0] == b[0] && b[1] < a[1]);
        });
        int n = box.size();
        int f[n];
        memset(f, 0, sizeof(f));
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                if (box[j][1] < box[i][1] && box[j][2] < box[i][2]) {
                    f[i] = max(f[i], f[j]);
                }
            }
            f[i] += box[i][2];
        }
        return *max_element(f, f + n);
    }
};
```

#### Go

```go
func pileBox(box [][]int) int {
	sort.Slice(box, func(i, j int) bool {
		a, b := box[i], box[j]
		return a[0] < b[0] || (a[0] == b[0] && b[1] < a[1])
	})
	n := len(box)
	f := make([]int, n)
	for i := 0; i < n; i++ {
		for j := 0; j < i; j++ {
			if box[j][1] < box[i][1] && box[j][2] < box[i][2] {
				f[i] = max(f[i], f[j])
			}
		}
		f[i] += box[i][2]
	}
	return slices.Max(f)
}
```

#### TypeScript

```ts
function pileBox(box: number[][]): number {
    box.sort((a, b) => (a[0] === b[0] ? b[1] - a[1] : a[0] - b[0]));
    const n = box.length;
    const f: number[] = new Array(n).fill(0);
    let ans: number = 0;
    for (let i = 0; i < n; ++i) {
        for (let j = 0; j < i; ++j) {
            if (box[j][1] < box[i][1] && box[j][2] < box[i][2]) {
                f[i] = Math.max(f[i], f[j]);
            }
        }
        f[i] += box[i][2];
        ans = Math.max(ans, f[i]);
    }
    return ans;
}
```

#### Swift

```swift
class Solution {
    func pileBox(_ box: [[Int]]) -> Int {
        let boxes = box.sorted {
            if $0[0] == $1[0] {
                return $0[1] > $1[1]
            } else {
                return $0[0] < $1[0]
            }
        }

        let n = boxes.count
        var f = Array(repeating: 0, count: n)
        var ans = 0

        for i in 0..<n {
            f[i] = boxes[i][2]
            for j in 0..<i {
                if boxes[j][1] < boxes[i][1] && boxes[j][2] < boxes[i][2] {
                    f[i] = max(f[i], f[j] + boxes[i][2])
                }
            }
            ans = max(ans, f[i])
        }

        return ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
