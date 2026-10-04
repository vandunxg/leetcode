---
comments: true
difficulty: Easy
tags:
    - JavaScript
---

<!-- problem:start -->

# [2774. Array Upper Bound 🔒](https://leetcode.com/problems/array-upper-bound)

[中文文档](/solution/2700-2799/2774.Array%20Upper%20Bound/README.md)

## Mô tả

<!-- description:start -->

<p>Viết code để mở rộng tất cả các mảng, sao cho bạn có thể gọi phương thức <code>upperBound()</code> trên bất kỳ mảng nào và phương thức này sẽ trả về chỉ số cuối cùng của một số <code>target</code> cho trước. <code>nums</code> là một mảng số được sắp xếp theo thứ tự tăng dần và có thể chứa các phần tử trùng nhau. Nếu không tìm thấy số <code>target</code> trong mảng, hãy trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,4,5], target = 5
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Chỉ số cuối cùng của giá trị target là 2
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,4,5], target = 2
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Vì không có số 2 trong mảng, trả về -1.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,4,6,6,6,6,7], target = 6
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Chỉ số cuối cùng của giá trị target là 5
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>4</sup></code></li>
	<li><code><font face="monospace">-10<sup>4</sup>&nbsp;&lt;= nums[i], target &lt;= 10<sup>4</sup></font></code></li>
	<li><code>nums</code>&nbsp;được sắp xếp theo thứ tự tăng dần.</li>
</ul>

<p>&nbsp;</p>
<strong>Câu hỏi mở rộng: </strong>Bạn có thể viết một thuật toán có độ phức tạp thời gian&nbsp;O(log n)&nbsp;không?

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Tìm chỉ số cuối cùng của $target$ trong một mảng đã sắp xếp. Duyệt từ phải sang trái là đúng, nhưng có thể đạt được độ phức tạp logarit.
>
> Tìm kiếm nhị phân chỉ số đầu tiên có giá trị lớn hơn $target$; chỉ số ngay trước đó là lần xuất hiện ngoài cùng bên phải nếu giá trị tại đó bằng $target$, nếu không thì giá trị này không tồn tại.

<!-- thinking:end -->

Mảng được sắp xếp theo thứ tự không giảm. Dùng tìm kiếm nhị phân để tìm chỉ số đầu tiên có giá trị lớn hơn $\textit{target}$, sau đó kiểm tra xem phần tử ngay trước đó có bằng $\textit{target}$ không. Nếu có, đó là vị trí xuất hiện cuối cùng; nếu không, trả về $-1$.

Độ phức tạp thời gian là $O(\log n)$ và độ phức tạp không gian là $O(1)$, trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### TypeScript

```ts
declare global {
    interface Array<T> {
        upperBound(target: number): number;
    }
}

Array.prototype.upperBound = function (target: number) {
    let left = 0;
    let right = this.length;
    while (left < right) {
        const mid = (left + right) >> 1;
        if (this[mid] > target) {
            right = mid;
        } else {
            left = mid + 1;
        }
    }
    return left > 0 && this[left - 1] == target ? left - 1 : -1;
};

// [3,4,5].upperBound(5); // 2
// [1,4,5].upperBound(2); // -1
// [3,4,6,6,6,6,7].upperBound(6) // 5
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Duyệt tuyến tính

<!-- thinking:start -->

> **Tư duy**
>
> Tìm kiếm nhị phân đạt độ phức tạp logarit nhưng cần xử lý logic ở hai đầu mảng. $lastIndexOf$ duyệt một lần từ phải sang trái và ngắn gọn hơn, dù độ phức tạp trong trường hợp xấu nhất là tuyến tính.

<!-- thinking:end -->

Gọi `lastIndexOf` để duyệt từ phải sang trái và trả về chỉ số cuối cùng của target, hoặc $-1$ nếu không tồn tại.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$, trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### TypeScript

```ts
declare global {
    interface Array<T> {
        upperBound(target: number): number;
    }
}

Array.prototype.upperBound = function (target: number) {
    return this.lastIndexOf(target);
};

// [3,4,5].upperBound(5); // 2
// [1,4,5].upperBound(2); // -1
// [3,4,6,6,6,6,7].upperBound(6) // 5
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
