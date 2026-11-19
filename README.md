# Nexa Monorepo Template

ใช้เป็นต้นแบบสำหรับสร้าง repo แยกของแต่ละโปรเจกต์ โดยใช้ Nuxt, NestJS และ SQLite ร่วมกับ pnpm workspaces และ Turborepo

## เริ่มโปรเจกต์

สร้าง repo ใหม่จาก template โดยคัดลอกไฟล์ซ่อนด้วย เช่น `.github`, `.codex` และ `.node-version` ใช้ Node ตาม `.node-version` และ pnpm ตามฟิลด์ `packageManager` ใน `package.json`

เมื่อตั้งต้น repo ที่ยังไม่มี `pnpm-lock.yaml` ให้ผู้ดูแลรัน `pnpm install --no-frozen-lockfile` ในเครื่องที่เข้าถึง npm registry ได้ แล้วบันทึก lockfile ไว้ใน repo ก่อนใช้ CI หรือสร้างโปรเจกต์จาก template

```sh
pnpm install --frozen-lockfile
pnpm db:migrate
pnpm dev
```

เว็บเปิดที่ <http://localhost:3000> และ API เปิดที่ <http://localhost:3001> ตรวจว่า API รับคำขอได้ด้วย `curl -fsS http://localhost:3001/health` ซึ่งควรตอบ `{"status":"ok"}`

ค่าเริ่มต้นของ SQLite คือ `file:./local.db` โดยอ้างตำแหน่งจาก `apps/api` ดู `apps/api/.env.example` สำหรับตัวแปรที่ตั้งได้ เมื่อแก้ Drizzle schema ให้รัน `pnpm db:generate` ตรวจ SQL แล้วรัน `pnpm db:migrate` จาก root เก็บ SQL migration และ metadata ไว้กับโค้ด

## ขอบเขตแพ็กเกจ

| ส่วน | หน้าที่ |
|---|---|
| `apps/web` | หน้า Nuxt และ integration ฝั่งเว็บ ใช้ `@nexa/ui` กับ contracts ใน `@nexa/shared` |
| `apps/api` | NestJS ประกอบ use cases กับ database adapter และเก็บ Drizzle schema กับ migrations |
| `packages/core` | กติกาธุรกิจและ use cases พร้อม interface ของ dependency ที่ use case ต้องใช้ |
| `packages/shared` | Types, validation schemas และ API contracts ที่ใช้ข้ามแอป |
| `packages/ui` | Nuxt layer สำหรับธีมและ wrappers กลาง `NexaForm`, `NexaTable` และ `NexaDialog` |

เริ่มเพิ่มกติกาธุรกิจใน `core` แล้วให้ Nest ประกอบ implementation ที่ต้องติดต่อฐานข้อมูล ใช้หน้าแรกของเว็บเป็นตัวอย่างการใช้ wrapper ทั้งสาม และใช้ไฟล์ layer สำหรับกำหนดธีมเริ่มต้นของโปรเจกต์

## ตรวจงาน

```sh
pnpm lint
pnpm typecheck
pnpm test
pnpm build
```

GitHub Actions ใช้คำสั่งชุดเดียวกันเมื่อเปิดหรืออัปเดต pull request ส่วนกติกาทีม Codex อยู่ใน `AGENTS.md` และโปรไฟล์อยู่ใน `.codex/agents/`
