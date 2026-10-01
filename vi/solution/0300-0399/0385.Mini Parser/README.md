---
comments: true
difficulty: Medium
tags:
    - Stack
    - Depth-First Search
    - String
---

<!-- problem:start -->

# [385. Mini Parser](https://leetcode.com/problems/mini-parser)

[中文文档](/solution/0300-0399/0385.Mini%20Parser/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi s biểu diễn dạng tuần tự hóa của một danh sách lồng nhau. Hãy cài đặt parser để giải tuần tự hóa chuỗi và trả về <code>NestedInteger</code> tương ứng.</p>

<p>Mỗi phần tử là một số nguyên hoặc một danh sách, trong đó các phần tử cũng có thể là số nguyên hoặc danh sách khác.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;324&quot;
<strong>Đầu ra:</strong> 324
<strong>Giải thích:</strong> Hãy trả về một đối tượng NestedInteger chứa số nguyên duy nhất 324.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;[123,[456,[789]]]&quot;
<strong>Đầu ra:</strong> [123,[456,[789]]]
<strong>Giải thích:</strong> Trả về một đối tượng NestedInteger chứa danh sách lồng nhau gồm 2 phần tử:
1. Một số nguyên có giá trị 123.
2. Một danh sách lồng nhau gồm hai phần tử:
    i. Một số nguyên có giá trị 456.
    ii. Một danh sách lồng nhau có một phần tử:
         a. Một số nguyên có giá trị 789
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>s</code> chỉ gồm chữ số, dấu ngoặc vuông <code>&quot;[]&quot;</code>, dấu âm <code>&#39;-&#39;</code> và dấu phẩy <code>&#39;,&#39;</code>.</li>
	<li><code>s</code> là dạng tuần tự hóa của một <code>NestedInteger</code> hợp lệ.</li>
	<li>Tất cả giá trị đầu vào nằm trong khoảng <code>[-10<sup>6</sup>, 10<sup>6</sup>]</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Phân tích chuỗi danh sách lồng nhau thành `NestedInteger`. Cú pháp gồm một số nguyên hoặc `[elem,…,elem]`, phù hợp với cách phân tích đệ quy từ trên xuống.
>
> Chuỗi rỗng hoặc `[]` biểu diễn danh sách rỗng; nếu ký tự đầu không phải `[` thì chuỗi biểu diễn một số nguyên. Trường hợp còn lại, tách tại dấu phẩy ở độ sâu $0$ (hoặc cuối chuỗi), gọi đệ quy rồi `add` kết quả. Theo dõi độ sâu ngoặc để chỉ tách ở cấp cao nhất.

<!-- thinking:end -->

Trước tiên, kiểm tra xem chuỗi $s$ có rỗng hoặc là danh sách rỗng hay không. Nếu có, trả về một `NestedInteger` rỗng. Nếu $s$ là số nguyên, trả về `NestedInteger` chứa số đó. Nếu không, duyệt chuỗi $s$ từ trái sang phải. Khi độ sâu hiện tại bằng $0$ và gặp dấu phẩy hoặc đến cuối chuỗi $s$, lấy một chuỗi con, gọi đệ quy để phân tích chuỗi con rồi thêm giá trị trả về vào danh sách. Khi gặp ngoặc vuông mở, tăng độ sâu lên $1$; khi gặp ngoặc vuông đóng, giảm độ sâu xuống $1$ rồi tiếp tục duyệt.

Sau khi duyệt xong, trả về kết quả.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
# """
# This is the interface that allows for creating nested lists.
# You should not implement it, or speculate about its implementation
# """
# class NestedInteger:
#    def __init__(self, value=None):
#        """
#        If value is not specified, initializes an empty list.
#        Otherwise initializes a single integer equal to value.
#        """
#
#    def isInteger(self):
#        """
#        @return True if this NestedInteger holds a single integer, rather than a nested list.
#        :rtype bool
#        """
#
#    def add(self, elem):
#        """
#        Set this NestedInteger to hold a nested list and adds a nested integer elem to it.
#        :rtype void
#        """
#
#    def setInteger(self, value):
#        """
#        Set this NestedInteger to hold a single integer equal to value.
#        :rtype void
#        """
#
#    def getInteger(self):
#        """
#        @return the single integer that this NestedInteger holds, if it holds a single integer
#        Return None if this NestedInteger holds a nested list
#        :rtype int
#        """
#
#    def getList(self):
#        """
#        @return the nested list that this NestedInteger holds, if it holds a nested list
#        Return None if this NestedInteger holds a single integer
#        :rtype List[NestedInteger]
#        """
class Solution:
    def deserialize(self, s: str) -> NestedInteger:
        if not s or s == '[]':
            return NestedInteger()
        if s[0] != '[':
            return NestedInteger(int(s))
        ans = NestedInteger()
        depth, j = 0, 1
        for i in range(1, len(s)):
            if depth == 0 and (s[i] == ',' or i == len(s) - 1):
                ans.add(self.deserialize(s[j:i]))
                j = i + 1
            elif s[i] == '[':
                depth += 1
            elif s[i] == ']':
                depth -= 1
        return ans
```

#### Java

```java
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * public interface NestedInteger {
 *     // Constructor initializes an empty nested list.
 *     public NestedInteger();
 *
 *     // Constructor initializes a single integer.
 *     public NestedInteger(int value);
 *
 *     // @return true if this NestedInteger holds a single integer, rather than a nested list.
 *     public boolean isInteger();
 *
 *     // @return the single integer that this NestedInteger holds, if it holds a single integer
 *     // Return null if this NestedInteger holds a nested list
 *     public Integer getInteger();
 *
 *     // Set this NestedInteger to hold a single integer.
 *     public void setInteger(int value);
 *
 *     // Set this NestedInteger to hold a nested list and adds a nested integer to it.
 *     public void add(NestedInteger ni);
 *
 *     // @return the nested list that this NestedInteger holds, if it holds a nested list
 *     // Return empty list if this NestedInteger holds a single integer
 *     public List<NestedInteger> getList();
 * }
 */
class Solution {
    public NestedInteger deserialize(String s) {
        if ("".equals(s) || "[]".equals(s)) {
            return new NestedInteger();
        }
        if (s.charAt(0) != '[') {
            return new NestedInteger(Integer.parseInt(s));
        }
        NestedInteger ans = new NestedInteger();
        int depth = 0;
        for (int i = 1, j = 1; i < s.length(); ++i) {
            if (depth == 0 && (s.charAt(i) == ',' || i == s.length() - 1)) {
                ans.add(deserialize(s.substring(j, i)));
                j = i + 1;
            } else if (s.charAt(i) == '[') {
                ++depth;
            } else if (s.charAt(i) == ']') {
                --depth;
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * class NestedInteger {
 *   public:
 *     // Constructor initializes an empty nested list.
 *     NestedInteger();
 *
 *     // Constructor initializes a single integer.
 *     NestedInteger(int value);
 *
 *     // Return true if this NestedInteger holds a single integer, rather than a nested list.
 *     bool isInteger() const;
 *
 *     // Return the single integer that this NestedInteger holds, if it holds a single integer
 *     // The result is undefined if this NestedInteger holds a nested list
 *     int getInteger() const;
 *
 *     // Set this NestedInteger to hold a single integer.
 *     void setInteger(int value);
 *
 *     // Set this NestedInteger to hold a nested list and adds a nested integer to it.
 *     void add(const NestedInteger &ni);
 *
 *     // Return the nested list that this NestedInteger holds, if it holds a nested list
 *     // The result is undefined if this NestedInteger holds a single integer
 *     const vector<NestedInteger> &getList() const;
 * };
 */
class Solution {
public:
    NestedInteger deserialize(string s) {
        if (s == "" || s == "[]") {
            return NestedInteger();
        }
        if (s[0] != '[') {
            return NestedInteger(stoi(s));
        }
        NestedInteger ans;
        int depth = 0;
        for (int i = 1, j = 1; i < s.size(); ++i) {
            if (depth == 0 && (s[i] == ',' || i == s.size() - 1)) {
                ans.add(deserialize(s.substr(j, i - j)));
                j = i + 1;
            } else if (s[i] == '[') {
                ++depth;
            } else if (s[i] == ']') {
                --depth;
            }
        }
        return ans;
    }
};
```

#### Go

```go
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * type NestedInteger struct {
 * }
 *
 * // Return true if this NestedInteger holds a single integer, rather than a nested list.
 * func (n NestedInteger) IsInteger() bool {}
 *
 * // Return the single integer that this NestedInteger holds, if it holds a single integer
 * // The result is undefined if this NestedInteger holds a nested list
 * // So before calling this method, you should have a check
 * func (n NestedInteger) GetInteger() int {}
 *
 * // Set this NestedInteger to hold a single integer.
 * func (n *NestedInteger) SetInteger(value int) {}
 *
 * // Set this NestedInteger to hold a nested list and adds a nested integer to it.
 * func (n *NestedInteger) Add(elem NestedInteger) {}
 *
 * // Return the nested list that this NestedInteger holds, if it holds a nested list
 * // The list length is zero if this NestedInteger holds a single integer
 * // You can access NestedInteger's List element directly if you want to modify it
 * func (n NestedInteger) GetList() []*NestedInteger {}
 */
func deserialize(s string) *NestedInteger {
	ans := &NestedInteger{}
	if s == "" || s == "[]" {
		return ans
	}
	if s[0] != '[' {
		v, _ := strconv.Atoi(s)
		ans.SetInteger(v)
		return ans
	}
	depth := 0
	for i, j := 1, 1; i < len(s); i++ {
		if depth == 0 && (s[i] == ',' || i == len(s)-1) {
			(*ans).Add(*deserialize(s[j:i]))
			j = i + 1
		} else if s[i] == '[' {
			depth++
		} else if s[i] == ']' {
			depth--
		}
	}
	return ans
}
```

#### TypeScript

```ts
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * class NestedInteger {
 *     If value is provided, then it holds a single integer
 *     Otherwise it holds an empty nested list
 *     constructor(value?: number) {
 *         ...
 *     };
 *
 *     Return true if this NestedInteger holds a single integer, rather than a nested list.
 *     isInteger(): boolean {
 *         ...
 *     };
 *
 *     Return the single integer that this NestedInteger holds, if it holds a single integer
 *     Return null if this NestedInteger holds a nested list
 *     getInteger(): number | null {
 *         ...
 *     };
 *
 *     Set this NestedInteger to hold a single integer equal to value.
 *     setInteger(value: number) {
 *         ...
 *     };
 *
 *     Set this NestedInteger to hold a nested list and adds a nested integer elem to it.
 *     add(elem: NestedInteger) {
 *         ...
 *     };
 *
 *     Return the nested list that this NestedInteger holds,
 *     or an empty list if this NestedInteger holds a single integer
 *     getList(): NestedInteger[] {
 *         ...
 *     };
 * };
 */

function deserialize(s: string): NestedInteger {
    if (s === '' || s === '[]') {
        return new NestedInteger();
    }
    if (s[0] !== '[') {
        return new NestedInteger(+s);
    }
    const ans: NestedInteger = new NestedInteger();
    let depth = 0;
    for (let i = 1, j = 1; i < s.length; ++i) {
        if (depth === 0 && (s[i] === ',' || i === s.length - 1)) {
            ans.add(deserialize(s.slice(j, i)));
            j = i + 1;
        } else if (s[i] === '[') {
            ++depth;
        } else if (s[i] === ']') {
            --depth;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Stack

<!-- thinking:start -->

> **Tư duy**
>
> Đệ quy sử dụng call stack. Có thể dùng stack tường minh: `[` đẩy một danh sách mới vào stack, chữ số được tích lũy, còn `,`/`]` sẽ thêm số nguyên hoặc danh sách con vào phần tử trên cùng. Khi gặp `]` và độ sâu $>1$, pop danh sách con rồi thêm vào danh sách cha. Cách này không dùng đệ quy.

<!-- thinking:end -->

Ta có thể dùng stack để mô phỏng quá trình đệ quy.

Trước tiên, kiểm tra xem chuỗi $s$ có biểu diễn số nguyên hay không. Nếu có, trả về `NestedInteger` chứa số nguyên đó. Nếu không, duyệt chuỗi $s$ từ trái sang phải. Với ký tự $c$ đang xét:

- Nếu $c$ là dấu âm, đặt cờ dấu âm thành `true`;
- Nếu $c$ là chữ số, ghép chữ số đó vào số hiện tại $x$, ban đầu $x$ bằng $0$;
- Nếu $c$ là ngoặc vuông mở, push một `NestedInteger` mới vào stack;
- Nếu $c$ là ngoặc vuông đóng hoặc dấu phẩy, kiểm tra xem ký tự ngay trước đó có phải chữ số không. Nếu có, thêm số hiện tại $x$ vào `NestedInteger` trên cùng của stack, áp dụng dấu âm nếu cần; sau đó đặt lại cờ dấu âm thành `false` và $x$ thành $0$. Nếu $c$ là ngoặc vuông đóng và stack hiện có nhiều hơn một phần tử, pop `NestedInteger` trên cùng rồi thêm nó vào `NestedInteger` kế tiếp trên stack.

Sau khi duyệt xong, trả về `NestedInteger` trên cùng của stack.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
# """
# This is the interface that allows for creating nested lists.
# You should not implement it, or speculate about its implementation
# """
# class NestedInteger:
#    def __init__(self, value=None):
#        """
#        If value is not specified, initializes an empty list.
#        Otherwise initializes a single integer equal to value.
#        """
#
#    def isInteger(self):
#        """
#        @return True if this NestedInteger holds a single integer, rather than a nested list.
#        :rtype bool
#        """
#
#    def add(self, elem):
#        """
#        Set this NestedInteger to hold a nested list and adds a nested integer elem to it.
#        :rtype void
#        """
#
#    def setInteger(self, value):
#        """
#        Set this NestedInteger to hold a single integer equal to value.
#        :rtype void
#        """
#
#    def getInteger(self):
#        """
#        @return the single integer that this NestedInteger holds, if it holds a single integer
#        Return None if this NestedInteger holds a nested list
#        :rtype int
#        """
#
#    def getList(self):
#        """
#        @return the nested list that this NestedInteger holds, if it holds a nested list
#        Return None if this NestedInteger holds a single integer
#        :rtype List[NestedInteger]
#        """
class Solution:
    def deserialize(self, s: str) -> NestedInteger:
        if s[0] != '[':
            return NestedInteger(int(s))
        stk, x, neg = [], 0, False
        for i, c in enumerate(s):
            if c == '-':
                neg = True
            elif c.isdigit():
                x = x * 10 + int(c)
            elif c == '[':
                stk.append(NestedInteger())
            elif c in ',]':
                if s[i - 1].isdigit():
                    if neg:
                        x = -x
                    stk[-1].add(NestedInteger(x))
                x, neg = 0, False
                if c == ']' and len(stk) > 1:
                    t = stk.pop()
                    stk[-1].add(t)
        return stk.pop()
```

#### Java

```java
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * public interface NestedInteger {
 *     // Constructor initializes an empty nested list.
 *     public NestedInteger();
 *
 *     // Constructor initializes a single integer.
 *     public NestedInteger(int value);
 *
 *     // @return true if this NestedInteger holds a single integer, rather than a nested list.
 *     public boolean isInteger();
 *
 *     // @return the single integer that this NestedInteger holds, if it holds a single integer
 *     // Return null if this NestedInteger holds a nested list
 *     public Integer getInteger();
 *
 *     // Set this NestedInteger to hold a single integer.
 *     public void setInteger(int value);
 *
 *     // Set this NestedInteger to hold a nested list and adds a nested integer to it.
 *     public void add(NestedInteger ni);
 *
 *     // @return the nested list that this NestedInteger holds, if it holds a nested list
 *     // Return empty list if this NestedInteger holds a single integer
 *     public List<NestedInteger> getList();
 * }
 */
class Solution {
    public NestedInteger deserialize(String s) {
        if (s.charAt(0) != '[') {
            return new NestedInteger(Integer.parseInt(s));
        }
        Deque<NestedInteger> stk = new ArrayDeque<>();
        int x = 0;
        boolean neg = false;
        for (int i = 0; i < s.length(); ++i) {
            char c = s.charAt(i);
            if (c == '-') {
                neg = true;
            } else if (Character.isDigit(c)) {
                x = x * 10 + c - '0';
            } else if (c == '[') {
                stk.push(new NestedInteger());
            } else if (c == ',' || c == ']') {
                if (Character.isDigit(s.charAt(i - 1))) {
                    if (neg) {
                        x = -x;
                    }
                    stk.peek().add(new NestedInteger(x));
                }
                x = 0;
                neg = false;
                if (c == ']' && stk.size() > 1) {
                    NestedInteger t = stk.pop();
                    stk.peek().add(t);
                }
            }
        }
        return stk.peek();
    }
}
```

#### C++

```cpp
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * class NestedInteger {
 *   public:
 *     // Constructor initializes an empty nested list.
 *     NestedInteger();
 *
 *     // Constructor initializes a single integer.
 *     NestedInteger(int value);
 *
 *     // Return true if this NestedInteger holds a single integer, rather than a nested list.
 *     bool isInteger() const;
 *
 *     // Return the single integer that this NestedInteger holds, if it holds a single integer
 *     // The result is undefined if this NestedInteger holds a nested list
 *     int getInteger() const;
 *
 *     // Set this NestedInteger to hold a single integer.
 *     void setInteger(int value);
 *
 *     // Set this NestedInteger to hold a nested list and adds a nested integer to it.
 *     void add(const NestedInteger &ni);
 *
 *     // Return the nested list that this NestedInteger holds, if it holds a nested list
 *     // The result is undefined if this NestedInteger holds a single integer
 *     const vector<NestedInteger> &getList() const;
 * };
 */
class Solution {
public:
    NestedInteger deserialize(string s) {
        if (s[0] != '[') {
            return NestedInteger(stoi(s));
        }
        stack<NestedInteger> stk;
        int x = 0;
        bool neg = false;
        for (int i = 0; i < s.size(); ++i) {
            if (s[i] == '-') {
                neg = true;
            } else if (isdigit(s[i])) {
                x = x * 10 + s[i] - '0';
            } else if (s[i] == '[') {
                stk.push(NestedInteger());
            } else if (s[i] == ',' || s[i] == ']') {
                if (isdigit(s[i - 1])) {
                    if (neg) {
                        x = -x;
                    }
                    stk.top().add(NestedInteger(x));
                }
                x = 0;
                neg = false;
                if (s[i] == ']' && stk.size() > 1) {
                    auto t = stk.top();
                    stk.pop();
                    stk.top().add(t);
                }
            }
        }
        return stk.top();
    }
};
```

#### Go

```go
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * type NestedInteger struct {
 * }
 *
 * // Return true if this NestedInteger holds a single integer, rather than a nested list.
 * func (n NestedInteger) IsInteger() bool {}
 *
 * // Return the single integer that this NestedInteger holds, if it holds a single integer
 * // The result is undefined if this NestedInteger holds a nested list
 * // So before calling this method, you should have a check
 * func (n NestedInteger) GetInteger() int {}
 *
 * // Set this NestedInteger to hold a single integer.
 * func (n *NestedInteger) SetInteger(value int) {}
 *
 * // Set this NestedInteger to hold a nested list and adds a nested integer to it.
 * func (n *NestedInteger) Add(elem NestedInteger) {}
 *
 * // Return the nested list that this NestedInteger holds, if it holds a nested list
 * // The list length is zero if this NestedInteger holds a single integer
 * // You can access NestedInteger's List element directly if you want to modify it
 * func (n NestedInteger) GetList() []*NestedInteger {}
 */
func deserialize(s string) *NestedInteger {
	if s[0] != '[' {
		v, _ := strconv.Atoi(s)
		ans := NestedInteger{}
		ans.SetInteger(v)
		return &ans
	}
	stk := []*NestedInteger{}
	x := 0
	neg := false
	for i, c := range s {
		if c == '-' {
			neg = true
		} else if c >= '0' && c <= '9' {
			x = x*10 + int(c-'0')
		} else if c == '[' {
			stk = append(stk, &NestedInteger{})
		} else if c == ',' || c == ']' {
			if s[i-1] >= '0' && s[i-1] <= '9' {
				if neg {
					x = -x
				}
				t := NestedInteger{}
				t.SetInteger(x)
				stk[len(stk)-1].Add(t)
			}
			x = 0
			neg = false
			if c == ']' && len(stk) > 1 {
				t := stk[len(stk)-1]
				stk = stk[:len(stk)-1]
				stk[len(stk)-1].Add(*t)
			}
		}
	}
	return stk[0]
}
```

#### TypeScript

```ts
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * class NestedInteger {
 *     If value is provided, then it holds a single integer
 *     Otherwise it holds an empty nested list
 *     constructor(value?: number) {
 *         ...
 *     };
 *
 *     Return true if this NestedInteger holds a single integer, rather than a nested list.
 *     isInteger(): boolean {
 *         ...
 *     };
 *
 *     Return the single integer that this NestedInteger holds, if it holds a single integer
 *     Return null if this NestedInteger holds a nested list
 *     getInteger(): number | null {
 *         ...
 *     };
 *
 *     Set this NestedInteger to hold a single integer equal to value.
 *     setInteger(value: number) {
 *         ...
 *     };
 *
 *     Set this NestedInteger to hold a nested list and adds a nested integer elem to it.
 *     add(elem: NestedInteger) {
 *         ...
 *     };
 *
 *     Return the nested list that this NestedInteger holds,
 *     or an empty list if this NestedInteger holds a single integer
 *     getList(): NestedInteger[] {
 *         ...
 *     };
 * };
 */

function deserialize(s: string): NestedInteger {
    if (s[0] !== '[') {
        return new NestedInteger(+s);
    }
    const stk: NestedInteger[] = [];
    let x = 0;
    let neg = false;
    for (let i = 0; i < s.length; ++i) {
        if (s[i] === '-') {
            neg = true;
        } else if (s[i] === '[') {
            stk.push(new NestedInteger());
        } else if (s[i] >= '0' && s[i] <= '9') {
            x = x * 10 + s[i].charCodeAt(0) - '0'.charCodeAt(0);
        } else if (s[i] === ',' || s[i] === ']') {
            if (s[i - 1] >= '0' && s[i - 1] <= '9') {
                stk[stk.length - 1].add(new NestedInteger(neg ? -x : x));
            }
            x = 0;
            neg = false;
            if (s[i] === ']' && stk.length > 1) {
                const t = stk.pop()!;
                stk[stk.length - 1].add(t);
            }
        }
    }
    return stk[0];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
