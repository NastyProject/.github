# NastyProject 🥫

Somos un equipo de estudiantes de la **Escuela Colombiana de Ingeniería Julio Garavito** (curso TDSE) construyendo **FoodMesh**: una plataforma cloud multi-tenant para reducir el desperdicio de alimentos conectando el consumo de los hogares con el retail.

---

## 🍅 El problema

En Colombia se pierden o desperdician **9,76 millones de toneladas de alimentos al año** (DNP, 2016). Los hogares (15,6%) y la distribución/retail (20,6%) concentran más de un tercio de ese total.

Hogares y supermercados operan como sistemas aislados:

- Los **hogares** gestionan su despensa a mano, sin saber qué está por vencer ni qué hay disponible en los puntos de venta cercanos.
- Los **supermercados** no tienen visibilidad de qué pasa con sus productos una vez salen de la tienda.

## 💡 Nuestro enfoque

FoodMesh actúa como **intermediario de datos de confianza** entre ambos extremos:

1. **Captura semiautomática del inventario:** el usuario fotografía su factura de compra y el sistema extrae los productos con OCR + IA, sin registro manual.
2. **Recomendaciones con IA generativa:** recetas basadas en el inventario actual, las preferencias del usuario y, sobre todo, los productos próximos a vencer.
3. **Señales de demanda agregadas y anónimas:** los supermercados reciben tendencias de consumo por zona, categoría y semana, nunca datos de un hogar individual. Si una señal tiene menos hogares que el umbral mínimo (k-anonimato), simplemente no se publica.

> **Alcance del prototipo (Fase 0):** el flujo del hogar completo: factura → OCR → inventario validado → recomendación de recetas. El canal con supermercados se demuestra con datos simulados, y la integración IoT queda para fases futuras.

## 🧩 Arquitectura

**Monolito modular** cuyos módulos corresponden a los servicios de la arquitectura objetivo, para poder separarlos en fases posteriores:

| Módulo | Responsabilidad |
|---|---|
| `auth` | Autenticación JWT y aislamiento por hogar |
| `inventory` | Facturas, ítems de inventario, fechas de vencimiento |
| `ocr` | Preprocesamiento de imagen, OCR y normalización con LLM |
| `recommendations` | Construcción del prompt, llamada al LLM y caché |
| `aggregation` | Señales de demanda anonimizadas con umbral mínimo |

**Pipeline de captura:**

```
Foto de factura → OpenCV (perspectiva, binarización) → Tesseract (spa)
    → LLM (normalización a {producto, cantidad, precio, categoría})
    → Validación del usuario → Inventario
```

## 🛠️ Tecnologías

**Backend**
- Python + FastAPI
- PostgreSQL con Row-Level Security por hogar
- SQLAlchemy 2 + Alembic
- Redis (caché de recomendaciones)
- OpenCV + Tesseract OCR + RapidFuzz
- LLM vía API externa
- Docker / Docker Compose
- Pytest

**Frontend**
- React + TypeScript (Vite)
- TanStack Query + React Router
- Tailwind CSS
- Tipos generados desde el OpenAPI del backend

**Infraestructura**
- AWS: EC2, RDS (PostgreSQL), S3, CloudWatch
- Vercel (frontend)

## 📦 Repositorios

| Repo | Descripción |
|---|---|
| [`foodmesh-backend`](./foodmesh-backend) | API REST en FastAPI, pipeline OCR y recomendador |
| [`foodmesh-frontend`](./foodmesh-frontend) | Aplicación web en React + TypeScript |

## 🗺️ Roadmap

| Fase | Objetivo |
|---|---|
| **0 — Este semestre** | Prototipo del flujo hogar: OCR + recomendación de recetas |
| 1 | Piloto con un supermercado aliado; QR estructurado en recibos |
| 2 | Expansión multi-tenant y predicción de demanda en producción |
| 3 | Integración con sensores IoT domésticos |

## 👥 Equipo

- Jacobo Diaz Alvarado
- Julián Ramirez Urueña
- Sebastián Enrique Barros Barros

---

<sub>Proyecto académico — TDSE, Escuela Colombiana de Ingeniería Julio Garavito, 2026.</sub>
