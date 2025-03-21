# Tổng hợp các kiểu dữ liệu dạng tập hợp
## 1. `Array` và `List`
- Array và List đều là những kiểu dữ liệu được sử dụng nhiều và phổ biến
### 1.1. Array
- Có kích thước cố định
- Truy cập phần tử nhanh (O(1))
- Không có sẵn phương thức thêm/xoá phần tử
```csharp
int[] numbers = new int[3] { 1, 2, 3 };
Debug.Log(numbers[0]); // 1
```
### 1.2. List
- Kích thước linh hoạt
- Thêm/xoá các phần tử dễ dàng
- Tìm kiếm phần tử chậm hơn HashSet.

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
## 3. `Dictionary`
## 4. `Queue`
## 5. `Stack`
## 6. `LinkedList`
