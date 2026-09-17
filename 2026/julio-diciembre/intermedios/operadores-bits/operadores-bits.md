# Operadores de Bits / Bitmask

**Autor:** Carlos Alberto Lara Hernandez - Kaarlarax

Los operadores bit a bit o _bitwise operators_ son operadores integrados en la mayoría de los lenguajes de programación y resultan ser muy eficientes.

Antes de comenzar entenderemos algo: todo lo que existe dentro de una computadora se almacena en bits, simples 1's y 0's. En el lenguaje de programación C++, existen operadores que los manipulan directamente. Un número entero en C++ lo podemos representar de la siguiente manera:

```text
S = 34 (base 10) = 100010 (base 2)
```

Un número entero se almacena en la memoria como una secuencia de bits. Por lo tanto, podemos utilizar enteros para representar un pequeño conjunto de valores booleanos. Todas las operaciones se realizarán a través de la manipulación de los bits del entero, lo que supone una elección mucho más eficiente en comparación con `vector<bool>`, `bitset` o `set<int>` de la STL de C++. La diferencia de velocidad es significativa en programación competitiva.

A continuación aprenderemos las operaciones fundamentales de manipulación de bits, analizaremos cada una con ejemplos visuales y descubriremos cómo las máscaras de bits (_bitmasks_) pueden ayudarnos a resolver problemas que parecen complejos de manera elegante y eficiente.

---

## 1. Representación Binaria

Un entero con signo de 32 (o 64) bits nos sirve para representar hasta 32 (o 64) elementos. Sin perder el ámbito generalista, todos los ejemplos mostrados a continuación utilizan un entero con signo de 32 bits, llamado $S$.

```text
  5|  4|  3|  2|  1|  0  <- índice (0 desde la derecha)
 32| 16|  8|  4|  2|  1  <- potencia de 2
  F|  E|  D|  C|  B|  A  <- etiqueta alfabética alternativa
```

**Ejemplo:**

$S = 34$ (base 10) = `100010` (base 2)

En el ejemplo anterior, el entero $S = 34$, o `100010` en binario, representa un pequeño conjunto $\{1, 5\}$ con un esquema de indexación basado en 0 (o $\{B, F\}$, utilizando la etiqueta alfabética alternativa), ya que los bits segundo y sexto (desde la derecha) de $S$ están activados.

> _Nota importante:_ para evitar problemas con la representación en complemento a dos, se deberían utilizar los enteros de 32 o 64 bits solo para máscaras de bits de 30 o 62 elementos, respectivamente.

---

## 2. Desplazamiento de Bits: `<<` y `>>`

Para multiplicar o dividir un entero por potencias de 2, basta con desplazar los bits hacia la izquierda (`<<`) o hacia la derecha (`>>`), respectivamente. Hay que tener en cuenta que el desplazamiento a la derecha provoca un truncado que supone el redondeo automático a la baja en la división, por ejemplo, $17 / 2 = 8$.

```cpp
int S = 34;              // S = 34 (base 10) = 100010 (base 2)
S = S << 1;             // S = S * 2 = 68 (base 10) = 1000100 (base 2)
S = S >> 2;             // S = S / 4 = 17 (base 10) = 10001 (base 2)
S = S >> 1;             // S = S / 2 = 8  (base 10) = 1000 (base 2)
                         // El LSB (bit menos significativo) desaparece
```

**Complejidad:** tiempo $O(1)$, memoria $O(1)$.

---

## 3. Activar un Bit (Set)

Para **activar** el elemento $j$-ésimo (con indexación basada en 0) en el conjunto, utilizamos la operación de bits OR: `S |= (1 << j)`.

```cpp
int S = 34;              // S = 34 (base 10) = 100010 (base 2)
int j = 3;               // Queremos activar el bit 3
// 1 << j = 8           // = 001000 <- el bit '1' se mueve a la izquierda 3 posiciones
S |= (1 << j);          // Operación OR: cierto si cualquiera de los bits lo es
// S = 42 (base 10)     // = 101010 (base 2)
```

Visualización:

```text
  S      = 100010   (34 en decimal)
  1 << 3 = 001000   (8 en decimal)
  ----------------
  OR     = 101010   (42 en decimal)
```

**Complejidad:** tiempo $O(1)$, memoria $O(1)$.

---

## 4. Comprobar un Bit (Check)

Para **comprobar** si el elemento $j$-ésimo está activado, utilizamos la operación de bits AND: `T = S & (1 << j)`.

- Si $T = 0$, entonces el elemento $j$-ésimo está **desactivado**.
- Si $T \neq 0$ (para ser exactos, $T = (1 \ll j)$), entonces el elemento $j$-ésimo está **activado**.

```cpp
int S = 42;              // S = 42 (base 10) = 101010 (base 2)

// Caso 1: Comprobar bit 3 (está activado)
int j = 3;
int T = S & (1 << j);   // 1 << 3 = 001000
                         // AND: cierto solo si ambos bits lo son
// T = 8 (base 10) = 001000 -> no es 0, tercer elemento ACTIVADO

// Caso 2: Comprobar bit 2 (está desactivado)
j = 2;
T = S & (1 << j);       // 1 << 2 = 000100
// T = 0 (base 10) = 000000 -> es 0, segundo elemento DESACTIVADO
```

Visualización:

```text
  S      = 101010   (42)        S      = 101010   (42)
  1 << 3 = 001000   (8)        1 << 2 = 000100    (4)
  ----------------            ----------------
  AND    = 001000   (8) ✓      AND    = 000000    (0) ✗
```

**Complejidad:** tiempo $O(1)$, memoria $O(1)$.

---

## 5. Desactivar un Bit (Clear)

Para **desactivar/limpiar** el elemento $j$-ésimo del conjunto, utilizamos la operación de bits AND combinada con NOT: `S &= ~(1 << j)`.

```cpp
int S = 42;              // S = 42 (base 10) = 101010 (base 2)
int j = 1;               // Queremos desactivar el bit 1
// ~(1 << j) = 111101   // ' ~ ' es el operador de bits NOT (invierte todos los bits)
S &= ~(1 << j);         // Operación AND con la máscara invertida
// S = 40 (base 10)     // = 101000 (base 2)
```

Visualización:

```text
  S        = 101010   (42)
  1 << 1   = 000010   (2)
  ~(1<<1)  = 111101   (invierte todos los bits)
  --------------------
  AND      = 101000   (40)
```

> _Nota:_ siempre usamos paréntesis en las operaciones de manipulación de bits para evitar errores accidentales debido a la precedencia de operadores.

**Complejidad:** tiempo $O(1)$, memoria $O(1)$.

---

## 6. Conmutar un Bit (Toggle)

Para **conmutar** (invertir el estado) del elemento $j$-ésimo del conjunto, utilizamos la operación de bits XOR: `S ^= (1 << j)`.

```cpp
// Caso 1: Activar un bit que estaba desactivado
int S = 40;              // S = 40 (base 10) = 101000 (base 2)
int j = 2;               // Queremos conmutar el bit 2
// 1 << j = 000100       // el bit '1' se mueve a la izquierda 2 posiciones
S ^= (1 << j);          // XOR: cierto si ambos bits son DIFERENTES
// S = 44 (base 10)     // = 101100 (base 2)

// Caso 2: Desactivar un bit que estaba activado
S = 40;                  // S = 40 (base 10) = 101000 (base 2)
j = 3;                   // Queremos conmutar el bit 3
// 1 << j = 001000
S ^= (1 << j);          // XOR: cierto si ambos bits son DIFERENTES
// S = 32 (base 10)     // = 100000 (base 2)
```

Visualización:

```text
  S      = 101000   (40)        S      = 101000   (40)
  1 << 2 = 000100   (4)        1 << 3 = 001000    (8)
  ----------------            ----------------
  XOR    = 101100   (44)        XOR    = 100000    (32)
  (se activó)                   (se desactivó)
```

**Complejidad:** tiempo $O(1)$, memoria $O(1)$.

---

## 7. Bit Menos Significativo Activado

Para obtener el valor del **bit menos significativo** de $S$ que está activado (el primero por la derecha), utilizamos: `T = (S & (-S))`.

```cpp
int S = 40;              // S  = 40  = 000...000101000 (32 bits)
                         // -S = -40 = 111...111011000 (complemento a dos)
int T = (S & (-S));     // Operación AND
// T = 8 (base 10)      // = 000...000001000 -> el tercer bit está activado
```

Visualización:

```text
  S  = 000...000101000   (40)
  -S = 111...111011000   (-40 en complemento a dos)
  ----------------------
  AND= 000...000001000   (8) -> bit 3 es el menos significativo activado
```

**Complejidad:** tiempo $O(1)$, memoria $O(1)$.

---

## 8. Activar Todos los Bits de un Conjunto de Tamaño $n$

Para activar todos los bits en un conjunto de tamaño $n$, utilizamos: `S = (1 << n) - 1`.

```cpp
// Ejemplo para n = 3
int n = 3;
int S = (1 << n) - 1;   // 1 << 3 = 8 = 1000, luego 8 - 1 = 7 = 111
// S = 7                 // = 111 (base 2) -> 3 bits activados

// Ejemplo para n = 5
n = 5;
S = (1 << n) - 1;       // 1 << 5 = 32 = 100000, luego 32 - 1 = 31 = 11111
// S = 31               // = 11111 (base 2) -> 5 bits activados
```

> _Precaución:_ cuidado con los desbordamientos cuando $n \geq 31$ (para `int`) o $n \geq 63$ (para `long long`).

**Complejidad:** tiempo $O(1)$, memoria $O(1)$.

---

## 9. Contar Bits Activados (Popcount)

Para contar cuántos bits están activados en $S$, podemos usar la función integrada del compilador GNU C++: `__builtin_popcount(S)`.

```cpp
int S = 42;                          // S = 42 = 101010 (base 2)
int activos = __builtin_popcount(S); // Cuenta los bits en 1
// activos = 3                       // Los bits 1, 3 y 5 están activados

// Para long long, usar:
// long long S = 1e18;
// int activos = __builtin_popcountll(S);
```

**Complejidad:** tiempo $O(1)$, memoria $O(1)$.

---

## 10. Comprobaciones Básicas

**¿Es par o impar?**

El bit menos significativo (LSB) determina la paridad: si es $1$, el número es impar; si es $0$, es par.

```cpp
bool isOdd  = (x & 1);     // true si es impar (el LSB es 1)
bool isEven = !(x & 1);    // true si es par  (el LSB es 0)
```

**¿Es potencia de 2?**

Un número $x$ es potencia de $2$ si y solo si tiene exactamente un bit encendido. Al restar $1$ a una potencia de $2$, todos los bits inferiores se encienden, por lo que el AND con el número original da $0$.

```cpp
bool isPowerOfTwo = (x != 0) && ((x & (x - 1)) == 0);
```

**Complejidad:** tiempo $O(1)$, memoria $O(1)$.

---

## 11. Módulo con Potencias de 2

Para calcular $x \pmod{2^k}$, basta con extraer los $k$ bits menos significativos usando una máscara de bits.

```cpp
int mod = x & ((1 << k) - 1);   // Retorna los k bits más a la derecha de x
```

Por ejemplo, $x \pmod{8}$ equivale a `x & 7` (ya que $8 = 2^3$ y la máscara es `111`).

**Complejidad:** tiempo $O(1)$, memoria $O(1)$.

---

## 12. Intercambio de Variables sin Variable Temporal (Swap con XOR)

El truco clásico de intercambio usando XOR se basa en las propiedades de auto-inversión del XOR: $A \oplus A = 0$ y $A \oplus 0 = A$.

```cpp
x ^= y;    // x = x ^ y
y ^= x;    // y = y ^ (x ^ y) = x
x ^= y;    // x = (x ^ y) ^ x = y
```

> _Precaución:_ este truco falla si `x` e `y` son la misma variable (mismo registro de memoria), ya que el resultado sería $0$. En la práctica, es preferible usar `std::swap` por legibilidad y seguridad.

**Complejidad:** tiempo $O(1)$, memoria $O(1)$.

---

## 13. Alternar una Variable entre Dos Valores Fijos

Si una variable $x$ alterna únicamente entre dos valores $A$ y $B$, podemos conmutarla en $O(1)$ sin condicionales.

```cpp
x = A ^ B ^ x;
```

Si $x = A$, entonces $A \oplus B \oplus A = B$. Si $x = B$, entonces $A \oplus B \oplus B = A$.

**Complejidad:** tiempo $O(1)$, memoria $O(1)$.

---

## 14. Propiedades Algebraicas y de Paridad

**Fórmulas de suma:**

Estas identidades son útiles cuando un problema proporciona el XOR o el AND de dos números y necesitas deducir la suma u otros valores.

| Identidad | Fórmula |
| --------- | --------- |
| Suma vía XOR y AND | $A + B = (A \oplus B) + 2 \times (A \ \& \ B)$ |
| Suma vía OR y AND | $A + B = (A \| B) + (A \ \& \ B)$ |

**Paridad de bits encendidos en XOR:**

Sea $\text{popcount}(n)$ la cantidad de bits encendidos en $n$. La paridad de $\text{popcount}(A \oplus B)$ es idéntica a la paridad de $\text{popcount}(A) + \text{popcount}(B)$.

- Si $\text{popcount}(A) + \text{popcount}(B)$ es **par** $\rightarrow$ $\text{popcount}(A \oplus B)$ es **par**.
- Si $\text{popcount}(A) + \text{popcount}(B)$ es **impar** $\rightarrow$ $\text{popcount}(A \oplus B)$ es **impar**.

Esto se debe a que el XOR no cambia la paridad total de bits encendidos entre ambos operandos.

**Complejidad:** tiempo $O(1)$, memoria $O(1)$.

---

## Resumen de Operaciones

| Operación | Fórmula | Descripción |
| --------- | --------- | --------- |
| Activar bit $j$ | `S \|= (1 << j)` | Pone a 1 el bit $j$-ésimo |
| Comprobar bit $j$ | `T = S & (1 << j)` | Verifica si el bit $j$-ésimo es 1 |
| Desactivar bit $j$ | `S &= ~(1 << j)` | Pone a 0 el bit $j$-ésimo |
| Conmutar bit $j$ | `S ^= (1 << j)` | Invierte el bit $j$-ésimo |
| Bit menos significativo | `T = (S & (-S))` | Obtiene el valor del primer bit activo |
| Activar $n$ bits | `S = (1 << n) - 1` | Activa los primeros $n$ bits |
| Contar bits activos | `__builtin_popcount(S)` | Cuenta cuántos bits están en 1 |
| Multiplicar por 2 | `S << 1` | Desplaza bits a la izquierda |
| Dividir entre 2 | `S >> 1` | Desplaza bits a la derecha |
| Módulo $2^k$ | `x & ((1 << k) - 1)` | Extrae los $k$ bits menos significativos |
| Par o impar | `x & 1` | LSB: $0$ par, $1$ impar |
| Potencia de 2 | `(x != 0) && !(x & (x - 1))` | Verifica si solo hay un bit encendido |
| Swap sin temporal | `x ^= y; y ^= x; x ^= y;` | Intercambia usando XOR |
| Alternar $A \leftrightarrow B$ | `x = A ^ B ^ x` | Conmuta entre dos valores fijos |
| Suma vía XOR y AND | $(A \oplus B) + 2(A \ \& \ B)$ | Calcula $A + B$ sin operador `+` |

---

## Aplicación paso a paso

1. **Problema:** Dado un conjunto de $n$ elementos ($n \leq 20$), encontrar todas las posibles combinaciones (subconjuntos) cuya suma de elementos sea igual a un valor $K$.
2. **Observaciones:** cada subconjunto puede representarse como un entero de $n$ bits donde el bit $j$ indica si el elemento $j$ pertenece o no al subconjunto. Con $n \leq 20$, hay $2^{20} \approx 10^6$ subconjuntos posibles, lo cual es manejable.
3. **Restricciones:** $n \leq 20$ obliga a una solución que itere sobre todos los subconjuntos; un enfoque con arreglos booleanos sería demasiado lento.
4. **Idea:** iterar desde $mask = 0$ hasta $mask = 2^n - 1$, usar las operaciones de bits para verificar qué elementos pertenecen al subconjunto actual y acumular la suma.
5. **Algoritmo:**
   - Iterar $mask$ de $0$ a $(1 \ll n) - 1$
   - Para cada $mask$, iterar $j$ de $0$ a $n-1$
   - Si el bit $j$ está activo (`mask & (1 << j)`), sumar el elemento $j$-ésimo
   - Si la suma es igual a $K$, guardar o imprimir el subconjunto
6. **Complejidad:** tiempo $O(n \cdot 2^n)$, memoria $O(n)$.
7. **Implementación:**

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main() {
    int n = 4, K = 6;
    vector<int> elems = {1, 2, 3, 5};

    for (int mask = 0; mask < (1 << n); mask++) {     // O(2^n) - Iterar todos los subconjuntos
        int suma = 0;                                  // O(1) - Reiniciar suma
        vector<int> subconjunto;                       // O(1) - Subconjunto actual

        for (int j = 0; j < n; j++) {                  // O(n) - Revisar cada elemento
            if (mask & (1 << j)) {                     // O(1) - Comprobar si el bit j está activo
                suma += elems[j];                      // O(1) - Agregar elemento al subtotal
                subconjunto.push_back(elems[j]);       // O(1) - Guardar elemento
            }
        }

        if (suma == K) {                               // O(1) - Verificar si la suma es K
            cout << "{";
            for (int i = 0; i < (int)subconjunto.size(); i++) {
                cout << subconjunto[i];
                if (i + 1 < (int)subconjunto.size()) cout << ", ";
            }
            cout << "}" << "\n";
        }
    }
    return 0;
}
```

**Complejidad:** tiempo $O(n \cdot 2^n)$, memoria $O(n)$.

---

## Errores comunes

- **No usar paréntesis en operaciones de bits:** `S & 1 << j` no es lo mismo que `S & (1 << j)` debido a la precedencia de operadores. Siempre usar paréntesis.
- **Confundir `&` con `&&` y `|` con `||`:** los operadores lógicos (`&&`, `||`) no son equivalentes a los operadores de bits (`&`, `|`). `&&` y `||` evalúan expresiones booleanas, mientras que `&` y `|` manipulan bits individuales.
- **Desbordamiento con `1 << n`:** cuando $n \geq 31$, `1 << n` causa desbordamiento en un `int`. Usar `1LL << n` para `long long` o asegurar que $n < 31$.
- **Usar enteros con signo para máscaras grandes:** el bit de signo (bit 31 en `int`, bit 63 en `long long`) puede causar comportamiento inesperado. Para máscaras de 31+ bits, usar `unsigned int` o `unsigned long long`.
- **Olvidar que el índice inicia en 0:** el bit 0 es el menos significativo (el de la derecha), no el más significativo.

---

## Problemas de práctica

Ahora que entiendes los operadores de bits y cómo las máscaras de bits (_bitmasks_) pueden ayudarte a resolver problemas de manera elegante y eficiente, es hora de programarlo.

- [Maximizing XOR](https://vjudge.net/problem/HackerRank-maximizing-xor)

### Problemas recomendados

- [**Crianza de Bacterias**](https://codeforces.com/problemset/problem/579/A)
- [**Cadenas de Bits**](https://cses.fi/problemset/task/1617/)
- [**Mochila (Knapsack)**](./editoriales/mochila-knapsack.md)
- [Inspirado en Tailandia](./editoriales/inspirado-en-tailandia.md)

### Editorial de Problemas

- [Editorial de Problemas](editoriales/editorial.md)

## Recursos adicionales

- [Visualgo - Bitmask](https://visualgo.net/bitmask): visualización interactiva de operaciones de máscaras de bits.
- [GeeksforGeeks - Bitwise Operators in C/C++](https://www.geeksforgeeks.org/bitwise-operators-in-cc/): referencia completa de operadores bit a bit con ejemplos.
- [CP-Algorithms - Bitmask DP](https://cp-algorithms.com/): técnicas avanzadas combinando bitmask con programación dinámica.
