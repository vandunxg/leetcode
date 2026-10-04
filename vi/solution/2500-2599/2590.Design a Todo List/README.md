---
comments: true
difficulty: Medium
tags:
    - Design
    - Array
    - Hash Table
    - String
    - Sorting
---

<!-- problem:start -->

# [2590. Design a Todo List 🔒](https://leetcode.com/problems/design-a-todo-list)

[中文文档](/solution/2500-2599/2590.Design%20a%20Todo%20List/README.md)

## Mô tả

<!-- description:start -->

<p>Thiết kế một Todo List, trong đó người dùng có thể thêm <strong>task</strong>, đánh dấu task là <strong>hoàn thành</strong> hoặc lấy danh sách các task đang chờ xử lý. Người dùng cũng có thể thêm <strong>tag</strong> cho task và lọc task theo một số tag nhất định.</p>

<p>Cài đặt class <code>TodoList</code>:</p>

<ul>
	<li><code>TodoList()</code> Khởi tạo object.</li>
	<li><code>int addTask(int userId, String taskDescription, int dueDate, List&lt;String&gt; tags)</code> Thêm một task cho user có ID <code>userId</code>, với hạn hoàn thành bằng <code>dueDate</code> và danh sách tag được gắn cho task. Giá trị trả về là ID của task. ID này bắt đầu từ <code>1</code> và tăng <strong>tuần tự</strong>. Nghĩa là ID của task đầu tiên phải là <code>1</code>, task thứ hai là <code>2</code>, v.v.</li>
	<li><code>List&lt;String&gt; getAllTasks(int userId)</code> Trả về danh sách tất cả task chưa được đánh dấu là hoàn thành của user có ID <code>userId</code>, theo thứ tự hạn hoàn thành. Trả về danh sách rỗng nếu user không có task nào chưa hoàn thành.</li>
	<li><code>List&lt;String&gt; getTasksForTag(int userId, String tag)</code> Trả về danh sách tất cả task chưa được đánh dấu là hoàn thành của user có ID <code>userId</code> và có <code>tag</code> trong danh sách tag, theo thứ tự hạn hoàn thành. Trả về danh sách rỗng nếu không tồn tại task phù hợp.</li>
	<li><code>void completeTask(int userId, int taskId)</code> Đánh dấu task có ID <code>taskId</code> là đã hoàn thành chỉ khi task tồn tại, thuộc về user có ID <code>userId</code> và chưa hoàn thành.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;TodoList&quot;, &quot;addTask&quot;, &quot;addTask&quot;, &quot;getAllTasks&quot;, &quot;getAllTasks&quot;, &quot;addTask&quot;, &quot;getTasksForTag&quot;, &quot;completeTask&quot;, &quot;completeTask&quot;, &quot;getTasksForTag&quot;, &quot;getAllTasks&quot;]
[[], [1, &quot;Task1&quot;, 50, []], [1, &quot;Task2&quot;, 100, [&quot;P1&quot;]], [1], [5], [1, &quot;Task3&quot;, 30, [&quot;P1&quot;]], [1, &quot;P1&quot;], [5, 1], [1, 2], [1, &quot;P1&quot;], [1]]
<strong>Đầu ra</strong>
[null, 1, 2, [&quot;Task1&quot;, &quot;Task2&quot;], [], 3, [&quot;Task3&quot;, &quot;Task2&quot;], null, null, [&quot;Task3&quot;], [&quot;Task3&quot;, &quot;Task1&quot;]]

<strong>Giải thích</strong>
TodoList todoList = new TodoList();
todoList.addTask(1, &quot;Task1&quot;, 50, []); // return 1. This adds a new task for the user with id 1.
todoList.addTask(1, &quot;Task2&quot;, 100, [&quot;P1&quot;]); // return 2. This adds another task for the user with id 1.
todoList.getAllTasks(1); // return [&quot;Task1&quot;, &quot;Task2&quot;]. User 1 has two uncompleted tasks so far.
todoList.getAllTasks(5); // return []. User 5 does not have any tasks so far.
todoList.addTask(1, &quot;Task3&quot;, 30, [&quot;P1&quot;]); // return 3. This adds another task for the user with id 1.
todoList.getTasksForTag(1, &quot;P1&quot;); // return [&quot;Task3&quot;, &quot;Task2&quot;]. This returns the uncompleted tasks that have the tag &quot;P1&quot; for the user with id 1.
todoList.completeTask(5, 1); // This does nothing, since task 1 does not belong to user 5.
todoList.completeTask(1, 2); // This marks task 2 as completed.
todoList.getTasksForTag(1, &quot;P1&quot;); // return [&quot;Task3&quot;]. This returns the uncompleted tasks that have the tag &quot;P1&quot; for the user with id 1.
                                   // Notice that we did not include &quot;Task2&quot; because it is completed now.
todoList.getAllTasks(1); // return [&quot;Task3&quot;, &quot;Task1&quot;]. User 1 now has 2 uncompleted tasks.

</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= userId, taskId, dueDate &lt;= 100</code></li>
	<li><code>0 &lt;= tags.length &lt;= 100</code></li>
	<li><code>1 &lt;= taskDescription.length &lt;= 50</code></li>
	<li><code>1 &lt;= tags[i].length, tag.length &lt;= 20</code></li>
	<li>Tất cả giá trị <code>dueDate</code> đều khác nhau.</li>
	<li>Tất cả chuỗi chỉ gồm các chữ cái tiếng Anh viết thường, viết hoa và chữ số.</li>
	<li>Mỗi method được gọi nhiều nhất <code>100</code> lần.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Sorted Set

<!-- thinking:start -->

> **Tư duy**
>
> Các task thuộc về từng user, các task chưa hoàn thành phải được liệt kê theo hạn hoàn thành, đồng thời cần lọc theo tag và đánh dấu hoàn thành theo id. Giới hạn số thao tác cho phép chúng ta duyệt tuyến tính để hoàn thành một task.
>
> Một hash map lưu một danh sách đã sắp xếp cho mỗi user theo dạng $(\textit{due},\textit{desc},\textit{tags},\textit{id},\textit{done})$. Các task được chèn theo đúng thứ tự hạn hoàn thành. Khi hoàn thành, ta đánh dấu task có id tương ứng.

<!-- thinking:end -->

Chúng ta dùng một hash table $tasks$ để lưu tập các task của mỗi user, trong đó key là ID user và value là một sorted set được sắp xếp theo hạn hoàn thành của task. Ngoài ra, chúng ta dùng biến $i$ để lưu ID task hiện tại.

Khi gọi method `addTask`, chúng ta thêm task vào tập task của user tương ứng và trả về ID task. Độ phức tạp thời gian của thao tác này là $O(\log n)$.

Khi gọi method `getAllTasks`, chúng ta duyệt qua tập task của user tương ứng, thêm mô tả của các task chưa hoàn thành vào danh sách kết quả rồi trả về danh sách kết quả. Độ phức tạp thời gian của thao tác này là $O(n)$.

Khi gọi method `getTasksForTag`, chúng ta duyệt qua tập task của user tương ứng, thêm mô tả của các task chưa hoàn thành vào danh sách kết quả rồi trả về danh sách kết quả. Độ phức tạp thời gian của thao tác này là $O(n)$.

Khi gọi method `completeTask`, chúng ta duyệt qua tập task của user tương ứng và đánh dấu task có ID task là $taskId$ đã hoàn thành. Độ phức tạp thời gian của thao tác này là $(n)$.

Độ phức tạp không gian là $O(n)$, trong đó $n$ là số lượng tất cả task.

<!-- tabs:start -->

#### Python3

```python
class TodoList:
    def __init__(self):
        self.i = 1
        self.tasks = defaultdict(SortedList)

    def addTask(
        self, userId: int, taskDescription: str, dueDate: int, tags: List[str]
    ) -> int:
        taskId = self.i
        self.i += 1
        self.tasks[userId].add([dueDate, taskDescription, set(tags), taskId, False])
        return taskId

    def getAllTasks(self, userId: int) -> List[str]:
        return [x[1] for x in self.tasks[userId] if not x[4]]

    def getTasksForTag(self, userId: int, tag: str) -> List[str]:
        return [x[1] for x in self.tasks[userId] if not x[4] and tag in x[2]]

    def completeTask(self, userId: int, taskId: int) -> None:
        for task in self.tasks[userId]:
            if task[3] == taskId:
                task[4] = True
                break


# Your TodoList object will be instantiated and called as such:
# obj = TodoList()
# param_1 = obj.addTask(userId,taskDescription,dueDate,tags)
# param_2 = obj.getAllTasks(userId)
# param_3 = obj.getTasksForTag(userId,tag)
# obj.completeTask(userId,taskId)
```

#### Java

```java
class Task {
    int taskId;
    String taskName;
    int dueDate;
    Set<String> tags;
    boolean finish;

    public Task(int taskId, String taskName, int dueDate, Set<String> tags) {
        this.taskId = taskId;
        this.taskName = taskName;
        this.dueDate = dueDate;
        this.tags = tags;
    }
}

class TodoList {
    private int i = 1;
    private Map<Integer, TreeSet<Task>> tasks = new HashMap<>();

    public TodoList() {
    }

    public int addTask(int userId, String taskDescription, int dueDate, List<String> tags) {
        Task task = new Task(i++, taskDescription, dueDate, new HashSet<>(tags));
        tasks.computeIfAbsent(userId, k -> new TreeSet<>(Comparator.comparingInt(a -> a.dueDate)))
            .add(task);
        return task.taskId;
    }

    public List<String> getAllTasks(int userId) {
        List<String> ans = new ArrayList<>();
        if (tasks.containsKey(userId)) {
            for (Task task : tasks.get(userId)) {
                if (!task.finish) {
                    ans.add(task.taskName);
                }
            }
        }
        return ans;
    }

    public List<String> getTasksForTag(int userId, String tag) {
        List<String> ans = new ArrayList<>();
        if (tasks.containsKey(userId)) {
            for (Task task : tasks.get(userId)) {
                if (task.tags.contains(tag) && !task.finish) {
                    ans.add(task.taskName);
                }
            }
        }
        return ans;
    }

    public void completeTask(int userId, int taskId) {
        if (tasks.containsKey(userId)) {
            for (Task task : tasks.get(userId)) {
                if (task.taskId == taskId) {
                    task.finish = true;
                    break;
                }
            }
        }
    }
}

/**
 * Your TodoList object will be instantiated and called as such:
 * TodoList obj = new TodoList();
 * int param_1 = obj.addTask(userId,taskDescription,dueDate,tags);
 * List<String> param_2 = obj.getAllTasks(userId);
 * List<String> param_3 = obj.getTasksForTag(userId,tag);
 * obj.completeTask(userId,taskId);
 */
```

#### Rust

```rust
use std::collections::{HashMap, HashSet};

#[derive(Clone)]
struct Task {
    task_id: i32,
    description: String,
    tags: HashSet<String>,
    due_date: i32,
}

struct TodoList {
    /// The global task id
    id: i32,
    /// The mapping from `user_id` to `task`
    user_map: HashMap<i32, Vec<Task>>,
}

impl TodoList {
    fn new() -> Self {
        Self {
            id: 1,
            user_map: HashMap::new(),
        }
    }

    fn add_task(
        &mut self,
        user_id: i32,
        task_description: String,
        due_date: i32,
        tags: Vec<String>,
    ) -> i32 {
        if self.user_map.contains_key(&user_id) {
            // Just add the task
            self.user_map.get_mut(&user_id).unwrap().push(Task {
                task_id: self.id,
                description: task_description,
                tags: tags.into_iter().collect::<HashSet<String>>(),
                due_date,
            });
            // Increase the global id
            self.id += 1;
            return self.id - 1;
        }
        // Otherwise, create a new user
        self.user_map.insert(
            user_id,
            vec![Task {
                task_id: self.id,
                description: task_description,
                tags: tags.into_iter().collect::<HashSet<String>>(),
                due_date,
            }],
        );
        self.id += 1;
        self.id - 1
    }

    fn get_all_tasks(&self, user_id: i32) -> Vec<String> {
        if !self.user_map.contains_key(&user_id) || self.user_map.get(&user_id).unwrap().is_empty()
        {
            return vec![];
        }
        // Get the task vector
        let mut ret_vec = (*self.user_map.get(&user_id).unwrap()).clone();
        // Sort by due date
        ret_vec.sort_by(|lhs, rhs| lhs.due_date.cmp(&rhs.due_date));
        // Return the description vector
        ret_vec.into_iter().map(|x| x.description).collect()
    }

    fn get_tasks_for_tag(&self, user_id: i32, tag: String) -> Vec<String> {
        if !self.user_map.contains_key(&user_id) || self.user_map.get(&user_id).unwrap().is_empty()
        {
            return vec![];
        }
        // Get the task vector
        let mut ret_vec = (*self.user_map.get(&user_id).unwrap()).clone();
        // Sort by due date
        ret_vec.sort_by(|lhs, rhs| lhs.due_date.cmp(&rhs.due_date));
        // Return the description vector
        ret_vec
            .into_iter()
            .filter(|x| x.tags.contains(&tag))
            .map(|x| x.description)
            .collect()
    }

    fn complete_task(&mut self, user_id: i32, task_id: i32) {
        if !self.user_map.contains_key(&user_id) || self.user_map.get(&user_id).unwrap().is_empty()
        {
            return;
        }
        self.user_map
            .get_mut(&user_id)
            .unwrap()
            .retain(|x| (*x).task_id != task_id);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
