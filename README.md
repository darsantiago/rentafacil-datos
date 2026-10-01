# rentafacil-datos

Parámetros tributarios públicos de Colombia que consume la app RentaFácil CO.
No contiene datos de contribuyentes: solo UVT, salario mínimo, tarifa de personas jurídicas y calendario de vencimientos de renta.

La app descarga `datos.json` al abrir y **pregunta antes de aplicar** cualquier cambio.

## Protocolo de actualización (lo sigue la tarea programada)

1. **Fuentes.** Primero la oficial: dian.gov.co (resoluciones, comunicados de prensa, calendario tributario) y el decreto en funcionpublica.gov.co o secretariasenado.gov.co. Como respaldo, transcripciones de Actualícese, Gerencie o consultorcontable.com.
2. **Regla de dos fuentes.** Un dato queda `"verificado": true` solo si lo confirman dos fuentes independientes, al menos una oficial o primaria. Si no, va con `false` y en `nota` o `fuente` se explica qué falta.
3. **Fechas del calendario.** Además se cruzan con el conteo de días hábiles en Colombia, con festivos y Ley Emiliani.
4. **Nunca borrar años anteriores.** Solo se agregan años o se corrigen valores, y cada corrección deja la causa en el mensaje de commit.
5. **Validación.** Antes de publicar: JSON válido, `esquema: 1`, UVT entre 20.000 y 500.000, fechas `AAAA-MM-DD`, rangos de personas naturales 1–100 y dígitos 0–9. La app rechaza archivos que no cumplan.
6. **Commit.** El mensaje dice qué cambió y con qué fuentes; si algo queda sin verificar, empieza por `[POR CONFIRMAR]`.

## Esquema 1

```json
{
  "esquema": 1,
  "generado": "AAAA-MM-DD",
  "app": { "version": "", "mensaje": "", "url": "" },
  "params": [{ "year": 2027, "uvt": 0, "smmlv": 0, "tarifa_pj": 0.35, "verificado": true, "nota": "fuente" }],
  "calendarios": {
    "2026": {
      "pn": [{ "desde": 1, "hasta": 2, "fecha": "2027-08-11" }],
      "pj": [{ "digito": 1, "cuotas": ["2027-05-12", "2027-07-09"] }],
      "grandes": [{ "digito": 1, "cuotas": ["2027-02-10", "2027-04-13", "2027-06-10"] }],
      "verificado": true, "grandes_verificado": true, "fuente": "Dec. ..."
    }
  }
}
```

- `params[].year`: año **de vigencia** de la UVT, que es el año gravable que usa esa UVT.
- `calendarios`: claves por **año gravable**. Las fechas son las del año siguiente, cuando se declara.
- `pn`: los dos últimos dígitos del NIT; "00" equivale a 100.
