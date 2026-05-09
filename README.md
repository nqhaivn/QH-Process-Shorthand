# Giới thiệu

Trong môi trường làm việc hiện đại, quy trình là xương sống của mọi tổ chức. Tuy nhiên, việc mô tả quy trình thường đối mặt với một nghịch lý: viết quá chi tiết thì dài dòng, khó đọc; viết quá ngắn gọn thì thiếu chính xác, khó hiểu.

**Ngôn ngữ Tốc ký Quy trình QH** (QH Process Shorthand, hay QHPS) ra đời để giải quyết nghịch lý này.

QHPS là một Domain-Specific Language (DSL) - ngôn ngữ chuyên biệt cho lĩnh vực mô tả quy trình. Lấy cảm hứng từ các ký hiệu chuẩn ISO 5807 nhưng được tối giản hóa triệt để, cho phép bất kỳ ai cũng có thể viết một quy trình hoàn chỉnh trong vài phút bằng giấy bút hoặc gõ văn bản, đồng thời đọc và hiểu luồng quy trình ngay lập tức.

QHPS không nhằm thay thế BPMN 2.0 hay các chuẩn quy trình chuyên nghiệp khác, mà là một lựa chọn nhẹ, dễ học hơn, phù hợp với nhịp độ làm việc hàng ngày, đặc biệt khi bạn cần ghi lại quy trình nhanh mà không muốn xử lý quá nhiều ký hiệu phức tạp.

# Ví dụ

Để dễ hình dung, hãy so sánh một quy trình thực tế: "Xử lý đơn nghỉ phép của nhân viên". Được viết bằng hai ngôn ngữ cho sơ đồ nổi tiếng nhất và nhiều người sử dụng nhất hiện nay là Mermaid và PlantUML với QHPS.

## 1. Quy trình được viết bằng Mermaid

```
flowchart TD
    %% Định nghĩa các node
    Start([Bắt đầu])
    End1([Từ chối])
    End2([Hoàn tất])

    NV1["Gửi đơn nghỉ phép"]
    TP1["Xem xét đơn"]
    TP2{"Đơn hợp lệ?"}
    HC1["Cập nhật vào hệ thống"]
    HC2["Lưu hồ sơ"]

    %% Luồng
    Start --> NV1
    NV1 --> TP1
    TP1 --> TP2

    TP2 -- Không --> End1
    TP2 -- Có --> HC1
    HC1 --> HC2
    HC2 --> End2
```

## 2. Quy trình được viết bằng PlantUML

```
@startuml
|Nhân viên|
start
:Gửi đơn nghỉ phép;
|Trưởng phòng|
:Xem xét đơn;
if (Đơn hợp lệ?) then (Không)
    stop
else (Có)
    |Hành chính|
    :Cập nhật vào hệ thống;
    :Lưu hồ sơ;
    stop
endif
@enduml
```

## 3. Quy trình được viết bằng QHPS

```
@1 Nhân viên (NV)
@2 Trưởng phòng (TP)
@3 Hành chính (HC)

@1 G1
1.1 Gửi đơn nghỉ phép -> 2.1

@2 G2
2.1 Xem xét đơn
2.2 ? Đơn hợp lệ?
    'Không' -> ./ Từ chối
    'Có' -> 3.1

@3 G3
3.1 Cập nhật vào hệ thống
3.2 Lưu hồ sơ -> ./ Hoàn tất
```

## 4. Sơ đồ kết quả

```
     Nhân viên (NV)         Trưởng phòng (TP)    Hành chính (HC)
┌─────────────────────┐
│ Gửi đơn nghỉ phép   │
└──────────┬──────────┘
           └───────────────────────┐                
                                   │
                          ┌────────▼─────────┐
                          │ Xem xét đơn      │
                          └────────┬─────────┘
                                   │
                          /────────▼─────────\
                          │ Đơn hợp lệ?      │
                          \──────┬──────┬────/
                                 │      │Có
                                 │      └──────────────┐
                          Không  │                     │ 
                          ┌──────▼──┐           ┌──────▼──────┐         
                          │ Từ chối │           │ Cập nhật vào│            
                          └─────────┘           │ hệ thống    │            
                                                └──────┬──────┘
                                                       │
                                                ┌──────▼──────┐
                                                │ Lưu hồ sơ   │
                                                └──────┬──────┘
                                                       │
                                                ┌──────▼──────┐
                                                │ Hoàn tất    │
                                                └─────────────┘
```
