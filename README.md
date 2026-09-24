# Modules, Packages & Crates
## Rust vs Other Languages + PPL Analysis

---

## 1. Rust vs Java

| Aspect | Rust | Java |
|---|---|---|
| **Syntax** | ใช้ `mod` สร้าง Module, `use` นำชื่อมาใช้ และ `pub` กำหนดการเข้าถึง | ใช้ `package` จัดกลุ่ม Class และ `import` นำ Class/Package มาใช้ |
| **Semantics / Behavior** | Package คือโปรเจกต์, Crate คือหน่วย Compilation และ Module ใช้แบ่งโครงสร้างภายใน Crate | Package ใช้จัดกลุ่ม Class/Interface และเป็น Namespace ของโปรแกรม |
| **Type System** | Static และ Strong Typing ตรวจสอบ Type ตอน Compile | Static และ Strong Typing ตรวจสอบ Type ตอน Compile |
| **Memory Management** | ใช้ Ownership และ Borrowing จัดการ Memory โดยไม่ใช้ Garbage Collector | ใช้ Garbage Collector จัดการ Memory อัตโนมัติ |
| **Safety** | Module เป็น Private โดย Default และ Compiler ตรวจสอบ Ownership/Borrowing | ใช้ Access Modifiers เช่น `public`, `private`, `protected` และ JVM ช่วยจัดการ Memory |

### Example

#### Rust

```rust
mod food {
    pub fn order() {
        println!("Order: Pizza");
    }
}

fn main() {
    food::order();
}
```

#### Java

```java
package food;

public class Food {
    public static void order() {
        System.out.println("Order: Pizza");
    }
}
```

```java
import food.Food;

public class Main {
    public static void main(String[] args) {
        Food.order();
    }
}
```

### Analysis

Rust แยกโครงสร้างเป็น **Package → Crate → Module** อย่างชัดเจน  
ขณะที่ Java เน้น **Package → Class/Interface**

ในเชิง PPL ทั้งคู่สนับสนุนแนวคิด:

- **Modularity** — แบ่งโปรแกรมออกเป็นส่วนย่อย
- **Abstraction** — ซ่อนรายละเอียดการทำงานภายใน
- **Scope** — กำหนดขอบเขตการเข้าถึงชื่อ
- **Namespace** — ป้องกันชื่อชนกัน
- **Information Hiding** — ควบคุมว่าส่วนใดสามารถเข้าถึงจากภายนอกได้

Rust ใช้ `pub` สำหรับเปิดเผยสมาชิกของ Module และใช้ Ownership/Borrowing
เพื่อช่วยตรวจสอบ Memory Safety ตั้งแต่ Compile Time

---

## 2. Rust vs C

| Aspect | Rust | C |
|---|---|---|
| **Syntax** | ใช้ `mod`, `use` และ `pub` ในการสร้าง ใช้งาน และควบคุม Module | ใช้ `.c`, `.h` และ `#include` เพื่อแบ่งและเชื่อมส่วนของโปรแกรม |
| **Semantics / Behavior** | มี Package, Crate และ Module System เป็นโครงสร้างที่ภาษารองรับโดยตรง | ไม่มี Module/Package System แบบ Rust โดยตรง ใช้ Source/Header Files และ Linkage แทน |
| **Type System** | Static และ Strong Typing พร้อมการตรวจสอบ Type ที่เข้มงวด | Static Typing แต่รองรับ Implicit Conversions และ Low-level Operations มากกว่า |
| **Memory Management** | Ownership และ Borrowing จัดการ Memory โดยไม่ใช้ Garbage Collector | Programmer จัดการ Memory เอง เช่น `malloc()` และ `free()` |
| **Safety** | Compiler ตรวจสอบ Ownership, Borrowing และ Lifetime ช่วยป้องกัน Memory Errors | Programmer ต้องรับผิดชอบ Memory Safety มากกว่า |

### Example

#### Rust

```rust
mod food {
    pub fn order() {
        println!("Order: Pizza");
    }
}

fn main() {
    food::order();
}
```

#### C

**food.h**

```c
void order();
```

**food.c**

```c
#include <stdio.h>
#include "food.h"

void order() {
    printf("Order: Pizza\n");
}
```

**main.c**

```c
#include "food.h"

int main() {
    order();
    return 0;
}
```

### Analysis

Rust มี **Module และ Crate System** เป็นส่วนหนึ่งของภาษา ทำให้
**Scope, Namespace และ Visibility** ถูกควบคุมอย่างเป็นระบบ

ในขณะที่ C ไม่มี Module System แบบ Rust โดยตรง แต่ใช้:

- `.h` สำหรับประกาศ Interface
- `.c` สำหรับ Implementation
- `#include` สำหรับนำเนื้อหาจาก Header File มาใช้
- Linkage สำหรับเชื่อมส่วนต่าง ๆ ของโปรแกรม

ในเชิง PPL Rust จึงเน้น **Abstraction, Modularity และ Compile-time Safety**
โดย Compiler สามารถตรวจสอบ Ownership, Borrowing และ Lifetime ได้

ส่วน C ให้ Programmer ควบคุม Memory โดยตรง จึงมีความยืดหยุ่นสูง
แต่ Programmer ต้องรับผิดชอบ Memory Safety มากกว่า

---

## 3. Rust vs Python

| Aspect | Rust | Python |
|---|---|---|
| **Syntax** | ใช้ `mod`, `use` และ `pub` สำหรับ Module และการเข้าถึง | ใช้ไฟล์ `.py` เป็น Module และ `import` เพื่อนำ Module/ชื่อมาใช้ |
| **Semantics / Behavior** | Package ประกอบด้วย Crate และภายใน Crate สามารถแบ่งเป็น Modules | Module โดยทั่วไปคือไฟล์ `.py` และ Package ใช้รวมหลาย Modules |
| **Type System** | Static และ Strong Typing ตรวจสอบ Type ตอน Compile | Dynamic และ Strong Typing โดยตรวจสอบ Type ขณะ Runtime |
| **Memory Management** | ใช้ Ownership และ Borrowing โดยไม่พึ่ง Garbage Collector | จัดการ Memory อัตโนมัติ โดย CPython ใช้ Reference Counting ร่วมกับ Garbage Collector |
| **Safety** | ตรวจสอบ Type, Ownership และ Borrowing ก่อน Run และใช้ Visibility Control | จัดการ Memory ให้อัตโนมัติ แต่ Type Error หลายชนิดเกิดได้ขณะ Runtime |

### Example

#### Rust

```rust
mod food {
    pub fn order() {
        println!("Order: Pizza");
    }
}

fn main() {
    food::order();
}
```

#### Python

**food.py**

```python
def order():
    print("Order: Pizza")
```

**main.py**

```python
import food

food.order()
```

### Analysis

Rust และ Python รองรับ **Module และ Package**
เพื่อแบ่งโปรแกรมออกเป็นส่วนย่อยเหมือนกัน แต่มีโครงสร้างและการทำงานต่างกัน

Rust มีโครงสร้าง:

```text
Package
   ↓
Crate
   ↓
Module
```

ส่วน Python สามารถมองแบบง่าย ๆ ได้ว่า:

```text
Package
   ↓
Module (.py)
   ↓
Function / Class
```

Rust มี **Crate เป็น Compilation Unit** และตรวจสอบ Type, Ownership
และ Borrowing ตอน Compile

ส่วน Python เป็นภาษาแบบ Dynamic จึงมีการตรวจสอบหลายอย่างขณะ Runtime

ในเชิง PPL ทั้งสองสนับสนุน:

- **Modularity**
- **Abstraction**
- **Namespace**
- **Scope**
- **Information Hiding**

แต่ Rust เน้น **Static Checking และ Compile-time Safety**
มากกว่า Python

---

# PPL Analysis Summary

เมื่อเปรียบเทียบ Rust กับ Java, C และ Python จะเห็นว่าแต่ละภาษา
มีวิธีจัดโครงสร้างโปรแกรมแตกต่างกัน

```text
Rust
Package
└── Crate
    └── Module
        └── Function / Struct / Enum


Java
Package
└── Class / Interface
    └── Method


C
Source / Header Files
├── .h
└── .c
    └── Function


Python
Package
└── Module (.py)
    └── Function / Class
```

## PPL Concepts ที่เกี่ยวข้อง

### 1. Modularity

แบ่งโปรแกรมขนาดใหญ่ออกเป็นส่วนย่อย ทำให้โค้ดเป็นระเบียบ
และแต่ละส่วนมีหน้าที่ชัดเจน

ตัวอย่าง Rust:

```rust
mod food {
    // จัดการอาหาร
}

mod drink {
    // จัดการเครื่องดื่ม
}
```

---

### 2. Abstraction

ผู้ใช้งาน Module ไม่จำเป็นต้องรู้รายละเอียดการทำงานทั้งหมดภายใน Module

```rust
mod food {
    fn prepare() {
        println!("Preparing food");
    }

    pub fn order() {
        prepare();
        println!("Order: Pizza");
    }
}
```

ผู้ใช้งานเพียงเรียก:

```rust
food::order();
```

โดยไม่จำเป็นต้องรู้ว่า `prepare()` ทำงานอย่างไร

---

### 3. Namespace

Module ช่วยป้องกันปัญหาชื่อ Function ชนกัน

```rust
mod food {
    pub fn order() {
        println!("Pizza");
    }
}

mod drink {
    pub fn order() {
        println!("Cola");
    }
}

fn main() {
    food::order();
    drink::order();
}
```

แม้ทั้งสอง Module จะมี Function ชื่อ `order()` เหมือนกัน
แต่ไม่เกิดปัญหา เพราะอยู่คนละ Namespace

```text
food::order()
drink::order()
```

---

### 4. Scope

Scope กำหนดว่าชื่อหรือ Function สามารถมองเห็น
และเรียกใช้งานได้จากบริเวณใดของโปรแกรม

Module ของ Rust ช่วยสร้างขอบเขตของชื่อภายในโปรแกรม

---

### 5. Information Hiding

Rust กำหนดสมาชิกของ Module เป็น Private โดย Default

```rust
mod food {
    fn prepare() {
        // Private
    }

    pub fn order() {
        // Public
    }
}
```

`prepare()` ถูกซ่อนจากภายนอก Module

แต่ `order()` มี `pub` จึงสามารถเรียกจากภายนอกได้

---

# Conclusion

Rust มีโครงสร้าง **Package → Crate → Module**
ที่แยกหน้าที่ของแต่ละระดับอย่างชัดเจน

เมื่อเปรียบเทียบกับภาษาอื่น:

- **Java** ใช้ Package และ Class/Interface
- **C** ใช้ Source File, Header File และ Linkage
- **Python** ใช้ Package และ Module

ในมุมมองของ PPL ระบบ Modules, Packages และ Crates ของ Rust
เกี่ยวข้องกับ **Modularity, Abstraction, Scope, Namespace และ
Information Hiding**

นอกจากนี้ Rust ยังใช้ **Ownership และ Borrowing**
เพื่อเพิ่ม Memory Safety โดยตรวจสอบตั้งแต่ Compile Time