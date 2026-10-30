# 🤖🔐 AMENAZAS DIGITALES
### Del ataque a la defensa: IA y Machine Learning para detectar comportamientos anómalos

**¿Podemos reconocer una señal de alerta antes de que se convierta en un incidente?**

Un laboratorio educativo de ciberseguridad que explora cómo utilizar Machine Learning para analizar registros de autenticación, identificar comportamientos inusuales y apoyar la toma de decisiones de los equipos de seguridad.

<div align="center">

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Orliluq/amenazas_digitales/blob/main/amenazas_digitales.ipynb)
[![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python)](https://www.python.org/)
[![Machine Learning](https://img.shields.io/badge/Machine%20Learning-Isolation%20Forest-orange)](https://scikit-learn.org/)
[![License MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Status](https://img.shields.io/badge/Status-Educational%20Project-informational)](#-alcance-y-limitaciones)

**[🚀 Ejecutar laboratorio](https://colab.research.google.com/github/Orliluq/amenazas_digitales/blob/main/amenazas_digitales.ipynb) · [📓 Explorar notebook](amenazas_digitales.ipynb) · [👩🏻‍💻 GitHub](https://github.com/Orliluq)**

</div>

---

## 🕸️ ¿De qué trata este proyecto?

Los ciberataques no siempre comienzan con una alarma evidente. A veces, la primera señal es una secuencia de intentos fallidos de inicio de sesión, un acceso desde una ubicación inusual o una actividad que se aparta del comportamiento habitual de una cuenta.

En un entorno donde usuarios, aplicaciones y dispositivos generan grandes cantidades de eventos, revisar cada registro manualmente puede convertirse en un desafío.

Este proyecto explora una aplicación concreta de la inteligencia artificial en ciberseguridad: **la detección de anomalías en registros de autenticación mediante Machine Learning**.

A través de Python y el algoritmo **Isolation Forest** de scikit-learn, construimos un pequeño detector que aprende patrones a partir de un conjunto histórico de actividad considerada normal y evalúa registros recientes para señalar aquellos que merecen una revisión.

> 🛡️ La IA no reemplaza el criterio de un equipo de seguridad. Puede ayudar a encontrar señales, organizar información y orientar una investigación que debe seguir siendo verificable.

## 🎯 Objetivos de aprendizaje

- Comprender qué es la detección de anomalías y cómo se aplica a la ciberseguridad.
- Explorar el funcionamiento de Isolation Forest.
- Preparar y validar registros de autenticación con pandas.
- Entrenar un modelo de Machine Learning con datos históricos.
- Analizar nuevos eventos y obtener puntuaciones de anomalía.
- Visualizar resultados para facilitar su interpretación.
- Reconocer las limitaciones de un detector estadístico.
- Explorar cómo la IA generativa puede apoyar la explicación y documentación de alertas.

## 🧭 ¿Cómo funciona el laboratorio?

El flujo de trabajo se divide en seis etapas:

1. **Preparación:** creación de dos conjuntos de datos CSV.
2. **Validación:** comprobación de columnas y valores.
3. **Entrenamiento:** ajuste del modelo Isolation Forest.
4. **Análisis:** evaluación de los registros recientes.
5. **Detección:** identificación de posibles anomalías.
6. **Visualización:** interpretación de las puntuaciones y revisión humana.

### 🔄 Flujo de análisis

```text
Historial de actividad revisada
             |
             v
    Preparación de datos
             |
             v
     Validación de datos
             |
             v
 Entrenamiento Isolation Forest
             |
             v
  Análisis de nuevos accesos
             |
             v
     Predicción + Score
             |
             v
 Visualización de resultados
             |
             v
      Revisión humana
             |
             v
 Investigación y respuesta
```

## 🧪 ¿Qué variables analizamos?

Cada registro representa una ventana de actividad de autenticación.

| Variable | Descripción |
|---|---|
| `fallos` | Cantidad de intentos de autenticación fallidos. |
| `cuentas_distintas` | Número de cuentas diferentes a las que se intentó acceder. |
| `hora_utc` | Hora en la que ocurrió la actividad, expresada en UTC. |
| `pais_nuevo` | Indica si el acceso proviene de un país no habitual (0 o 1). |
| `mfa_fallida` | Cantidad de intentos fallidos de autenticación multifactor. |

Estas características permiten representar distintos aspectos del comportamiento de acceso.

**Importante:** son variables ilustrativas. El laboratorio no está conectado a un proveedor de identidad ni recibe registros reales de una infraestructura empresarial.

### 📁 Archivos de datos

| Archivo | Función |
|---|---|
| `accesos_normales.csv` | Historial de ejemplo utilizado para entrenar el modelo. |
| `accesos_recientes.csv` | Nuevos registros que se desean analizar. |

Ambos archivos se generan mediante celdas del notebook y utilizan las mismas columnas.

Los datos son sintéticos y educativos. No representan usuarios reales ni víctimas de ciberataques.

## 🧠 ¿Por qué Isolation Forest?

[Isolation Forest](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.IsolationForest.html) es un algoritmo de Machine Learning no supervisado utilizado para identificar observaciones atípicas (*outliers*).

A diferencia de un clasificador supervisado, no necesita que cada registro de entrenamiento tenga una etiqueta previa como «ataque» o «normal».

Su funcionamiento se basa en árboles de aislamiento que permiten identificar observaciones que se separan con mayor facilidad del resto.

### ⚙️ Configuración del modelo

```python
modelo = IsolationForest(
    n_estimators=200,
    contamination=0.02,
    random_state=42
)
```

| Parámetro | Valor | Función |
|---|---:|---|
| `n_estimators` | 200 | Número de árboles de aislamiento. |
| `contamination` | 0.02 | Proporción esperada de observaciones atípicas para establecer el umbral. |
| `random_state` | 42 | Semilla para reproducibilidad. |

**Una aclaración importante:** `contamination=0.02` no significa que el 2 % de los registros sean ataques reales. Es un parámetro para determinar el umbral de clasificación y debe ajustarse según los datos y el contexto.

### 🔎 Interpretación de resultados

El método `predict()` devuelve:

| Resultado | Interpretación |
|---|---|
| `1` | No identificado como anomalía. |
| `-1` | Posible anomalía. |

También utilizamos `decision_function()` para obtener una puntuación de decisión:

- Puntuaciones más bajas: observaciones más atípicas.
- Puntuaciones más altas: observaciones más similares al patrón aprendido.

La puntuación no representa la probabilidad de que exista un ataque. Tampoco debe asumirse que cualquier valor negativo equivale automáticamente a una anomalía: la clasificación depende del umbral aprendido por el modelo.

## 📊 Visualización de resultados

El notebook utiliza Matplotlib y Seaborn para representar las puntuaciones de anomalía.

El gráfico permite:

- Comparar las puntuaciones de los accesos recientes.
- Identificar los registros más atípicos.
- Visualizar cuáles fueron clasificados como posibles anomalías.
- Facilitar la revisión de los resultados.

### 📌 Resultado de la ejecución de demostración

Con los datos sintéticos incluidos actualmente, el notebook devuelve:

```text
RESULTADO DEL ANÁLISIS

No se detectaron anomalías con este modelo.
```

Esto significa que ninguno de los registros recientes fue clasificado como anomalía con el umbral configurado en esa ejecución.

**No significa que todos los accesos sean seguros.**

El conjunto de datos es pequeño y educativo. Su propósito es demostrar el funcionamiento del proceso, no medir la eficacia de un sistema de detección en producción.

## 🚀 Instalación y ejecución

### Opción 1. Google Colab (recomendada)

No necesitas instalar Python en tu equipo para comenzar.

1. Accede al notebook desde el siguiente enlace.
2. Abre el laboratorio en Google Colab.
3. Ejecuta las celdas en orden.
4. Observa la preparación de los datos, el entrenamiento y los resultados.

<div align="center">

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Orliluq/amenazas_digitales/blob/main/amenazas_digitales.ipynb)

</div>

### Opción 2. Ejecutar localmente

**Requisitos:**

- Python 3.
- pip.
- Jupyter Notebook o JupyterLab.

**1. Clonar el repositorio**

```bash
git clone https://github.com/Orliluq/amenazas_digitales.git

cd amenazas_digitales
```

**2. Crear un entorno virtual**

```bash
python -m venv .venv
```

Activarlo en Windows:

```powershell
.venv\Scripts\Activate.ps1
```

Activarlo en macOS o Linux:

```bash
source .venv/bin/activate
```

**3. Instalar las dependencias**

```bash
python -m pip install --upgrade pip

pip install pandas scikit-learn matplotlib seaborn jupyterlab
```

**4. Ejecutar JupyterLab**

```bash
jupyter lab
```

Abre `amenazas_digitales.ipynb` y ejecuta las celdas en orden.

> 💡 El notebook utiliza la instrucción mágica `%%writefile` de IPython para generar los archivos CSV. Por eso debe ejecutarse en Jupyter o Google Colab, no como un archivo Python convencional.

## 🗂️ Estructura del repositorio

```text
amenazas_digitales/
│
├── 📓 amenazas_digitales.ipynb
│   └── Laboratorio de Machine Learning
│
├── 📄 README.md
│   └── Documentación del proyecto
│
└── ⚖️ LICENSE
    └── Licencia MIT
```

Los archivos CSV se generan durante la ejecución del notebook y no están versionados actualmente en el repositorio.

## 🤖 ¿Dónde entra la IA generativa?

Este proyecto distingue dos capacidades que pueden complementarse:

| Tecnología | Aplicación |
|---|---|
| Machine Learning — Isolation Forest | Implementado: identifica observaciones estadísticamente atípicas. |
| IA generativa — modelos de lenguaje | Propuesta: interpretar alertas, organizar evidencias y preparar resúmenes para analistas. |

La IA generativa todavía no está conectada mediante una API en este laboratorio.

### ✨ Prompt para analizar alertas

```text
Actúa como asistente de análisis de ciberseguridad defensiva.

Analiza los siguientes eventos de autenticación anonimizados.

Identifica patrones inusuales, explica qué indicadores merecen
investigación y sugiere acciones preventivas.

No afirmes que existe una intrusión sin evidencias suficientes.

Distingue entre:
- Hechos observados.
- Hipótesis.
- Recomendaciones.

No inventes información que no esté presente en los registros.
No atribuyas la actividad a una persona o grupo sin evidencias.
```

La combinación de ambas tecnologías permite explorar un flujo interesante:

- **Machine Learning:** identifica comportamientos estadísticamente inusuales.
- **IA generativa:** ayuda a interpretar y documentar los resultados.
- **Criterio humano:** verifica evidencias y decide las acciones correspondientes.

## 🛡️ Alcance y limitaciones

Este repositorio es un laboratorio educativo y demostrativo. No es una solución de ciberseguridad lista para producción.

Algunas limitaciones importantes:

- Los datos son pequeños y sintéticos.
- No existen etiquetas verificadas de ataques.
- Una anomalía no significa necesariamente una amenaza.
- Un acceso considerado normal no garantiza que sea seguro.
- No se bloquean usuarios automáticamente.
- No existe integración en tiempo real con SIEM, VPN, firewalls o proveedores de identidad.
- No se han evaluado métricas de rendimiento sobre un conjunto de datos real y etiquetado.

### 🔐 Buenas prácticas

1. Anonimizar los registros antes de utilizarlos.
2. No publicar contraseñas, tokens ni información personal.
3. No compartir información confidencial con servicios externos de IA sin autorización.
4. Verificar las alertas con evidencia técnica.
5. Mantener supervisión humana antes de tomar decisiones.
6. Utilizar exclusivamente datos propios, sintéticos o autorizados.

## 🔭 Próximas mejoras

Algunas ideas para continuar evolucionando el proyecto:

- [ ] Ampliar el conjunto de datos sintéticos.
- [ ] Experimentar con diferentes valores de `contamination`.
- [ ] Incorporar métricas de evaluación con datos etiquetados.
- [ ] Analizar patrones temporales por usuario.
- [ ] Añadir validaciones de rangos y tipos de datos.
- [ ] Incorporar pruebas automatizadas.
- [ ] Exportar las alertas a CSV.
- [ ] Construir un panel interactivo de visualización.
- [ ] Explorar integración con fuentes de eventos de seguridad.
- [ ] Incorporar IA generativa para resumir alertas con revisión humana.

Estas son propuestas de evolución, no funcionalidades implementadas actualmente.

## 📚 Recursos para seguir aprendiendo

- [Scikit-learn — Isolation Forest](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.IsolationForest.html)
- [Scikit-learn — Outlier Detection](https://scikit-learn.org/stable/modules/outlier_detection.html)
- [Pandas — Documentación oficial](https://pandas.pydata.org/docs/)
- [Matplotlib — Documentación oficial](https://matplotlib.org/stable/)
- [Seaborn — Documentación oficial](https://seaborn.pydata.org/)
- [Google Colab](https://colab.research.google.com/)
- [NIST — Cybersecurity Framework](https://www.nist.gov/cyberframework)

## 👩🏻‍💻 Sobre la autora

**Orli Dun**  
Systems Engineer · Full Stack Developer · Tech Content Creator

Creo, aprendo y comparto tecnología con una convicción: la tecnología cobra más valor cuando podemos entenderla, experimentar con ella y ponerla al servicio de las personas.

Este laboratorio forma parte de una exploración sobre ciberseguridad, inteligencia artificial y desarrollo de software: aprender a pasar **del análisis de una amenaza a la construcción de una posible defensa**.

🔗 [GitHub](https://github.com/Orliluq) · [LinkedIn](https://www.linkedin.com/in/orlibetdungonzalez/) · [Mi mundo digital](https://orlidun.vercel.app/)

---

<div align="center">

### 💚 APRENDER. EXPERIMENTAR. PROTEGER.

**La IA puede ayudarnos a detectar señales. El criterio humano convierte esas señales en decisiones responsables.**

⭐ Si este laboratorio te resulta útil, puedes darle una estrella al repositorio y compartirlo con tu comunidad.

[⬆️ Volver al inicio](#-amenazas-digitales)

</div>
