# 💊 Dosify

_Recordatorio de medicamentos y suplementos_

**Dosify** es una aplicación Android que ayuda a recordar la toma de medicamentos y suplementos (por ejemplo, creatina) con **alarmas reales**, un **historial verificable de tomas** y un **control de pastillas restantes** que avisa cuándo reclamar un nuevo suministro.

> A diferencia de una alarma normal del celular, aquí queda constancia de si la toma se realizó o no.

---

## 🩺 Problema que resuelve

Muchos pacientes olvidan tomar sus medicamentos, lo que afecta su salud y la efectividad de los tratamientos. Las alarmas comunes suenan, pero no dejan registro: después no se sabe con certeza si la dosis se tomó. Tampoco avisan cuándo se está acabando el medicamento y hay que volver a reclamarlo.

Esta app resuelve ambos puntos con recordatorios automáticos, confirmación de cada toma y un aviso anticipado de reabastecimiento.

---

## ✨ Funcionalidades

- **Medicamentos y suplementos ilimitados**, cada uno con su propia programación.
- **Horarios flexibles**
  - Horas fijas: `08:00, 14:00, 20:00`
  - O "cada X horas" a partir de una hora inicial (ej. cada 8 h desde las 06:00 → 06:00, 14:00 y 22:00).
- **Alarma con sonido de alarma y vibración**, que se repite hasta que respondes la notificación.
- **Confirmación de toma**
  - Botón **"✔ Ya la tomé"** directamente en la notificación.
  - Lista con **checkbox** en la pestaña *Tomas* para marcar o desmarcar manualmente.
- **Historial de tomas** con fecha y hora, y contador de tomadas vs. total.
- **Control de pastillas y plan de reabastecimiento**
  - Al registrar el medicamento indicas para cuántos días lo reclamaste (ej. 25 o 30 días).
  - La app calcula las pastillas según las tomas por día y las pastillas por toma.
  - Descuenta pastillas cada vez que marcas una toma.
  - **Notifica 1 día antes de que se acaben** (con 30 días de suministro, avisa el día 29).
  - Botón **"Reclamé / reabastecer"** para reiniciar el conteo.
- **Las alarmas sobreviven al reinicio** del teléfono.
- **Funciona sin internet**: todos los datos se guardan localmente en el dispositivo.

---

## 📱 Plataforma y tecnologías

| Elemento | Detalle |
|---|---|
| Sistema operativo | Android 8.0+ (minSdk 26, targetSdk 35) |
| Lenguaje | Kotlin |
| Interfaz | Jetpack Compose + Material 3 |
| Base de datos | SQLite (local) |
| Alarmas | `AlarmManager` (alarmas exactas) |
| Notificaciones | `NotificationManager` con canal de alta importancia |
| IDE | Android Studio |
| Build | Gradle 8.9, Android Gradle Plugin 8.7.2, Kotlin 2.0.21 |

---

## 🎨 Diseño de pantallas

```
┌─────────────────────────┐   ┌─────────────────────────┐
│ MEDICAMENTOS            │   │ TOMAS                   │
│                         │   │ Tomadas: 12 de 14       │
│ ┌─────────────────────┐ │   │                         │
│ │ Creatina            │ │   │ ☑ Creatina              │
│ │ Horarios: 08:00     │ │   │   lun 19 oct, 08:00     │
│ │ Quedan 25 (~25 días)│ │   │ ☐ Amoxicilina           │
│ │ [Reclamé] [Eliminar]│ │   │   lun 19 oct, 06:00     │
│ └─────────────────────┘ │   │ ☑ Amoxicilina           │
│                      [+]│   │   dom 18 oct, 22:00     │
├─────────────────────────┤   ├─────────────────────────┤
│ Medicamentos  |  Tomas  │   │ Medicamentos  |  Tomas  │
└─────────────────────────┘   └─────────────────────────┘
```

**Flujo de uso**

1. **Medicamentos** → lista de medicamentos con horarios y pastillas restantes.
2. **Botón +** → formulario: nombre, horarios (o cada X horas), pastillas por toma y días de suministro.
3. **Alarma** → suena en cada horario; se confirma desde la notificación.
4. **Tomas** → historial con checkbox para verificar qué dosis se tomaron.

---

## 🚀 Instalación

### Requisitos

- [Android Studio](https://developer.android.com/studio) (versión reciente, Ladybug o superior recomendada)
- JDK 17 (incluido con Android Studio)
- Un emulador o un celular Android 8.0+ con depuración USB activada

### Pasos

1. Clonar el repositorio:

   ```bash
   git clone https://github.com/JuanxDrock/Recordatorio-medicamentos.git
   ```

2. Abrir la carpeta del proyecto en Android Studio (**File → Open**).
3. Esperar a que Gradle termine de sincronizar.
4. Ejecutar la app con **Run ▶** en un emulador o dispositivo físico.

### Permisos que solicita la app

| Permiso | Para qué sirve |
|---|---|
| `POST_NOTIFICATIONS` | Mostrar los recordatorios (Android 13+; se pide al abrir la app) |
| `USE_EXACT_ALARM` / `SCHEDULE_EXACT_ALARM` | Programar alarmas a la hora exacta |
| `RECEIVE_BOOT_COMPLETED` | Reprogramar las alarmas después de reiniciar el teléfono |

---

## 🗂️ Estructura del proyecto

```
Dosify/
├── build.gradle.kts
├── settings.gradle.kts
├── gradle.properties
└── app/
    ├── build.gradle.kts
    └── src/main/
        ├── AndroidManifest.xml
        └── java/com/medreminder/
            ├── MainActivity.kt   # Interfaz (Compose): medicamentos, tomas, diálogos
            ├── Scheduler.kt      # Alarmas, notificaciones y receivers
            └── Data.kt           # Base de datos SQLite y modelos
```

---

## ⚠️ Notas importantes

- En algunos fabricantes (Xiaomi, Samsung, Huawei, Oppo) conviene **desactivar la optimización de batería** para la app; de lo contrario el sistema puede retrasar o cancelar las alarmas.
- Los datos se almacenan solo en el dispositivo: si se desinstala la app, se pierde el historial.
- El modo "cada X horas" genera los horarios dentro de un día de 24 h; funciona mejor con intervalos que dividen 24 (2, 3, 4, 6, 8, 12 h).
- Esta app es una ayuda de recordatorio y **no reemplaza la indicación de un profesional de la salud**.

---

## 📝 Changelog

- **v0.1** — Versión funcional base: alta de medicamentos con horarios fijos o cada X horas, alarmas con sonido y vibración, confirmación de toma desde la notificación, historial con checkbox, control de pastillas y aviso 1 día antes de que se acaben, reabastecimiento y reprogramación tras reinicio.

## 🛣️ Roadmap

- **v0.2** — Edición de medicamentos existentes, posponer alarma, registro e inicio de sesión de usuarios.
- **v0.3** — Exportación de reportes de adherencia (PDF/CSV).
- **v0.4** — Panel de administrador: gestión de usuarios y reportes de adherencia.
- **v1.0** — Versión estable con todas las funcionalidades planificadas.

---

## 👤 Autor

Desarrollado por [JuanxDrock](https://github.com/JuanxDrock).
