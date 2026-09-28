## 10. Rust vs. Other Languages

**Comparison Language:** Java / C / Python

| Aspect | Rust | Java | C | Python |
|---|---|---|---|---|
| **Syntax** | ใช้ `mod` เพื่อประกาศ Module, `use` เพื่อนำชื่อจาก Module อื่นเข้ามาใช้ใน Scope และ `pub` เพื่อกำหนดให้ Item สามารถเข้าถึงจากภายนอกได้ ส่วน Package และ Crate ถูกจัดการผ่านโครงสร้างของ Cargo Project | ใช้ `package` เพื่อระบุว่า Class หรือ Interface อยู่ใน Package ใด และใช้ `import` เพื่อนำ Class หรือ Type จาก Package อื่นมาใช้งาน การเข้าถึงควบคุมด้วย `public`, `private`, `protected` เป็นต้น | แบ่งโปรแกรมออกเป็น Source File `.c` และ Header File `.h` ใช้ `#include` เพื่อนำ Declaration จาก Header มาใช้ และใช้ `static` / `extern` เพื่อควบคุมการมองเห็นและ Linkage | ไฟล์ `.py` แต่ละไฟล์สามารถเป็น Module และสามารถรวมหลาย Module เป็น Package ใช้ `import` หรือ `from ... import ...` เพื่อนำ Module หรือชื่อที่ต้องการมาใช้งาน |
| **Semantics / Behavior** | **Package** คือหน่วยของ Cargo Project ที่สามารถมี Crate ได้ ส่วน **Crate** เป็นหน่วยที่ Compiler นำไป Compile และ **Module** ใช้จัดโครงสร้างและ Namespace ภายใน Crate ทำให้แบ่งส่วนของโปรแกรมออกเป็นหมวดหมู่ได้ | **Package** ใช้จัดกลุ่ม Class และ Interface ที่เกี่ยวข้องกัน รวมทั้งช่วยสร้าง Namespace เพื่อป้องกันชื่อชนกัน ส่วน Class/Interface เป็นโครงสร้างหลักที่เก็บข้อมูลและพฤติกรรมของโปรแกรม | C ไม่มี Module System ในรูปแบบเดียวกับ Rust โดยทั่วไปจะแบ่งโปรแกรมเป็นหลาย Source File และ Header File แต่ละ `.c` ถูก Compile เป็น Translation Unit ก่อนนำ Object Files มา Link รวมกันเป็นโปรแกรม | **Module** คือไฟล์ Python ที่เก็บ Code เช่น Function หรือ Class ส่วน **Package** ใช้จัดกลุ่มหลาย Module เมื่อใช้ `import` Python จะค้นหาและโหลด Module เพื่อให้สามารถเรียกใช้สิ่งที่อยู่ภายในได้ |
| **Type System** | เป็น **Static Typing** และ **Strong Typing** โดย Compiler ตรวจสอบ Type ตั้งแต่ Compile Time ส่วน Module System ช่วยควบคุมว่า Type, Function หรือ Item ใดสามารถเข้าถึงได้จากส่วนอื่นของโปรแกรม | เป็น **Static Typing** และ **Strong Typing** โดย Type ของตัวแปรและ Method ถูกตรวจสอบตอน Compile และ Access Modifier ใช้กำหนดว่าสมาชิกใดสามารถเข้าถึงจากส่วนอื่นได้ | เป็น **Static Typing** คือ Type ถูกกำหนดและตรวจสอบระหว่างการ Compile แต่ C อนุญาตการทำงานระดับต่ำและการแปลง Type บางรูปแบบได้มากกว่า Rust | เป็น **Dynamic Typing** โดยทั่วไปไม่จำเป็นต้องประกาศ Type ของตัวแปรล่วงหน้า และ Type ของ Object ถูกตรวจสอบขณะโปรแกรมทำงาน |
| **Memory Management** | ใช้ระบบ **Ownership และ Borrowing** ร่วมกับแนวคิด RAII เพื่อจัดการทรัพยากร โดยไม่จำเป็นต้องมี Garbage Collector เมื่อค่าหมด Scope ทรัพยากรที่เป็นเจ้าของจะถูกปล่อยตามกฎของภาษา | Memory ส่วนใหญ่ถูกจัดการอัตโนมัติด้วย **Garbage Collector (GC)** ของ JVM ซึ่งตรวจหา Object ที่ไม่ถูกใช้งานแล้วและคืน Memory ให้ระบบ | Programmer มีหน้าที่จัดการ Dynamic Memory โดยตรง เช่น จอง Memory ด้วย `malloc()` และคืนด้วย `free()` หากจัดการไม่ถูกต้องอาจเกิด Memory Leak หรือปัญหาจาก Pointer ได้ | Python จัดการ Memory ให้อัตโนมัติ โดยใน CPython ใช้ **Reference Counting** และมี **Garbage Collector** ช่วยจัดการ Reference Cycle |
| **Safety** | เน้นการตรวจสอบตั้งแต่ **Compile Time** เช่น Type, Visibility, Ownership และ Borrowing ช่วยป้องกันปัญหาหลายประเภทก่อนโปรแกรมทำงาน | Compiler ช่วยตรวจ Type และ Access Control ขณะที่ JVM และ Garbage Collector ช่วยจัดการ Memory ทำให้ Programmer ไม่ต้องจัดการ Pointer และคืน Memory โดยตรงเหมือน C | ให้ Programmer ควบคุม Memory และ Pointer ได้โดยตรง จึงมีความยืดหยุ่นสูง แต่ต้องรับผิดชอบ Memory Safety เองมากกว่า เช่น Dangling Pointer, Invalid Memory Access และ Memory Leak | Memory ถูกจัดการอัตโนมัติ จึงไม่ต้องใช้ `malloc()`/`free()` โดยตรง แต่ข้อผิดพลาดบางประเภท เช่น Type Error, Attribute Error หรือ Import Error อาจตรวจพบเมื่อ Runtime |

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
