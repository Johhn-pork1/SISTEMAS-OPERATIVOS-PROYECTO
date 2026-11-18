# Sistema Distribuido Tolerante a Fallos con Consenso Raft

Proyecto académico desarrollado para la materia de **Sistemas Distribuidos** (Centro Universitario UAEM Tianguistenco). Implementa un clúster heterogéneo de 5 nodos operando de manera interoperable entre **Windows** y **Ubuntu Linux** mediante sockets TCP.

---

## 1. Esquema Topológico y Arquitectura de Red

El clúster opera sobre una subred local en modo **Adaptador Puente (Bridged Mode)**, permitiendo comunicación directa bidireccional a nivel de capa 3 entre el sistema anfitrión y la máquina virtual:

```text
+-----------------------------------------------------------------------------+
|                    RED LOCAL / ADAPTADOR PUENTE (LAN)                       |
+--------------------------------------+--------------------------------------+
                                       |
               +-----------------------+-----------------------+
               |                                               |
+--------------v--------------------+           +--------------v--------------------+
|     HOST ANFITRIÓN (Windows)      |           |    MÁQUINA VIRTUAL (Ubuntu)       |
|         IP: 192.168.100.X         |           |        IP: 192.168.100.23         |
+-----------------------------------+           +-----------------------------------+
| [Nodo 4] - Activo (Puerto 5004)   |<-- TCP -->| [Nodo 1] - Activo (Puerto 5001)   |
|   * Participa en quórum           |  Sockets  |   * Participa en quórum           |
|   * Ejecuta tareas y réplicas     |  (JSON)   |   * Ejecuta tareas y réplicas     |
|                                   |           |                                   |
| [Nodo 5] - Espejo (Puerto 5005)   |<-- Sync --| [Nodo 2] - Activo (Puerto 5002)   |
|   * Respaldo pasivo total         |  Pasivo   |   * Participa en quórum           |
|   * Excluido de votaciones        |           |   * Ejecuta tareas y réplicas     |
|   * Rechaza ejecución de tareas   |           |                                   |
|                                   |           | [Nodo 3] - Activo (Puerto 5003)   |





|                                   |           |   * Participa en quórum           |
|                                   |           |   * Ejecuta tareas y réplicas     |
+-----------------------------------+           +-----------------------------------+
