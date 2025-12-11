# Anteproyecto  
**Proyecto Intermodular de Administración de Sistemas Informáticos en Red**

---

## 1. TÍTULO  
Servidor DHCP con resolución DNS y gestión de base de datos

## 2. DESCRIPCIÓN  
El objetivo es configurar un servidor con una base de datos para que los clientes puedan acceder a ella y desde ese servidor que proporcione IPs a esos clientes.

## 3. Justificación  

### Importancia del proyecto  
La gestión correcta de direcciones IP, resolución de nombres y registro centralizado de eventos es crítica para la operativa de cualquier red. Un servidor DHCP que se integre con DNS y con una base de datos permite asignaciones automáticas de IP, actualizaciones dinámicas de nombres (DNS) y un histórico consultable de leases y cambios. Esto reduce errores humanos, evita conflictos de direcciones, facilita la administración y ofrece trazabilidad, funcionalidades imprescindibles en entornos empresariales y útiles como práctica formativa en entornos educativos.

### Problema o necesidad  
En muchas redes pequeñas y medianas la asignación de IP y la gestión de DNS se hace manualmente o con soluciones fragmentadas, lo que provoca:  
- Conflictos de IP por registros desactualizados.  
- Dificultad para localizar equipos en la red por nombres incorrectos.  
- Falta de registro centralizado para auditoría y análisis.  

Mi solución busca automatizar la asignación, mantener la coherencia DNS/IP mediante actualizaciones dinámicas y almacenar la información en una base de datos para consulta y análisis.

### Estado del Arte  
Existen varias soluciones maduras en el mercado y en el software libre:  
- **ISC Kea DHCP**: servidor DHCP moderno diseñado por ISC que soporta backends de base de datos (MySQL/PostgreSQL) para almacenar leases y configuraciones, y ofrece gestión vía REST API, idóneo cuando se requiere persistencia y escalabilidad.  
- **PowerDNS**: servidor DNS que puede usar bases de datos SQL (MySQL/Postgres) como backend, facilitando la gestión de zonas desde una base de datos y la integración con sistemas externos.  
- **dnsmasq**: solución ligera que combina DNS y DHCP en equipos con pocos recursos; es muy usada en entornos LAN y dispositivos/routers por su simplicidad.  
- **Windows Server (DHCP + DNS)**: entornos Windows integran DHCP con DNS dinámico (RFC 2136) para registrar automáticamente hostnames cuando se asignan direcciones por DHCP, funcionalidad común en infraestructuras empresariales Windows.

### Cómo se diferencia mi proyecto a lo demas / aporte de valor de mi proyecto  
Mi proyecto propone diseñar e implementar un servidor DHCP integrado con resolución DNS y una base de datos de soporte, orientado a un entorno de laboratorio/pyme y con las siguientes aportaciones prácticas frente a las opciones existentes:  
- **Uso de backends SQL para trazabilidad**: almacenar leases y eventos en MySQL/Postgres para facilitar consultas, auditoría y generación de informes (aprovechando características de Kea/PowerDNS). Esto ofrece más capacidad de análisis que soluciones ligeras que solo usan ficheros.  
- **Integración práctica DNS↔DHCP con DDNS**: demostrar la actualización dinámica de registros DNS cuando cambian las IPs asignadas por DHCP, mostrando interoperabilidad entre servidores abiertos y/o Windows.  
- **Coste y escalabilidad razonable**: elegir software libre (Kea, PowerDNS, dnsmasq según necesidades) para comparar enfoques, desde una solución ligera hasta una más robusta con base de datos, mostrando trade-offs frente a appliances comerciales (Infoblox).

### Conclusión  
Este proyecto es relevante porque aborda una necesidad real de coherencia y trazabilidad en la gestión de red, aporta una implementación práctica y educativa que combina componentes existentes de código abierto con una capa de persistencia y consulta en base de datos, y permite comparar alternativas aportando criterios claros para su selección y despliegue.

## 4. Objetivos  

### Objetivo General  
Diseñar e implementar un servidor de red que integre servicios de DHCP, DNS y base de datos, con el fin de automatizar la asignación de direcciones IP, mantener la coherencia de los registros de nombres y almacenar la información de forma centralizada para su consulta y análisis.

### Objetivos Específicos  
1. Configurar un servidor DHCP para la asignación automática de direcciones IP dentro de un rango definido.  
2. Implementar la integración entre el servidor DHCP y el servidor DNS para habilitar actualizaciones dinámicas de registros.  
3. Desplegar un servidor DNS que permita la resolución de nombres dentro de la red local.  
4. Diseñar una base de datos que almacene información sobre leases de DHCP y registros DNS actualizados.  
5. Conectar el servidor DHCP y el servidor DNS con la base de datos para registrar y consultar información de forma centralizada.  
6. Realizar pruebas de funcionamiento verificando la correcta asignación de IPs, actualización de nombres en DNS y almacenamiento en la base de datos.  
7. Documentar el proceso de instalación, configuración y pruebas para que pueda ser reproducido en otros entornos.

## 5. Lenguajes de programación y tecnologías  

### Plataforma y virtualización  
- **Propuesta principal**: VirtualBox o VMware Workstation, facilitan la creación de laboratorios reproducibles en equipos de aula/portátil.  
  *Justificación*: sencillos de usar, compatibilidad con imágenes ISO de Linux/Windows y buen control de redes virtuales (NAT/Bridged/Host-only).  
- **Alternativa para servidor/producción**: Proxmox VE o KVM.  
  *Justificación*: más orientados a entornos reales/servidores y permiten snapshots y gestión centralizada.

### Sistemas operativos  
- **Windows Server 2019/2022** → servidor principal.  
  *Justificación*: incluye servicios nativos de DHCP, DNS y se integra con Active Directory, facilitando la gestión centralizada de usuarios y equipos.

### Servidor DHCP  
- **DHCP de Windows Server**.  
  *Justificación*: servicio nativo, confiable y con soporte para asignación automática de direcciones IP, reservas, y registro de eventos en el visor de sucesos.

### Servidor DNS  
- **DNS de Windows Server integrado con DHCP**.  
  *Justificación*: permite actualizaciones dinámicas (DDNS) y resolución de nombres interna, sincronizando automáticamente registros con el DHCP.

### Base de datos  
- **SQL Server Express** (o MySQL/MariaDB en Windows como alternativa).  
  *Justificación*: almacenar información de leases, registros DNS y eventos para auditoría y consultas desde scripts o aplicaciones.

### Lenguajes y scripting  
- **PowerShell** → Automatización de tareas de configuración, consultas a DHCP/DNS y exportación de datos a la base de datos.  
- **Python (opcional)** → Para análisis de datos, generación de informes o pruebas automatizadas si se desea integrar scripts externos.

### Herramientas y utilidades  
- **RSAT (Remote Server Administration Tools)** → Gestión remota de roles y servicios de Windows Server.  
- **Wireshark / tcpdump** → Verificación del tráfico DHCP y DNS para pruebas de laboratorio.  
- **SQL Server Management Studio (SSMS)** → Gestión de la base de datos y consultas.  
- **VS Code / PowerShell ISE** → Edición de scripts y automatización.

### Protocolos y estándares  
DHCPv4, DDNS (RFC 2136) y protocolos SQL para conexión con la base de datos.

## 7. Gestor de bases de datos  

### Sistema de gestión de bases de datos  
**SQL Server Express**.  
*Justificación*:  
- Integración nativa con Windows Server.  
- Permite crear bases de datos locales para almacenar información de leases de DHCP, registros DNS y auditoría.  
- Fácil administración mediante SQL Server Management Studio (SSMS).  
- Compatible con consultas T-SQL y scripts de automatización en PowerShell.

### Estructura de la base de datos  
La base de datos almacenará principalmente información de:  
- **Leases de DHCP**: direcciones IP asignadas, MAC del dispositivo, fecha de asignación y expiración.  
- **Registros DNS**: relación entre nombres de host y direcciones IP, tipo de registro (A, PTR) y fecha de actualización.  
- **Auditoría / Logs**: acciones de asignación, liberación o modificación de registros para trazabilidad.

### Modelo Entidad-Relación 
**Entidades principales**:  
- Dispositivo  
- Leases_DHCP  
- Registros_DNS  
<img width="609" height="474" alt="imagen" src="https://github.com/user-attachments/assets/067995ee-9aa3-409a-9ea5-6808e7522e32" />

**Relaciones**:  
- Un Dispositivo puede tener varios Leases_DHCP a lo largo del tiempo (relación 1:N).  
- Un Dispositivo puede tener varios Registros_DNS (relación 1:N).  
- Cada Lease_DHCP y Registro_DNS está asociado a un único Dispositivo.

## 8. Entornos de aplicación del proyecto  

### Hardware  
- **PC o portátil para laboratorio virtual**  
  - Procesador mínimo: Intel i5 / AMD equivalente.  
  - RAM: 16 GB (para correr varias máquinas virtuales simultáneamente).  
  - Almacenamiento: 500 GB SSD (para alojar las VMs y bases de datos).  
  *Justificación*: recursos suficientes para ejecutar Windows Server y clientes virtuales de prueba sin problemas de rendimiento.

### Switch o router virtual  
- Usado dentro de VirtualBox para conectar las máquinas virtuales.  
  *Justificación*: permite simular una red local completa, controlar subredes y probar DHCP/DNS.

### Software  
- **Windows Server 2019/2022 (VM)**  
  Rol de DHCP, DNS y opcionalmente Active Directory.  
  *Justificación*: proporciona servicios nativos integrados de red y administración centralizada.  
- **Clientes Windows/Linux (VMs)**  
  Para probar asignación de IP automática, resolución de nombres y consultas DNS.  
- **SQL Server Express**  
  Almacena leases, registros DNS y logs de auditoría.  
  *Justificación*: gestor de base de datos ligero y compatible con Windows Server.  
- **VirtualBox**  
  Para crear y gestionar las máquinas virtuales del laboratorio.  
  *Justificación*: permite configurar redes internas y realizar pruebas de laboratorio sin afectar la máquina principal.  
- **PowerShell**  
  Para automatizar tareas de administración, consultas y generación de informes.  
- **RSAT (Remote Server Administration Tools)**  
  Para gestionar remotamente los roles y servicios del servidor Windows.  
- **Wireshark / tcpdump**  
  Para monitorizar y verificar tráfico DHCP y DNS durante pruebas.  
- **SQL Server Management Studio (SSMS)**  
  Para crear, consultar y gestionar la base de datos.

### Configuraciones de red  
- **Red interna VM**: Subred configurada para DHCP. Switch virtual conectado a todas las VMs.  
- **Rangos DHCP y reservas**: Definidos en Windows Server para probar asignación dinámica y control de IPs críticas.  
- **Integración DHCP ↔ DNS**: Actualización dinámica de registros DNS basada en los leases asignados.

### Herramientas de desarrollo y pruebas  
- **Visual Studio Code / PowerShell ISE**: Edición de scripts de automatización y consultas SQL.  
- **Figma / Balsamiq**: Para prototipos de panel de administración web o visualización de la interfaz.  
- **Opcional: Docker / contenedores Windows**: Para pruebas de servicios aislados si se desea modularidad.

## 9. Fases de desarrollo  

### Planificación inicial  
Definir los objetivos del proyecto y los servicios a implementar: DHCP, DNS y base de datos.  
Identificar recursos necesarios: hardware para VM, licencias de Windows Server, software adicional (SQL Server Express, herramientas de administración).

### Análisis y diseño  
Analizar requisitos funcionales y técnicos: rangos de IP, subredes, nombres de dominio, estructura de la base de datos.  
Diseñar la arquitectura del laboratorio virtual con Windows Server: roles a instalar, configuración de red y seguridad.  
Elaborar diagramas de flujo de asignación de IP, resolución de nombres y registro en la base de datos.
<img width="1183" height="681" alt="Captura desde 2025-12-11 11-48-35" src="https://github.com/user-attachments/assets/7e8e96fe-e70c-4122-af2f-323f32c4200b" />
### Desarrollo e implementación  
Instalar Windows Server en la máquina virtual y configurar roles de DHCP y DNS.  
Configurar la base de datos SQL Server Express y crear las tablas necesarias para registrar leases y registros DNS.  
Automatizar tareas y consultas mediante PowerShell y, opcionalmente, Python.  
Integrar DHCP, DNS y base de datos para que trabajen de forma coordinada.

### Pruebas y validación  
Realizar pruebas de asignación automática de IP a clientes en la red virtual.  
Verificar actualización dinámica de registros DNS con los cambios de IP.  
Comprobar almacenamiento correcto de datos en la base de datos y capacidad de consultas e informes.  
Corregir errores o inconsistencias detectadas durante las pruebas.

### Despliegue y puesta en marcha  
Finalizar la configuración del entorno del laboratorio.  
Documentar paso a paso la instalación, configuración, scripts y resultados de pruebas.  
Preparar un entorno estable listo para demostración o uso educativo.

### Mantenimiento y soporte  
Monitorear la asignación de IPs, registros DNS y actividad en la base de datos.  
Realizar actualizaciones de Windows Server, roles y base de datos según sea necesario.  
Proporcionar soporte para corrección de errores o mejoras futuras.

## 10. Módulos del ciclo formativo relacionados  

### Implantación de Sistemas Operativos  
Conexión con el proyecto: La instalación, configuración y gestión de Windows Server para desplegar los servicios DHCP, DNS y SQL Server se basa directamente en los conocimientos adquiridos en este módulo.

### Servicios de Red e Internet  
Conexión con el proyecto: Configurar servicios de red como DHCP y DNS, gestionar subredes, dominios y resolución de nombres es contenido central de este módulo.

### Administración de Sistemas Gestores de Bases de Datos  
Conexión con el proyecto: La creación de la base de datos, diseño de tablas y consultas para almacenar leases y registros DNS se relaciona directamente con la gestión de bases de datos enseñada en este módulo.

### Seguridad y Alta Disponibilidad de Sistemas  
Conexión con el proyecto: La configuración segura del servidor, gestión de permisos, y seguimiento de registros y auditorías se relaciona con la seguridad y resiliencia de sistemas aprendida en este módulo.

## 11. Anexos y bibliografía  

### Anexos  
- **Diagramas de red**: Diagrama del laboratorio virtual mostrando la VM de Windows Server, clientes conectados, subredes, rangos de IP, servicios DHCP, DNS y base de datos.  
- **Capturas de pantalla de configuraciones**:  
  - Configuración del rol DHCP en Windows Server.  
  - Configuración del rol DNS con actualizaciones dinámicas.  
  - Creación de la base de datos en SQL Server Express y tablas para almacenar leases y registros DNS.  
- **Scripts y automatizaciones**: Scripts de PowerShell para consultar leases, actualizar registros DNS y volcar información en la base de datos.

### Bibliografía  
- Implementación de DHCP utilizando PowerShell  
  https://learn.microsoft.com/en-us/windows-server/networking/technologies/dhcp/dhcp-deploy-wps  
- Instalación y configuración del servidor DHCP  
  https://learn.microsoft.com/en-us/windows-server/networking/technologies/dhcp/quickstart-install-configure-dhcp-server  
- Instalación de AD DS, DNS y DHCP utilizando PowerShell en Windows Server 2016  
  https://malwaremily.medium.com/install-ad-ds-dns-and-dhcp-using-powershell-on-windows-server-2016-ac331e5988a7  
- Uso de PowerShell para instalar un servidor DHCP en Windows Server 2019 (Server Core) con Active Directory Domain Controller  
  https://mikefrobbins.com/2018/12/06/use-powershell-to-install-a-dhcp-server-on-a-windows-server-2019-server-core-active-directory-domain-controller/  
- Uso de PowerShell para desplegar DHCP en Windows Server 2022  
  Tutorial que muestra cómo desplegar y configurar el servicio DHCP utilizando PowerShell en Windows Server 2022.  
  https://jotelulu.com/en-gb/support/tutorials/how-to-deploy-dhcp-using-powershell/
