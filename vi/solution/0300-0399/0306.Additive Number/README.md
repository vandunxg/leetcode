---
comments: true
difficulty: Medium
tags:
    - String
    - Backtracking
---

<!-- problem:start -->

# [306. Additive Number](https://leetcode.com/problems/additive-number)

[中文文档](/solution/0300-0399/0306.Additive%20Number/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Số cộng</strong> là chuỗi mà các chữ số có thể tạo thành một <strong>dãy cộng</strong>.</p>

<p>Một <strong>dãy cộng</strong> hợp lệ phải có <strong>ít nhất</strong> ba số. Từ số thứ ba trở đi, mỗi số trong dãy phải bằng tổng của hai số đứng trước nó.</p>

<p>Cho một chuỗi chỉ chứa chữ số, hãy trả về <code>true</code> nếu đó là <strong>số cộng</strong>, nếu không thì trả về <code>false</code>.</p>

<p><strong>Lưu ý:</strong> Các số trong dãy cộng <strong>không được</strong> có số 0 ở đầu, vì vậy dãy <code>1, 2, 03</code> hoặc <code>1, 02, 3</code> không hợp lệ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> &quot;112358&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> 
Các chữ số có thể tạo thành dãy cộng: 1, 1, 2, 3, 5, 8. 
1 + 1 = 2, 1 + 2 = 3, 2 + 3 = 5, 3 + 5 = 8
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> &quot;199100199&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> 
Dãy cộng là: 1, 99, 100, 199.&nbsp;
1 + 99 = 100, 99 + 100 = 199
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= num.length &lt;= 35</code></li>
	<li><code>num</code> chỉ gồm các chữ số.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Bạn sẽ xử lý thế nào khi các số nguyên đầu vào quá lớn và gây tràn số?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Số cộng là chuỗi ghép từ ít nhất ba số, trong đó từ số thứ ba trở đi, mỗi số bằng tổng của hai số đứng trước. Khi đã xác định hai số đầu tiên, phần còn lại của chuỗi sẽ được quyết định.
>
> Duyệt các vị trí tách để chọn hai số đầu tiên, bỏ qua trường hợp có số 0 ở đầu, rồi đệ quy kiểm tra từng tiền tố còn lại có bằng tổng của hai số trước đó hay không. Nếu khớp, chuyển sang cặp số tiếp theo; thành công khi đã xử lý hết chuỗi. Giới hạn độ dài giúp số trường hợp cần duyệt vẫn khả thi.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isAdditiveNumber(self, num: str) -> bool:
        def dfs(a, b, num):
            if not num:
                return True
            if a + b > 0 and num[0] == '0':
                return False
            for i in range(1, len(num) + 1):
                if a + b == int(num[:i]):
                    if dfs(b, a + b, num[i:]):
                        return True
            return False

        n = len(num)
        for i in range(1, n - 1):
            for j in range(i + 1, n):
                if i > 1 and num[0] == '0':
                    break
                if j - i > 1 and num[i] == '0':
                    continue
                if dfs(int(num[:i]), int(num[i:j]), num[j:]):
                    return True
        return False
```

#### Java

```java
class Solution {
    public boolean isAdditiveNumber(String num) {
        int n = num.length();
        for (int i = 1; i < Math.min(n - 1, 19); ++i) {
            for (int j = i + 1; j < Math.min(n, i + 19); ++j) {
                if (i > 1 && num.charAt(0) == '0') {
                    break;
                }
                if (j - i > 1 && num.charAt(i) == '0') {
                    continue;
                }
                long a = Long.parseLong(num.substring(0, i));
                long b = Long.parseLong(num.substring(i, j));
                if (dfs(a, b, num.substring(j))) {
                    return true;
                }
            }
        }
        return false;
    }

    private boolean dfs(long a, long b, String num) {
        if ("".equals(num)) {
            return true;
        }
        if (a + b > 0 && num.charAt(0) == '0') {
            return false;
        }
        for (int i = 1; i < Math.min(num.length() + 1, 19); ++i) {
            if (a + b == Long.parseLong(num.substring(0, i))) {
                if (dfs(b, a + b, num.substring(i))) {
                    return true;
                }
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isAdditiveNumber(string num) {
        int n = num.size();
        for (int i = 1; i < min(n - 1, 19); ++i) {
            for (int j = i + 1; j < min(n, i + 19); ++j) {
                if (i > 1 && num[0] == '0') break;
                if (j - i > 1 && num[i] == '0') continue;
                auto a = stoll(num.substr(0, i));
                auto b = stoll(num.substr(i, j - i));
                if (dfs(a, b, num.substr(j, n - j))) return true;
            }
        }
        return false;
    }

    bool dfs(long long a, long long b, string num) {
        if (num == "") return true;
        if (a + b > 0 && num[0] == '0') return false;
        for (int i = 1; i < min((int) num.size() + 1, 19); ++i)
            if (a + b == stoll(num.substr(0, i)))
                if (dfs(b, a + b, num.substr(i, num.size() - i)))
                    return true;
        return false;
    }
};
```

#### Go

```go
func isAdditiveNumber(num string) bool {
	n := len(num)
	var dfs func(a, b int64, num string) bool
	dfs = func(a, b int64, num string) bool {
		if num == "" {
			return true
		}
		if a+b > 0 && num[0] == '0' {
			return false
		}
		for i := 1; i < min(len(num)+1, 19); i++ {
			c, _ := strconv.ParseInt(num[:i], 10, 64)
			if a+b == c {
				if dfs(b, c, num[i:]) {
					return true
				}
			}
		}
		return false
	}
	for i := 1; i < min(n-1, 19); i++ {
		for j := i + 1; j < min(n, i+19); j++ {
			if i > 1 && num[0] == '0' {
				break
			}
			if j-i > 1 && num[i] == '0' {
				continue
			}
			a, _ := strconv.ParseInt(num[:i], 10, 64)
			b, _ := strconv.ParseInt(num[i:j], 10, 64)
			if dfs(a, b, num[j:]) {
				return true
			}
		}
	}
	return false
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
