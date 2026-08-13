# Thiết kế Biên giới Service (Service Boundary Design)

## 1. Danh sách Service
| Service | Cổng | Database | Trách nhiệm chính |
| :--- | :--- | :--- | :--- |
| **api-gateway** | 8080 | *(Không DB)* | Điểm vào duy nhất, định tuyến, xác thực sơ bộ, CORS |
| **auth-service** | 8081 | `auth_db` | Quản lý User, Student, đăng nhập, sinh/xác thực JWT |
| **course-service** | 8082 | `course_db` | Quản lý Course, tìm kiếm, phân trang, quản lý số chỗ |
| **registration-service** | 8083 | `registration_db` | Quản lý Registration, gọi sang course-service để đăng ký |

## 2. Nguyên tắc sở hữu dữ liệu (Data Ownership)
* **Database per Service:** Mỗi service sở hữu database riêng, không truy cập trực tiếp DB của nhau.
* **REST/gRPC Inter-service:** Truy xuất dữ liệu chéo bắt buộc thông qua API.
* **Loose Coupling:** `registration-service` chỉ lưu `courseId` (scalar value), không khai báo quan hệ khóa ngoại (FK) vật lý tới bảng môn học.

## 3. Bảng định tuyến Gateway (Dự kiến)
| Route | Forward tới | Ghi chú |
| :--- | :--- | :--- |
| `/api/auth/**` | `http://localhost:8081` | Public (Login/Register) |
| `/api/courses/**` | `http://localhost:8082` | GET Public, POST/PUT/DELETE yêu cầu Role ADMIN |
| `/api/registrations/**` | `http://localhost:8083` | Yêu cầu JWT (STUDENT / ADMIN) |