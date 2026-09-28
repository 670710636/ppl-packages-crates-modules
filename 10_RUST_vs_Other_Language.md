## 10. Rust vs. Other Languages

| Aspect | Rust | Java | C | Python |
|---|---|---|---|---|
| **Program Organization** | Package → Crate → Module → Item | Package → Class / Interface | Source File + Header File | Package → Module |
| **Syntax** | ใช้ `mod` สร้าง Module, `use` นำชื่อเข้ามาใน Scope และ `pub` กำหนด Visibility | ใช้ `package` จัดกลุ่ม Class/Interface, `import` นำ Type มาใช้ และ Access Modifier ควบคุมการเข้าถึง | ใช้ `.c`, `.h`, `#include` รวมถึง `static` และ `extern` ในการจัดโครงสร้างและควบคุม Linkage | ใช้ไฟล์ `.py` เป็น Module และใช้ `import` หรือ `from ... import ...` นำชื่อมาใช้ |
| **Semantics / Behavior** | Package เป็นหน่วยของ Cargo Project, Crate เป็นหน่วย Compilation และ Module ใช้แบ่งโครงสร้าง/Namespace ภายใน Crate | Package ใช้จัดกลุ่ม Type และสร้าง Namespace ส่วน Class/Interface เป็นโครงสร้างสำคัญของโปรแกรม | ไม่มี Module System แบบ Rust โดยตรง การแบ่งโปรแกรมอาศัย Translation Unit, Header File และ Linkage | Package ใช้รวม Module ส่วน Module สร้าง Namespace และถูกโหลดผ่านระบบ Import ใน Runtime |
| **Visibility** | Item เป็น Private โดย Default และใช้ `pub`, `pub(crate)`, `pub(super)` ฯลฯ เพื่อเปิดการเข้าถึง | ใช้ `public`, `private`, `protected` และ package-private | ใช้ Scope, Linkage เช่น `static`/`extern` และการออกแบบ Header/API | ไม่มี Access Control แบบ Rust; `_name` ใช้เป็น Convention เพื่อสื่อว่าเป็น Internal |
| **Type System** | Static + Strong Typing; Module System ควบคุมว่า Type/Member ใดสามารถมองเห็นได้ | Static + Strong Typing; Access Control ควบคุมการเข้าถึง Class และ Member | Static Typing และรองรับ Low-level Operations/Implicit Conversions มากกว่า Rust | Dynamic + Strong Typing; Type หลายกรณีถูกตรวจขณะ Runtime |
| **Memory Management** | Ownership, Borrowing และ RAII โดยไม่มี Garbage Collector | Garbage Collector ของ JVM จัดการ Memory อัตโนมัติ | Programmer จัดการ Memory เอง เช่น `malloc()` และ `free()` | Automatic Memory Management; CPython ใช้ Reference Counting ร่วมกับ Garbage Collection |
| **Safety** | Compiler ตรวจ Type, Visibility, Ownership และ Borrowing ตั้งแต่ Compile Time | Compiler ตรวจ Type และ Access Control ขณะที่ JVM/GC ช่วยด้าน Runtime และ Memory Safety | ให้ Low-level Control สูง แต่ Programmer ต้องรับผิดชอบ Pointer และ Memory Safety มากกว่า | จัดการ Memory ให้อัตโนมัติ แต่ Type/Attribute/Import Error หลายกรณีพบใน Runtime |
| **Namespace** | Module สร้าง Namespace เช่น `crate::food::order` | Package/Class ช่วยแบ่ง Namespace เช่น `food.Food` | ไม่มี Namespace แบบ Rust โดยตรง | แต่ละ Module มี Namespace ของตนเอง เช่น `food.order` |
| **Encapsulation** | Module + Visibility ใช้ซ่อน Implementation และเปิด Public API | Class + Access Modifier ใช้ควบคุมการเข้าถึง | ใช้ File, `static`, opaque types และ API/Header Design | ส่วนใหญ่ใช้ Naming Convention และ API Design |
| **Main PPL Characteristic** | Compile-time Safety + Explicit Modularity | OOP / Class-oriented Organization | Low-level Control | Dynamic Flexibility |

---

## Code Examples

เพื่อให้เห็นความแตกต่างชัดเจน ตัวอย่างทุกภาษาจะทำงานเหมือนกัน คือสร้างส่วน `food`
ที่มี `order()` สำหรับแสดงข้อความ:

```text
Order: Pizza
```

---

### Rust Example

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

**โครงสร้าง**

```text
Package
└── Crate
    └── Module: food
        └── Function: order()
```

- `mod food` สร้าง Module
- `pub` ทำให้ `order()` สามารถเรียกจากภายนอก Module ได้
- `food::order()` เรียก Function ผ่าน Module Path

---

### Java Example

**Food.java**

```java
package food;

public class Food {
    public static void order() {
        System.out.println("Order: Pizza");
    }
}
```

**Main.java**

```java
import food.Food;

public class Main {
    public static void main(String[] args) {
        Food.order();
    }
}
```

**โครงสร้าง**

```text
Package: food
└── Class: Food
    └── Method: order()
```

- `package food` กำหนด Package
- `public` กำหนดการเข้าถึง
- `import food.Food` นำ Class มาใช้
- `Food.order()` เรียก Method ผ่าน Class

---

### C Example

**food.h**

```c
#ifndef FOOD_H
#define FOOD_H

void order(void);

#endif
```

**food.c**

```c
#include <stdio.h>
#include "food.h"

void order(void) {
    printf("Order: Pizza\n");
}
```

**main.c**

```c
#include "food.h"

int main(void) {
    order();
    return 0;
}
```

**โครงสร้าง**

```text
food.h
└── Function Declaration

food.c
└── Function Definition

main.c
└── Function Call
```

- `.h` ใช้ประกาศ Interface
- `.c` ใช้เก็บ Implementation
- `#include` นำเนื้อหาจาก Header มาใช้ใน Translation Unit
- ไม่มี `mod` และ `pub` แบบ Rust

---

### Python Example

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

**โครงสร้าง**

```text
Module: food.py
└── Function: order()

main.py
└── import food
    └── food.order()
```

- `food.py` เป็น Module
- `import food` นำ Module มาใช้
- `food.order()` เรียก Function ผ่าน Namespace ของ Module
- ไม่มี `pub` แบบ Rust

---

## Code Comparison Summary

| Language | การแบ่งโปรแกรม | การควบคุมการเข้าถึง | การนำมาใช้ | การเรียก |
|---|---|---|---|---|
| **Rust** | `mod food` | `pub` | `use` (เมื่อจำเป็น) | `food::order()` |
| **Java** | `package food` + `class Food` | `public`, `private`, `protected` | `import food.Food` | `Food.order()` |
| **C** | `food.h` + `food.c` | Scope / Linkage เช่น `static`, `extern` | `#include "food.h"` | `order()` |
| **Python** | `food.py` | Convention เช่น `_name` | `import food` | `food.order()` |

---

## Analysis

จากตัวอย่างจะเห็นว่าทั้ง 4 ภาษาใช้แนวคิด **Modularity** เพื่อแบ่งโปรแกรมออกเป็นส่วนย่อยเหมือนกัน แต่ใช้กลไกต่างกัน

```text
Rust                    Java
Package                 Package
└── Crate               └── Class
    └── Module              └── Method
        └── Function


C                       Python
Header + Source         Package
└── Function            └── Module
                            └── Function / Class
```

**Rust** มี Package, Crate และ Module System เป็นโครงสร้างที่รองรับโดยภาษา และใช้ `pub` ควบคุม Visibility โดย Compiler สามารถตรวจสอบ Scope และการเข้าถึงได้ตั้งแต่ Compile Time

**Java** เน้น Package และ Class/Interface และใช้ Access Modifier เช่น `public`, `private` และ `protected` เพื่อควบคุมการเข้าถึง

**C** ไม่มี Module System แบบ Rust โดยตรง แต่ใช้ Source File, Header File, Scope และ Linkage ในการแบ่ง Interface และ Implementation

**Python** ใช้ไฟล์ `.py` เป็น Module และ `import` เพื่อนำ Module มาใช้ แต่ไม่มี Visibility Control ที่บังคับแบบ `pub` ของ Rust โดยมักใช้ Convention เช่น `_name`

ในมุมมอง PPL ความแตกต่างสำคัญของ Rust คือการรวม **Modularity, Namespace, Scope, Visibility และ Information Hiding** เข้ากับการตรวจสอบของ Compiler และยังทำงานร่วมกับ **Ownership และ Borrowing** เพื่อเพิ่ม Compile-time และ Memory Safety
