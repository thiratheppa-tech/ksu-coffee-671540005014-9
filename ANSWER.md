# ANSWER — Thirathep Phanpadab 671540005014-9

- Platform: Vercel
- Production URL: https://ksu-coffee-671540005014-9.vercel.app

## คำตอบ

1. เลือก Vercel ซึ่งรันโค้ดแบบ Serverless คือรันเป็นฟังก์ชันชั่วคราวเมื่อมี request เข้ามา ไม่มีเซิร์ฟเวอร์ที่เปิดค้างให้เราดูแล บน production ไฟล์ `server.js` ไม่ถูกใช้ เพราะ Vercel เสิร์ฟโฟลเดอร์ `public` เป็นไฟล์ static (ตาม `vercel.json`) และรัน `api/info.js` เป็น serverless function เอง ส่วน `server.js` มีไว้รันทดสอบในเครื่องเท่านั้น

2. ตั้ง `STUDENT_ID` ที่ Project Settings → Environment Variables บน Vercel ไม่ commit `.env` เพราะเป็นค่าตั้งค่าหรือข้อมูลลับของแต่ละเครื่อง ถ้าขึ้น repo สาธารณะทุกคนจะเห็น และค่าบน production ก็ไม่ควรผูกกับโค้ด `/api/info` แสดง NOT SET ได้เมื่อ: ยังไม่ได้ตั้งตัวแปร, ชื่อพิมพ์ผิด, ไม่ได้เลือก environment Production, หรือตั้งหลัง deploy แล้วแต่ยังไม่มี deployment ใหม่ (ค่าใหม่มีผลเฉพาะ deployment ที่สร้างหลังตั้ง) ของผมเกิดเพราะตอน deploy ครั้งแรกยังไม่ได้เพิ่มตัวแปร จึงเพิ่มแล้ว push ใหม่ให้ deploy ใหม่

3. ครั้งแรก 0.660 วินาที / ครั้งที่สอง 0.620 วินาที — เกือบเท่ากัน ต่างกันไม่ถึง 0.05 วินาที เพราะฟังก์ชันถูกเรียกอยู่บ่อยช่วงที่ทดสอบ จึงยังไม่หลับ (ไม่เกิด cold start) เวลาที่เห็นส่วนใหญ่เป็นเวลาเครือข่ายและการเชื่อมต่อ ถ้าทิ้งไว้นานแล้วเรียกครั้งแรกอาจช้ากว่าเพราะต้องปลุกฟังก์ชัน

4. หลัง `git push` GitHub แจ้ง Vercel ผ่านการเชื่อมต่อ repo ว่ามี commit ใหม่บน branch main Vercel จึงดึงโค้ดไป deploy เป็น Production deployment ใหม่ อัตโนมัติ หน้า production จึงเปลี่ยนเป็น Menu v2 ค่า `commit` ใน `/api/info` ตรงกับ `git log` คือ `c2482b350d3ae741321ac103983cd88d7fedb4ad` เพราะค่านั้นมาจากตัวแปรที่ Vercel ใส่ให้ (`VERCEL_GIT_COMMIT_SHA`) ไม่ได้เขียนในโค้ด

5. นำมา deploy ตรง ๆ ไม่ได้ เพราะ Vercel ไม่รัน container และไม่อ่าน `docker-compose.yml` ส่วนบนแพลตฟอร์มแบบ container เช่น Render ก็รับ image/Dockerfile ได้ครั้งละ 1 service ต่อโปรเจกต์ ไม่ได้รัน Compose ที่มีสองบริการพร้อมกันให้ ต้องแยก deploy แต่ละ service เองและแก้ลิงก์ให้ชี้ไปที่ URL จริงของแต่ละตัว
