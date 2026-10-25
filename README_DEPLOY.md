# คู่มือการติดตั้ง E-SARABUN-TH ขึ้นใช้งานบน GitHub Pages + Google Drive + Firebase
(สถาปัตยกรรม Serverless 100% ฟรีตลอดชีพ)

---

## 📌 ภาพรวมสถาปัตยกรรม (Architecture)
1. **Frontend**: เผยแพร่ผ่าน **GitHub Pages** (ไฟล์ `index.html` อยู่ที่ Root)
2. **Document Processing & Storage**: สร้างเอกสารและบันทึกลง Google Drive Folder ของหน่วยงานผ่าน **Google Apps Script (GAS) Web App** (ไฟล์ `Code.gs`)
3. **Database**: บันทึกประวัติเอกสารแบบ Realtime ผ่าน **Firebase Realtime Database**
4. **AI Assistant**: เรียกใช้งานโมเดล AI (Google Gemini, DeepSeek, OpenAI, Claude หรือ Built-in) โดยตรงจากหน้าเว็บผ่าน Client-side API Key ที่เก็บอย่างปลอดภัยใน `localStorage` ของผู้ใช้

---

## 🚀 ขั้นตอนที่ 1: ตั้งค่า Google Apps Script (สร้างไฟล์และบันทึกลง Google Drive)

1. เปิดเบราว์เซอร์ไปที่ **[Google Drive](https://drive.google.com/)** แล้วสร้างโฟลเดอร์สำหรับเก็บเอกสาร เช่น `E-Sarabun Documents`
2. ดับเบิลคลิกเข้าไปในโฟลเดอร์นั้น แล้วดูที่แถบ URL ด้านบน:
   ```
   https://drive.google.com/drive/folders/1A2b3C4d5E6f7G8h9...
   ```
   👉 คัดลอกรหัสหลัง `folders/` เก็บไว้ นั่นคือ **Google Drive Folder ID**

3. ไปที่ **[Google Apps Script](https://script.google.com/)** แล้วกด **"โครงการใหม่" (New Project)**
4. คัดลอกโค้ดทั้งหมดจากไฟล์ `Code.gs` ในโฟลเดอร์โปรเจกต์นี้ ไปวางแทนที่ในโปรเจกต์ GAS
5. คลิกปุ่ม **"การทำให้ใช้งานได้" (Deploy)** ด้านบนขวา ➔ เลือก **"การทำให้ใช้งานได้ใหม่" (New deployment)**
6. คลิกไอคอนรูปฟันเฟือง ➔ เลือกประเภท **"เว็บแอป" (Web app)**:
   - **คำอธิบาย**: `E-Sarabun Engine v1`
   - **ดำเนินการในฐานะ (Execute as)**: `ฉัน (Me)`
   - **ผู้มีสิทธิ์เข้าถึง (Who has access)**: **`ทุกคน (Anyone)`** *(สำคัญมาก เพื่อให้ GitHub Pages ยิงเชื่อมต่อได้)*
7. กดปุ่ม **"ทำให้ใช้งานได้" (Deploy)** และอนุมัติสิทธิ์ (Authorize access) ด้วยบัญชี Google ของท่าน
8. คัดลอก **URL เว็บแอป (Web app URL)** เก็บไว้:
   ```
   https://script.google.com/macros/s/AKfycb.../exec
   ```

---

## 🔥 ขั้นตอนที่ 2: ตั้งค่า Firebase Realtime Database (เก็บบันทึกประวัติ)

1. ไปที่ **[Firebase Console](https://console.firebase.google.com/)**
2. กด **"เพิ่มโปรเจกต์" (Add project)** ➔ ตั้งชื่อโปรเจกต์ เช่น `e-sarabun-school` ➔ กดดำเนินการต่อจนเสร็จ
3. ในเมนูด้านซ้าย เลือก **Build (สร้าง)** ➔ **Realtime Database**
4. กดปุ่ม **"สร้างฐานข้อมูล" (Create Database)** ➔ เลือกเซิร์ฟเวอร์ (แนะนำ: `Singapore asia-southeast1` หรือ `United States`)
5. ที่แท็บ **กฎ (Rules)** ให้ปรับสิทธิ์สำหรับการอ่าน/เขียน:
   ```json
   {
     "rules": {
       ".read": true,
       ".write": true
     }
   }
   ```
   แล้วกด **"เผยแพร่" (Publish)**
6. คัดลอก **Database URL** ที่แสดงบนหัวตาราง:
   ```
   https://e-sarabun-school-default-rtdb.asia-southeast1.firebasedatabase.app
   ```

---

## 🌐 ขั้นตอนที่ 3: นำเว็บไซต์ขึ้น GitHub Pages

1. สร้าง Repository ใหม่บน [GitHub.com](https://github.com/) ตั้งชื่อ เช่น `e-sarabun` (เลือกเป็น Public)
2. เปิด Command Prompt หรือ PowerShell ในโฟลเดอร์โปรเจกต์นี้ แล้วรันคำสั่ง:
   ```bash
   git init
   git add index.html static Code.gs README_DEPLOY.md
   git commit -m "Deploy e-sarabun Serverless Edition to GitHub Pages"
   git branch -M main
   git remote add origin https://github.com/USERNAME/e-sarabun.git
   git push -u origin main
   ```
3. บนหน้า GitHub ของ Repository ➔ ไปที่ **Settings** (แถบด้านบน)
4. เลื่อนเมนูด้านซ้าย เลือก **Pages**
5. ในหัวข้อ **Build and deployment**:
   - **Source**: เลือก `Deploy from a branch`
   - **Branch**: เลือก `main` และโฟลเดอร์ `/ (root)`
   - กดปุ่ม **Save**
6. รอประมาณ 1-2 นาที GitHub Pages จะสร้าง URL เว็บไซต์ให้ทันที:
   👉 **`https://USERNAME.github.io/e-sarabun/`**

---

## ⚙️ ขั้นตอนที่ 4: เชื่อมต่อและเริ่มใช้งาน

1. เปิดเว็บไซต์ GitHub Pages ของท่าน: `https://USERNAME.github.io/e-sarabun/`
2. คลิกปุ่มสีเขียวด้านบน **"ตั้งค่า Drive & Firebase"**
3. ป้อนข้อมูล 3 ช่องที่ได้จากขั้นตอนก่อนหน้า:
   - **Google Apps Script Web App URL**: `https://script.google.com/macros/s/.../exec`
   - **Google Drive Folder ID**: รหัสโฟลเดอร์ Google Drive
   - **Firebase Realtime Database URL**: `https://...firebasedatabase.app`
4. กดปุ่ม **"ทดสอบเชื่อมต่อ"** ➔ ระบบจะแสดงข้อความว่าเชื่อมต่อสำเร็จ
5. กดปุ่ม **"บันทึกการตั้งค่า"**
6. เริ่มสร้างเอกสาร กรอกข้อมูล หรือใช้ AI ร่างเอกสาร จากนั้นคลิกปุ่ม **"เซฟลง Google Drive & Firebase"** หรือกด **"พิมพ์ / บันทึก PDF ทันที"** ได้เลยทันที!

---

### 🎉 สิทธิประโยชน์ที่ได้รับ
- ✅ **ไม่มีค่าเซิร์ฟเวอร์ตลอดชีพ (0 บาท)**
- ✅ เข้าใช้งานได้จากมือถือ ไอแพด แท็บเล็ต คอมพิวเตอร์ ทุกที่ทุกเวลา
- ✅ ไฟล์เอกสารถูกเซฟเก็บใน Google Drive ขององค์กรแยกเป็นระเบียบ
- ✅ สามารถกดแก้ไขข้อมูลย้อนหลัง และเปิดดูไฟล์ใน Drive ได้ในคลิกเดียว
