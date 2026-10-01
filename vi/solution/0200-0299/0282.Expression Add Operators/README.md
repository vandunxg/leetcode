---
comments: true
difficulty: Hard
tags:
    - Math
    - String
    - Backtracking
---

<!-- problem:start -->

# [282. Expression Add Operators](https://leetcode.com/problems/expression-add-operators)

[中文文档](/solution/0200-0299/0282.Expression%20Add%20Operators/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>num</code> chỉ gồm các chữ số và số nguyên <code>target</code>, hãy trả về <em><strong>mọi cách</strong> chèn các toán tử nhị phân </em><code>&#39;+&#39;</code><em>, </em><code>&#39;-&#39;</code><em> và/hoặc </em><code>&#39;*&#39;</code><em> vào giữa các chữ số của </em><code>num</code><em> sao cho biểu thức thu được có giá trị bằng <code>target</code>.</p>

<p>Lưu ý: các toán hạng trong biểu thức trả về <strong>không được</strong> có số 0 ở đầu.</p>

<p><strong>Lưu ý</strong> rằng một số có thể gồm nhiều chữ số.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;123&quot;, target = 6
<strong>Đầu ra:</strong> [&quot;1*2*3&quot;,&quot;1+2+3&quot;]
<strong>Giải thích:</strong> Cả &quot;1*2*3&quot; và &quot;1+2+3&quot; đều có giá trị bằng 6.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;232&quot;, target = 8
<strong>Đầu ra:</strong> [&quot;2*3+2&quot;,&quot;2+3*2&quot;]
<strong>Giải thích:</strong> Cả &quot;2*3+2&quot; và &quot;2+3*2&quot; đều có giá trị bằng 8.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;3456237490&quot;, target = 9191
<strong>Đầu ra:</strong> []
<strong>Giải thích:</strong> Không thể tạo biểu thức nào từ &quot;3456237490&quot; có giá trị bằng 9191.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= num.length &lt;= 10</code></li>
	<li><code>num</code> chỉ gồm các chữ số.</li>
	<li><code>-2<sup>31</sup> &lt;= target &lt;= 2<sup>31</sup> - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta có thể chèn $+$, $-$, $*$ hoặc ghép các chữ số liền nhau thành một số. Phép nhân có độ ưu tiên cao hơn phép cộng, và các số được ghép không được có số 0 ở đầu.
>
> DFS lưu toán hạng cuối cùng $prev$ và giá trị hiện tại $curr$. Với phép cộng và trừ, ta cập nhật trực tiếp $curr$; với phép nhân, ta điều chỉnh lại hạng tử cuối bằng $curr-prev+prev\times\textit{next}$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def addOperators(self, num: str, target: int) -> List[str]:
        ans = []

        def dfs(u, prev, curr, path):
            if u == len(num):
                if curr == target:
                    ans.append(path)
                return
            for i in range(u, len(num)):
                if i != u and num[u] == '0':
                    break
                next = int(num[u : i + 1])
                if u == 0:
                    dfs(i + 1, next, next, path + str(next))
                else:
                    dfs(i + 1, next, curr + next, path + "+" + str(next))
                    dfs(i + 1, -next, curr - next, path + "-" + str(next))
                    dfs(
                        i + 1,
                        prev * next,
                        curr - prev + prev * next,
                        path + "*" + str(next),
                    )

        dfs(0, 0, 0, "")
        return ans
```

#### Java

```java
class Solution {
    private List<String> ans;
    private String num;
    private int target;

    public List<String> addOperators(String num, int target) {
        ans = new ArrayList<>();
        this.num = num;
        this.target = target;
        dfs(0, 0, 0, "");
        return ans;
    }

    private void dfs(int u, long prev, long curr, String path) {
        if (u == num.length()) {
            if (curr == target) ans.add(path);
            return;
        }
        for (int i = u; i < num.length(); i++) {
            if (i != u && num.charAt(u) == '0') {
                break;
            }
            long next = Long.parseLong(num.substring(u, i + 1));
            if (u == 0) {
                dfs(i + 1, next, next, path + next);
            } else {
                dfs(i + 1, next, curr + next, path + "+" + next);
                dfs(i + 1, -next, curr - next, path + "-" + next);
                dfs(i + 1, prev * next, curr - prev + prev * next, path + "*" + next);
            }
        }
    }
}
```

#### C#

```cs
public class Expression {
    public long Value;

    public override string ToString() {
        return Value.ToString();
    }
}

public class BinaryExpression : Expression {
    public char Operator;

    public Expression LeftChild;
    public Expression RightChild;

    public override string ToString() {
        return string.Format("{0}{1}{2}", LeftChild, Operator, RightChild);
    }
}

public class Solution {
    public IList<string> AddOperators(string num, int target) {
        var results = new List<string>();
        if (string.IsNullOrEmpty(num)) return results;
        this.num = num;
        this.results = new List<Expression>[num.Length, num.Length, 3];
        foreach (var ex in Search(0, num.Length - 1, 0)) {
            if (ex.Value == target) {
                results.Add(ex.ToString());
            }
        }
        return results;
    }

    private string num;
    private List<Expression>[,,] results;

    private List<Expression> Search(int left, int right, int level) {
        if (results[left, right, level] != null) {
            return results[left, right, level];
        }
        var result = new List<Expression>();
        if (level < 2) {
            for (var i = left + 1; i <= right; ++i) {
                List<Expression> leftResult, rightResult;
                leftResult = Search(left, i - 1, level);
                rightResult = Search(i, right, level + 1);
                foreach (var l in leftResult) {
                    foreach (var r in rightResult) {
                        var newObjects = new List<Tuple<char, long>>();
                        if (level == 0) {
                            newObjects.Add(Tuple.Create('+', l.Value + r.Value));
                            newObjects.Add(Tuple.Create('-', l.Value - r.Value));
                        }
                        else {
                            newObjects.Add(Tuple.Create('*', l.Value * r.Value));
                        }
                        foreach (var newObject in newObjects) {
                            result.Add(new BinaryExpression {
                                Value = newObject.Item2,
                                Operator = newObject.Item1,
                                LeftChild = l,
                                RightChild = r
                            });
                        }
                    }
                }
            }
        }
        else {
            if (left == right || num[left] != '0') {
                long x = 0;
                for (var i = left; i <= right; ++i) {
                    x = x * 10 + num[i] - '0';
                }
                result.Add(new Expression {
                    Value = x
                });
            }
        }
        if (level < 2) {
            result.AddRange(Search(left, right, level + 1));
        }
        return results[left, right, level] = result;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
