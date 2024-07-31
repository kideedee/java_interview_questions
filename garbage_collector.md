
Garbage Collector (GC) trong Java là một thành phần quan trọng của Java Virtual Machine (JVM) chịu trách nhiệm quản lý bộ nhớ tự động. Nó giúp giải phóng bộ nhớ không còn được sử dụng nữa để làm cho bộ nhớ hiệu quả và tránh tình trạng rò rỉ bộ nhớ. Dưới đây là một cái nhìn tổng quan về Garbage Collector trong Java:

### 1. **Khái Niệm Cơ Bản**

- **Garbage Collection (GC)**: Là quá trình tự động thu dọn các đối tượng không còn được tham chiếu bởi chương trình và giải phóng bộ nhớ mà các đối tượng đó chiếm dụng.
- **Mục Đích**: Đảm bảo rằng bộ nhớ heap không bị lấp đầy bởi các đối tượng không còn cần thiết, giúp giảm nguy cơ của OutOfMemoryError và cải thiện hiệu suất ứng dụng.

### 2. **Các Loại Garbage Collectors**

Java cung cấp nhiều loại Garbage Collectors khác nhau, mỗi loại có các ưu và nhược điểm riêng. Các loại chính bao gồm:

- **Serial Garbage Collector**:
  - **Đặc Điểm**: Sử dụng một luồng đơn để thu dọn rác và phân phối bộ nhớ. Thích hợp cho các ứng dụng nhỏ hoặc đơn luồng.
  - **Tham Số JVM**: `-XX:+UseSerialGC`
  
- **Parallel Garbage Collector (Throughput Collector)**:
  - **Đặc Điểm**: Sử dụng nhiều luồng để thu dọn rác, cải thiện hiệu suất cho các ứng dụng đa luồng và có yêu cầu cao về hiệu suất.
  - **Tham Số JVM**: `-XX:+UseParallelGC`

- **Concurrent Mark-Sweep (CMS) Garbage Collector**:
  - **Đặc Điểm**: Thiết kế để giảm thiểu thời gian dừng (pause time) bằng cách thực hiện thu dọn rác đồng thời với các luồng của ứng dụng. Thích hợp cho các ứng dụng yêu cầu thời gian phản hồi thấp.
  - **Tham Số JVM**: `-XX:+UseConcMarkSweepGC`
  
- **G1 Garbage Collector (Garbage-First)**:
  - **Đặc Điểm**: Chia bộ nhớ heap thành các khu vực nhỏ và thu dọn rác theo các khu vực này. Được thiết kế để cân bằng giữa việc giảm thời gian dừng và cải thiện hiệu suất tổng thể.
  - **Tham Số JVM**: `-XX:+UseG1GC`
  
- **ZGC (Z Garbage Collector)**:
  - **Đặc Điểm**: Một GC có khả năng tối ưu hóa thời gian dừng, hoạt động gần như không có thời gian dừng (low latency) và phù hợp cho các ứng dụng có yêu cầu cao về thời gian phản hồi.
  - **Tham Số JVM**: `-XX:+UseZGC`

- **Shenandoah Garbage Collector**:
  - **Đặc Điểm**: Tương tự như ZGC, cung cấp khả năng giảm thiểu thời gian dừng gần như bằng không, phù hợp cho các ứng dụng lớn và yêu cầu thấp về thời gian dừng.
  - **Tham Số JVM**: `-XX:+UseShenandoahGC`

### 3. **Quá Trình Garbage Collection**

- **Young Generation Collection (Minor GC)**:
  - **Đặc Điểm**: Thu dọn rác trong Young Generation (Eden Space và Survivor Spaces). Thường xảy ra thường xuyên và nhanh hơn vì chỉ có các đối tượng ngắn hạn được kiểm tra.
  - **Quá Trình**: Các đối tượng mới được tạo trong Eden Space. Khi Eden đầy, các đối tượng sống sót được chuyển đến Survivor Spaces. Nếu một đối tượng sống đủ lâu, nó sẽ được chuyển đến Old Generation.

- **Old Generation Collection (Major GC / Full GC)**:
  - **Đặc Điểm**: Thu dọn rác trong Old Generation và đôi khi trong cả Young Generation. Xảy ra ít thường xuyên hơn nhưng có thể tốn thời gian hơn.
  - **Quá Trình**: Tìm và thu dọn các đối tượng lâu dài không còn được sử dụng, và có thể làm sạch cả Young Generation để giảm áp lực bộ nhớ.

### 4. **Tối Ưu Hóa Garbage Collection**

- **Tuning GC Parameters**: Các tham số như `-Xms`, `-Xmx`, `-XX:NewSize`, `-XX:MaxNewSize` có thể được cấu hình để điều chỉnh kích thước của các khu vực heap và cải thiện hiệu suất GC.
- **Monitoring and Profiling**: Sử dụng các công cụ như VisualVM, Java Mission Control, hoặc các công cụ khác để giám sát và phân tích hiệu suất GC và bộ nhớ.

### 5. **Ví Dụ Cấu Hình GC**

Dưới đây là ví dụ về cách cấu hình GC trong JVM:

```sh
java -Xms1g -Xmx4g -XX:+UseG1GC -XX:MaxGCPauseMillis=200 -jar myapplication.jar
```

- `-Xms1g`: Đặt kích thước bộ nhớ heap ban đầu là 1 GB.
- `-Xmx4g`: Đặt kích thước bộ nhớ heap tối đa là 4 GB.
- `-XX:+UseG1GC`: Sử dụng Garbage-First (G1) GC.
- `-XX:MaxGCPauseMillis=200`: Cố gắng giữ thời gian dừng GC dưới 200 ms.

### 6. **Lưu Ý và Thực Hành Tốt Nhất**

- **Giám Sát và Tinh Chỉnh**: Theo dõi hiệu suất và điều chỉnh cấu hình GC theo yêu cầu của ứng dụng để tối ưu hóa hiệu suất.
- **Sự Cân Bằng**: Lựa chọn GC phù hợp dựa trên yêu cầu của ứng dụng, chẳng hạn như thời gian dừng thấp (low latency) hay hiệu suất throughput cao.
- **Cập Nhật và Nâng Cấp**: Các phiên bản mới của JVM thường cung cấp cải tiến về GC, vì vậy việc cập nhật phiên bản JVM có thể mang lại lợi ích về hiệu suất.

Tóm lại, Garbage Collector trong Java là một phần quan trọng của hệ thống quản lý bộ nhớ, giúp đảm bảo rằng bộ nhớ được sử dụng hiệu quả và các đối tượng không còn cần thiết được thu dọn kịp thời. Hiểu và tối ưu hóa GC là chìa khóa để duy trì hiệu suất và ổn định của ứng dụng Java.