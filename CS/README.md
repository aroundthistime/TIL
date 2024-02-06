# Computer Science knowledge applied to overally regardless of domains

### 1. Memory Layout
![Memory Layout Image](image.png)
- **Text/Code Segment (텍스트 영역 혹은 코드 영역)**: **Where codes of the program to execute is located**. When user executes a program, `OS` takes code from `HARD DISK` and places it here. Then the `CPU` would handle the operation inside here one by one.
- **Data Segment (데이터 영역)**: Where **`global variables` and `static variables`** are stored.
- **Heap Segment (힙 영역)**: Where **data created during `runtime`** is stored, and therefore the **size of this will be decided on `runtime`**. It is a memory area where programmer can manage, but for some languages (eg. `javascript`, `Java`) garbage collector does the job automatically.
- **Stack Segment (스택 영역)**: Where **`local variables` and `arguments` will be stored when a function is called**. The data will be **popped when the function has been terminated**.
- Data allocation of `Stack Segment` and `Heap Segment` will be done on `runtime` (Dynamic memory allocation)