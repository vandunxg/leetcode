---
comments: true
difficulty: Medium
rating: 1361
source: Weekly Contest 255 Q2
tags:
    - Array
    - Hash Table
    - String
    - Backtracking
---

<!-- problem:start -->

# [1980. Find Unique Binary String](https://leetcode.com/problems/find-unique-binary-string)

[中文文档](/solution/1900-1999/1980.Find%20Unique%20Binary%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng các chuỗi <code>nums</code> gồm <code>n</code> chuỗi nhị phân <strong>khác nhau</strong>, mỗi chuỗi có độ dài <code>n</code>, hãy trả về <em>một chuỗi nhị phân có độ dài </em><code>n</code><em> và <strong>không xuất hiện</strong> trong </em><code>nums</code><em>. Nếu có nhiều đáp án, bạn có thể trả về <strong>bất kỳ</strong> đáp án nào</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [&quot;01&quot;,&quot;10&quot;]
<strong>Đầu ra:</strong> &quot;11&quot;
<strong>Giải thích:</strong> &quot;11&quot; không xuất hiện trong nums. &quot;00&quot; cũng là một đáp án đúng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [&quot;00&quot;,&quot;01&quot;]
<strong>Đầu ra:</strong> &quot;11&quot;
<strong>Giải thích:</strong> &quot;11&quot; không xuất hiện trong nums. &quot;10&quot; cũng là một đáp án đúng.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [&quot;111&quot;,&quot;011&quot;,&quot;001&quot;]
<strong>Đầu ra:</strong> &quot;101&quot;
<strong>Giải thích:</strong> &quot;101&quot; không xuất hiện trong nums. &quot;000&quot;, &quot;010&quot;, &quot;100&quot; và &quot;110&quot; cũng là các đáp án đúng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>1 &lt;= n &lt;= 16</code></li>
	<li><code>nums[i].length == n</code></li>
	<li><code>nums[i] </code>là <code>&#39;0&#39;</code> hoặc <code>&#39;1&#39;</code>.</li>
	<li>Tất cả các chuỗi trong <code>nums</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Có $n$ chuỗi có độ dài $n$, nhưng có $2^n$ chuỗi có thể tạo ra. Trong $n+1$ trọng số Hamming có thể có, chỉ có $n$ trọng số xuất hiện, nên sẽ có một trọng số bị thiếu.
>
> Một bit mask ghi nhận các số lượng số 1 đã xuất hiện; ta trả về chuỗi gồm số lượng đó các số 1, được bổ sung bằng các số 0.

<!-- thinking:end -->

Vì số lượng ký tự `'1'` trong một chuỗi nhị phân có độ dài $n$ có thể là $0, 1, 2, \cdots, n$ (tổng cộng có $n + 1$ khả năng), ta luôn có thể tìm được một chuỗi nhị phân mới có số lượng ký tự `'1'` khác với mọi chuỗi trong $\textit{nums}$.

Ta dùng một số nguyên $\textit{mask}$ để ghi nhận số lượng ký tự `'1'` trong tất cả các chuỗi, trong đó bit thứ $i$ của $\textit{mask}$ bằng $1$ cho biết có một chuỗi nhị phân độ dài $n$ chứa đúng $i$ ký tự `'1'` xuất hiện trong $\textit{nums}$, còn bằng $0$ nếu không có.

Sau đó, ta lần lượt xét $i$ bắt đầu từ $0$, biểu diễn số lượng ký tự `'1'` trong một chuỗi nhị phân có độ dài $n$. Nếu bit thứ $i$ của $\textit{mask}$ bằng $0$, điều đó có nghĩa là không có chuỗi nhị phân độ dài $n$ nào chứa đúng $i$ ký tự `'1'`, và ta có thể trả về chuỗi đó làm đáp án.

Độ phức tạp thời gian là $O(L)$, trong đó $L$ là tổng độ dài của tất cả các chuỗi trong $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findDifferentBinaryString(self, nums: List[str]) -> str:
        mask = 0
        for x in nums:
            mask |= 1 << x.count("1")
        for i in count(0):
            if mask >> i & 1 ^ 1:
                return "1" * i + "0" * (len(nums) - i)
```

#### Java

```java
class Solution {
    public String findDifferentBinaryString(String[] nums) {
        int mask = 0;
        for (var x : nums) {
            int cnt = 0;
            for (int i = 0; i < x.length(); ++i) {
                if (x.charAt(i) == '1') {
                    ++cnt;
                }
            }
            mask |= 1 << cnt;
        }
        for (int i = 0;; ++i) {
            if ((mask >> i & 1) == 0) {
                return "1".repeat(i) + "0".repeat(nums.length - i);
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    string findDifferentBinaryString(vector<string>& nums) {
        int mask = 0;
        for (auto& x : nums) {
            int cnt = count(x.begin(), x.end(), '1');
            mask |= 1 << cnt;
        }
        for (int i = 0;; ++i) {
            if (mask >> i & 1 ^ 1) {
                return string(i, '1') + string(nums.size() - i, '0');
            }
        }
    }
};
```

#### Go

```go
func findDifferentBinaryString(nums []string) string {
	mask := 0
	for _, x := range nums {
		mask |= 1 << strings.Count(x, "1")
	}
	for i := 0; ; i++ {
		if mask>>i&1 == 0 {
			return strings.Repeat("1", i) + strings.Repeat("0", len(nums)-i)
		}
	}
}
```

#### TypeScript

```ts
function findDifferentBinaryString(nums: string[]): string {
    let mask = 0;
    for (let x of nums) {
        const cnt = x.split('').filter(c => c === '1').length;
        mask |= 1 << cnt;
    }
    for (let i = 0; ; ++i) {
        if (((mask >> i) & 1) === 0) {
            return '1'.repeat(i) + '0'.repeat(nums.length - i);
        }
    }
}
```

#### JavaScript

```js
/**
 * @param {string[]} nums
 * @return {string}
 */
var findDifferentBinaryString = function (nums) {
    let mask = 0;
    for (let x of nums) {
        const cnt = x.split('').filter(c => c === '1').length;
        mask |= 1 << cnt;
    }
    for (let i = 0; ; ++i) {
        if (((mask >> i) & 1) === 0) {
            return '1'.repeat(i) + '0'.repeat(nums.length - i);
        }
    }
};
```

#### C#

```cs
public class Solution {
    public string FindDifferentBinaryString(string[] nums) {
        int mask = 0;
        foreach (var x in nums) {
            int cnt = x.Count(c => c == '1');
            mask |= 1 << cnt;
        }
        int i = 0;
        while ((mask >> i & 1) == 1) {
            i++;
        }
        return string.Format("{0}{1}", new string('1', i), new string('0', nums.Length - i));
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Xây dựng

<!-- thinking:start -->

> **Tư duy**
>
> Việc đếm các bit phải duyệt qua mọi ký tự. Phép đường chéo Cantor lật $\textit{nums}[i][i]$ để kết quả khác mỗi chuỗi đầu vào ở ít nhất một vị trí.

<!-- thinking:end -->

Ta có thể xây dựng một chuỗi nhị phân $\textit{ans}$ có độ dài $n$, trong đó bit thứ $i$ của $\textit{ans}$ khác bit thứ $i$ của $\textit{nums}[i]$. Vì tất cả các chuỗi trong $\textit{nums}$ đều khác nhau, $\textit{ans}$ sẽ không xuất hiện trong $\textit{nums}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của các chuỗi trong $\textit{nums}$. Không tính phần không gian dùng cho chuỗi kết quả, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findDifferentBinaryString(self, nums: List[str]) -> str:
        ans = [None] * len(nums)
        for i, s in enumerate(nums):
            ans[i] = "1" if s[i] == "0" else "0"
        return "".join(ans)
```

#### Java

```java
class Solution {
    public String findDifferentBinaryString(String[] nums) {
        int n = nums.length;
        char[] ans = new char[n];
        for (int i = 0; i < n; i++) {
            ans[i] = nums[i].charAt(i) == '0' ? '1' : '0';
        }
        return new String(ans);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string findDifferentBinaryString(vector<string>& nums) {
        int n = nums.size();
        string ans(n, '0');
        for (int i = 0; i < n; i++) {
            ans[i] = nums[i][i] == '0' ? '1' : '0';
        }
        return ans;
    }
};
```

#### Go

```go
func findDifferentBinaryString(nums []string) string {
	ans := make([]byte, len(nums))
	for i, s := range nums {
		if s[i] == '0' {
			ans[i] = '1'
		} else {
			ans[i] = '0'
		}
	}
	return string(ans)
}
```

#### TypeScript

```ts
function findDifferentBinaryString(nums: string[]): string {
    const n = nums.length;
    const ans: string[] = new Array(n);
    for (let i = 0; i < n; i++) {
        ans[i] = nums[i][i] === '0' ? '1' : '0';
    }
    return ans.join('');
}
```

#### JavaScript

```js
/**
 * @param {string[]} nums
 * @return {string}
 */
var findDifferentBinaryString = function (nums) {
    const n = nums.length;
    const ans = new Array(n);
    for (let i = 0; i < n; i++) {
        ans[i] = nums[i][i] === '0' ? '1' : '0';
    }
    return ans.join('');
};
```

#### C#

```cs
public class Solution {
    public string FindDifferentBinaryString(string[] nums) {
        int n = nums.Length;
        char[] ans = new char[n];
        for (int i = 0; i < n; i++) {
            ans[i] = nums[i][i] == '0' ? '1' : '0';
        }
        return new string(ans);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
