# Proyecto 1 : Compute and Identity Management

## Temas Tratados
VMs, RBAC, Azure AD, Policies, Encryption, Cost Management

## Resumen
Este proyecto implica implementar una máquina virtual, protegerla mediante RBAC, aplicar políticas de Azure para la gobernanza de recursos, cifrar datos confidenciales y monitorear costos. Enseña conceptos básicos para administrar identidades, gobernanza y recursos informáticos de Azure.

## Escenario
Una empresa desea implementar una máquina virtual para su aplicación web, protegerla con control de acceso basado en roles, aplicar una política para convenciones de nombres, cifrar datos confidenciales y monitorear el costo de los recursos implementados.

## Diagrama
![project1-Diagram](https://github.com/user-attachments/assets/cf2a863c-c8f1-4c77-9186-9d2550d62a07)


## Tarea 1: Crear una Máquina Virtual

1. En el **Portal Azure** seleccionar **Máquinas Virtuales** > **Crear** - 
  
<img width="259" alt="01" src="https://github.com/user-attachments/assets/54ca4ff4-37bb-4493-8677-9037051a221d" /> -

<img width="178" alt="02" src="https://github.com/user-attachments/assets/2f27a447-a480-43b1-ba17-728565bd4a2f" />


2. Selecciona Subscripción, Grupo de Recursos, Región, tamaño de VM y so (Windows/Linux). Para este caso, se creará un nuevo Grupo de Recursos y se utilizará una VM Ubuntu.
<img width="489" alt="03-ub" src="https://github.com/user-attachments/assets/5e10bbd6-79e3-4960-81e8-9e81111849f2" />

3. Configura un nombre de usuario admin/contraseña o llave SSH. Se utilizará llave SSH
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

6. Vuelve al Almacén de Claves creado con anterioridad, ingresa a **Control de Acceso (IAM)** > **Agregar Asignación de Roles**
<img width="352" alt="11" src="https://github.com/user-attachments/assets/382f01fc-ff71-48d1-967b-2897cea2b135" />

7. Selecciona el Rol **Administrador de Key Vault** > Siguiente
<img width="258" alt="12" src="https://github.com/user-attachments/assets/177a19ca-64bc-4783-a3da-b20253725cd6" />

8. En la pestaña **Miembros**, en la opción **Asignar acceso a** Seleccionar **Usuario, Grupo...**, luego seleccionar **+ Seleccionar miembros** y buscar el grupo creado con anterioridad > Revisar y Asignar (puede tardar en propagarse)
<img width="236" alt="13" src="https://github.com/user-attachments/assets/50bc9095-5e7d-4641-a563-074d2ef0c03f" />

9.  Regresa al Almacén de Claves, selecciona **Objetos** > **Claves**, selecciona **+ Generar o Importar**
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
2. ingresa a **Redes** > **Configuración de Red**, Verifica que el **Grupo de Seguridad de Red (NSG)** tenga una regla de entrada que admita la conexión en el Puerto 22
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


## Tarea 4: Aplicar Directivas de Azure de Azure

1. Vuelve al **Portal de Azure** y busca **Directiva**
<img width="188" alt="26" src="https://github.com/user-attachments/assets/ae64527b-3e0e-4ac8-a175-f456031f5392" />

2. Selecciona **Creación** > **Definiciones**, selecciona **Definición de iniciativa**
<img width="451" alt="27" src="https://github.com/user-attachments/assets/e953ee72-783c-4e41-a4d4-042acc30afae" />

3. Selecciona la ubicación de la iniciativa, dale un nombre y categoría > Siguiente
<img width="492" alt="28" src="https://github.com/user-attachments/assets/8329e042-6dbe-4984-b18b-3860408f16ca" />

4. Selecciona **Agregar definiciones de directiva**, busca **Tag** y agrega algunas definiciones que empiecen por **Inherit a tag from resource group…** y **Inherit a tag from the suscription**
<img width="954" alt="29" src="https://github.com/user-attachments/assets/6c79d83b-38ab-41d4-87ca-040f11a66d9d" />
 
5. Repite el paso anterior, esta vez buscando **Allow** y selecciona **Allowed resource types**, selecciona agregar > Siguiente
<img width="587" alt="30" src="https://github.com/user-attachments/assets/52a4eb88-2780-496a-be9e-8cf7a457613d" />

6. Selecciona **Crear un grupo**, crear un grupo con el nombre **Tags**, Guardar
<img width="944" alt="31" src="https://github.com/user-attachments/assets/741cbcfe-6bdb-4734-ad47-93deac301461" />

7. Seleccionar **Anterior** para volver a la pestaña de **Directivas**, seleccionar directiva y seleccionar opción **Editar grupos**, seleccionar grupo **Tags** y guardar
<img width="850" alt="32" src="https://github.com/user-attachments/assets/6b928a06-568b-41a0-8b5d-753cec8796b1" />
<img width="348" alt="33" src="https://github.com/user-attachments/assets/35e2f375-0558-4ccd-b612-5dae53b23355" />

9. Seleccionar pestaña **Parámetros de Iniciativa**, seleccionar **Crear parámetro de iniciativa**, agregar Nombre para mostrar y Valores permitidos, guardar > Siguiente
<img width="336" alt="34" src="https://github.com/user-attachments/assets/2fef6773-32b8-4557-b614-b7fff163b07a" />

10. En la pestaña de **Parámetro de la directiva**, para las id's de referencia **Inherit a tag from resource group…** y **Inherit a tag from the suscription**, establecer el **Tipo de valor** como **Utiliceel parámetro de la iniciativa** y en **Valores** seleccionar el **Parámetro de iniciativa** creado en el punto anterior. Para el id de referencia **Tipos de recursos permitidos**, en tipo de valor seleccionar **Establecer valor** y en **Valores** seleccionar **VirtualMachines** > Revisar y Crear > Crear
<img width="761" alt="35" src="https://github.com/user-attachments/assets/ab933e2e-abcf-4184-ab8c-ba365118391e" />

11. Asignar la iniciativa a nivel de Suscripción, seleccionar **Creación** > **Asignaciones** > **Asignar iniciativa**
<img width="403" alt="36" src="https://github.com/user-attachments/assets/f1f81726-f4a5-4f0c-9e85-9a11fd1670ef" />

12. En **Ámbito** seleccionar la suscripción, en **Definición de iniciativa** seleccionar la definición creada y dar un nombre de asignación
<img width="668" alt="37" src="https://github.com/user-attachments/assets/63903e49-4019-42d9-8e50-969bb3ef2467" />

13. Seleccionar pestaña **Mensajes de no cumplimiento**, agregar un mensaje de no cumplimiento predeterminado y seleccionar la definición de directiva para asociar el mensaje > Revisar y crear > Crear
<img width="654" alt="38" src="https://github.com/user-attachments/assets/41eeaec1-3e65-4168-83c2-e5e54341c800" />

14. Para revisar asignación de Directiva, en el Portal de Azure, seleccionar Máquinas Virtuales y la VM creada, seleccionar **Operaciones** > **Directivas**
<img width="829" alt="39" src="https://github.com/user-attachments/assets/5bb727ae-c417-4045-b69c-851228adba66" />

## Tarea 5: Monitorear y administrar costos

1. en el **Portal de Azure** seleccionar **Administración de Costos + facturación**
<img width="292" alt="40" src="https://github.com/user-attachments/assets/5923c3c9-f42e-4b1b-b670-f04618abc18c" />

2.  Seleccionar **Cost Management** > **Presupuestos** > **+ Agregar**
<img width="392" alt="41" src="https://github.com/user-attachments/assets/413f9825-f2f4-43d4-b876-8e7e0a0447ec" />

3.  Establece un límite de gasto mensual para la suscripción, agrega un nombre para el presupuesto, período de restablecimiento y fecha de expiración
<img width="399" alt="42" src="https://github.com/user-attachments/assets/fc2baad3-36d3-4954-b2e9-971f3242f8d7" />

4.  Configura notificaciones por correo electrónico o grupos de acciones para los umbrales (p. ej., 50 %, 80 %, 100 %).
<img width="388" alt="43" src="https://github.com/user-attachments/assets/f014ca85-176c-45eb-a1dc-6599c5387f4e" />

5.  Revise el análisis de costos para identificar tendencias y optimizar el gasto. Revise las recomendaciones del asesor para ver las recomendaciones de costos.

## Limpia tus recursos

Si está trabajando con **su propia suscripción**, tómese un minuto para eliminar los recursos del laboratorio. Esto garantizará que se liberen recursos y se minimicen los costos. La manera más sencilla de eliminar los recursos de laboratorio es eliminar el grupo de recursos de laboratorio. 
