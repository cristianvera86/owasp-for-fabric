
# 🔐 OWASP Top 10 Adaptado a Business Intelligence y Microsoft Fabric

## 📘 Descripción

Este framework fue desarrollado para cubrir un vacío existente en la seguridad de plataformas de datos modernas como **Microsoft Fabric**, **Power BI**, y entornos de **Business Intelligence (BI)** en general. Inspirado en los principios de OWASP, MITRE ATT&CK y experiencias reales en entornos productivos, propone un **Top 10 de riesgos específicos** para ecosistemas de datos.

---

## 🎯 Objetivo

- Proveer un marco práctico y accionable para auditar y fortalecer la seguridad en entornos de BI.
- Exponer riesgos que no están cubiertos por OWASP tradicional.
- Ofrecer controles concretos basados en herramientas reales del ecosistema Microsoft y buenas prácticas DevSecOps.

---

## 📌 Top 10 de Riesgos

| ID      | Riesgo                                | Basado en OWASP                            | Ejemplo en Fabric                                                                 | Cómo Detectarlo                                                       | Controles Sugeridos                                                                                 |
|---------|----------------------------------------|--------------------------------------------|-----------------------------------------------------------------------------------|------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------|
| FAB-01 | Exposición de dashboards sensibles     | A01 (Broken Access Control), CNA-06        | Dashboard con datos PII compartido públicamente o con invitados sin revisión.    | Auditoría de comparticiones, Power BI Scanner, DLP + alertas.         | DLP con etiquetas sensibles, revisión de permisos, políticas de compartición restringida.           |
| FAB-02 | Exportación no controlada de datasets  | DSC (Data at Rest/Export), CNA-10          | Un analista exporta un dataset completo con PII y lo guarda fuera de la organización. | Alertas de exportación en Sentinel, logs de auditoría de Power BI.    | Bloqueo DLP, Policy tips, Sensitivity Labels + restricciones de exportación.                        |
| FAB-03 | Secrets embebidos en pipelines         | CNA-03 (Insecure Secrets Management)       | Notebook contiene claves API de acceso a storage embebidas en código Python.     | Escaneo de notebooks (TruffleHog, GitHub Secret Scanning).            | Uso de Azure Key Vault + managed identity, revisión en CI/CD.                                      |
| FAB-04 | IAM excesivo en workspaces             | CNA-01 (Inadequate IAM), A05               | Todos los usuarios tienen rol Admin en un workspace con datos financieros.        | PowerShell/Azure CLI, revisión de roles, panel de administración.     | Revisión periódica de permisos, RBAC granular, acceso basado en grupos.                            |
| FAB-05 | Falta de logging/auditoría             | A09 (Security Logging), CNA-10             | No hay registro de quién accedió o descargó un dataset etiquetado como confidencial. | Logs en Purview, M365 Compliance Center, Sentinel.                    | Activación de logging, integración con SIEM, revisión periódica de logs.                            |
| FAB-06 | Uso de datos sin clasificación         | DSC, CNA-02                                | Dataset con datos sensibles sin etiquetas ni restricciones.                       | Escaneo con Purview, revisión de etiquetas.                          | Auto-aplicación de etiquetas, clasificación automática, uso obligatorio de labels.                  |
| FAB-07 | Acceso externo no controlado           | A07 (Auth Failures), CNA-01                | Invitados externos acceden a dashboards sin MFA ni restricciones.                 | Logs de Entra ID, Power BI audit, Microsoft Defender Identity.        | MFA, políticas de acceso condicional, segmentación por tenant, alertas en Sentinel.                |
| FAB-08 | Pipelines con privilegios elevados     | A05, CNA-01                                | Un pipeline tiene permisos de contributor sobre todos los workspaces y storages. | IAM audit, revisión de identidad usada, configuración DevOps.         | Principio de menor privilegio, segmentación de identidades, revisión de permisos por etapa.         |
| FAB-09 | Inyección de código en notebooks       | A03 (Injection), CNA-05                    | Entrada de usuario ejecuta código sin sanitización.                               | Test ofensivo, revisión de código, escaneo estático.                  | Validación de inputs, escaping, revisión manual de código ejecutado.                               |
| FAB-10 | Shadow IT / artefactos fuera de control| A05, CNA-06                                | Usuarios crean dashboards o conexiones a orígenes sin gobernanza.                 | Power BI Scanner, revisión de workspaces y actividad no registrada.   | Restricción de creación de artefactos, políticas de autoservicio, gobierno de orígenes.             |




---

## 🧰 Herramientas compatibles

- Microsoft Purview
- Microsoft Sentinel
- Power BI Scanner API
- Azure DevOps Pipelines
- GitHub Advanced Security
- Microsoft Defender for Cloud

---

## 🛠 Cómo usar este framework

1. Evaluá tu entorno usando los 10 riesgos como guía.
2. Implementá controles sugeridos: DLP, IAM, etiquetas, auditorías.
3. Automatizá validaciones en pipelines CI/CD.
4. Integrá alertas en Sentinel y revisá periódicamente.
5. Compartilo con tu equipo de BI, IT y seguridad.

---

## 🧠 ¿Por qué esto importa?

Los incidentes recientes (Power BI, Microsoft Fabric, USAF) muestran que los datos pueden filtrarse sin malware, sin exploits. Solo hace falta **una mala configuración** o **un exceso de confianza**.

---

## 📄 Licencia y créditos

Este framework es una creación original inspirada en OWASP, MITRE ATT&CK y experiencias reales. Puede ser reutilizado, adaptado y extendido con fines educativos, comerciales o comunitarios citando la fuente original.

> **Autor:** Cristian Vera
> **LinkedIn:** linkedin.com/in/cristianjvera
> **Versión:** 1.0 — Octubre 2025

---

