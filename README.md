#  OWASP Top 10 Adaptado a Microsoft Fabric (v2.0)

##  Descripción

Framework de seguridad específico para **Microsoft Fabric**, **Power BI** y ecosistemas de **Business Intelligence**.

**Versión 2.0** — Actualizado con:
- OWASP Top 10 **2025**
- OWASP LLM Top 10 **2025** (para Fabric Copilot y AI features)
- OWASP API Security Top 10 **2023**

> **Autor:** Cristian Vera  
> **Repositorio:** [github.com/cristianvera86/owasp-for-fabric](https://github.com/cristianvera86/owasp-for-fabric)  
> **Versión:** 2.0 — Junio 2026

---

##  Objetivo

- Proveer un marco práctico y accionable para auditar la seguridad en entornos de BI/Fabric
- Mapear riesgos específicos de plataformas de datos al nuevo OWASP Top 10 2025
- Incluir riesgos emergentes de AI/LLM relevantes para Fabric Copilot
- Ofrecer controles concretos basados en herramientas del ecosistema Microsoft

---

## OWASP Top 10 2025 — Cambios Clave

| # | OWASP 2025 | Cambio vs 2021 |
|---|------------|----------------|
| A01 | Broken Access Control | Se mantiene #1 |
| A02 | Cryptographic Failures | Se mantiene |
| A03 | Injection | Se mantiene |
| A04 | Insecure Design | Se mantiene |
| A05 | Security Misconfiguration | Se mantiene |
| A06 | Vulnerable and Outdated Components | Se mantiene |
| A07 | Identification and Authentication Failures | Se mantiene |
| A08 | Software and Data Integrity Failures | Se mantiene |
| A09 | Security Logging and Monitoring Failures | Se mantiene |
| A10 | Server-Side Request Forgery (SSRF) | Se mantiene |

**Nuevas consideraciones 2025:**
- Mayor énfasis en **Supply Chain Security** (A08)
- **AI/ML Security** integrado transversalmente
- **Zero Trust** como principio de diseño (A04)

---

## Top 15 de Riesgos para Microsoft Fabric

### Riesgos Core (FAB-01 a FAB-10)

| ID | Riesgo | OWASP 2025 | Ejemplo en Fabric | Cómo Detectarlo | Controles |
|----|--------|------------|-------------------|-----------------|-----------|
| **FAB-01** | Broken Access Control en Dashboards | **A01** | Dashboard con PII compartido públicamente o con invitados sin revisión | Power BI Scanner, auditoría de compartición, DLP alerts | DLP + Sensitivity Labels, revisión periódica de permisos, políticas de compartición |
| **FAB-02** | Data Exfiltration via Export | **A01 + A08** | Analista exporta dataset completo con PII fuera de la organización | Alertas de exportación en Sentinel, logs de auditoría | Bloqueo de exportación, Sensitivity Labels, DLP policies |
| **FAB-03** | Hardcoded Secrets in Notebooks | **A02** | Notebook contiene API keys o connection strings en código | TruffleHog, GitHub Secret Scanning, grep en CI/CD | Azure Key Vault + Managed Identity, revisión en pipelines |
| **FAB-04** | Excessive IAM Privileges | **A01** | Todos los usuarios tienen rol Admin en workspace financiero | PowerShell/CLI audit, panel de administración | RBAC granular, acceso basado en grupos, revisiones periódicas |
| **FAB-05** | Insufficient Security Logging | **A09** | Sin registro de quién accedió o descargó datos confidenciales | Verificar logs en Purview, M365 Compliance, Sentinel | Activar logging completo, integración SIEM, alertas |
| **FAB-06** | Data Governance Failures | **A04** | Datasets sin clasificación, ownership ni lineage | Escaneo con Purview, revisión de etiquetas | Auto-labeling, clasificación obligatoria, data stewards |
| **FAB-07** | Authentication Bypass | **A07** | Invitados externos acceden sin MFA ni restricciones | Logs de Entra ID, Defender for Identity | MFA obligatorio, Conditional Access, segmentación |
| **FAB-08** | Pipeline Privilege Escalation | **A05** | Service Principal con Contributor sobre todos los workspaces | IAM audit, revisión de identidades en DevOps | Least privilege, scope por workspace, TakeOver limitado |
| **FAB-09** | Code Injection in Notebooks | **A03** | Input de usuario ejecuta código Spark sin sanitización | Test ofensivo, code review, SAST | Validación de inputs, escaping, sandboxing |
| **FAB-10** | Shadow IT / Ungoverned Artifacts | **A05** | Usuarios crean workspaces y conexiones sin gobernanza | Power BI Scanner API, inventario de actividad | Restricción de creación, políticas de autoservicio |

### Nuevos Riesgos 2025 (FAB-11 a FAB-15)

| ID | Riesgo | OWASP 2025 | Ejemplo en Fabric | Cómo Detectarlo | Controles |
|----|--------|------------|-------------------|-----------------|-----------|
| **FAB-11** | Vulnerable Notebook Dependencies | **A06** | pandas < 2.0 con CVE conocido en notebooks | Dependabot, pip-audit, safety check | Escaneo en CI/CD, actualización automática, Environment policies |
| **FAB-12** | SSRF via Dataflows/Pipelines | **A10** | Pipeline conecta a metadata service interno (169.254.x.x) | Test de penetración, WAF logs | Whitelist de URLs, firewall rules, validación de endpoints |
| **FAB-13** | Prompt Injection (Copilot) | **LLM01** | Usuario manipula respuestas de Fabric Copilot via prompts maliciosos | Logs de Copilot, análisis de prompts | Input validation, guardrails, rate limiting |
| **FAB-14** | Dataset Supply Chain Attack | **A08** | Dataset externo contiene datos envenenados o malware en archivos | Validación de origen, checksums, sandboxing | Verificación de fuentes, cuarentena, scanning |
| **FAB-15** | Lakehouse ACL Misconfiguration | **A01** | Folder en OneLake accesible públicamente por error | ACL audit, Purview scanning | Least privilege en ACLs, revisión periódica, alertas |

---

## Riesgos de AI/LLM (OWASP LLM Top 10 2025)

Aplicables a **Fabric Copilot**, **Semantic Link**, y features de AI en Fabric:

| OWASP LLM | Riesgo | Aplicación en Fabric | Controles |
|-----------|--------|---------------------|-----------|
| **LLM01** | Prompt Injection | Copilot ejecuta acciones no autorizadas via prompt malicioso | Input sanitization, guardrails, logging |
| **LLM02** | Insecure Output Handling | Copilot genera código SQL/DAX con vulnerabilidades | Code review, validación de output |
| **LLM03** | Training Data Poisoning | Modelos custom entrenados con datos comprometidos | Validación de datasets, lineage |
| **LLM06** | Sensitive Information Disclosure | Copilot expone datos PII en respuestas | DLP en outputs, Sensitivity Labels |
| **LLM08** | Excessive Agency | Copilot tiene permisos excesivos para ejecutar acciones | Least privilege, scope limitado |
| **LLM09** | Overreliance | Usuarios confían ciegamente en outputs de Copilot | Training, validación humana |

---

## Herramientas y Controles

### Detección y Monitoreo

| Herramienta | Para qué | Riesgos que cubre |
|-------------|----------|-------------------|
| **Microsoft Purview** | Clasificación, DLP, Data Lineage, Audit | FAB-01, FAB-02, FAB-06, FAB-15 |
| **Microsoft Sentinel** | SIEM, alertas, threat hunting | FAB-05, FAB-07, FAB-08 |
| **Defender for Cloud** | Postura de seguridad, vulnerabilidades | FAB-11, FAB-12, FAB-14 |
| **Power BI Scanner API** | Inventario, permisos, compartición | FAB-01, FAB-04, FAB-10 |
| **GitHub Advanced Security** | Secret scanning, code scanning, Dependabot | FAB-03, FAB-09, FAB-11 |

### Prevención

| Control | Implementación | Riesgos que mitiga |
|---------|----------------|-------------------|
| **Sensitivity Labels** | Purview + Power BI | FAB-01, FAB-02, FAB-06 |
| **Conditional Access** | Entra ID | FAB-07 |
| **Managed Identity** | Azure + Fabric | FAB-03, FAB-08 |
| **DLP Policies** | Purview + M365 | FAB-02, LLM06 |
| **RBAC Granular** | Fabric Workspaces | FAB-04, FAB-15 |

---

## Matriz de Priorización

| Prioridad | Riesgos | Impacto | Esfuerzo |
|-----------|---------|---------|----------|
| 🔴 **Crítica** | FAB-03, FAB-09, FAB-13 | Alto | Medio |
| 🟠 **Alta** | FAB-01, FAB-02, FAB-04, FAB-11 | Alto | Bajo-Medio |
| 🟡 **Media** | FAB-05, FAB-06, FAB-07, FAB-08, FAB-14 | Medio | Medio |
| 🟢 **Baja** | FAB-10, FAB-12, FAB-15 | Bajo-Medio | Alto |

---

## 🛠 Implementación en CI/CD

### Controles Automatizados en Pipeline

```yaml
# Azure DevOps Pipeline - Security Gates
stages:
  - stage: SecurityScan
    jobs:
      - job: SecretScanning
        steps:
          - script: |
              pip install trufflehog
              trufflehog filesystem --directory ./notebooks --fail
            displayName: 'FAB-03: Secret Scanning'
      
      - job: DependencyCheck
        steps:
          - script: |
              pip install safety pip-audit
              pip-audit --require-hashes --strict
            displayName: 'FAB-11: Dependency Vulnerabilities'
      
      - job: CodeScanning
        steps:
          - task: AdvancedSecurity-Codeql-Init@1
          - task: AdvancedSecurity-Codeql-Analyze@1
            displayName: 'FAB-09: Code Injection Analysis'
```

### Post-Deploy Validations

```python
# Validar permisos después del deploy (FAB-04)
import requests

def audit_workspace_permissions(workspace_id, token):
    headers = {'Authorization': f'Bearer {token}'}
    
    # Obtener usuarios del workspace
    resp = requests.get(
        f'https://api.powerbi.com/v1.0/myorg/groups/{workspace_id}/users',
        headers=headers
    )
    
    users = resp.json().get('value', [])
    admins = [u for u in users if u['groupUserAccessRight'] == 'Admin']
    
    if len(admins) > 3:
        raise SecurityException(f'FAB-04: Demasiados admins ({len(admins)})')
    
    return users
```

---

##  Checklist de Auditoría

### Pre-Deploy
- [ ] **FAB-03**: Escaneo de secrets en notebooks completado
- [ ] **FAB-09**: Code review de inputs en notebooks
- [ ] **FAB-11**: Dependencias actualizadas y sin CVEs

### Post-Deploy
- [ ] **FAB-01**: Verificar compartición de dashboards
- [ ] **FAB-04**: Auditar roles de workspace
- [ ] **FAB-06**: Confirmar Sensitivity Labels aplicadas
- [ ] **FAB-15**: Revisar ACLs de Lakehouse

### Periódico (Mensual)
- [ ] **FAB-05**: Revisar logs de acceso en Sentinel
- [ ] **FAB-07**: Auditar accesos externos
- [ ] **FAB-10**: Inventario de workspaces no gobernados
- [ ] **FAB-14**: Validar integridad de datasets externos

---

## Referencias

### OWASP
- [OWASP Top 10 2025](https://owasp.org/Top10/)
- [OWASP LLM Top 10](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
- [OWASP API Security Top 10](https://owasp.org/API-Security/)

### Microsoft
- [Fabric Security Documentation](https://learn.microsoft.com/en-us/fabric/security/)
- [Power BI Security Whitepaper](https://learn.microsoft.com/en-us/power-bi/guidance/whitepaper-powerbi-security)
- [Microsoft Purview](https://learn.microsoft.com/en-us/purview/)
- [Defender for Cloud](https://learn.microsoft.com/en-us/azure/defender-for-cloud/)

### Herramientas
- [TruffleHog](https://github.com/trufflesecurity/trufflehog)
- [pip-audit](https://github.com/pypa/pip-audit)
- [GitHub Advanced Security](https://docs.github.com/en/code-security)

---

##  Changelog

| Versión | Fecha | Cambios |
|---------|-------|---------|
| 2.0 | Junio 2026 | Actualización a OWASP 2025, nuevos riesgos FAB-11 a FAB-15, integración LLM Top 10 |
| 1.0 | Octubre 2025 | Versión inicial con 10 riesgos |

---


