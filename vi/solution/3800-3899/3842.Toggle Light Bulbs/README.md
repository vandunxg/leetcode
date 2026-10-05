---
comments: true
difficulty: Easy
rating: 1160
source: Weekly Contest 489 Q1
tags:
    - Array
    - Hash Table
    - Sorting
    - Simulation
---

<!-- problem:start -->

# [3842. Toggle Light Bulbs](https://leetcode.com/problems/toggle-light-bulbs)

[中文文档](/solution/3800-3899/3842.Toggle%20Light%20Bulbs/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>bulbs</code> gồm các số từ 1 đến 100.</p>

<p>Có 100 bóng đèn được đánh số từ 1 đến 100. Ban đầu, tất cả bóng đèn đều tắt.</p>

<p>Với mỗi phần tử <code>bulbs[i]</code> trong mảng <code>bulbs</code>:</p>

<ul>
	<li>Nếu bóng đèn <code>bulbs[i]<sup>th</sup></code> hiện đang tắt, hãy bật nó lên.</li>
	<li>Nếu không, hãy tắt nó đi.</li>
</ul>

<p>Hãy trả về danh sách các số nguyên biểu thị những bóng đèn đang bật sau cùng, <strong>được sắp xếp</strong> theo thứ tự <strong>tăng dần</strong>. Nếu không có bóng đèn nào bật, hãy trả về một danh sách rỗng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> bulbs<span class="example-io"> = [10,30,20,10]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[20,30]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Bóng đèn <code>bulbs[0] = 10<sup>th</sup></code> hiện đang tắt. Ta bật nó lên.</li>
	<li>Bóng đèn <code>bulbs[1] = 30<sup>th</sup></code> hiện đang tắt. Ta bật nó lên.</li>
	<li>Bóng đèn <code>bulbs[2] = 20<sup>th</sup></code> hiện đang tắt. Ta bật nó lên.</li>
	<li>Bóng đèn <code>bulbs[3] = 10<sup>th</sup></code> hiện đang bật. Ta tắt nó đi.</li>
	<li>Cuối cùng, bóng đèn thứ 20<sup>th</sup> và thứ 30<sup>th</sup> đang bật.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> bulbs<span class="example-io"> = [100,100]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Bóng đèn <code>bulbs[0] = 100<sup>th</sup></code> hiện đang tắt. Ta bật nó lên.</li>
	<li>Bóng đèn <code>bulbs[1] = 100<sup>th</sup></code> hiện đang bật. Ta tắt nó đi.</li>
	<li>Cuối cùng, không có bóng đèn nào bật.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= bulbs.length &lt;= 100</code></li>
	<li><code>1 &lt;= bulbs[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Các bóng đèn được đánh số từ $1$ đến $100$; mỗi lần xuất hiện một số sẽ toggle bóng đèn tương ứng. Độ dài mảng nhiều nhất là $100$, nên ta có thể mô phỏng.
>
> Trạng thái cuối chỉ phụ thuộc vào tính chẵn lẻ của số lần xuất hiện của mỗi số.
>
> Ta XOR $1$ vào một mảng có độ dài $101$, rồi thu thập các chỉ số vẫn bằng $1$.
>
> Các chỉ số đó đã ở thứ tự tăng dần.

<!-- thinking:end -->

Ta sử dụng một mảng $\textit{st}$ có độ dài $101$ để ghi lại trạng thái của mỗi bóng đèn. Ban đầu, tất cả phần tử đều bằng $0$, biểu thị tất cả bóng đèn đều đang tắt. Với mỗi phần tử $\textit{bulbs}[i]$ trong mảng $\textit{bulbs}$, ta toggle giá trị của $\textit{st}[\textit{bulbs}[i]]$ (tức là $0$ trở thành $1$ và $1$ trở thành $0$). Cuối cùng, ta duyệt mảng $\textit{st}$, thêm các chỉ số có giá trị bằng $1$ vào danh sách kết quả, rồi trả về kết quả.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{bulbs}$. Độ phức tạp không gian là $O(M)$, trong đó $M$ là số hiệu bóng đèn lớn nhất.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def toggleLightBulbs(self, bulbs: list[int]) -> list[int]:
        st = [0] * 101
        for x in bulbs:
            st[x] ^= 1
        return [i for i, x in enumerate(st) if x]
```

#### Java

```java
class Solution {
    public List<Integer> toggleLightBulbs(List<Integer> bulbs) {
        int[] st = new int[101];
        for (int x : bulbs) {
            st[x] ^= 1;
        }
        List<Integer> ans = new ArrayList<>();
        for (int i = 0; i < st.length; ++i) {
            if (st[i] == 1) {
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
    vector<int> toggleLightBulbs(vector<int>& bulbs) {
        vector<int> st(101, 0);
        for (int x : bulbs) {
            st[x] ^= 1;
        }
        vector<int> ans;
        for (int i = 0; i < 101; ++i) {
            if (st[i]) {
                ans.push_back(i);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func toggleLightBulbs(bulbs []int) []int {
	st := make([]int, 101)
	for _, x := range bulbs {
		st[x] ^= 1
	}
	ans := make([]int, 0)
	for i := 0; i < 101; i++ {
		if st[i] == 1 {
			ans = append(ans, i)
		}
	}
	return ans
}
```

#### TypeScript

```ts
function toggleLightBulbs(bulbs: number[]): number[] {
    const st: number[] = new Array(101).fill(0);
    for (const x of bulbs) {
        st[x] ^= 1;
    }
    const ans: number[] = [];
    for (let i = 0; i < 101; i++) {
        if (st[i] === 1) {
            ans.push(i);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
