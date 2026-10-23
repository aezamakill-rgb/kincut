# ONNX Runtime สำหรับ ASR ออฟไลน์

คัดลอกจาก `node_modules/onnxruntime-web/dist/` ที่ติดตั้งไว้แล้ว
รุ่น `1.31.0-dev.20260914-8d85527a0` ซึ่งตรงกับ Transformers.js 4.3.1
ห้ามนำ wasm คนละรุ่นมาปนกับ factory `.mjs` หรือแพ็กเกจ

- `ort-wasm-simd-threaded.asyncify.{mjs,wasm}`: WebGPU และ WASM บนเบราว์เซอร์ทั่วไป
- `ort-wasm-simd-threaded.{mjs,wasm}`: WASM บน Safari ก่อน 26
- `LICENSE-onnxruntime.txt`: MIT จาก Microsoft ONNX Runtime

Worker ตั้ง `wasmPaths` ให้เป็น URL ภายในแอปและใช้ `useWasmCache` ของ
Transformers.js เพื่อเก็บ wasm และ factory ใน Cache API `transformers-cache`
ไม่โหลด runtime จาก CDN โมเดลโหลดจาก Hugging Face ครั้งแรกและใช้ cache ต่อไป

ไฟล์ runtime รวมประมาณ 41.2 MB ปัจจุบัน Workbox ของแอปไม่ precache `.wasm`
จึงต้องเรียก ASR ออนไลน์สำเร็จหนึ่งครั้งก่อนใช้ออฟไลน์ (รวมการ cache โมเดลด้วย)
หากต้องการ precache runtime ตั้งแต่ติดตั้ง ให้ผู้ดูแล `vite.config.ts` เพิ่ม
`wasm,mjs` ใน glob และเพิ่ม `maximumFileSizeToCacheInBytes` ให้เกิน 27 MB
