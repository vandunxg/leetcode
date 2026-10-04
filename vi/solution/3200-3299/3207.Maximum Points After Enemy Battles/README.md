---
comments: true
difficulty: Medium
rating: 1591
source: Biweekly Contest 134 Q2
tags:
    - Greedy
    - Array
---

<!-- problem:start -->

# [3207. Maximum Points After Enemy Battles](https://leetcode.com/problems/maximum-points-after-enemy-battles)

[中文文档](/solution/3200-3299/3207.Maximum%20Points%20After%20Enemy%20Battles/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>enemyEnergies</code> biểu thị giá trị năng lượng của các kẻ địch khác nhau.</p>

<p>Bạn cũng được cho một số nguyên <code>currentEnergy</code> biểu thị lượng năng lượng ban đầu của bạn.</p>

<p>Bạn bắt đầu với 0 điểm và ban đầu tất cả kẻ địch đều chưa được đánh dấu.</p>

<p>Bạn có thể thực hiện <strong>một trong hai</strong> thao tác sau <strong>không </strong>hoặc nhiều lần để kiếm điểm:</p>

<ul>
    <li>Chọn một kẻ địch <strong>chưa được đánh dấu</strong> <code>i</code> sao cho <code>currentEnergy &gt;= enemyEnergies[i]</code>. Khi chọn thao tác này:

    <ul>
         <li>Bạn nhận được 1 điểm.</li>
         <li>Năng lượng của bạn giảm đi bằng năng lượng của kẻ địch, cụ thể là <code>currentEnergy = currentEnergy - enemyEnergies[i]</code>.</li>
    </ul>
    </li>
    <li>Nếu có <strong>ít nhất</strong> 1 điểm, bạn có thể chọn một kẻ địch <strong>chưa được đánh dấu</strong> <code>i</code>. Khi chọn thao tác này:
    <ul>
         <li>Năng lượng của bạn tăng lên bằng năng lượng của kẻ địch, cụ thể là <code>currentEnergy = currentEnergy + enemyEnergies[i]</code>.</li>
         <li><font face="monospace">e</font>nemy <code>i</code> được <strong>đánh dấu</strong>.</li>
    </ul>
    </li>

</ul>

<p>Trả về một số nguyên biểu thị số điểm <strong>tối đa</strong> bạn có thể nhận được sau cùng khi thực hiện các thao tác một cách tối ưu.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">enemyEnergies = [3,2,2], currentEnergy = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có thể thực hiện các thao tác sau để nhận được 3 điểm, đây là số điểm tối đa:</p>

<ul>
    <li>Thực hiện thao tác thứ nhất với kẻ địch 1: <code>points</code> tăng thêm 1 và <code>currentEnergy</code> giảm đi 2. Khi đó, <code>points = 1</code> và <code>currentEnergy = 0</code>.</li>
    <li>Thực hiện thao tác thứ hai với kẻ địch 0: <code>currentEnergy</code> tăng thêm 3 và kẻ địch 0 được đánh dấu. Khi đó, <code>points = 1</code>, <code>currentEnergy = 3</code> và các kẻ địch được đánh dấu = <code>[0]</code>.</li>
    <li>Thực hiện thao tác thứ nhất với kẻ địch 2: <code>points</code> tăng thêm 1 và <code>currentEnergy</code> giảm đi 2. Khi đó, <code>points = 2</code>, <code>currentEnergy = 1</code> và các kẻ địch được đánh dấu = <code>[0]</code>.</li>
    <li>Thực hiện thao tác thứ hai với kẻ địch 2: <code>currentEnergy</code> tăng thêm 2 và kẻ địch 2 được đánh dấu. Khi đó, <code>points = 2</code>, <code>currentEnergy = 3</code> và các kẻ địch được đánh dấu = <code>[0, 2]</code>.</li>
    <li>Thực hiện thao tác thứ nhất với kẻ địch 1: <code>points</code> tăng thêm 1 và <code>currentEnergy</code> giảm đi 2. Khi đó, <code>points = 3</code>, <code>currentEnergy = 1</code> và các kẻ địch được đánh dấu = <code>[0, 2]</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">enemyEnergies = </span>[2]<span class="example-io">, currentEnergy = 10</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích: </strong></p>

<p>Thực hiện thao tác thứ nhất 5 lần với kẻ địch 0 sẽ cho số điểm tối đa.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= enemyEnergies.length &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= enemyEnergies[i] &lt;= 10<sup>9</sup></code></li>
    <li><code>0 &lt;= currentEnergy &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác hoặc ghi được $\lfloor\textit{energy}/e\rfloor$ điểm trên một kẻ địch chưa được đánh dấu, hoặc tiêu tốn một điểm để hấp thụ năng lượng của kẻ địch đó. Với $n\le 10^5$ và năng lượng lên đến $10^9$, việc mô phỏng từng thao tác một sẽ cần quá nhiều lượt.
>
> Việc ghi điểm luôn nên nhắm vào kẻ địch có năng lượng thấp nhất, còn năng lượng nên được lấy từ kẻ địch có năng lượng lớn nhất. Trước tiên hãy sắp xếp; nếu năng lượng hiện tại đã nhỏ hơn giá trị nhỏ nhất thì đáp án là $0$. Nếu không, hãy hấp thụ các kẻ địch từ lớn đến nhỏ, ghi được nhiều điểm nhất có thể với kẻ địch có năng lượng nhỏ nhất trước mỗi lần hấp thụ. Chỉ cần duyệt một lần sau khi sắp xếp.

<!-- thinking:end -->

Theo mô tả bài toán, chúng ta cần ghi điểm bằng cách đánh bại các kẻ địch có giá trị năng lượng nhỏ nhất, đồng thời tăng năng lượng bằng cách đánh bại các kẻ địch có giá trị năng lượng lớn nhất và đánh dấu chúng.

Vì vậy, ta có thể sắp xếp các kẻ địch theo giá trị năng lượng, sau đó bắt đầu từ kẻ địch có năng lượng lớn nhất, luôn chọn kẻ địch có năng lượng nhỏ nhất để ghi điểm và tiêu hao năng lượng. Tiếp theo, ta cộng giá trị năng lượng của kẻ địch có năng lượng lớn nhất vào năng lượng hiện tại và đánh dấu kẻ địch đó. Lặp lại các bước trên cho đến khi tất cả kẻ địch đều được đánh dấu.

Độ phức tạp thời gian là $O(n \log n)$ và độ phức tạp không gian là $O(\log n)$, trong đó $n$ là số lượng kẻ địch.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumPoints(self, enemyEnergies: List[int], currentEnergy: int) -> int:
        enemyEnergies.sort()
        if currentEnergy < enemyEnergies[0]:
            return 0
        ans = 0
        for i in range(len(enemyEnergies) - 1, -1, -1):
            ans += currentEnergy // enemyEnergies[0]
            currentEnergy %= enemyEnergies[0]
            currentEnergy += enemyEnergies[i]
        return ans
```

#### Java

```java
class Solution {
    public long maximumPoints(int[] enemyEnergies, int currentEnergy) {
        Arrays.sort(enemyEnergies);
        if (currentEnergy < enemyEnergies[0]) {
            return 0;
        }
        long ans = 0;
        for (int i = enemyEnergies.length - 1; i >= 0; --i) {
            ans += currentEnergy / enemyEnergies[0];
            currentEnergy %= enemyEnergies[0];
            currentEnergy += enemyEnergies[i];
        }
        return ans;
    }
};
```

#### C++

```cpp
class Solution {
public:
    long long maximumPoints(vector<int>& enemyEnergies, int currentEnergy) {
        sort(enemyEnergies.begin(), enemyEnergies.end());
        if (currentEnergy < enemyEnergies[0]) {
            return 0;
        }
        long long ans = 0;
        for (int i = enemyEnergies.size() - 1; i >= 0; --i) {
            ans += currentEnergy / enemyEnergies[0];
            currentEnergy %= enemyEnergies[0];
            currentEnergy += enemyEnergies[i];
        }
        return ans;
    }
};
```

#### Go

```go
func maximumPoints(enemyEnergies []int, currentEnergy int) (ans int64) {
    sort.Ints(enemyEnergies)
    if currentEnergy < enemyEnergies[0] {
        return 0
    }
    for i := len(enemyEnergies) - 1; i >= 0; i-- {
        ans += int64(currentEnergy / enemyEnergies[0])
        currentEnergy %= enemyEnergies[0]
        currentEnergy += enemyEnergies[i]
    }
    return
}
```

#### TypeScript

```ts
function maximumPoints(enemyEnergies: number[], currentEnergy: number): number {
    enemyEnergies.sort((a, b) => a - b);
    if (currentEnergy < enemyEnergies[0]) {
        return 0;
    }
    let ans = 0;
    for (let i = enemyEnergies.length - 1; ~i; --i) {
        ans += Math.floor(currentEnergy / enemyEnergies[0]);
        currentEnergy %= enemyEnergies[0];
        currentEnergy += enemyEnergies[i];
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
