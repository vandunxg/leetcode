---
comments: true
difficulty: Easy
rating: 1166
source: Biweekly Contest 139 Q1
tags:
    - Array
---

<!-- problem:start -->

# [3285. Find Indices of Stable Mountains](https://leetcode.com/problems/find-indices-of-stable-mountains)

[中文文档](/solution/3200-3299/3285.Find%20Indices%20of%20Stable%20Mountains/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> ngọn núi xếp thành một hàng, mỗi ngọn núi có một độ cao. Cho mảng số nguyên <code>height</code>, trong đó <code>height[i]</code> là độ cao của ngọn núi thứ <code>i</code>, và một số nguyên <code>threshold</code>.</p>

<p>Một ngọn núi được gọi là <strong>ổn định</strong> nếu ngọn núi ngay trước nó (<strong>nếu tồn tại</strong>) có độ cao <strong>lớn hơn nghiêm ngặt</strong> <code>threshold</code>. <strong>Lưu ý</strong> rằng ngọn núi 0 <strong>không ổn định</strong>.</p>

<p>Trả về một mảng chứa chỉ số của <em>tất cả</em> các ngọn núi <strong>ổn định</strong> theo <strong>bất kỳ</strong> thứ tự nào.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">height = [1,2,3,4,5], threshold = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[3,4]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Ngọn núi 3 ổn định vì <code>height[2] == 3</code> lớn hơn <code>threshold == 2</code>.</li>
	<li>Ngọn núi 4 ổn định vì <code>height[3] == 4</code> lớn hơn <code>threshold == 2</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">height = [10,1,10,1,10], threshold = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,3]</span></p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">height = [10,1,10,1,10], threshold = 10</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[]</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n == height.length &lt;= 100</code></li>
	<li><code>1 &lt;= height[i] &lt;= 100</code></li>
	<li><code>1 &lt;= threshold &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ số $i\ge 1$ ổn định khi và chỉ khi ngọn núi bên trái nó cao hơn ngưỡng. Vì $n\le 100$, ta chỉ cần lọc theo đúng định nghĩa.
>
> Thu thập mọi $i$ thỏa mãn $\textit{height}[i-1]>\textit{threshold}$. Không cần cấu trúc prefix nào.

<!-- thinking:end -->

Ta duyệt trực tiếp các ngọn núi bắt đầu từ chỉ số $1$. Nếu độ cao của ngọn núi bên trái lớn hơn $threshold$, ta thêm chỉ số của ngọn núi hiện tại vào mảng kết quả.

Sau khi duyệt xong, ta trả về mảng kết quả.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{height}$. Không tính phần bộ nhớ dùng cho mảng kết quả, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def stableMountains(self, height: List[int], threshold: int) -> List[int]:
        return [i for i in range(1, len(height)) if height[i - 1] > threshold]
```

#### Java

```java
class Solution {
    public List<Integer> stableMountains(int[] height, int threshold) {
        List<Integer> ans = new ArrayList<>();
        for (int i = 1; i < height.length; ++i) {
            if (height[i - 1] > threshold) {
                ans.add(i);
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
    vector<int> stableMountains(vector<int>& height, int threshold) {
        vector<int> ans;
        for (int i = 1; i < height.size(); ++i) {
            if (height[i - 1] > threshold) {
                ans.push_back(i);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func stableMountains(height []int, threshold int) (ans []int) {
	for i := 1; i < len(height); i++ {
		if height[i-1] > threshold {
			ans = append(ans, i)
		}
	}
	return
}
```

#### TypeScript

```ts
function stableMountains(height: number[], threshold: number): number[] {
    const ans: number[] = [];
    for (let i = 1; i < height.length; ++i) {
        if (height[i - 1] > threshold) {
            ans.push(i);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
