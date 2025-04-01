# Tổng hợp các lưu ý về kiểu dữ liệu dạng tập hợp
## 1. `Array` và `List`
- Array và List đều là những kiểu dữ liệu được sử dụng nhiều và phổ biến
### 1.1. Array
- Có kích thước cố định
- Truy cập phần tử nhanh (O(1))
- Không có sẵn phương thức thêm/xoá phần tử
- [Read more docs](https://learn.microsoft.com/en-us/dotnet/api/system.array?view=net-9.0)
```csharp
int[] numbers = new int[3] { 1, 2, 3 };
Debug.Log(numbers[0]); // 1
```
### 1.2. List
- Kích thước linh hoạt
- Thêm/xoá các phần tử dễ dàng
- Tìm kiếm phần tử chậm hơn HashSet.
- [Read more docs](https://learn.microsoft.com/en-us/dotnet/api/system.array?view=net-9.0)
```csharp
List<string> players = new List<string>() { "Alice", "Bob" };
players.Add("Charlie");
Debug.Log(players[1]); // Bob
```
### 1.3. Lưu ý khi sử dụng vòng lặp for/foreach ([Read more](https://github.com/pancake-llc/foundation/wiki/performance-guide))
- Đối với `Array`, vòng lặp foreach đôi khi có thể nhanh như, hoặc thậm chí nhanh hơn, vòng lặp for. Điều này là do trình biên dịch C# tối ưu hóa foreach cho mảng, khiến nó gần như tương đương với vòng lặp for. Vì mảng có kích thước cố định và các phần tử của chúng được lưu trữ trong một khối bộ nhớ liền kề, trình biên dịch có thể tạo mã IL (Ngôn ngữ trung gian) rất hiệu quả cho foreach trên mảng, thường khiến nó hiệu quả như vòng lặp for.
- Đối với `List`, vòng lặp for thường nhanh hơn foreach khi lặp qua các đối tượng List. Điều này là do foreach tạo ra một đối tượng Enumerator, có thể thêm một chút chi phí, trong khi for truy cập trực tiếp các phần tử theo chỉ mục. Việc phân bổ thêm này trong foreach có thể ảnh hưởng một chút đến hiệu suất, đặc biệt là trong các tình huống mà chi phí thu gom rác (GC) là một mối quan tâm.
- Tóm lại:
    - Sử dụng for với Lists: Đối với các trường hợp bạn đang lặp lại các danh sách lớn hoặc có mã nhạy cảm về hiệu suất, for thường được ưu tiên cho List. Điều này có thể tránh việc phân bổ enumerator bổ sung đi kèm với foreach.
    - Sử dụng foreach với Mảng: Nếu bạn đang lặp qua các mảng, foreach thường có hiệu suất ngang bằng với for và có thể giúp mã của bạn dễ đọc hơn.
 

## 2. `HashSet`
- HashSet rất hữu ích khi bạn cần lưu các phần tử duy nhất và thực hiện các tháo tác tập hợp hiệu quả
- [Read more docs](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.hashset-1?view=net-9.0)

```csharp
HashSet<string> enemyNames = new HashSet<string>();

enemyNames.Add("Zombie");
enemyNames.Add("Skeleton");
enemyNames.Add("Orc");

// Thêm phần tử trùng lặp sẽ không có tác dụng
enemyNames.Add("Zombie"); // Không được thêm vào
```
### 2.1 Ưu điểm của HashSet
- Hiệu suất cao: Các thao tác Contains, Add, Remove có độ phức tạp O(1)
- Tự động loại bỏ trùng lặp: Đảm bảo mọi phần tử là duy nhất
### 2.2 So sánh với List
|Tính năng|HashSet|List|
|:--------|:------|:---|
|Tốc độ tìm kiếm|O(1)|O(n)|
|Cho phép trùng lặp|Không|Có|
|Thứ tự phần tử|Không đảm bảo|Được đảm bảo|
|Tốc độ thêm/xóa|Nhanh|Chậm hơn (đặc biệt khi xóa)|

==> Tóm lại: `HashSet là sự lựa chọn tuyệt vời khi bạn cần lưu trữ các phần tử duy nhất và thường xuyên kiểm tra sự tồn tại của phần tử`
## 3. `Dictionary`
- Là cấu trúc dữ liệu cho phép lưu trữ dữ liệu theo dạng cặp `key-value` với hiệu suất truy xuất cao
- [Read more docs](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.dictionary-2?view=net-9.0)

```csharp
using System.Collections.Generic;

// Khởi tạo Dictionary cơ bản
Dictionary<string, int> playerScores = new Dictionary<string, int>();

// Khởi tạo với giá trị ban đầu
Dictionary<string, string> weaponTypes = new Dictionary<string, string>() {
    {"sword", "melee"},
    {"bow", "ranged"},
    {"staff", "magic"}
};
```
```csharp
playerScores.Add("Player1", 100);
playerScores.Add("Player2", 150);

// Hoặc sử dụng indexer
playerScores["Player3"] = 200;

int score = playerScores["Player1"]; // Lấy giá trị

// Kiểm tra tồn tại key trước khi truy cập
if (playerScores.ContainsKey("Player2")) {
    Debug.Log("Score của Player2: " + playerScores["Player2"]);
}
```
### 3.1 Ưu điểm 
- Truy xuất nhanh: Độ phức tạp O(1) cho các thao tác tìm kiếm, thêm, xóa
- Tổ chức linh hoạt: Dễ dàng ánh xạ giữa các đối tượng game
- Hiệu suất cao: Lý tưởng cho hệ thống truy câp nhanh như inventory, player data, ...
### 3.2 Lưu ý quan trọng
- Key phải là duy nhất: Mỗi key chỉ xuất hiện một lần trong Dictionary
- Key không được null: Sẽ gây ra exception nếu cố gắng sử dụng null key
- Kiểm tra tồn tại: Luôn kiểm tra ContainsKey trước khi truy cập để tránh KeyNotFoundException
- Hiệu suất: Dictionary chiếm nhiều bộ nhớ hơn List/Array nhưng truy xuất nhanh hơn
### 3.3 So sánh với các cấu trúc khác
| Tính năng               | Dictionary<TKey,TValue> | List<T>          | Array (T[])      | HashSet<T>       |
|-------------------------|------------------------|------------------|------------------|------------------|
| **Truy cập bằng key**    | ✅ O(1)                | ❌ Không hỗ trợ  | ❌ Không hỗ trợ  | ❌ Chỉ kiểm tra tồn tại |
| **Truy cập bằng index**  | ❌ Không hỗ trợ        | ✅ O(1)          | ✅ O(1)          | ❌ Không hỗ trợ  |
| **Tìm kiếm phần tử**     | ✅ O(1) (by key)       | ⚠️ O(n)         | ⚠️ O(n)         | ✅ O(1)          |
| **Thêm phần tử**         | ✅ O(1)                | ✅ O(1)*         | ❌ Kích thước cố định | ✅ O(1)          |
| **Xóa phần tử**          | ✅ O(1)                | ⚠️ O(n)         | ❌ Không hỗ trợ  | ✅ O(1)          |
| **Lưu trữ key-value**    | ✅ Có                  | ❌ Không         | ❌ Không         | ❌ Chỉ lưu giá trị |
| **Cho phép trùng lặp**   | ❌ Key phải duy nhất   | ✅ Có            | ✅ Có            | ❌ Giá trị duy nhất |
| **Bảo toàn thứ tự**      | ❌ Không đảm bảo       | ✅ Có            | ✅ Có            | ❌ Không đảm bảo |
| **Tốt cho**              | Tra cứu nhanh bằng key | Danh sách có thứ tự | Dữ liệu cố định | Kiểm tra tồn tại nhanh |

*Ghi chú: 
- ✅ = Hỗ trợ tốt 
- ⚠️ = Có thể chậm với dữ liệu lớn 
- ❌ = Không hỗ trợ
- O(1)* với List: O(1) nếu Capacity chưa đầy, O(n) khi cần mở rộng

==> Tóm lại: `Dictionary là công cụ mạnh mẽ để quản lý dữ liệu trong Unity, đặc biệt khi bạn cần ánh xạ giữa các định danh và giá trị với hiệu suất cao.`
## 4. `Queue`
- Queue là cấu trúc dữ liệu hàng đợi (FIFO - First In First Out), nghĩa là phần tử nào được thêm vào trước thì sẽ được lấy ra trước. Nó hữu ích khi bạn cần quản lý một danh sách các hành động, sự kiện hoặc nhiệm vụ cần xử lý theo thứ tự.
```csharp
Queue<int> queue = new Queue<int>(); // Khởi tạo queue chứa số nguyên

void Add(){
    queue.Enqueue(1);
    queue.Enqueue(2);
    queue.Enqueue(3);
}

int Get(){
    int firstElement = queue.Dequeue(); // Lấy ra 1 và xóa khỏi hàng đợi
    return firstElement; // kết quả trả về bằng 1
}

int Peek(){
    int front = queue.Peek(); // Xem phần tử đầu tiên nhưng không xóa
    return front
}
```
### Ví dụ sử dụng Queue để quản lý nhiệm vụ trong game
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class TaskManager : MonoBehaviour
{
    Queue<string> taskQueue = new Queue<string>();

    void Start()
    {
        // Thêm nhiệm vụ vào hàng đợi
        taskQueue.Enqueue("Nhặt vũ khí");
        taskQueue.Enqueue("Tấn công kẻ địch");
        taskQueue.Enqueue("Thu thập vật phẩm");

        StartCoroutine(ProcessTasks());
    }

    IEnumerator ProcessTasks()
    {
        while (taskQueue.Count > 0)
        {
            string currentTask = taskQueue.Dequeue();
            Debug.Log("Thực hiện nhiệm vụ: " + currentTask);
            yield return new WaitForSeconds(2); // Giả lập thời gian thực hiện nhiệm vụ
        }

        Debug.Log("Hoàn thành tất cả nhiệm vụ!");
    }
}

```
📌 Giải thích:
- Danh sách nhiệm vụ được quản lý bằng Queue.
- Dùng Coroutine để thực hiện nhiệm vụ tuần tự (mỗi nhiệm vụ mất 2 giây).
- Khi taskQueue.Count == 0, tất cả nhiệm vụ đã hoàn thành.

### Khi nào ta nên dùng Queue trong unity?
- Quản lý danh sách nhiệm vụ tuần tự (như AI, NPC thực hiện hành động theo thứ tự).
- Xử lý hàng đợi sự kiện (như log chat, thông báo hệ thống).
- Tạo hệ thống Pooling (quản lý đối tượng tái sử dụng để tối ưu hiệu năng).
- Tính toán đường đi AI (bằng BFS - Breadth First Search).

## 5. `Stack`
- Stack là cấu trúc dữ liệu ngăn xếp (LIFO - Last In First Out), tức là phần tử nào được thêm vào sau sẽ được lấy ra trước. Nó thích hợp sử dụng để quản lý các trạng thái, hệ thống Undo/Redo hoặc giải quyết bài toán đệ quy.
```csharp
Stack<int> stack = new Stack<int>(); // Khởi tạo stack chứa số nguyên

void Push(){
    stack.Push(10);
    stack.Push(20);
    stack.Push(30);
}

int Pop(){
    int lastElement = stack.Pop(); // Lấy ra 30 và xóa khỏi stack
    return lastElement;
}

int Peek(){
    int topElement = stack.Peek(); // Xem phần tử trên cùng nhưng không xóa
    return topElement;
}
```
### Ví dụ về hệ thống Undo/Redo bằng Stack
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class UndoRedoManager : MonoBehaviour
{
    Stack<string> undoStack = new Stack<string>();
    Stack<string> redoStack = new Stack<string>();

    void Start()
    {
        PerformAction("Di chuyển lên");
        PerformAction("Di chuyển phải");
        PerformAction("Tấn công");

        Undo();
        Undo();
        Redo();
    }

    void PerformAction(string action)
    {
        undoStack.Push(action);
        redoStack.Clear(); // Khi thực hiện hành động mới, không thể Redo nữa
        Debug.Log("Hành động: " + action);
    }

    void Undo()
    {
        if (undoStack.Count > 0)
        {
            string lastAction = undoStack.Pop();
            redoStack.Push(lastAction);
            Debug.Log("Undo: " + lastAction);
        }
        else
        {
            Debug.Log("Không thể Undo!");
        }
    }

    void Redo()
    {
        if (redoStack.Count > 0)
        {
            string lastUndoneAction = redoStack.Pop();
            undoStack.Push(lastUndoneAction);
            Debug.Log("Redo: " + lastUndoneAction);
        }
        else
        {
            Debug.Log("Không thể Redo!");
        }
    }
}

```
📌 Giải thích:
- undoStack lưu trữ các hành động đã thực hiện.
- redoStack lưu trữ các hành động đã Undo để có thể Redo.
- Khi thực hiện hành động mới, redoStack bị xóa để tránh Redo lỗi.

### So sánh Stack và Queue

| Đặc điểm  | `Stack<T>` (LIFO) | `Queue<T>` (FIFO) |
|-----------|------------------|------------------|
| **Cách hoạt động** | Lấy phần tử cuối cùng trước | Lấy phần tử đầu tiên trước |
| **Phương thức chính** | `Push()`, `Pop()`, `Peek()` | `Enqueue()`, `Dequeue()`, `Peek()` |
| **Ứng dụng** | Hệ thống Undo/Redo, đệ quy, duyệt cây | Quản lý hàng đợi, xử lý nhiệm vụ tuần tự |

### Khi nào nên dùng Stack trong unity?
- Hệ thống Undo/Redo (ví dụ: chỉnh sửa bản đồ, hành động nhân vật).
- Duyệt cây đệ quy (DFS - Depth First Search).
- Hệ thống xử lý trạng thái nhân vật (State Machine).
- Quản lý lịch sử hành động của AI.
## 6. `LinkedList`
