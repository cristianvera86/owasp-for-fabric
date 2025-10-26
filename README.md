
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

| ID | Riesgo | Basado en |
|----|--------|-----------|
| FAB-01 | Exposición de dashboards sensibles | A01 / CNA-06 |
| FAB-02 | Exportación no controlada de datasets | DSC / CNA-10 |
| FAB-03 | Secrets embebidos en pipelines | CNA-03 |
| FAB-04 | IAM excesivo en workspaces | CNA-01 / A05 |
| FAB-05 | Falta de logging/auditoría en artefactos | A09 / CNA-10 |
| FAB-06 | Datos sin clasificación ni etiquetas | DSC / CNA-02 |
| FAB-07 | Acceso externo no controlado | A07 / CNA-01 |
| FAB-08 | Pipelines con privilegios elevados | A05 / CNA-01 |
| FAB-09 | Inyección de código en notebooks | A03 / CNA-05 |
| FAB-10 | Shadow IT: artefactos fuera de control | A05 / CNA-06 |



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

¿Te gustaría aportar o usar este marco en tu organización? ¡Forkealo, mejoralo y compartilo con la comunidad!
