# API รายการสินค้า
สถานะ: PROPOSED
รุ่น: v1
เจ้าของข้อเสนอ: [ใส่ชื่อของคุณ]

คำขอ: GET /api/v1/catalog/products
Request body: ไม่ส่ง

ข้อมูลสินค้า:
| id | ชื่อสินค้า | priceSatang | available |
| :--- | :--- | :---: | :---: |
| pencil | ดินสอ | 1200 | true |
| ruler | ไม้บรรทัด | 2000 | false |
| eraser | ยางลบ | 800 | true |
| sharpener | กบเหลาดินสอ | 1500 | false |

ผลตอบกลับ:
| Query ที่ส่ง | Status ที่คาด | JSON Response Body | เหตุผล |
| :--- | :---: | :--- | :--- |
| available=true | 200 | `{"data":[{"id":"pencil","name":"ดินสอ","priceSatang":1200,"available":true},{"id":"eraser","name":"ยางลบ","priceSatang":800,"available":true}]}` | ทั้ง 2 รายการมี available เป็น true |
| available=false | 200 | `{"data":[{"id":"ruler","name":"ไม้บรรทัด","priceSatang":2000,"available":false},{"id":"sharpener","name":"กบเหลาดินสอ","priceSatang":1500,"available":false}]}` | ทั้ง 2 รายการมี available เป็น false |
| ไม่ส่ง | 200 | `{"data":[{"id":"pencil","name":"ดินสอ","priceSatang":1200,"available":true},{"id":"ruler","name":"ไม้บรรทัด","priceSatang":2000,"available":false},{"id":"eraser","name":"ยางลบ","priceSatang":800,"available":true},{"id":"sharpener","name":"กบเหลาดินสอ","priceSatang":1500,"available":false}]}` | ไม่ระบุ available จึงส่งข้อมูลทั้งหมด |
| available=yes | 422 | `{"error":"Validation failed","details":[{"field":"available","message":"Must be true or false"}]}` | ค่าพารามิเตอร์ไม่ถูกต้อง |

กรณีผิดพลาด: 
- **422 (Unprocessable Entity):** ข้อมูลพารามิเตอร์ผิด หรือใส่ค่า `available` นอกเหนือจาก `true` / `false`
- **500 (Internal Server Error):** เกิดข้อผิดพลาดภายในเซิร์ฟเวอร์
- **เหตุผลที่ยังไม่กำหนด 401/403:** เพราะเป็น API สำหรับดึงข้อมูลสินค้าสาธารณะ (Public) ยังไม่มีระบบยืนยันตัวตนหรือการจำกัดสิทธิ์การเข้าถึง