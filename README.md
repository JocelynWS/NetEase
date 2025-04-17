# SALARY-API: Ứng dụng tính lương Gross sang Net

Ứng dụng này cung cấp một API đơn giản để chuyển đổi lương Gross sang lương Net dựa trên số người phụ thuộc. Nó bao gồm hai endpoint chính:

* **GET /calculate:** Tính lương Net từ lương Gross và số người phụ thuộc được truyền qua query parameters.
* **POST /upload:** Tính lương Net cho nhiều người từ một file Excel hoặc CSV được tải lên.

## Cấu trúc thư mục

salary-api/
├── static/             # Chứa file frontend (index.html)
│   └── index.html
├── cmd/                # Chứa entry point của ứng dụng
│   └── main.go
├── internal/
│   ├── control/        # Chứa logic nghiệp vụ (tính lương)
│   │   └── calculateSalary.go
│   ├── model/          # Chứa định nghĩa struct dữ liệu (nếu có)
│   │   └── salary.go
│   └── routes/         # Chứa định nghĩa các API endpoints
│       └── server.go
├── go.mod
├── go.sum
└── README.md          # File này


## Yêu cầu

* **Go:** Phiên bản 1.18 trở lên (để hỗ trợ `go.mod`).
* **Trình duyệt web:** Để tương tác với giao diện người dùng (nếu bạn đã thêm).

## Hướng dẫn chạy

1.  **Clone repository (nếu bạn có):**
    ```bash
    git clone <your_repository_url>
    cd salary-api
    ```

2.  **Tải các dependencies:**
    ```bash
    go mod tidy
    go mod download
    ```

3.  **Chạy backend server:**
    ```bash
    go run cmd/main.go
    ```
    Server sẽ khởi chạy và lắng nghe trên cổng `http://localhost:8081`.

4.  **Mở giao diện người dùng (frontend):**
    Mở trình duyệt web của bạn và truy cập `http://localhost:8081/frontend`.

## Sử dụng API

### 1. Tính lương Net (API GET)

* **Endpoint:** `/calculate`
* **Phương thức:** `GET`
* **Tham số (query parameters):**
    * `gross`: Số lương Gross (bắt buộc).
    * `dependents`: Số người phụ thuộc (bắt buộc, có thể là 0).
* **Ví dụ:**
    `http://localhost:8081/calculate?gross=15000000&dependents=1`
* **Phản hồi (JSON):**
    ```json
    {
        "gross_salary": 15000000,
        "dependents": 1,
        "net_salary": 13615000
    }
    ```
    (Lưu ý: Giá trị `net_salary` là một ví dụ và sẽ phụ thuộc vào logic tính toán thuế của bạn).

### 2. Tính lương Net từ file (API POST)

* **Endpoint:** `/upload`
* **Phương thức:** `POST`
* **Headers:** `Content-Type: multipart/form-data`
* **Body (form-data):**
    * `file`: Chọn file Excel (`.xlsx`) hoặc CSV (`.csv`) chứa dữ liệu lương. File dự kiến có các cột theo thứ tự: `Tên`, `Lương Gross`, `Số người phụ thuộc` (header có thể có hoặc không, code hiện tại bỏ qua header).
* **Ví dụ (sử dụng `curl`):**
    ```bash
    curl -X POST -F "file=@path/to/your/salary_data.xlsx" http://localhost:8081/upload
    ```
    hoặc
    ```bash
    curl -X POST -F "file=@path/to/your/salary_data.csv" http://localhost:8081/upload
    ```
* **Phản hồi (JSON):**
    ```json
    {
        "salaries": [
            {
                "name": "Nhân viên A",
                "gross": 10000000,
                "dependents": 0,
                "net": 9250000
            },
            {
                "name": "Nhân viên B",
                "gross": 20000000,
                "dependents": 2,
                "net": 17470000
            },
            // ... các nhân viên khác
        ]
    }
    ```
    (Lưu ý: Giá trị `net` là một ví dụ và sẽ phụ thuộc vào logic tính toán thuế của bạn).

