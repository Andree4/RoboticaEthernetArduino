# 🤖 RoboticaEthernetArduino

**Control remoto de dispositivos robóticos mediante Arduino y interfaz web**

Este proyecto implementa un sistema de control robótico completo que permite gestionar remotamente múltiples componentes electrónicos a través de una interfaz web moderna y intuitiva. Utilizando un Arduino equipado con Ethernet Shield, el sistema actúa como servidor HTTP proporcionando control en tiempo real de relés, LEDs, motores paso a paso y monitoreo de sensores ambientales.

## 🌟 Características Principales

- **🌐 Control Web Remoto**: Interfaz web responsive para control desde cualquier dispositivo
- **🔌 Conectividad Ethernet**: Comunicación estable y confiable mediante shield W5100
- **🎛️ Control Múltiple**: Gestión simultánea de relés, LEDs y motores
- **🌡️ Monitoreo Ambiental**: Lectura en tiempo real de temperatura y humedad
- **⚙️ Motor Paso a Paso**: Control preciso de posicionamiento angular (0° a 360°)
- **📊 Visualización**: Gráfico tipo reloj para mostrar la posición del motor
- **🔄 Actualización Automática**: Datos de sensores actualizados cada segundo

## 📦 Estructura del Proyecto

```
📁 RoboticaEthernetArduino/
├── 📄 index.html          # Interfaz web principal
├── 📄 arduino.ino         # Código fuente para Arduino
├── 📄 README.md           # Documentación del proyecto
├── 📁 assets/             # Recursos adicionales (opcional)
│   ├── 🖼️ images/         # Imágenes del proyecto
│   └── 🎨 styles/         # Estilos CSS adicionales
└── 📁 docs/               # Documentación técnica detallada
    ├── 📄 wiring.md       # Guía de conexiones
    └── 📄 api.md          # Documentación de la API
```

## 🧰 Lista de Componentes

### Componentes Principales

| Componente | Cantidad | Especificaciones | Precio Aprox. |
|------------|----------|------------------|---------------|
| **Arduino UNO R3** | 1 | Microcontrolador ATmega328P | $15-25 USD |
| **Ethernet Shield W5100** | 1 | Compatible con Arduino UNO | $10-20 USD |
| **Módulo Relé 5V** | 1 | 1 canal, corriente máx. 10A | $3-5 USD |
| **Motor Paso a Paso 28BYJ-48** | 1 | 5V, 64 pasos/revolución | $5-8 USD |
| **Driver ULN2003** | 1 | Para control del motor | $2-3 USD |
| **Sensor DHT11** | 1 | Temperatura y humedad | $3-5 USD |

### Componentes Adicionales

| Componente | Cantidad | Especificaciones |
|------------|----------|------------------|
| LED (cualquier color) | 1 | 5mm, 3.3V |
| Resistencia 220Ω | 1 | Para limitación de corriente del LED |
| Protoboard | 1 | 830 puntos de conexión |
| Cables Dupont | 20+ | Macho-macho, macho-hembra |
| Cable Ethernet | 1 | Para conexión de red |
| Fuente de alimentación | 1 | 9V-12V, 1A (para Arduino) |

## 🔌 Diagrama de Conexiones

### Tabla de Conexiones Detallada

| Pin Arduino | Componente | Pin/Terminal | Descripción |
|-------------|------------|--------------|-------------|
| **Digital 2** | Driver ULN2003 | IN1 | Control motor - Bobina A |
| **Digital 3** | Driver ULN2003 | IN2 | Control motor - Bobina B |
| **Digital 4** | Driver ULN2003 | IN3 | Control motor - Bobina C |
| **Digital 5** | Driver ULN2003 | IN4 | Control motor - Bobina D |
| **Digital 6** | Módulo Relé | IN | Señal de control del relé |
| **Digital 7** | Sensor DHT11 | DATA | Lectura de datos del sensor |
| **Digital 13** | LED | Ánodo (+) | Control de LED (con resistencia) |
| **5V** | Múltiples | VCC/+ | Alimentación positiva |
| **GND** | Múltiples | GND/- | Tierra común |

### ⚠️ Notas Importantes de Conexión

- **Alimentación Externa**: El motor paso a paso requiere alimentación externa de 5V para funcionamiento óptimo
- **Tierra Común**: Conectar todas las tierras (GND) en un punto común
- **Resistencia LED**: Usar resistencia de 220Ω en serie con el LED para evitar daños
- **Ethernet Shield**: Asegurar conexión correcta con pines SPI (10, 11, 12, 13)

## 🌐 Interfaz Web - Características

### Diseño y Funcionalidad

La interfaz web ha sido desarrollada con tecnologías modernas para proporcionar una experiencia de usuario fluida:

#### **Tecnologías Utilizadas**
- **HTML5**: Estructura semántica y moderna
- **CSS3**: Estilos responsivos con Flexbox/Grid
- **JavaScript ES6+**: Lógica de control y comunicación asíncrona
- **Fetch API**: Comunicación HTTP con el Arduino

#### **Controles Disponibles**

1. **🔴 Control de Relé**
   - Botón toggle para activar/desactivar
   - Indicador visual del estado actual
   - Ideal para control de dispositivos de alta potencia

2. **💡 Control de LED**
   - Interruptor visual intuitivo
   - Estado reflejado en tiempo real
   - Perfecto para indicadores luminosos

3. **⚡ Control Simultáneo**
   - Activación conjunta de relé y LED
   - Útil para rutinas de activación múltiple

4. **🎯 Control de Motor Paso a Paso**
   - Selector de ángulo (0° a 360°)
   - Visualización gráfica tipo reloj
   - Control preciso de posicionamiento

5. **🌡️ Monitor Ambiental**
   - Temperatura en tiempo real (°C)
   - Humedad relativa (%)
   - Actualización automática cada segundo

## 🔗 API REST del Arduino

El Arduino implementa un servidor HTTP que expone los siguientes endpoints:

### Endpoints de Control

#### **POST /rele**
Alterna el estado del relé
```http
POST http://192.168.199.214/rele
```
**Respuesta:**
```json
{
  "status": "success",
  "device": "rele",
  "state": "ON"
}
```

#### **POST /led**
Controla el estado del LED
```http
POST http://192.168.199.214/led
```

#### **POST /ambos**
Activa simultáneamente relé y LED
```http
POST http://192.168.199.214/ambos
```

### Endpoints de Lectura

#### **GET /sensor**
Obtiene datos del sensor DHT11
```http
GET http://192.168.199.214/sensor
```
**Respuesta:**
```json
{
  "success": true,
  "temperature": "24.5",
  "humidity": "62.8",
  "timestamp": "2024-05-23T10:30:00Z"
}
```

#### **GET /motor/{angulo}**
Posiciona el motor en el ángulo especificado
```http
GET http://192.168.199.214/motor/180
```
**Parámetros:**
- `angulo`: Valor entre 0 y 360 grados

## ⚙️ Guía de Instalación y Configuración

### Paso 1: Preparación del Hardware

1. **Ensamblaje del Circuito**
   ```bash
   # Verificar conexiones según tabla de pines
   # Usar multímetro para verificar continuidad
   # Probar alimentación antes de conectar Arduino
   ```

2. **Instalación del Ethernet Shield**
   - Apilar cuidadosamente sobre el Arduino
   - Verificar alineación correcta de pines
   - Conectar cable Ethernet al router/switch

### Paso 2: Configuración del Software

1. **Preparación del IDE de Arduino**
   ```bash
   # Instalar librerías requeridas:
   # - DHT sensor library
   # - Ethernet library (incluida por defecto)
   # - Stepper library (incluida por defecto)
   ```

2. **Configuración de Red**
   ```cpp
   // En el archivo arduino.ino, modificar:
   IPAddress ip(192, 168, 1, 214);        // IP del Arduino
   IPAddress gateway(192, 168, 1, 1);     // Gateway de tu red
   IPAddress subnet(255, 255, 255, 0);    // Máscara de subred
   ```

3. **Carga del Código**
   ```bash
   # 1. Abrir arduino.ino en el IDE
   # 2. Seleccionar puerto y placa correctos
   # 3. Compilar y subir el código
   # 4. Verificar en monitor serie la inicialización
   ```

### Paso 3: Configuración de la Interfaz Web

1. **Modificar IP en el HTML**
   ```javascript
   // En index.html, actualizar:
   const ARDUINO_IP = "http://192.168.1.214";
   ```

2. **Servir el Archivo HTML**
   ```bash
   # Opción 1: Abrir directamente en navegador
   # Opción 2: Usar servidor local
   python -m http.server 8000
   # Luego visitar: http://localhost:8000
   ```

## 📊 Especificaciones Técnicas

### Motor Paso a Paso
- **Modelo**: 28BYJ-48
- **Voltaje**: 5V DC
- **Pasos por revolución**: 2048 (con reductor interno)
- **Ángulo por paso**: 0.176°
- **Precisión**: ±3% 
- **Torque**: 34 mN⋅m aproximadamente

### Sensor DHT11
- **Rango de temperatura**: 0°C a 50°C (±2°C precisión)
- **Rango de humedad**: 20% a 90% RH (±5% precisión)
- **Tiempo de respuesta**: 2 segundos
- **Frecuencia de muestreo**: 1 Hz máximo

### Conectividad de Red
- **Protocolo**: HTTP/1.1
- **Puerto**: 80 (configurable)
- **Velocidad**: 10/100 Mbps (dependiente del shield)
- **Latencia típica**: <50ms en LAN

## 💡 Ejemplos de Uso Prácticos

### Caso 1: Sistema de Riego Automatizado
```javascript
// Activar bomba de agua mediante relé
fetch(`${ARDUINO_IP}/rele`, { method: 'POST' })
  .then(() => console.log('Riego activado'));

// Monitorear humedad ambiental
setInterval(async () => {
  const data = await fetch(`${ARDUINO_IP}/sensor`).then(r => r.json());
  if (data.humidity < 30) {
    // Activar riego si humedad es baja
    activarRiego();
  }
}, 60000); // Cada minuto
```

### Caso 2: Seguidor Solar
```javascript
// Posicionar panel solar según hora del día
function posicionarPanel() {
  const hora = new Date().getHours();
  const angulo = (hora - 6) * 15; // 15° por hora desde las 6 AM
  
  if (angulo >= 0 && angulo <= 180) {
    fetch(`${ARDUINO_IP}/motor/${angulo}`);
  }
}
```

### Caso 3: Sistema de Monitoreo
```javascript
// Dashboard en tiempo real
function actualizarDashboard() {
  fetch(`${ARDUINO_IP}/sensor`)
    .then(r => r.json())
    .then(data => {
      document.getElementById('temp').textContent = data.temperature + '°C';
      document.getElementById('hum').textContent = data.humidity + '%';
      
      // Alertas automáticas
      if (parseFloat(data.temperature) > 30) {
        activarVentilacion();
      }
    });
}
```

## 🚀 Extensiones y Mejoras Futuras

### Hardware
- [ ] **Sensores adicionales**: Presión atmosférica, luz ambiente, pH
- [ ] **Actuadores**: Servomotores, motores DC con encoders
- [ ] **Comunicación inalámbrica**: Módulo WiFi ESP8266/ESP32
- [ ] **Alimentación**: Panel solar con batería de respaldo

### Software
- [ ] **Base de datos**: Logging histórico de datos de sensores
- [ ] **Notificaciones**: Email/SMS para alertas críticas
- [ ] **API REST completa**: CRUD operations para configuraciones
- [ ] **Interfaz móvil**: App nativa para Android/iOS
- [ ] **Dashboard avanzado**: Gráficos históricos, análisis de tendencias

## 🛠️ Solución de Problemas Comunes

### Problema: Arduino no responde via Ethernet
```bash
# Verificaciones:
1. Cable Ethernet conectado correctamente
2. LED del shield Ethernet parpadeando
3. IP configurada en rango de red local
4. Ping al Arduino: ping 192.168.1.214
5. Monitor serie mostrando inicialización exitosa
```

### Problema: Motor paso a paso no se mueve
```bash
# Posibles causas:
1. Conexiones sueltas en driver ULN2003
2. Alimentación insuficiente (usar fuente externa 5V)
3. Secuencia de pasos incorrecta en código
4. Motor defectuoso (probar con código de prueba)
```

### Problema: Lecturas incorrectas del DHT11
```bash
# Soluciones:
1. Verificar conexión del pin DATA
2. Agregar resistencia pull-up de 10kΩ
3. Aumentar delay entre lecturas
4. Verificar librería DHT instalada correctamente
```

## 🤝 Contribuciones

¡Las contribuciones son bienvenidas! Por favor:

1. **Fork** el repositorio
2. **Crea** una rama para tu feature (`git checkout -b feature/nueva-funcionalidad`)
3. **Commit** tus cambios (`git commit -am 'Agregar nueva funcionalidad'`)
4. **Push** a la rama (`git push origin feature/nueva-funcionalidad`)
5. **Crea** un Pull Request

### Áreas de Contribución
- 🐛 **Bug fixes**: Corrección de errores
- ✨ **Features**: Nuevas funcionalidades
- 📚 **Documentación**: Mejoras en documentación
- 🎨 **UI/UX**: Mejoras en interfaz de usuario
- ⚡ **Performance**: Optimizaciones de rendimiento

## 📄 Licencia

Este proyecto está bajo la Licencia MIT. Ver el archivo `LICENSE` para más detalles.

```
MIT License

Copyright (c) 2024 RoboticaEthernetArduino

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software...
```

## 👨‍💻 Autor

**Tu Nombre**
- GitHub: [@tuusuario](https://github.com/tuusuario)
- Email: tu.email@ejemplo.com
- LinkedIn: [Tu Perfil](https://linkedin.com/in/tuperfil)

## 🙏 Agradecimientos

- Comunidad Arduino por las librerías y ejemplos
- Contribuidores del proyecto Ethernet Shield
- Desarrolladores de las librerías DHT y Stepper
- Beta testers y colaboradores del proyecto

---

⭐ **¡Si este proyecto te fue útil, no olvides darle una estrella!** ⭐

📧 **¿Preguntas o sugerencias?** Abre un [issue](https://github.com/tuusuario/RoboticaEthernetArduino/issues) o envía un pull request.
