---
comments: true
difficulty: Medium
tags:
    - JavaScript
---

<!-- problem:start -->

# [2618. Check if Object Instance of Class](https://leetcode.com/problems/check-if-object-instance-of-class)

[中文文档](/solution/2600-2699/2618.Check%20if%20Object%20Instance%20of%20Class/README.md)

## Mô tả

<!-- description:start -->

<p>Viết một hàm kiểm tra xem một giá trị đã cho có phải là instance của một class hoặc superclass đã cho hay không. Trong bài toán này, một object được coi là instance của một class nếu object đó có quyền truy cập vào các method của class đó.</p>

<p>Không có ràng buộc nào về kiểu dữ liệu có thể được truyền vào hàm. Ví dụ, giá trị hoặc class có thể là <code>undefined</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> func = () =&gt; checkIfInstanceOf(new Date(), Date)
<strong>Đầu ra:</strong> true
<strong>Giải thích: </strong>Object được trả về bởi constructor Date, theo định nghĩa, là một instance của Date.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> func = () =&gt; { class Animal {}; class Dog extends Animal {}; return checkIfInstanceOf(new Dog(), Animal); }
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong>
class Animal {};
class Dog extends Animal {};
checkIfInstanceOf(new Dog(), Animal); // true

Dog là subclass của Animal. Vì vậy, một object Dog là instance của cả Dog và Animal.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> func = () =&gt; checkIfInstanceOf(Date, Date)
<strong>Đầu ra:</strong> false
<strong>Giải thích: </strong>Một constructor của date không thể về mặt logic là instance của chính nó.
</pre>

<p><strong class="example">Ví dụ 4:</strong></p>

<pre>
<strong>Đầu vào:</strong> func = () =&gt; checkIfInstanceOf(5, Number)
<strong>Đầu ra:</strong> true
<strong>Giải thích: </strong>5 là một Number. Lưu ý rằng từ khóa "instanceof" sẽ trả về false. Tuy nhiên, 5 vẫn được coi là một instance của Number vì nó truy cập được các method của Number, chẳng hạn như "toFixed()".
</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần xác định một object có nằm trên prototype chain của một constructor hay không, bao gồm cả boxed primitive và `null`/`undefined`. `instanceof` native không xử lý được primitive và không kiểm tra constructor không hợp lệ.
>
> Duyệt bằng `Object.getPrototypeOf` sẽ tái hiện phép kiểm tra này: nếu kết quả bằng `classFunction.prototype` thì trả về thành công. Nếu constructor không tồn tại thì trả về false ngay.
>
> Loại bỏ trường hợp `classFunction` là nullish, sau đó lần lượt đi lên qua các prototype cho đến khi kết thúc prototype chain.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function checkIfInstanceOf(obj: any, classFunction: any): boolean {
    if (classFunction === null || classFunction === undefined) {
        return false;
    }
    while (obj !== null && obj !== undefined) {
        const proto = Object.getPrototypeOf(obj);
        if (proto === classFunction.prototype) {
            return true;
        }
        obj = proto;
    }
    return false;
}

/**
 * checkIfInstanceOf(new Date(), Date); // true
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
