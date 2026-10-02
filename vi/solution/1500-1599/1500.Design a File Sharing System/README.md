---
comments: true
difficulty: Medium
tags:
    - Design
    - Hash Table
    - Data Stream
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1500. Design a File Sharing System 🔒](https://leetcode.com/problems/design-a-file-sharing-system)

[中文文档](/solution/1500-1599/1500.Design%20a%20File%20Sharing%20System/README.md)

## Mô tả

<!-- description:start -->

<p>Chúng ta sẽ sử dụng một hệ thống chia sẻ file để chia sẻ một file rất lớn gồm <code>m</code> <b>chunk</b> nhỏ, có ID từ <code>1</code> đến <code>m</code>.</p>

<p>Khi người dùng tham gia hệ thống, hệ thống cần gán cho họ một ID <b>duy nhất</b>. ID duy nhất này chỉ được sử dụng <b>một lần</b> cho mỗi người dùng, nhưng khi một người dùng rời hệ thống, ID đó có thể được <b>tái sử dụng</b>.</p>

<p>Người dùng có thể request một chunk cụ thể của file, hệ thống cần trả về danh sách ID của tất cả người dùng sở hữu chunk này. Nếu người dùng nhận được danh sách ID không rỗng, họ sẽ nhận chunk được request thành công.</p>

<p><br />
Hãy implement class <code>FileSharing</code>:</p>

<ul>
	<li><code>FileSharing(int m)</code> Khởi tạo object với một file gồm <code>m</code> chunk.</li>
	<li><code>int join(int[] ownedChunks)</code>: Một người dùng mới tham gia hệ thống và sở hữu một số chunk của file. Hệ thống cần gán cho người dùng một ID là <b>số nguyên dương nhỏ nhất</b> chưa bị người dùng nào khác sử dụng. Trả về ID được gán.</li>
	<li><code>void leave(int userID)</code>: Người dùng có <code>userID</code> sẽ rời hệ thống, bạn không thể lấy các chunk của file từ họ nữa.</li>
	<li><code>int[] request(int userID, int chunkID)</code>: Người dùng <code>userID</code> request chunk file có <code>chunkID</code>. Trả về danh sách ID của tất cả người dùng sở hữu chunk này theo thứ tự tăng dần.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ:</strong></p>

<pre>
<b>Input:</b>
[&quot;FileSharing&quot;,&quot;join&quot;,&quot;join&quot;,&quot;join&quot;,&quot;request&quot;,&quot;request&quot;,&quot;leave&quot;,&quot;request&quot;,&quot;leave&quot;,&quot;join&quot;]
[[4],[[1,2]],[[2,3]],[[4]],[1,3],[2,2],[1],[2,1],[2],[[]]]
<b>Output:</b>
[null,1,2,3,[2],[1,2],null,[],null,1]
<b>Giải thích:</b>
FileSharing fileSharing = new FileSharing(4); // We use the system to share a file of 4 chunks.

fileSharing.join([1, 2]);    // A user who has chunks [1,2] joined the system, assign id = 1 to them and return 1.

fileSharing.join([2, 3]);    // A user who has chunks [2,3] joined the system, assign id = 2 to them and return 2.

fileSharing.join([4]);       // A user who has chunk [4] joined the system, assign id = 3 to them and return 3.

fileSharing.request(1, 3);   // The user with id = 1 requested the third file chunk, as only the user with id = 2 has the file, return [2] . Notice that user 1 now has chunks [1,2,3].

fileSharing.request(2, 2);   // The user with id = 2 requested the second file chunk, users with ids [1,2] have this chunk, thus we return [1,2].

fileSharing.leave(1);        // The user with id = 1 left the system, all the file chunks with them are no longer available for other users.

fileSharing.request(2, 1);   // The user with id = 2 requested the first file chunk, no one in the system has this chunk, we return empty list [].

fileSharing.leave(2);        // The user with id = 2 left the system.

fileSharing.join([]);        // A user who doesn&#39;t have any chunks joined the system, assign id = 1 to them and return 1. Notice that ids 1 and 2 are free and we can reuse them.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= m &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= ownedChunks.length &lt;= min(100, m)</code></li>
	<li><code>1 &lt;= ownedChunks[i] &lt;= m</code></li>
	<li>Các giá trị của <code>ownedChunks</code> là duy nhất.</li>
	<li><code>1 &lt;= chunkID &lt;= m</code></li>
	<li><code>userID</code> được đảm bảo là một người dùng trong hệ thống nếu bạn <strong>gán</strong> ID <strong>chính xác</strong>.</li>
	<li>Sẽ có nhiều nhất <code>10<sup>4</sup></code> lời gọi đến <code>join</code>, <code>leave</code> và <code>request</code>.</li>
	<li>Mỗi lời gọi <code>leave</code> sẽ có một lời gọi <code>join</code> tương ứng.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong></p>

<ul>
	<li>Điều gì xảy ra nếu hệ thống nhận diện người dùng bằng địa chỉ IP thay vì ID duy nhất, và người dùng ngắt kết nối rồi kết nối lại từ cùng một IP?</li>
	<li>Nếu người dùng trong hệ thống thường xuyên tham gia và rời hệ thống mà không request chunk nào, solution của bạn có còn hiệu quả không?</li>
	<li>Nếu tất cả người dùng tham gia hệ thống một lần, request tất cả file rồi rời đi, solution của bạn có còn hiệu quả không?</li>
<li>Nếu hệ thống được dùng để chia sẻ <code>n</code> file, trong đó file thứ <code>ith</code> gồm <code>m[i]</code>, bạn cần thay đổi những gì?</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lần join phải nhận ID dương chưa được sử dụng nhỏ nhất, và ID của người rời đi phải được tái sử dụng ngay; đồng thời chúng ta cần theo dõi các chunk mỗi người dùng đang sở hữu và khi request thì trả về mọi người dùng hiện đang sở hữu chunk đó. Nếu quét từ $1$ trong mỗi lần join, thời gian sẽ tuyến tính theo số người dùng từng xuất hiện, điều này không phù hợp khi có đến $10^4$ lời gọi và $m \le 10^5$.
>
> Khi chưa có ID nào được tái sử dụng, các ID mới tăng dần; các ID đã được giải phóng tạo thành một pool có thể tái sử dụng. Một counter tăng dần cấp ID mới, còn min-heap lưu các ID đã được giải phóng, nhờ đó ID trống nhỏ nhất có thể được lấy trong thời gian logarit. Hash map lưu ánh xạ user $\to$ tập chunk. Một request sẽ quét các user đang online, điều này chấp nhận được với giới hạn số lời gọi; nếu kết quả không rỗng, requester cũng nhận chunk đó.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class FileSharing:
    def __init__(self, m: int):
        self.cur = 0
        self.chunks = m
        self.reused = []
        self.user_chunks = defaultdict(set)

    def join(self, ownedChunks: List[int]) -> int:
        if self.reused:
            userID = heappop(self.reused)
        else:
            self.cur += 1
            userID = self.cur
        self.user_chunks[userID] = set(ownedChunks)
        return userID

    def leave(self, userID: int) -> None:
        heappush(self.reused, userID)
        self.user_chunks.pop(userID)

    def request(self, userID: int, chunkID: int) -> List[int]:
        if chunkID < 1 or chunkID > self.chunks:
            return []
        res = []
        for k, v in self.user_chunks.items():
            if chunkID in v:
                res.append(k)
        if res:
            self.user_chunks[userID].add(chunkID)
        return sorted(res)


# Your FileSharing object will be instantiated and called as such:
# obj = FileSharing(m)
# param_1 = obj.join(ownedChunks)
# obj.leave(userID)
# param_3 = obj.request(userID,chunkID)
```

#### Java

```java
class FileSharing {
    private int chunks;
    private int cur;
    private TreeSet<Integer> reused;
    private TreeMap<Integer, Set<Integer>> userChunks;

    public FileSharing(int m) {
        cur = 0;
        chunks = m;
        reused = new TreeSet<>();
        userChunks = new TreeMap<>();
    }

    public int join(List<Integer> ownedChunks) {
        int userID;
        if (reused.isEmpty()) {
            ++cur;
            userID = cur;
        } else {
            userID = reused.pollFirst();
        }
        userChunks.put(userID, new HashSet<>(ownedChunks));
        return userID;
    }

    public void leave(int userID) {
        reused.add(userID);
        userChunks.remove(userID);
    }

    public List<Integer> request(int userID, int chunkID) {
        if (chunkID < 1 || chunkID > chunks) {
            return Collections.emptyList();
        }
        List<Integer> res = new ArrayList<>();
        for (Map.Entry<Integer, Set<Integer>> entry : userChunks.entrySet()) {
            if (entry.getValue().contains(chunkID)) {
                res.add(entry.getKey());
            }
        }
        if (!res.isEmpty()) {
            userChunks.computeIfAbsent(userID, k -> new HashSet<>()).add(chunkID);
        }
        return res;
    }
}

/**
 * Your FileSharing object will be instantiated and called as such:
 * FileSharing obj = new FileSharing(m);
 * int param_1 = obj.join(ownedChunks);
 * obj.leave(userID);
 * List<Integer> param_3 = obj.request(userID,chunkID);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
