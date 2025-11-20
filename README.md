# Optuna-SEIQR
🦠 Modelo Estocástico SEIQR: Dinámica COVID-19 en Argentina

Simulación computacional basada en agentes para la propagación viral considerando vectores de movilidad variables y estocasticidad.

📋 Resumen del Proyecto

Este proyecto implementa un modelo epidemiológico espacial y estocástico (SEIQR) para simular la propagación del COVID-19 en Argentina a lo largo de 600 días. A diferencia de los modelos deterministas tradicionales (SIR/SEIR), este enfoque incorpora:

Estocasticidad: Uso del método de Monte Carlo (100 promedios) para capturar la variabilidad de los brotes.

Movilidad Dinámica: Vectores de control KT (ruido) y nu (movilidad interurbana) que cambian en ventanas de 20 días.

Geografía Real: Input basado en densidad poblacional (densK7_Arg.csv) y redes de rutas (mob275-523_Arg.csv).

⚙️ Dinámica del Modelo

El sistema evoluciona a través de 5 estados principales para cada celda de la grilla geográfica:

Estado

Descripción

Parámetro Asociado

S

Susceptible

Población inicial ($1-\eta$)

E

Expuesto (Latente)

Periodo de latencia ($\epsilon = 1$ día)

I

Infectado

Periodo infeccioso ($\sigma = 14$ días)

Q

Cuarentena

Tasa de aislamiento dependiente del tiempo ($p$)

R

Recuperado

Inmunidad natural ($\Omega = 140$ días)

Ecuaciones de Transmisión

La incidencia $G(t)$ se calcula localmente considerando la densidad de población $\rho$ y una matriz estocástica $U$:

$$ G(t) = U_{i,j} \cdot \rho_{i,j} \cdot (1 - e^{-\beta I_{i,j}}) \cdot S_{i,j} $$

🚦 Vectores de Movilidad y Control

Uno de los aportes clave de este código es la modulación temporal de la movilidad, representada por dos arrays de 30 ventanas (cada una de 20 días).

🔵 KT (rui): Controla la aparición estocástica de nuevos focos ("ruido").

🔴 $\nu$ (Movilidad): Controla la probabilidad de contagio a larga distancia entre nodos conectados.

Fig 1. Evolución temporal de los parámetros de control. Se observa cómo la movilidad y el ruido fluctúan, simulando aperturas y cierres de cuarentenas.

📊 Resultados y Validación

El modelo fue contrastado con datos históricos de casos reportados en Argentina (Covid19arData).

Metodología de Validación

Se ejecutaron 100 simulaciones (Monte Carlo).

Se obtuvo la serie temporal promedio de nuevos casos (yym).

Se aplicó un filtro Savitzky-Golay para suavizar el ruido estocástico.

Se comparó con la curva real.

Fig 2. La línea naranja representa la simulación del modelo (con sombra de desviación estándar) vs. las barras azules de datos oficiales.

💻 Estructura del Código

El script principal simulacion_seiqr.py está optimizado con NumPy para manejar matrices de gran tamaño ($Q \times P \times T$).

# Snippet: Cálculo del movimiento a celdas vecinas
filt5 = (ro > 0) & (II[:,:,t] >= eta) & (S[:,:,t] <= 1-eta)
auxil = np.zeros((Q,P))

# Propagación a vecinos (Norte, Sur, Este, Oeste)
auxil[0:Q-1, :] = filt6[1:Q, :]  # ... lógica matricial ...
auxil[auxil > 1] = 1

# Aplicación de estocasticidad local
filt7 = (II[:,:,t] <= 0) & (S[:,:,t+1] >= eta) & (auxil3)
II[filt7, t+1] = eta  # Nuevo infectado local


🚀 Instalación y Uso

Clonar el repositorio:

git clone [https://github.com/tu-usuario/covid19-seiqr-model.git](https://github.com/tu-usuario/covid19-seiqr-model.git)


Instalar dependencias:

pip install numpy pandas matplotlib scipy


Ejecutar la simulación:

python simulacion_seiqr.py


Esto generará los archivos .mat con los resultados y las gráficas .png.

📄 Licencia

Este proyecto se distribuye bajo la licencia MIT. Si utilizas este código para investigación, por favor cita este repositorio.
