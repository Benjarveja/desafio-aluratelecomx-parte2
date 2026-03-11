# Análisis de Fuga de Clientes - TelecomX LATAM

## Descripción del Proyecto

Este proyecto corresponde al segundo desafío del curso de Data Science de Alura LATAM. El objetivo principal es identificar los factores clave que impulsan la pérdida de clientes (**Churn**) en la empresa de telecomunicaciones TelecomX, y desarrollar modelos predictivos que permitan anticipar el abandono para implementar estrategias de retención efectivas.

El análisis procesa datos históricos de 7,267 usuarios, transformando datos crudos en hallazgos accionables que apoyan la toma de decisiones estratégicas.

---

## Estructura del Proyecto

```
desafio-aluratelecomx-parte2/
├── TelecomX_LATAM(1).ipynb   # Notebook principal con el análisis completo
├── datos_tratados.csv         # Dataset procesado y transformado
└── README.md                  # Documentación del proyecto
```

---

## Flujo de Trabajo

El análisis sigue el proceso ETL (Extracción, Transformación y Carga) junto con un pipeline de modelado predictivo:

### 1. Extracción

Los datos se obtienen directamente desde el repositorio del desafío en formato JSON:

```
https://raw.githubusercontent.com/ingridcristh/challenge2-data-science-LATAM/refs/heads/main/TelecomX_Data.json
```

### 2. Transformación

Se aplicaron los siguientes procesos de limpieza e ingeniería de datos:

- **Desanidación de datos**: Expansión de objetos JSON anidados (`customer`, `phone`, `internet`, `account`) a formato tabular.
- **Conversión de tipos**: Variables categóricas (`Yes`/`No`) convertidas a booleanas para optimizar el procesamiento.
- **Tratamiento de valores nulos**: La columna `Charges.Total` fue transformada a numérico, gestionando nulos como `0`.
- **Ingeniería de variables**: Creación del campo `Cuentas_Diarias` (cargo diario) para normalizar el análisis de costos.

### 3. Análisis Exploratorio de Datos (EDA)

Se analizaron distribuciones generales, demográficas y de servicios, identificando los siguientes patrones:

- **Antigüedad (Tenure)**: El riesgo de fuga es extremadamente alto durante los primeros 6 meses de permanencia.
- **Cargos mensuales**: Los clientes que cancelan tienden a tener cargos mensuales significativamente más altos.
- **Perfil demográfico**: Los adultos mayores presentan mayor volatilidad; tener pareja o dependientes actúa como factor de estabilidad.
- **Tecnología y método de pago**: La Fibra Óptica y el pago mediante Cheque Electrónico están fuertemente correlacionados con tasas de abandono más altas.

---

## Modelado Predictivo

Se entrenaron y compararon dos algoritmos de clasificación para predecir la fuga de clientes:

| Modelo | Descripción |
|---|---|
| Regresión Logística | Modelo lineal con regularización, optimizado mediante `GridSearchCV` |
| Random Forest | Ensamble de árboles de decisión con pesos de clase balanceados |

### Preparación de Datos para Modelado

- División del dataset: 70% entrenamiento / 30% prueba con estratificación.
- Escalado de variables mediante `StandardScaler`.
- Optimización de hiperparámetros con `GridSearchCV` usando validación cruzada (5 pliegues).

### Tecnologías utilizadas

- **Python 3**
- **Pandas** - Manipulación y análisis de datos
- **NumPy** - Operaciones numéricas
- **Matplotlib / Seaborn** - Visualización de datos
- **Scikit-learn** - Modelado predictivo (Regresión Logística, Random Forest, GridSearchCV, StandardScaler)

---

## Principales Hallazgos

### Variables con Mayor Poder Predictivo

Según el análisis de importancia de variables del modelo Random Forest y los coeficientes de la Regresión Logística:

1. **Tipo de Contrato (`Contract`)**: El contrato mes a mes es el predictor individual más fuerte de fuga. Los clientes con este tipo de contrato tienen mayor flexibilidad para abandonar el servicio.
2. **Antigüedad (`tenure`)**: Existe una relación inversamente proporcional entre permanencia y churn. Los clientes nuevos presentan el mayor riesgo.
3. **Cargos Totales y Mensuales (`Charges.Total` / `Charges.Monthly`)**: Los altos costos mensuales están positivamente correlacionados con la fuga, indicando sensibilidad al precio, especialmente cuando no se perciben servicios de valor agregado.

### Factores Protectores

- Contratos a largo plazo (1 o 2 años).
- Servicios de **Seguridad Online** (`OnlineSecurity`) y **Soporte Técnico** (`TechSupport`).
- Protección de Dispositivos (`DeviceProtection`).

### Desempeño del Modelo

El modelo de Regresión Logística Optimizada alcanzó un **Recall de aproximadamente 74%**, lo que lo hace eficaz para identificar a la mayoría de los clientes en riesgo real de fuga. La precisión se sitúa entre el 50% y 56%, con presencia de falsos positivos; sin embargo, en este contexto el costo de una falsa alarma (ofrecer un beneficio a un cliente fiel) es menor que el costo de perder un cliente definitivamente.

---

## Conclusiones

- Los contratos mes a mes son el principal predictor de abandono, ya que la falta de compromiso a largo plazo facilita la salida ante cualquier insatisfacción.
- Los servicios de valor agregado (seguridad online, soporte técnico, protección de dispositivos) funcionan como "anclas" de retención, reduciendo drásticamente las tasas de churn.
- La fuga se concentra en usuarios con planes de fibra óptica costosos que no perciben un valor proporcional o carecen de servicios de soporte incluidos.

---

## Recomendaciones Estratégicas

### 1. Migración de Contratos - Programa "Pase al Año"
Implementar campañas para transicionar clientes mes a mes a contratos anuales mediante descuentos progresivos (15% de descuento o primeros 2 meses gratuitos). Dirigir la campaña a clientes con menos de 6 meses de antigüedad y contrato mensual.

### 2. Empaquetamiento de Servicios - Paquete "Conexión Segura"
Incluir servicios de Seguridad Online y Soporte Técnico de forma gratuita o subsidiada durante los primeros 3 meses para nuevos usuarios de Fibra Óptica, aumentando el costo de cambio y el valor percibido.

### 3. Programa de Fidelización "Senior Gold"
Crear un canal de atención preferencial y talleres de alfabetización digital para adultos mayores, con bonificaciones por permanencia cada 12 meses.

### 4. Onboarding Temprano
Establecer un programa de seguimiento personalizado durante el primer trimestre, asegurando que los clientes conozcan y utilicen todos los beneficios contratados. Ofrecer auditorías de cuenta para clientes con cargos mensuales en el cuartil superior.

### 5. Incentivos de Pago Automático
Ofrecer créditos en factura por cambiar el método de pago de Cheque Electrónico a métodos automáticos (tarjeta de crédito o transferencia bancaria), reduciendo la fricción mensual asociada al pago manual.

---

## Cómo Ejecutar el Proyecto

1. Clonar el repositorio:
   ```bash
   git clone https://github.com/Benjarveja/desafio-aluratelecomx-parte2.git
   ```

2. Instalar las dependencias necesarias:
   ```bash
   pip install numpy pandas matplotlib seaborn scikit-learn
   ```

3. Abrir el notebook principal:
   ```bash
   jupyter notebook "TelecomX_LATAM(1).ipynb"
   ```

4. Ejecutar las celdas en orden para reproducir el análisis completo.

---

## Autor

Desarrollado como parte del desafío de Data Science - TelecomX LATAM, Alura.
