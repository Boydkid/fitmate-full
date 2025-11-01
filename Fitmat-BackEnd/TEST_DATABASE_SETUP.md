# Test Database Setup Guide

## วิธีการตั้งค่า Test Database

### วิธีที่ 1: ใช้ MySQL Test Database (แนะนำ)

1. **สร้าง Test Database:**
```sql
CREATE DATABASE fitmat_test;
```

2. **สร้างไฟล์ `.env.test` ใน `Fitmat-BackEnd/`:**
```env
DATABASE_URL="mysql://root:yourpassword@localhost:3306/fitmat_test"
JWT_SECRET="test-secret-key-for-testing-only"
```

3. **Run Prisma migrations สำหรับ test database:**
```bash
cd Fitmat-BackEnd

# Set DATABASE_URL to test database
export DATABASE_URL="mysql://root:yourpassword@localhost:3306/fitmat_test"

# Run migrations
npx prisma migrate deploy

# หรือ push schema (สำหรับ testing)
npx prisma db push
```

### วิธีที่ 2: ใช้ Production Database (ไม่แนะนำ)

**⚠️ คำเตือน:** วิธีนี้จะเขียนข้อมูลลงใน production database และอาจทำให้ข้อมูลเสียหาย

```env
DATABASE_URL="mysql://root:yourpassword@localhost:3306/fitmat_production"
```

### วิธีที่ 3: ใช้ Separate Test Environment Variable

1. สร้าง `.env.test`:**
```env
TEST_DATABASE_URL="mysql://root:password@localhost:3306/fitmat_test"
JWT_SECRET="test-secret-key"
```

2. Setup จะใช้ `TEST_DATABASE_URL` แทน `DATABASE_URL` โดยอัตโนมัติ

## Running Tests

หลังจากตั้งค่า `.env.test` แล้ว:

```bash
cd Fitmat-BackEnd
npm test
```

## Test Database Best Practices

1. **ใช้ Database แยก:** อย่าใช้ production database สำหรับ testing
2. **Cleanup:** ใช้ `beforeEach` หรือ `afterEach` เพื่อ cleanup test data
3. **Transactions:** ใช้ transactions และ rollback หลังแต่ละ test (ถ้าเป็นไปได้)
4. **Isolation:** แต่ละ test ควรเป็นอิสระจากกัน

## Troubleshooting

### Error: "the URL must start with the protocol `mysql://`"

**ปัญหา:** DATABASE_URL ไม่มี format ที่ถูกต้อง

**แก้ไข:**
- ตรวจสอบว่า `.env.test` มี DATABASE_URL ที่ถูกต้อง
- Format: `mysql://username:password@host:port/database_name`

### Error: "Cannot connect to database"

**ปัญหา:** Database server ไม่ได้รัน หรือ credentials ไม่ถูกต้อง

**แก้ไข:**
- ตรวจสอบว่า MySQL server กำลังรันอยู่
- ตรวจสอบ username, password, host, และ port
- ตรวจสอบว่า database ถูกสร้างแล้ว

### Tests fail with database errors

**ปัญหา:** Schema ยังไม่ได้ migrate ไปยัง test database

**แก้ไข:**
```bash
# Set DATABASE_URL to test database
export DATABASE_URL="mysql://root:password@localhost:3306/fitmat_test"

# Generate Prisma Client
npx prisma generate

# Push schema to database
npx prisma db push
```

## Example .env.test

```env
# Test Database Configuration
DATABASE_URL="mysql://root:your_password@localhost:3306/fitmat_test"
JWT_SECRET="test-secret-key-for-testing-only-do-not-use-in-production"

# Optional: Email config for testing
# EMAIL_USER="test@gmail.com"
# EMAIL_PASSWORD="test-app-password"
```

## Notes

- `.env.test` ควรถูกเพิ่มใน `.gitignore` (ไม่ commit credentials)
- ใช้ `.env.test.example` เป็น template
- สำหรับ CI/CD: ใช้ environment variables แทน `.env.test` file


