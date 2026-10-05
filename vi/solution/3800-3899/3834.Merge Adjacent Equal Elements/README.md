---
comments: true
difficulty: Medium
rating: 1428
source: Weekly Contest 488 Q2
tags:
    - Stack
    - Array
    - Simulation
---

<!-- problem:start -->

# [3834. Merge Adjacent Equal Elements](https://leetcode.com/problems/merge-adjacent-equal-elements)

[中文文档](/solution/3800-3899/3834.Merge%20Adjacent%20Equal%20Elements/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Bạn phải <strong>liên tục</strong> thực hiện phép gộp sau cho đến khi không thể thực hiện thêm thay đổi nào:</p>

<ul>
	<li>Nếu có <strong>hai phần tử kề nhau bằng nhau</strong>, hãy chọn cặp kề nhau như vậy <strong>ngoài cùng bên trái</strong> trong mảng hiện tại và thay chúng bằng một phần tử duy nhất có giá trị bằng <strong>tổng</strong> của chúng.</li>
</ul>

<p>Sau mỗi phép gộp, kích thước mảng <strong>giảm</strong> đi 1. Lặp lại quá trình trên với mảng đã cập nhật cho đến khi không thể thực hiện thêm phép gộp nào.</p>

<p>Trả về mảng cuối cùng sau khi thực hiện tất cả các phép gộp có thể.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,1,1,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[3,4]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Hai phần tử ở giữa bằng nhau và được gộp thành <code>1 + 1 = 2</code>, khi đó mảng trở thành <code>[3, 2, 2]</code>.</li>
	<li>Hai phần tử cuối bằng nhau và được gộp thành <code>2 + 2 = 4</code>, khi đó mảng trở thành <code>[3, 4]</code>.</li>
	<li>Không còn hai phần tử kề nhau bằng nhau. Vì vậy, đáp án là <code>[3, 4]</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,2,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[8]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Hai phần tử đầu bằng nhau và được gộp thành <code>2 + 2 = 4</code>, khi đó mảng trở thành <code>[4, 4]</code>.</li>
	<li>Hai phần tử đầu bằng nhau và được gộp thành <code>4 + 4 = 8</code>, khi đó mảng trở thành <code>[8]</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,7,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[3,7,5]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có hai phần tử kề nhau bằng nhau trong mảng, nên không có phép gộp nào được thực hiện.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code>​​​​​​​</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Stack

<!-- thinking:start -->

> **Tư duy**
>
> Liên tục thay cặp phần tử kề nhau bằng nhau ngoài cùng bên trái bằng tổng của chúng. Vì $n \le 10^5$, việc quét lại từ đầu sau mỗi lần gộp sẽ có độ phức tạp bậc hai.
>
> Một phép gộp chỉ ảnh hưởng đến phần tử bên trái của tổng mới; phần bên phải chưa xử lý không bị thay đổi. Một stack lưu phần tiền tố đã ổn định.
>
> Duyệt từ trái sang phải; khi hai phần tử trên cùng bằng nhau, lấy chúng ra rồi đưa tổng vào stack.
>
> Việc luôn gộp cặp ngoài cùng bên trái tương đương với quá trình từ trái sang phải này, và mỗi giá trị chỉ được thêm vào rồi lấy ra khỏi stack một số lần hằng số.

<!-- thinking:end -->

Ta có thể dùng một stack để mô phỏng quá trình gộp các phần tử kề nhau bằng nhau.

Định nghĩa một stack $\textit{stk}$ để lưu các phần tử đã được xử lý của mảng hiện tại. Duyệt từng phần tử $x$ của mảng đầu vào $\textit{nums}$ và đưa nó vào stack. Sau đó kiểm tra xem hai phần tử trên cùng của stack có bằng nhau không. Nếu bằng nhau, lấy chúng ra rồi đưa tổng của chúng trở lại stack. Lặp lại quá trình này cho đến khi hai phần tử trên cùng của stack không còn bằng nhau. Cuối cùng, các phần tử trong stack chính là mảng sau khi gộp xong.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(n)$, dùng để lưu các phần tử trong stack.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def mergeAdjacent(self, nums: List[int]) -> List[int]:
        stk = []
        for x in nums:
            stk.append(x)
            while len(stk) > 1 and stk[-1] == stk[-2]:
                stk.append(stk.pop() + stk.pop())
        return stk
```

#### Java

```java
class Solution {
    public List<Long> mergeAdjacent(int[] nums) {
        List<Long> stk = new ArrayList<>();
        for (int x : nums) {
            stk.add((long) x);
            while (stk.size() > 1 && stk.get(stk.size() - 1).equals(stk.get(stk.size() - 2))) {
                long a = stk.remove(stk.size() - 1);
                long b = stk.remove(stk.size() - 1);
                stk.add(a + b);
            }
        }
        return stk;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<long long> mergeAdjacent(vector<int>& nums) {
        vector<long long> stk;
        for (int x : nums) {
            stk.push_back(x);
            while (stk.size() > 1 && stk.back() == stk[stk.size() - 2]) {
                long long a = stk.back();
                stk.pop_back();
                long long b = stk.back();
                stk.pop_back();
                stk.push_back(a + b);
            }
        }
        return stk;
    }
};
```

#### Go

```go
func mergeAdjacent(nums []int) []int64 {
	stk := []int64{}
	for _, x := range nums {
		stk = append(stk, int64(x))
		for len(stk) > 1 && stk[len(stk)-1] == stk[len(stk)-2] {
			a := stk[len(stk)-1]
			stk = stk[:len(stk)-1]
			b := stk[len(stk)-1]
			stk = stk[:len(stk)-1]
			stk = append(stk, a+b)
		}
	}
	return stk
}
```

#### TypeScript

```ts
function mergeAdjacent(nums: number[]): number[] {
    const stk: number[] = [];
    for (const x of nums) {
        stk.push(x);
        while (stk.length > 1 && stk.at(-1)! === stk.at(-2)!) {
            const a = stk.pop()!;
            const b = stk.pop()!;
            stk.push(a + b);
        }
    }
    return stk;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
