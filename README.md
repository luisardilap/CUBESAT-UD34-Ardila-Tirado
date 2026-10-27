# CubeSat UD34 — Tarjeta On-Board Computer (OBC)

Diseño electrónico y captura esquemática de la tarjeta de computador de a bordo
del CubeSat UD34, correspondiente a la **Práctica 4** del Laboratorio de Diseño
de Productos Electrónicos.

---

## Identificación

| Campo | Valor |
|---|---|
| **Tarjeta** | On-Board Computer (OBC) — PCB-2 de la arquitectura de la Práctica 3 |
| **Integrantes** | Luis Alberto Ardila Páez · Daniel Esteban Tirado Rojas |
| **Curso** | Laboratorio de Diseño de Productos Electrónicos, 2026-2 |
| **Institución** | Departamento de Ingeniería Electrónica y Telecomunicaciones, Universidad de Antioquia |
| **Versión de Altium Designer** | *[completar: versión exacta utilizada]* |
| **Etiqueta de esta entrega** | `entrega-practica-cuatro` |

### Resumen técnico

| Parámetro | Valor |
|---|---|
| Microcontrolador | STM32L496ZGT6 (ARM Cortex-M4 con FPU, LQFP144) |
| Hojas esquemáticas | 13 (1 principal + 12 subordinadas) |
| Componentes | 125 unidades en 38 líneas de BOM |
| Costo de componentes | USD 87,98 por tarjeta a precio unitario |
| Interfaz con el satélite | Dos conectores PC/104 de 52 contactos (`H1`, `H2`) |

---

## Contenido del repositorio

```
/
├── README.md                       Este archivo
├── Altium/                         Proyecto nativo
│   ├── CubeSatUD3A_Ardila-Tirado.PrjPcb
│   ├── *.SchDoc                    13 hojas esquemáticas
│   ├── *.Harness                   Definiciones de arnés
│   ├── ActiveBOM.BomDoc            Lista de materiales gestionada
│   └── OBC.PcbDoc                  Documento de PCB (vacío en esta entrega)
├── Librerias/                      Librerías propias editables
│   ├── *.SchLib                    Símbolos
│   └── *.PcbLib                    Huellas
├── Datasheets/                     Hojas de datos y notas de aplicación
├── Calculos/
│   └── Calculos_Practica4_OBC.xlsx Matriz de restricciones, cálculos y costos
├── Entregables/
│   ├── Informe_Practica4.pdf       Informe técnico
│   ├── Esquematicos_OBC.pdf        13 hojas exportadas
│   ├── BOM_OBC.xlsx                BOM exportada
│   └── Validacion_ERC.png          Evidencia de la verificación eléctrica
└── Informe/                        Fuentes LaTeX del informe
    ├── informe_practica4.tex
    └── figures/
```

---

## Cómo abrir el proyecto

### Requisitos

- Altium Designer *[completar: versión]* o posterior.
- **Sesión iniciada en la cuenta de Altium.** Varios componentes provienen del
  catálogo en la nube (sus referencias de librería empiezan por `CMP-`), por lo
  que sus huellas solo se resuelven con conexión. Sin sesión activa, el proyecto
  abre pero los componentes aparecen sin huella asignada.

### Pasos

1. Clonar el repositorio:
   ```
   git clone https://github.com/luisardilap/CUBESAT-UD34-Ardila-Tirado.git
   ```
2. Abrir `Altium/CubeSatUD3A_Ardila-Tirado.PrjPcb` desde
   **File → Open Project**.
3. Verificar en el panel **Projects** que el proyecto quede como activo
   (en negrita).
4. Añadir la carpeta `Librerias/` en
   **Project → Project Options → Search Path Libraries**, para que el proyecto
   resuelva sus dependencias en cualquier equipo.
5. Abrir `Top.SchDoc` para ver la jerarquía completa. Doble clic sobre
   cualquier bloque entra a esa hoja; el botón de subir jerarquía regresa.

### Verificar el diseño

**Project → Validate PCB Project.** El resultado esperado se describe en la
sección siguiente.

### Exportar los entregables

- Esquemáticos en PDF: **File → Smart PDF**, alcance *Current Project*,
  calidad vectorial.
- BOM: abrir `ActiveBOM.BomDoc` y exportar a Excel. Requiere conexión para
  actualizar los precios de proveedor.

---

## Estado del diseño

### Verificación eléctrica

La validación reporta **20 mensajes de error**, ninguno de los cuales indica una
conexión incorrecta. Se clasifican así:

| Cantidad | Mensaje | Naturaleza |
|---|---|---|
| 17 | *Net has only one pin* | Recursos reservados y dos interfaces pendientes de llevar al conector |
| 2 | *Net contains multiple Output Pins* | Búferes de tres estados en las líneas MISO: excepción justificada |
| 1 | *Input Port / Output Sheet Entry* | Inconsistencia de declaración de dirección en `nRESET` |

El análisis completo está en la sección de verificación del informe técnico.

### Recursos reservados

El diseño deja deliberadamente disponibles 16 líneas de propósito general hacia
el conector de pila, 9 GPIO adicionales, 12 entradas de ADC y 2 UART. Cada línea
de `PC104_GPIO` admite varias funciones alternas del microcontrolador, de modo
que puede reconfigurarse por software sin rehacer la PCB.

### Pendientes conocidos

- [ ] Reducir `R14` a 6,79 kΩ con tolerancia del 1 % (periodo del watchdog)
- [ ] Sustituir `RC1608F60R4CS`, reportado en estado *End of Life*
- [ ] Completar descripciones vacías o genéricas en 8 líneas de la BOM
- [ ] Llevar `ADC1`/`ADC3` (interfaz IF-02) e `I2C1` (IF-05) al conector de pila
- [ ] Añadir el conector de cámara (IF-06)
- [ ] Coordinar con la pareja de la PCB-4 la reasignación del buffer de imagen
- [ ] Cerrar las 12 cotizaciones con precio estimado
- [ ] Confirmar proveedor de PCB y fecha de consulta

---

## Trazabilidad con la Práctica 3

Cinco especificaciones se revisaron con fundamento numérico y quedan
documentadas en el informe: **ET-12** (frecuencia del procesador), **ET-09**
(memoria de trabajo), **ET-10** (almacenamiento masivo), **ET-34** (exactitud
del reloj) y **RNF-15** (integridad de memoria).

El informe explica, para cada una, el cálculo que sustenta la revisión y el
efecto sobre el resto del sistema.

---

## Historial de versiones

| Etiqueta | Fecha | Contenido |
|---|---|---|
| `entrega-practica-cuatro` | *[completar: fecha]* | Diseño esquemático completo, BOM con precios, matriz de restricciones y análisis de costos |
