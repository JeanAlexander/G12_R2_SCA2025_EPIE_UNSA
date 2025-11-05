# 🤖 Control Adaptativo de Manipulador Robótico Fanuc CRX-5iA

Este proyecto desarrolla e implementa **controladores avanzados** para el **manipulador colaborativo Fanuc CRX-5iA (6 grados de libertad)**, integrando MATLAB/Simulink con **RoboDK** para simulación en lazo cerrado.

Incluye los siguientes esquemas de control:

- 🧩 **Backstepping**
- ⚙️ **Modos Deslizantes (SMC)**
- 📈 **Control Adaptativo Basado en Modelo (MRAC)**
- 🔄 **Control Híbrido Backstepping + SMC**

---

## 🧠 Objetivo

Diseñar, simular y analizar el comportamiento dinámico de controladores avanzados aplicados al **robot colaborativo CRX-5iA**, considerando su modelo no lineal y la interacción con entornos reales a través de RoboDK.

---

## 🧰 Herramientas utilizadas

| Software | Descripción |
|-----------|--------------|
| **MATLAB/Simulink (R2023b o superior)** | Entorno de diseño y simulación de controladores. |
| **RoboDK** | Simulador 3D y middleware de conexión con robots industriales. |
| **robolink_sfunc (S-Function)** | Bloque para comunicación en tiempo real MATLAB ↔ RoboDK. |

---

## ⚙️ Estructura del proyecto

📦 CRX5iA_Control
│
├── 📁 SimulinkModels/
│ ├── BACKSTEPPINGCRX5iA.slx
│ ├── MRAC_CRX5iA.slx
│ ├── SMC_CRX5iA.slx
│ └── HYBRID_BS_SMC_CRX5iA.slx
│
├── 📁 Controllers/
│ ├── Backstepping_Controller.m
│ ├── SMC_CRX5iA.m
│ ├── MRAC_Controller.m
│ └── Hybrid_BS_SMC.m
│
├── 📁 RoboDK/
│ ├── CRX5iA_RoboDK_Scene.rdk
│ └── robolink_sfunc.mexw64
│
├── 📄 README.md
└── 📄 LICENSE

---

## 🦾 Controladores desarrollados

### 1️⃣ Backstepping
Diseñado a partir de la ecuación dinámica:
\[
M(q)\ddot{q} + C(q,\dot{q})\dot{q} + G(q) = \tau
\]

El controlador se basa en la definición de errores y una velocidad virtual:
\[
\tau = M(q)[\ddot{q}_d - K_1(\dot{q}-\dot{q}_d) - K_2(q-q_d)] + C(q,\dot{q})\dot{q} + G(q)
\]

📂 Archivo: `Backstepping_Controller.m`

---

### 2️⃣ Control por Modos Deslizantes (SMC)
Define la superficie de control:
\[
s = \dot{e} + \Lambda e
\]

Y aplica la ley robusta:
\[
\tau = M(\ddot{q}_r) + C\dot{q}_r + G - K \cdot \text{sat}(s)
\]

📂 Archivo: `SMC_CRX5iA.m`

---

### 3️⃣ Control Adaptativo MRAC
Basado en un modelo de referencia y una ley de adaptación tipo MIT Rule:
\[
\dot{\theta} = -\Gamma Y^T e
\]
\[
\tau = Y(q,\dot{q},q_d,\dot{q}_d)\theta
\]

📂 Archivo: `MRAC_Controller.m`

---

### 4️⃣ Control Híbrido Backstepping + SMC
Combinación jerárquica de ambos métodos:
\[
\tau = \tau_{BS} - K \cdot \text{sat}(s)
\]
Proporciona robustez frente a incertidumbre y estabilidad global.

📂 Archivo: `Hybrid_BS_SMC.m`

---

## 🔗 Integración con RoboDK

El bloque **RoboDK S-Function** actúa como interfaz entre Simulink y el robot:

- **Entradas:** Torque (`tau`)  
- **Salidas:** Posición articular `q` y velocidad `dq` (rad, rad/s)

📘 Importante:
- Todas las **posiciones articulares deben estar en radianes**.
- El modelo RoboDK debe estar configurado con el mismo número de articulaciones (6 DOF).
- Se recomienda una **frecuencia de simulación de 1 kHz (dt = 0.001 s)**.

---

## ▶️ Ejecución

1. Abre **RoboDK** y carga `CRX5iA_RoboDK_Scene.rdk`.  
2. Abre el modelo correspondiente en **Simulink** (`*.slx`).  
3. Conecta RoboDK mediante el bloque `robolink_sfunc`.  
4. Ajusta la trayectoria deseada `qd`, `dqd`, `ddqd` desde bloques *Constant* o *Signal Builder*.  
5. Ejecuta la simulación.

---

## 📊 Resultados esperados

- Seguimiento estable de trayectoria en cada articulación.  
- Convergencia del error de posición a cero.  
- Diferente desempeño entre controladores:
  - Backstepping: buena precisión, pero sensible a incertidumbre.
  - SMC: alta robustez, posible *chattering*.
  - MRAC: adaptación suave a parámetros variables.
  - Híbrido BS+SMC: equilibrio entre estabilidad y robustez.

---

## 🧑‍💻 Autor

**Jean Alexander Arosquipa Ccama**  
Facultad de Ingeniería Electrónica — Universidad Nacional de San Agustín de Arequipa  
📅 2025  
📧 [tu_correo@unsa.edu.pe]  
🔗 [LinkedIn](https://linkedin.com) · [RoboDK](https://robodk.com) · [MATLAB](https://www.mathworks.com)

---

## 🪪 Licencia

Este proyecto se distribuye bajo licencia **MIT**, por lo que puede ser usado libremente con fines educativos y de investigación.

---

## 🌟 Agradecimientos

- **Fanuc Robotics** por el modelo CRX-5iA.  
- **RoboDK** por el simulador y la API de integración.  
- **MathWorks** por MATLAB/Simulink.  
