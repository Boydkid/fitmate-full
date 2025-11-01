# 🎨 คู่มือการใช้ Test Report แบบ Visual

## 📊 HTML Test Report

ตอนนี้คุณสามารถดู test results แบบ visual พร้อมรูปภาพและไอคอนได้แล้ว!

## 🚀 วิธีใช้งาน

### Backend Tests

```bash
cd Fitmat-BackEnd

# ติดตั้ง dependencies (ครั้งแรกเท่านั้น)
npm install

# รัน tests พร้อม generate HTML report
npm run test:report
```

หรือถ้าต้องการแค่ coverage:

```bash
npm run test:coverage
```

### Frontend Tests

```bash
cd Fitmat-FrontEnd

# ติดตั้ง dependencies (ครั้งแรกเท่านั้น)
npm install

# รัน tests พร้อม generate HTML report
npm run test:report
```

หรือถ้าต้องการแค่ coverage:

```bash
npm run test:coverage
```

## 📁 ไฟล์ที่สร้าง

หลังจากรัน `npm run test:report` จะมี:

### Backend
- `Fitmat-BackEnd/test-report/test-report.html` - HTML test report
- `Fitmat-BackEnd/coverage/` - Coverage reports (HTML, LCOV, Text)

### Frontend
- `Fitmat-FrontEnd/test-report/test-report.html` - HTML test report
- `Fitmat-FrontEnd/coverage/` - Coverage reports (HTML, LCOV, Text)

## 🎯 วิธีเปิดดู Report

### วิธีที่ 1: Auto-open (แนะนำ)
เมื่อรัน `npm run test:report` ระบบจะเปิด HTML report อัตโนมัติใน browser

### วิธีที่ 2: เปิดด้วยมือ
1. ไปที่ `Fitmat-BackEnd/test-report/test-report.html` (หรือ Frontend)
2. Double-click ไฟล์ หรือเปิดด้วย browser
3. ดูรายละเอียด tests ทั้งหมด

## 📊 สิ่งที่เห็นใน Report

### 1. Summary
- ✅ **Total Tests** - จำนวน tests ทั้งหมด
- ✅ **Passed** - จำนวน tests ที่ผ่าน (สีเขียว)
- ❌ **Failed** - จำนวน tests ที่ล้มเหลว (สีแดง)
- ⏱️ **Duration** - เวลาที่ใช้รัน tests

### 2. Test Suites
รายละเอียดของแต่ละ test suite:
- ชื่อ test suite
- จำนวน tests
- ผลลัพธ์ (ผ่าน/ล้มเหลว)
- เวลาที่ใช้

### 3. Individual Tests
รายละเอียดของแต่ละ test:
- ✅ Test name
- ✅ Status (Passed/Failed)
- ✅ Duration
- ✅ Error messages (ถ้าล้มเหลว)

### 4. Coverage Report
- **Statements** - % ของโค้ดที่ถูก execute
- **Branches** - % ของ branches ที่ถูก test
- **Functions** - % ของ functions ที่ถูก test
- **Lines** - % ของ lines ที่ถูก test

## 🎨 Features

### Visual Indicators
- ✅ **สีเขียว** - Tests ที่ผ่าน
- ❌ **สีแดง** - Tests ที่ล้มเหลว
- ⚠️ **สีเหลือง** - Warnings
- 📊 **Progress bars** - แสดง coverage percentage

### Interactive
- **Click เพื่อดูรายละเอียด** - คลิกที่ test suite เพื่อดูรายละเอียด
- **Filter** - กรองตาม status (Passed/Failed)
- **Search** - ค้นหา test ตามชื่อ

## 📝 Coverage Report

### ดู Coverage แยก
1. ไปที่ `coverage/` folder
2. เปิดไฟล์ `index.html` ใน browser
3. ดู coverage แบบ visual พร้อม:
   - รายละเอียดแต่ละไฟล์
   - บรรทัดที่ถูก/ไม่ได้ test
   - Percentage ของแต่ละไฟล์

## 🔧 การตั้งค่า

### ปรับแต่ง Report

แก้ไข `jest.config.js`:

```javascript
reporters: [
  'default',
  [
    'jest-html-reporters',
    {
      publicPath: './test-report',
      filename: 'test-report.html',
      expand: true,
      openReport: true,  // เปิด browser อัตโนมัติ
      inlineSource: false,
      pageTitle: 'Fitmat Test Report',  // เปลี่ยน title
    },
  ],
],
```

## 📋 Commands

### Backend
```bash
npm test              # รัน tests แบบปกติ
npm run test:watch    # Watch mode
npm run test:coverage # Coverage only
npm run test:report   # HTML report + Coverage
```

### Frontend
```bash
npm test              # รัน tests แบบปกติ
npm run test:watch    # Watch mode
npm run test:coverage # Coverage only
npm run test:report   # HTML report + Coverage
```

## 🎉 ตัวอย่างผลลัพธ์

หลังจากรัน `npm run test:report` คุณจะเห็น:

1. **Terminal Output** - ผลลัพธ์ใน terminal (เหมือนเดิม)
2. **Browser Opens** - เปิด HTML report อัตโนมัติ
3. **Visual Report** - ดู test results แบบ visual
4. **Coverage Report** - ดู coverage แบบ interactive

## ⚠️ หมายเหตุ

- HTML reports ถูกเก็บไว้ใน `test-report/` folder
- Coverage reports ถูกเก็บไว้ใน `coverage/` folder
- ไฟล์เหล่านี้ถูก ignore ใน `.gitignore` (ไม่ commit)
- ทุกครั้งที่รัน tests ใหม่จะ overwrite reports เก่า

## 🎯 Tips

1. **ใช้ HTML Report** - ง่ายต่อการดูและแชร์
2. **ดู Coverage** - ตรวจสอบว่าโค้ดถูก test ครบหรือยัง
3. **Auto-open** - ตั้ง `openReport: true` เพื่อเปิดอัตโนมัติ
4. **Share Reports** - แชร์ HTML report ให้ทีมดูได้

---

**ตอนนี้คุณสามารถดู test results แบบ visual พร้อมรูปภาพและไอคอนได้แล้ว!** 🎊


