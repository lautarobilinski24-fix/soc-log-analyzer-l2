# SOC Log Analyzer L2

Aplicación web para el **Reto Práctico · Analista SOC Nivel 2**. Importa el Excel exportado del SIEM del cliente, detecta y correlaciona eventos sospechosos, los clasifica por criticidad y genera el informe técnico en PDF, con IA integrada.

Es un solo archivo, `index.html`, sin servidor ni instalación. El Excel se procesa en el navegador.

## Cómo usarla

1. Abrí `index.html` en el navegador (Chrome, Edge o Firefox) o publicala (ver más abajo).
2. **01 · Carga**: arrastrá el `.xlsx` o tocá *Usar archivo del caso*.
3. Recorré las pestañas **02 Análisis → 03 Dashboard → 04 Riesgos → 05 Informe → 06 Exportar**.
4. **IA**: tocá el indicador "IA" arriba a la derecha, elegí el proveedor y pegá la API key (Gemini tiene clave gratuita en aistudio.google.com). Después usá **Revisar análisis con IA** (pestaña 02) y **Redactar con IA** (pestaña 05).

## Publicarla (Vercel o GitHub Pages)

- **Vercel**: creá un repo con `index.html`, después en vercel.com elegí *Add New → Project*, importá el repo y tocá *Deploy*.
- **GitHub Pages**: en el repo, *Settings → Pages → Deploy from branch (main / root)*.

## Cómo cubre la consigna

| Requisito | Dónde |
|---|---|
| 01 Carga de archivos (.xlsx) | Pestaña 01: lector .xlsx nativo y tabla con búsqueda y filtros |
| 02 Análisis de seguridad y correlación | Pestaña 02: 6 categorías de detección; los hallazgos se agrupan en incidentes por IP o cuenta compartida (ventana de 90 min) |
| 03 Dashboard interactivo por criticidad | Pestaña 03: filtros por criticidad, tooltips y clic en los gráficos para filtrar |
| 04 Clasificación Crítico/Alto/Medio/Bajo con justificación | Pestaña 04: criterios explícitos y justificación de cada incidente |
| 05 Informe técnico | Pestaña 05: las 8 secciones pedidas, más anexos de IoC, MITRE, higiene y metodología |
| 06 Exportación PDF | Pestaña 06 |
| IA integrada | Botones *Revisar análisis con IA* (02) y *Redactar con IA* (05) |
| Sin Sentinel | No usa Sentinel |
| Rigor analítico | Cada hallazgo cita sus Log_ID. Lo que devuelve la IA se compara con los Log_ID del Excel y se marcan los que no existen |

## Qué detecta (6 categorías)

- **Fuerza bruta**: ráfagas de 4625 por IP de origen y password spraying (muchas cuentas desde una misma IP).
- **Inicios sospechosos**: 4624 exitoso después de una fuerza bruta, desde una IP externa o fuera de horario.
- **Escalamiento**: 4672 en sesiones comprometidas o externas y cambios de grupos privilegiados.
- **Comandos y procesos**: PowerShell `-EncodedCommand`, `vssadmin delete shadows`, compresión (7z/tar), reconocimiento (`net view`, `Get-SmbShare`) y shells lanzadas por IIS.
- **Movimiento lateral**: logons de red (LogonType 3, `source=`) de una cuenta comprometida hacia otros servidores.
- **Otras anomalías**: exfiltración por proxy (bytes_out), conexiones salientes de PowerShell, acceso masivo a archivos, tareas y servicios de persistencia, beaconing, web shell y sondeo web, más observaciones de higiene.
