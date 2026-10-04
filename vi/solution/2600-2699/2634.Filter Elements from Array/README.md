---
comments: true
difficulty: Easy
tags:
    - JavaScript
---

<!-- problem:start -->

# [2634. Filter Elements from Array](https://leetcode.com/problems/filter-elements-from-array)

[中文文档](/solution/2600-2699/2634.Filter%20Elements%20from%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>arr</code> và một hàm lọc <code>fn</code>, hãy trả về mảng đã lọc <code>filteredArr</code>.</p>

<p>Hàm <code>fn</code> nhận một hoặc hai đối số:</p>

<ul>
	<li><code>arr[i]</code> - số&nbsp;trong&nbsp;<code>arr</code></li>
	<li><code>i</code>&nbsp;- chỉ số của <code>arr[i]</code></li>
</ul>

<p><code>filteredArr</code> chỉ nên chứa những phần tử trong&nbsp;<code>arr</code> mà biểu thức <code>fn(arr[i], i)</code> đánh giá thành một giá trị <strong>truthy</strong>. Giá trị&nbsp;<strong>truthy</strong>&nbsp;là giá trị mà&nbsp;<code>Boolean(value)</code>&nbsp;trả về&nbsp;<code>true</code>.</p>

<p>Hãy giải bài toán mà không sử dụng phương thức dựng sẵn <code>Array.filter</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [0,10,20,30], fn = function greaterThan10(n) { return n &gt; 10; }
<strong>Đầu ra:</strong> [20,30]
<strong>Giải thích:</strong>
const newArray = filter(arr, fn); // [20, 30]
Hàm loại bỏ các giá trị không lớn hơn 10</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1,2,3], fn = function firstIndex(n, i) { return i === 0; }
<strong>Đầu ra:</strong> [1]
<strong>Giải thích:</strong>
fn cũng có thể nhận chỉ số của mỗi phần tử
Trong trường hợp này, hàm loại bỏ các phần tử không ở chỉ số 0
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [-2,-1,0,1,2], fn = function plusOne(n) { return n + 1 }
<strong>Đầu ra:</strong> [-2,0,1,2]
<strong>Giải thích:</strong>
Các giá trị falsey như 0 sẽ bị loại bỏ
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= arr.length &lt;= 1000</code></li>
	<li><code>-10<sup>9</sup>&nbsp;&lt;= arr[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Không được sử dụng `Array.filter`. Một lượt duyệt tuần tự sẽ giữ lại các phần tử mà $fn(arr[i],i)$ là truthy, đồng thời bảo toàn thứ tự.

<!-- thinking:end -->

Chúng ta duyệt qua mảng $arr$ và với mỗi phần tử $arr[i]$, nếu $fn(arr[i], i)$ là true, chúng ta thêm phần tử đó vào mảng kết quả. Cuối cùng, trả về mảng kết quả.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $arr$. Không tính phần bộ nhớ dành cho mảng kết quả, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### TypeScript

```ts
function filter(arr: number[], fn: (n: number, i: number) => any): number[] {
    const ans: number[] = [];
    for (let i = 0; i < arr.length; ++i) {
        if (fn(arr[i], i)) {
            ans.push(arr[i]);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
