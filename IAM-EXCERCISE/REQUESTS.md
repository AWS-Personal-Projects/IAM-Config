# Documento de Requerimientos Técnicos (TRD)
**Proyecto:** Implementación de Seguridad IAM y Segregación de Roles
**Versión:** 1.0
**Fecha:** 18 de Febrero, 2026

---

## 1. Matriz de Usuarios y Accesos

A continuación, se detalla la nómina de identidades a aprovisionar, sus roles funcionales y el tipo de credencial requerida.

| Nombre Usuario | Cargo | Departamento | Grupo IAM Sugerido | Tipo de Acceso |
| :--- | :--- | :--- | :--- | :--- |
| **`admin.luis`** | Cloud Lead | Ingeniería | `G_CloudAdmin` | Consola + Programático (SDK) |
| **`admin.ana`** | DevOps Senior | Ingeniería | `G_CloudAdmin` | Consola + Programático (SDK) |
| **`dev.carlos`** | Backend Lead | Desarrollo | `G_BackendDevelopers` | Consola + Programático (SDK) |
| **`dev.sofia`** | Junior Dev | Desarrollo | `G_BackendDevelopers` | Solo Consola |
| **`dev.miguel`** | Junior Dev | Desarrollo | `G_BackendDevelopers` | Solo Consola |
| **`data.elena`** | Data Scientist | Analítica | `G_DataScience` | Solo Programático (SDK) |
| **`data.jorge`** | Data Engineer | Analítica | `G_DataScience` | Consola + Programático (SDK) |
| **`audit.roberto`**| CISO Externo | Seguridad | `G_Auditors` | Solo Consola (Solo Lectura) |

---

## 2. Especificación de Permisos por Departamento

Se definen las **Políticas (Policies)** que deben aplicarse a cada grupo para cumplir con el principio de *Menor Privilegio*.

### 🔴 Departamento de Ingeniería (Admin)
* **Grupo:** `G_CloudAdmin`
* **Permiso Base:** `AdministratorAccess` (AWS Managed Policy).
* **Condición de Seguridad:**
    * Se debe adjuntar una política que **DENIEGUE** todas las acciones si el usuario no ha iniciado sesión con **MFA (Multi-Factor Authentication)**.
    * *Objetivo:* Proteger las cuentas con privilegios elevados contra el robo de contraseñas.

### 🟠 Departamento de Desarrollo (Backend)
* **Grupo:** `G_BackendDevelopers`
* **Política Personalizada (Customer Managed Policy):**
    * **EC2 (Cómputo):**
        Acceso completo
    * **EKS (Kubernetes):**
         Solo Lectura (Solo visibilidad)
    * **S3 (Almacenamiento):**
        Acceso completo

### 🔵 Departamento de Analítica (Data)
* **Grupo:** `G_DataScience`
* **Política Personalizada:**
    * **S3 (Almacenamiento):**
        solo lectura

### 🟢 Departamento de Auditoría (Seguridad)
* **Grupo:** `G_Auditors`
* **Permiso Adicional:**
    Acceso específico a **AWS CloudTrail** para descargar y leer los historiales de logs (`LookupEvents`, `GetTrailStatus`).
    * *Objetivo:* Permitir auditoría forense sin riesgo de modificación de la infraestructura.

---
