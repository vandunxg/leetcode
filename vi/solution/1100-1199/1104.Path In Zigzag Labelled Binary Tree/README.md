---
comments: true
difficulty: Medium
rating: 1544
source: Weekly Contest 143 Q2
tags:
    - Tree
    - Math
    - Binary Tree
---

<!-- problem:start -->

# [1104. Path In Zigzag Labelled Binary Tree](https://leetcode.com/problems/path-in-zigzag-labelled-binary-tree)

[中文文档](/solution/1100-1199/1104.Path%20In%20Zigzag%20Labelled%20Binary%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Trong một cây nhị phân vô hạn, mỗi node có hai node con và các node được gán nhãn theo từng hàng.</p>

<p>Ở các hàng có số thứ tự lẻ (tức hàng thứ nhất, thứ ba, thứ năm, ...), nhãn được gán từ trái sang phải; còn ở các hàng có số thứ tự chẵn (hàng thứ hai, thứ tư, thứ sáu, ...), nhãn được gán từ phải sang trái.</p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1100-1199/1104.Path%20In%20Zigzag%20Labelled%20Binary%20Tree/images/tree.png" style="width: 300px; height: 138px;" /></p>

<p>Cho <code>label</code> của một node trong cây, hãy trả về các nhãn trên đường đi từ root đến node có nhãn <code>label</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> label = 14
<strong>Output:</strong> [1,3,4,14]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> label = 26
<strong>Output:</strong> [1,2,6,10,26]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= label &lt;= 10^6</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Trong cây nhị phân đầy đủ thông thường, node cha có nhãn $\lfloor \textit{label}/2\rfloor$, nhưng ở đây các hàng lẻ và chẵn được gán nhãn theo hai hướng ngược nhau nên công thức đó không còn đúng. Hàng $i$ chứa các nhãn trong đoạn $[2^{i-1},2^i-1]$; với một hàng được gán nhãn theo chiều ngược, nhãn đối xứng là $2^{i-1}+2^i-1-\textit{label}$, và nhãn của node cha thực sự bằng nhãn đối xứng đó chia cho $2$.
>
> Xác định hàng chứa $\textit{label}$, rồi đi ngược lên bằng cách tìm nhãn đối xứng và dịch phải, đồng thời ghi mỗi nhãn vào vị trí tương ứng với hàng để thu được đường đi theo thứ tự từ root.
<!-- thinking:end -->

Trong cây nhị phân đầy đủ, số node ở hàng thứ $i$ là $2^{i-1}$, và các nhãn node ở hàng thứ $i$ nằm trong đoạn $[2^{i-1}, 2^i - 1]$. Trong bài này, node ở hàng lẻ được gán nhãn từ trái sang phải, còn node ở hàng chẵn được gán nhãn từ phải sang trái. Vì vậy, với node có nhãn $label$ ở hàng thứ $i$, nhãn của node đối xứng với nó là $2^{i-1} + 2^i - 1 - label$. Do đó, nhãn node cha thực sự của node có nhãn $label$ là $(2^{i-1} + 2^i - 1 - label) / 2$. Ta có thể tìm đường đi từ root đến node có nhãn $label$ bằng cách liên tục tìm nhãn đối xứng rồi tìm nhãn node cha cho đến khi đến root.

Cuối cùng, cần đảo ngược đường đi vì đề bài yêu cầu thứ tự từ root đến node có nhãn $label$.

Độ phức tạp thời gian là $O(\log n)$, với $n$ là nhãn của node. Không tính phần bộ nhớ dành cho đáp án, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def pathInZigZagTree(self, label: int) -> List[int]:
        x = i = 1
        while (x << 1) <= label:
            x <<= 1
            i += 1
        ans = [0] * i
        while i:
            ans[i - 1] = label
            label = ((1 << (i - 1)) + (1 << i) - 1 - label) >> 1
            i -= 1
        return ans
```

#### Java

```java
class Solution {
    public List<Integer> pathInZigZagTree(int label) {
        int x = 1, i = 1;
        while ((x << 1) <= label) {
            x <<= 1;
            ++i;
        }
        List<Integer> ans = new ArrayList<>();
        for (; i > 0; --i) {
            ans.add(label);
            label = ((1 << (i - 1)) + (1 << i) - 1 - label) >> 1;
        }
        Collections.reverse(ans);
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> pathInZigZagTree(int label) {
        int x = 1, i = 1;
        while ((x << 1) <= label) {
            x <<= 1;
            ++i;
        }
        vector<int> ans;
        for (; i > 0; --i) {
            ans.push_back(label);
            label = ((1 << (i - 1)) + (1 << i) - 1 - label) >> 1;
        }
        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```

#### Go

```go
func pathInZigZagTree(label int) (ans []int) {
	x, i := 1, 1
	for x<<1 <= label {
		x <<= 1
		i++
	}
	for ; i > 0; i-- {
		ans = append(ans, label)
		label = ((1 << (i - 1)) + (1 << i) - 1 - label) >> 1
	}
	for i, j := 0, len(ans)-1; i < j; i, j = i+1, j-1 {
		ans[i], ans[j] = ans[j], ans[i]
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
