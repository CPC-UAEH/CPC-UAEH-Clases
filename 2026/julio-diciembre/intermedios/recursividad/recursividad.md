# Recursividad

**Autor:** Carlos Alberto Lara Hernandez - Kaarlarax

La recursividad es una técnica para resolver un problema expresándolo en términos de versiones más pequeñas del mismo problema. Una función recursiva se llama a sí misma, directa o indirectamente, y cada llamada trabaja con sus propios parámetros y variables locales.

En programación competitiva, la recursividad aparece en problemas de búsqueda exhaustiva, backtracking, divide y vencerás, árboles y grafos. Aprender a trazar las llamadas y analizar su costo ayuda a decidir cuándo es una herramienta clara y cuándo conviene usar una alternativa iterativa.

En esta clase construiremos soluciones desde sus casos base y recursivos, distinguiremos varias formas de recursión incluyendo *tail recursion* y *non-tail recursion* y analizaremos su tiempo y memoria.

---

## 1. La idea de recursión

Una función recursiva necesita dos piezas:

1. **Caso base:** resuelve directamente el caso más pequeño y detiene las llamadas.
2. **Caso recursivo:** transforma el problema actual en uno o más problemas más pequeños.

Además, cada llamada debe acercarse al caso base. Si no hay caso base, o si los parámetros no progresan hacia él, las llamadas pueden continuar hasta agotar la pila.

### Ejemplo: suma de los primeros $n$ enteros

Sea $S(n)=1+2+\dots+n$. Podemos expresar la suma así:

$$
S(n)=
\begin{cases}
0 & \text{si } n=0,\\
n+S(n-1) & \text{si } n>0.
\end{cases}
$$

```cpp
long long suma(int n) {
    if (n == 0) {              // Caso base
        return 0;
    }
    return n + suma(n - 1);    // Caso recursivo
}
```

Para `suma(4)`, primero se construyen las llamadas `suma(4)`, `suma(3)`, `suma(2)`, `suma(1)` y `suma(0)`. Al llegar al caso base, los resultados regresan y se calculan `1`, `3`, `6` y `10`.

**Complejidad:** tiempo $O(n)$, memoria $O(n)$ por las $n+1$ llamadas activas.

---

## 2. La pila de llamadas

Cada llamada a una función conserva su propio contexto: parámetros, variables locales y el punto al que debe regresar cuando termine la llamada. Estos contextos se apilan; la última llamada en entrar es la primera en terminar.

```text
suma(4) espera a suma(3)
  suma(3) espera a suma(2)
    suma(2) espera a suma(1)
      suma(1) espera a suma(0)
        suma(0) devuelve 0
      suma(1) devuelve 1
    suma(2) devuelve 3
  suma(3) devuelve 6
suma(4) devuelve 10
```

Las llamadas activas determinan la memoria adicional. Aunque la función no declare arreglos, una profundidad recursiva de $n$ normalmente necesita $O(n)$ espacio de pila.

---

## 3. Recursión lineal

En la recursión lineal, cada ejecución de la función realiza como máximo una llamada recursiva. La suma anterior es lineal: para `suma(n)` se llama a `suma(n - 1)` una vez.

### Factorial

Para $n\geq 1$, $n! = n\cdot(n-1)!$, con $0!=1$.

```cpp
long long factorial(int n) {
    if (n == 0) {
        return 1;
    }
    return n * factorial(n - 1);
}
```

**Complejidad:** tiempo $O(n)$, memoria $O(n)$. Para entradas grandes, además de la profundidad, hay que considerar que el factorial puede exceder el rango del tipo numérico.

---

## 4. Recursión de cola (*tail recursion*)

Una función tiene recursión de cola cuando la llamada recursiva es la última operación que debe realizar la función. No queda trabajo pendiente en esa activación después de que la llamada regresa.

Una forma de sumar los primeros $n$ enteros con una llamada de cola es pasar el acumulado como parámetro:

```cpp
long long sumaCola(int n, long long acumulado = 0) {
    if (n == 0) {
        return acumulado;
    }
    return sumaCola(n - 1, acumulado + n); // La llamada es la última operación
}
```

En `suma(4)`, cada llamada espera que la siguiente produzca el resultado. En `sumaCola(4)`, el trabajo pendiente se incorpora al acumulado antes de llamar de nuevo.

La forma de cola **no implica automáticamente memoria $O(1)$ en C++**. El estándar de C++ no garantiza que el compilador elimine esas llamadas (optimización de llamadas de cola). En general, se debe considerar una profundidad de $O(n)$ y usar un ciclo si se necesita controlar explícitamente el espacio.

```cpp
long long sumaIterativa(int n) {
    long long acumulado = 0;
    for (int i = 1; i <= n; ++i) {
        acumulado += i;
    }
    return acumulado;
}
```

**Complejidad de `sumaCola`:** tiempo $O(n)$, memoria de pila $O(n)$ en el caso general.
**Complejidad de `sumaIterativa`:** tiempo $O(n)$, memoria adicional $O(1)$.

---

## 5. Recursión no de cola (*non-tail recursion*): factorial

En la recursión no de cola queda trabajo pendiente después de la llamada recursiva. La versión usual del factorial es un ejemplo: cuando `factorial(n - 1)` regresa, todavía hay que multiplicar por `n`.

```cpp
long long factorial(int n) {
    if (n == 0) {
        return 1;
    }
    return n * factorial(n - 1); // La multiplicación ocurre al regresar
}
```

La diferencia con la versión de cola es dónde se conserva el trabajo: en la no de cola, las multiplicaciones quedan pendientes en las llamadas; en la de cola, el acumulado lleva el resultado parcial. Ambas pueden hacer $O(n)$ llamadas y ocupar $O(n)$ de pila en C++.

> **Importante:** *tail* y *non-tail* describen la posición de la llamada recursiva, no la cantidad de llamadas por función. Por ejemplo, una función puede hacer varias llamadas y aun así se analiza como recursión múltiple, no como lineal.

---

## 6. Recursión múltiple

Hay recursión múltiple cuando una activación genera dos o más llamadas recursivas. Fibonacci ingenuo ilustra el caso:

```cpp
long long fibonacci(int n) {
    if (n <= 1) {
        return n;
    }
    return fibonacci(n - 1) + fibonacci(n - 2);
}
```

La recurrencia de tiempo es $T(n)=T(n-1)+T(n-2)+O(1)$, que crece exponencialmente: $O(\varphi^n)$, y suele acotarse por $O(2^n)$. La profundidad máxima de la pila es $O(n)$, aunque el número total de llamadas sea exponencial.

Este ejemplo también muestra que una definición recursiva correcta no necesariamente produce un algoritmo eficiente: los mismos valores se calculan muchas veces. La programación dinámica con memoización puede reutilizar resultados.

---

## 7. Recursión directa e indirecta

- **Directa:** la función se llama a sí misma, como `factorial`.
- **Indirecta o mutua:** una función llama a otra que, eventualmente, vuelve a llamar a la primera.

```cpp
bool esPar(int n);

bool esImpar(int n) {
    if (n == 0) return false;
    return esPar(n - 1);
}

bool esPar(int n) {
    if (n == 0) return true;
    return esImpar(n - 1);
}
```

En ambos casos hay que comprobar que la secuencia de llamadas alcance un caso base. En este ejemplo, el código presupone que `n` es no negativo.

---

## 8. Backtracking: recursión para explorar decisiones

En backtracking, cada llamada representa una decisión parcial. Se prueban opciones, se continúa recursivamente y, al regresar, se deshace la elección para explorar otra alternativa.

### Ejemplo: generar cadenas binarias de longitud $n$

```cpp
#include <iostream>
#include <string>
using namespace std;

void generar(int n, string actual) {
    if ((int)actual.size() == n) {
        cout << actual << '\n';
        return;
    }

    generar(n, actual + '0');
    generar(n, actual + '1');
}

int main() {
    int n = 3;
    generar(n, "");
}
```

Para $n=3$, se generan las $2^3=8$ cadenas posibles. El árbol de llamadas tiene dos ramas por nivel y profundidad $n$.

**Complejidad:** se producen $2^n$ cadenas de longitud $n$, así que el tiempo de salida es $O(n\cdot2^n)$. La profundidad recursiva es $O(n)$; almacenar la cadena actual requiere $O(n)$, sin contar la salida.

Si se modifica una estructura compartida (`string`, vector o tablero), hay que deshacer el cambio antes de explorar la siguiente opción. Pasar una copia, como en el ejemplo, evita el deshacer explícito, pero puede generar copias y trabajo adicional.

---

## 9. Divide y vencerás

En divide y vencerás, el problema se divide en subproblemas, se resuelven recursivamente y se combinan sus resultados. La búsqueda binaria es un ejemplo: en cada paso conserva solo la mitad del intervalo.

```cpp
#include <vector>
using namespace std;

int busquedaBinaria(const vector<int>& a, int objetivo, int izquierda, int derecha) {
    if (izquierda > derecha) {
        return -1;
    }

    int medio = izquierda + (derecha - izquierda) / 2;
    if (a[medio] == objetivo) {
        return medio;
    }
    if (a[medio] < objetivo) {
        return busquedaBinaria(a, objetivo, medio + 1, derecha);
    }
    return busquedaBinaria(a, objetivo, izquierda, medio - 1);
}
```

Cada llamada reduce a la mitad el intervalo, así que hay $O(\log n)$ niveles. La búsqueda binaria recursiva usa $O(\log n)$ de pila; su versión iterativa usa $O(1)$ memoria adicional.

---

## 10. Cómo diseñar una solución recursiva

1. **Define qué significa la función.** Escribe en una frase qué resultado devuelve o qué tarea realiza `f(estado)`.
2. **Busca los casos base.** Incluye todos los estados pequeños que se resuelven sin volver a llamar.
3. **Define las opciones recursivas.** Expresa la respuesta del estado actual mediante estados más pequeños.
4. **Verifica el progreso.** Cada llamada debe acercarse a algún caso base.
5. **Traza una entrada pequeña.** Dibuja llamadas y retornos para comprobar el orden y el resultado.
6. **Cuenta llamadas y profundidad.** El número total de llamadas determina el tiempo; la máxima cantidad simultánea determina la pila.
7. **Evalúa una alternativa.** Si hay muchas llamadas repetidas, considera memoización; si solo queda una cadena lineal de llamadas, considera un ciclo.

---

## 11. Errores comunes

- **Olvidar un caso base:** puede causar llamadas hasta el desbordamiento de pila.
- **No avanzar hacia el caso base:** por ejemplo, llamar `f(n)` de nuevo sin cambiar `n`.
- **Confundir llamadas con profundidad:** Fibonacci ingenuo tiene tiempo exponencial, pero profundidad lineal.
- **Suponer que toda recursión de cola usa $O(1)$ memoria en C++:** esa optimización no está garantizada.
- **Dejar trabajo pendiente sin reconocerlo:** `n * f(n - 1)` es no de cola porque la multiplicación ocurre al regresar.
- **Ignorar cálculos repetidos:** en recursión múltiple, subproblemas iguales pueden generar mucho trabajo redundante.
- **Modificar el estado y no restaurarlo en backtracking:** las decisiones de una rama pueden contaminar las siguientes.
- **Usar recursión con profundidad excesiva:** puede exceder el límite de pila aun cuando el tiempo sea aceptable.
- **No revisar el rango del resultado:** que la profundidad sea segura no impide que una suma o producto desborde el tipo numérico.

---

## Aplicación paso a paso: mover discos en las Torres de Hanói

1. **Problema:** mover $n$ discos de una varilla origen a una varilla destino, usando una varilla auxiliar.
2. **Observaciones:** solo se mueve un disco a la vez; nunca se coloca un disco grande encima de uno pequeño.
3. **Restricciones:** para mover el disco más grande, primero deben moverse los $n-1$ discos superiores a la varilla auxiliar.
4. **Idea:** mover recursivamente $n-1$ discos al auxiliar, mover el disco grande al destino y mover los $n-1$ discos del auxiliar al destino.
5. **Caso base:** con cero discos no hay nada que mover.
6. **Algoritmo:** efectuar las dos llamadas recursivas y el movimiento del disco más grande entre ellas.
7. **Complejidad:** $T(n)=2T(n-1)+O(1)$, por lo que el tiempo es $O(2^n)$; profundidad y pila $O(n)$.

```cpp
#include <iostream>
using namespace std;

void hanoi(int n, char origen, char auxiliar, char destino) {
    if (n == 0) {
        return;
    }

    hanoi(n - 1, origen, destino, auxiliar);
    cout << "Mover disco " << n << " de " << origen << " a " << destino << '\n';
    hanoi(n - 1, auxiliar, origen, destino);
}

int main() {
    int n;
    cin >> n;
    hanoi(n, 'A', 'B', 'C');
}
```

El número de movimientos satisface $M(n)=2M(n-1)+1$, con $M(0)=0$. De aquí resulta $M(n)=2^n-1$: la solución es clara, pero el volumen de salida crece exponencialmente.

## Problemas de práctica

- [Tower of Hanoi - CSES](https://cses.fi/problemset/task/2165/)

### Problemas recomendados

- [**Creating Strings - CSES**](https://cses.fi/problemset/task/1622/)
- [**Apple Division - CSES**](https://cses.fi/problemset/task/1623/)
- [Grid Paths - CSES](https://cses.fi/problemset/task/1625/)

### Editorial de problemas

- [Editoriales de Recursividad](editoriales/editorial.md)

## Recursos adicionales

- [Programiz - C++ Recursion](https://www.programiz.com/cpp-programming/recursion): definición, ejemplo factorial y ventajas y desventajas.
- [GeeksforGeeks - Introduction to Recursion](https://www.geeksforgeeks.org/introduction-to-recursion-data-structure-and-algorithm-tutorials/): casos base, flujo de ejecución, pila y aplicaciones.
- [5 Simple Steps for Solving Any Recursive Problem (YouTube)](https://www.youtube.com/watch?v=ngCos392W4w): recurso en video para estructurar el razonamiento recursivo.
- [Recursion in C - lista de reproducción (YouTube)](https://www.youtube.com/playlist?list=PLBlnK6fEyqRjTO_UNGKuaaoxEqvSF0t5h): serie de videos de introducción a la recursión.
- [Towers of Hanoi: A Complete Recursive Visualization (YouTube)](https://www.youtube.com/watch?v=rf6uf3jNjbo): visualización del árbol de llamadas de Torres de Hanói.

Los videos se enlazan como material de apoyo; esta clase es una explicación original y no una transcripción literal. Las transcripciones de YouTube no estuvieron disponibles durante la preparación de este documento.
