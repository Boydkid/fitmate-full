# 🚀 คู่มือการ Deploy ด้วย GitHub

## ✅ ข้อมูลเบื้องต้น

**ตอบ:** ใช้ GitHub deploy project นี้ได้ แต่มีข้อจำกัดบางอย่าง

### ข้อจำกัด

1. **Frontend (Next.js)**: 
   - GitHub Pages รองรับเฉพาะ **static sites**
   - ต้องใช้ **static export** (`output: 'export'`)
   - **API Routes ไม่ทำงาน** - ต้องเรียก backend โดยตรง

2. **Backend (Express.js)**:
   - GitHub Pages **ไม่รองรับ server-side code**
   - ต้อง deploy ที่ hosting service อื่น เช่น:
     - **Vercel** (แนะนำ)
     - **Render**
     - **Railway**
     - **Heroku**
     - **DigitalOcean App Platform**

---

## 📋 ขั้นตอนการ Deploy

### ส่วนที่ 1: ตั้งค่า GitHub Pages

#### 1. เปิดใช้ GitHub Pages

1. ไปที่ repository ของคุณบน GitHub
2. ไปที่ **Settings** → **Pages**
3. ตั้งค่า:
   - **Source**: `GitHub Actions` (แนะนำ) หรือ `Deploy from a branch`
   - **Branch**: `main` (หรือ branch ที่คุณต้องการ)

#### 2. ตั้งค่า Secrets (ถ้าจำเป็น)

1. ไปที่ **Settings** → **Secrets and variables** → **Actions**
2. เพิ่ม secret:
   - `NEXT_PUBLIC_API_BASE_URL`: URL ของ backend ที่ deploy แล้ว
     - ตัวอย่าง: `https://your-backend.railway.app/api` หรือ `https://api.fitmat.com/api`

---

### ส่วนที่ 2: Deploy Frontend ด้วย GitHub Actions

#### ไฟล์ที่สร้างไว้แล้ว

- `.github/workflows/deploy-frontend.yml` - GitHub Actions workflow สำหรับ deploy

#### ขั้นตอน

1. **เปิดใช้ Static Export**:
   - เพิ่ม environment variable ใน GitHub Actions:
     - ไปที่ **Settings** → **Secrets and variables** → **Actions** → **Variables**
     - เพิ่ม: `NEXT_PUBLIC_USE_STATIC_EXPORT` = `true`

2. **ตั้งค่า API Base URL**:
   - เพิ่ม secret: `NEXT_PUBLIC_API_BASE_URL` = `https://your-backend-url.com/api`
   - หรือแก้ไขใน workflow file: `.github/workflows/deploy-frontend.yml`

3. **Push code**:
   ```bash
   git add .
   git commit -m "Setup GitHub Pages deployment"
   git push origin main
   ```

4. **ตรวจสอบการ Deploy**:
   - ไปที่ **Actions** tab ใน GitHub
   - ดู workflow run `Deploy Frontend to GitHub Pages`
   - รอให้ build และ deploy เสร็จ

5. **เข้าถึงเว็บไซต์**:
   - URL จะเป็น: `https://your-username.github.io/FITMAT/` (ถ้าตั้งค่า basePath)
   - หรือ: `https://your-username.github.io/` (ถ้าไม่ตั้ง basePath)

---

### ส่วนที่ 3: Deploy Backend

Backend **ไม่สามารถ deploy บน GitHub Pages** ได้ ต้องใช้ hosting service อื่น

#### ตัวเลือกที่แนะนำ

##### ตัวเลือก 1: Vercel (แนะนำ)

1. **ติดตั้ง Vercel CLI**:
   ```bash
   npm i -g vercel
   ```

2. **Deploy**:
   ```bash
   cd Fitmat-BackEnd
   vercel
   ```

3. **ตั้งค่า Environment Variables**:
   - ไปที่ Vercel Dashboard → Project Settings → Environment Variables
   - เพิ่มตัวแปรทั้งหมดจาก `.env`

4. **Build Settings**:
   - Build Command: `npm install && npx prisma generate`
   - Output Directory: `dist` (ถ้าใช้ TypeScript)

##### ตัวเลือก 2: Render

1. สร้าง **Web Service** ใหม่
2. เชื่อมต่อ GitHub repository
3. ตั้งค่า:
   - **Root Directory**: `Fitmat-BackEnd`
   - **Build Command**: `npm install && npx prisma generate`
   - **Start Command**: `npm run dev` หรือ `node dist/server.js`
4. เพิ่ม **Environment Variables**

##### ตัวเลือก 3: Railway

1. สร้าง project ใหม่
2. เชื่อมต่อ GitHub repository
3. ตั้งค่า service:
   - **Root Directory**: `Fitmat-BackEnd`
   - **Build Command**: `npm install && npx prisma generate`
   - **Start Command**: `npm run dev`
4. เพิ่ม environment variables

---

## ⚙️ การตั้งค่า Environment Variables

### Frontend (สำหรับ GitHub Actions)

ตั้งค่าใน **GitHub Secrets**:

```
NEXT_PUBLIC_USE_STATIC_EXPORT=true
NEXT_PUBLIC_API_BASE_URL=https://your-backend-url.com/api
```

### Backend (สำหรับ hosting service)

ตั้งค่าใน hosting service (Vercel, Render, Railway, etc.):

```env
DATABASE_URL="mysql://user:password@host:3306/fitmat"
PORT=4000
JWT_SECRET="your-super-secret-jwt-key"
STRIPE_SECRET_KEY="sk_live_..." (ถ้าใช้)
EMAIL_USER="your-email@gmail.com"
EMAIL_PASSWORD="app-password"
CONTACT_NOTIFY_EMAIL="admin@example.com"
```

---

## 🔧 แก้ไขปัญหา (Troubleshooting)

### Frontend ไม่สามารถเรียก API ได้

**สาเหตุ**: GitHub Pages ใช้ static export จึงไม่มี API routes

**แก้ไข**:
1. ตรวจสอบว่า `NEXT_PUBLIC_USE_STATIC_EXPORT=true`
2. ตรวจสอบว่า `NEXT_PUBLIC_API_BASE_URL` ถูกต้อง
3. ตรวจสอบ CORS settings ใน backend

### Build ล้มเหลว

**สาเหตุ**: Environment variables ไม่ครบ

**แก้ไข**:
1. ตรวจสอบ GitHub Secrets
2. ตรวจสอบว่า workflow file มีการตั้งค่าถูกต้อง

### Images ไม่แสดง

**สาเหตุ**: Static export ต้องใช้ `unoptimized: true`

**แก้ไข**: ตรวจสอบ `next.config.ts` ว่ามีการตั้งค่า `images.unoptimized: true`

---

## 📝 หมายเหตุสำคัญ

### สำหรับ Static Export

⚠️ **ข้อจำกัด**:
- ไม่สามารถใช้ **API Routes** (`/api/*`) ได้
- ไม่สามารถใช้ **Server-Side Rendering (SSR)** ได้
- ไม่สามารถใช้ **Incremental Static Regeneration (ISR)** ได้
- ต้องเรียก backend API โดยตรงจาก frontend

### สำหรับ Backend

⚠️ **สำคัญ**:
- Backend ต้องรองรับ **CORS** สำหรับ frontend domain
- ตั้งค่า CORS ให้รองรับ GitHub Pages URL
- ใช้ HTTPS สำหรับ production

---

## 🎯 สรุป

### ✅ ทำได้

- ✅ Deploy frontend ไป GitHub Pages (ด้วย static export)
- ✅ ใช้ GitHub Actions สำหรับ CI/CD
- ✅ ตั้งค่า custom domain

### ❌ ไม่สามารถทำได้

- ❌ Deploy backend ไป GitHub Pages (ต้องใช้ hosting อื่น)

### 🔄 ทางเลือก

- ใช้ **Vercel** สำหรับทั้ง frontend และ backend (แนะนำสำหรับ Next.js)
- ใช้ **Netlify** สำหรับ frontend และ **Vercel/Render** สำหรับ backend

---

## 📞 ความช่วยเหลือ

ถ้ามีปัญหา:
1. ตรวจสอบ GitHub Actions logs
2. ตรวจสอบ browser console สำหรับ CORS errors
3. ตรวจสอบ backend logs
4. อ่าน documentation:
   - [GitHub Pages](https://docs.github.com/en/pages)
   - [Next.js Deployment](https://nextjs.org/docs/deployment)

