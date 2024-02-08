# Generic information about Javascript

### 1. Garbage Collections
- Javascript has automatic memory management system called `Garbage Collector`.
- `Garbage Collector` uses the concept of `reachability` to determine whether the memory could be cleaned or not.
- `Reachable value` is a value which can be accessed or utilized. `Reachable values` are not removed from the memory.
- `Garbage Collector` uses `Mark-and-Sweep` algorithm to determine resources reachable from the root.
- Extra optimization approaches of `Garbage Collection`:
    - `Generational collection (세대별 수집)`: **Most of the objects in `javascript` have a short life span**. They're created, used, and soon cleared from the memory. Therefore it's reasonable to **split objects into 2 sets (old and new), strongly examine new ones, and examine less the old ones** since they have higher possibility to be resued.
    - `Incremental collection (점진적 수집)`: Examining objects all at once is a heavy task, so the **engine splits the exiting objects into multiple parts and perform `Garbage collection` by groups one after another**.
    - `idle-time collection (유휴 시간 수집)`: **Only perform `Garbage Collection` when `CPU` is idle (resting)** - to prevent `Garbage Collection` from critically affecting the main performance.
- Detailed memory collection approaches could be different by the environment (eg. browser engines).