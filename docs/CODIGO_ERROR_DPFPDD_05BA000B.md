# Documentación Técnica del Error DPFPDD_E_FAILURE (0x05BA000B / 96075787)

Este documento resume el error devuelto por `dpfpdd_query_devices()` de la librería nativa de DigitalPersona. Consulta la documentación del SDK instalado por un canal autorizado para la referencia completa.

---

## 1. Identificación y Cálculo del Código de Error

* **Valor Decimal:** `96075787`
* **Valor Hexadecimal:** `0x05BA000B`
* **Nombre de la Constante:** `DPFPDD_E_FAILURE`

1. `_DP_FACILITY` = `0x05BA`
2. `_DP_FACILITY << 16` = `0x05BA0000`
3. `DPERROR(0x0b)` = `0x000B | 0x05BA0000` = **`0x05BA000B`**
4. En base 10 (decimal): `(5 × 16^7) + (11 × 16^6) + (10 × 16^5) + 11` = **`96075787`**

---

## 2. Significado

`DPFPDD_E_FAILURE` representa un fallo genérico no especificado. La API puede devolverlo cuando no logra enumerar los lectores y no hay una causa más concreta disponible.

---

## 3. Causa y Diagnóstico Práctico en un PC Nuevo

El código `DPFPDD_E_FAILURE` devuelto por `dpfpdd_query_devices()` indica un **"Fallo inesperado / no especificado"** al interactuar con el subsistema USB o el driver en modo kernel.

Aunque `dpfpdd_init()` retorne `OK` (ya que solo carga la DLL en la memoria del proceso), `dpfpdd_query_devices()` falla por alguna de las siguientes causas en un equipo recién configurado:

1. **Driver incorrecto (Windows Update):**
   * Windows 10/11 suele instalar un controlador genérico por omisión o un driver WBF (*Windows Biometric Framework*).
   * La API de bajo nivel `dpfpdd.dll` requiere el paquete de controladores nativos **DigitalPersona RTE (Run-Time Environment)** o **U.are.U SDK Driver**.
2. **Conflicto con el Servicio `DpHost`:**
   * Si el servicio `DpHost` (*DigitalPersona Authentication Service*) se encuentra activo, bloquea la comunicación con el dispositivo para uso exclusivo de inicio de sesión de Windows.
   * **Comprobación:** Abrir `services.msc` y verificar si el servicio `DigitalPersona` o `DpHost` está en ejecución.
3. **Puerto USB / Suspensión selectiva:**
   * Probar conectando el lector a un puerto **USB 2.0 directo** (evitar HUBs o puertos USB 3.0/3.2 con ahorro de energía agresivo en BIOS).
