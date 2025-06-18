# 🎤 Festival de Conciertos en Colombia

## Requerimientos

Realiza y documenta en el repositorio las siguientes tareas:

---

### **Consultas**

1. **Expresiones Regulares**
    - Buscar bandas cuyo nombre **empiece por la letra “A”**.
    - Buscar asistentes cuyo **nombre contenga "Gómez"**.
2. **Operadores de Arreglos**
    - Buscar asistentes que tengan `"Rock"` dentro de su campo `generos_favoritos`.
3. **Aggregation Framework**
    - Agrupar presentaciones por `escenario` y contar cuántas presentaciones hay por cada uno.
    - Calcular el **promedio de duración** de las presentaciones.

---

### **Funciones en system.js**

1. Crear una función llamada `escenariosPorCiudad(ciudad)` que devuelva todos los escenarios en esa ciudad.
2. Crear una función llamada `bandasPorGenero(genero)` que devuelva todas las bandas activas de ese género.

### **Transacciones (requiere replica set)**

1. Simular compra de un boleto:
    - Insertar nuevo boleto en `boletos_comprados` de un asistente.
    - Disminuir en 1 la capacidad del escenario correspondiente.
2. Reversar la compra:
    - Eliminar el boleto insertado anteriormente.
    - Incrementar la capacidad del escenario.

---

### **Índices + Consultas**

1. Crear un índice en `bandas.nombre` y buscar una banda específica por nombre.
2. Crear un índice en `presentaciones.escenario` y hacer una consulta para contar presentaciones de un escenario.
3. Crear un índice compuesto en `asistentes.ciudad` y `edad`, luego consultar asistentes de Bogotá menores de 30.