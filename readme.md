<div align="center">
  <img src="https://raw.githubusercontent.com/KaarLarax/CPC_UAEH_Documentation/refs/heads/main/Recursos/logoCPC.png" alt="Logo del Club de Programación Competitiva de la UAEH" width="180">

# Clases CPC · UAEH

  **Material de apoyo para aprender, practicar y competir en programación competitiva.**

  <p>
    <a href="https://cpcjudge.com/"><img src="https://img.shields.io/badge/CPC%20Judge-practica-111827?style=for-the-badge" alt="CPC Judge"></a>
    <a href="https://github.com/CPC-UAEH/Clases"><img src="https://img.shields.io/github/last-commit/CPC-UAEH/Clases?style=for-the-badge&label=actualizado" alt="Última actualización"></a>
    <a href="https://github.com/CPC-UAEH/Clases/issues"><img src="https://img.shields.io/github/issues/CPC-UAEH/Clases?style=for-the-badge&label=issues" alt="Issues abiertas"></a>
    <a href="https://github.com/CPC-UAEH/Clases/graphs/contributors"><img src="https://img.shields.io/github/contributors/CPC-UAEH/Clases?style=for-the-badge&label=contribuidores" alt="Contribuidores"></a>
  </p>

  <p>
    <a href="#contenido">Contenido</a> ·
    <a href="#cómo-aprender-con-este-material">Cómo aprender</a> ·
    <a href="#contribuir">Contribuir</a> ·
    <a href="#autoría-y-mantenimiento">Autoría</a>
  </p>
</div>

## Sobre el proyecto

Este repositorio reúne apuntes, explicaciones, análisis de complejidad y editoriales utilizados como material complementario para las sesiones del **Club de Programación Competitiva de la UAEH**.

El objetivo es construir una colección de material técnico en español que acompañe a los alumnos durante las clases, las sesiones de entrenamiento y la preparación para competencias. El contenido prioriza el razonamiento detrás de los algoritmos sobre la memorización de soluciones.

> **Recomendación:** intenta resolver cada problema antes de consultar su editorial.

## Contenido

El material se organiza por año, periodo académico, nivel y tema.

```text
.
├── 2026
│   └── julio-diciembre
│       └── intermedios
│           ├── complejidad-computacional
│           │   ├── complejidad-computacional.md
│           │   └── editoriales
│           │       ├── editorial.md
│           │       ├── gauss-suma.md
│           │       ├── again-twenty-five.md
│           │       ├── posada-liborio.md
│           │       └── abbb.md
│           └── operadores-bits
│               ├── operadores-bits.md
│               └── editoriales
│                   ├── editorial.md
│                   ├── crianza-de-bacterias.md
│                   ├── cadenas-de-bits.md
│                   ├── mochila-knapsack.md
│                   └── inspirado-en-tailandia.md
├── template
│   ├── README.md
│   └── periodo-academico
│       └── nivel
│           └── tema
│               ├── tema.md
│               └── editoriales
│                   └── problema.md
└── readme.md
```

Para crear material de un nuevo periodo, consulta la [plantilla de clases](./template/README.md).

### 2026 · Julio–Diciembre

#### Nivel intermedio

- [Complejidad computacional](./2026/julio-diciembre/intermedios/complejidad-computacional/complejidad-computacional.md)
  - [Editoriales de problemas resaltados](./2026/julio-diciembre/intermedios/complejidad-computacional/editoriales/editorial.md)
- [Operadores bit a bit](./2026/julio-diciembre/intermedios/operadores-bits/operadores-bits.md)
  - [Editoriales de problemas resaltados](./2026/julio-diciembre/intermedios/operadores-bits/editoriales/editorial.md)

## Cómo aprender con este material

Las explicaciones siguen este proceso:

```text
Problema → Observaciones → Restricciones → Idea → Algoritmo → Complejidad → Implementación
```

Las editoriales no muestran únicamente una solución. Documentan las observaciones y decisiones que permiten llegar a ella.

Para practicar los problemas, visita [CPC Judge](https://cpcjudge.com/).

## Roadmap de intermedios

- [x] Complejidad computacional
- [x] Operadores bit a bit
- [ ] Recursividad
- [ ] Divisores
- [ ] GCD y LCM
- [ ] Prefix Sum
- [ ] STL de C++
- [ ] Ordenamientos
- [ ] Divide y vencerás / Merge Sort

El roadmap puede cambiar conforme avance el material de las clases.

## Contribuir

Las contribuciones son bienvenidas. Puedes ayudar con:

- correcciones de errores o de redacción;
- ejemplos adicionales;
- nuevas explicaciones y editoriales;
- propuestas de problemas o material;
- revisión de Pull Requests existentes.

Consulta las [reglas para contribuir](./.github/CONTRIBUTING.md) y el
[Código de Conducta](./.github/CODE_OF_CONDUCT.md) antes de participar.

### Flujo recomendado

1. Revisa los [issues abiertos](https://github.com/CPC-UAEH/Clases/issues) o crea uno nuevo.
2. Haz un fork del repositorio y crea una rama descriptiva.
3. Realiza tus cambios y usa commits claros, por ejemplo:

   ```text
   docs: mejorar explicación de búsqueda binaria
   docs: agregar editorial del problema X
   fix: corregir análisis de complejidad
   ```

4. Abre un Pull Request explicando qué cambiaste y por qué.

Para cambios pequeños de documentación, también puedes abrir directamente un Pull Request desde GitHub.

## Autoría y mantenimiento

Este proyecto es mantenido de forma colaborativa por **KaarLarax**, el [Club de Programación Competitiva de la UAEH](https://github.com/cpc-uaeh) y todas las personas que han contribuido al repositorio.

- **Mantenimiento:** [KaarLarax](https://github.com/KaarLarax), en coordinación con el [Club de Programación Competitiva de la UAEH](https://github.com/cpc-uaeh)
- **Contribuciones:** todas las personas que han aportado contenido, correcciones, ideas o revisiones
- **Práctica de problemas:** [CPC Judge](https://cpcjudge.com/)

## Contribuidores

Gracias a todas las personas que ayudan a mejorar este material.

<div align="center">
  <a href="https://github.com/CPC-UAEH/Clases/graphs/contributors">
    <img src="https://contrib.rocks/image?repo=CPC-UAEH/Clases" alt="Contribuidores del repositorio">
  </a>
</div>

La imagen se actualiza automáticamente con los contribuidores registrados en GitHub. También puedes consultar la [lista completa de contribuidores](https://github.com/CPC-UAEH/Clases/graphs/contributors).

## Licencia

Este repositorio está bajo la licencia [Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)](./LICENSE). Puedes reutilizar, adaptar y compartir el material siempre que otorgues atribución y distribuyas las obras derivadas bajo la misma licencia.

## Reconocimientos

Material desarrollado como apoyo para las actividades del **Club de Programación Competitiva de la UAEH**.
