# Asistente de Rutas de Cajamarca

Una herramienta para consultar rutas de transporte publico en Cajamarca, Peru.

```
backend/     API FastAPI
frontend/    Interfaz React + Vite
database/    Datos en SQLite (rutas, puntos, aliases)
```

## Que permite hacer

- Buscar rutas directas entre dos referencias.
- Ver que rutas pasan por un lugar.
- Conocer horarios, frecuencias y tarifas.
- Explorar información de lugares turisticos, comerciales y de salud.
- Resolver nombres ambiguos con sugerencias.
- Funciona sin clave de API.

## Tecnologias

- **Backend**: FastAPI + SQLite + Uvicorn
- **Frontend**: React 18 + TypeScript + Vite
- **Clasificacion**: Parser determinista + Gemini (opcional)
- **Datos**: SQLite local, solo datos de consulta

## Inicio rapido

```bash
# Backend
python -m backend.init_db --reset
python -m backend.main

# Frontend
cd frontend && npm install && npm run build
```

Abrir `http://127.0.0.1:8000`.

## Configuracion

Variables de entorno en `.env`:

```dotenv
DB_PATH=transita_cajamarca.db
GEMINI_API_KEY=tu_clave_aqui
GEMINI_MODEL=gemini-3.5-flash-lite
```

Gemini es opcional. Sin la clave, el sistema sigue funcionando con el parser determinista.

## Comandos utiles

```bash
# Inicializar base de datos
python -m backend.init_db

# Reinicializar (borra y recrea)
python -m backend.init_db --reset

# Tests (no requieren Gemini)
python -m unittest discover -s tests -v

# Build del frontend
cd frontend && npm install && npm run build
```

## Endpoints

```
GET  /                    Interfaz
GET  /api/health          Estado y conteos
POST /api/consultar       Consulta principal
GET  /api/rutas           Listado de rutas
POST /api/proxima-unidad  Proxima salida
```

## Estado actual y limites

- Solo rutas directas. Sin transbordos.
- Tiempos y distancias son del recorrido completo, no del tramo consultado.
- Proxima salida es teorica desde el inicio de la ruta.
- Puntos del itinerario son referencias viales, no paraderos certificados.
- Los datos dependen de lo cargado en la base SQLite.

## Continuar desde aqui

### Prioridad alta

- Confirmar y mejorar la calidad de los datos de rutas.
- Mantener un despliegue reproducible en Railway.
- Agregar tests al proceso de merge automatizado.
- Evaluar persistencia adecuada para SQLite o migrar a PostgreSQL si hay datos dinamicos.

### Producto

- Rutas con transbordos.
- Buscar tramos especificos en lugar del recorrido completo.
- Diferenciar paraderos confirmados de referencias viales.
- Mejorar la resolucion de lugares ambiguos.
- Ampliar cobertura de horarios y frecuencia real.

### Frontend

- Estados de carga, errores y respuestas lentas.
- Navegacion mobile y accesibilidad.
- Optimizar carga inicial.
- Mapas solo si existe informacion geografica confiable.

### IA

- Medir cuando Gemini realmente mejora el resultado.
- Reducir llamadas externas innecesarias.
- Controlar latencia y costos de API.
- Mantener el parser determinista como base verificable.
