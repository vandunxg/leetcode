---
comments: true
difficulty: Medium
rating: 1498
source: Weekly Contest 246 Q2
tags:
    - Math
    - String
---

<!-- problem:start -->

# [1904. The Number of Full Rounds You Have Played](https://leetcode.com/problems/the-number-of-full-rounds-you-have-played)

[中文文档](/solution/1900-1999/1904.The%20Number%20of%20Full%20Rounds%20You%20Have%20Played/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn đang tham gia một giải đấu cờ vua trực tuyến. Một ván cờ bắt đầu sau mỗi <code>15</code> phút. Ván đầu tiên trong ngày bắt đầu lúc <code>00:00</code>, và cứ sau mỗi <code>15</code> phút lại có một ván mới bắt đầu.</p>

<ul>
	<li>Ví dụ, ván thứ hai bắt đầu lúc <code>00:15</code>, ván thứ tư bắt đầu lúc <code>00:45</code>, và ván thứ bảy bắt đầu lúc <code>01:30</code>.</li>
</ul>

<p>Cho hai chuỗi <code>loginTime</code> và <code>logoutTime</code>, trong đó:</p>

<ul>
	<li><code>loginTime</code> là thời điểm bạn đăng nhập vào trò chơi, và</li>
	<li><code>logoutTime</code> là thời điểm bạn đăng xuất khỏi trò chơi.</li>
</ul>

<p>Nếu <code>logoutTime</code> <strong>sớm hơn</strong> <code>loginTime</code>, điều đó có nghĩa là bạn đã chơi từ <code>loginTime</code> đến nửa đêm và từ nửa đêm đến <code>logoutTime</code>.</p>

<p>Trả về <em>số ván cờ hoàn chỉnh mà bạn đã chơi trong giải đấu</em>.</p>

<p><strong>Lưu ý:</strong>&nbsp;Tất cả thời điểm đã cho đều tuân theo đồng hồ 24 giờ. Nghĩa là ván đầu tiên trong ngày bắt đầu lúc <code>00:00</code> và ván cuối cùng bắt đầu lúc <code>23:45</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> loginTime = &quot;09:31&quot;, logoutTime = &quot;10:14&quot;
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Bạn đã chơi trọn một ván từ 09:45 đến 10:00.
Bạn không chơi trọn ván từ 09:30 đến 09:45 vì đã đăng nhập lúc 09:31, sau khi ván bắt đầu.
Bạn không chơi trọn ván từ 10:00 đến 10:15 vì đã đăng xuất lúc 10:14, trước khi ván kết thúc.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> loginTime = &quot;21:30&quot;, logoutTime = &quot;03:00&quot;
<strong>Đầu ra:</strong> 22
<strong>Giải thích:</strong> Bạn đã chơi 10 ván hoàn chỉnh từ 21:30 đến 00:00 và 12 ván hoàn chỉnh từ 00:00 đến 03:00.
10 + 12 = 22.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>loginTime</code> và <code>logoutTime</code> có định dạng <code>hh:mm</code>.</li>
	<li><code>00 &lt;= hh &lt;= 23</code></li>
	<li><code>00 &lt;= mm &lt;= 59</code></li>
	<li><code>loginTime</code> và <code>logoutTime</code> không bằng nhau.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Chuyển đổi sang phút

<!-- thinking:start -->

> **Tư duy**
>
> Những ván chơi trọn vẹn bắt đầu tại các mốc cách nhau $15$ phút, còn nếu thời điểm đăng xuất sớm hơn thời điểm đăng nhập thì phiên chơi đã đi qua nửa đêm. Chuyển cả hai thời điểm thành số phút rồi cộng thêm $1440$ khi cần sẽ tránh phải xử lý ngày tháng ở dạng chuỗi.
>
> Thời điểm bắt đầu phải được làm tròn lên mốc bắt đầu của ván tiếp theo, còn thời điểm kết thúc phải được làm tròn xuống mốc trước đó; hiệu của chúng khi tính theo đơn vị $15$ phút chính là số ván.
>
> Cộng $14$ trước khi chia thời điểm bắt đầu cho $15$, chia trực tiếp thời điểm kết thúc, rồi chặn kết quả ở 0 sẽ xử lý cả những phiên không có ván hoàn chỉnh nào.

<!-- thinking:end -->

Ta có thể chuyển các chuỗi đầu vào thành số phút $a$ và $b$. Nếu $a > b$, nghĩa là phiên chơi đi qua nửa đêm, nên ta cần cộng số phút trong một ngày là $1440$ vào $b$.

Sau đó, làm tròn $a$ lên bội số gần nhất của $15$ và làm tròn $b$ xuống bội số gần nhất của $15$. Cuối cùng, trả về hiệu giữa $b$ và $a$. Lưu ý rằng ta nên lấy giá trị lớn hơn giữa $0$ và $b - a$.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfRounds(self, loginTime: str, logoutTime: str) -> int:
        def f(s: str) -> int:
            return int(s[:2]) * 60 + int(s[3:])

        a, b = f(loginTime), f(logoutTime)
        if a > b:
            b += 1440
        a, b = (a + 14) // 15, b // 15
        return max(0, b - a)
```

#### Java

```java
class Solution {
    public int numberOfRounds(String loginTime, String logoutTime) {
        int a = f(loginTime), b = f(logoutTime);
        if (a > b) {
            b += 1440;
        }
        return Math.max(0, b / 15 - (a + 14) / 15);
    }

    private int f(String s) {
        int h = Integer.parseInt(s.substring(0, 2));
        int m = Integer.parseInt(s.substring(3, 5));
        return h * 60 + m;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfRounds(string loginTime, string logoutTime) {
        auto f = [](string& s) {
            int h, m;
            sscanf(s.c_str(), "%d:%d", &h, &m);
            return h * 60 + m;
        };
        int a = f(loginTime), b = f(logoutTime);
        if (a > b) {
            b += 1440;
        }
        return max(0, b / 15 - (a + 14) / 15);
    }
};
```

#### Go

```go
func numberOfRounds(loginTime string, logoutTime string) int {
	f := func(s string) int {
		var h, m int
		fmt.Sscanf(s, "%d:%d", &h, &m)
		return h*60 + m
	}
	a, b := f(loginTime), f(logoutTime)
	if a > b {
		b += 1440
	}
	return max(0, b/15-(a+14)/15)
}
```

#### TypeScript

```ts
function numberOfRounds(startTime: string, finishTime: string): number {
    const f = (s: string): number => {
        const [h, m] = s.split(':').map(Number);
        return h * 60 + m;
    };
    let [a, b] = [f(startTime), f(finishTime)];
    if (a > b) {
        b += 1440;
    }
    return Math.max(0, Math.floor(b / 15) - Math.ceil(a / 15));
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
