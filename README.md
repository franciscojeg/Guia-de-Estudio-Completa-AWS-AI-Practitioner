
# Guía de Estudio Completa: AWS AI Practitioner

## Tabla de Contenidos

1. Terminología Central de IA/ML y Paradigmas de Aprendizaje
2. Flujo de Trabajo (Workflow) de ML de Extremo a Extremo
3. Tipos Comunes de Prompts y Uso
4. Servicios de Alto Nivel de AWS AI
5. Servicios Paraguas de SageMaker y sus Funciones
6. Servicios Paraguas de Bedrock
7. Observabilidad y Monitoreo en AWS
8. Seguridad: AWS PrivateLink y Prácticas Fundamentales
9. Métricas de Evaluación Clave
10. Modelo de Responsabilidad Compartida
11. Personalización de Modelos
12. Despliegue de Modelos para Inferencia
13. Capacidades de GenAI y Objetivos Responsables
14. Opciones de Base de Datos Vectorial en AWS
15. Tipos de Modelos de Bedrock y sus Casos de Uso
16. Evaluación Automática vs. Humana
17. Modelos de Bedrock que Soportan Ajuste Fino
18. Jerarquía y Conceptos Fundamentales de IA/ML
19. Tipos de Aprendizaje y Errores de Modelado

---

## 1. Terminología Central de IA/ML y Paradigmas de Aprendizaje

### Terminología Clave de IA/ML

#### Tokens
- **Definición**: Unidades textuales más pequeñas que un modelo de IA puede comprender
- **Ejemplos**: Palabras, letras, puntuación
- **Uso en AWS**: Bedrock cobra por tokens de entrada y salida
- **Límite de contexto**: Los modelos tienen límites (ej: Claude tiene 200K tokens)

#### Embeddings (Incrustaciones)
- **Definición**: Representaciones numéricas de objetos del mundo real (palabras, imágenes)
- **Estructura**: Vector grande asociado a un dato en espacio multidimensional
- **Dimensionalidad**: Vectores de 384 a 4096+ dimensiones
- **Función**: Capturar significado semántico donde elementos similares están cerca
- **Uso**: Búsqueda semántica, RAG, clasificación
- **En AWS**: Amazon Titan Embeddings, modelos de embedding en Bedrock

#### Regresión, Clasificación y Clustering

| Tipo | Paradigma | Descripción | Ejemplo |
|------|-----------|-------------|---------|
| **Clasificación** | Supervisado | Predice categorías discretas (binaria o multiclase) | Spam/No spam, clasificar imágenes |
| **Regresión** | Supervisado | Predice valores continuos | Predecir precio de casa, temperatura |
| **Clustering** | No Supervisado | Agrupa datos similares sin etiquetas previas | Segmentación de clientes, detección de anomalías |

### Paradigmas de Aprendizaje

Los algoritmos de Machine Learning se dividen en tres paradigmas principales:

#### 1. Aprendizaje Supervisado
- **Datos**: Etiquetados (entrada + salida correspondiente)
- **Objetivo**: Aprender mapeo de entrada → salida
- **Tareas**: Clasificación y Regresión
- **Ejemplos**: Clasificación de imágenes, predicción de ventas
- **Algoritmos**: Random Forest, SVM, redes neuronales

#### 2. Aprendizaje No Supervisado
- **Datos**: Sin etiquetas
- **Objetivo**: Encontrar estructuras, grupos o patrones
- **Tareas**: Clustering, reducción de dimensionalidad
- **Ejemplos**: Segmentación de clientes, detección de anomalías
- **Algoritmos**: K-means, PCA, autoencoders

#### 3. Aprendizaje por Refuerzo (RL)
- **Método**: Agente aprende mediante prueba y error
- **Componentes**: Estado, acción, recompensa
- **Proceso**: Interacciones con el entorno, recibiendo recompensas/penalizaciones
- **Ejemplos**: Juegos, robótica, optimización
- **En AWS**: SageMaker RL, AWS DeepRacer

---

## 2. Flujo de Trabajo (Workflow) de ML de Extremo a Extremo

El ciclo de vida del desarrollo de Machine Learning se compone de varias etapas, conocidas como la **Canalización de ML**.

| Etapa | Propósito | Servicios AWS (SageMaker) |
|-------|-----------|---------------------------|
| **1. Recopilación de datos** | Obtener la información necesaria | S3, Glue |
| **2. Análisis exploratorio (EDA)** | Comprender la estructura de los datos | SageMaker Studio |
| **3. Procesamiento previo** | Limpiar y transformar los datos | **SageMaker Data Wrangler** (interfaz visual para importar, transformar, analizar y exportar) |
| **4. Ingeniería de características** | Crear nuevas características para mejorar rendimiento | **SageMaker Feature Store** (repositorio para almacenar y compartir características) |
| **5. Entrenamiento del modelo** | Enseñar al modelo a aprender de los datos | SageMaker Notebooks, Model Training, **SageMaker Debugger** (guarda estado interno del modelo) |
| **6. Ajuste de hiperparámetros** | Optimizar parámetros de entrenamiento | SageMaker Hyperparameter Tuning |
| **7. Evaluación del modelo** | Medir el rendimiento del modelo | SageMaker Experiments |
| **8. Implementación** | Poner el modelo en producción | **SageMaker Endpoints** |
| **9. Supervisión (Monitoreo)** | Detectar desviaciones de calidad y deriva | **SageMaker Model Monitor** |

---

## 3. Tipos Comunes de Prompts y Uso

La **Ingeniería de Prompts** es la técnica de diseñar entradas claras y específicas para guiar a un modelo de IA generativa.

### Técnicas de Prompting

#### Zero-shot
- **Cuándo usar**: Tarea simple sin ejemplos necesarios
- **Descripción**: No se proporcionan ejemplos previos
- **Ejemplo**: "Traduce este texto al español: [texto]"
- **Base**: El modelo se basa únicamente en su entrenamiento inicial

#### One-shot / Few-shot
- **Cuándo usar**: Tarea compleja que necesita ejemplos
- **Descripción**: Se incluyen uno o varios ejemplos de la tarea deseada
- **Ejemplo**: Dar 2-3 ejemplos de clasificación antes de la tarea real
- **También conocido como**: Aprendizaje en contexto (In-context learning)
- **Ventaja**: Muy rentable

#### Cadena de Pensamiento (Chain of Thought - CoT)
- **Cuándo usar**: Razonamiento complejo
- **Descripción**: Guiar al modelo para mostrar razonamiento paso a paso
- **Ejemplo**: "Pensemos paso a paso..."
- **Utilidad**: Problemas complejos, matemáticas y lógica

#### System Prompts
- **Cuándo usar**: Definir comportamiento general del modelo
- **Ejemplo**: "Eres un asistente técnico experto en AWS"

#### Instruction Following
- **Cuándo usar**: Tareas específicas con formato de salida
- **Ejemplo**: "Resume en 3 puntos principales"

---

## 4. Servicios de Alto Nivel de AWS AI

AWS ofrece servicios gestionados para simplificar la implementación de soluciones de IA por dominio:

| Servicio | Dominio | Propósito Principal | Features Clave |
|----------|---------|---------------------|----------------|
| **Amazon Polly** | Voz | Text-to-Speech (TTS) | Múltiples voces e idiomas, SSML |
| **Amazon Transcribe** | Voz | Speech-to-Text (STT) - ASR | Identificación de hablantes, vocabulario personalizado |
| **Amazon Textract** | Visión/Documentos | Extrae texto y detalles de documentos | ML + OCR, análisis de facturas, IDs, recibos |
| **Amazon Comprehend** | NLP | Encuentra información y relaciones en texto | Análisis de sentimientos, extracción de entidades |
| **Amazon Lex** | NLP | Construir chatbots conversacionales | Interfaces voz y texto, IVR |
| **Amazon Rekognition** | Visión por Computadora | Analiza imágenes y videos | Detección de objetos, reconocimiento facial, moderación |
| **Amazon Translate** | NLP | Traducción automática neuronal | Rápida y alta calidad |
| **Amazon Fraud Detector** | ML Aplicado | Identifica actividades fraudulentas | Detección en línea |
| **Amazon Personalize** | ML Aplicado | Sistema de recomendaciones | Personalización de productos/contenido |

---

## 5. Servicios Paraguas de SageMaker y sus Funciones

**Amazon SageMaker** es un servicio integral para construir, entrenar y desplegar modelos de ML a cualquier escala.

### Componentes Principales

#### SageMaker Studio
- **Función**: IDE visual integrado para ML
- **Características**: Notebooks, debugging, visualización
- **Beneficio**: Crear y compartir notebooks

#### SageMaker JumpStart
- **Función**: Centro de ML para acelerar desarrollo
- **Características**: Modelos y algoritmos preentrenados listos para usar
- **Beneficio**: Selección, personalización y despliegue rápido

#### SageMaker Data Wrangler
- **Función**: Preparación de datos visual
- **Características**: Importar, transformar, analizar y exportar datos
- **Beneficio**: Feature engineering sin código

#### SageMaker Feature Store
- **Función**: Repositorio centralizado de features
- **Componentes**:
  - **Online Store**: Baja latencia para inferencia en tiempo real
  - **Offline Store**: Acceso por lotes para entrenamiento o inferencia batch
- **Beneficio**: Compartir features entre equipos

#### SageMaker Autopilot (AutoML)
- **Función**: AutoML - genera modelos automáticamente
- **Automatiza**: Selección de algoritmos, preprocesamiento, ajuste
- **Tipos de problemas**: Clasificación binaria, multiclase y regresión
- **Beneficio**: Explainability integrada

#### SageMaker Clarify
- **Función**: Detección de bias y explainability
- **Características**: Detecta sesgos potenciales en datos y modelos
- **Beneficio**: Informes de explicabilidad, fairness metrics

#### SageMaker Model Monitor
- **Función**: Monitoreo post-despliegue
- **Características**: Alertas sobre desviaciones de calidad
- **Detecta**: Deriva de datos y calidad del modelo

#### SageMaker Debugger
- **Función**: Ayuda durante el entrenamiento
- **Características**: Guarda periódicamente estado interno del modelo
- **Detecta**: Problemas con gradientes y tensores
- **Alertas**: Eventos de CloudWatch al activarse reglas

#### SageMaker Pipelines
- **Función**: Orquestación de workflows ML
- **Beneficio**: CI/CD para ML

#### SageMaker Ground Truth
- **Función**: Etiquetado de datos
- **Características**: Workforce humana o automática

#### SageMaker Endpoints
- **Función**: Deployment de modelos
- **Características**: Escalado automático, deployment controlado

---

## 6. Servicios Paraguas de Bedrock

**Amazon Bedrock** es un servicio totalmente gestionado que proporciona acceso a modelos fundacionales de alto rendimiento a través de una única API.

### Componentes Principales

#### Foundation Models
- **Acceso**: Múltiples modelos (Claude, Titan, Llama, etc.)
- **Interfaz**: API unificada
- **Beneficio**: Facilita creación y escalado de aplicaciones GenAI

#### Bedrock Agents (Agentes de LLM)
- **Definición**: LLMs con memoria, capacidad de planificación y herramientas
- **Capabilities**: 
  - Llamar APIs
  - Usar herramientas (funciones Lambda)
  - Razonamiento multi-step
- **Función**: Descomponer solicitudes complejas y ejecutar tareas autónomamente
- **Uso**: Automatización de tareas complejas

#### Bedrock Guardrails (Barandillas)
- **Función**: Características de seguridad flexibles
- **Filtros**:
  - Contenido nocivo
  - Blasfemias y temas sensibles
  - PII (Información Personal Identificable)
  - Respuestas falsas (alucinaciones)
- **Beneficio**: Uso responsable y ético

#### Bedrock Knowledge Bases (Bases de Conocimiento)
- **Función**: Implementación de RAG en Bedrock
- **Componentes**:
  - Ingesta de documentos/datos estructurados
  - Fragmentación (chunking) automática
  - Creación de embeddings
  - Vector store integrado
  - Retrieval optimizado
- **Uso**: Datos específicos para RAG

#### Bedrock Model Evaluation
- **Función**: Comparación de modelos
- **Métodos**: Métricas automáticas y evaluación humana

#### Bedrock Fine-tuning
- **Función**: Personalización de modelos base
- **Incluye**: Continued pretraining

#### Bedrock Prompt Management
- **Función**: Gestión de prompts
- **Características**: Versionamiento, pruebas A/B

---

## 7. Observabilidad y Monitoreo en AWS

El monitoreo es crucial para la etapa final del ciclo de vida del ML.

### Amazon CloudWatch

#### CloudWatch Logs
- **Función**: Logs de aplicaciones y servicios
- **Uso**: Logs de SageMaker endpoints
- **Herramienta**: CloudWatch Logs Insights para queries

#### CloudWatch Metrics
- **Función**: Métricas de uso de recursos
- **Monitorea**: Latencia de endpoints, uso de recursos
- **Visualización**: Dashboards personalizados

#### CloudWatch Alarms
- **Función**: Alertas basadas en umbrales
- **Integración**: SNS para notificaciones

### SageMaker Model Monitor

Servicio que supervisa modelos en producción y envía alertas vía CloudWatch.

#### Tipos de Monitoreo

| Tipo | Función | Detecta |
|------|---------|---------|
| **Data Quality Monitoring** | Detecta drift en distribución de datos de entrada | Cambios en datos de entrada vs baseline |
| **Model Quality Monitoring** | Monitorea accuracy y otras métricas | Degradación de rendimiento (requiere ground truth) |
| **Bias Drift Monitoring** | Detecta cambios en métricas de fairness | Deriva de sesgo (integración con Clarify) |
| **Feature Attribution Drift** | Monitorea cambios en importancia de features | SHAP values |

---

## 8. Seguridad: AWS PrivateLink y Prácticas Fundamentales

### AWS PrivateLink
- **Función**: Conectividad privada entre VPCs y servicios AWS
- **Uso**: Acceder a Bedrock/SageMaker sin internet público
- **Beneficio**: Tráfico no sale de la red AWS

### Prácticas Fundamentales

#### Encryption (Cifrado)
- **En reposo**: S3, EBS con KMS
- **En tránsito**: TLS/HTTPS
- **Servicio**: AWS Key Management Service (KMS)

#### IAM (Identity and Access Management)
- **Principio**: Mínimo privilegio (Least Privilege)
- **Gestión**: Roles específicos por servicio
- **Configuración**: Políticas granulares

#### VPC Configuration
- **Componentes**: Security groups y NACLs
- **Aislamiento**: VPC endpoints
- **Beneficio**: Aislamiento de red

#### Auditing
- **Logs de API**: CloudTrail
- **Compliance**: AWS Config

#### Data Protection
- **PII**: Guardrails de Bedrock para redacción
- **Descubrimiento**: Amazon Macie para datos sensibles

#### Control de Acceso
- **Servicios**: Amazon Q Business
- **Integración**: IAM Identity Center y SAML 2.0

---

## 9. Métricas de Evaluación Clave

Métricas utilizadas principalmente para evaluar Modelos Fundacionales en tareas de generación de texto:

### BLEU (Bilingual Evaluation Understudy)
- **Uso**: Traducción automática
- **Método**: Compara n-gramas entre predicción y referencia humana
- **Rango**: 0-1 (más alto = mejor)
- **Limitación**: No considera significado semántico

### ROUGE (Recall-Oriented Understudy for Gisting Evaluation)
- **Uso**: Resúmenes de texto
- **Variantes**:
  - **ROUGE-N**: Overlap de n-gramas
  - **ROUGE-L**: Subsecuencia común más larga
- **Enfoque**: Recall (qué tanto de la referencia se captura)
- **Método**: Compara superposición entre resúmenes producidos y de referencia

### BERTScore
- **Uso**: Evaluación de generación de texto
- **Método**: Compara similitudes semánticas a nivel de token
- **Base**: Utiliza embeddings de BERT
- **Ventaja**: Captura similitud semántica

### Otras Métricas Importantes

| Métrica | Uso | Descripción |
|---------|-----|-------------|
| **Accuracy** | Clasificación | % de predicciones correctas (útil con clases balanceadas) |
| **Precision** | Clasificación | De las predicciones positivas, cuántas son correctas |
| **Recall (Sensibilidad)** | Clasificación | De los positivos reales, cuántos se detectan |
| **F1 Score** | Clasificación | Media armónica de precision y recall |
| **AUC-ROC** | Clasificación binaria | Área bajo curva ROC |
| **Perplexity** | Modelos de lenguaje | Más bajo = mejor |

---

## 10. Modelo de Responsabilidad Compartida

Define cómo se dividen las responsabilidades de seguridad y cumplimiento entre AWS y sus clientes.

### Responsabilidades de AWS (Security OF the Cloud)

#### Infraestructura
- Hardware físico
- Networking
- Instalaciones (datacenters)
- Regiones y zonas de disponibilidad

#### Software
- Hypervisor
- Servicios gestionados (plataforma)
- Mantenimiento y patches del servicio

#### Ejemplo en ML
- AWS gestiona: infraestructura de SageMaker, disponibilidad de Bedrock
- AWS mantiene: seguridad de los foundation models base

### Responsabilidades del Cliente (Security IN the Cloud)

#### Datos
- Encriptación de datos
- Clasificación de información
- Backup y recovery
- Proteger datos sensibles (PII)

#### Configuración
- IAM policies
- Network configuration (VPC, security groups)
- Configuración de servicios

#### Aplicación
- Código de la aplicación
- Prompts y configuración de modelos
- Monitoreo y logging

#### Ejemplo en ML
- Cliente gestiona: datos de entrenamiento, prompts, guardrails configurados
- Cliente configura: quién puede acceder a modelos, qué datos se procesan

---

## 11. Personalización de Modelos: Prompting vs Fine-tuning vs RAG vs Continued Pretraining

Diferentes enfoques para adaptar un Modelo Fundacional a tareas específicas:

| Enfoque | Propósito Principal | Costo/Esfuerzo | Datos Necesarios | Cuándo Usar |
|---------|---------------------|----------------|------------------|-------------|
| **Prompting** | Guiar con entradas claras y específicas | Muy rentable (solo entrada) | Ninguno | Primera opción, tareas generales |
| **RAG** | Consultar base de conocimientos externa | Medio | Documentos | Conocimiento específico/actualizado |
| **Fine-tuning** | Adaptar a conjunto de datos específico | Alto | Miles de ejemplos | Comportamiento/tarea específica |
| **Continued Pretraining** | Entrenamiento con corpus grande de dominio | Muy Alto | Millones de tokens | Nuevo dominio técnico o idioma |
| **Pre-entrenamiento** | Entrenar desde cero | Extremadamente costoso | Enormes volúmenes | Crear nuevo modelo fundacional |

### Detalles por Enfoque

#### Prompting
- **Ventajas**: Sin costo adicional, rápido, flexible
- **Limitaciones**: Limitado por contexto, no aprende nuevo conocimiento
- **Técnicas**: Zero-shot, Few-shot, Chain of Thought

#### RAG (Retrieval-Augmented Generation)
- **Función**: Optimiza salida consultando base de conocimientos
- **Ventajas**: Información fresca, citaciones, menor costo que fine-tuning
- **Limitaciones**: Dependiente de calidad de retrieval
- **AWS**: Bedrock Knowledge Bases
- **Beneficio**: Respuestas más precisas, actualizadas y contextualizadas
- **Comparación**: Más rápido y económico que fine-tuning para actualizar conocimiento

#### Fine-tuning
- **Función**: Reentrenar modelo con datos específicos
- **Ventajas**: Adapta comportamiento del modelo, crea modelos fiables con menos datos
- **Limitaciones**: Costoso, requiere datos de calidad, puede olvidar conocimiento general
- **AWS**: Bedrock fine-tuning, SageMaker Training
- **Casos de uso**: Tono/estilo específico, formato estructurado, vocabulario de dominio

#### Continued Pretraining
- **Función**: Entrenar modelo base con corpus grande
- **Ventajas**: Modelo aprende vocabulario y patrones del dominio
- **Limitaciones**: Muy costoso, requiere datos masivos
- **Diferencia con fine-tuning**: Más datos, menos estructurado

---

## 12. Despliegue de Modelos para Inferencia

La **inferencia** es el proceso en el que los modelos entrenados extraen conocimientos de datos nuevos.

### Tipos de Inferencia

#### Real-Time Inference (Tiempo Real)
- **Cuándo**: Latencia baja requerida (milisegundos)
- **Uso**: Aplicaciones interactivas, chatbots, detección de fraude, sistemas de recomendación
- **AWS**: SageMaker Real-Time Endpoints, Online Store de Feature Store
- **Características**:
  - Endpoints persistentes
  - Auto-scaling disponible
  - Mayor costo (siempre corriendo)

#### Batch Transform (Inferencia por Lotes)
- **Cuándo**: Procesar grandes volúmenes sin requisitos de tiempo real
- **Uso**: Procesamiento nocturno, análisis de datasets completos
- **AWS**: SageMaker Batch Transform, Offline Store de Feature Store
- **Características**:
  - Sin endpoint permanente
  - Procesa archivos de S3
  - Más económico para volúmenes grandes
  - Eficiencia > inmediatez

#### Asynchronous Inference
- **Cuándo**: Payloads grandes o tiempos de procesamiento largos
- **Uso**: Procesamiento de videos, documentos grandes
- **AWS**: SageMaker Asynchronous Inference
- **Características**:
  - Cola de requests
  - Escala a cero cuando no hay demanda
  - Notificaciones SNS al completar

#### Serverless Inference
- **Cuándo**: Tráfico intermitente o impredecible
- **Uso**: Aplicaciones con uso variable
- **AWS**: SageMaker Serverless Endpoints
- **Características**:
  - Escala automáticamente incluyendo a cero
  - Pago por uso (no por tiempo activo)
  - Cold start posible

### Despliegues Controlados (Guardrails de Implementación)

**SageMaker Deployment Guardrails**: Salvaguardas que controlan el cambio de tráfico hacia nuevos modelos

| Tipo | Descripción |
|------|-------------|
| **Blue/Green** | Todo el tráfico a la vez |
| **Canary** | Una pequeña porción de tráfico primero |
| **Linear** | En pasos espaciados linealmente |

- **Característica adicional**: Capacidad de reversión automática

---

## 13. Capacidades de GenAI y Objetivos Responsables

### Capacidades de GenAI

La **IA Generativa** se enfoca en producir nuevos datos en diversos formatos:

- Generación de imágenes, video y audio
- Resúmenes y traducción de texto
- Creación de chatbots
- Generación de código
- Datos sintéticos

### Objetivos de IA Responsable

Un sistema de IA responsable se caracteriza por:

#### Fairness (Equidad)
- **Objetivo**: Evitar discriminación por raza, género, edad
- **Herramienta AWS**: SageMaker Clarify
- **Acción**: Detectar y mitigar bias

#### Explainability (Explicabilidad)
- **Objetivo**: Entender por qué el modelo decidió algo
- **Métodos**: SHAP values, attention weights
- **Importancia**: Regulación y confianza

#### Transparency (Transparencia)
- **Objetivo**: Documentar capacidades y limitaciones
- **Métodos**: Model cards
- **Acción**: Divulgación de uso de AI

#### Privacy (Privacidad)
- **Objetivo**: Protección de PII
- **Principio**: Data minimization
- **Herramienta AWS**: Guardrails para redacción

#### Safety (Seguridad)
- **Objetivo**: Prevenir outputs nocivos
- **Método**: Content filtering
- **Herramienta AWS**: Bedrock Guardrails

#### Accountability (Responsabilidad)
- **Objetivo**: Auditoría de decisiones
- **Método**: Human-in-the-loop
- **Herramienta**: Logs y trazabilidad

#### Robustness (Robustez)
- **Objetivo**: Resistencia a inputs adversariales
- **Método**: Consistencia en outputs
- **Acción**: Testing exhaustivo

#### Inclusivity (Inclusividad)
- **Objetivo**: Accesible y beneficioso para todos

#### Governance (Gobernanza)
- **Objetivo**: Políticas y procesos claros

### Herramientas AWS para IA Responsable

- **Amazon SageMaker Clarify**: Detecta sesgos y explica predicciones
- **Guardrails para Amazon Bedrock**: Filtra contenido dañino y previene alucinaciones

---

## 14. Opciones de Base de Datos Vectorial en AWS

Las **Bases de Datos Vectoriales** almacenan embeddings para búsquedas de similitud semántica rápidas. Son esenciales para implementar RAG.

### Opciones Disponibles

#### Amazon OpenSearch Service
- **Características**:
  - Motor de búsqueda con capacidad vectorial
  - k-NN search
  - Escalable y gestionado
- **Uso**: RAG, búsqueda semántica

#### Amazon RDS for PostgreSQL (con pgvector)
- **Características**:
  - Extensión pgvector
  - Base de datos relacional + vectores
- **Uso**: Cuando ya usas PostgreSQL

#### Amazon Aurora PostgreSQL (con pgvector)
- **Características**:
  - Compatible con PostgreSQL
  - Mayor performance que RDS
  - Serverless disponible
- **Uso**: Alto rendimiento con PostgreSQL

#### Amazon Neptune
- **Características**:
  - Base de datos de grafos
  - Soporte para embeddings
- **Uso**: Relaciones complejas + búsqueda vectorial

#### Amazon DocumentDB
- **Características**:
  - Compatible con MongoDB
  - Soporte nativo para vectores
- **Uso**: Aplicaciones NoSQL con vectores

#### Amazon MemoryDB for Redis
- **Características**:
  - In-memory con persistencia
  - Baja latencia
  - VSS (Vector Similarity Search)
- **Uso**: Aplicaciones de ultra-baja latencia

#### FAISS on EC2/ECS
- **Características**:
  - Librería de Meta
  - Alto rendimiento
  - Requiere gestión manual
- **Uso**: Control total sobre implementación

#### Pinecone / Weaviate (AWS Marketplace)
- **Características**:
  - Vector databases especializadas
  - Managed service
  - Integración con AWS
- **Uso**: Soluciones especializadas

---

## 15. Tipos de Modelos de Bedrock y sus Casos de Uso

Amazon Bedrock proporciona acceso a **Modelos Fundacionales (FMs)** a gran escala.

### Tipos de Modalidad

- **Modelos Fundacionales (FMs)**: Entrenados en conjuntos de datos diversos
- **LLMs (Large Language Models)**: Especializados en lenguaje
- **Modelos Multimodales**: Procesan múltiples tipos de datos (texto, imágenes, audio)

### Modelos Específicos

#### Modelos de Texto (LLMs)

| Modelo | Fortalezas | Usos Principales |
|--------|------------|------------------|
| **Amazon Titan Text** | Generación de texto, soporte multiidioma | Chatbots, resúmenes, Q&A, generación de contenido |
| **Anthropic Claude** | Seguridad, alineación con valores humanos, contexto largo (200K tokens) | Conversación avanzada, análisis de documentos, coding |
| **Meta Llama** | Código abierto, tamaño compacto, eficiencia | Aplicaciones generales, recursos limitados |
| **AI21 Jurassic** | - | Contenido empresarial |
| **Cohere Command** | - | Generación y clasificación |

#### Modelos de Embeddings

| Modelo | Características | Usos |
|--------|----------------|------|
| **Amazon Titan Embeddings** | Dimensiones: 1024 o 384, Multilingüe | RAG, búsqueda semántica |
| **Cohere Embed** | Alta calidad para retrieval | Sistemas de búsqueda |

#### Modelos Multimodales

| Modelo | Modalidades | Usos |
|--------|-------------|------|
| **Anthropic Claude** (versiones multimodales) | Texto + imágenes | Análisis de documentos con imágenes, OCR inteligente |
| **Amazon Titan Image Generator** | Texto → imagen | Marketing, diseño, creación de arte digital |
| **Amazon Titan Multimodal Embeddings** | Embeddings para texto e imágenes | Búsqueda cross-modal |

#### Modelos de Difusión

| Modelo | Especialidad | Usos |
|--------|--------------|------|
| **Stable Diffusion** | Generación de imágenes de alta calidad, versatilidad en estilos | Arte digital, diseño gráfico |

---

## 16. Evaluación Automática vs. Humana (Human-in-the-Loop)

### Human-in-the-Loop (HITL)

#### Qué es
- Humanos revisan y validan outputs del modelo
- Intervención manual en el proceso
- Considerado el **"estándar de oro"** para evaluar calidad

#### Cuándo usar
- Contenido creativo (requiere juicio subjetivo)
- Decisiones críticas (médicas, legales)
- Tareas con matices culturales/contextuales
- Evaluación de seguridad y ética

#### Ventajas
- Alta calidad de evaluación
- Detecta problemas sutiles
- Captura aspectos no medibles automáticamente

#### Desventajas
- Costoso
- Lento
- No escala bien

#### Herramientas AWS
- **SageMaker Ground Truth** (labeling)
- **Amazon Augmented AI (A2I)**: Simplifica incorporación de revisión humana para garantizar resultados de alta calidad
- **Bedrock Model Evaluation** con evaluadores humanos

### Automatic Evaluation

#### Qué es
- Métricas computadas automáticamente
- Sin intervención humana

#### Cuándo usar
- Tareas objetivas (clasificación, traducción)
- Iteraciones rápidas de desarrollo
- Monitoreo continuo
- Comparación de múltiples modelos

#### Ventajas
- Rápido y escalable
- Consistente y reproducible
- Económico

#### Desventajas
- Puede no capturar calidad real
- Limitado a aspectos medibles
- Puede no detectar problemas sutiles

#### Métricas Comunes
- Accuracy, F1, BLEU, ROUGE
- Perplexity, BERTScore

#### Herramientas AWS
- **SageMaker Model Monitor**
- **Bedrock Model Evaluation** (métricas automáticas)
- **CloudWatch Metrics**

### Estrategia Híbrida (Recomendada)

| Fase | Enfoque |
|------|---------|
| **Desarrollo** | Automatic evaluation para iteración rápida |
| **Validación** | HITL para casos críticos y edge cases |
| **Producción** | Automatic monitoring + HITL sampling periódico |

---

## 17. Modelos de Bedrock que Soportan Ajuste Fino (Fine-tuning)

Amazon Bedrock soporta la **personalización de modelos**, incluyendo fine-tuning.

### Modelos que Soportan Fine-tuning

| Modelo | Soporte | Casos de Uso |
|--------|---------|--------------|
| **Amazon Titan Text** | ✅ Soporta | Adaptar tono empresarial |
| **Amazon Titan Image Generator** | ✅ Soporta | Estilo visual de marca |
| **Cohere Command** | ✅ Soporta | Tareas de clasificación específicas |
| **Meta Llama** | ✅ Algunas versiones | Verificar documentación actual |

### Modelos que NO Soportan Fine-tuning

| Modelo | Soporte | Alternativa |
|--------|---------|-------------|
| **Anthropic Claude** | ❌ No soporta en Bedrock | Prompting avanzado, RAG |
| **AI21 Jurassic** | ⚠️ Verificar | Verificar disponibilidad actual |

**Nota**: Aunque las fuentes confirman que Bedrock ofrece capacidad de personalización incluyendo fine-tuning, no especifican una lista exacta y exhaustiva de todos los modelos que actualmente lo soportan.

### Proceso de Fine-tuning en Bedrock

#### 1. Preparación de Datos
- **Formato**: JSONL
- **Estructura**: Pares de prompt-completion
- **Cantidad**: Mínimo de ejemplos (varía por modelo)
- **Calidad**: Generalmente 100-1000+ ejemplos
- **Diversidad**: Variedad de casos

#### 2. Configuración
- Seleccionar modelo base
- Hiperparámetros (learning rate, epochs)
- Configuración de validación

#### 3. Ejecución
- Job de fine-tuning gestionado
- Monitoreo de progreso

#### 4. Evaluación
- Comparación con modelo base
- Métricas de validación

#### 5. Deployment
- Crear endpoint con modelo personalizado
- A/B testing opcional

### Consideraciones

#### Costos
- Training cost (por hora de GPU)
- Inference cost (típicamente igual al modelo base)
- Storage del modelo fine-tuned

#### Casos de Uso Ideales
- Tono/estilo específico consistente
- Formato de salida muy estructurado
- Vocabulario de dominio específico
- Clasificación con categorías fijas

---

## 18. Jerarquía y Conceptos Fundamentales de IA/ML

### Jerarquía de IA, ML y Deep Learning

La relación entre estos campos es jerárquica:

```
Inteligencia Artificial (IA)
    └── Machine Learning (ML)
        └── Deep Learning (DL)
            └── IA Generativa (GenAI)
```

#### 1. Inteligencia Artificial (IA)
- **Definición**: Campo general de informática centrado en crear sistemas que imitan la inteligencia humana
- **Tareas**: Comprender lenguaje natural, resolver desafíos complejos

#### 2. Aprendizaje Automático (ML)
- **Definición**: Subconjunto de IA que enseña a computadoras a aprender de datos
- **Característica**: Mejora rendimiento con el tiempo sin programación explícita

#### 3. Aprendizaje Profundo (Deep Learning - DL)
- **Definición**: Subconjunto especializado de ML
- **Técnica**: Usa Redes Neuronales Artificiales (RNA) con múltiples capas
- **Fortaleza**: Aprende patrones y representaciones complejas de grandes conjuntos de datos

#### 4. IA Generativa (GenAI)
- **Definición**: Subconjunto del Deep Learning
- **Enfoque**: Producir datos nuevos y originales
- **Outputs**: Texto, audio, imágenes, datos sintéticos

---

## 19. Tipos de Aprendizaje y Errores de Modelado

### Errores de Modelado: Sesgo/Varianza y Sobreajuste/Subajuste

El **Sobreajuste** y el **Subajuste** son problemas de rendimiento, mientras que el **Sesgo** y la **Varianza** son los tipos de errores subyacentes.

| Problema | Error Asociado | Causa | Consecuencia |
|----------|----------------|-------|--------------|
| **Sobreajuste (Overfitting)** | Alta Varianza | Modelo demasiado sensible a datos de entrenamiento específicos; los memoriza | Funciona bien con datos de entrenamiento, pero mal con datos nuevos |
| **Subajuste (Underfitting)** | Alto Sesgo | Modelo hace suposiciones incorrectas o es demasiado simple | Falla en capturar patrones, bajo rendimiento general (entrenamiento y nuevos) |

### Definiciones de Errores

#### Sesgo (Bias)
- **Definición**: Diferencia entre la predicción promedio del modelo y el valor real
- **Relación**: Un sesgo alto se relaciona con el subajuste

#### Varianza (Variance)
- **Definición**: Mide la inconsistencia de las predicciones al entrenarse en diferentes conjuntos de datos
- **Relación**: Una varianza alta se relaciona con el sobreajuste

### Trade-off Sesgo-Varianza

El objetivo es encontrar el balance óptimo entre sesgo y varianza para lograr la mejor generalización.

---

## Metáfora para Solidificar la Comprensión

Visualice el ecosistema de AWS para ML como una **cocina de alta tecnología**:

### Amazon SageMaker
🏭 **La cocina completa y bien equipada**
- Todas las estaciones de trabajo (Data Wrangler, Feature Store, Debugger)
- Chefs especializados (Autopilot)
- Para crear una receta compleja (el modelo)

### Amazon Bedrock
🍽️ **Servicio de catering de modelos fundacionales**
- Modelos ya horneados de los mejores chefs del mundo (Anthropic, Amazon, etc.)
- No necesita construir la estufa ni gestionar la infraestructura

### Fine-Tuning
🧂 **Agregar especias**
- A una receta pre-hecha para adaptarla a su gusto local

### RAG
📚 **Libro de referencia de recetas**
- Libro específico de su abuela (base de conocimientos corporativa)
- Al lado del modelo para verificar información reciente antes de responder

---

## Tips Finales para el Examen

### Conceptos Clave a Memorizar

1. **Diferencias principales**: Supervised vs unsupervised vs reinforcement
2. **Cuándo usar cada técnica**: Prompting → RAG → Fine-tuning → Continued pretraining
3. **Servicios específicos**: Qué hace cada servicio AWS (Polly, Transcribe, etc.)
4. **Componentes de Bedrock**: Agents, Guardrails, Knowledge Bases
5. **Tipos de deployment**: Real-time vs batch vs async vs serverless
6. **Responsabilidad compartida**: Qué gestiona AWS vs qué gestionas tú
7. **Métricas**: BLEU para traducción, ROUGE para resúmenes
8. **Jerarquía**: IA > ML > DL > GenAI
9. **Errores**: Alto sesgo = underfitting, Alta varianza = overfitting
10. **SageMaker vs Bedrock**: SageMaker para ML completo, Bedrock para GenAI

### Estrategia de Estudio

✅ Revise los servicios AWS y sus funciones específicas

✅ Practique identificando cuándo usar cada técnica de personalización

✅ Entienda las diferencias entre los tipos de inferencia

✅ Memorice las métricas clave y sus usos

✅ Comprenda el modelo de responsabilidad compartida

✅ Revise casos de uso de cada servicio de alto nivel

---

**¡Buena suerte en tu examen AWS AI Practitioner!** 🚀
