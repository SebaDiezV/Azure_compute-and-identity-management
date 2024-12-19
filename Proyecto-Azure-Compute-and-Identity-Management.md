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



