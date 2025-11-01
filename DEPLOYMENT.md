# 🚀 คู่มือการ Deploy FITMAT

## 📋 สารบัญ

1. [GitHub Pages Deployment (Frontend)](#github-pages-deployment-frontend)
2. [Backend Deployment](#backend-deployment)
3. [DNS Configuration](#dns-configuration)
4. [Environment Variables](#environment-variables)

---

## 🌐 GitHub Pages Deployment (Frontend)

### ขั้นตอนที่ 1: ตั้งค่า Custom Domain

1. ไปที่ **Settings** → **Pages** ใน repository ของคุณ
2. ในส่วน **Custom domain** กรอก: `www.fitmat.com`
3. GitHub จะแสดงข้อความแจ้งเตือนว่ากำลังตรวจสอบ DNS

### ขั้นตอนที่ 2: ตั้งค่า DNS Records

คุณต้องไปตั้งค่า DNS ที่ผู้ให้บริการ domain ของคุณ (เช่น Namecheap, GoDaddy, Cloudflare)

#### สำหรับโดเมน `www.fitmat.com`:

1. **CNAME Record** (แนะนำ):
   ```
   Type: CNAME
   Name: www
   Value: kengkwxx.github.io
   TTL: 3600 (หรือค่าเริ่มต้น)
   ```

2. **หรือ A Records** (ถ้าไม่ใช้ CNAME):
   ```
   Type: A
   Name: www
   Value: 185.199.108.153
   TTL: 3600
   
   Type: A
   Name: www
   Value: 185.199.109.153
   TTL: 3600
   
   Type: A
   Name: www
   Value: 185.199.110.153
   TTL: 3600
   
   Type: A
   Name: www
   Value: 185.199.111.153
   TTL: 3600
   ```

### ขั้นตอนที่ 3: รอ DNS Propagation

- DNS เปลี่ยนแปลงอาจใช้เวลาถึง 48 ชั่วโมง
- ตรวจสอบได้ที่: https://dnschecker.org/
- เมื่อ DNS ถูกต้องแล้ว GitHub จะแสดง ✅

### ขั้นตอนที่ 4: เปิดใช้ HTTPS

เมื่อ DNS ตรวจสอบผ่านแล้ว:
1. กลับไปที่ **Settings** → **Pages**
2. ติ๊กช่อง **Enforce HTTPS** (จะเปิดใช้งานได้เมื่อ DNS ถูกต้อง)
3. บันทึก

---

## ⚙️ Backend Deployment

Backend ต้อง deploy ไปยัง hosting service เช่น:
- **Vercel** (แนะนำสำหรับ Next.js/Express)
- **Heroku**
- **Railway**
- **Render**
- **DigitalOcean App Platform**
- **AWS Elastic Beanstalk**

### ตัวอย่าง: Deploy ไป Vercel

1. **ติดตั้ง Vercel CLI:**
   ```bash
   npm i -g vercel
   ```

2. **Deploy:**
   ```bash
   cd Fitmat-BackEnd
   vercel
   ```

3. **ตั้งค่า Environment Variables ใน Vercel Dashboard:**
   - ไปที่ Project Settings → Environment Variables
   - เพิ่มตัวแปรทั้งหมดจาก `.env`

4. **ปรับ build settings:**
   - Build Command: `npm install && npx prisma generate`
   - Output Directory: (ไม่ต้องตั้ง)
   - Install Command: `npm install`

### ตัวอย่าง: Deploy ไป Render

1. สร้าง **Web Service** ใหม่
2. เชื่อมต่อ GitHub repository
3. ตั้งค่า:
   - **Build Command:** `npm install && npx prisma generate`
   - **Start Command:** `npm run dev` (หรือใช้ production script)
4. เพิ่ม **Environment Variables** ทั้งหมด

---

## 🔧 Environment Variables

### Frontend (.env.local)

สร้างไฟล์ `.env.local` ใน `Fitmat-FrontEnd/`:

```env
# API Base URL (ต้องใช้ URL ของ Backend ที่ deploy แล้ว)
NEXT_PUBLIC_API_BASE_URL=https://your-backend-domain.com/api

# สำหรับ Production
# NEXT_PUBLIC_API_BASE_URL=https://api.fitmat.com/api
```

### Backend (.env)

สร้างไฟล์ `.env` ใน `Fitmat-BackEnd/`:

```env
# Database
DATABASE_URL="mysql://user:password@host:3306/fitmat"

# Server
PORT=4000

# JWT
JWT_SECRET="your-super-secret-jwt-key-change-this-in-production"

# Stripe (ถ้าใช้)
STRIPE_SECRET_KEY="sk_live_..."
STRIPE_WEBHOOK_SECRET="whsec_..."

# Email (Gmail App Password)
EMAIL_USER="your-email@gmail.com"
EMAIL_PASSWORD="your-app-password"
CONTACT_NOTIFY_EMAIL="admin@example.com"
```

### 🔐 Security Notes

⚠️ **สำคัญ:**
- อย่า commit ไฟล์ `.env` หรือ `.env.local` ลง Git
- ใช้ environment variables ใน hosting service
- เปลี่ยน `JWT_SECRET` เป็นค่า random ที่แข็งแรงใน production
- ใช้ Gmail App Password ไม่ใช่รหัสผ่านปกติ

---

## 🔄 Build & Deploy Workflow

### Frontend (Next.js)

1. **Build สำหรับ Production:**
   ```bash
   cd Fitmat-FrontEnd
   npm run build
   ```

2. **Test Production Build:**
   ```bash
   npm run start
   ```

3. **สำหรับ GitHub Pages:**
   - GitHub Pages รองรับ static HTML
   - Next.js ต้องใช้ `next export` (deprecated) หรือใช้ Vercel/Netlify แทน
   - **แนะนำ:** ใช้ Vercel สำหรับ Next.js แทน GitHub Pages

### Backend (Express)

1. **Build TypeScript:**
   ```bash
   cd Fitmat-BackEnd
   npx tsc
   ```

2. **Run Production:**
   ```bash
   node dist/server.js
   ```

---

## 🌍 Production Checklist

### Frontend
- [ ] ตั้งค่า `NEXT_PUBLIC_API_BASE_URL` เป็น production backend URL
- [ ] Build และ test production build
- [ ] ตรวจสอบว่า environment variables ถูกต้อง
- [ ] ตั้งค่า custom domain (ถ้ามี)
- [ ] เปิดใช้ HTTPS

### Backend
- [ ] ตั้งค่า production database (MySQL)
- [ ] เปลี่ยน `JWT_SECRET` เป็นค่าใหม่
- [ ] ตั้งค่า environment variables ทั้งหมด
- [ ] ตั้งค่า CORS ให้รองรับ frontend domain
- [ ] ตั้งค่า Stripe keys สำหรับ production (ถ้าใช้)
- [ ] ตั้งค่า email credentials

### Database
- [ ] รัน migrations: `npx prisma migrate deploy`
- [ ] Generate Prisma Client: `npx prisma generate`
- [ ] Backup database (สำคัญ!)

### Security
- [ ] ตรวจสอบ `.gitignore` ว่ามี `.env` แล้ว
- [ ] ตรวจสอบว่าไม่มี secrets ใน code
- [ ] เปิดใช้ HTTPS ทั้ง frontend และ backend
- [ ] ตั้งค่า CORS ให้จำกัดเฉพาะ domain ที่อนุญาต

---

## 📝 หมายเหตุสำหรับ GitHub Pages

⚠️ **ข้อจำกัด:**
- GitHub Pages รองรับ static sites เท่านั้น
- Next.js โดยปกติต้องใช้ server-side rendering
- **วิธีแก้:**
  1. ใช้ `output: 'export'` ใน `next.config.ts` (static export)
  2. หรือใช้ Vercel/Netlify สำหรับ Next.js

### วิธีใช้ Static Export (ถ้าต้องใช้ GitHub Pages)

อัพเดท `Fitmat-FrontEnd/next.config.ts`:

```typescript
const nextConfig: NextConfig = {
  reactStrictMode: true,
  output: 'export', // สำหรับ static export
  images: {
    unoptimized: true, // จำเป็นสำหรับ static export
  },
};
```

⚠️ **ข้อจำกัดของ Static Export:**
- ไม่สามารถใช้ API Routes (`/api/*`) ได้
- ไม่สามารถใช้ Server-Side Rendering (SSR) ได้
- ไม่สามารถใช้ Incremental Static Regeneration (ISR) ได้

---

## 🆘 Troubleshooting

### DNS ไม่ผ่านการตรวจสอบ
- รอ 24-48 ชั่วโมง (DNS propagation)
- ตรวจสอบ DNS records ที่ถูกต้อง
- ใช้ `dig www.fitmat.com` หรือ `nslookup www.fitmat.com` ตรวจสอบ

### HTTPS เปิดใช้ไม่ได้
- DNS ต้องถูกต้องก่อน
- รอให้ GitHub ตรวจสอบ DNS เสร็จ
- ตรวจสอบว่า domain มี SSL certificate

### API ไม่เชื่อมต่อ
- ตรวจสอบ `NEXT_PUBLIC_API_BASE_URL` ใน frontend
- ตรวจสอบ CORS settings ใน backend
- ตรวจสอบว่า backend server รันอยู่

---

## 📞 ความช่วยเหลือ

ถ้ามีปัญหาเพิ่มเติม:
1. ตรวจสอบ documentation ของ hosting service
2. ตรวจสอบ GitHub Pages documentation: https://docs.github.com/en/pages
3. ตรวจสอบ Next.js deployment docs: https://nextjs.org/docs/deployment

