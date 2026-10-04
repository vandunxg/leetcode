---
comments: true
difficulty: Hard
tags:
    - Array
    - Math
---

<!-- problem:start -->

# [2647. Color the Triangle Red 🔒](https://leetcode.com/problems/color-the-triangle-red)

[Tài liệu tiếng Trung](/solution/2600-2699/2647.Color%20the%20Triangle%20Red/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>n</code>. Xét một tam giác đều có độ dài cạnh <code>n</code>, được chia thành <code>n<sup>2</sup></code> tam giác đều đơn vị. Tam giác có <code>n</code> hàng được đánh số <strong>từ 1</strong>, trong đó hàng thứ <code>i<sup>th</sup></code> có <code>2i - 1</code> tam giác đều đơn vị.</p>

<p>Các tam giác trong hàng thứ <code>i<sup>th</sup></code> cũng được đánh số <strong>từ 1</strong>, với tọa độ từ <code>(i, 1)</code> đến <code>(i, 2i - 1)</code>. Hình dưới đây minh họa một tam giác có độ dài cạnh <code>4</code> cùng cách đánh số các tam giác.</p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2600-2699/2647.Color%20the%20Triangle%20Red/images/triangle4.jpg" style="width: 402px; height: 242px;" />
<p>Hai tam giác là <strong>hàng xóm</strong> nếu chúng <strong>chung một cạnh</strong>. Ví dụ:</p>

<ul>
	<li>Các tam giác <code>(1,1)</code> và <code>(2,2)</code> là hàng xóm</li>
	<li>Các tam giác <code>(3,2)</code> và <code>(3,3)</code> là hàng xóm.</li>
	<li>Các tam giác <code>(2,2)</code> và <code>(3,3)</code> không phải hàng xóm vì chúng không chung cạnh nào.</li>
</ul>

<p>Ban đầu, tất cả các tam giác đơn vị đều có màu <strong>trắng</strong>. Bạn muốn chọn <code>k</code> tam giác và tô chúng thành <strong>đỏ</strong>. Sau đó, chúng ta sẽ chạy thuật toán sau:</p>

<ol>
	<li>Chọn một tam giác trắng có <strong>ít nhất hai</strong> hàng xóm màu đỏ.

    <ul>
        <li>Nếu không có tam giác nào như vậy, dừng thuật toán.</li>
    </ul>
    </li>
    <li>Tô tam giác đó thành <strong>đỏ</strong>.</li>
    <li>Quay lại bước 1.</li>

</ol>

<p>Hãy chọn <code>k</code> nhỏ nhất có thể và tô đỏ <code>k</code> tam giác trước khi chạy thuật toán, sao cho sau khi thuật toán dừng, tất cả các tam giác đơn vị đều có màu đỏ.</p>

<p>Trả về <em>một danh sách 2D chứa tọa độ của các tam giác mà bạn sẽ tô đỏ ban đầu</em>. Đáp án phải có kích thước nhỏ nhất có thể. Nếu có nhiều đáp án hợp lệ, trả về bất kỳ đáp án nào.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2600-2699/2647.Color%20the%20Triangle%20Red/images/example1.jpg" style="width: 500px; height: 263px;" />
<pre>
<strong>Đầu vào:</strong> n = 3
<strong>Đầu ra:</strong> [[1,1],[2,1],[2,3],[3,1],[3,5]]
<strong>Giải thích:</strong> Ban đầu, ta chọn 5 tam giác như hình để tô đỏ. Sau đó, ta chạy thuật toán:
- Chọn (2,2), tam giác có ba hàng xóm màu đỏ, và tô nó thành đỏ.
- Chọn (3,2), tam giác có hai hàng xóm màu đỏ, và tô nó thành đỏ.
- Chọn (3,4), tam giác có ba hàng xóm màu đỏ, và tô nó thành đỏ.
- Chọn (3,3), tam giác có ba hàng xóm màu đỏ, và tô nó thành đỏ.
Có thể chứng minh rằng chọn bất kỳ 4 tam giác nào rồi chạy thuật toán cũng không thể làm tất cả các tam giác chuyển thành màu đỏ.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2600-2699/2647.Color%20the%20Triangle%20Red/images/example2.jpg" style="width: 300px; height: 101px;" />
<pre>
<strong>Đầu vào:</strong> n = 2
<strong>Đầu ra:</strong> [[1,1],[2,1],[2,3]]
<strong>Giải thích:</strong> Ban đầu, ta chọn 3 tam giác như hình để tô đỏ. Sau đó, ta chạy thuật toán:
- Chọn (2,2), tam giác có ba hàng xóm màu đỏ, và tô nó thành đỏ.
Có thể chứng minh rằng chọn bất kỳ 2 tam giác nào rồi chạy thuật toán cũng không thể làm tất cả các tam giác chuyển thành màu đỏ.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm quy luật

<!-- thinking:start -->

> **Tư duy**
>
> Ta phải tô ít ô nhất có thể để mọi tam giác trắng đều có hai cạnh màu đỏ. Với $n \le 1000$, tam giác có $O(n^2)$ ô, nên không thể tìm kiếm trực tiếp.
>
> Quan sát hình vẽ cho thấy ô ở trên cùng luôn có màu đỏ, và cứ mỗi bốn hàng tính từ dưới lên lại lặp lại một quy luật thưa. Ta xuất các tọa độ theo chu kỳ đó, từ hàng $n$ xuống hàng $2$.

<!-- thinking:end -->

Ta vẽ hình để quan sát, từ đó nhận thấy hàng đầu tiên chỉ có một tam giác và tam giác đó bắt buộc phải được tô màu, còn từ hàng cuối cùng đến hàng thứ hai, quy luật tô màu của mỗi bốn hàng là giống nhau:

1. Hàng cuối cùng được tô tại $(n, 1)$, $(n, 3)$, ..., $(n, 2n - 1)$.
1. Hàng $n - 1$ được tô tại $(n - 1, 2)$.
1. Hàng $n - 2$ được tô tại $(n - 2, 3)$, $(n - 2, 5)$, ..., $(n - 2, 2n - 5)$.
1. Hàng $n - 3$ được tô tại $(n - 3, 1)$.

<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2600-2699/2647.Color%20the%20Triangle%20Red/images/demo3.png" style="width: 50%">

Vì vậy, ta có thể tô hàng đầu tiên theo các quy tắc trên, sau đó bắt đầu từ hàng cuối cùng và tô mỗi lần bốn hàng cho đến khi kết thúc ở hàng thứ hai.

<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2600-2699/2647.Color%20the%20Triangle%20Red/images/demo2.png" style="width: 80%">

Độ phức tạp thời gian là $(n^2)$, trong đó $n$ là tham số được cho trong đề bài. Không tính phần bộ nhớ của mảng đáp án, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def colorRed(self, n: int) -> List[List[int]]:
        ans = [[1, 1]]
        k = 0
        for i in range(n, 1, -1):
            if k == 0:
                for j in range(1, i << 1, 2):
                    ans.append([i, j])
            elif k == 1:
                ans.append([i, 2])
            elif k == 2:
                for j in range(3, i << 1, 2):
                    ans.append([i, j])
            else:
                ans.append([i, 1])
            k = (k + 1) % 4
        return ans
```

#### Java

```java
class Solution {
    public int[][] colorRed(int n) {
        List<int[]> ans = new ArrayList<>();
        ans.add(new int[] {1, 1});
        for (int i = n, k = 0; i > 1; --i, k = (k + 1) % 4) {
            if (k == 0) {
                for (int j = 1; j < i << 1; j += 2) {
                    ans.add(new int[] {i, j});
                }
            } else if (k == 1) {
                ans.add(new int[] {i, 2});
            } else if (k == 2) {
                for (int j = 3; j < i << 1; j += 2) {
                    ans.add(new int[] {i, j});
                }
            } else {
                ans.add(new int[] {i, 1});
            }
        }
        return ans.toArray(new int[0][]);
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> colorRed(int n) {
        vector<vector<int>> ans;
        ans.push_back({1, 1});
        for (int i = n, k = 0; i > 1; --i, k = (k + 1) % 4) {
            if (k == 0) {
                for (int j = 1; j < i << 1; j += 2) {
                    ans.push_back({i, j});
                }
            } else if (k == 1) {
                ans.push_back({i, 2});
            } else if (k == 2) {
                for (int j = 3; j < i << 1; j += 2) {
                    ans.push_back({i, j});
                }
            } else {
                ans.push_back({i, 1});
            }
        }
        return ans;
    }
};
```

#### Go

```go
func colorRed(n int) (ans [][]int) {
	ans = append(ans, []int{1, 1})
	for i, k := n, 0; i > 1; i, k = i-1, (k+1)%4 {
		if k == 0 {
			for j := 1; j < i<<1; j += 2 {
				ans = append(ans, []int{i, j})
			}
		} else if k == 1 {
			ans = append(ans, []int{i, 2})
		} else if k == 2 {
			for j := 3; j < i<<1; j += 2 {
				ans = append(ans, []int{i, j})
			}
		} else {
			ans = append(ans, []int{i, 1})
		}
	}
	return
}
```

#### TypeScript

```ts
function colorRed(n: number): number[][] {
    const ans: number[][] = [[1, 1]];
    for (let i = n, k = 0; i > 1; --i, k = (k + 1) % 4) {
        if (k === 0) {
            for (let j = 1; j < i << 1; j += 2) {
                ans.push([i, j]);
            }
        } else if (k === 1) {
            ans.push([i, 2]);
        } else if (k === 2) {
            for (let j = 3; j < i << 1; j += 2) {
                ans.push([i, j]);
            }
        } else {
            ans.push([i, 1]);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
