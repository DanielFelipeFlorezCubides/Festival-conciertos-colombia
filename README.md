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
2. Crear una función llamada `bandasPorGenero(genero)` que devuelva todas las bandas activas de ese género :

- Función bandasPorGenero(genero)   
    ```js
        function bandasPorGenero(genero) {
        return db.bandas.find({ genero: genero, activa: true }).toArray();
        }
    ```
### **Transacciones (requiere replica set)**

1. Simular compra de un boleto:
    - Insertar nuevo boleto en `boletos_comprados` de un asistente.
    - Disminuir en 1 la capacidad del escenario correspondiente.
```js
const session = db.getMongo().startSession();
session.startTransaction();

try {
  const boletosCollection = session.getDatabase("festival_conciertos").asistentes;
  const escenariosCollection = session.getDatabase("festival_conciertos").escenarios;

  // Insertar boleto
  boletosCollection.updateOne(
    { _id: ObjectId("ASISTENTE_ID") },
    {
      $push: {
        boletos_comprados: {
          escenario: "Escenario Principal",
          dia: "2025-06-23"
        }
      }
    }
  );

  // Disminuir capacidad
  escenariosCollection.updateOne(
    { nombre: "Escenario Principal" },
    { $inc: { capacidad: -1 } }
  );

  session.commitTransaction();
  session.endSession();
  } catch (error) {
  session.abortTransaction();
  session.endSession();
  throw error;
}
```
2. Reversar la compra:
    - Eliminar el boleto insertado anteriormente.
    - Incrementar la capacidad del escenario.
```   js 
const session = db.getMongo().startSession();
session.startTransaction();

try {
  const boletosCollection = session.getDatabase("festival_conciertos").asistentes;
  const escenariosCollection = session.getDatabase("festival_conciertos").escenarios;

  // Eliminar boleto
  boletosCollection.updateOne(
    { _id: ObjectId("ASISTENTE_ID") },
    {
      $pull: {
        boletos_comprados: {
          escenario: "Escenario Principal",
          dia: "2025-06-23"
        }
      }
    }
  );

  // Aumentar capacidad
  escenariosCollection.updateOne(
    { nombre: "Escenario Principal" },
    { $inc: { capacidad: 1 } }
  );

  session.commitTransaction();
  session.endSession();
} catch (error) {
  session.abortTransaction();
  session.endSession();
  throw error;
}
```
---

### **Índices + Consultas**

1. Crear un índice en `bandas.nombre` y buscar una banda específica por nombre.
2. Crear un índice en `presentaciones.escenario` y hacer una consulta para contar presentaciones de un escenario.
3. Crear un índice compuesto en `asistentes.ciudad` y `edad`, luego consultar asistentes de Bogotá menores de 30.

 Índices y Consultas

1 Índice en bandas.nombre y búsqueda por nombre
```js
db.bandas.createIndex({ nombre: 1 });

db.bandas.find({ nombre: "Aterciopelados" });
```
2 Índice en presentaciones.escenario y contar presentaciones
```js

db.presentaciones.createIndex({ escenario: 1 });

db.presentaciones.countDocuments({ escenario: "Escenario Principal" });
```
3 Índice compuesto en asistentes.ciudad y edad, y consulta
```js
db.asistentes.createIndex({ ciudad: 1, edad: 1 });

db.asistentes.find({ ciudad: "Bogotá", edad: { $lt: 30 } });
```
---

