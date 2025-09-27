# Hướng dẫn test các chức năng bằng Postman

## 1. Đăng ký tài khoản
- **Phương thức:** POST
- **URL:** http://localhost:3000/auth/register
- **Body:** (x-www-form-urlencoded hoặc JSON)
  - `username`: tên đăng nhập
  - `password`: mật khẩu
  - `email`: email

**Kết quả mong đợi:**
- Status: 201 Created hoặc 302 Redirect
- Thông báo đăng ký thành công
- [Demo](./public/results/register.png)
- Check cookie user: [Demo](./public/results/check_user.png)
## 2. Đăng nhập
- **Phương thức:** POST
- **URL:** http://localhost:3000/auth/login
- **Body:** (x-www-form-urlencoded hoặc JSON)
  - `username`: tên đăng nhập
  - `password`: mật khẩu

**Kết quả mong đợi:**
- Status: 200 OK hoặc 302 Redirect
- Có session cookie được thiết lập
- Chuyển hướng đến trang profile hoặc trả về thông tin user
- [Demo](./public/results/login.png)
- Check cookie session: [Demo](./public/results/check_session.png)
## 3. Xem thông tin cá nhân (profile)
- **Phương thức:** GET
- **URL:** http://localhost:3000/auth/profile
- **Yêu cầu:** Đã đăng nhập (có cookie hoặc token nếu sử dụng)
**Headers cần thiết:**
- Cookie: session cookie từ bước đăng nhập

**Kết quả mong đợi:**
- Status: 200 OK
- Trả về thông tin user hoặc trang profile
- [Demo](./public/results/profile.png)

**Nếu chưa đăng nhập:**
- Status: 401 Unauthorized hoặc 302 Redirect đến trang login

## 4. Đăng xuất
- **Phương thức:** GET
- **URL:** http://localhost:3000/auth/logout

**Yêu cầu:** Đã đăng nhập (có session cookie)

**Kết quả mong đợi:**
- Status: 200 OK hoặc 302 Redirect
- Session cookie bị xóa
- Chuyển hướng đến trang login hoặc home
- [Demo](./public/results/logout.png)
---

## Cấu hình Postman

### Bật Cookie cho Session
1. Trong Postman, vào **Settings** (biểu tượng bánh răng)
2. Tại tab **General**, bật **Automatically follow redirects**
3. Tại tab **Cookies**, bật **Capture cookies automatically**

**Lưu ý:**
- Nếu sử dụng session/cookie, cần bật "Enable cookie" trong Postman để lưu trạng thái đăng nhập.
- Nếu API trả về token, cần thêm token vào header cho các request cần xác thực.
- Test theo thứ tự: Đăng ký → Đăng nhập → Xem profile → Đăng xuất

## Các trường hợp test khác

### Test đăng nhập sai thông tin
```json
{
  "username": "wronguser",
  "password": "wrongpass"
}
```
**Kết quả:** Status 401 hoặc thông báo lỗi

### Test truy cập profile khi chưa đăng nhập
**URL:** GET http://localhost:3000/auth/profile (không có cookie)
**Kết quả:** Status 401 hoặc redirect đến login

## Tham khảo
- Đường dẫn các API có thể thay đổi tùy cấu hình trong file `routes/auth.js` và `app.js`.
- Nếu gặp lỗi, kiểm tra lại server đã chạy chưa bằng lệnh: `node app.js`.
- Có thể sử dụng Postman Collection runner để chạy tự động tất cả test cases.
