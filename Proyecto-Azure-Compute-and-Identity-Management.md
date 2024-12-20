# Proyecto 1 : Compute and Identity Management

## Temas Tratados
VMs, RBAC, Azure AD, Policies, Encryption, Cost Management

## Resumen
Este proyecto implica implementar una máquina virtual, protegerla mediante RBAC, aplicar políticas de Azure para la gobernanza de recursos, cifrar datos confidenciales y monitorear costos. Enseña conceptos básicos para administrar identidades, gobernanza y recursos informáticos de Azure.

## Escenario
Una empresa desea implementar una máquina virtual para su aplicación web, protegerla con control de acceso basado en roles, aplicar una política para convenciones de nombres, cifrar datos confidenciales y monitorear el costo de los recursos implementados.

## Diagrama


## Tarea 1: Crear una Máquina Virtual

1. En el **Portal Azure**  seleccionar **Máquinas Virtuales** > **Crear** - 
  
<img width="259" alt="01" src="https://github.com/user-attachments/assets/54ca4ff4-37bb-4493-8677-9037051a221d" /> -

<img width="178" alt="02" src="https://github.com/user-attachments/assets/2f27a447-a480-43b1-ba17-728565bd4a2f" />


2. Selecciona Subscripción, Grupo de Recursos , Región, tamaño de VM y so (Windows/Linux). Para este caso, se creará un nuevo Grupo de Recursos y se utilizará una VM Ubuntu.
<img width="489" alt="03-ub" src="https://github.com/user-attachments/assets/5e10bbd6-79e3-4960-81e8-9e81111849f2" />

3. Configura un nombre de usuario admin/contraseña o llave SSH. Se ultilizará llave SSH
<img width="456" alt="04-ub" src="https://github.com/user-attachments/assets/01957f4a-6473-4b3c-8cf9-678cd373935c" />

4. Agregar un disco de datos durante la creación. Crear un nuevo disco > Tipo de Origen > Ninguno.
Si selecciona Blob de almacenamiento, podrá reutilizar discos externos a Azure o configuraciones personalizadas que haya cargado.
<img width="425" alt="05" src="https://github.com/user-attachments/assets/a6874a14-688a-4570-b5a9-ca8ec77146a4" />

5. Configurar redes: Asigna una IP privada estática a través de la sección Redes avanzadas.
Utiliza un grupo de seguridad de red (NSG) para permitir o bloquear el tráfico.
<img width="482" alt="06" src="https://github.com/user-attachments/assets/24a601bc-25bc-4e6e-aeb0-871ad05f267e" />

 >**Nota sobre NSG:** **Ninguno**: úsalo si estás administrando la configuración de la NIC manualmente o conectando una NIC existente.
**Básico**: úsalo para implementaciones rápidas y simples, como entornos de prueba o aplicaciones a pequeña escala.
**Avanzado**: úsalo cuando necesites un control detallado, como para cargas de trabajo de producción o máquinas virtuales que requieren configuraciones de red personalizadas.

6. Revisar y crear la máquina virtual. Descargar clave privada (Se utilizará más adelante).
<img width="237" alt="06-ub" src="https://github.com/user-attachments/assets/7005d4a4-5f15-4462-8bc7-566a021c508f" />

## Tarea 2: Configurar Acceso Basado en Roles (RBAC)

Ahora crea un almacén de claves y almacena la clave privada. Ten en cuenta que tendrás que asignarte los permisos (principio de privilegio mínimo).

1. En el **Portal de Azure** busca e ingresa a **Almacenes de Claves** > Crear
<img width="190" alt="07" src="https://github.com/user-attachments/assets/eef5e1ae-4db4-4f15-905a-a4d43bca693a" />

2. Crea el Almacen de Claves en el mismo Grupo de Recursos y misma Región en la que se creó la máquina virtual
<img width="439" alt="08" src="https://github.com/user-attachments/assets/638d306b-5264-460e-ae0b-d57ca04349b1" />

3. Revisar y Crear

4. Vuelve al Portal de Azure e ingresa a **Entra ID**
<img width="253" alt="09" src="https://github.com/user-attachments/assets/93021a34-77bd-4b5f-95ef-bd1ce1c0a317" />

5. Crea un **Grupo** y agrega tu usuario como **Propietario** y **Miembro** del Grupo
<img width="403" alt="10" src="https://github.com/user-attachments/assets/c1b3b893-ae79-4cc3-93f7-303b54daeb5c" />

6. Vuelve al Almacen de Claves creado con anterioridad, ingresa a **Control de Acceso (IAM)** > **Agregar Asignación de Roles**
<img width="352" alt="11" src="https://github.com/user-attachments/assets/382f01fc-ff71-48d1-967b-2897cea2b135" />

7. Selecciona el Rol **Administrador de Key Vault** > Siguiente
<img width="258" alt="12" src="https://github.com/user-attachments/assets/177a19ca-64bc-4783-a3da-b20253725cd6" />

8. En la pestaña **Miembros**, en la opción **Asignar acceso a** Seleccionar **Usuario, Grupo...**, luego seleccionar **+ Seleccionar miembros** y buscar el grupo creado con anterioridad > Revisar y Asignar (puede tardar en propagarse)
<img width="236" alt="13" src="https://github.com/user-attachments/assets/50bc9095-5e7d-4641-a563-074d2ef0c03f" />

9.  Regresa al Almacen de Claves, selecciona **Objetos** > **Claves**, selecciona **+ Generar o Importar**
<img width="257" alt="14" src="https://github.com/user-attachments/assets/306a86b2-5382-4353-8fa9-4de49c8fc896" />

10.  Selecciona la opción de **Importar**, carga el archivo con la clave privada SSH guardado (último punto de la tarea 1), deja el resto de opciones por defecto > Crear
<img width="705" alt="15" src="https://github.com/user-attachments/assets/75db01e3-1c13-483e-8e40-b1fb116fe3e7" />

11.  Ingresa a la Máquina Virtual creada, ingresa a **Control de Acceso (IAM)** > **Agregar Asignación de Roles**
<img width="350" alt="16" src="https://github.com/user-attachments/assets/2a74e033-b89d-4695-81d2-5daea51f97fc" />

12.  Selecciona el rol de **Colaborador de Máquina Virtual** > Siguiente
<img width="233" alt="17" src="https://github.com/user-attachments/assets/1eb4b8eb-bb9d-4328-8909-64f873ae61a1" />

13.  En la pestaña **Miembros**, en la opción **Asignar acceso a** Seleccionar **Usuario, Grupo...**, luego seleccionar **+ Seleccionar miembros** y buscar el grupo creado con anterioridad > Revisar y Asignar
<img width="229" alt="18" src="https://github.com/user-attachments/assets/a6a06447-6cb7-4970-98b9-9022018349ee" />

14.  Verifica los permisos que se le han otorgado al Grupo, ve a **Entra ID** > **Administrar** > **Todos los Grupos** > **T Grupo** > **Asignaciones de roles de Azure**
<img width="570" alt="19" src="https://github.com/user-attachments/assets/1d1acf56-a5bd-40eb-8235-3cb3533c96e1" />

## Tarea 3: Acceder a la VM

1. Desde el **Portal de Azure** ingresa a **Máquinas Virtuales** y selecciona la máquina Virtual creada
2. ingresa a **Redes** > **Configuración de Red**, Verifica que el **Grupo de Seguridad de Red (NSG)** tenga una regla de entrada que admita la conexíon en el Puerto 22
<img width="768" alt="21" src="https://github.com/user-attachments/assets/045d37d0-155e-42e8-b46d-1fd1065ca1d5" />

3.  Selecciona **Conectar**, luego en **SSH mediante CLI de Azure** > Seleccionar, Configurar y Conectar
<img width="951" alt="22" src="https://github.com/user-attachments/assets/9ce48cfa-ffdd-4727-97ef-a8683ac6cd80" />

4.  Se abre una nueva CLI pidiendo conectar a la VM
<img width="406" alt="23" src="https://github.com/user-attachments/assets/bb2077c3-e21b-4588-a696-7dd00d8ca220" />

5.  Escribe **Exit** y presiona enter para salir de la VM, en el prompt de la CLI ejecuta el siguiente comando

   ```CLI
   az network public-ip list
   ```
<img width="287" alt="24" src="https://github.com/user-attachments/assets/3e57b137-1141-4afe-b9d3-ef75ad2ca893" />

7. Prueba la IP en el navegador para ver que solo sea accesible a través de SSH y no de HTTPS debido a NSG
   <img width="570" alt="25" src="https://github.com/user-attachments/assets/99460c2b-000a-4a3a-a6fb-2072252cd6ae" />




