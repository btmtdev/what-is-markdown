# บันทึกการเปลี่ยนแปลง

การเปลี่ยนแปลงที่สำคัญทั้งหมดจะถูกบันทึกไว้ในไฟล์นี้

รูปแบบอ้างอิงจาก [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
ใช้ [Semantic Versioning](https://semver.org/spec/v2.0.0.html)

## [Unreleased]

### เพิ่มเติม
- รองรับ Dark mode

## [1.2.0] - 2026-05-20

### เพิ่มเติม
- ระบบยืนยันตัวตนด้วย OAuth2
- Rate limiting บน API endpoints

### เปลี่ยนแปลง
- อัปเกรด database driver เป็น v5

### แก้ไข
- Memory leak ใน WebSocket handler

## [1.1.0] - 2026-04-15

### เพิ่มเติม
- ฟังก์ชันค้นหา
- ส่งออกเป็น CSV

### เลิกใช้
- Legacy `/api/v1/users` endpoint (ใช้ `/api/v2/users` แทน)

## [1.0.0] - 2026-03-01

### เพิ่มเติม
- เวอร์ชันแรก
- CRUD สำหรับผู้ใช้
- REST API
- หน้า Dashboard เบื้องต้น

[Unreleased]: https://github.com/username/my-project/compare/v1.2.0...HEAD
[1.2.0]: https://github.com/username/my-project/compare/v1.1.0...v1.2.0
[1.1.0]: https://github.com/username/my-project/compare/v1.0.0...v1.1.0
[1.0.0]: https://github.com/username/my-project/releases/tag/v1.0.0
