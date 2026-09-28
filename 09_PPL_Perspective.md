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
}```

### 9.2 Semantics

ใน Rust **Package, Crate และ Module** มีหน้าที่แตกต่างกันในการจัดโครงสร้างโปรแกรม

- **Package** คือหน่วยของ Cargo Project ใช้สำหรับจัดการโปรเจกต์และรวบรวม Crate ที่เกี่ยวข้อง
- **Crate** คือหน่วยที่ Rust Compiler นำไป Compile โดยสามารถเป็น **Binary Crate** หรือ **Library Crate**
- **Module** ใช้แบ่งและจัดกลุ่ม Code ภายใน Crate รวมถึงช่วยสร้าง Namespace
- **Item** คือสิ่งที่อยู่ภายใน Module เช่น Function, Struct, Enum หรือ Constant

ตัวอย่าง Path:

```text
crate::food::order
```

หมายถึงเริ่มจาก Crate ปัจจุบัน → เข้า Module `food` → เข้าถึง Item `order`

ดังนั้น Package และ Crate ช่วยกำหนดโครงสร้างระดับโปรเจกต์และการ Compile ส่วน Module ช่วยแบ่ง Code ภายใน Crate ให้เป็นหมวดหมู่และควบคุมการเข้าถึงได้ชัดเจนขึ้น

---

### 9.3 Type System

Rust เป็นภาษาแบบ **Static Typing** หมายความว่า Type จะถูกตรวจสอบในช่วง Compile Time ก่อนที่โปรแกรมจะทำงาน

สำหรับ **Modules, Packages และ Crates** นั้น Module System ไม่ได้เป็น Type System โดยตรง แต่ทำงานร่วมกับ Type System โดยช่วยกำหนด Scope และ Visibility ของ Type และ Item ต่าง ๆ

ตัวอย่าง:

```rust
mod user {
    pub struct User {
        pub name: String,
    }
}

fn main() {
    let u = user::User {
        name: String::from("Alice"),
    };

    println!("{}", u.name);
}
```

ในตัวอย่างนี้

- `User` เป็น `struct` ที่มี Type ชัดเจน
- `name` มี Type เป็น `String`
- `pub struct User` ทำให้ `User` สามารถเข้าถึงจากภายนอก Module `user`
- `pub name` ทำให้ Field `name` สามารถเข้าถึงจากภายนอกได้
- Compiler ตรวจสอบทั้งความถูกต้องของ Type และการเข้าถึง Item

ดังนั้น **Type System** ทำหน้าที่ตรวจสอบความถูกต้องของ Type ส่วน **Module System และ Visibility** ช่วยควบคุมว่าส่วนใดของโปรแกรมสามารถมองเห็นและใช้งาน Item นั้นได้

---

### 9.4 Memory / Resource Management

**Packages, Crates และ Modules ไม่ได้ทำหน้าที่จัดการ Memory โดยตรง** แต่ Code ที่อยู่ภายใน Module ยังคงทำงานภายใต้กฎการจัดการ Memory ของ Rust

Rust ใช้แนวคิดสำคัญ ได้แก่

- **Ownership** — กำหนดว่า Value ใดมีตัวแปรใดเป็นเจ้าของ
- **Borrowing** — อนุญาตให้ยืม Value ไปใช้งานโดยไม่จำเป็นต้องย้าย Ownership
- **References** — ใช้อ้างอิง Value เช่น `&String`
- เมื่อ Value หมด Scope ทรัพยากรที่ Value นั้นเป็นเจ้าของจะถูกปล่อยตามกฎของ Rust

ตัวอย่าง:

```rust
mod message {
    pub fn show(text: &String) {
        println!("{}", text);
    }
}

fn main() {
    let text = String::from("Hello");

    message::show(&text);

    println!("{}", text);
}
```

Function `show()` อยู่ภายใน Module `message` และรับ `&String` ซึ่งเป็นการ Borrow ข้อมูล

ดังนั้น Ownership ของ `text` ไม่ได้ถูกย้ายไปยัง Function `show()` ทำให้ `main()` ยังสามารถใช้ `text` ต่อได้หลังจากเรียก Function

การแบ่ง Code เป็น Module จึงไม่ได้เปลี่ยนกฎของ Ownership และ Borrowing แต่ช่วยจัดโครงสร้างของ Code ที่ใช้กฎเหล่านี้ให้ชัดเจนขึ้น

---

### 9.5 Abstraction / Other PPL Concepts

#### Abstraction

Module ช่วยสร้าง **Abstraction** โดยสามารถซ่อนรายละเอียดการทำงานภายใน Module และเปิดเผยเฉพาะ Item ที่ต้องการให้ส่วนอื่นของโปรแกรมใช้งานผ่าน `pub`

ผู้ใช้งาน Module จึงสามารถเรียกใช้ Public API ได้โดยไม่จำเป็นต้องรู้รายละเอียด Implementation ภายในทั้งหมด

#### Scope

Rust ใช้ **Lexical Scope หรือ Static Scope** ซึ่งขอบเขตของชื่อสามารถพิจารณาได้จากโครงสร้างของ Source Code

แต่ละ Module มี Scope ของตัวเอง และสามารถใช้ `use` เพื่อนำชื่อจาก Path อื่นเข้ามาอยู่ใน Scope ปัจจุบัน

#### Visibility

Item ภายใน Module เป็น **Private โดย Default**

หากต้องการให้ Code ภายนอกสามารถเข้าถึง Item ได้ ต้องกำหนด Visibility เช่น

```rust
pub fn order() {
    println!("Order food");
}
```

`pub` ทำให้ Function `order()` สามารถเข้าถึงได้จากภายนอก Module ตามกฎ Visibility ของ Rust

#### Namespace

Module ทำหน้าที่เป็น **Namespace** ช่วยจัดกลุ่มชื่อและลดปัญหาการใช้ชื่อซ้ำกัน

ตัวอย่าง:

```text
crate::customer::create
crate::product::create
```

ทั้งสอง Module สามารถมี Function ชื่อ `create` เหมือนกันได้ เพราะ Function อยู่ภายใต้ Namespace ที่แตกต่างกัน คือ `customer` และ `product`

ดังนั้น Module System ช่วยสนับสนุนแนวคิดสำคัญทาง PPL ได้แก่ **Abstraction, Scope, Visibility และ Namespace**

---

### 9.6 Why Rust?

Rust ใช้ **Package, Crate และ Module System** เพื่อช่วยให้โปรแกรมมีโครงสร้างที่ชัดเจน สามารถแบ่ง Code ออกเป็นส่วนย่อย และควบคุมการเข้าถึง Item ได้

แนวคิดเหล่านี้ช่วย Rust ในหลายด้าน ได้แก่

- **Safety** — Item ภายใน Module เป็น Private โดย Default และ Programmer ต้องระบุ `pub` เมื่อต้องการเปิดให้ส่วนอื่นเข้าถึง
- **Reliability** — การแบ่ง Code เป็น Module ช่วยแยกหน้าที่ของแต่ละส่วน และลดการเข้าถึงส่วนภายในโดยไม่จำเป็น
- **Maintainability** — Package, Crate และ Module ช่วยจัดโปรแกรมขนาดใหญ่ให้เป็นส่วนย่อย ทำให้อ่าน แก้ไข และดูแล Code ได้ง่ายขึ้น
- **Compile-time Checking** — Compiler สามารถตรวจสอบ Path, Visibility และการเข้าถึง Item ก่อนที่โปรแกรมจะทำงาน

ดังนั้น Module System ของ Rust ไม่ได้มีหน้าที่เพียงแบ่งไฟล์หรือจัด Code เท่านั้น แต่ยังช่วยสร้าง **Abstraction, Namespace และ Visibility** ที่ชัดเจน พร้อมให้ Compiler ตรวจสอบข้อผิดพลาดหลายอย่างได้ตั้งแต่ Compile Time
