---
comments: true
difficulty: Easy
---

<!-- problem:start -->

# [08.06. Hanota](https://leetcode.cn/problems/hanota-lcci)

[中文文档](/lcci/08.06.Hanota/README.md)

## Mô tả

<!-- description:start -->

<p>Trong bài toán Tháp Hà Nội kinh điển, bạn có 3 cọc và N đĩa có kích thước khác nhau, có thể đặt trượt lên bất kỳ cọc nào. Ban đầu, các đĩa được sắp xếp theo thứ tự tăng dần về kích thước từ trên xuống dưới (tức là mỗi đĩa nằm trên một đĩa lớn hơn). Bài toán có các ràng buộc sau:</p>
<p>(1) Mỗi lần chỉ được di chuyển một đĩa.<br />
(2) Một đĩa được lấy khỏi đỉnh của một cọc và đặt lên cọc khác.<br />
(3) Không được đặt một đĩa lên trên một đĩa nhỏ hơn.</p>
<p>Hãy viết chương trình dùng stack để chuyển các đĩa từ cọc đầu tiên sang cọc cuối cùng.</p>
<p><strong>Ví dụ 1:</strong></p>
<pre>

<strong>Đầu vào: </strong>A = [2, 1, 0], B = [], C = []

<strong>Đầu ra: </strong>C = [2, 1, 0]

</pre>
<p><strong>Ví dụ 2:</strong></p>
<pre>

<strong>Đầu vào: </strong>A = [1, 0], B = [], C = []

<strong>Đầu ra: </strong>C = [1, 0]

</pre>
<p><strong>Lưu ý:</strong></p>
<ol>
	<li><code>A.length &lt;= 14</code></li>
</ol>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Tháp Hà Nội không cho phép đặt đĩa lớn lên đĩa nhỏ. Nếu thử mọi nước đi hợp lệ, ta sẽ duyệt $3^n$ trạng thái.
>
> Phương án tối ưu là duy nhất: chuyển $n-1$ đĩa sang cọc trung gian, chuyển đĩa lớn nhất, rồi chuyển $n-1$ đĩa lên trên nó.
>
> $dfs(n,a,b,c)$ thực hiện đúng thứ tự đó; với $n=1$ chỉ cần pop/append một lần. Độ dài chuỗi là $2^n-1$.

<!-- thinking:end -->

Ta thiết kế một hàm $dfs(n, a, b, c)$, biểu diễn việc chuyển $n$ đĩa từ $a$ sang $c$, với $b$ là cọc phụ.

Trước hết, ta chuyển $n - 1$ đĩa từ $a$ sang $b$, sau đó chuyển đĩa thứ $n$ từ $a$ sang $c$, và cuối cùng chuyển $n - 1$ đĩa từ $b$ sang $c$.

Độ phức tạp thời gian là $O(2^n)$, còn độ phức tạp không gian là $O(n)$. Ở đây, $n$ là số đĩa.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def hanota(self, A: List[int], B: List[int], C: List[int]) -> None:
        def dfs(n, a, b, c):
            if n == 1:
                c.append(a.pop())
                return
            dfs(n - 1, a, c, b)
            c.append(a.pop())
            dfs(n - 1, b, a, c)

        dfs(len(A), A, B, C)
```

#### Java

```java
class Solution {
    public void hanota(List<Integer> A, List<Integer> B, List<Integer> C) {
        dfs(A.size(), A, B, C);
    }

    private void dfs(int n, List<Integer> a, List<Integer> b, List<Integer> c) {
        if (n == 1) {
            c.add(a.remove(a.size() - 1));
            return;
        }
        dfs(n - 1, a, c, b);
        c.add(a.remove(a.size() - 1));
        dfs(n - 1, b, a, c);
    }
}
```

#### C++

```cpp
class Solution {
public:
    void hanota(vector<int>& A, vector<int>& B, vector<int>& C) {
        auto dfs = [&](this auto&& dfs, int n, vector<int>& a, vector<int>& b, vector<int>& c) {
            if (n == 1) {
                c.push_back(a.back());
                a.pop_back();
                return;
            }
            dfs(n - 1, a, c, b);
            c.push_back(a.back());
            a.pop_back();
            dfs(n - 1, b, a, c);
        };
        dfs(A.size(), A, B, C);
    }
};
```

#### Go

```go
func hanota(A []int, B []int, C []int) []int {
	var dfs func(n int, a, b, c *[]int)
	dfs = func(n int, a, b, c *[]int) {
		if n == 1 {
			*c = append(*c, (*a)[len(*a)-1])
			*a = (*a)[:len(*a)-1]
			return
		}
		dfs(n-1, a, c, b)
		*c = append(*c, (*a)[len(*a)-1])
		*a = (*a)[:len(*a)-1]
		dfs(n-1, b, a, c)
	}
	dfs(len(A), &A, &B, &C)
	return C
}
```

#### TypeScript

```ts
/**
 Do not return anything, modify C in-place instead.
 */
function hanota(A: number[], B: number[], C: number[]): void {
    const dfs = (n: number, a: number[], b: number[], c: number[]) => {
        if (n === 1) {
            c.push(a.pop()!);
            return;
        }
        dfs(n - 1, a, c, b);
        c.push(a.pop()!);
        dfs(n - 1, b, a, c);
    };
    dfs(A.length, A, B, C);
}
```

#### Swift

```swift
class Solution {
    func hanota(_ A: inout [Int], _ B: inout [Int], _ C: inout [Int]) {
        dfs(n: A.count, a: &A, b: &B, c: &C)
    }

    private func dfs(n: Int, a: inout [Int], b: inout [Int], c: inout [Int]) {
        if n == 1 {
            c.append(a.removeLast())
            return
        }
        dfs(n: n - 1, a: &a, b: &c, c: &b)
        c.append(a.removeLast())
        dfs(n: n - 1, a: &b, b: &a, c: &c)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Lặp (Stack)

<!-- thinking:start -->

> **Tư duy**
>
> Đệ quy đã tạo ra chuỗi ngắn nhất, nhưng một số judge giới hạn độ sâu lời gọi.
>
> Một stack tường minh gồm các task $(n,a,b,c)$ sẽ thực hiện nước đi khi $n=1$; nếu không, nó push ba task con sao cho khi pop, chúng được thực hiện theo thứ tự “chuyển $n-1$, chuyển đĩa cơ sở, chuyển $n-1$”.

<!-- thinking:end -->

Ta có thể dùng một stack để mô phỏng quá trình đệ quy.

Ta định nghĩa một struct $Task$, biểu diễn một task, trong đó $n$ là số đĩa, còn $a$, $b$, $c$ là ba cọc.

Ta push task ban đầu $Task(len(A), A, B, C)$ vào stack, sau đó liên tục xử lý task ở đỉnh stack cho đến khi stack rỗng.

Nếu $n = 1$, ta trực tiếp chuyển đĩa từ $a$ sang $c$.

Ngược lại, ta push ba task con vào stack, cụ thể là:

1. Chuyển $n - 1$ đĩa từ $b$ sang $c$ với sự hỗ trợ của $a$;
2. Chuyển đĩa thứ $n$ từ $a$ sang $c$;
3. Chuyển $n - 1$ đĩa từ $a$ sang $b$ với sự hỗ trợ của $c$.

Độ phức tạp thời gian là $O(2^n)$, còn độ phức tạp không gian là $O(n)$. Ở đây, $n$ là số đĩa.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def hanota(self, A: List[int], B: List[int], C: List[int]) -> None:
        stk = [(len(A), A, B, C)]
        while stk:
            n, a, b, c = stk.pop()
            if n == 1:
                c.append(a.pop())
            else:
                stk.append((n - 1, b, a, c))
                stk.append((1, a, b, c))
                stk.append((n - 1, a, c, b))
```

#### Java

```java
class Solution {
    public void hanota(List<Integer> A, List<Integer> B, List<Integer> C) {
        Deque<Task> stk = new ArrayDeque<>();
        stk.push(new Task(A.size(), A, B, C));
        while (stk.size() > 0) {
            Task task = stk.pop();
            int n = task.n;
            List<Integer> a = task.a;
            List<Integer> b = task.b;
            List<Integer> c = task.c;
            if (n == 1) {
                c.add(a.remove(a.size() - 1));
            } else {
                stk.push(new Task(n - 1, b, a, c));
                stk.push(new Task(1, a, b, c));
                stk.push(new Task(n - 1, a, c, b));
            }
        }
    }
}

class Task {
    int n;
    List<Integer> a;
    List<Integer> b;
    List<Integer> c;

    public Task(int n, List<Integer> a, List<Integer> b, List<Integer> c) {
        this.n = n;
        this.a = a;
        this.b = b;
        this.c = c;
    }
}
```

#### C++

```cpp
struct Task {
    int n;
    vector<int>* a;
    vector<int>* b;
    vector<int>* c;
};

class Solution {
public:
    void hanota(vector<int>& A, vector<int>& B, vector<int>& C) {
        stack<Task> stk;
        stk.push({(int) A.size(), &A, &B, &C});
        while (!stk.empty()) {
            Task task = stk.top();
            stk.pop();
            if (task.n == 1) {
                task.c->push_back(task.a->back());
                task.a->pop_back();
            } else {
                stk.push({task.n - 1, task.b, task.a, task.c});
                stk.push({1, task.a, task.b, task.c});
                stk.push({task.n - 1, task.a, task.c, task.b});
            }
        }
    }
};
```

#### Go

```go
func hanota(A []int, B []int, C []int) []int {
	stk := []Task{{len(A), &A, &B, &C}}
	for len(stk) > 0 {
		task := stk[len(stk)-1]
		stk = stk[:len(stk)-1]
		if task.n == 1 {
			*task.c = append(*task.c, (*task.a)[len(*task.a)-1])
			*task.a = (*task.a)[:len(*task.a)-1]
		} else {
			stk = append(stk, Task{task.n - 1, task.b, task.a, task.c})
			stk = append(stk, Task{1, task.a, task.b, task.c})
			stk = append(stk, Task{task.n - 1, task.a, task.c, task.b})
		}
	}
	return C
}

type Task struct {
	n       int
	a, b, c *[]int
}
```

#### TypeScript

```ts
/**
 Do not return anything, modify C in-place instead.
 */
function hanota(A: number[], B: number[], C: number[]): void {
    const stk: any[] = [[A.length, A, B, C]];
    while (stk.length) {
        const [n, a, b, c] = stk.pop()!;
        if (n === 1) {
            c.push(a.pop());
        } else {
            stk.push([n - 1, b, a, c]);
            stk.push([1, a, b, c]);
            stk.push([n - 1, a, c, b]);
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
