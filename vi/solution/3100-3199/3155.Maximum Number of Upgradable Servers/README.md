---
comments: true
difficulty: Medium
tags:
    - Array
    - Math
    - Binary Search
---

<!-- problem:start -->

# [3155. Maximum Number of Upgradable Servers 🔒](https://leetcode.com/problems/maximum-number-of-upgradable-servers)

[中文文档](/solution/3100-3199/3155.Maximum%20Number%20of%20Upgradable%20Servers/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có <code>n</code> trung tâm dữ liệu và cần nâng cấp các server của chúng.</p>

<p>Bạn được cho bốn mảng <code>count</code>, <code>upgrade</code>, <code>sell</code> và <code>money</code> có cùng độ dài <code>n</code>, lần lượt cho biết:</p>

<ul>
	<li>Số lượng server</li>
	<li>Chi phí nâng cấp một server</li>
	<li>Số tiền nhận được khi bán một server</li>
	<li>Số tiền ban đầu bạn có</li>
</ul>

<p>cho mỗi trung tâm dữ liệu.</p>

<p>Hãy trả về một mảng <code>answer</code>, trong đó với mỗi trung tâm dữ liệu, phần tử tương ứng trong <code>answer</code> biểu thị số lượng server <strong>tối đa</strong> có thể nâng cấp.</p>

<p>Lưu ý rằng số tiền từ một trung tâm dữ liệu <strong>không thể</strong> được sử dụng cho trung tâm dữ liệu khác.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">count = [4,3], upgrade = [3,5], sell = [4,2], money = [8,9]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[3,2]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Đối với trung tâm dữ liệu đầu tiên, nếu bán một server, chúng ta sẽ có <code>8 + 4 = 12</code> đơn vị tiền và có thể nâng cấp 3 server còn lại.</p>

<p>Đối với trung tâm dữ liệu thứ hai, nếu bán một server, chúng ta sẽ có <code>9 + 2 = 11</code> đơn vị tiền và có thể nâng cấp 2 server còn lại.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">count = [1], upgrade = [2], sell = [1], money = [1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0]</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= count.length == upgrade.length == sell.length == money.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= count[i], upgrade[i], sell[i], money[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Một trung tâm dữ liệu có thể bán một số server để lấy tiền nâng cấp số server còn lại. Việc thử mọi $x\le count[i]$ là khả thi nhưng không cần thiết.
>
> Số tiền từ việc bán $count-x$ server cộng với tiền mặt phải đủ để chi trả $x\cdot upgrade$. Biến đổi bất đẳng thức này, ta được $x\le(count\cdot sell+money)/(upgrade+sell)$ và $x\le count$.
>
> Với mỗi trung tâm, lấy giá trị nhỏ hơn giữa cận trên đó và số lượng server. Phép chia số nguyên sẽ tự động lấy phần nguyên.

<!-- thinking:end -->

Với mỗi trung tâm dữ liệu, giả sử chúng ta có thể nâng cấp $x$ server, khi đó $x \times \textit{upgrade[i]} \leq \textit{count[i]} \times \textit{sell[i]} + \textit{money[i]}$. Tức là $x \leq \frac{\textit{count[i]} \times \textit{sell[i]} + \textit{money[i]}}{\textit{upgrade[i]} + \textit{sell[i]}}$. Ngoài ra, $x \leq \textit{count[i]}$, vì vậy ta có thể lấy giá trị nhỏ hơn giữa hai cận này.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Không tính phần bộ nhớ được sử dụng cho mảng kết quả, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxUpgrades(
        self, count: List[int], upgrade: List[int], sell: List[int], money: List[int]
    ) -> List[int]:
        ans = []
        for cnt, cost, income, cash in zip(count, upgrade, sell, money):
            ans.append(min(cnt, (cnt * income + cash) // (cost + income)))
        return ans
```

#### Java

```java
class Solution {
    public int[] maxUpgrades(int[] count, int[] upgrade, int[] sell, int[] money) {
        int n = count.length;
        int[] ans = new int[n];
        for (int i = 0; i < n; ++i) {
            ans[i] = Math.min(
                count[i], (int) ((1L * count[i] * sell[i] + money[i]) / (upgrade[i] + sell[i])));
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> maxUpgrades(vector<int>& count, vector<int>& upgrade, vector<int>& sell, vector<int>& money) {
        int n = count.size();
        vector<int> ans;
        for (int i = 0; i < n; ++i) {
            ans.push_back(min(count[i], (int) ((1LL * count[i] * sell[i] + money[i]) / (upgrade[i] + sell[i]))));
        }
        return ans;
    }
};
```

#### Go

```go
func maxUpgrades(count []int, upgrade []int, sell []int, money []int) (ans []int) {
	for i, cnt := range count {
		ans = append(ans, min(cnt, (cnt*sell[i]+money[i])/(upgrade[i]+sell[i])))
	}
	return
}
```

#### TypeScript

```ts
function maxUpgrades(
    count: number[],
    upgrade: number[],
    sell: number[],
    money: number[],
): number[] {
    const n = count.length;
    const ans: number[] = [];
    for (let i = 0; i < n; ++i) {
        const x = ((count[i] * sell[i] + money[i]) / (upgrade[i] + sell[i])) | 0;
        ans.push(Math.min(x, count[i]));
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
