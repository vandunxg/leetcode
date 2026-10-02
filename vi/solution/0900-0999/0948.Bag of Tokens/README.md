---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - Two Pointers
    - Sorting
---

<!-- problem:start -->

# [948. Bag of Tokens](https://leetcode.com/problems/bag-of-tokens)

[中文文档](/solution/0900-0999/0948.Bag%20of%20Tokens/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn bắt đầu với <strong>năng lượng</strong> ban đầu là <code>power</code>, <strong>điểm</strong> ban đầu là <code>0</code>, và một túi token được biểu diễn bằng mảng số nguyên <code>tokens</code>, trong đó mỗi <code>tokens[i]</code> là giá trị của token<em><sub>i</sub></em>.</p>

<p>Mục tiêu của bạn là <strong>tối đa hóa</strong> tổng <strong>điểm</strong> bằng cách sử dụng các token một cách hợp lý. Trong mỗi lượt, bạn có thể sử dụng một token <strong>chưa được chơi</strong> theo một trong hai cách sau (không thể dùng cả hai cách cho cùng một token):</p>

<ul>
	<li><strong>Ngửa</strong>: Nếu năng lượng hiện tại ít nhất bằng <code>tokens[i]</code>, bạn có thể chơi token<em><sub>i</sub></em>, mất <code>tokens[i]</code> năng lượng và nhận <code>1</code> điểm.</li>
	<li><strong>Sấp</strong>: Nếu điểm hiện tại ít nhất là <code>1</code>, bạn có thể chơi token<em><sub>i</sub></em>, nhận <code>tokens[i]</code> năng lượng và mất <code>1</code> điểm.</li>
</ul>

<p>Hãy trả về <em>điểm số <strong>cao nhất</strong> có thể đạt được sau khi chơi <strong>bất kỳ số lượng</strong> token nào</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block" style="
    border-color: var(--border-tertiary);
    border-left-width: 2px;
    color: var(--text-secondary);
    font-size: .875rem;
    margin-bottom: 1rem;
    margin-top: 1rem;
    overflow: visible;
    padding-left: 1rem;
">
<p><strong>Input:</strong> <span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
">tokens = [100], power = 50</span></p>

<p><strong>Output:</strong> <span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
">0</span></p>

<p><strong>Giải thích</strong><strong>:</strong> Vì điểm ban đầu là <code>0</code>, bạn không thể chơi token theo cách sấp. Bạn cũng không thể chơi theo cách ngửa vì năng lượng (<code>50</code>) thấp hơn <code>tokens[0]</code>&nbsp;(<code>100</code>).</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block" style="
    border-color: var(--border-tertiary);
    border-left-width: 2px;
    color: var(--text-secondary);
    font-size: .875rem;
    margin-bottom: 1rem;
    margin-top: 1rem;
    overflow: visible;
    padding-left: 1rem;
">
<p><strong>Input:</strong> <span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
">tokens = [200,100], power = 150</span></p>

<p><strong>Output:</strong> <span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
">1</span></p>

<p><strong>Giải thích:</strong> Chơi token<em><sub>1</sub></em> (<code>100</code>) theo cách ngửa, giảm năng lượng còn&nbsp;<code>50</code> và tăng điểm lên&nbsp;<code>1</code>.</p>

<p>Không cần chơi token<em><sub>0</sub></em>, vì bạn không thể chơi nó theo cách ngửa để tăng điểm. Điểm cao nhất có thể đạt được là <code>1</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block" style="
    border-color: var(--border-tertiary);
    border-left-width: 2px;
    color: var(--text-secondary);
    font-size: .875rem;
    margin-bottom: 1rem;
    margin-top: 1rem;
    overflow: visible;
    padding-left: 1rem;
">
<p><strong>Input:</strong> <span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
">tokens = [100,200,300,400], power = 200</span></p>

<p><strong>Output:</strong> <span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
">2</span></p>

<p><strong>Giải thích:</strong> Chơi các token theo thứ tự sau để đạt điểm <code>2</code>:</p>

<ol>
	<li>Chơi token<em><sub>0</sub></em> (<code>100</code>) theo cách ngửa, giảm năng lượng còn <code>100</code> và tăng điểm lên <code>1</code>.</li>
	<li>Chơi token<em><sub>3</sub></em> (<code>400</code>) theo cách sấp, tăng năng lượng lên <code>500</code> và giảm điểm còn <code>0</code>.</li>
	<li>Chơi token<em><sub>1</sub></em> (<code>200</code>) theo cách ngửa, giảm năng lượng còn <code>300</code> và tăng điểm lên <code>1</code>.</li>
	<li>Chơi token<em><sub>2</sub></em> (<code>300</code>) theo cách ngửa, giảm năng lượng còn <code>0</code> và tăng điểm lên <code>2</code>.</li>
</ol>

<p><span style="color: var(--text-secondary); font-size: 0.875rem;">Điểm cao nhất có thể đạt được là </span><code style="color: var(--text-secondary); font-size: 0.875rem;">2</code><span style="color: var(--text-secondary); font-size: 0.875rem;">.</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= tokens.length &lt;= 1000</code></li>
	<li><code>0 &lt;= tokens[i], power &lt; 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Sorting + Two Pointers

<!-- thinking:start -->

> **Tư duy**
>
> Token rẻ giúp đổi lấy điểm, token đắt giúp đổi lấy năng lượng; mục tiêu là đạt điểm cao nhất. Ta dùng ít năng lượng nhất để lấy điểm, và khi cần thì đổi một điểm để lấy nhiều năng lượng nhất. Sau khi sắp xếp, con trỏ trái dùng token rẻ nhất; nếu thiếu năng lượng nhưng vẫn còn điểm, con trỏ phải bán token đắt nhất. Ta theo dõi điểm tối đa trong quá trình này.

<!-- thinking:end -->

Có hai cách sử dụng token: tiêu hao năng lượng để lấy điểm, hoặc tiêu hao điểm để lấy năng lượng. Ta nên dùng ít năng lượng nhất có thể để kiếm được nhiều điểm nhất.

Vì vậy, ta sắp xếp token theo lượng năng lượng cần dùng, rồi sử dụng hai con trỏ: một di chuyển từ trái sang phải, con còn lại từ phải sang trái. Mỗi lượt, ta cố gắng tiêu hao năng lượng để lấy điểm nhiều nhất có thể, rồi cập nhật điểm tối đa. Nếu năng lượng hiện tại không đủ để dùng token đang xét, ta thử đổi điểm để lấy năng lượng từ token đó. Nếu không còn đủ điểm, ta dừng.

Độ phức tạp thời gian là $O(n \log n)$ và độ phức tạp không gian là $O(\log n)$. Trong đó, $n$ là số lượng token.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def bagOfTokensScore(self, tokens: List[int], power: int) -> int:
        tokens.sort()
        ans = score = 0
        i, j = 0, len(tokens) - 1
        while i <= j:
            if power >= tokens[i]:
                power -= tokens[i]
                score, i = score + 1, i + 1
                ans = max(ans, score)
            elif score:
                power += tokens[j]
                score, j = score - 1, j - 1
            else:
                break
        return ans
```

#### Java

```java
class Solution {
    public int bagOfTokensScore(int[] tokens, int power) {
        Arrays.sort(tokens);
        int ans = 0, score = 0;
        for (int i = 0, j = tokens.length - 1; i <= j;) {
            if (power >= tokens[i]) {
                power -= tokens[i++];
                ans = Math.max(ans, ++score);
            } else if (score > 0) {
                power += tokens[j--];
                --score;
            } else {
                break;
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
    int bagOfTokensScore(vector<int>& tokens, int power) {
        sort(tokens.begin(), tokens.end());
        int ans = 0, score = 0;
        for (int i = 0, j = tokens.size() - 1; i <= j;) {
            if (power >= tokens[i]) {
                power -= tokens[i++];
                ans = max(ans, ++score);
            } else if (score > 0) {
                power += tokens[j--];
                --score;
            } else {
                break;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func bagOfTokensScore(tokens []int, power int) (ans int) {
	sort.Ints(tokens)
	i, j := 0, len(tokens)-1
	score := 0
	for i <= j {
		if power >= tokens[i] {
			power -= tokens[i]
			i++
			score++
			ans = max(ans, score)
		} else if score > 0 {
			power += tokens[j]
			j--
			score--
		} else {
			break
		}
	}
	return
}
```

#### TypeScript

```ts
function bagOfTokensScore(tokens: number[], power: number): number {
    tokens.sort((a, b) => a - b);
    let [i, j] = [0, tokens.length - 1];
    let [ans, score] = [0, 0];
    while (i <= j) {
        if (power >= tokens[i]) {
            power -= tokens[i++];
            ans = Math.max(ans, ++score);
        } else if (score) {
            power += tokens[j--];
            score--;
        } else {
            break;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
