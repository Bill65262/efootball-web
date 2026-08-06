# eFootball ID Shop — Project Memory

## โปรเจกต์คืออะไร
เว็บขาย eFootball ID ชื่อ "eFootball ID Shop" — ผู้เล่นเลือกดูนักเตะ (ไก่เดี่ยว / ไก่คู่) ราคา และสถานะ (available/sold) แล้วติดต่อซื้อผ่าน Messenger/Facebook

## URL เว็บ
- **Public:** https://mookmookshop.xyz
- **Localhost (dev):** http://localhost:8765/

## โครงสร้างไฟล์ปัจจุบัน (อัปเดต 2026-05-14)
```
X:\Web\
├── index.html              ← หน้าเว็บหลัก (HTML+CSS+JS รวมไฟล์เดียว)
├── CNAME                   ← = mookmookshop.xyz
├── .nojekyll
├── .gitignore
├── img/
│   ├── tabs.png
│   └── header.png
├── data/
│   ├── playersdata.json    ← ข้อมูลนักเตะทั้งหมด (ชื่อ, card_img)
│   ├── singleplayers.json  ← สถานะ+ราคาไก่เดี่ยว
│   ├── duocombos.json      ← ข้อมูลไก่คู่ + ราคา + สถานะ (~1900 ไอดี)
│   └── duoplayers.json
├── assets/template/
│   ├── single/             ← รูปไก่เดี่ยว (97 ไฟล์)
│   └── duo/                ← รูปไก่คู่ (2010+ ไฟล์) แบ่ง A-Z
├── Tools/                  ← เครื่องมือ dev (ไม่ push ขึ้น GitHub)
│   ├── manager.py          ← source code Manager GUI
│   ├── Manager.exe         ← โปรแกรมจัดการหลัก
│   ├── Tool.py             ← Toolbox GUI (เพิ่ม 2026-05-14)
│   ├── watermark.py        ← standalone watermark tool (เก็บไว้อ้างอิง)
│   ├── push_updated.py     ← script push รูปที่แปลงใหม่ (ใช้ครั้งเดียว)
│   ├── Manager.spec
│   ├── จัดการระบบไก่.spec
│   ├── build/
│   └── dist/
├── Memory/
│   └── project.md          ← ไฟล์นี้
└── Bin/                    ← ไฟล์ไม่ใช้แล้ว (เก็บไว้ก่อน)
    ├── cloudflared.exe
    ├── push_duo.py
    ├── frontend/
    └── backend/
```

## GitHub Repo
- **Repo:** https://github.com/Bill65262/efootball-web
- **Branch:** main
- **Deploy:** GitHub Pages จาก root (/)
- **Token:** เก็บใน manager.py (GITHUB_TOKEN)

## DNS / Hosting
- **Registrar:** Namecheap
- **DNS Records:**
  - A Record `@` → 185.199.108-111.153 (GitHub Pages IPs)
  - CNAME `www` → bill65262.github.io
- **GitHub Pages Custom Domain:** mookmookshop.xyz (apex)
- **HTTPS:** เปิดใช้งานแล้ว

## Manager.py — ฟังก์ชั่นหลัก
ใช้ absolute path ทั้งหมด (`X:\Web\...`) → ย้ายไปอยู่ `Tools/` แล้วก็ยังทำงานได้ปกติ

| ปุ่ม | ทำอะไร |
|------|--------|
| บันทึก + อัปเดตเว็บ | save JSON + push index.html, img, data ขึ้น GitHub API |
| อัพเดทนักเตะ+ราคา | push เฉพาะ JSON data |
| เพิ่ม Template ใหม่ | scan รูปใหม่จาก assets/ แล้ว push (ข้ามไฟล์ที่มีอยู่แล้วบน GitHub) |
| เปิดเว็บ | เปิด https://mookmookshop.xyz |
| localhost | เปิด http://localhost:8765/ |
| โหลดข้อมูลใหม่ | reload JSON จาก local |

## Tool.py — Toolbox GUI (ใหม่ 2026-05-14)
ไฟล์ `X:\Web\Tools\Tool.py` — แอพรวมเครื่องมือ เพิ่ม tab ได้เรื่อยๆ
- **Tab 1: Crop & Watermark** — crop รูป 640×480 และใส่ลายน้ำ "FB : Mook Mook"

## Crop & Watermark — กฎสำคัญ
- **ทำงานเฉพาะรูปขนาด 640×480 เท่านั้น** (screenshot ดิบจากเกม)
- Crop box: `(110, 60, 530, 420)` → ผลลัพธ์ขนาด 420×360
- ลายน้ำ: "FB : Mook Mook", opacity 10%, rotation -45°
- โหมด "Crop + Watermark": crop ก่อน → ส่ง list ไฟล์ที่ crop แล้วไป watermark (ไม่สแกนใหม่)

## X:\PlayerCollector\Captured — รูป template ไก่คู่
- โฟลเดอร์เก็บรูปก่อน/หลังแปลง แบ่งโฟลเดอร์ A-Z ตามชื่อไฟล์
- รูปดิบ (ยังไม่แปลง): 640×480 — มี UI เกมครบ (Back, Collective Strength, โลโก้สโมสร)
- รูปที่แปลงแล้ว: 420×360 — ครอปเฉพาะฟอร์เมชั่น + มีลายน้ำ
- มี `Test/` subfolder ไว้ preview รูปก่อนแปลง

## การ push รูปใหม่ขึ้น GitHub
- ปุ่ม "เพิ่ม Template ใหม่" ใน Manager **ข้ามไฟล์ที่มีอยู่บน GitHub แล้ว** → รูปที่แปลงใหม่แต่ชื่อเดิมจะไม่ถูก re-upload
- วิธีแก้: ใช้ `push_updated.py` — ใส่ชื่อไฟล์ที่แปลงใหม่ แล้วรันเพื่อ force push เฉพาะไฟล์นั้น
- **ราคาไม่รีเซ็ต** ถ้า push แค่รูป เพราะราคาอยู่ใน JSON แยกต่างหาก
- ราคารีเซ็ตเมื่อ: กด "เพิ่ม Template ใหม่" หลังจากลบรูปออกไป (JSON rebuild ใหม่หมด)

## การ push ไฟล์ขึ้น GitHub
Manager ใช้ **GitHub API** (ไม่ใช่ git) push ไฟล์โดยตรง:
- ต้องดึง SHA ของไฟล์ก่อน แล้วค่อย PUT ทับ
- ถ้า SHA หมดอายุระหว่าง push จะ error 422 → retry แก้ได้

## Local HTTP Server
- manager.py รัน `http.server` ที่ port **8765** serve จาก `X:\Web\`
- ใช้ทดสอบเว็บ offline ก่อน push

## หมายเหตุ
- ถ้า JSON เสีย (git conflict markers) ให้ดึงจาก GitHub API มาแทนทับ
- Manager.exe ต้องปิดก่อนถึงจะ build ทับได้
- `Tools/` ไม่ push ขึ้น GitHub (อยู่ใน .gitignore)
