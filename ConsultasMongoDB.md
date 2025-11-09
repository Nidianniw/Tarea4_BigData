# Código de consultas y análisis de resultados
## 1️⃣ Consultas básicas
### - Inserción
```python
db.users.insertMany([
  {UserID:200001, Name:"Mario Perez", Gender:"Male", City:"Bogotá", Country:"Colombia"},
  {UserID:200002, Name:"Sofia Ortiz", Gender:"Female", City:"Medellín", Country:"Colombia"}
])
```
Se realiza una inserción de 2 documentos en un solo comando 
