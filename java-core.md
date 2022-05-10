# BASIC 

## 1. Differences between JDK,JRE and JVM?
### JVM
- JVM (shortening for Java Virtual Machine) is abstract device(vitural) help computer run java programming.It provides runtime environment in where Java ByteCodes an be executed.
### JRE
- JRE (là viết tắt của Java Runtime Environment) được sử dụng để cung cấp môi trường runtime. Nó là trình triển khai của JVM. JRE bao gồm tập hợp các thư viện và các file khác mà JVM sử dụng tại runtime. Trình triển khai của JVM cũng được công bố bởi các công ty khác ngoài Sun Micro Systems.

- JRE (Shortening for Java Runtime Environment) being used for providing runtime environment.It is implementation program of JVM.JRE contains a set of libraries and other files which JVM used at runtime.Implementation program of JVM is published by other companies other than **Sun Micro Systems**
### JDK
- JDK (là viết tắt của Java Development Kit) bao gồm JRE và các Development Tool.
- JDK (Shortening for Java Development Kit) contains JRE and other Development Tool.
## 2. Heap and Stack in JAVA
### Stack
- A stack is a linear data structure.
- A stack is a place in the computer memory where all the variables that are declared and initialized **before** runtime are stored
- Data is stored in stack using the Last In First Out (LIFO) method
- Storage in memory is allocated and deallocated at only one end of the memory called the ***top of the stack*** 
- Stack is a section of memory and it associated registers that is used for temporary storage of information in which the most recently stored items is the first to be retrieved.