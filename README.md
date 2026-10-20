# Employee Management & K-Means Salary Analysis App
## ระบบจัดการข้อมูลพนักงาน (CRUD) และวิเคราะห์กลุ่มเงินเดือนด้วย K-Means (Tier 1, Tier 2, Tier 3)

ระบบ Full-Stack ที่พัฒนาด้วย **Express.js (Backend)** และ **React Native (Frontend)** พร้อมระบบยืนยันตัวตนสำหรับผู้ดูแลระบบ (Admin Login) และการวิเคราะห์กลุ่มเงินเดือนด้วย Machine Learning อัลกอริทึม **K-Means Clustering** ผ่าน **JSON API** โดยเก็บข้อมูลในหน่วยความจำ (In-Memory Database)

---

## 🚀 ข้อมูลการเข้าสู่ระบบ (Admin Login)

- **Username**: `admin`
- **Password**: `admin123` (หรือ `admin`)

---

## 📂 โครงสร้างโปรเจกต์ (Project Structure)

```text
employee-management-app/
├── backend/
│   ├── data/
│   │   └── employeesData.js       # ข้อมูล In-Memory จำลองพนักงานเริ่มต้น 7 คน (เกินเกณฑ์ 5 คน)
│   ├── routes/
│   │   ├── authRoutes.js          # API สำหรับ Login ตรวจสอบ admin / admin123
│   │   └── employeeRoutes.js      # API สำหรับ CRUD พนักงาน และ Endpoint วิเคราะห์ K-Means
│   ├── utils/
│   │   └── kmeans.js              # อัลกอริทึม K-Means 1D แบ่งเงินเดือนเป็น 3 Tiers
│   ├── package.json
│   ├── server.js                  # Express.js Server (Port 5000)
│   └── test-api.js                # สคริปต์ Automated Test ทดสอบ CRUD และ K-Means 100%
│
└── frontend/
    ├── App.js                     # Root Component จัดการ State Login และ Tab Navigation
    ├── components/
    │   └── EmployeeModal.js       # Modal เพิ่ม/แก้ไขข้อมูลพนักงาน (Form Validation)
    ├── screens/
    │   ├── LoginScreen.js         # หน้าจอล็อกอินสำหรับ Admin
    │   ├── EmployeeListScreen.js  # หน้ารายการพนักงาน, ค้นหา, กรอง, สลับสถานะ, ลบ, แก้ไข
    │   └── KMeansAnalysisScreen.js# หน้าแสดงผลวิเคราะห์ K-Means 3 Tiers, กราฟเปรียบเทียบ และ JSON Viewer
    ├── services/
    │   └── api.js                 # API Client เชื่อมต่อ Express Backend ผ่าน JSON
    ├── package.json
    └── app.json
```

---

## 🛠️ วิธีการรันโปรเจกต์ (How to Run)

### 1. รัน Backend (Express.js)

เปิด Terminal ที่ 1:
```bash
cd backend
npm install   # หากยังไม่ได้ติดตั้ง
npm start     # หรือ node server.js
```
> เซิร์ฟเวอร์จะเริ่มต้นที่: **`http://localhost:5000`**

### 2. รัน Frontend (React Native / Expo)

เปิด Terminal ที่ 2:
```bash
cd frontend
npm run web   # เพื่อเปิดใช้งานบน Web Browser ทันที
```
> หรือรัน `npx expo start` เพื่อสแกน QR Code ผ่านแอป **Expo Go** บนโทรศัพท์มือถือ (iOS / Android)

---

## 📊 การวิเคราะห์เงินเดือนด้วย K-Means (3 Tiers)

อัลกอริทึมคำนวณการจัดกลุ่ม 1 มิติ (1D K-Means Clustering) โดยแบ่งออกเป็น 3 กลุ่ม ($k = 3$):
1. **Tier 1 (High Salary)**: เงินเดือนระดับสูง (Senior / Executive / Specialist)
2. **Tier 2 (Mid Salary)**: เงินเดือนระดับกลาง (Mid-Level / Professional)
3. **Tier 3 (Entry Salary)**: เงินเดือนระดับเริ่มต้น (Junior / Support / Entry)

### แต่ละ Tier แสดงข้อมูล:
- **เงินเดือนเฉลี่ย (Average Salary)**
- **จำนวนพนักงานในกลุ่ม (Employee Count)**
- **ช่วงเงินเดือนต่ำสุด - สูงสุด (Min - Max Salary)**
- **ค่า Centroid ของกลุ่ม**
- **รายชื่อพนักงานทั้งหมดที่อยู่ในกลุ่มนั้น**

---

## 📡 สรุป REST API Endpoints (JSON)

| Method | Endpoint | รายละเอียด |
|---|---|---|
| `POST` | `/api/auth/login` | เข้าสู่ระบบ Admin (รับ `{ username, password }`) |
| `GET` | `/api/employees` | ดึงรายชื่อพนักงานทั้งหมด (รองรับ `?search=` และ `?status=`) |
| `GET` | `/api/employees/:id` | ดึงข้อมูลพนักงานตาม ID |
| `POST` | `/api/employees` | เพิ่มพนักงานใหม่ (Create) |
| `PUT` | `/api/employees/:id` | อัปเดตข้อมูลพนักงาน (Update) |
| `PATCH` | `/api/employees/:id/status` | เปลี่ยนสถานะพนักงาน (Active / Inactive) |
| `DELETE` | `/api/employees/:id` | ลบข้อมูลพนักงาน (Delete) |
| `GET` | `/api/employees/analysis/kmeans` | ผลการวิเคราะห์ K-Means แบ่ง 3 Tiers (JSON Format) |
| `POST` | `/api/employees/reset` | รีเซ็ตข้อมูลกลับเป็นตัวอย่างเริ่มต้น 7 คน |

---

## 🧪 การทดสอบระบบ (Automated Tests)

สามารถรันสคริปต์ทดสอบตรรกะทั้งหมดได้ทันที:
```bash
cd backend
node test-api.js
```
ผลการทดสอบผ่าน 21/21 รายการ (100% Pass)

