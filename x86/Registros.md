Vea también [[registros-cpu|8086 registros]]

![[registros-3.png]]

![[registros-1.png]]

![[registros-2.png]]

[https://learn.microsoft.com/es-es/windows-hardware/drivers/debugger/x64-architecture](https://learn.microsoft.com/es-es/windows-hardware/drivers/debugger/x64-architecture)

La arquitectura x64 es una extensión compatible con versiones anteriores de x86. Proporciona un nuevo modo de 64 bits y un modo heredado de 32 bits, que es idéntico a x86.

El término "x64" incluye AMD 64 e Intel64. Los conjuntos de instrucciones son casi idénticos.
## Registros

x64 amplía los registros de uso general de x86 para que sean de 64 bits y agrega registros de 64 bits nuevos. Los registros de 64 bits tienen nombres que comienzan por "r". Por ejemplo, la extensión de 64 bits de **eax** se denomina **rax**. Los nuevos registros se denominan **r8** a **r15**.

Los 32 bits inferiores, 16 bits y 8 bits de cada registro son direccionables directamente en operandos. Esto incluye registros, como **esi**, cuyos 8 bits inferiores no eran direccionables anteriormente. En la tabla siguiente se especifican los nombres de lenguaje de ensamblado para las partes inferiores de los registros de 64 bits.

| Registro de 64 bits | 32 bits inferiores | 16 bits inferiores | 8 bits inferiores |
| ------------------- | ------------------ | ------------------ | ----------------- |
| Rax                 | Eax                | ax                 | al                |
| Rbx                 | Ebx                | Bx                 | bl                |
| rcx                 | ecx                | Cx                 | cl                |
| rdx                 | Edx                | Dx                 | Dl                |
| Rsi                 | Esi                | si                 | sil               |
| Rdi                 | Edi                | di                 | Dil               |
| Rbp                 | Ebp                | bp                 | bpl               |
| Rsp                 | Esp                | sp                 | Spl               |
| r8                  | r8d                | r8w                | r8b               |
| r9                  | r9d                | r9w                | r9b               |
| r10                 | r10d               | r10w               | r10b              |
| r11                 | r11d               | r11w               | r11b              |
| r12                 | r12d               | r12w               | r12b              |
| r13                 | r13d               | r13w               | r13b              |
| r14                 | r14d               | r14w               | r14b              |
| r15                 | r15d               | r15w               | r15b              |
Las operaciones que se generan en un subsegaréster de 32 bits se extienden automáticamente a cero en todo el registro de 64 bits. Las operaciones que se generan en los subsésteres de 8 o 16 bits no se extienden con cero (este comportamiento es compatible con x86).

Los 8 bits altos de **ax**, **bx**, **cx** y **dx** siguen siendo direccionables como **ah**, **bh**, **ch**, **dh** , pero no se pueden usar con todos los tipos de operandos.

El registro **eip** y **flags** del puntero de instrucción se han ampliado a 64 bits (**rip** y **rflags**, respectivamente).

El procesador x64 también proporciona varios conjuntos de registros de punto flotante:

- Ocho registros x80 bits x87.
- Ocho registros MMX de 64 bits. (Estos registros se superponen con los registros x87).
- El conjunto original de ocho registros SSE de 128 bits se incrementa a dieciséis.
## Convenciones de llamada

A diferencia de x86, el compilador de C/C++ solo admite una convención de llamada en x64. Esta convención de llamada aprovecha el mayor número de registros disponibles en x64:

- Los cuatro primeros parámetros enteros o de puntero se pasan en los registros **rcx**, **rdx**, **r8** y **r9** .

- Los cuatro primeros parámetros de punto flotante se pasan en los cuatro primeos registros SSE, **xmm0**-**xmm3**.

- El autor de la llamada reserva espacio en la pila para los argumentos pasados en los registros. La función llamada puede usar este espacio para volcar el contenido de los registros en la pila.

- Los argumentos adicionales se pasan en la pila.

- Se devuelve un valor devuelto de entero o puntero en el registro **rax** , mientras que se devuelve un valor devuelto de punto flotante en **xmm0**.

- **rax**, **rcx**, **rdx**, **r8**-**r11** son volátiles.

- **rbx**, **rbp**, **rdi**, **rsi**, **r12**-**r15** son no volátiles.

La convención de llamada para C++ es similar. **Este puntero** se pasa como primer parámetro implícito. Los tres parámetros siguientes se pasan en los registros restantes, mientras que el resto se pasan en la pila.
## Modos de direccionamiento

Los modos de direccionamiento en modo de 64 bits son similares, pero no idénticos a x86.

- Las instrucciones que hacen referencia a registros de 64 bits se realizan automáticamente con precisión de 64 bits. Por ejemplo, **mov rax, [rbx]** mueve 8 bytes a partir de **rbx** a **rax**.

- Se ha agregado una forma especial de la instrucción **mov** para constantes inmediatas de 64 bits o direcciones constantes. Para todas las demás instrucciones, las constantes inmediatas o las direcciones constantes siguen siendo de 32 bits.

- x64 proporciona un nuevo modo de direccionamiento relativo a **la extracción**. Las instrucciones que hacen referencia a una sola dirección constante se codifican como desplazamientos de **rip**. Por ejemplo, la instrucción **mov rax, [**_addr_**]** mueve 8 bytes a partir de la**extracción**_del addr_ + a **rax**.

Instrucciones, como **jmp**, **llamada**, **inserción** y **pop**, que hacen referencia implícitamente al puntero de instrucción y el puntero de pila los tratan como registros de 64 bits en x64.
## Consulte también

- [X86-64 Wikipedia](https://en.wikipedia.org/wiki/X86-64)
    
- [Recursos para desarrolladores de AMD 64](https://developer.amd.com/resources/)
    
- [Intel: introducción al ensamblado x64](https://software.intel.com/content/www/us/en/develop/articles/introduction-to-x64-assembly.html)
    
- [x64 Primer: todo lo que necesita saber para empezar a programar sistemas Windows de 64 bits - Matt Pietrek](https://learn.microsoft.com/es-es/archive/msdn-magazine/2006/may/x64-starting-out-in-64-bit-windows-systems-with-visual-c)
    
- [La historia de las convenciones de llamada, parte 5: amd64 Raymond Chen](https://devblogs.microsoft.com/oldnewthing/20040114-00/?p=41053)