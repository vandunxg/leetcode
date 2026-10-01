---
comments: true
---

<!-- problem:start -->

# [05.02. Binary Number to String](https://leetcode.cn/problems/binary-number-to-string-lcci)

[中文文档](/lcci/05.02.Binary%20Number%20to%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số thực nằm giữa O và 1 (ví dụ: 0.72) được truyền vào dưới dạng double, hãy in ra biểu diễn nhị phân. Nếu số đó không thể được biểu diễn chính xác trong hệ nhị phân với tối đa 32 ký tự, hãy in &quot;ERROR&quot;.</p>
<p><strong>Ví dụ 1:</strong></p>
<pre>

<strong> Input</strong>: 0.625

<strong> Output</strong>: &quot;0.101&quot;

</pre>
<p><strong>Ví dụ 2:</strong></p>
<pre>

<strong> Input</strong>: 0.1

<strong> Output</strong>: &quot;ERROR&quot;

<strong> Note</strong>: 0.1 không thể được biểu diễn chính xác trong hệ nhị phân.

</pre>

<p><strong>Note: </strong></p>
<ol>
	<li>Hai ký tự &quot;0.&quot; này cũng được tính vào 32 ký tự.</li>
</ol>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phân số thập phân sang phân số nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Một số thực trong $(0,1)$ cần được viết ở dạng nhị phân. Một số thập phân hữu hạn có thể lặp vô hạn trong hệ nhị phân; nếu cần hơn $32$ bit thì kết quả là lỗi.
>
> Nhân đôi phân số sẽ cho bit tiếp theo; phần phân số còn lại tiếp tục được nhân đôi. Nếu $32$ bit vẫn chưa đủ để đưa phần còn lại về $0$, số đó không thể được biểu diễn.
>
> Bắt đầu với `0.`, lặp `num *= 2`, thêm phần nguyên rồi trừ phần nguyên đó. Nếu còn phần $num$ thì trả về `ERROR`.

<!-- thinking:end -->

Phương pháp chuyển một phân số thập phân thành phân số nhị phân như sau: nhân phần thập phân với $2$, lấy phần nguyên làm chữ số tiếp theo của phân số nhị phân, rồi lấy phần thập phân làm số bị nhân cho lần nhân tiếp theo, cho đến khi phần thập phân bằng $0$ hoặc độ dài của phân số nhị phân vượt quá $32$ bit.

Ví dụ, giả sử ta muốn chuyển $0.8125$ thành phân số nhị phân, quá trình thực hiện như sau:

$$
\begin{aligned}
0.8125 \times 2 &= 1.625 \quad \textit{take the integer part} \quad 1 \\
0.625 \times 2 &= 1.25 \quad \textit{take the integer part} \quad 1 \\
0.25 \times 2 &= 0.5 \quad \textit{take the integer part} \quad 0 \\
0.5 \times 2 &= 1 \quad \textit{take the integer part} \quad 1 \\
\end{aligned}
$$

Vì vậy, biểu diễn phân số nhị phân của phân số thập phân $0.8125$ là $0.1101_{(2)}$.

Trong bài này, vì số thực nằm giữa $0$ và $1$, phần nguyên của nó chắc chắn là $0$. Ta chỉ cần chuyển phần thập phân thành phân số nhị phân theo phương pháp trên. Dừng chuyển đổi khi phần thập phân bằng $0$ hoặc độ dài của phân số nhị phân không nhỏ hơn $32$ bit.

Cuối cùng, nếu phần thập phân khác $0$, điều đó có nghĩa là số thực không thể được biểu diễn trong hệ nhị phân với tối đa $32$ bit, hãy trả về chuỗi `"ERROR"`. Nếu không, hãy trả về phân số nhị phân đã chuyển đổi.

Độ phức tạp thời gian là $O(C)$, độ phức tạp không gian là $O(C)$. Trong đó, $C$ là độ dài của phân số nhị phân, tối đa là $32$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def printBin(self, num: float) -> str:
        ans = '0.'
        while len(ans) < 32 and num:
            num *= 2
            x = int(num)
            ans += str(x)
            num -= x
        return 'ERROR' if num else ans
```

#### Java

```java
class Solution {
    public String printBin(double num) {
        StringBuilder ans = new StringBuilder("0.");
        while (ans.length() < 32 && num != 0) {
            num *= 2;
            int x = (int) num;
            ans.append(x);
            num -= x;
        }
        return num != 0 ? "ERROR" : ans.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string printBin(double num) {
        string ans = "0.";
        while (ans.size() < 32 && num != 0) {
            num *= 2;
            int x = (int) num;
            ans.push_back('0' + x);
            num -= x;
        }
        return num != 0 ? "ERROR" : ans;
    }
};
```

#### Go

```go
func printBin(num float64) string {
	ans := &strings.Builder{}
	ans.WriteString("0.")
	for ans.Len() < 32 && num != 0 {
		num *= 2
		x := byte(num)
		ans.WriteByte('0' + x)
		num -= float64(x)
	}
	if num != 0 {
		return "ERROR"
	}
	return ans.String()
}
```

#### Swift

```swift
class Solution {
    func printBin(_ num: Double) -> String {
        var num = num
        var ans = "0."

        while ans.count < 32 && num != 0 {
            num *= 2
            let x = Int(num)
            ans.append("\(x)")
            num -= Double(x)
        }

        return num != 0 ? "ERROR" : ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
