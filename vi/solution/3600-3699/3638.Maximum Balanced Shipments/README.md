---
comments: true
difficulty: Medium
rating: 1463
source: Weekly Contest 461 Q2
tags:
    - Stack
    - Greedy
    - Array
    - Dynamic Programming
    - Monotonic Stack
---

<!-- problem:start -->

# [3638. Maximum Balanced Shipments](https://leetcode.com/problems/maximum-balanced-shipments)

[中文文档](/solution/3600-3699/3638.Maximum%20Balanced%20Shipments/README.md)

## Mô tả

<!-- description:start -->

<p data-end="365" data-start="23">Bạn được cho một mảng số nguyên <code data-end="62" data-start="54">weight</code> có độ dài <code data-end="76" data-start="73">n</code>, biểu diễn trọng lượng của <code data-end="109" data-start="106">n</code> kiện hàng được sắp xếp thành một hàng thẳng. Một <strong data-end="161" data-start="149">lô hàng</strong> là một mảng con liên tiếp của các kiện hàng. Một lô hàng được xem là <strong data-end="247" data-start="235">cân bằng</strong> nếu trọng lượng của <strong data-end="284" data-start="269">kiện hàng cuối cùng</strong> <strong>nhỏ hơn nghiêm ngặt</strong> <strong data-end="329" data-start="311">trọng lượng lớn nhất</strong> trong toàn bộ lô hàng.</p>

<p data-end="528" data-start="371">Chọn một tập các <strong data-end="406" data-start="387">lô hàng không chồng lấn</strong>, liên tiếp và cân bằng sao cho <strong data-end="496" data-start="449">mỗi kiện hàng xuất hiện trong nhiều nhất một lô hàng</strong> (có thể không xếp một số kiện hàng vào lô nào).</p>

<p data-end="587" data-start="507">Trả về <strong data-end="545" data-start="518">số lượng lớn nhất</strong> lô hàng cân bằng có thể tạo thành.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">weight = [2,5,1,4,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p data-end="136" data-start="62">Ta có thể tạo được tối đa hai lô hàng cân bằng như sau:</p>

<ul>
    <li data-end="163" data-start="140">Lô hàng 1: <code>[2, 5, 1]</code>

    <ul>
         <li data-end="195" data-start="168">Trọng lượng kiện hàng lớn nhất = 5</li>
         <li data-end="275" data-start="200">Trọng lượng kiện hàng cuối cùng = 1, nhỏ hơn nghiêm ngặt 5. Vì vậy, đây là một lô hàng cân bằng.</li>
    </ul>
    </li>
    <li data-end="299" data-start="279">Lô hàng 2: <code>[4, 3]</code>
    <ul>
         <li data-end="331" data-start="304">Trọng lượng kiện hàng lớn nhất = 4</li>
         <li data-end="411" data-start="336">Trọng lượng kiện hàng cuối cùng = 3, nhỏ hơn nghiêm ngặt 4. Vì vậy, đây là một lô hàng cân bằng.</li>
    </ul>
    </li>

</ul>

<p data-end="519" data-start="413">Không thể chia các kiện hàng để tạo được nhiều hơn hai lô hàng cân bằng, nên đáp án là 2.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">weight = [4,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p data-end="635" data-start="574">Trong trường hợp này, không thể tạo được lô hàng cân bằng nào:</p>

<ul>
    <li data-end="772" data-start="639">Một lô hàng <code>[4, 4]</code> có trọng lượng lớn nhất là 4 và trọng lượng kiện hàng cuối cùng cũng là 4, không nhỏ hơn nghiêm ngặt trọng lượng lớn nhất. Vì vậy, đây không phải là lô hàng cân bằng.</li>
    <li data-end="885" data-start="775">Các lô hàng chỉ có một kiện <code>[4]</code> có trọng lượng kiện hàng cuối cùng bằng trọng lượng kiện hàng lớn nhất, nên không cân bằng.</li>
</ul>

<p data-end="958" data-is-last-node="" data-is-only-node="" data-start="887">Vì không có cách nào tạo được dù chỉ một lô hàng cân bằng, đáp án là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li data-end="8706" data-start="8671"><code data-end="8704" data-start="8671">2 &lt;= n &lt;= 10<sup>5</sup></code></li>
    <li data-end="8733" data-start="8709"><code data-end="8733" data-start="8709">1 &lt;= weight[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy

<!-- thinking:start -->

> **Tư duy**
>
> Một lô hàng cân bằng kết thúc bằng một kiện hàng có trọng lượng nhỏ hơn nghiêm ngặt giá trị lớn nhất của đoạn đó. Các đoạn phải liên tiếp và mục tiêu là tạo được nhiều đoạn nhất có thể.
>
> Cắt sớm nhất có thể: duy trì giá trị lớn nhất hiện tại và kết thúc lô hàng khi xuất hiện một giá trị $x$ nhỏ hơn nghiêm ngặt, sau đó đặt lại giá trị lớn nhất.
>
> Cắt sớm giúp giải phóng các phần tử phía sau để tạo thêm lô hàng mà không bao giờ làm giảm số lượng lô hàng. Chỉ cần duyệt qua mảng một lần.

<!-- thinking:end -->

Ta duy trì giá trị lớn nhất $\text{mx}$ của mảng đang duyệt, rồi lần lượt xét từng phần tử $x$ trong mảng. Nếu $x < \text{mx}$, điều đó có nghĩa là phần tử hiện tại có thể làm kiện hàng cuối cùng của một lô hàng cân bằng, nên ta tăng đáp án lên một và đặt lại $\text{mx}$ về 0. Ngược lại, ta cập nhật $\text{mx}$ thành giá trị của phần tử hiện tại $x$.

Sau khi duyệt xong, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$, chỉ sử dụng thêm một lượng không gian hằng số.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxBalancedShipments(self, weight: List[int]) -> int:
        ans = mx = 0
        for x in weight:
            mx = max(mx, x)
            if x < mx:
                ans += 1
                mx = 0
        return ans
```

#### Java

```java
class Solution {
    public int maxBalancedShipments(int[] weight) {
        int ans = 0;
        int mx = 0;
        for (int x : weight) {
            mx = Math.max(mx, x);
            if (x < mx) {
                ++ans;
                mx = 0;
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
    int maxBalancedShipments(vector<int>& weight) {
        int ans = 0;
        int mx = 0;
        for (int x : weight) {
            mx = max(mx, x);
            if (x < mx) {
                ++ans;
                mx = 0;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxBalancedShipments(weight []int) (ans int) {
    mx := 0
    for _, x := range weight {
        mx = max(mx, x)
        if x < mx {
            ans++
            mx = 0
        }
    }
    return
}
```

#### TypeScript

```ts
function maxBalancedShipments(weight: number[]): number {
    let [ans, mx] = [0, 0];
    for (const x of weight) {
        mx = Math.max(mx, x);
        if (x < mx) {
            ans++;
            mx = 0;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
