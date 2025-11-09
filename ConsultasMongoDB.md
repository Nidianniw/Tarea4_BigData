# Código de consultas para el Dataset de usuarios de redes sociales
## 1️⃣ Consultas básicas
### - Inserción
Se realiza una inserción de 2 documentos en un solo comando 
```python
db.Usuario.insertMany([
  {UserID:200001, Name:"Mario Perez", Gender:"Male", City:"Bogotá", Country:"Colombia"},
  {UserID:200002, Name:"Sofia Ortiz", Gender:"Female", City:"Medellín", Country:"Colombia"}
])
```
### - Selección
Mediante el comando “find” se selecciona a mujeres que viven en el país de Poland
```python
db.Usuarios.find(
  { Country: "Poland", Gender: "Female" },
  { Name: 1, _id: 1 }
```
### - Actualización
Se realiza una actualización “updateMany” donde se buscan todos los usuarios cuyo campo Country sea "Indonesia" y Cambia el campo City a "Jakarta" 
```python
db.Usuarios.updateMany(
  { Country:"Indonesia" },      
  { $set:{ City:"Jakarta" } }    
)

```
### - Eliminación de documentos
Mediante el comando “deleteMany” se eliminan todos los documentos que tengan como Country a Chile.
```python
db.Usuarios.deleteMany({ Country:"Chile" })
```
## 2️⃣Consultas con filtros y operadores
### - Consultas con filtros
Usuarios cuyo nombre comienza por “A”
```python
db.Usuarios.find({ Name: /^A/ })
```
Buscar usuarios en Colombia
```python
db.Usuarios.find({Country: "Colombia"})
```
### - Consultas con operadores
Buscar mujeres nacidas después del 2000
```python
db.Usuarios.find({ Gender: "Female", DOB: { $gt: new Date ("2000") } })
```
Usuarios que NO son de “Indonesia”
```python
db.Usuarios.find({ Country: { $ne:"Indonesia" } })
```
Usuarios en Colombia o Poloniaa”
```python
db.Usuarios.find({ $or: [ { Country: "Colombia" }, { Country: "Poland" } ] })

```
## 3️⃣ Consultas de agregación para calcular estadísticas 
### - Contar
Contar usuarios por país
```python
db.Usuarios.aggregate([
  { $group: { _id: "$Country", total: { $sum: 1 } } }
])
```
Contar mujeres por ciudad
```python
db.users.aggregate([
  { $match: { Gender: "Female" } },
  { $group: { _id: "$City", totalMujeres: { $sum: 1 } } }
])
```
### - Sumar
Ciudad con mayor número de intereses asociados (suma general)
```python
db.Usuarios.aggregate([
  { $unwind:"$Interests" },
  { $group:{ _id:"$City", totalIntereses:{ $sum:1 } } },
  { $sort:{ totalIntereses:-1 } },
  { $limit:5 }
])
```
Suma total de usuarios agrupados por país pero solo de los países que tienen más de 500 usuarios.
```python
db.Usuarios.aggregate([
  { $group: { _id:"$Country", totalUsuarios:{ $sum:1 } } },
  { $match: { totalUsuarios: { $gt:500 } } },
  { $sort: { totalUsuarios:-1 } }
])
```
### - Promediar
Promedio de edad por país
```python
db.Usuarios.aggregate([
  {$addFields: {edad: {$dateDiff: {startDate: "$DOB",endDate: "$$NOW",unit: "year"  }}}  },
  { $group: {_id: "$Country",promedioEdad: { $avg: "$edad" }}}])
```
Usuarios con edad entre 20 y 30 años
```python
db.Usuarios.aggregate([
  { $addFields:{ edad: { $subtract:[ { $year:new Date() }, { $year:"$DOB" } ] } } },
  { $match:{ edad:{ $gte:20, $lte:30 } } }
```
