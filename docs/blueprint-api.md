# Blueprint API - CRS Microservices System

## 1. auth-service (Port 8081 | Gateway Prefix: /api/auth)
| Method | Endpoint | Mô tả | Quyền truy cập |
| :--- | :--- | :--- | :--- |
| `POST` | `/auth/login` | Đăng nhập, nhận JWT Token | Public |
| `POST` | `/auth/register` | Đăng ký tài khoản mới | Public |

## 2. course-service (Port 8082 | Gateway Prefix: /api/courses)
| Method | Endpoint | Mô tả | Quyền truy cập |
| :--- | :--- | :--- | :--- |
| `GET` | `/courses` | Danh sách môn học (Search, Pagination) | Public |
| `GET` | `/courses/{id}` | Chi tiết 1 môn học | Public |
| `POST` | `/courses` | Thêm môn học mới | ADMIN |
| `PUT` | `/courses/{id}` | Cập nhật môn học | ADMIN |
| `DELETE` | `/courses/{id}` | Xóa môn học | ADMIN |

### Internal API (Chỉ gọi giữa các Microservices, không qua Gateway)
| Method | Endpoint | Mô tả |
| :--- | :--- | :--- |
| `PATCH` | `/internal/courses/{id}/reserve-seat` | Trừ số chỗ còn lại khi đăng ký thành công |
| `PATCH` | `/internal/courses/{id}/release-seat` | Cộng lại số chỗ khi hủy đăng ký |

## 3. registration-service (Port 8083 | Gateway Prefix: /api/registrations)
| Method | Endpoint | Mô tả | Quyền truy cập |
| :--- | :--- | :--- | :--- |
| `POST` | `/registrations` | Đăng ký học phần (Gọi reserve-seat) | STUDENT |
| `GET` | `/registrations/my` | Danh sách học phần đã đăng ký của tôi | STUDENT |
| `DELETE` | `/registrations/{id}` | Hủy đăng ký (Gọi release-seat) | STUDENT / ADMIN |