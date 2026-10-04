---
comments: true
difficulty: Medium
tags:
    - Database
---

<!-- problem:start -->

# [3050. Pizza Toppings Cost Analysis 🔒](https://leetcode.com/problems/pizza-toppings-cost-analysis)

[中文文档](/solution/3000-3099/3050.Pizza%20Toppings%20Cost%20Analysis/README.md)

## Mô tả

<!-- description:start -->

<p>Bảng: <code><font face="monospace">Toppings</font></code></p>

<pre>
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| topping_name | varchar |
| cost         | decimal |
+--------------+---------+
topping_name là khóa chính của bảng này.
Mỗi hàng trong bảng chứa tên topping và chi phí của topping đó.
</pre>

<p>Hãy viết một lời giải để tính <strong>tổng chi phí</strong> của <strong>tất cả các tổ hợp pizza gồm <code>3</code> topping</strong> có thể tạo từ danh sách topping đã cho. Tổng chi phí của các topping phải được <strong>làm tròn</strong> đến <code>2</code> chữ số <strong>thập phân</strong>.</p>

<p><strong>Lưu ý:</strong></p>

<ul>
	<li><strong>Không</strong> bao gồm những pizza có topping bị <strong>lặp lại</strong>. Ví dụ: &lsquo;Pepperoni, Pepperoni, Onion Pizza&rsquo;.</li>
	<li>Các topping <strong>phải được</strong> liệt kê theo <strong>thứ tự bảng chữ cái</strong>. Ví dụ: &#39;Chicken, Onions, Sausage&#39;. &#39;Onion, Sausage, Chicken&#39; là không hợp lệ.</li>
</ul>

<p>Trả về<em> bảng kết quả được sắp xếp theo tổng chi phí theo thứ tự </em><em><strong>giảm dần</strong></em><em> và tổ hợp topping theo thứ tự <strong>tăng dần</strong>.</em></p>

<p>Định dạng kết quả như trong ví dụ sau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong>
Bảng Toppings:
+--------------+------+
| topping_name | cost |
+--------------+------+
| Pepperoni    | 0.50 |
| Sausage      | 0.70 |
| Chicken      | 0.55 |
| Extra Cheese | 0.40 |
+--------------+------+
<strong>Đầu ra:</strong>
+--------------------------------+------------+
| pizza                          | total_cost |
+--------------------------------+------------+
| Chicken,Pepperoni,Sausage      | 1.75       |
| Chicken,Extra Cheese,Sausage   | 1.65       |
| Extra Cheese,Pepperoni,Sausage | 1.60       |
| Chicken,Extra Cheese,Pepperoni | 1.45       |
+--------------------------------+------------+
<strong>Giải thích:</strong>
Chỉ có bốn tổ hợp khác nhau có thể tạo với ba topping:
- Chicken, Pepperoni, Sausage: Tổng chi phí là $1.75 (Chicken $0.55, Pepperoni $0.50, Sausage $0.70).
- Chicken, Extra Cheese, Sausage: Tổng chi phí là $1.65 (Chicken $0.55, Extra Cheese $0.40, Sausage $0.70).
- Extra Cheese, Pepperoni, Sausage: Tổng chi phí là $1.60 (Extra Cheese $0.40, Pepperoni $0.50, Sausage $0.70).
- Chicken, Extra Cheese, Pepperoni: Tổng chi phí là $1.45 (Chicken $0.55, Extra Cheese $0.40, Pepperoni $0.50).
Bảng kết quả được sắp xếp theo tổng chi phí giảm dần.</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hàm cửa sổ + JOIN có điều kiện

<!-- thinking:start -->

> **Tư duy**
>
> Chúng ta cần mọi tổ hợp gồm ba topping cùng chi phí tương ứng, trong đó tên được nối theo thứ tự từ điển. Một phép tự JOIN ba bảng đơn giản sẽ tạo ra cả các hoán vị.
>
> Đánh số thứ hạng cho tên rồi JOIN với điều kiện $r_1<r_2<r_3$ sẽ tạo ra mỗi tổ hợp đúng một lần và tên đã ở đúng thứ tự.
>
> Dùng rank dạng window cùng hai phép JOIN bất đẳng thức, sau đó sắp xếp theo chi phí giảm dần và tên tăng dần.

<!-- thinking:end -->

Đầu tiên, chúng ta dùng hàm cửa sổ để sắp xếp bảng theo trường `topping_name` và thêm trường `rk` vào mỗi hàng, biểu thị thứ hạng của hàng hiện tại.

Sau đó, chúng ta dùng phép JOIN có điều kiện để JOIN bảng `T` ba lần, lần lượt đặt tên là `t1`, `t2`, `t3`. Điều kiện JOIN là `t1.rk < t2.rk` và `t2.rk < t3.rk`. Tiếp theo, chúng ta tính tổng giá của ba topping, sắp xếp theo giá giảm dần, rồi sắp xếp theo tên topping tăng dần.

<!-- tabs:start -->

#### MySQL

```sql
# Write your MySQL query statement below
WITH
    T AS (
        SELECT *, RANK() OVER (ORDER BY topping_name) AS rk
        FROM Toppings
    )
SELECT
    CONCAT(t1.topping_name, ',', t2.topping_name, ',', t3.topping_name) AS pizza,
    t1.cost + t2.cost + t3.cost AS total_cost
FROM
    T AS t1
    JOIN T AS t2 ON t1.rk < t2.rk
    JOIN T AS t3 ON t2.rk < t3.rk
ORDER BY 2 DESC, 1 ASC;
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
