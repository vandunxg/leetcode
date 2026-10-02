---
comments: true
difficulty: Hard
tags:
    - Stack
    - Breadth-First Search
    - Memoization
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [488. Zuma Game](https://leetcode.com/problems/zuma-game)

[中文文档](/solution/0400-0499/0488.Zuma%20Game/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn đang chơi một biến thể của trò Zuma.</p>

<p>Trong biến thể Zuma này, trên board có <strong>một hàng duy nhất</strong> gồm các quả bóng màu. Mỗi quả có thể màu đỏ <code>&#39;R&#39;</code>, vàng <code>&#39;Y&#39;</code>, xanh dương <code>&#39;B&#39;</code>, xanh lá <code>&#39;G&#39;</code> hoặc trắng <code>&#39;W&#39;</code>. Bạn cũng có một số quả bóng màu trong tay.</p>

<p>Mục tiêu của bạn là <strong>xóa hết</strong> bóng khỏi board. Trong mỗi lượt:</p>

<ul>
	<li>Chọn <strong>bất kỳ</strong> quả bóng nào trong tay và chèn vào giữa hai quả trong hàng hoặc ở một trong hai đầu hàng.</li>
	<li>Nếu có một nhóm gồm <strong>ba quả bóng liên tiếp trở lên</strong> có <strong>cùng màu</strong>, hãy xóa nhóm bóng đó khỏi board.
	<ul>
		<li>Nếu việc xóa này khiến các nhóm mới gồm ba quả bóng cùng màu trở lên được tạo thành, tiếp tục xóa từng nhóm cho đến khi không còn nhóm nào.</li>
	</ul>
	</li>
	<li>Nếu trên board không còn quả bóng nào thì bạn thắng.</li>
	<li>Lặp lại quy trình này cho đến khi bạn thắng hoặc hết bóng trong tay.</li>
</ul>

<p>Cho chuỗi <code>board</code> biểu diễn hàng bóng trên board và chuỗi <code>hand</code> biểu diễn các bóng trong tay. Hãy trả về <em>số bóng <strong>ít nhất</strong> cần chèn để xóa hết bóng khỏi board. Nếu không thể xóa hết bóng bằng số bóng trong tay, trả về </em><code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> board = &quot;WRRBBW&quot;, hand = &quot;RB&quot;
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không thể xóa hết bóng. Cách tốt nhất bạn có thể làm là:
- Chèn &#39;R&#39; để board thành WRR<u>R</u>BBW. W<u>RRR</u>BBW -&gt; WBBW.
- Chèn &#39;B&#39; để board thành WBB<u>B</u>W. W<u>BBB</u>W -&gt; WW.
Trên board vẫn còn bóng nhưng bạn đã hết bóng để chèn.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> board = &quot;WWRRBBWW&quot;, hand = &quot;WRBRW&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Để xóa hết bóng trên board:
- Chèn &#39;R&#39; để board thành WWRR<u>R</u>BBWW. WW<u>RRR</u>BBWW -&gt; WWBBWW.
- Chèn &#39;B&#39; để board thành WWBB<u>B</u>WW. WW<u>BBB</u>WW -&gt; <u>WWWW</u> -&gt; rỗng.
Cần dùng 2 quả bóng trong tay để xóa hết bóng trên board.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> board = &quot;G&quot;, hand = &quot;GGGGG&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Để xóa hết bóng trên board:
- Chèn &#39;G&#39; để board thành G<u>G</u>.
- Chèn &#39;G&#39; để board thành GG<u>G</u>. <u>GGG</u> -&gt; rỗng.
Cần dùng 2 quả bóng trong tay để xóa hết bóng trên board.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= board.length &lt;= 16</code></li>
	<li><code>1 &lt;= hand.length &lt;= 5</code></li>
	<li><code>board</code> và <code>hand</code> chỉ gồm các ký tự <code>&#39;R&#39;</code>, <code>&#39;Y&#39;</code>, <code>&#39;B&#39;</code>, <code>&#39;G&#39;</code> và <code>&#39;W&#39;</code>.</li>
	<li>Hàng bóng ban đầu trên board <strong>không</strong> có nhóm nào gồm ba quả bóng cùng màu liên tiếp trở lên.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chèn bóng từ tay để xóa các dãy từ ba quả trở lên; cần tìm số lần chèn ít nhất để làm trống board. Có nhiều lựa chọn vị trí và màu, nhưng board và hand đều ngắn.
>
> Chạy BFS trên trạng thái (board, số bóng còn lại trong tay). Với mỗi màu khác nhau và mỗi vị trí, chèn bóng rồi liên tục xóa các dãy dài ít nhất $3$ bằng regex. Khi board rỗng, số bóng đã dùng là đáp án.
>
> Chỉ thử mỗi màu một lần để tránh mở rộng các trạng thái hand giống nhau. Set visited lưu board để tránh lặp trạng thái. Cần lặp thao tác xóa vì một lần xóa có thể kéo theo các lần xóa tiếp theo.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findMinStep(self, board: str, hand: str) -> int:
        def remove(s):
            while len(s):
                next = re.sub(r'B{3,}|G{3,}|R{3,}|W{3,}|Y{3,}', '', s)
                if len(next) == len(s):
                    break
                s = next
            return s

        visited = set()
        q = deque([(board, hand)])
        while q:
            state, balls = q.popleft()
            if not state:
                return len(hand) - len(balls)
            for ball in set(balls):
                b = balls.replace(ball, '', 1)
                for i in range(1, len(state) + 1):
                    s = state[:i] + ball + state[i:]
                    s = remove(s)
                    if s not in visited:
                        visited.add(s)
                        q.append((s, b))
        return -1
```

#### Java

```java
class Solution {
    public int findMinStep(String board, String hand) {
        final Zuma zuma = Zuma.create(board, hand);
        final HashSet<Long> visited = new HashSet<>();
        final ArrayList<Zuma> init = new ArrayList<>();

        visited.add(zuma.board());
        init.add(zuma);
        return bfs(init, 0, visited);
    }

    private int bfs(ArrayList<Zuma> curr, int k, HashSet<Long> visited) {
        if (curr.isEmpty()) {
            return -1;
        }

        final ArrayList<Zuma> next = new ArrayList<>();

        for (Zuma zuma : curr) {
            ArrayList<Zuma> neib = zuma.getNextLevel(k, visited);
            if (neib == null) {
                return k + 1;
            }

            next.addAll(neib);
        }
        return bfs(next, k + 1, visited);
    }
}

record Zuma(long board, long hand) {
    public static Zuma create(String boardStr, String handStr) {
        return new Zuma(Zuma.encode(boardStr, false), Zuma.encode(handStr, true));
    }

    public ArrayList<Zuma> getNextLevel(int depth, HashSet<Long> visited) {
        final ArrayList<Zuma> next = new ArrayList<>();
        final ArrayList<long[]> handList = this.buildHandList();
        final long[] boardList = new long[32];
        final int size = this.buildBoardList(boardList);

        for (long[] pair : handList) {
            for (int i = 0; i < size; ++i) {
                final long rawBoard = pruningCheck(boardList[i], pair[0], i * 3, depth);
                if (rawBoard == -1) {
                    continue;
                }

                final long nextBoard = updateBoard(rawBoard);
                if (nextBoard == 0) {
                    return null;
                }

                if (pair[1] == 0 || visited.contains(nextBoard)) {
                    continue;
                }

                visited.add(nextBoard);
                next.add(new Zuma(nextBoard, pair[1]));
            }
        }
        return next;
    }

    private long pruningCheck(long insBoard, long ball, int pos, int depth) {
        final long L = (insBoard >> (pos + 3)) & 0x7;
        final long R = (insBoard >> (pos - 3)) & 0x7;

        if (depth == 0 && (ball != R) && (L != R) || depth > 0 && (ball != R)) {
            return -1;
        }
        return insBoard | (ball << pos);
    }

    private long updateBoard(long board) {
        long stack = 0;

        for (int i = 0; i < 64; i += 3) {
            final long curr = (board >> i) & 0x7;
            final long top = (stack) &0x7;

            // pop (if possible)
            if ((top > 0) && (curr != top) && (stack & 0x3F) == ((stack >> 3) & 0x3F)) {
                stack >>= 9;
                if ((stack & 0x7) == top) stack >>= 3;
            }

            if (curr == 0) {
                // done
                break;
            }
            // push and continue
            stack = (stack << 3) | curr;
        }
        return stack;
    }

    private ArrayList<long[]> buildHandList() {
        final ArrayList<long[]> handList = new ArrayList<>();
        long prevBall = 0;
        long ballMask = 0;

        for (int i = 0; i < 16; i += 3) {
            final long currBall = (this.hand >> i) & 0x7;
            if (currBall == 0) {
                break;
            }

            if (currBall != prevBall) {
                prevBall = currBall;
                handList.add(
                    new long[] {currBall, ((this.hand >> 3) & ~ballMask) | (this.hand & ballMask)});
            }
            ballMask = (ballMask << 3) | 0x7;
        }
        return handList;
    }

    private int buildBoardList(long[] buffer) {
        int ptr = 0;
        long ballMask = 0x7;
        long insBoard = this.board << 3;
        buffer[ptr++] = insBoard;

        while (true) {
            final long currBall = this.board & ballMask;
            if (currBall == 0) {
                break;
            }

            ballMask <<= 3;
            insBoard = (insBoard | currBall) & ~ballMask;
            buffer[ptr++] = insBoard;
        }
        return ptr;
    }

    private static long encode(String stateStr, boolean sortFlag) {
        final char[] stateChars = stateStr.toCharArray();
        if (sortFlag) {
            Arrays.sort(stateChars);
        }

        long stateBits = 0;
        for (char ch : stateChars) {
            stateBits = (stateBits << 3) | Zuma.encode(ch);
        }
        return stateBits;
    }

    private static long encode(char ch) {
        return switch (ch) {
            case 'R' -> 0x1;
            case 'G' -> 0x2;
            case 'B' -> 0x3;
            case 'W' -> 0x4;
            case 'Y' -> 0x5;
            case ' ' -> 0x0;
            default  ->
                throw new IllegalArgumentException("Invalid char: " + ch);
        };
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
