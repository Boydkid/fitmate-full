# คำแนะนำการตั้งค่า Test Database

## 📝 ขั้นตอนการตั้งค่า Test Database

### 1. สร้าง Test Database

เปิด MySQL และรันคำสั่ง:

```sql
CREATE DATABASE fitmate_db_test;
```

### 2. ไฟล์ `.env.test` ถูกสร้างแล้ว

ไฟล์ `.env.test` ถูกสร้างใน `Fitmat-BackEnd/` แล้ว โดยใช้:
- Database: `fitmate_db_test` (แยกจาก production)
- Username: `root`
- Password: `123456` (เหมือนกับ .env)
- Port: `3306`

### 3. Run Prisma Migrations สำหรับ Test Database

รันคำสั่งต่อไปนี้ใน terminal:

```bash
cd Fitmat-BackEnd

# ตั้งค่า DATABASE_URL ชั่วคราว
set DATABASE_URL=mysql://root:123456@localhost:3306/fitmate_db_test

# Generate Prisma Client
npx prisma generate

# Push schema ไปยัง test database
npx prisma db push
```

**สำหรับ Linux/Mac:**
```bash
export DATABASE_URL="mysql://root:123456@localhost:3306/fitmate_db_test"
npx prisma generate
npx prisma db push
```

### 4. รัน Tests

หลังจาก setup database แล้ว:

```bash
npm test
```

## ✅ Checklist

- [x] ไฟล์ `.env.test` ถูกสร้างแล้ว
- [ ] สร้าง test database (`fitmate_db_test`)
- [ ] Run Prisma migrations (`npx prisma db push`)
- [ ] รัน tests (`npm test`)

## 📋 ไฟล์ที่สร้างแล้ว

- ✅ `.env.test` - Test environment variables
- ✅ `.env.test.example` - Template สำหรับ reference
- ✅ `TEST_DATABASE_SETUP.md` - รายละเอียดการตั้งค่า
- ✅ `TESTING_QUICK_START.md` - Quick start guide

## ⚠️ หมายเหตุ

1. **Database แยก:** `fitmate_db_test` แยกจาก `fitmate_db` (production)
2. **ไม่ commit .env.test:** ไฟล์นี้ควรอยู่ใน `.gitignore` (ไม่ commit credentials)
3. **Test Data:** Test database จะถูกใช้สำหรับ tests เท่านั้น

## 🔍 ตรวจสอบ

ตรวจสอบว่า test database ถูกสร้างแล้ว:

```sql
SHOW DATABASES LIKE 'fitmate_db_test';
```

ควรเห็น `fitmate_db_test` ในรายการ

## 🚀 Ready to Test!

หลังจาก setup database แล้ว รัน:

```bash
npm test
```

Tests ทั้งหมดควรผ่าน! ✅


