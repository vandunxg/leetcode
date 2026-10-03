---
comments: true
difficulty: Medium
rating: 1390
source: Weekly Contest 278 Q2
tags:
    - Array
---

<!-- problem:start -->

# [2155. All Divisions With the Highest Score of a Binary Array](https://leetcode.com/problems/all-divisions-with-the-highest-score-of-a-binary-array)

[中文文档](/solution/2100-2199/2155.All%20Divisions%20With%20the%20Highest%20Score%20of%20a%20Binary%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng nhị phân <strong>được đánh chỉ số từ 0</strong> <code>nums</code> có độ dài <code>n</code>. Có thể chia <code>nums</code> tại chỉ số <code>i</code> (với <code>0 &lt;= i &lt;= n)</code> thành hai mảng (có thể rỗng) <code>nums<sub>left</sub></code> và <code>nums<sub>right</sub></code>:</p>

<ul>
	<li><code>nums<sub>left</sub></code> chứa tất cả phần tử của <code>nums</code> từ chỉ số <code>0</code> đến <code>i - 1</code> <strong>(bao gồm cả hai đầu mút)</strong>, còn <code>nums<sub>right</sub></code> chứa tất cả phần tử của nums từ chỉ số <code>i</code> đến <code>n - 1</code> <strong>(bao gồm cả hai đầu mút)</strong>.</li>
	<li>Nếu <code>i == 0</code>, <code>nums<sub>left</sub></code> là <strong>rỗng</strong>, còn <code>nums<sub>right</sub></code> chứa tất cả phần tử của <code>nums</code>.</li>
	<li>Nếu <code>i == n</code>, <code>nums<sub>left</sub></code> chứa tất cả phần tử của nums, còn <code>nums<sub>right</sub></code> là <strong>rỗng</strong>.</li>
</ul>

<p><strong>Điểm số của phép chia</strong> tại chỉ số <code>i</code> là <strong>tổng</strong> của số lượng số <code>0</code> trong <code>nums<sub>left</sub></code> và số lượng số <code>1</code> trong <code>nums<sub>right</sub></code>.</p>

<p>Hãy trả về <em><strong>tất cả các chỉ số phân biệt</strong> có <strong>điểm số phép chia</strong> <strong>lớn nhất</strong></em>. Bạn có thể trả về đáp án theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,0,1,0]
<strong>Đầu ra:</strong> [2,4]
<strong>Giải thích:</strong> Chia tại chỉ số
- 0: nums<sub>left</sub> là []. nums<sub>right</sub> là [0,0,<u><strong>1</strong></u>,0]. Điểm số là 0 + 1 = 1.
- 1: nums<sub>left</sub> là [<u><strong>0</strong></u>]. nums<sub>right</sub> là [0,<u><strong>1</strong></u>,0]. Điểm số là 1 + 1 = 2.
- 2: nums<sub>left</sub> là [<u><strong>0</strong></u>,<u><strong>0</strong></u>]. nums<sub>right</sub> là [<u><strong>1</strong></u>,0]. Điểm số là 2 + 1 = 3.
- 3: nums<sub>left</sub> là [<u><strong>0</strong></u>,<u><strong>0</strong></u>,1]. nums<sub>right</sub> là [0]. Điểm số là 2 + 0 = 2.
- 4: nums<sub>left</sub> là [<u><strong>0</strong></u>,<u><strong>0</strong></u>,1,<u><strong>0</strong></u>]. nums<sub>right</sub> là []. Điểm số là 3 + 0 = 3.
Hai chỉ số 2 và 4 đều có điểm số phép chia lớn nhất là 3.
Lưu ý rằng đáp án [4,2] cũng được chấp nhận.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,0,0]
<strong>Đầu ra:</strong> [3]
<strong>Giải thích:</strong> Chia tại chỉ số
- 0: nums<sub>left</sub> là []. nums<sub>right</sub> là [0,0,0]. Điểm số là 0 + 0 = 0.
- 1: nums<sub>left</sub> là [<u><strong>0</strong></u>]. nums<sub>right</sub> là [0,0]. Điểm số là 1 + 0 = 1.
- 2: nums<sub>left</sub> là [<u><strong>0</strong></u>,<u><strong>0</strong></u>]. nums<sub>right</sub> là [0]. Điểm số là 2 + 0 = 2.
- 3: nums<sub>left</sub> là [<u><strong>0</strong></u>,<u><strong>0</strong></u>,<u><strong>0</strong></u>]. nums<sub>right</sub> là []. Điểm số là 3 + 0 = 3.
Chỉ chỉ số 3 có điểm số phép chia lớn nhất là 3.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1]
<strong>Đầu ra:</strong> [0]
<strong>Giải thích:</strong> Chia tại chỉ số
- 0: nums<sub>left</sub> là []. nums<sub>right</sub> là [<u><strong>1</strong></u>,<u><strong>1</strong></u>]. Điểm số là 0 + 2 = 2.
- 1: nums<sub>left</sub> là [1]. nums<sub>right</sub> là [<u><strong>1</strong></u>]. Điểm số là 0 + 1 = 1.
- 2: nums<sub>left</sub> là [1,1]. nums<sub>right</sub> là []. Điểm số là 0 + 0 = 0.
Chỉ chỉ số 0 có điểm số phép chia lớn nhất là 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>nums[i]</code> chỉ có thể là <code>0</code> hoặc <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tổng tiền tố

<!-- thinking:start -->

> **Tư duy**
>
> Điểm số tại $i$ là số lượng số 0 ở bên trái cộng với số lượng số 1 ở bên phải. Nếu duyệt lại cả hai phía cho mỗi $i$, độ phức tạp sẽ là $O(n^2)$.
>
> Khi di chuyển điểm chia một bước, hai số lượng này được cập nhật dựa trên bit hiện tại. Ta duy trì lần lượt $\textit{l0}$ và $\textit{r1}$, đồng thời lưu điểm số tốt nhất cùng các chỉ số tương ứng.
>
> Cộng $x\oplus 1$ vào $\textit{l0}$, trừ $x$ khỏi $\textit{r1}$, rồi cập nhật danh sách khi $t$ bằng hoặc vượt $\textit{mx}$.

<!-- thinking:end -->

Ta bắt đầu từ $i = 0$, sử dụng hai biến $\textit{l0}$ và $\textit{r1}$ để lần lượt ghi nhận số lượng số 0 ở bên trái và số lượng $1$s ở bên phải của $i$. Ban đầu, $\textit{l0} = 0$, còn $\textit{r1} = \sum \textit{nums}$.

Ta duyệt qua mảng $\textit{nums}$. Với mỗi $i$, ta cập nhật $\textit{l0}$ và $\textit{r1}$, tính điểm số hiện tại $t = \textit{l0} + \textit{r1}$. Nếu $t$ bằng điểm số lớn nhất hiện tại $\textit{mx}$, ta thêm $i$ vào mảng đáp án. Nếu $t$ lớn hơn $\textit{mx}$, ta cập nhật $\textit{mx}$ thành $t$, xóa mảng đáp án, rồi thêm $i$ vào mảng đáp án.

Sau khi duyệt xong, ta trả về mảng đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxScoreIndices(self, nums: List[int]) -> List[int]:
        l0, r1 = 0, sum(nums)
        mx = r1
        ans = [0]
        for i, x in enumerate(nums, 1):
            l0 += x ^ 1
            r1 -= x
            t = l0 + r1
            if mx == t:
                ans.append(i)
            elif mx < t:
                mx = t
                ans = [i]
        return ans
```

#### Java

```java
class Solution {
    public List<Integer> maxScoreIndices(int[] nums) {
        int l0 = 0, r1 = Arrays.stream(nums).sum();
        int mx = r1;
        List<Integer> ans = new ArrayList<>();
        ans.add(0);
        for (int i = 1; i <= nums.length; ++i) {
            int x = nums[i - 1];
            l0 += x ^ 1;
            r1 -= x;
            int t = l0 + r1;
            if (mx == t) {
                ans.add(i);
            } else if (mx < t) {
                mx = t;
                ans.clear();
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
    vector<int> maxScoreIndices(vector<int>& nums) {
        int l0 = 0, r1 = accumulate(nums.begin(), nums.end(), 0);
        int mx = r1;
        vector<int> ans = {0};
        for (int i = 1; i <= nums.size(); ++i) {
            int x = nums[i - 1];
            l0 += x ^ 1;
            r1 -= x;
            int t = l0 + r1;
            if (mx == t) {
                ans.push_back(i);
            } else if (mx < t) {
                mx = t;
                ans = {i};
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxScoreIndices(nums []int) []int {
	l0, r1 := 0, 0
	for _, x := range nums {
		r1 += x
	}
	mx := r1
	ans := []int{0}
	for i, x := range nums {
		l0 += x ^ 1
		r1 -= x
		t := l0 + r1
		if mx == t {
			ans = append(ans, i+1)
		} else if mx < t {
			mx = t
			ans = []int{i + 1}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function maxScoreIndices(nums: number[]): number[] {
    const n = nums.length;
    let [l0, r1] = [0, nums.reduce((a, b) => a + b, 0)];
    let mx = r1;
    const ans: number[] = [0];
    for (let i = 1; i <= n; ++i) {
        const x = nums[i - 1];
        l0 += x ^ 1;
        r1 -= x;
        const t = l0 + r1;
        if (mx === t) {
            ans.push(i);
        } else if (mx < t) {
            mx = t;
            ans.length = 0;
            ans.push(i);
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_score_indices(nums: Vec<i32>) -> Vec<i32> {
        let mut l0 = 0;
        let mut r1: i32 = nums.iter().sum();
        let mut mx = r1;
        let mut ans = vec![0];

        for i in 1..=nums.len() {
            let x = nums[i - 1];
            l0 += x ^ 1;
            r1 -= x;
            let t = l0 + r1;
            if mx == t {
                ans.push(i as i32);
            } else if mx < t {
                mx = t;
                ans = vec![i as i32];
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
