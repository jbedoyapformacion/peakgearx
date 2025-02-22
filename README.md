# peakgearx
Este proyecto es un sistema web desarrollado con Python y FastAPI que permite a los excursionistas determinar los elementos óptimos que pueden llevar para escalar un risco. 

# PeakGearX - Sistema de Optimización de Equipamiento para Excursionistas

## Descripción del Proyecto
Este proyecto es un sistema web desarrollado con **Python y FastAPI** que permite a los excursionistas determinar los elementos óptimos que pueden llevar para escalar un risco. La optimización se basa en dos factores:

1. **Calorías mínimas requeridas** para la escalada.
2. **Peso máximo permitido** en la mochila.

El sistema usa un **algoritmo de optimización (problema de la mochila)** para seleccionar los elementos que cumplan con el mínimo de calorías y que tengan el menor peso posible.

---

## 📌 Requerimientos

Los excursionistas necesitan un software que les ayude a elegir los elementos óptimos para su viaje.

Ejemplo de entrada y salida:

**Entrada:**
- Mínimo de calorías: `15`
- Peso máximo: `10`
- Lista de elementos:
  - E1: Peso `5`, Calorías `3`
  - E2: Peso `3`, Calorías `5`
  - E3: Peso `5`, Calorías `2`
  - E4: Peso `1`, Calorías `8`
  - E5: Peso `2`, Calorías `3`

**Salida:**
Los elementos óptimos son: `E1, E2, E4`, ya que su peso total es `9` y aportan `16` calorías.

---

## 🚀 Tecnologías Utilizadas
- **Lenguaje:** Python 3.x
- **Framework:** FastAPI
- **Base de Datos:** SQLite (opcional para persistencia)
- **Servidor Web:** Uvicorn
- **Despliegue en Azure:** Azure App Service / Azure VM

---

## 🏗️ Instalación y Configuración
### 1️⃣ Clonar el repositorio
```bash
git clone https://github.com/usuario/PeakGearX.git
cd PeakGearX
```

### 2️⃣ Crear un entorno virtual e instalar dependencias
```bash
python -m venv venv
source venv/bin/activate  # Linux/macOS
venv\Scripts\activate  # Windows
pip install -r requirements.txt
```

### 3️⃣ Ejecutar el servidor FastAPI
```bash
uvicorn main:app --reload
```
Accede a la API en `http://127.0.0.1:8000` y la documentación en `http://127.0.0.1:8000/docs`

---

## 🛠️ Desarrollo de la API con FastAPI
### 📌 Definición del Modelo de Datos
```python
from pydantic import BaseModel
from typing import List

class Elemento(BaseModel):
    nombre: str
    peso: float
    calorias: float

class Requerimiento(BaseModel):
    calorias_minimas: float
    peso_maximo: float
    elementos: List[Elemento]
```

### 📌 Lógica para Seleccionar los Elementos Óptimos (Problema de la Mochila)
```python
def mochila_optima(requerimiento: Requerimiento):
    elementos = sorted(requerimiento.elementos, key=lambda x: x.calorias/x.peso, reverse=True)
    peso_total, calorias_total, seleccionados = 0, 0, []
    
    for elemento in elementos:
        if peso_total + elemento.peso <= requerimiento.peso_maximo:
            seleccionados.append(elemento.nombre)
            peso_total += elemento.peso
            calorias_total += elemento.calorias
            if calorias_total >= requerimiento.calorias_minimas:
                break
    return {"elementos_seleccionados": seleccionados, "peso_total": peso_total, "calorias_total": calorias_total}
```

### 📌 Creación de Rutas con FastAPI
```python
from fastapi import FastAPI
from models import Requerimiento
from logic import mochila_optima

app = FastAPI()

@app.post("/calcular")
def calcular_optimo(requerimiento: Requerimiento):
    return mochila_optima(requerimiento)
```

---

## 🌍 Despliegue en Azure App Service

### 1️⃣ Autenticarse en Azure e instalar Azure CLI
```bash
az login
```

### 2️⃣ Crear un grupo de recursos y servicio en Azure
```bash
az group create --name PeakGearXGroup --location eastus
az webapp create --resource-group PeakGearXGroup --plan PeakGearXPlan --name PeakGearXApp --runtime "PYTHON:3.9"
```

### 3️⃣ Desplegar la aplicación en Azure
```bash
git push azure main
```

---

## 📄 Documentación y Pruebas
FastAPI genera automáticamente documentación interactiva en:
- **Swagger UI**: [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)
- **Redoc**: [http://127.0.0.1:8000/redoc](http://127.0.0.1:8000/redoc)

### 📌 Prueba con `curl`
```bash
curl -X 'POST' \
  'http://127.0.0.1:8000/calcular' \
  -H 'Content-Type: application/json' \
  -d '{"calorias_minimas": 15, "peso_maximo": 10, "elementos": [{"nombre": "E1", "peso": 5, "calorias": 3}, {"nombre": "E2", "peso": 3, "calorias": 5}, {"nombre": "E3", "peso": 5, "calorias": 2}, {"nombre": "E4", "peso": 1, "calorias": 8}, {"nombre": "E5", "peso": 2, "calorias": 3}]}'
```

---

## 📢 Contribuciones
Si deseas contribuir, por favor abre un **pull request** con una breve descripción de tu mejora.

---

## 📜 Licencia
Este proyecto está bajo la licencia MIT. ¡Úsalo libremente!

---

## 📩 Contacto
- 📧 Email: contacto@peakgearx.com
- 🌐 Website: [https://peakgearx.com](https://peakgearx.com)

