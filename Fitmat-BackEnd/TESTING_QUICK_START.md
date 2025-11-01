# Testing Quick Start Guide

## 🚀 Quick Start

### 1. ติดตั้ง Dependencies

```bash
cd Fitmat-BackEnd
npm install
```

### 2. ตั้งค่า Test Database

**วิธีที่เร็วที่สุด:**

1. สร้างไฟล์ `.env.test` ใน `Fitmat-BackEnd/`:
```env
DATABASE_URL="mysql://root:yourpassword@localhost:3306/fitmat_test"
JWT_SECRET="test-secret-key"
```

2. สร้าง test database:
```sql
CREATE DATABASE fitmat_test;
```

3. Run Prisma schema:
```bash
# Set DATABASE_URL temporarily
export DATABASE_URL="mysql://root:yourpassword@localhost:3306/fitmat_test"

# Generate and push schema
npx prisma generate
npx prisma db push
```

### 3. รัน Tests

```bash
npm test
```

## 📋 สรุป Test Results

จากที่รัน tests แล้ว:

✅ **Tests ที่ผ่าน (13 tests):**
- JWT utilities (jwt.test.ts) - ไม่ต้องการ database
- Auth validation tests - validation logic

❌ **Tests ที่ล้มเหลว (8 tests):**
- Auth API tests ที่ต้องการ database (register, login)
- Trainer API tests ที่ต้องการ database

## 🔧 แก้ไข Tests ที่ล้มเหลว

### ปัญหา: "the URL must start with the protocol `mysql://`"

**สาเหตุ:** ไม่มี `.env.test` file หรือ DATABASE_URL ไม่ถูกต้อง

**แก้ไข:**
1. สร้าง `.env.test` file (ดูตัวอย่างใน `.env.test.example`)
2. ตั้งค่า DATABASE_URL ให้ถูกต้อง
3. รัน tests อีกครั้ง

## 📝 File Structure

```
Fitmat-BackEnd/
├── .env.test              # Test environment variables (ต้องสร้างเอง)
├── .env.test.example      # Template สำหรับ .env.test
├── jest.config.js         # Jest configuration
├── src/
│   └── __tests__/
│       ├── setup.ts       # Test setup
│       ├── auth.test.ts   # Auth API tests
│       ├── jwt.test.ts    # JWT utility tests
│       └── trainer.test.ts # Trainer API tests
└── TEST_DATABASE_SETUP.md # Database setup guide
```

## ✅ Checklist

- [ ] ติดตั้ง dependencies (`npm install`)
- [ ] สร้าง test database (`CREATE DATABASE fitmat_test;`)
- [ ] สร้าง `.env.test` file
- [ ] Run Prisma migrations (`npx prisma db push`)
- [ ] รัน tests (`npm test`)

## 🆘 Troubleshooting

### Tests fail with database errors?

1. ตรวจสอบว่า MySQL server กำลังรันอยู่
2. ตรวจสอบ credentials ใน `.env.test`
3. ตรวจสอบว่า database `fitmat_test` ถูกสร้างแล้ว
4. รัน `npx prisma db push` อีกครั้ง

### Tests pass but you see warnings?

- Warning เกี่ยวกับ DATABASE_URL: ตรวจสอบ format ของ DATABASE_URL
- Warning เกี่ยวกับ dotenv: ไม่เป็นไร เป็นแค่ informative message

## 📚 Documentation

- `TESTING.md` - คู่มือการใช้งาน testing
- `TEST_DATABASE_SETUP.md` - รายละเอียดการตั้งค่า database
- `TESTING_SETUP.md` - สรุปการตั้งค่าทั้งหมด


