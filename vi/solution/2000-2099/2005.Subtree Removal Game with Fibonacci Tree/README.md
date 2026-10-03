---
comments: true
difficulty: Hard
tags:
    - Tree
    - Math
    - Dynamic Programming
    - Binary Tree
    - Game Theory
    - 'Sprague–Grundy '
---

<!-- problem:start -->

# [2005. Subtree Removal Game with Fibonacci Tree 🔒](https://leetcode.com/problems/subtree-removal-game-with-fibonacci-tree)

[中文文档](/solution/2000-2099/2005.Subtree%20Removal%20Game%20with%20Fibonacci%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cây <strong>Fibonacci</strong> là một cây nhị phân được tạo bằng hàm <code>order(n)</code>:</p>

<ul>
	<li><code>order(0)</code> là cây rỗng.</li>
	<li><code>order(1)</code> là cây nhị phân chỉ có <strong>một node</strong>.</li>
	<li><code>order(n)</code> là cây nhị phân gồm một node gốc, với cây con trái là <code>order(n - 2)</code> và cây con phải là <code>order(n - 1)</code>.</li>
</ul>

<p>Alice và Bob chơi một trò chơi trên cây <strong>Fibonacci</strong>, trong đó Alice đi trước. Ở mỗi lượt, một người chơi chọn một node rồi xóa node đó <strong>và</strong> cây con của nó. Người chơi bị buộc phải xóa <code>root</code> sẽ thua.</p>

<p>Với số nguyên <code>n</code>, hãy trả về <code>true</code> nếu Alice thắng hoặc <code>false</code> nếu Bob thắng, giả sử cả hai người chơi đều chơi tối ưu.</p>

<p>Cây con của một cây nhị phân <code>tree</code> là cây gồm một node trong <code>tree</code> và tất cả các hậu duệ của node đó. Bản thân cây <code>tree</code> cũng được xem là một cây con của chính nó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong><br />
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2005.Subtree%20Removal%20Game%20with%20Fibonacci%20Tree/images/image-20210914173520-3.png" style="width: 200px; height: 184px;" /></p>

<pre>
<strong>Đầu vào:</strong> n = 3
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong>
Alice chọn node 1 trong cây con phải.
Bob chọn node 1 trong cây con trái hoặc node 2 trong cây con phải.
Alice chọn node còn lại mà Bob không chọn.
Bob buộc phải chọn node gốc 3, nên Bob sẽ thua.
Trả về true vì Alice thắng.
</pre>

<p><strong class="example">Ví dụ 2:</strong><br />
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2005.Subtree%20Removal%20Game%20with%20Fibonacci%20Tree/images/image-20210914173634-4.png" style="width: 75px; height: 75px;" /></p>

<pre>
<strong>Đầu vào:</strong> n = 1
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong>
Alice buộc phải chọn node gốc 1, nên Alice sẽ thua.
Trả về false vì Alice thua.
</pre>

<p><strong class="example">Ví dụ 3:</strong><br />
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2005.Subtree%20Removal%20Game%20with%20Fibonacci%20Tree/images/image-20210914173425-1.png" style="width: 100px; height: 106px;" /></p>

<pre>
<strong>Đầu vào:</strong> n = 2
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong>
Alice chọn node 1.
Bob buộc phải chọn node gốc 2, nên Bob sẽ thua.
Trả về true vì Alice thắng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Cây Fibonacci được xây dựng từ $order(n-2)$ và $order(n-1)$; số lượng node tăng theo cấp số nhân, nên không thể dựng toàn bộ cây. Với $n \le 100$, ta chỉ cần kiểm tra người chơi đầu tiên có thắng hay không.
>
> Việc xóa một node cùng cây con của nó tạo thành một subtraction game trên các cây con độc lập. Các số Grundy của hai cây con quyết định root có phải là một thế $N$ hay không.
>
> Hãy đệ quy trên $order(n)$: cây rỗng và cây chỉ có một node là các thế thua; với $n \ge 2$, phép xor (hoặc quy tắc parity tương đương) của các cây con cho biết người chiến thắng. Các tab code đều trống, nên phần trình bày tập trung vào phép rút gọn theo lý thuyết trò chơi này.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python

```

#### Java

```java

```

#### C++

```cpp

```

#### Go

```go

```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
