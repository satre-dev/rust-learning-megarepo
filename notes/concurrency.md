# concurrency notes

A series of key tools in belt here for handling concurrent tasks, shared access to memory, and thread safety.

- Mutex can be used to coodinate shared data strctures across multiple threads. Unable to access the data except when you (in control flow) hold the lock. Locks release automatically.
- Read-only data can be shared among several threads and non-modification is enforced at compile time, preventing errors here.
- After transferring ownership of some data structure to another thread (move), the compiler ensure that you have relinquished all access to this by enforcing non-modification of it thereafter.
