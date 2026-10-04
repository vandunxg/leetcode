---
comments: true
difficulty: Hard
tags:
    - JavaScript
---

<!-- problem:start -->

# [2759. Convert JSON String to Object 🔒](https://leetcode.com/problems/convert-json-string-to-object)

[中文文档](/solution/2700-2799/2759.Convert%20JSON%20String%20to%20Object/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>str</code>, hãy trả về JSON đã phân tích cú pháp <code>parsedStr</code>. Có thể giả sử <code>str</code> là một chuỗi JSON hợp lệ, nên nó chỉ bao gồm chuỗi, số, mảng, object, boolean và null. <code>str</code> không chứa các ký tự ẩn hoặc ký tự escape.</p>

<p>Hãy giải bài toán mà không sử dụng phương thức tích hợp sẵn <code>JSON.parse</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> str = &#39;{&quot;a&quot;:2,&quot;b&quot;:[1,2,3]}&#39;
<strong>Đầu ra:</strong> {&quot;a&quot;:2,&quot;b&quot;:[1,2,3]}
<strong>Giải thích:</strong>&nbsp;Trả về object được biểu diễn bởi chuỗi JSON.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> str = &#39;true&#39;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Các kiểu nguyên thủy là JSON hợp lệ.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> str = &#39;[1,5,&quot;false&quot;,{&quot;a&quot;:2}]&#39;
<strong>Đầu ra:</strong> [1,5,&quot;false&quot;,{&quot;a&quot;:2}]
<strong>Giải thích:</strong> Trả về mảng được biểu diễn bởi chuỗi JSON.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>str</code> là một chuỗi JSON hợp lệ</li>
	<li><code>1 &lt;= str.length &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Phân tích cú pháp một chuỗi JSON đúng định dạng mà không dùng $eval$ hoặc $JSON.parse$. Việc tách chuỗi bằng regular expression không xử lý được cấu trúc lồng nhau hoặc ký tự escape.
>
> Duy trì một chỉ số $i$ và chọn parser dựa trên ký tự hiện tại: dùng các parser đệ quy cho object, mảng, chuỗi, số và ba literal. Chuỗi xử lý dấu gạch chéo ngược; các cấu trúc hợp thành dừng khi gặp dấu phẩy hoặc dấu ngoặc đóng. Một lần gọi từ nút gốc sẽ phân tích cú pháp giá trị.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function jsonParse(str: string): any {
    const n = str.length;
    let i = 0;

    const parseTrue = (): boolean => {
        i += 4;
        return true;
    };

    const parseFalse = (): boolean => {
        i += 5;
        return false;
    };

    const parseNull = (): null => {
        i += 4;
        return null;
    };

    const parseNumber = (): number => {
        let s = '';
        while (i < n) {
            const c = str[i];
            if (c === ',' || c === '}' || c === ']') {
                break;
            }
            s += c;
            i++;
        }
        return Number(s);
    };

    const parseArray = (): any[] => {
        const arr: any[] = [];
        i++;
        while (i < n) {
            const c = str[i];
            if (c === ']') {
                i++;
                break;
            }
            if (c === ',') {
                i++;
                continue;
            }
            const value = parseValue();
            arr.push(value);
        }
        return arr;
    };

    const parseString = (): string => {
        let s = '';
        i++;
        while (i < n) {
            const c = str[i];
            if (c === '"') {
                i++;
                break;
            }
            if (c === '\\') {
                i++;
                s += str[i];
            } else {
                s += c;
            }
            i++;
        }
        return s;
    };

    const parseObject = (): any => {
        const obj: any = {};
        i++;
        while (i < n) {
            const c = str[i];
            if (c === '}') {
                i++;
                break;
            }
            if (c === ',') {
                i++;
                continue;
            }
            const key = parseString();
            i++;
            const value = parseValue();
            obj[key] = value;
        }
        return obj;
    };
    const parseValue = (): any => {
        const c = str[i];
        if (c === '{') {
            return parseObject();
        }
        if (c === '[') {
            return parseArray();
        }
        if (c === '"') {
            return parseString();
        }
        if (c === 't') {
            return parseTrue();
        }
        if (c === 'f') {
            return parseFalse();
        }
        if (c === 'n') {
            return parseNull();
        }
        return parseNumber();
    };
    return parseValue();
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
