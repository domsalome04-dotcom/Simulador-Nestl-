# 📦 Sistema Dinámico de Trazabilidad Total e Inventario — Nestlé CD Guayaquil

![Estado](https://img.shields.io/badge/Estado-Prototipo_Final-success)
![Hackathon](https://img.shields.io/badge/Evento-Hackathon_Innolab_Desafío_2-blue)
![Arquitectura](https://img.shields.io/badge/Arquitectura-Híbrida_IoT_&_Drones-purple)

## 📌 Resumen Ejecutivo
Esta propuesta técnica automatiza la gestión de inventario en el Centro de Distribución de Nestlé Guayaquil, una instalación con **24.000 posiciones y racks de hasta 12 metros de altura**. 

En lugar de incurrir en el enorme gasto de etiquetar cada posición o producto con chips RFID desechables, la solución propone una **Arquitectura Híbrida: RFID Reutilizable + Sensores ToF + Enjambre de Drones Multi-función**. El sistema opera de forma inteligente: no vigila todo constantemente a fuerza bruta, sino que utiliza sensores económicos para detectar cambios físicos y despliega drones de auditoría estrictamente bajo demanda, ahorrando batería, tiempo operativo y recursos.

---

## ⚙️ Los 6 Componentes del Sistema

### 1. Control de flujo dinámico (Portales RFID + Tags Reutilizables)
*   **Funcionamiento:** Se instalan portales RFID únicamente en los muelles de entrada y salida.
*   **Ingreso:** El operario asocia un tag RFID reutilizable (tipo tarjeta plástica) al pallet. El portal lee el tag de forma masiva y sin detener al montacargas.
*   **Despacho:** Al salir hacia el camión, el portal registra la salida y el operario retira el tag para devolverlo al stock.
*   **Ventaja:** Mantiene el gasto de consumibles RFID en **$0 dólares**, ya que los mismos tags circulan indefinidamente en lugar de comprar 24.000 chips anuales.

### 2. Sensores ToF (Ubicación y ocupación en tiempo real)
*   **Funcionamiento:** Sensores láser económicos (Time-of-Flight) instalados en las posiciones de alta rotación (Zonas A, B, y C, donde ocurre el 80% de los errores).
*   **Ventaja:** Detectan instantáneamente si un espacio está ocupado o vacío. Si un pallet es retirado sin una orden en SAP, el sensor dispara una alerta inmediata sin esperar a una auditoría manual.

### 3. Cadena de frío y monitoreo de temperatura
*   **Funcionamiento:** Integra los termohidrómetros IoT existentes en las zonas refrigeradas.
*   **Ventaja:** Cuando el dron del enjambre vuela cerca de la zona fría, utiliza su cámara térmica para confirmar visualmente la temperatura de paso (ej. verificando que se mantenga entre −4°C y −6°C), sin requerir vuelos dedicados exclusivamente a esto.

### 4. Enjambre "Charlie": Drones de auditoría bajo demanda
*   **Funcionamiento:** El dron se activa únicamente cuando el WMS acumula **15 eventos de movimiento físico** reportados por los sensores ToF, o cuando ocurre una anomalía (retiro no autorizado).
*   **Doble Sensor:** 
    *   **Sensor de rango (LiDAR):** Mide la distancia exacta al pallet para determinar su ubicación precisa (Rack, Nivel, Posición).
    *   **Cámara RGB:** Lee el código de barras impreso (SSCC) para confirmar el contenido.
*   **Ventaja:** Ahorra hasta un 80% de batería frente al patrullaje constante, elimina el trabajo humano a 12 metros de altura y confirma ubicación y contenido con exactitud.

### 5. Torre de Control (Dashboard / Gemelo Digital)
*   **Funcionamiento:** Pantalla unificada donde los supervisores visualizan la realidad de la bodega de forma remota.
*   **Ventaja:** Incluye un mapa de calor de ocupación por zonas (A-L), disponibilidad en tiempo real, telemetría del enjambre, estado de la cadena de frío y métricas de exactitud (ej. 99.4%).

### 6. Integración total con SAP WMS
*   **Funcionamiento:** Cruza la información física (ToF, Drones, Temperatura) con la información lógica (lo que SAP espera que haya).
*   **Ventaja:** Si el dron confirma un error físico frente a la base de datos, el sistema actualiza SAP de forma autónoma y sin intervención manual.

---

## 🔄 Tabla Comparativa: Proceso Manual vs. Automatizado

| Proceso | Método Manual (Hoy) | Arquitectura Híbrida (Nueva) |
| :--- | :--- | :--- |
| **Recepción** | Escanear código al recibir pallet | Portal RFID lo registra solo al cruzar |
| **Control de posiciones** | Contar físicamente cada posición | Sensores ToF vigilan solos; dron confirma on-demand |
| **Lectura en altura (12m)** | Subir en canasta o bajar el pallet | Dron lee a distancia (LiDAR + RGB), nadie sube |
| **Detección de faltantes** | Descubrir error hasta próximo conteo (días) | Se detecta en minutos/horas por ToF o Dron |
| **Control de temperatura** | Revisión manual o sistema separado | El dron la confirma de paso con cámara térmica |
| **Sincronización** | Verificar contra SAP con planillas | Comparación y actualización automática real-time |

---

## 📈 Inversión, Escalabilidad y Retorno (ROI)

La implementación está diseñada bajo un enfoque modular para reducir el riesgo financiero:
*   **Fase Piloto Modular:** Se automatizan únicamente las zonas de alta rotación (A, B y C) instalando ToF y desplegando a "Charlie", ya que allí ocurren la mayoría de las mermas.
*   **CAPEX (Inversión Inicial):** Fraccionada por fases (portales, sensores piloto, un dron inicial y software).
*   **OPEX (Costos Operativos):** Prácticamente $0 gracias a la circulación indefinida de tags reutilizables.
*   **Fuentes de ROI:** Eliminación del ~2% de pérdida diaria por descuadres, reducción radical de horas-hombre en conteos, eliminación de accidentes en altura y protección continua de inventario refrigerado.

---

## 🎮 Cómo usar el Simulador Interactivo

El repositorio incluye un gemelo digital interactivo renderizado en 3D (`Three.js`) que recrea los procesos físicos del CD Guayaquil. 

Para probar la propuesta en vivo, ingresa al siguiente enlace de despliegue:
👉 **[INSERTA AQUÍ TU ENLACE DE GITHUB PAGES / VERCEL]**

Una vez dentro de la plataforma:
1.  Utiliza el panel inferior para navegar **paso a paso** por la narrativa del proceso (desde el ingreso masivo por RFID hasta la detección de anomalías ToF y el despliegue del dron).
2.  Observa el **Panel de la Torre de Control** (a la derecha) para ver cómo cambian los KPIs (Exactitud, Eventos, Temperatura) en tiempo real conforme avanzan los montacargas y el dron "Charlie" ejecuta la auditoría.
3.  La vista 3D girará y enfocará automáticamente los puntos de interés (ej. cámara térmica cian en cadena de frío, LiDAR verde durante la auditoría).
