**Definitions**
**Concurrency** - in computer science, concurrency is the ability for different parts or units of  program, algorithm, or problem to be executed out-of-order or at the same time simultaneously partial order, without affecting the final outcome.

**Mutexes or (mutual exclusion)** - In computer science, mutual exclusion is a property of concurrency control, which is instituted for the purpose of **preventing race  conditions**.

**Semaphores** - in computer science, a semaphore is a variable or abstract data  type used to control access to a common resource by multiple threads and avoid critical section problems in a concurrent system such as a  multitasking operating system.

**JVM** - Java virtual machine

**CSP** - (Communicating Sequential Processes)

**Understanding concurrency**
Problems that can arise from `concurrency` in go is accessing resources. If we change the value of a variable in go we might have inconsistency with that variable. solutions included **mutexes, semaphores, locks**. This locks the resource while another is accessing it.

**GoRoutines**
 only exist in the JVM threads of go runtime controlled by Go runtime, not OS. It is as simple as using the `go` keyword.

**GoRoutine communication**
go concurrency does not communicate by sharing memory; instead, share memory by communicating. use channels over shared resources

