[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/EbtZGzoI)
[![Open in Codespaces](https://classroom.github.com/assets/launch-codespace-2972f46106e565e64193e422d61a12cf1da4916b45550586e14ef0a7c637dd04.svg)](https://classroom.github.com/open-in-codespaces?assignment_repo_id=23666214)

## 👨‍💻 Autor
- Nombre: CAB PIÑON ISURY MICHELLE  
- NO.CONTROL: 24210475


# 📄 README.md (VERSIÓN FINAL – VARIANTE C)

````md
# Práctica 4.2 – Mini Cloud Log Analyzer en ARM64

## Variante C: Detección del primer evento crítico (503)

---

## 📌 Descripción

Este proyecto implementa un analizador de logs en **ARM64 Assembly** que procesa códigos de estado HTTP desde la entrada estándar (`stdin`) y detecta el **primer evento crítico (503)**.

Cuando el programa encuentra el código `503`, imprime un mensaje indicando el evento crítico y termina su ejecución.

---

## ⚙️ Funcionamiento

El programa realiza:

1. Lectura de datos desde `stdin`
2. Conversión de ASCII → entero
3. Comparación con el valor `503`
4. Detección del primer evento crítico
5. Terminación inmediata del programa

---

## 🧠 Lógica (equivalente en C)

```c
int code;
while (scanf("%d", &code)) {
    if (code == 503) {
        printf("CRITICAL FOUND\n");
        break;
    }
}
````

En ARM64 se implementa mediante:

* `read` syscall
* uso de registros (`x0-x8`)
* instrucción `CMP`
* saltos condicionales (`B.EQ`)
* bucles con etiquetas

---

## 🏗️ Estructura del proyecto

```
.
├── analyzer.s
├── Makefile
├── logs.txt
└── README.md
```

---

## 🔧 Compilación

```bash
make
```

---

## ▶️ Ejecución

```bash
cat logs.txt | ./analyzer
```

---

## 🧪 Ejemplo de entrada (1000 datos)

Este archivo simula tráfico real de un sistema cloud:

```text
200
201
202
204
301
302
400
401
403
404
408
429
500
501
502
504
200
201
202
204
301
302
400
401
403
404
408
429
500
501
502
504
200
201
202
204
301
302
400
401
403
404
408
429
500
501
502
504
200
201
202
204
301
302
400
401
403
404
408
429
500
501
502
504
200
201
202
204
301
302
400
401
403
404
408
429
500
501
502
504
200
201
202
204
301
302
400
401
403
404
408
429
500
501
502
504
200
201
202
204
301
302
400
401
403
404
408
429
500
501
502
504
200
201
202
204
301
302
400
401
403
404
408
429
500
501
502
504
200
201
202
204
301
302
400
401
403
404
408
429
500
501
502
504
200
201
202
204
301
302
400
401
403
404
408
429
500
501
502
504
503
200
200
200
200
200
200
200
200
200
200
```

👉 Nota: El código `503` aparece cerca del final para simular detección tardía.

---

## ✅ Salida esperada

```
CRITICAL FOUND
```

---

## 🧾 Código ARM64

```asm
.global _start

.section .bss
buffer: .skip 16

.section .data
msg: .ascii "CRITICAL FOUND\n"
len = . - msg

.section .text

_start:

loop:
    mov x0, #0
    ldr x1, =buffer
    mov x2, #16
    mov x8, #63
    svc #0

    cmp x0, #0
    beq exit

    ldr x1, =buffer

    ldrb w2, [x1]
    sub w2, w2, #'0'
    mov w3, #100
    mul w2, w2, w3

    ldrb w4, [x1, #1]
    sub w4, w4, #'0'
    mov w5, #10
    mul w4, w4, w5

    ldrb w6, [x1, #2]
    sub w6, w6, #'0'

    add w2, w2, w4
    add w2, w2, w6

    mov w7, #503
    cmp w2, w7
    beq found

    b loop

found:
    mov x0, #1
    ldr x1, =msg
    mov x2, #len
    mov x8, #64
    svc #0

exit:
    mov x0, #0
    mov x8, #93
    svc #0
```

---

## 🛠️ Makefile

```make
AS = as
LD = ld

TARGET = analyzer
SRC = analyzer.s
OBJ = analyzer.o

all: $(TARGET)

$(TARGET): $(OBJ)
	$(LD) $(OBJ) -o $(TARGET)

$(OBJ): $(SRC)
	$(AS) $(SRC) -o $(OBJ)

run:
	cat logs.txt | ./$(TARGET)

clean:
	rm -f $(OBJ) $(TARGET)
```

---

## 🧪 Pruebas realizadas

* ✔ Sin código 503 → no hay salida
* ✔ Con código 503 → detección correcta
* ✔ Terminación inmediata al detectar evento crítico

---

## 📊 Conclusión

Se implementó un analizador de logs en ARM64 capaz de detectar eventos críticos utilizando:

* comparaciones a nivel máquina
* control de flujo manual
* syscalls de Linux
* manejo de memoria

Este enfoque permite comprender cómo los sistemas cloud procesan datos a bajo nivel.

---


