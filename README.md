# Mitre & Detections Rules Framework

Un framework para definir, gestionar y desplegar reglas de detección alineadas con el marco MITRE ATT&CK. Este proyecto facilita la creación de reglas reproducibles, su mapeo a técnicas y tácticas MITRE, pruebas automatizadas y exportación a sistemas de detección (SIEM, EDR, SOAR).

## Tabla de contenidos
- [Resumen](#resumen)
- [Características principales](#características-principales)
- [Cómo funciona](#cómo-funciona)
- [Formato de las reglas](#formato-de-las-reglas)
- [Quickstart](#quickstart)
- [Ejemplo de regla](#ejemplo-de-regla)
- [Mapeo MITRE ATT&CK](#mapeo-mitre-attack)
- [Testing y CI](#testing-y-ci)
- [Contribuir](#contribuir)
- [Licencia y contacto](#licencia-y-contacto)

## Resumen
El Mitre & Detections Rules Framework proporciona una estructura común para describir reglas de detección, metadata asociada (severidad, plataformas, referencias), y su mapeo a MITRE ATT&CK. Permite validar reglas, ejecutar pruebas unitarias y convertir/exportar reglas a múltiples formatos de destino.

## Características principales
- Definición de reglas en YAML/JSON con metadata estandarizada.
- Mapeo directo a tactics/techniques de MITRE ATT&CK.
- Validación y pruebas automatizadas de reglas.
- Generadores/convertidores para exportar a SIEMs/EDRs comunes.
- Plantillas y ejemplos para acelerar la creación de reglas.
- Control de versiones y trazabilidad de cambios por regla.

## Cómo funciona
1. Escribir reglas en el formato canónico del repositorio (YAML/JSON).
2. Añadir metadata obligatoria: id, nombre, descripción, severidad, plataformas, y mapeo MITRE.
3. Ejecutar validadores para comprobar sintaxis y consistencia.
4. Ejecutar pruebas que simulan eventos/telemetría para verificar comportamiento.
5. Exportar reglas a los formatos soportados por tu stack (p. ej., reglas Sigma, detecciones para EDR X).
6. Desplegar en pipelines CI/CD para aplicación automática.

## Formato de las reglas
Cada regla debe incluir campos mínimos:
- id: Identificador único (p. ej., `RA-0001`)
- title: Título legible
- description: Explicación de la detección
- severity: low | medium | high | critical
- platforms: [windows, linux, macos, network, cloud]
- detection: condiciones, consultas o patrones
- mitre:
  - tactic: TXXXX
  - technique: TXXXX
  - subtechnique: TXXXX.X (opcional)
- references: URLs o artículos
- author: Nombre o equipo
- tags: Lista de etiquetas

El repositorio incluye un validador que garantiza la presencia y el formato correctos de estos campos antes de aceptar o exportar una regla.

## Quickstart
Requisitos:
- Git
- Python 3.8+ (u otra runtime según herramientas incluidas)
- Virtualenv (opcional)

Pasos básicos:
1. Clona el repositorio:
   git clone https://github.com/cyberauditor-framework/Mitre-Detections-Rules-Framework.git
2. Crea un entorno virtual e instala dependencias:
   python -m venv .venv
   source .venv/bin/activate
   pip install -r requirements.txt
3. Validar una regla:
   ./tools/validate_rule.py rules/mi_regla.yml
4. Ejecutar tests:
   pytest tests/
5. Exportar reglas a un formato objetivo:
   ./tools/export.py --format sigma --output exports/sigma/

(Ajusta los nombres de scripts/paths según la estructura real del repositorio.)

## Ejemplo de regla
```yaml
# Ejemplo mínimo de regla (YAML)
id: RA-0001
title: Detección de ejecución sospechosa de PowerShell
description: Detecta ejecución de PowerShell con parámetros encodificados o inyección de comandos.
severity: high
platforms:
  - windows
detection:
  type: query
  query: 'ProcessCommandLine contains "powershell" and (ProcessCommandLine contains "-EncodedCommand" or ProcessCommandLine contains "-e")'
mitre:
  tactic: TA0002
  technique: T1059
references:
  - https://attack.mitre.org/techniques/T1059/
author: Equipo de Detecciones
tags:
  - powershell
  - execution
```

## Mapeo MITRE ATT&CK
Este framework exige que cada regla incluya un mapeo MITRE mínimo (táctica y técnica). Esto permite:
- Agrupar y priorizar reglas por tácticas relevantes.
- Generar matrices ATT&CK con cobertura de detección.
- Facilitar reportes de brechas y evolución de la cobertura.

Se recomienda mantener una carpeta con archivos de mapeo y scripts que generen métricas de cobertura.

## Testing y CI
- Validadores unitarios verifican la sintaxis y la coherencia de la metadata.
- Tests de detección emplean datasets sintéticos o replay de telemetría para verificar que la regla se dispara cuando corresponde.
- Integración en CI: cada PR ejecuta validación y tests; las reglas que fallan no se permiten fusionar.
- Exports automáticos en pipelines permiten publicar artefactos para despliegue.

## Contribuir
1. Abre un issue para proponer nuevas reglas o cambios en el formato.
2. Crea una rama para tu contribución.
3. Incluye tests que demuestren la efectividad de la regla.
4. Sigue las convenciones de commit y el template de PR del repositorio.

Para detalles sobre el formato y validaciones, revisa los documentos en la carpeta `docs/` (si aplica) o contacta al equipo responsable.

## Buenas prácticas al crear reglas
- Favor la especificidad sobre reglas demasiado genéricas.
- Añade ejemplos y contraejemplos en los tests.
- Incluye referencias y razonamiento para la lógica de la detección.
- Mapea siempre la regla a MITRE (táctica y técnica) para facilitar priorización.

## Licencia y contacto
Este proyecto está bajo la licencia [MIT](LICENSE) (ajusta si corresponde).  
Para preguntas o soporte: contacta a los mantenedores en el repositorio o abre un issue.

---
Si quieres, puedo:
- Añadir comandos específicos según los scripts del repositorio.
- Generar plantillas de reglas en YAML o JSON.
- Crear un ejemplo de pipeline CI para validación y exportación automática.
