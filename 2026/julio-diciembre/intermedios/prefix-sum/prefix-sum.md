# Prefix-Sum

**Autor:** Nicole Abigail Chow-Flores - Nikkacf24

## 1. El problema

Imaginemos que tenemos un vector de $n$ números, y nos van a hacer $q$ consultas (queries) del tipo:

¿Cuál es la suma de los elementos entre la posición $l$ y la posición $r$?

Ejemplo:

```cpp
vector<long long> a = {0, 2, 4, 1, 6, 3, 8, 5};
// Ejemplo de una query: suma entre l=3 y r=6
```

Un detalle importante a recordar para despues es que: **el vector no cambia**. Lo unico que cambia entre cada query, son los valores de `l` y `r`.

---

### El enfoque ingenuo: recorrer cada vez

Lo primero que se nos ocurre a todos es, para cada consulta, recorrer el vector desde `l` hasta `r` y sumar uno por uno.

```cpp
vector<long long> a = {0, 2, 4, 1, 6, 3, 8, 5};     //arreglo original indexado en 1
int q = 5;                                          //# de queries

while(q--) {
    int l, r;                                       //índices
    cin >> l >> r;
    long long suma = 0;                             //variable acumuladora - long long para que no se desborde
    for(int i = l; i <= r; i++){
        suma += a[i];                               //acumular cada elemento del arreglo a dentro del rango
    }
    cout << suma << "\n";
}
```

**¿Por qué es lento?** En el peor caso, cada query cuesta $O(n)$, porque podrían pedirnos la suma de todo el vector y, por lo tanto, tendríamos que recorrer todos sus elementos.

Si tenemos $q$ queries el costo total sería $O(n * q)$ ya que por cada query, debemos recorrer todo el arreglo.

Con $n = 10^5$ y $q = 10^5$, resulta en $10^{10}$ operaciones, lo que provocaría un **TLE** (Time Limit Exceeded).

La pista para optimizar está en el propio problema: si el vector no cambia, y estamos recalculando lo mismo una y otra vez, **la solución es calcularlo una sola vez y reutilizarlo**.

---

## 2. Antes de construir la solución: cómo vamos a indexar?

Vamos a resolver el problema usando `vector<long long>` para evitar desbordamiento y vamos a indexar **desde 1 hasta $n$** en vez de desde 0 (ppara entender mejor la fórmula y evitar errores de acceso a índices inválidos).

- El vector original `a` se indexa desde `a[1]` hasta `a[n]`. La posición `a[0]` existe pero no la usamos.
- Vamos a construir un segundo vector, `prefix`, de tamaño `n + 1` (por lo mismo de indexar desde 1), donde `prefix[0]` va a representar "la suma de cero elementos", es decir, 0.

```cpp
int n = 7;                                         //numero de elementos
vector<long long> a = {0, 2, 4, 1, 6, 3, 8, 5};    // a[0] no se usa; el vector real empieza en a[1]
vector<long long> prefix(n + 1, 0);                // tamaño n+1, todo inicializado en 0
```

---

## 3. Construyendo `prefix`

Vamos a construir un vector `prefix` donde cada posición `i` va a guardar **la suma de todos los elementos desde el inicio hasta la posición `i`**:

$$prefix[i] = a[1] + a[2] + . . . + a[i]$$

Para lograr esto, la idea es seguir una regla:

```text
El valor actual es igual a la suma que llevamos acumulada + el elemento actual.
```

La fórmula es:

$$prefix[i] = prefix[i - 1] + a[i]$$

Esto se puede implementar de la siguiente manera:

```cpp
for (int i = 1; i <= n; i++) {              // se recorre todo el vector
    prefix[i] = prefix[i - 1] + a[i];       //prefix[i] = el total anterior + el elemento actual
}
```

El ciclo recorre el vector una sola vez, por lo que construir prefix tiene una complejidad de $O(n)$.

La operación que realizamos dentro del ciclo es una suma entre dos valores, por lo que cuesta $O(1)$.

### Paso a Paso

Tenemos:

`vector<long long> a = {0, 2, 4, 1, 6, 3, 8, 5};`

Inicialmente:
$prefix[0] = 0$

- Índice 1: Sumamos lo acumulado anteriormente (0) + el nuevo elemento (2).
  - prefix[1] = prefix[1 - 1] + a[1] = prefix[0] + a[1] = 0 + 2 = 2
  - prefix[1] = 2
- Índice 2: Sumamos lo acumulado anteriormente (2) + el nuevo elemento (4).
  - prefix[2] = prefix[2 - 1] + a[2] = prefix[1] + a[2] = 2 + 4 = 6
  - prefix[2] = 6
- Índice 3: Sumamos lo acumulado anteriormente (6) + el nuevo elemento (1).
  - prefix[3] = prefix[3 - 1] + a[3] = prefix[2] + a[3] = 6 + 1 = 7
  - prefix[3] = 7
- Índice 4: Sumamos lo acumulado anteriormente (7) + el nuevo elemento (6).
  - prefix[4] = prefix[4 - 1] + a[4] = prefix[3] + a[4] = 7 + 6 = 13
  - prefix[4] = 13
- Índice 5: Sumamos lo acumulado anteriormente (13) + el nuevo elemento (3).
  - prefix[5] = prefix[5 - 1] + a[5] = prefix[4] + a[5] = 13 + 3 = 16
  - prefix[5] = 16
- Índice 6: Sumamos lo acumulado anteriormente (16) + el nuevo elemento (8).
  - prefix[6] = prefix[6 - 1] + a[6] = prefix[5] + a[6] = 16 + 8 = 24
  - prefix[6] = 24
- Índice 7: Sumamos lo acumulado anteriormente (24) + el nuevo elemento (5).
  - prefix[7] = prefix[7 - 1] + a[7] = prefix[6] + a[7] = 24 + 5 = 29
  - prefix[7] = 29

Con nuestro ejemplo, `prefix` queda así:

| i | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| a\[i\] | – | 2 | 4 | 1 | 6 | 3 | 8 | 5 |
| prefix\[i\] | 0 | 2 | 6 | 7 | 13 | 16 | 24 | 29 |

Podemos verificar que en cada índice $i$ del arreglo `prefix`, el valor representa la suma de los elementos desde $a[1]$ hasta $a[i]$.

Por ejemplo: `prefix[4] = 13` significa que:

$$prefix[4] = a[1] + a[2] + a[3] + a[4] = 2 + 4 + 1 + 6 = 13$$

Aquí es donde es importante la indexación en 1 en vez de 0. Al aplicar la fórmula prefix[i] = prefix[i - 1] + a[i], si i = 1, entonces i - 1 = 0, que es un índice válido en nuestro vector.

Si estuviera indexado en 0 y i = 0, entonces i - 1 = -1, que no es un índice válido en nuestro vector y nos daría un error.

> Nota: Si se puede indexar en 0, pero debes adaptar tu código a ello, por temas de facilidad, explicamos con indexación en 1.

---

## 4. ¿Qué estamos logrando con Prefix Sum?

Hasta este punto, todavía no hemos respondido ninguna consulta.

Lo que hicimos fue preprocesar el vector.

En lugar de guardar solamente los valores originales, construimos otro vector que contiene información acumulada. Ahora tenemos información que podemos reutilizar y adaptarla a distintos problemas de programación competitiva. Uno muy común es el de consultas entre rangos.

---

## 5. Consultas

Ahora que ya tenemos construido nuestro vector prefix, podemos usarlo para responder las consultas sin tener que recorrer nuevamente todos los elementos entre l y r.

Supongamos que queremos responder la siguiente query:

```text
¿Cuál es la suma de los elementos desde l = 3 hasta r = 6?
```

Es decir, queremos calcular:
$$a[3] + a[4] + a[5] + a[6]$$

Suponiendo que:

```text
a = {0, 2, 4, 1, 6, 3, 8, 5}
```

Esto es igual a 1 + 6 + 3 + 8 = 18. Podemos obtener este resultado utilizando los valores que ya calculamos en prefix.

> Recordemos que `prefix[i]` contiene la suma de todos los elementos desde `a[1]` hasta `a[i]`.

`prefix = {0, 2, 6, 7, 13, 16, 24, 29}`

Sabemos que:
$$prefix[6] = a[1] + a[2] + a[3] + a[4] + a[5] + a[6] $$

Y que:
$$prefix[2] = a[1] + a[2]$$

Si restamos `prefix[2]` de `prefix[6]`, eliminamos los elementos que no pertenecen al rango que queremos:

$$prefix[6] - prefix[2] = 24 - 6 = 18$$

Así es como obtenemos exactamente la suma que necesitamos.

### La Fórmula General

Para cualquier consulta entre l y r, podemos utilizar:

$$prefix[r] - prefix[l-1];$$

¿Por qué usamos $l$ - 1?

Porque `prefix[r]` contiene todos los elementos desde `a[1]` hasta `a[r]`, pero nosotros solamente queremos los elementos desde `a[l]` hasta `a[r]`.

Al restar `prefix[l-1]`, eliminamos todos los elementos anteriores a `l`.

---

## 6. Código en C++

```cpp
#include <bits/stdc++.h>
using namespace std;


int main() {
    int n, q; cin >> n >> q;
    vector<long long> a(n + 1);
    for (int i = 1; i <= n; i++) cin >> a[i];        // O(n) - leer la entrada

    vector<long long> prefix(n + 1, 0);
    for (int i = 1; i <= n; i++) {                    // O(n) - construir el vector de prefijos, una sola vez
        prefix[i] = prefix[i - 1] + a[i];
    }

    // Responder consultas
    while (q--) {                                     // O(q) - cada consulta cuesta O(1)
        int l, r; cin >> l >> r;
        cout << prefix[r] - prefix[l - 1] << "\n";
    }

    return 0;
}
```

---

## 7. Complejidad

| Enfoque | Preprocesamiento | Por consulta | Total con $q$ consultas |
| --- | --- | --- | --- |
| Ingenuo | $O(1)$ | $O(n)$ | $O(n * q)$ |
| Prefix Sum | $O(n)$ | $O(1)$ | $O(n + q)$ |

Con $n = 10^5$ y $q = 10^5$, pasamos de $10^{10}$ operaciones a apenas $2 * 10^5$.

---

## 8. Errores Comunes de Prefix Sum en Programación Competitiva

### 1. No considerar el tipo de dato

Si los elementos del vector pueden ser grandes, la suma de varios elementos puede superar el límite de un `int`.

Por eso utilizamos:

```cpp
long long
```

Esto es importante porque aunque cada elemento individual pueda caber dentro de un `int`, **la suma de muchos elementos puede no caber**.

---

### 2. Confundir la indexación en 0 con la indexación en 1

Uno de los errores más comunes es mezclar ambas formas de indexación.

Si estamos utilizando esta forma de indexación basada en 1, la fórmula correcta es:

$$prefix[i] = prefix[i-1] + a[i]$$

Y para una consulta entre `l` y `r`:

$$prefix[r] - prefix[l-1]$$

Si mezclamos estas fórmulas con una implementación indexada desde 0, podemos terminar accediendo a índices incorrectos.

---

### 3. Olvidar inicializar `prefix[0]`

Cuando utilizamos indexación en 1, necesitamos que:

$$prefix[0] = 0$$

Esto representa la suma de cero elementos.

Si `prefix[0]` no está correctamente inicializado, el primer cálculo puede utilizar un valor incorrecto.

Por eso podemos inicializar el vector de esta manera:

```cpp
vector<long long> prefix(n + 1, 0);
```

Así todas sus posiciones del vector quedan inicializadas en `0`.

---

### 4. Usar `prefix[l]` en lugar de `prefix[l-1]`

Otro error común aparece al responder una consulta.

La fórmula correcta es:

$$prefix[r] - prefix[l-1]$$

No:

$$prefix[r] - prefix[l]$$

Por ejemplo, si queremos calcular la suma entre `l = 3` y `r = 6`:

$$prefix[6] - prefix[2]$$

Esto nos da:

$$24 - 6 = 18$$

Si utilizáramos `prefix[3]`:

$$prefix[6] - prefix[3] = 24 - 7 = 17$$

Estaríamos eliminando también `a[3]`, aunque `a[3]` sí pertenece al rango que queremos sumar.

La razón de utilizar `l - 1` es que queremos eliminar solamente los elementos **anteriores a `l`**.

---

### 5. Confundir el arreglo original con `prefix`

Es importante recordar que `a` y `prefix` almacenan información diferente.

Por ejemplo:

| $i$         |  1 |  2 |  3 |  4 |  5 |  6 |  7 |
| ----------- | -: | -: | -: | -: | -: | -: | -: |
| `a[i]`      |  2 |  4 |  1 |  6 |  3 |  8 |  5 |
| `prefix[i]` |  2 |  6 |  7 | 13 | 16 | 24 | 29 |

`a[i]` contiene **el valor original** en esa posición.

`prefix[i]` contiene **la suma acumulada hasta esa posición**.

Por ejemplo:

$$a[4] = 6$$

pero:

$$prefix[4] = 13$$

No representan lo mismo.

---

### 6. Equivocarse con los límites del ciclo

Siempre hay que mantener consistencia entre:

- la forma en que almacenamos los datos;
- la forma en que construimos `prefix`;
- la forma en que respondemos las consultas.

---

La mayoría de estos errores no están relacionados con la idea de Prefix Sum en sí, sino con **indexación, límites y tipos de datos**, que son aspectos especialmente importantes en programación competitiva.

---

## Problemas de Práctica

- [C11O25. La suma otoñal](https://cpcjudge.com/problem/lasumaotonial)

---

### Problemas recomendados

- [**Static Range Sum Queries**](https://cses.fi/problemset/task/1646/)

---

### Editorial de Problemas

- [Editorial de Problemas](editoriales/editorial.md)

---

## Recursos Adicionales

En esta sección encontraras algunos recursos externos para poder entender más y mejorar tu comprensión acerca del tema.

Links y recursos:

- [Prefix Sum — A Complete Guide](https://codeforces.com/blog/entry/146389)
- [Introduction to Prefix Sums](https://usaco.guide/silver/prefix-sums?lang=cpp)
