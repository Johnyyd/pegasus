# Quy trình tấn công của mã độc gián điệp Pegasus

Sơ đồ dưới đây mô tả vòng đời của một cuộc tấn công zero-click tiêu chuẩn, từ giai đoạn định vị mục tiêu cho đến cơ chế tự hủy để xóa dấu vết.

## Sơ đồ luồng hoạt động (Flowchart)

```mermaid
graph TD
    %% Định dạng các Node
    classDef attacker fill:#d32f2f,stroke:#b71c1c,stroke-width:2px,color:#fff;
    classDef server fill:#1976d2,stroke:#0d47a1,stroke-width:2px,color:#fff;
    classDef legitimate fill:#9e9e9e,stroke:#616161,stroke-width:2px,color:#fff;
    classDef target fill:#388e3c,stroke:#1b5e20,stroke-width:2px,color:#fff;
    classDef action fill:#f57c00,stroke:#e65100,stroke-width:2px,color:#fff;
    classDef destroy fill:#424242,stroke:#212121,stroke-width:2px,color:#fff;

    A[Attacker Dashboard]:::attacker -->|Bước 1: Lệnh nhắm mục tiêu| B(Infection Server):::server
    B -->|Bước 2: Nạp Exploit Chain| C[Máy chủ Trung gian hợp lệ <br> Apple APNs / WhatsApp]:::legitimate
    C -->|Bước 3: Đẩy Push/Message vô hình| D[Thiết bị Mục tiêu]:::target
    
    D -.->|Bước 4: HĐH tự động render| E(Thoát Hộp cát & Chạy mã Trinh sát):::action
    E -->|Bước 5a: Validation| F{Kiểm tra Honeypot / Máy ảo?}:::action
    
    F -->|Môi trường bị theo dõi| K[Kích hoạt Kill Switch]:::destroy
    F -->|Môi trường thực - An toàn| G(Kernel Exploit / Chiếm Root):::action
    
    G -->|Bước 6| H[Bung Full Module lên RAM]:::action
    H -->|Bước 7: System Hooking| I(Tuồn dữ liệu qua PATN):::server
    I -->|Bước 8: Giám sát vòng đời| J{Kiểm tra trạng thái thiết bị?}:::action
    
    J -->|Bước 9a: Ổn định| H
    J -->|Bước 9b: Khởi động lại máy - Mất RAM| L[Máy chủ tự động Tái nhiễm]:::server
    L -.->|Bắn lại Payload ngầm| C
    
    J -->|Bước 9c: Bất thường - SIM/Offline>60 ngày| K
    A -.->|Bước 10: Nút tự hủy thủ công từ xa| K
