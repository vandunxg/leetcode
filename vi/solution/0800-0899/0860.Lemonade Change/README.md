---
comments: true
difficulty: Easy
tags:
    - Greedy
    - Array
---

<!-- problem:start -->

# [860. Lemonade Change](https://leetcode.com/problems/lemonade-change)

[中文文档](/solution/0800-0899/0860.Lemonade%20Change/README.md)

## Mô tả

<!-- description:start -->

<p>Tại quầy bán nước chanh, mỗi ly có giá <code>$5</code>. Khách hàng xếp hàng mua và lần lượt gọi món theo thứ tự trong mảng <code>bills</code>. Mỗi khách chỉ mua một ly và thanh toán bằng tờ <code>$5</code>, <code>$10</code> hoặc <code>$20</code>. Bạn phải thối tiền chính xác để số tiền khách thực trả là <code>$5</code>.</p>

<p>Lưu ý, ban đầu bạn không có tiền lẻ.</p>

<p>Cho mảng số nguyên <code>bills</code>, trong đó <code>bills[i]</code> là tờ tiền khách thứ <code>i<sup>th</sup></code> dùng để thanh toán. Hãy trả về <code>true</code> <em>nếu bạn có thể thối tiền chính xác cho mọi khách hàng; nếu không thì trả về</em> <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> bills = [5,5,5,10,20]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> 
Từ 3 khách đầu tiên, ta lần lượt nhận được ba tờ $5.
Từ khách thứ tư, ta nhận tờ $10 và thối lại một tờ $5.
Với khách thứ năm, ta thối lại một tờ $10 và một tờ $5.
Vì mọi khách đều được thối tiền chính xác, ta trả về true.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> bills = [5,5,10,10,20]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> 
Từ hai khách đầu tiên, ta lần lượt nhận được hai tờ $5.
Với hai khách tiếp theo, ta nhận một tờ $10 và thối lại một tờ $5 cho mỗi người.
Với khách cuối cùng, ta không thể thối lại $15 vì chỉ còn hai tờ $10.
Vì không phải khách nào cũng được thối tiền chính xác, đáp án là false.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= bills.length &lt;= 10<sup>5</sup></code></li>
	<li><code>bills[i]</code> là <code>5</code>, <code>10</code> hoặc <code>20</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các tờ tiền chỉ có mệnh giá $5$, $10$ và $20$, còn tiền thối phải lấy từ số tiền đã nhận. Vì $n\le 10^5$, ta dùng greedy: với tờ $20$, ưu tiên thối một tờ $10$ và một tờ $5$; nếu không có thì thối ba tờ $5$.
>
> Theo dõi số tờ $5$ và $10$ đang có; nếu số tờ $5$ âm thì không thể thối tiền chính xác. Không cần lưu toàn bộ lịch sử giao dịch.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def lemonadeChange(self, bills: List[int]) -> bool:
        five = ten = 0
        for v in bills:
            if v == 5:
                five += 1
            elif v == 10:
                ten += 1
                five -= 1
            else:
                if ten:
                    ten -= 1
                    five -= 1
                else:
                    five -= 3
            if five < 0:
                return False
        return True
```

#### Java

```java
class Solution {
    public boolean lemonadeChange(int[] bills) {
        int five = 0, ten = 0;
        for (int v : bills) {
            switch (v) {
                case 5 -> ++five;
                case 10 -> {
                    ++ten;
                    --five;
                }
                case 20 -> {
                    if (ten > 0) {
                        --ten;
                        --five;
                    } else {
                        five -= 3;
                    }
                }
            }
            if (five < 0) {
                return false;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool lemonadeChange(vector<int>& bills) {
        int five = 0, ten = 0;
        for (int v : bills) {
            if (v == 5) {
                ++five;
            } else if (v == 10) {
                ++ten;
                --five;
            } else {
                if (ten) {
                    --ten;
                    --five;
                } else {
                    five -= 3;
                }
            }
            if (five < 0) {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func lemonadeChange(bills []int) bool {
	five, ten := 0, 0
	for _, v := range bills {
		if v == 5 {
			five++
		} else if v == 10 {
			ten++
			five--
		} else {
			if ten > 0 {
				ten--
				five--
			} else {
				five -= 3
			}
		}
		if five < 0 {
			return false
		}
	}
	return true
}
```

#### TypeScript

```ts
function lemonadeChange(bills: number[]): boolean {
    let [five, ten] = [0, 0];
    for (const x of bills) {
        switch (x) {
            case 5:
                five++;
                break;
            case 10:
                five--;
                ten++;
                break;
            case 20:
                if (ten) {
                    ten--;
                    five--;
                } else {
                    five -= 3;
                }
                break;
        }

        if (five < 0) {
            return false;
        }
    }
    return true;
}
```

#### JavaScript

```js
function lemonadeChange(bills) {
    let [five, ten] = [0, 0];
    for (const x of bills) {
        switch (x) {
            case 5:
                five++;
                break;
            case 10:
                five--;
                ten++;
                break;
            case 20:
                if (ten) {
                    ten--;
                    five--;
                } else {
                    five -= 3;
                }
                break;
        }

        if (five < 0) {
            return false;
        }
    }
    return true;
}
```

#### Rust

```rust
impl Solution {
    pub fn lemonade_change(bills: Vec<i32>) -> bool {
        let (mut five, mut ten) = (0, 0);
        for bill in bills.iter() {
            match bill {
                5 => {
                    five += 1;
                }
                10 => {
                    five -= 1;
                    ten += 1;
                }
                _ => {
                    if ten != 0 {
                        ten -= 1;
                        five -= 1;
                    } else {
                        five -= 3;
                    }
                }
            }

            if five < 0 {
                return false;
            }
        }
        true
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Viết gọn trên một dòng

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 đã xử lý đầy đủ mọi quy tắc thối tiền. Lời giải 2 gộp hai biến đếm tương tự vào một lượt quét bằng $\textit{every}$, phân nhánh theo mệnh giá bằng các phép kiểm tra xor.
>
> Ý tưởng không đổi; chỉ viết phần cài đặt thành một dòng, và vẫn thất bại khi số tờ $5$ âm.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
const lemonadeChange = (bills: number[], f = 0, t = 0): boolean =>
    bills.every(
        x => (
            (!(x ^ 5) && ++f) ||
                (!(x ^ 10) && (--f, ++t)) ||
                (!(x ^ 20) && (t ? (f--, t--) : (f -= 3), 1)),
            f >= 0
        ),
    );
```

#### JavaScript

```js
const lemonadeChange = (bills, f = 0, t = 0) =>
    bills.every(
        x => (
            (!(x ^ 5) && ++f) ||
                (!(x ^ 10) && (--f, ++t)) ||
                (!(x ^ 20) && (t ? (f--, t--) : (f -= 3), 1)),
            f >= 0
        ),
    );
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
