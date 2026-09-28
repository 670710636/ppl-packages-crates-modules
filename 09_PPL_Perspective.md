## 9. PPL Perspective

ในมุมมองของ Principles of Programming Languages แนวคิด **Packages, Crates และ Modules** ของ Rust ช่วยจัดโครงสร้างโปรแกรม แบ่งขอบเขตของชื่อ (Namespace) และควบคุมการเข้าถึงส่วนต่าง ๆ ของโปรแกรมอย่างชัดเจน

### 9.1 Syntax

Rust มี Syntax สำหรับจัดการ Module และการเข้าถึง Item ภายใน Module เช่น

- `mod` ใช้ประกาศ Module
- `pub` ใช้กำหนดให้ Item สามารถเข้าถึงจากภายนอกได้
- `use` ใช้นำ Path เข้ามาใน Scope เพื่อเรียกใช้งานได้สะดวกขึ้น
- `crate` ใช้อ้างถึง Crate ปัจจุบัน
- `super` ใช้อ้างถึง Parent Module
- `self` ใช้อ้างถึง Module ปัจจุบัน

ตัวอย่าง:

```rust
mod food {
    pub fn order() {
        println!("Order food");
    }
}

use crate::food::order;

fn main() {
    order();
}