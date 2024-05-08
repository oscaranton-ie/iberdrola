**SALESFORCE Data Cloud (CDP)**

**BluePrint**

Histórico de versiones:

+--------+-----------+-------------------------------------------------+
| V      | Fecha     | Resumen de cambios                              |
| ersión |           |                                                 |
+========+===========+=================================================+
| 1.0    | 2         | Versión inicial del documento                   |
|        | 2/05/2023 |                                                 |
+--------+-----------+-------------------------------------------------+
| 1.1    |           | Modificados casos de uso                        |
|        |           |                                                 |
|        |           | Modificado el diseño del caso de uso 5          |
|        |           |                                                 |
|        |           | Formato fechas y horas                          |
|        |           |                                                 |
|        |           | Mappings modificados                            |
|        |           |                                                 |
|        |           | Data Streams modificados                        |
|        |           |                                                 |
|        |           | Añadido naming convention                       |
|        |           |                                                 |
|        |           | Modelo de datos Data Cloud                      |
|        |           |                                                 |
|        |           | Tipo de categoría Engagement en Consents        |
|        |           |                                                 |
|        |           | Añadido más detalle sobre las reglas de         |
|        |           | unificación                                     |
|        |           |                                                 |
|        |           | Añadido nuevo apartado de gestión de            |
|        |           | consentimientos                                 |
+--------+-----------+-------------------------------------------------+
| 1.2    |           | Modificados Acuerdos de interfaz                |
|        |           |                                                 |
|        |           | Modificados mappings                            |
|        |           |                                                 |
|        |           | Modificado Segmentación                         |
|        |           |                                                 |
|        |           | Añadido detalle sobre la unificación de         |
|        |           | perfiles                                        |
+--------+-----------+-------------------------------------------------+
| 2.0    | 0         |                                                 |
|        | 4/10/2023 |                                                 |
+--------+-----------+-------------------------------------------------+

Control de la preparación, revisión y aprobación:

  ------------------------------------------------------------------------
               Nombre           Rol                       Fecha
  ------------ ---------------- ------------------------- ----------------
  Autor        David Tuñón      Ingeniero de datos CDP    22/05/2023

  Revisor      Sergio Ruiz      Arquitectura Salesforce   

                                                          
  ------------------------------------------------------------------------

**Índice:**

[[Introducción y objetivo del
documento]{.underline}](#introducción-y-objetivo-del-documento)

[[Descripción de Salesforce Data
Cloud]{.underline}](#descripción-de-salesforce-data-cloud)

> [[Conectar datos]{.underline}](#conectar-datos)
>
> [[Ingestar datos]{.underline}](#ingestar-datos)
>
> [[Preparar y modelar datos]{.underline}](#preparar-y-modelar-datos)
>
> [[Mapeo de datos (Data
> Mapping)]{.underline}](#mapeo-de-datos-data-mapping)
>
> [[Unificar fuentes (Unified Source
> Profiles)]{.underline}](#unificar-fuentes-unified-source-profiles)
>
> [[Enriquecer datos (Calculated
> Insights)]{.underline}](#enriquecer-datos-calculated-insights)
>
> [[Crear y activar segmentos]{.underline}](#crear-y-activar-segmentos)
>
> [[Actuar en los datos (Data
> Action)]{.underline}](#actuar-en-los-datos-data-action)

[[Arquitectura de las
integraciones]{.underline}](#arquitectura-de-las-integraciones)

[[Diseño funcional]{.underline}](#diseño-funcional)

[[Diseño técnico]{.underline}](#diseño-técnico)

> [[Data Streams]{.underline}](#data-streams)
>
> [[Cliente (SFTP)]{.underline}](#cliente-sftp)
>
> [[Contrato (SFTP)]{.underline}](#contrato-sftp)
>
> [[Servicio (SFTP)]{.underline}](#servicio-sftp)
>
> [[Consentimientos (SFTP)]{.underline}](#consentimientos-sftp)
>
> [[CATs (SFTP)]{.underline}](#cats-sftp)
>
> [[Delio (SFTP)]{.underline}](#delio-sftp)
>
> [[Interaction Studio (IS
> Connector)]{.underline}](#interaction-studio-is-connector)
>
> [[Marketing Cloud (MKTC
> Connector)]{.underline}](#marketing-cloud-mktc-connector)
>
> [[Mappings]{.underline}](#mappings)
>
> [[Cliente]{.underline}](#cliente)
>
> [[Contrato]{.underline}](#contrato)
>
> [[Servicio]{.underline}](#servicio)
>
> [[Consentimientos]{.underline}](#consentimientos)
>
> [[CATs]{.underline}](#cats)
>
> [[Delio]{.underline}](#delio)
>
> [[Gestión de
> Consentimientos]{.underline}](#gestión-de-consentimientos)
>
> [[Reglas de unificación]{.underline}](#reglas-de-unificación)
>
> [[Calculated Insights]{.underline}](#calculated-insights)
>
> [[Segmentos]{.underline}](#segmentos)
>
> [[Activaciones]{.underline}](#activaciones)
>
> [[Marketing Cloud]{.underline}](#marketing-cloud)
>
> [[Interaction Studio]{.underline}](#interaction-studio)

[[Naming Convention]{.underline}](#naming-convention)

[[Pruebas de carga]{.underline}](#pruebas-de-carga)

[[Borrado de datos en CDP]{.underline}](#borrado-de-datos-en-cdp)

[[Glosario de términos]{.underline}](#glosario-de-términos)

# Introducción y objetivo del documento

En este documento se va a detallar funcional y técnicamente cómo se
construirán los 9 casos de uso acordados en el alcance de este proyecto
GAIA con la herramienta de salesforce Data Cloud.

Con el fin de que se entienda el diseño técnico de este proyecto, se
hará una descripción de la plataforma Salesforce Data Cloud de cara a
entender las capacidades y funcionalidades que posee Data Cloud.

Se determinará para qué casos de uso es necesario utilizar el CDP, ya
que para los 9 no es necesario, además habrá integración de datos de
Salesforce Marketing Cloud o Interaction Studio que deberá incluirse en
el alcance aunque no se utilicen para algún caso de uso, pero es posible
que dicha información se quiera consumir vía API Rest por otros
sistemas.

Se detallará desde donde, como y cuando viaja la información de cada
caso de uso y por último se diseñará técnicamente la configuración de la
herramienta de CDP para acometer estos requisitos funcionales.

# Descripción de Salesforce Data Cloud

Salesforce Data Cloud es una solución de Plataforma de Datos del Cliente
(Customer Data Platform, CDP) diseñada para ofrecer una vista unificada
y en tiempo real de los clientes. Como parte integral del ecosistema de
Salesforce, esta herramienta ayudará a Iberdrola a recoger, unificar,
segmentar y activar los datos de sus clientes.

Ventajas de utilizar Salesforce Data Cloud:

1.  **Unificación de datos**: Salesforce Data Cloud permite a las
    empresas combinar datos de múltiples fuentes para obtener una visión
    360 grados de los clientes. Los datos pueden provenir de diversas
    fuentes, incluyendo un CRM construido adhoc (ya que podemos ingestar
    datos desde ficheros), sistemas ERP, redes sociales, sitios web, y
    más.

2.  **Segmentación avanzada**: Con Salesforce Data Cloud,Iberdrola puede
    segmentar los datos de sus clientes basándose en diversos criterios,
    como el comportamiento, las preferencias, el historial de
    contrataciones, etc.

3.  **Insights en tiempo real**: Gracias a la capacidad de procesamiento
    en tiempo real, Iberdrola puede obtener insights instantáneos sobre
    sus clientes y adaptar sus estrategias de marketing y ventas de
    manera oportuna.

4.  **Integración con otras herramientas de Salesforce**: Salesforce
    Data Cloud se integra perfectamente con otras soluciones de
    Salesforce, como Sales Cloud, Marketing Cloud, y Service Cloud. Esto
    permite a Iberdrola aprovechar al máximo sus datos y mejorar la
    eficacia de sus operaciones. En siguientes casos de uso se podrá
    integrar con Salesforce Sales para todo el modelo de oportunidades y
    ventas que se desplegará en esta herramienta.

A continuación vamos a detallar con que funcionalidades dentro de la
herramienta se puede construir un flujo de trabajo desde el dato
ingestado hasta la activación en una plataforma de marketing:

![](media/image20.png){width="6.267716535433071in"
height="1.8888888888888888in"}

Es importante determinar los tipos de datos que se quieren usar para la
unificación y segmentación de clientes. Al hacerlo, hay que considerar
qué casos de uso necesitarán ser en **tiempo real**. Un anti-patrón es
asumir automatizar todo desde Salesforce CDP. Sin embargo, Salesforce
CDP no admite la ingesta en tiempo real a menos que uses la API de
Ingesta o la activación en tiempo real, por lo que es muy importante
determinar por qué y cuándo se necesita esta automatización en tiempo
real. Por ejemplo, si se quiere enviar un correo electrónico tan pronto
como se realice una contratación, entonces es mejor enviar los datos de
la transacción directamente a Marketing Cloud, en lugar de enviarlos
primero a CDP y luego esperar que los datos lleguen a Marketing Cloud de
inmediato.

También es importante identificar el **identificador único** a nivel de
Iberdrola que se planea usar para crear tus perfiles unificados. Esta es
la ID contra la que se resolverán todos los perfiles. Esto es
importante, ya que Salesforce CDP no creará un identificador único
inmutable a nivel de Iberdrola por sí solo. En nuestro caso será el
cod_cliente.

También es importante tener una **clave primaria definida** para cada
objeto de fuente de datos. La clave primaria permite que Salesforce CDP
identifique de manera única un registro. Salesforce CDP no genera
automáticamente claves primarias; espera que estas claves estén
presentes en los datos de origen como un atributo. Puedes usar campos de
fórmula para crear claves primarias para aquellos objetos en los datos
de origen que no tienen una clave primaria establecida como atributo y
establecerla como la clave primaria para cada uno de estos objetos.

## Conectar datos

Los conectores son flujos de datos especializados (Data Streams) que se
comunican con fuentes externas para transmitir los datos de dichas
fuentes a un objeto fuente de Data Cloud (DSO), y a continuación se
mapea contra un objeto del data lake (DLO), donde se almacena este raw
data.

Data Cloud tiene conectores para: Marketing Cloud Email Studio,
MobileConnect, MobilePush, Marketing Cloud Data Extensions, para
Salesforce CRM, Ingestion API, Interaction Studio y para datos fuera de
Salesforce a través de proveedores de almacenamiento en la nube como AWS
S3.

Los ficheros que acepta son CSV y TSV o comprimidos como GZ y ZIP.

Importante: Se deben utilizar caracteres ASCII en los campos de texto,
como el nombre de archivo, el wildcard, el nombre de directorio o el
editor de sintaxis de fórmulas. UTF-8 a veces contiene caracteres que no
son ASCII y no se garantiza que funcionen correctamente. Además, no se
debe usar : (dos puntos) y \' (comillas simples) en nombres de archivos
y fórmulas.

## Ingestar datos

Customer Data Platform reconoce la gestión de datos como dos fases
diferentes: ingesta de datos y modelado de datos. En la primera fase de
ingesta de datos de Data Cloud, hay que introducir los datos desde
diferentes orígenes definiendo **Data Streams**. En el consumo de datos,
los datos de origen se obtienen tal cual, lo que significa que los
campos y sus tipos de datos se importan sin transformación. Para ampliar
el esquema se puede introducir cálculos basados en filas para limpiar
campos de origen o construir campos adicionales. La definición de
esquema resultante se denomina el Objeto de origen de datos (DSO). Un
DSO se rellena con datos de acuerdo con una programación en una
transmisión de datos. Además esto se modela contra un DLO.

El **esquema de datos de entrada** de un Data Stream contiene:

-   **Etiqueta de encabezado**: Cada columna de encabezado de origen
    identificada en los datos sin procesar. No es modificable en diseño,
    así garantizamos que podemos hacer referencias cruzadas con el
    origen.

-   **Etiqueta de campo**: Un nombre de visualización de Data Cloud
    propuesto para cada columna de encabezado de origen.

-   **Nombre API de campo**: Un nombre de API propuesto de Data Cloud
    para cada columna de encabezado de origen.

-   **Tipo de datos**: Un tipo de datos sugerido para cada columna de
    encabezado de origen. Data Cloud admite cuatro tipos de datos al
    consumir el objeto de origen de datos (DSO): texto, número, fecha y
    fecha/hora. No se pueden cambiar los tipos de datos después de crear
    el DSO.

Además se puede definir el esquema de origen proporcionando una
**categoría de datos**:

-   Tipo **Profile**. Son datos de tipo perfil de cliente donde podrás
    seleccionar cual es la primary key, el campo de modificación de
    record (fecha y hora de la última modificación de ese registro para
    hacer cargas incrementales)o el identificador de la unidad de
    organización.

-   Tipo **Engagement**. Datos de comportamiento del usuario donde
    puedes seleccionar los anteriores campos de Profile y además un
    campo de fecha y hora del evento.

-   Tipo **Other**. Otros datos que no sean de los anteriores dos donde
    podrás seleccionar los campos al igual que los de tipo Profile.

En este punto además se deben configurar los **horarios de
actualización** apropiados. Nota: Por defecto, se realiza una
actualización completa para las extensiones de datos (como Marketing
Cloud) una vez cada 24 horas. En la configuración, la frecuencia de
\"cada hora\" se refiere a la frecuencia con la que Salesforce CDP busca
datos incrementales en la fuente, pero la fuente envía una actualización
completa solo una vez cada 24 horas, como resultado, puede tardar hasta
24 horas para que los datos iniciales aparezcan en Salesforce CDP.

Durante la configuración de SFTP, se deberá seleccionar la opción de
**actualizar solo los nuevos archivos**. Los archivos deben incluir el
encabezado para que el sistema pueda leer el esquema del archivo
correctamente. Además, se pueden configurar alertas para ser notificado
sobre archivos faltantes, para que se notifique si hay un problema con
un archivo que falta durante una actualización programada del flujo de
datos.

Por defecto, las **fechas/horas** ingeridas se interpretan como si
estuvieran en **UTC**. Salesforce CDP localiza todas las fechas/marcas
de tiempo usando la zona horaria configurada para la organización. Si
los datos de origen no están en UTC, el enfoque recomendado es incluir
la zona horaria en los datos ingeridos, y asegurarte de que se
selecciona el formato de fecha correcto en la configuración del flujo de
datos.

Al traer **registros de perfil** durante la ingesta, se debe tener en
cuenta que deben ser sobrescritos en su totalidad. Esto significa que
cada vez que cualquier valor en el registro necesita ser actualizado, el
registro completo debe ser enviado nuevamente. Por ejemplo, el campo de
Lifetime Value podría cambiar regularmente para un cliente, por lo que
no juntar esto con la información principal del perfil como su nombre y
fecha de nacimiento, que nunca cambian. Una buena práctica es dividir
los valores estáticos y dinámicos en flujos de datos separados.

## Preparar y modelar datos

Durante el modelado de datos, los esquemas de origen conservados se
integran en un modelo de datos ampliable.

El modelo de datos tiene tecnología del Modelo de datos de Customer 360,
a través de un objeto de origen de datos (DSO y DLOs) en la experiencia
de relaciones y asignación de objeto de modelo de datos (DMO). La
información sobre una persona particular se almacena en el DMO de
Unified Individual, permitiendo a los usuarios de la herramienta crear
una estrategia de segmentación y publicar segmentos en destinos de
activación.

En este punto es donde debemos hacer las **transformaciones necesarias**
entre los objetos del data lake de entrada de data streams, los cuales
llamamos DSO, y los DLO objetivos para mapear contra los DMOs (Objetos
estándar del modelo customer 360 o personalizados).

![](media/image13.png){width="6.267716535433071in"
height="1.3333333333333333in"}

Estas transformaciones nos permiten crear nuevos campos en el DLO de
destino con el fin de calcular valores en base a los datos de entrada.

Las funciones de transformación soportadas se puede ver en la
documentación de salesforce: [[Supported Library Functions
(salesforce.com)]{.underline}](https://help.salesforce.com/s/articleView?language=en_US&id=sf.c360_a_supported_library_functions.htm&type=5).
Además los operadores disponibles también se pueden consultar en
[[Supported Library Operators
(salesforce.com)]{.underline}](https://help.salesforce.com/s/articleView?id=sf.c360_a_supported_library_operators.htm&type=5).

El modelo de datos estándar de Data Cloud es normalizado, por lo que los
datos de entrada deben ser normalizados antes de mapearse contra este.
Ejemplo de datos normalizados:

![](media/image6.png){width="6.267716535433071in"
height="3.0833333333333335in"}

Ejemplo de datos no normalizados, en una tabla en la que te vienen todos
los datos agregados para un customer Id:

![](media/image11.png){width="6.267716535433071in"
height="1.4583333333333333in"}

Un caso de uso claro para el que normalizar datos de entrada es si en el
mismo registro de entrada para un cliente te informan varios campos de
email o teléfono, esto hay que normalizarlo en tablas separadas para que
el modelo de datos de Data Cloud lo acepte, puesto que el objeto
estándar Contact Point Email o Contact Point Phone no acepta varios a la
vez.

Entonces como recomendación en la implementación de carga de datos en
Data Cloud, es preferible que vengan normalizados. Por lo tanto si a
futuro se requiere que los ficheros de cliente traigan más de un dato de
contacto se debería normalizar la entrada. Para la implementación actual
y como están definidos los acuerdos de interfaz que más adelante veremos
no hace falta normalizar ninguna fuente de entrada.

## Mapeo de datos (Data Mapping)

Los datos ingeridos por todos los flujos de datos (Data Streams) se
escriben en objetos de lago de datos (DLO). Después de crear sus flujos
de datos, debe asociar sus DLO a objetos de modelo de datos (DMO). Solo
los campos mapeados y los objetos con relaciones se pueden usar para
Segmentación y Activación.

Las asignaciones y las relaciones se configuran automáticamente solo
para flujos de datos que se implementan a través de paquetes de datos
estándar. Sin embargo, debe asegurarse de que las asignaciones y
relaciones requeridas estén configuradas correctamente para flujos de
datos personalizados.

Premisas y consideraciones a tener en cuenta en los mapeos de datos:

-   Cuando se mapea un DMO de la categoría Perfil u Otro, debes asignar
    el campo de clave primaria. Solo se puede guardar la asignación de
    datos después de asignar la clave primaria.

-   Cuando se mapea un DMO de la categoría Comportamiento, debes asignar
    el campo de clave primaria y fecha y hora del evento. Solo se puede
    guardar la asignación de datos después de asignar ambos campos.

-   Para utilizar la resolución de identidades, segmentación y
    activación, asigne los campos y las relaciones necesarias para los
    datos del Party Area. Se puede usar el diagrama de relaciones de
    entidades (ERD) para el Party Area de cara a planificar la
    asignación. También se debe mapear el objeto Individual y un Punto
    de contacto o el objeto Identificación de parte.

-   Los valores para el campo individual.Id deben ser únicos en todas
    las fuentes de datos que se asignan a la DMO individual. En el caso
    de Iberdrola este será el cod_cliente.

-   La referencia al objeto Individual en otros objetos dentro del
    modelo de datos se realiza a través del atributo Party. Piense en
    ello como una relación de clave externa que se vincula a un atributo
    Individual.Id. En el diagrama de relación de la entidad Party, puede
    ver que cada objeto \<canal\> del Punto de Contacto junto con la
    Identificación de la Parte tiene esa referencia. Si bien en algunos
    casos puede ser confuso, cuando se trata de atributos, Party =
    Individual.Id.

-   Hay que asignar al menos un atributo de punto de contacto \<canal\>
    para permitir que funcionen los procesos de unificación y
    activación. Un objeto de punto de contacto \<canal\> contiene
    detalles sobre una persona, como la dirección de correo electrónico
    o el número de teléfono. Sin el punto de contacto, la activación no
    es factible porque no hay un detalle de contacto a través del cual
    interactuar con los clientes. Importante en los mapeos de datos
    asignar el atributo Party y la primary key del objeto contact point
    con la primary key del objeto de entrada DLO.

-   El objeto Identificación de la parte representa conjuntos de
    identificadores de terceros para una persona, como un número de
    tarjeta de fidelización, número de identificación personal, número
    de licencia de conducir u otros puntos de datos similares que pueden
    asociar numerosos registros de clientes. Para los mismos valores de
    Tipo de identificación de parte y Nombre de identificación, Data
    Cloud compara los registros con el mismo Número de identificación.
    En principio para este proyecto el objeto Party Identification Id no
    hace falta utilizarlo.

Importante destacar que se pueden crear relaciones entre objetos
seleccionando la cardinalidad de las mismas. Se pueden crear entre
objetos estandar, entre estandar y personalizados y entre personalizados
solo.

## Unificar fuentes (Unified Source Profiles)

Se usa esta funcionalidad para consolidar datos de diferentes fuentes en
una vista única de cliente llamado perfil único. La resolución de
identidad utiliza reglas de coincidencia y reconciliación para vincular
datos sobre individuos en perfiles unificados. Cada perfil unificado
contiene todos los valores únicos de punto de contacto de todas las
fuentes.

Hay que configurar los conjuntos de reglas de resolución de identidad
después de mapear los datos de origen a los objetos del modelo de datos
(DMO). El mapeo debe completarse antes de crear conjuntos de reglas
(Rulesets), estos conjuntos de reglas contienen reglas de coincidencia y
reconciliación que especifican cómo vincular múltiples fuentes de datos
en un perfil unificado.

El trabajo de conjunto de reglas (Ruleset Job) procesa los perfiles de
origen de acuerdo con las reglas de mapeo, coincidencia y reconciliación
que se configuran para crear y actualizar tus perfiles unificados. El
trabajo se ejecuta al menos una vez al día para cada conjunto de reglas,
a menos que no detecte cambios en los datos de origen o en la
configuración del conjunto de reglas. Si nada ha cambiado, se omite el
trabajo del conjunto de reglas.

Data Cloud procesa los registros de cada fuente de datos ingerida para
alimentar los **objetos individuales**. Estos objetos individuales están
relacionados con los datos del perfil del cliente. Cuando el proceso de
resolución de identidad crea los objetos unificados, los vincula a los
objetos individuales de origen y a la información relacionada con los
registros de origen.

Un **objeto individual unificado** consolida información sobre el
cliente, como el nombre, en objetos individuales unificados creados por
el proceso de resolución de identidad. La unificación se determina en
base a las configuraciones del conjunto de reglas. El objeto individual
unificado reconcilia todos los valores de campo para su uso en segmentos
y activaciones.

Data Cloud crea **objetos de punto de contacto unificados** para agregar
puntos de contacto de diferentes fuentes de datos, basados en mapeos y
reglas en cada conjunto de reglas. Los objetos de punto de contacto
unificados retienen todos los puntos de contacto únicos para un
individuo unificado.

Data Cloud crea **objetos de enlace unificad**o para servir como el
enlace entre los datos de origen y los objetos unificados. Los objetos
de enlace unificado te permiten ver los datos de origen de cada perfil
unificado y son necesarios para crear insights calculados para perfiles
unificados.

Para que el la resolución de identidad funcione correctamente se deben
tener los siguientes mapeos en el modelado de datos:

+--------------+---------------+---------------+----------------------+
| **OBJECT     | **OBJECT API  | **FIELD       | **REQUIRED MAPPING** |
| LABEL**      | NAME**        | LABELS**      |                      |
+==============+===============+===============+======================+
| **           | ssot\_\_Indi  | Individual Id | Individual.ID →      |
| Individual** | vidual\_\_dlm | (primary key) | Conta                |
|              |               |               | ctPointAddress.Party |
|              |               | First Name    |                      |
|              |               |               | Individual.ID →      |
|              |               | Last Name     | C                    |
|              |               |               | ontactPointApp.Party |
|              |               |               |                      |
|              |               |               | Individual.ID →      |
|              |               |               | Con                  |
|              |               |               | tactPointEmail.Party |
|              |               |               |                      |
|              |               |               | Individual.ID →      |
|              |               |               | Con                  |
|              |               |               | tactPointPhone.Party |
|              |               |               |                      |
|              |               |               | Individual.ID →      |
|              |               |               | PartyId              |
|              |               |               | entificationId.Party |
+--------------+---------------+---------------+----------------------+
| **Contact    | ssot\_\_      | Contact Point | Conta                |
| Point        | ContactPointA | Address Id    | ctPointAddress.Party |
| Address**    | ddress\_\_dlm | (primary key) | → Individual.ID      |
|              |               |               |                      |
|              |               | Address Line  |                      |
|              |               | 1             |                      |
|              |               |               |                      |
|              |               | City          |                      |
|              |               |               |                      |
|              |               | Party         |                      |
|              |               |               |                      |
|              |               | Postal Code   |                      |
|              |               |               |                      |
|              |               | State         |                      |
|              |               | Province      |                      |
+--------------+---------------+---------------+----------------------+
| **Contact    | ssot          | Contact Point | C                    |
| Point App**  | \_\_ContactPo | App Id        | ontactPointApp.Party |
|              | intApp\_\_dlm | (primary key) | → Individual.ID      |
+--------------+---------------+---------------+----------------------+
| **Contact    | ssot\_        | Contact Point | Con                  |
| Point        | \_ContactPoin | Email Id      | tactPointEmail.Party |
| Email**      | tEmail\_\_dlm | (primary key) | → Individual.ID      |
|              |               |               |                      |
|              |               | Party         |                      |
|              |               |               |                      |
|              |               | Email Address |                      |
+--------------+---------------+---------------+----------------------+
| **Contact    | ssot\_        | Contact Point | Con                  |
| Point        | \_ContactPoin | Phone Id      | tactPointPhone.Party |
| Phone**      | tPhone\_\_dlm | (primary key) | → Individual.ID      |
|              |               |               |                      |
|              |               | Formatted     |                      |
|              |               | E164 Phone    |                      |
|              |               | Number        |                      |
|              |               |               |                      |
|              |               | Party         |                      |
+--------------+---------------+---------------+----------------------+
| **Party      | ssot\_\_      | Party         | PartyId              |
| Iden         | PartyIdentifi | I             | entificationId.Party |
| tification** | cation\_\_dlm | dentification | → Individual.ID      |
|              |               | Id (primary   |                      |
|              |               | key)          |                      |
|              |               |               |                      |
|              |               | I             |                      |
|              |               | dentification |                      |
|              |               | Name          |                      |
|              |               |               |                      |
|              |               | I             |                      |
|              |               | dentification |                      |
|              |               | Number        |                      |
|              |               |               |                      |
|              |               | Party         |                      |
|              |               |               |                      |
|              |               | Party         |                      |
|              |               | I             |                      |
|              |               | dentification |                      |
|              |               | Type          |                      |
+--------------+---------------+---------------+----------------------+

Las **reglas de conciliación** son del tipo:

-   **Última actualización**. Esta regla especifica que se debe
    seleccionar el valor del registro actualizado más recientemente para
    incluirlo en el perfil unificado. Esta regla de conciliación sólo
    está disponible cuando el campo Fecha de última modificación se
    asigna desde el flujo de datos.

-   **Más frecuente**. Esta regla especifica que se debe seleccionar el
    valor que ocurre con más frecuencia para incluirlo en el perfil
    unificado. Si se selecciona Ignorar valores vacíos, el proceso de
    resolución de identidad no selecciona valores nulos, incluso si nulo
    es el valor que aparece con más frecuencia para el campo.

-   **Prioridad de fuente**. Esta regla ordena los objetos del lago de
    datos en orden de mayor a menor preferencia para su inclusión en el
    perfil unificado. Para estabilizar los valores de ID, hay que
    utilizar la regla de conciliación de Prioridad de origen en los
    campos de ID. Si se selecciona Ignorar valores vacíos, el proceso de
    resolución de identidad selecciona el valor no nulo de mayor
    prioridad.

Las reglas de conciliación no se aplican a puntos de contacto como el
correo electrónico o el número de teléfono. Todos los puntos de contacto
siguen siendo parte de un perfil unificado, por lo que todos los puntos
de contacto están disponibles al crear activaciones para segmentos en un
perfil unificado. Utilice el orden de prioridad de la fuente en las
activaciones para asegurarse de entregar un punto de contacto desde la
fuente deseada hasta su objetivo de activación.

Las **reglas de matcheo o coincidencia (match rules)** le indican a Data
Cloud qué perfiles unificar durante el proceso de resolución de
identidad.

Se pueden usar reglas de coincidencia para vincular varios registros en
un individuo unificado. Las reglas de coincidencia te permiten
especificar las condiciones en las que se aplica la unificación.

Las reglas de coincidencia por defecto son:

+-----------------------------+----------------------------------------+
| **NOMBRE DE LA REGLA DE     | **CRITERIO DE COINCIDENCIA**           |
| COINCIDENCIA**              |                                        |
+=============================+========================================+
| Fuzzy Name and Normalized   | -   Individual First Name (Fuzzy -     |
| Email                       |     Medium Precision)                  |
|                             |                                        |
|                             | -   Individual Last Name (Exact)       |
|                             |                                        |
|                             | -   Contact Point Email Email Address  |
|                             |     (Exact Normalized)                 |
+-----------------------------+----------------------------------------+
| Fuzzy Name and Normalized   | -   Individual First Name (Fuzzy -     |
| Phone                       |     Medium Precision)                  |
|                             |                                        |
|                             | -   Individual Last Name (Exact)       |
|                             |                                        |
|                             | -   Contact Point Phone Formatted E164 |
|                             |     Phone Number (Exact Normalized)    |
+-----------------------------+----------------------------------------+
| Fuzzy Name and Normalized   | -   Individual First Name (Fuzzy -     |
| Address                     |     Medium Precision)                  |
|                             |                                        |
|                             | -   Individual Last Name (Exact)       |
|                             |                                        |
|                             | -   Contact Point Address Address Line |
|                             |     1 (Exact Normalized)               |
|                             |                                        |
|                             | -   Contact Point Address City (Exact) |
|                             |                                        |
|                             | -   Contact Point Address Country      |
|                             |     (Exact Normalized)                 |
+-----------------------------+----------------------------------------+
| Fuzzy Name and Normalized   | -   Individual First Name (Fuzzy -     |
| Phone and Normalized Email  |     Medium Precision)                  |
|                             |                                        |
|                             | -   Individual Last Name (Exact)       |
|                             |                                        |
|                             | -   Contact Point Phone Formatted E164 |
|                             |     Phone Number (Exact Normalized)    |
|                             |                                        |
|                             | -   Contact Point Email Email Address  |
|                             |     (Exact Normalized)                 |
+-----------------------------+----------------------------------------+

## Enriquecer datos (Calculated Insights)

Se puede usar un calculated insights para definir y calcular métricas
multidimensionales en todo su estado digital en Data Cloud. Se pueden
crear métricas a nivel de perfil, segmento y población. Por ejemplo, se
puede evaluar el rendimiento a nivel de canal o usar métricas de
productos para comprender el comportamiento de compra y navegación.

Se pueden crear usando ANSI SQL o desde un Salesforce package.

  -----------------------------------------------------------------------
  SELECT \<Attributes\>, \<Aggregation\[\_Measures\_\]\>\
  FROM \<Data Model Object\>\
  JOIN \[Inner \| Left \| Right \| Full\] \<Data Model Object\>
  \[Optional\]\
  WHERE \<predicate on rows\> \[Optional\]\
  GROUP BY \<columns\[\_Dimensions\_\]\>
  -----------------------------------------------------------------------

  -----------------------------------------------------------------------

## Crear y activar segmentos

Con Data Cloud se pueden crear objetivos de activación y activar
segmentos de datos.

Un **objetivo de activación (Activation Target)** es la ubicación,
incluida la información de autenticación y autorización, donde se envían
los datos de un segmento durante la activación (por ejemplo, Marketing
Cloud).

La **segmentación** es para desglosar sus datos en segmentos útiles para
comprender, orientar y analizar a sus clientes. Se pueden publicar en un
horario elegido o según sea necesario. Ya sean básicos o complejos, los
segmentos son cruciales para usar Data Cloud.

La **activación** es el proceso que materializa y publica un segmento en
las plataformas de activación. Un objetivo de activación se utiliza para
almacenar información de autenticación y autorización para una
plataforma de activación determinada. Cuando publicas los segmentos, se
pueden incluir puntos de contacto y atributos adicionales a los
objetivos de activación.

Se pueden crear objetivos de activación en Data Cloud para publicar
segmentos en un SFTP, por lo que se pueden activar estos sin mapear
datos de contacto.

Señalamos una serie de conceptos clave sobre la segmentación:

-   Segment On. Define la entidad de destino (objeto) utilizada para
    crear el segmento. Por ejemplo, se puede crear un segmento en
    Individuo unificado (B2C) o Cuenta (B2B). Puede elegir cualquier
    entidad marcada como tipo Perfil durante la ingesta.

-   Publicar un segmento en Data Cloud. Después de personalizar un
    segmento en Data Cloud, se puede publicar en los objetivos de
    activación cuando se desee o según un cronograma cuando se creó el
    segmento. El horario de publicación estándar es de 12 o 24 horas.
    También puede cambiar el cronograma del segmento editando sus
    propiedades. Si se publican demasiados segmentos simultáneamente,
    Data Cloud espera 30 minutos y comienza a publicar sus segmentos
    nuevamente.

## Actuar en los datos (Data Action)

Con Data cloud puedes actuar ante eventos en tiempo real, por ejemplo me
llega un evento de carrito abandonado, mandamos un sms.

Un **objetivo de acción de datos** es donde se activan los resultados de
una acción de datos. Se pueden enviar acciones de datos a destinos para
impulsar la orquestación basada en eventos. Por ejemplo, un cliente
contrató un contrato de luz que suele llevar asociado un servicio de
smart home de interés y navegó por estas páginas en la web pública de
Iberdrola. Se puede enviar al cliente un correo electrónico con una
campaña promocional sobre Smart Home.

Los destinos u objetivos de activación soportados son:

-   Salesforce Platform Event---Send. Enviar una acción de datos al bus
    de eventos central para crear una aplicación de flujo basada en
    información casi en tiempo real generada en Data Cloud.

-   Webhook. Un webhook es un tipo de API que se basa en eventos en
    lugar de en solicitudes.

-   Marketing Cloud. Escenarios basados en información y eventos con las
    funciones de journies y mensajería de Marketing Cloud.

# Arquitectura de las integraciones

Como de momento no se necesitan activaciones en tiempo real,
utilizaremos la ingesta de datos de cliente mediante ficheros CSV
depositados en un SFTP.

Los datos de comportamiento web/App se integrarán en Salesforce Data
Cloud en primera instancia desde el origen de CATs (Aunque los procesa y
deposita en un fichero CSV en el SFTP un proceso Batch), y más adelante
cuando esté implantado desde Salesforce Interaction Studio.

Los datos de comportamiento email y push se integrarán desde la fuente
de Marketing Cloud.

En el siguiente diagrama se ven los sistemas y por otro lado las
funcionalidades de Salesforce Data Cloud que son necesarias para
implementar la solución. En otro documento To Be para este proyecto ya
se ha ilustrado un diagrama de arquitectura necesario para la solución,
pero en este se quiere ilustrar Data Cloud con todas sus funcionalidades
como centro orquestador de los datos del ecosistema Iberdrola.

![](media/image10.png){width="6.267716535433071in"
height="3.3055555555555554in"}

En el diagrama se puede observar cómo se van a configurar **3 fuentes de
entrada (conectores)** a Salesforce Data Cloud con sus respectivos
flujos de datos (Data Stream):

-   **SFTP**. La forma de integrar los datos será mediante un fichero
    CSV y a través del conector nativo Secure File Transfer Connector.
    [Permite importar datos de fuentes dispares y aisladas. Se pueden
    traer archivos de datos CSV desde un servidor SFTP a Data Cloud
    utiliza.]{.mark}La frecuencia de carga será diaria y siempre
    secuenciada con los cálculos e ingestas de SAS y los procesos Batch.
    Los bloques de información o entidades a integrar con esta fuente de
    entrada son:

    -   Data Stream de cliente. Se construirá un flujo de entrada de
        datos de cliente

    -   Data Streams de consentimientos

    -   Data stream de emails

    -   Data stream de teléfonos

    -   Data Stream de leads

    -   Data Stream de contratos

    -   Data Stream de servicios

-   **Salesforce Marketing Cloud**. La forma de integrar el bloque de
    información de comportamiento del usuario en base a los emails, push
    y sms enviados será mediante el conector nativo de Data Cloud, en
    concreto mediante data bundles (si son paquetes de datos estándar
    entre ambos sistemas) o Data extensions si son conjuntos de datos
    personalizados. Importante destacar que solo se ingestan 90 días de
    datos al crear flujos de datos con Marketing Cloud. Se utilizarán
    los Data Bundles de EmailStudio y MobilePush de cara a que Data
    Cloud genere los Data Streams correspondientes, así como la
    asignación a sus DMOs. Estos Data Streams se explicarán en el
    apartado correspondiente de este documento.

-   **Salesforce Interaction Studio**. El método de integración para
    ingestar los datos de comportamiento web/app del usuario con Data
    Cloud será mediante su conector nativo, en concreto mediante los
    Starter Bundles que se asocian con un dataset de Interaction Studio.
    Se tendrán dos datasets, uno para integración y otro para
    producción, por lo que en principio de cara a hacer pruebas
    integraremos el primero.

# Diseño funcional

En este documento no se va a explicar qué casos de uso acometeremos con
Salesforce Data Cloud y que entidades, bloques de información y datos
van a tener que viajar hasta esta herramienta para conseguir activar a
la audiencia correcta.

Esta definición queda detallada en el documento "[[IBD - CDP Segmentos
Casos de
uso/audiencias]{.underline}](https://docs.google.com/spreadsheets/d/1ntsM6_M8azqG6jeefpFau6OL8sziKQgyw-UJgZqbmkw/edit#gid=0)".

# Diseño técnico

[En este apartado se pretende explicar técnicamente cómo se van a
desarrollar los 9 casos de uso dentro de Salesforce Data Cloud. Solo
**se utilizará un entorno con datos productivos**.]{.mark}

[Se crearán los siguientes tipos y cantidad de usuarios:]{.mark}

-   [Un usuario admin (Customer Data Cloud Admin), de cara a mantener la
    creación de usuarios y acción sobre cualquier funcionalidad de Data
    Cloud.]{.mark}

-   [Un usuario Administrador de Marketing (Customer Data Cloud For
    Marketing Admin), de cara a mantener toda la estructura técnica de
    objetos, segmentos, etc. Podría ser el mismo que el
    anterior.]{.mark}

-   [Para los desarrolladores de CDP deberemos crear un perfil con dos
    roles:]{.mark}

    -   [Customer Data Cloud For Marketing Data Aware Specialist]{.mark}

    -   [Customer Data Cloud For Marketing Manager]{.mark}

[A continuación, se muestra la tabla de permisos en Data Cloud por
rol.]{.mark}

![](media/image17.png){width="6.267716535433071in"
height="5.069444444444445in"}

## Data Streams

De cara a definir los Data Streams que se van a crear en Data Cloud para
este proyecto lo primero de todo es listar las entidades de datos que
vamos a necesitar, por lo que serán:

1.  **Cliente**. Donde se ingestará toda la información relacionada con
    el cliente, como datos generales, datos de contacto, personales,
    historial, indicadores de comportamiento.

2.  **Contrato**. Dónde vendrán informados los datos generales del
    contrato, datos de tipificación, datos de suministro, de canales, de
    la vivienda, etc. Es la tabla de hechos de los contratos de un
    cliente con fechas de alta, enganche y desenganche del contrato.

3.  **Servicio**. Será la tabla de hechos de servicios, donde se
    relacionarán los servicios asociados a un contrato específico con
    sus fechas de activación y desactivación.

4.  **Consentimientos**. Dónde vendrán informados los datos de
    consentimiento de un cliente, tanto los pertenecientes a Iberdrola
    como los de Robinson asociados a sus puntos de contacto de email y
    teléfono.

5.  **Navegación digital**. En primera instancia, únicamente se
    ingestarán datos procedentes de CATS con los que se podrá detectar
    si es una acción de visita a una página de producto concreta de
    Iberdrola o si ha iniciado un proceso de contratación y lo ha dejado
    en el camino. Más adelante en cuanto esté disponible Interaction
    Studio se ingestarán los data streams correspondientes a esta
    herramienta.

6.  **Delio**. Se ingestarán las interacciones de un usuario con
    Iberdrola a través del Call center o la web vía Delio. Se podrá
    identificar si es ya cliente en Iberdrola o no y tratarlo como un
    lead, además de que productos esta intentando contratar.

En definitiva los orígenes de datos que tendremos para estas entidades
son:

1.  **Sidat**, extraído mediante SAS y volcado en un SFTP.

2.  **CATS**, volcado en Sidat y extraído mediante un proceso batch para
    volcar el fichero CSV en un SFTP. Posteriormente esta navegación
    viajará desde **Interaction Studio** con el conector nativo a Data
    Cloud.

3.  **Delio**, que almacena la vista en una tabla de Sidat para que SAS
    lo pueda extraer y volcar en un fichero en un SFTP.

Es importante señalar que de cara a borrar registros en cadena durante
toda la vida del dato en CDP se pueden generar ficheros con el prefijo
Deletion\_"Nombre del fichero del data stream" y se deposita en una
carpeta delete/ bajo la carpeta del data stream. Ej:

*Deletion_Cliente_20231906111000.csv*

![](media/image9.png){width="5.022141294838145in"
height="1.8543285214348206in"}

### Cliente (SFTP)

El acuerdo de interfaz comprometido para el fichero CSV que se
depositará en una instancia de SFTP es el siguiente:

  -------------------------------------------------------------------------------------------------------------
  **Bloque de      **Nombre atributo**           **Descripción**          **Tipo**      **Nullable**   **PK**
  información**                                                                                        
  ---------------- ----------------------------- ------------------------ ------------- -------------- --------
  Historial de     antiguedad_cliente            Antiguedad del cliente   Text          Si             
  Cliente                                                                                              

  Datos personales apellidos_cliente             Apellidos                Text          No             

  Auditoria        auditoria                     Última modificación      datetime      No             

  Perfil de        clase_cliente                 Clase de cliente         Text(2)       Si             
  cliente                                                                                              

  Indicadores de   clasif_potencial_2_vivienda   Posible segunda vivienda Number        Si             
  comportamiento                                                                                       
  cliente                                                                                              

  Información      CNAE_cliente_desc             CNAE del cliente         Text          Si             
  Laboral                                                                                              

  **Datos          **COD_CLIENTE**               **Cod_cliente**          **Text(8)**   **No**         **X**
  personales**                                                                                         

  Ubicación        DES_CCAA_FS                   CCAA fiscal              Text          Si             

  Ubicación        DES_POBLACION_FS              Poblacion Fiscal         Text          Si             

  Ubicación        DES_PROVINCIA_FS              Provincia fiscal         Text          Si             

  Datos de         DIRECCION_FS                  Dirección fiscal         Text          No             
  contacto                                                                                             

  Datos personales EDAD                          Edad                     Text          Si             

  Historial de     FEC_ALTA_CLIENTE              Fecha de alta del        Date          No             
  Cliente                                        cliente                                               

  Perfil de        GRADO_DIGITALIZACION          Grado digitalización con Text          Si             
  cliente                                        Iberdrola                                             

  Historial de     ind_baja_anterior_CC_ele      Baja anterior por Cambio Number        Si             
  Cliente                                        de Comercializador -                                  
                                                 Electricidad                                          

  Historial de     ind_baja_anterior_CC_gas      Baja anterior por Cambio Number        Si             
  Cliente                                        de Comercializador - Gas                              

  Información      IND_CLI_VULNERABLE            Indicador cliente        Number        Si             
  Laboral                                        Vulnerable                                            

  Información      IND_EMPLEADO_IBD              Indicador de ser         Number        Si             
  Laboral                                        empleado de Iberdrola                                 

  Información      IND_VIP                       Indicador cliente VIP    Number        Si             
  Laboral                                                                                              

  Información      MERCADO_SOLVENCIA_CLI         Mercado Solvencia        Text          Si             
  Laboral                                        Cliente                                               

  Historial de     meses_baja_anterior_CC_ele    Meses desde la última    Text          Si             
  Cliente                                        baja por Cambio de                                    
                                                 Comercializador -                                     
                                                 Electricidad                                          

  Historial de     meses_baja_anterior_CC_gas    Meses desde la última    Text          Si             
  Cliente                                        baja por Cambio de                                    
                                                 Comercializador - Gas                                 

  Datos personales NOM_CLIENTE                   Nombre                   Text          No             

  Datos personales NUM_DCO_IDENTIDAD             NIF                      Text          Si             

  Reclamaciones    NUM_REC_ABIERTAS_ANIO_CLI     Reclamaciones abiertas   Number        Si             
                                                 ultimo año                                            

  Reclamaciones    NUM_REC_ABIERTAS_CLI          Reclamaciones abiertas   Number        Si             

  Reclamaciones    NUM_RECLAMACIONES_CLI         Reclamaciones totales    Number        Si             

  Perfil de        ind_impactado_MI              Ha recibido              Text          Si             
  cliente                                        comunicaciones Mi                                     
                                                 Iberdrola                                             

  Perfil de        ind_adherido_MI               Está adherido a Mi       Text          Si             
  cliente                                        Iberdrola                                             

  Ubicación        pais                          País                     Text          No             

  Perfil de        PERFIL_DIGITAL                Perfil digitalización    Text          Si             
  cliente                                                                                              

  Indicadores de   PERFIL_PSICO                  Perfil psicográfico      Text          Si             
  comportamiento                                                                                       
  cliente                                                                                              

  Indicadores de   PROB_ABANDONO_CAT_CLI         Probabilidad de abandono Text          Si             
  comportamiento                                                                                       
  cliente                                                                                              

  Segmentación     segmento                      Segmento                 Text          Si             

  Indicadores de   tipo_nba                      Tipo NBA                 Text          Si             
  comportamiento                                                                                       
  cliente                                                                                              

                   ahorro_cu_aerotermia          Ahorro del caso de uso   Number        Si             
                                                 de aerotermia                                         
  -------------------------------------------------------------------------------------------------------------

Se consumirá un fichero **CSV** en formato UTF-8 delimitado por comas y
con comillas dobles en cada valor, con una frecuencia diaria e
incremental, con lo que se solo se depositarán los ficheros con los
cambios en datos de cliente cada día. []{.mark}

El nombre del fichero depositado tendrá la siguiente forma (Entidad +
'\_' + timestamp):

*Cliente_20230101140000.csv*

En la configuración de entrada del SFTP deberemos indicar:

-   **Conexión**: Indicaremos el nombre de la conexión del SFTP creada.

-   **Directorio**. Guardaremos cada archivo en su carpeta
    correspondiente asociada a la entidad que aplica.

-   **Nombre de fichero el siguiente wildcard**: Cliente\_\*.\*.csv

-   **Source**. Indicaremos el maestro de donde proceden los datos. En
    este caso indicaremos sftp_sas_delta.

![](media/image14.png){width="5.765625546806649in"
height="2.2507010061242343in"}

La **categoría seleccionada para el DLO** a ingestar en Data Cloud será
**Profile**. La **Primary Key** de estos registros será el
**COD_CLIENTE**, que es un código que identifica unívocamente a un
cliente dentro de Iberdrola. **Record Modified Field** será el campo
auditoría del fichero de entrada y **Organization Unit Identifier** lo
dejaremos vacío.

![](media/image8.png){width="4.458333333333333in"
height="2.3958333333333335in"}

Para este Data Stream de ingesta de datos de cliente deberemos generar
las siguientes **transformaciones** o creación de nuevos campos de
entrada **(New Formula Field)**:

1.  **Contact Point Address Id (Text).** Se calculará a partir de la
    suma de cod_cliente más Dirección fiscal y será la PK del objeto
    Contact Point Address.

2.  **Party Identification Type (Text).** Que será texto fijo igual a
    "Person Identifier".

3.  **Identification Name (Text).** Que será texto fijo igual a
    "Documento de identidad".

4.  **Party Identification Id (Text).** se calculará a partir de la suma
    de cod_cliente + NIF y será la PK de Party Identification.

Además al configurar el Data Stream introduciremos los siguientes
parámetros de configuración:

-   Modo de refresco: Upsert

-   Refrescar solo ficheros nuevos

-   Emitir fallo si no encuentra el fichero cada día

-   Hora de actualización diaria a las 10:00 UTC. Por lo que el fichero
    desde SAS debe depositarse antes en el sftp.

![](media/image1.png){width="5.526042213473316in"
height="4.2684536307961505in"}

### Contrato (SFTP)

El acuerdo de interfaz comprometido para el fichero CSV que se
depositará en una instancia de SFTP es el siguiente:

  ----------------------------------------------------------------------------------------------------------------------
  **Bloque de         **Nombre atributo**            **Descripción**    **Tipo**   **Nullable**   **PK**   **Maestro**
  información**                                                                                            
  ------------------- ------------------------------ ------------------ ---------- -------------- -------- -------------
  Auditoria           auditoria                      Última             datetime   No                      
                                                     modificación                                          

  Indicadores de      CLASIF_PROB_ABAND_CAT_CTO      Probabilidad de    Text       Si                      
  comportamiento                                     abandono                                              
  contrato                                                                                                 

  Indicadores de      cluster_segm_unica             Cluster            Text       Si                      
  comportamiento                                     Segmentacion Unica                                    
  contrato                                           Particulares                                          

  Datos del Cliente   COD_CLIENTE                    cod_cliente        Text       No                      

  **Datos Generales   **COD_CONTRATO**               **Cod_contrato**   **Text**   **No**         **X**    
  del Contrato**                                                                                           

  Datos Generales del COD_POSTAL_CO                  Postal             Text       Si                      
  Contrato                                           Correspondencia                                       

  Detalle del         COD_POSTAL_PS                  Codigo Postal                                         
  Suministro                                         Punto Suministro                                      

  Detalle del         COD_PS                         Punto de           Text       Si                      
  Suministro                                         suministro                                            

  Detalle del         COD_RS                         Receptor de        Text       Si                      
  Suministro                                         suministro                                            

  Datos del Cliente   COD_SOCIEDAD                   cod_sociedad       Text       No                      

  Detalle del         CUPS                           CUPS               Text       Si                      
  Suministro                                                                                               

  Canales y Extras    DES_CANAL_CONTRATACION         Canal de           Text       Si                      
  del Contrato                                       contratación                                          

  Datos Generales del DES_CCAA_CO                    CCAA               Text       Si                      SAS-SIDAT
  Contrato                                           Correspondencia                                       

  Detalle del         DES_CCAA_PS                    CCAA Punto         Text       Si                      SAS-SIDAT
  Suministro                                         Suministro                                            

  Indicadores de      des_grupo_economico            Descripcion grupo  Text       Si                      
  comportamiento                                     economico                                             
  contrato                                                                                                 

  Tipificación del    DES_PDT_PTLAR                  Producto           Text       Si                      
  Contrato                                           particular                                            
                                                     descripción (Plan)                                    

  Tipificación del    DES_PLAN                       descripcion del                                       
  Contrato                                           plan                                                  

  Datos Generales del DES_POBLACION_CO               Poblacion          Text       Si                      
  Contrato                                           Correspondencia                                       

  Detalle del         DES_POBLACION_PS               Poblacion Punto                                       
  Suministro                                         Suministro                                            

  Datos Generales del DES_PROVINCIA_CO               Provincia          Text       Si                      SAS-SIDAT
  Contrato                                           Correspondencia                                       

  Detalle del         DES_PROVINCIA_PS               Provincia Punto    Text       Si                      SAS-SIDAT
  Suministro                                         Suministro                                            

  Tipificación del    DES_TIP_CONTRATO               Tipo de contrato   Text       No                      
  Contrato                                           (normal,                                              
                                                     eventual,\...)                                        

  Reclamaciones       DES_TIP_RECLA                  Tipo de ultima     Text       Si                      
                                                     reclamacion                                           

  Reclamaciones       DES_TIP_RESUL_CNMC             Resultado          Text       Si                      
                                                     Reclamacion CNMC                                      

  Detalle del         DES_TIP_SECTOR_ENE             Tipo de sector     Text       Si                      
  Suministro                                         energía                                               

  Reclamaciones       DES_TIP_SUB_RECLA              Subtipo de la      Text       Si                      
                                                     última reclamación                                    

  Canales y Extras    DESC_TIPO_SMART_SOLAR          Descriptivo        Text       Si                      
  del Contrato                                       Indicador smart                                       
                                                     solar                                                 

  Canales y Extras    DESC_TIPO_VEHICULO_ELECTRICO   Descriptivo        Text       Si                      
  del Contrato                                       Indicador vehículo                                    
                                                     eléctrico                                             

  Datos Generales del DIRECCION_COMPLETA_CORRES      Direccion          Text       Si                      
  Contrato                                           Correspondencia                                       

  Detalle del         DIRECCION_COMPLETA_PS          Direccion Punto    Text       Si                      SAS-SIDAT
  Suministro                                         Suministro                                            

  Datos Generales del FEC_ALTA_CONTRATO              Fecha alta         Date       No                      
  Contrato                                           contrato                                              

  Tipificación del    FEC_BAJA_CTO                   Fecha de baja      Date       No                      
  Contrato                                                                                                 

  Tipificación del    FEC_EGC_INI_CONTTO             Fecha de enganche  Date       No                      
  Contrato                                                                                                 

  Tipificación del    FEC_RENOVACION                 Fecha de última    Date       No                      
  Contrato                                           renovación                                            

  Reclamaciones       FEC_ULT_RECLA                  Fecha de la última datetime   Si                      
                                                     reclamación                                           

  Consumo Energético  IMPORTE_FACTU_12_MESES         Importe factura 12 Number     Si                      
                                                     meses                                                 

  Consumo Energético  IMPORTE_FACTU_MEDIA_MENSUAL    Importe factura    Number                             
                                                     media mensual                                         

  Indicadores de      ind_aire_acondicionado         Indicador Aire     Number     Si                      
  comportamiento                                     Acondicionado                                         
  contrato                                                                                                 

  Indicadores de      ind_calefaccion_electrica      Indicador          Number     Si                      
  comportamiento                                     Calefaccion                                           
  contrato                                           electrica                                             

  Datos del Cliente   IND_CLI_MOROSO                 Indicador Cliente  Number     Si                      
                                                     Moroso                                                

  Canales y Extras    IND_DOMICILIADO                Indicador de       Number     Si                      
  del Contrato                                       domiciliado                                           

  Canales y Extras    ind_fe                         Indicador de fact  Number     Si                      
  del Contrato                                       ele                                                   

  Datos de la         IND_MONOFINCAS                 Indicar Monofinca  Number     Si                      
  Vivienda                                                                                                 

  Indicadores de      ind_posible_2v                 Indicador posible  Number                             
  comportamiento                                     segunda vivienda                                      
  contrato                                                                                                 

  Indicadores de      ind_vivienda_alquiler          Indicador Vivienda Number     Si                      
  comportamiento                                     alquiler                                              
  contrato                                                                                                 

  Canales y Extras    MERCADO_SOLVENCIA_CTO          Mercado solvencia  Text       Si                      
  del Contrato                                       Cto                                                   

  Datos del Cliente   motivo_baja_calculado          Motivo de baja     Text       Si                      

  Reclamaciones       NUM_RECLA_ABIERTAS             Reclamaciones      Number     Si                      
                                                     abiertas                                              

  Detalle del         pais                           Pais               Text       Si                      
  Suministro                                                                                               

  Segmentación        segmento                       Segmento           Text       Si                      

  Tipificación del    tarifa_td                      Tarifa código      Text       Si                      
  Contrato                                                                                                 

  Tipificación del    TIP_ALTA_CTO                   Tipo de alta       Text       Si                      
  Contrato                                           contrato                                              

  Tipificación del    TIP_BAJA_CTO                   Tipo de baja       Text       Si                      
  Contrato                                           contrato                                              

  Datos Generales del TIP_EST_CONTRATO               Estado contrato    Text       No                      
  Contrato                                                                                                 

  Datos del Cliente   TIP_IDIOMA_FACTU               Idioma preferente  Text       Si                      

  Datos de la         TIPOLOGIA_VIVIENDA             Tipología de       Text       Si                      
  Vivienda                                           viviendas                                             

  Consumo Energético  VAL_CSU_12_MESES               Consumo últimos 12 Number     Si                      
                                                     meses                                                 

  Datos de la         ZONA_CLIMA_LOCAL               Zona clima local   Text       Si                      
  Vivienda                                                                                                 

  Datos de la         ZONA_RATIO                     Zona ratio         Text       Si                      
  Vivienda                                                                                                 
  ----------------------------------------------------------------------------------------------------------------------

Se consumirá un fichero **CSV** en formato UTF-8 delimitado por comas y
con comillas dobles en cada valor, con una frecuencia diaria e
incremental, con lo que se solo se depositarán los ficheros con los
cambios en datos de cliente cada día. Estos ficheros además contendrán
una **fecha de modificación del registro** de contrato, para tener una
trazabilidad de dichos registros.

El nombre del fichero depositado tendrá la siguiente forma (Entidad +
'\_' + timestamp):

*Contrato_20230101140000.csv*

En la configuración de entrada del SFTP deberemos indicar:

-   **Conexión**: Indicaremos el nombre de la conexión del SFTP creada.

-   **Directorio**. Guardaremos cada archivo en su carpeta
    correspondiente asociada a la entidad que aplica.

-   **Nombre de fichero el siguiente wildcard**: Contrato\_\[0-2\].csv y
    Contrato\_\[3-5\].csv

-   **Source**. Indicaremos el maestro de donde proceden los datos. En
    este caso indicaremos sftp_sas_delta.

Debido a límites de carga, se crearán dos data streams que recibirán
información de contrato.

La **categoría seleccionada para el DLO** a ingestar en Data Cloud será
**Profile**. La **Primary Key** de estos registros será el
**COD_CONTRATO**, que es un código que identifica unívocamente a un
contrato dentro de Iberdrola. **Record Modified Field** será el campo
auditoría del fichero de entrada y **Organization Unit Identifier** lo
dejaremos vacío.

Para este Data Stream de ingesta de datos de contrato no deberemos
generar ninguna **transformación** o creación de nuevos campos de
entrada.

Además al configurar el Data Stream introduciremos los siguientes
parámetros de configuración:

-   Modo de refresco: Full Refresh

-   Refrescar solo ficheros nuevos

-   Emitir fallo si no encuentra el fichero cada día

-   Hora de actualización diaria a las 11:00 UTC. Por lo que el fichero
    desde SAS debe depositarse antes en el SFTP.

### Servicio (SFTP)

El acuerdo de interfaz comprometido para el fichero CSV que se
depositará en una instancia de SFTP es el siguiente:

  --------------------------------------------------------------------------------------------------------
  **Bloque de      **Nombre atributo**  **Descripción**   **Tipo**   **Nullable**   **PK**   **Maestro**
  información**                                                                              
  ---------------- -------------------- ----------------- ---------- -------------- -------- -------------
  Auditoria        auditoria            Última            datetime   No                      SAS-SIDAT
                                        modificación                                         

  Datos generales  COD_CONTRATO         Código contrato   Text       No                      Delta
  del servicio                                                                               

  Datos generales  COD_EFV              Código servicio   Text       No                      Delta
  del servicio                                                                               

  Categorización   COD_GRUPO_EFV        Agrupación de     Text       No                      Delta
  del servicio                          servicios                                            

  Categorización   DES_EFV              Descriptivo de    Text       No                      Delta
  del servicio                          servicio                                             

  Categorización   DES_GRUPO_EFV        Descriptivo de    Text       No                      Delta
  del servicio                          agrupación de                                        
                                        servicio                                             

  Datos generales  fec_activacion       Fecha de          Date       No                      Delta
  del servicio                          activación                                           

  Datos generales  fec_desactivacion    Fecha de          Date       No                      Delta
  del servicio                          desactivación                                        

  Datos generales  TIP_EST_EFF          Estado servicio   Text       No                      Delta
  del servicio                                                                               

  **Datos          **Id**               **Id único**      **Text**   **No**         **X**    **Delta**
  generales del                                                                              
  servicio**                                                                                 
  --------------------------------------------------------------------------------------------------------

Se consumirá un fichero **CSV** en formato UTF-8 delimitado por comas y
con comillas dobles en cada valor, con una frecuencia diaria e
incremental, con lo que se solo se depositarán los ficheros con los
cambios en datos de servicios cada día. Estos ficheros además contendrán
una **fecha de modificación del registro** de servicio, para tener una
trazabilidad de dichos registros.

El nombre del fichero depositado tendrá la siguiente forma (Entidad +
'\_' + timestamp):

*Servicio_20230101140000.csv*

En la configuración de entrada del SFTP deberemos indicar:

-   **Conexión**: Indicaremos el nombre de la conexión del SFTP creada.

-   **Directorio**. Guardaremos cada archivo en su carpeta
    correspondiente asociada a la entidad que aplica.

-   **Nombre de fichero el siguiente wildcard**: Servicio\_\*.\*.csv

-   **Source**. Indicaremos el maestro de donde proceden los datos. En
    este caso indicaremos sftp_sas_delta.

La **categoría seleccionada para el DLO** a ingestar en Data Cloud será
**Profile**. La **Primary Key** de estos registros será **id**, que es
un código que identifica unívocamente a un servicio concreto dentro de
Iberdrola. **Record Modified Field** será el campo auditoría del fichero
de entrada y **Organization Unit Identifier** lo dejaremos vacío.

Para este Data Stream de ingesta de datos de contrato no deberemos
generar ninguna **transformación** o creación de nuevos campos de
entrada.

Además al configurar el Data Stream introduciremos los siguientes
parámetros de configuración:

-   Modo de refresco: Upsert. Aunque el fichero sea incremental y solo
    venga registros nuevos o modificados.

-   Refrescar solo ficheros nuevos

-   Emitir fallo si no encuentra el fichero cada día

-   Hora de actualización diaria a las 10:00 UTC. Por lo que el fichero
    desde SAS debe depositarse antes en el SFTP.

### Consentimientos (SFTP)

El acuerdo de interfaz comprometido para el fichero CSV que se
depositará en una instancia de SFTP es el siguiente. Queda reflejado en
este apartado en base a los tipos de consentimientos que tiene a día de
hoy iberdrola (ver [[Gestión de
Consentimientos]{.underline}](#gestión-de-consentimientos)).

**Consentimientos Terceros**

El texto legal que acepta o no el cliente de Iberdrola para este tipo de
consentimiento es:

*"Consiento que Iberdrola utilice información de terceras empresas para
adaptar las ofertas y promociones a mis intereses y necesidades."*

  --------------------------------------------------------------------------------------------------------------
  **Bloque de         **Nombre atributo**   **Descripción**   **Tipo**   **Nullable**   **PK**   **Maestro**
  información**                                                                                  
  ------------------- --------------------- ----------------- ---------- -------------- -------- ---------------
  Datos del cliente   COD_CLIENTE           Código de cliente Text(8)    No                      Delta

  Datos del cliente   COD_SOCIEDAD          Código de         Number     No                      Delta
                                            sociedad                                             

  Consentimientos uso IND_CONSEN_TERCEROS   Indicador de      Text       No                      Delta
  de datos                                  consentimiento de                                    
                                            terceros                                             

  Datos del           auditoria             Última            datetime   No                      SAS-SIDAT
  consentimiento                            modificación                                         

  **Datos del         **Id**                **Id único**      **Text**   **No**         **X**    **SAS-SIDAT**
  consentimiento**                                                                               
  --------------------------------------------------------------------------------------------------------------

Se consumirá un fichero **CSV** en formato UTF-8 delimitado por comas y
con comillas dobles en cada valor, con una frecuencia diaria e
incremental, con lo que se solo se depositarán los ficheros con los
cambios en datos de consentimientos de cliente cada día. Estos ficheros
además contendrán una **fecha de modificación del registro** de
consentimientos, para tener una trazabilidad de dichos registros.

El nombre del fichero depositado tendrá la siguiente forma (Entidad +
'\_' + timestamp):

*Consentimientos_Terceros_20230101140000.csv*

En la configuración de entrada del SFTP deberemos indicar:

-   **Conexión**: Indicaremos el nombre de la conexión del SFTP creada.

-   **Directorio**. Guardaremos cada archivo en su carpeta
    correspondiente asociada a la entidad que aplica.

-   **Nombre de fichero el siguiente wildcard**:
    Consentimientos_Terceros\_\*.\*.csv

-   **Source**. Indicaremos el maestro de donde proceden los datos. En
    este caso indicaremos sftp_sas_delta.

La **categoría seleccionada para el DLO** a ingestar en Data Cloud será
**Other.** La **Primary Key** de estos registros será **Id**, un nuevo
campo de entrada, el cual es un código que identifica unívocamente a un
cliente dentro de Iberdrola. **Record Modified Field y Event Time
Field** será el campo auditoría del fichero de entrada y **Organization
Unit Identifier** lo dejaremos vacío.

Para este Data Stream de ingesta de datos de cliente deberemos generar
las siguientes **transformaciones** o creación de nuevos campos de
entrada **(New Formula Field)**:

1.  **Third Party Consent Status (Text).** Permite identificar si el
    cliente ha dado o no su consentimiento. Si el campo
    IND_CONSEN_TERCEROS es igual a "S" el valor será "Opt In" si no será
    "Opt Out".

2.  **Party Consent Name (Text).** Que será texto fijo igual a "Third
    Party Data Consent".

3.  **Effective To Date (Datetime).** Será una fecha fija igual a
    DATE("2999", "12", "31").

Además al configurar el Data Stream introduciremos los siguientes
parámetros de configuración:

-   Modo de refresco: Upsert. Aunque el fichero sea incremental y solo
    venga registros nuevos o modificados.

-   Refrescar solo ficheros nuevos

-   Emitir fallo si no encuentra el fichero cada día

-   Hora de actualización diaria a las 12:00 UTC. Por lo que el fichero
    desde SAS debe depositarse antes en el SFTP.

**Consentimientos Winback**

El texto legal que acepta o no el cliente de Iberdrola para este tipo de
consentimiento es:

*"Consiento recibir comunicaciones comerciales de ofertas y promociones
de terceros y, de Iberdrola tras la baja del contrato."*

  -----------------------------------------------------------------------------------------------------------------
  **Bloque de        **Nombre atributo**       **Descripción**   **Tipo**   **Nullable**   **PK**   **Maestro**
  información**                                                                                     
  ------------------ ------------------------- ----------------- ---------- -------------- -------- ---------------
  Datos del cliente  COD_CLIENTE               Código de cliente Text(8)    No                      Delta

  Datos del cliente  COD_SOCIEDAD              Código de         Number     No                      Delta
                                               sociedad                                             

  Consentimientos    IND_CONSEN_COMUNICACION   Indicador de      Text       No                      Delta
  comunicaciones                               consentimientos                                      
                                               de comunicación                                      
                                               después de la                                        
                                               baja                                                 

  Datos del          auditoria                 Última            datetime   No                      SAS-SIDAT
  consentimiento                               modificación                                         

  **Datos del        **Id**                    **Id único**      **Text**   **No**         **X**    **SAS-SIDAT**
  consentimiento**                                                                                  
  -----------------------------------------------------------------------------------------------------------------

Se consumirá un fichero **CSV** en formato UTF-8 delimitado por comas y
con comillas dobles en cada valor, con una frecuencia diaria e
incremental, con lo que se solo se depositarán los ficheros con los
cambios en datos de consentimientos de cliente cada día. Estos ficheros
además contendrán una **fecha de modificación del registro** de
consentimientos, para tener una trazabilidad de dichos registros.

El nombre del fichero depositado tendrá la siguiente forma (Entidad +
'\_' + timestamp):

*Consentimientos_Winback_20230101140000.csv*

En la configuración de entrada del SFTP deberemos indicar:

-   **Conexión**: Indicaremos el nombre de la conexión del SFTP creada.

-   **Directorio**. Guardaremos cada archivo en su carpeta
    correspondiente asociada a la entidad que aplica.

-   **Nombre de fichero el siguiente wildcard**:
    Consentimientos_Winback\_\*.\*.csv

-   **Source**. Indicaremos el maestro de donde proceden los datos. En
    este caso indicaremos sftp_sas_delta.

La **categoría seleccionada para el DLO** a ingestar en Data Cloud será
**Other**. La **Primary Key** de estos registros será el **Id**, un
nuevo campo de entrada, el cual es un código que identifica unívocamente
a un cliente dentro de Iberdrola. **Record Modified Field y Event Time
Field** será el campo auditoría del fichero de entrada y **Organization
Unit Identifier** lo dejaremos vacío.

Para este Data Stream de ingesta de datos de cliente deberemos generar
las siguientes **transformaciones** o creación de nuevos campos de
entrada **(New Formula Field)**:

1.  **Winback Consent Status (Text).** Permite identificar si el cliente
    ha dado o no su consentimiento. Si el campo IND_CONSEN_COMUNICACION
    es igual a "S" el valor será "Opt In" si no será "Opt Out".

2.  **Party Consent Name (Text).** Que será texto fijo igual a "Winback
    Consent".

3.  **Effective To Date (Datetime).** Será una fecha fija igual a
    DATE("2999", "12", "31").

Además al configurar el Data Stream introduciremos los siguientes
parámetros de configuración:

-   Modo de refresco: Upsert. Aunque el fichero sea incremental y solo
    venga registros nuevos o modificados.

-   Refrescar solo ficheros nuevos

-   Emitir fallo si no encuentra el fichero cada día

-   Hora de actualización diaria a las 12:00 UTC. Por lo que el fichero
    desde SAS debe depositarse antes en el SFTP.

**Consentimientos Perfilado**

El texto legal que acepta o no el cliente de Iberdrola para este tipo de
consentimiento es:

*"Me opongo a que Iberdrola utilice mi información como cliente para que
adapten las comunicaciones comerciales a mis intereses y necesidades. No
recibiré comunicaciones que pudieran ser de mi interés (por ejemplo,
descuentos o promociones que me permitan ahorrar)."*

  --------------------------------------------------------------------------------------------------------------
  **Bloque de         **Nombre atributo**   **Descripción**   **Tipo**   **Nullable**   **PK**   **Maestro**
  información**                                                                                  
  ------------------- --------------------- ----------------- ---------- -------------- -------- ---------------
  Datos del cliente   COD_CLIENTE           Código de cliente Text(8)    No                      Delta

  Datos del cliente   COD_SOCIEDAD          Código de         Number     No                      Delta
                                            sociedad                                             

  Consentimientos uso IND_OPOSI_PERFILADO   Indicador de      Text       No                      Delta
  de datos                                  consentimientos                                      
                                            de perfilado en                                      
                                            base a sus datos                                     

  Datos del           auditoria             Última            datetime   No                      SAS-SIDAT
  consentimiento                            modificación                                         

  **Datos del         **Id**                **Id único**      **Text**   **No**         **X**    **SAS-SIDAT**
  consentimiento**                                                                               
  --------------------------------------------------------------------------------------------------------------

Se consumirá un fichero **CSV** en formato UTF-8 delimitado por comas y
con comillas dobles en cada valor, con una frecuencia diaria e
incremental, con lo que se solo se depositarán los ficheros con los
cambios en datos de consentimientos de cliente cada día. Estos ficheros
además contendrán una **fecha de modificación del registro** de
consentimientos, para tener una trazabilidad de dichos registros.

El nombre del fichero depositado tendrá la siguiente forma (Entidad +
'\_' + timestamp):

*Consentimientos_Perfilado_20230101140000.csv*

En la configuración de entrada del SFTP deberemos indicar:

-   **Conexión**: Indicaremos el nombre de la conexión del SFTP creada.

-   **Directorio**. Guardaremos cada archivo en su carpeta
    correspondiente asociada a la entidad que aplica.

-   **Nombre de fichero el siguiente wildcard**:
    Consentimientos_Perfilado\_\*.\*.csv

-   **Source**. Indicaremos el maestro de donde proceden los datos. En
    este caso indicaremos sftp_sas_delta.

La **categoría seleccionada para el DLO** a ingestar en Data Cloud será
**Other**. La **Primary Key** de estos registros será el **Id**, un
nuevo campo de entrada, el cual es un código que identifica unívocamente
a un cliente dentro de Iberdrola. **Record Modified Field y Event Time
Field** será el campo auditoría del fichero de entrada y **Organization
Unit Identifier** lo dejaremos vacío.

Para este Data Stream de ingesta de datos de cliente deberemos generar
las siguientes **transformaciones** o creación de nuevos campos de
entrada **(New Formula Field)**:

1.  **Profiling Consent Status (Text).** Permite identificar si el
    cliente ha dado o no su consentimiento. Si el campo
    IND_OPOSI_PERFILADO es igual a "N" el valor será "Opt In" si no será
    "Opt Out".

2.  **Party Consent Name (Text).** Que será texto fijo igual a
    "Profiling Consent".

3.  **Effective To Date (Datetime).** Será una fecha fija igual a
    DATE("2999", "12", "31").

Además al configurar el Data Stream introduciremos los siguientes
parámetros de configuración:

-   Modo de refresco: Upsert. Aunque el fichero sea incremental y solo
    venga registros nuevos o modificados.

-   Refrescar solo ficheros nuevos

-   Emitir fallo si no encuentra el fichero cada día

-   Hora de actualización diaria a las 12:00 UTC. Por lo que el fichero
    desde SAS debe depositarse antes en el SFTP.

**Consentimientos Comunicaciones**

El texto legal que acepta o no el cliente de Iberdrola para este tipo de
consentimiento es:

*"Me opongo a recibir comunicaciones comerciales de Iberdrola. No
recibiré ninguna comunicación sobre ofertas y promociones de
Iberdrola."*

  ----------------------------------------------------------------------------------------------------------------
  **Bloque de        **Nombre atributo**      **Descripción**   **Tipo**   **Nullable**   **PK**   **Maestro**
  información**                                                                                    
  ------------------ ------------------------ ----------------- ---------- -------------- -------- ---------------
  Datos del cliente  COD_CLIENTE              Código de cliente Text(8)    No                      Delta

  Datos del cliente  COD_SOCIEDAD             Código de         Number     No                      Delta
                                              sociedad                                             

  Consentimientos    IND_OPOSI_COMUNICACION   Indicador de      Text       No                      Delta
  comunicaciones                              oposición a                                          
                                              comunicaciones                                       
                                              comerciales                                          

  Datos del          auditoria                Última            datetime   No                      SAS-SIDAT
  consentimiento                              modificación                                         

  **Datos del        **Id**                   **Id único**      **Text**   **No**         **X**    **SAS-SIDAT**
  consentimiento**                                                                                 
  ----------------------------------------------------------------------------------------------------------------

Se consumirá un fichero **CSV** en formato UTF-8 delimitado por comas y
con comillas dobles en cada valor, con una frecuencia diaria e
incremental, con lo que se solo se depositarán los ficheros con los
cambios en datos de consentimientos de cliente cada día. Estos ficheros
además contendrán una **fecha de modificación del registro** de
consentimientos, para tener una trazabilidad de dichos registros.

El nombre del fichero depositado tendrá la siguiente forma (Entidad +
'\_' + timestamp):

*Consentimientos_Comunicaciones_20230101140000.csv*

En la configuración de entrada del SFTP deberemos indicar:

-   **Conexión**: Indicaremos el nombre de la conexión del SFTP creada.

-   **Directorio**. Guardaremos cada archivo en su carpeta
    correspondiente asociada a la entidad que aplica.

-   **Nombre de fichero el siguiente wildcard**:
    Consentimientos_Comunicaciones\_\*.\*.csv

-   **Source**. Indicaremos el maestro de donde proceden los datos. En
    este caso indicaremos sftp_sas_delta.

La **categoría seleccionada para el DLO** a ingestar en Data Cloud será
**Other**. La **Primary Key** de estos registros será el **Id**, un
nuevo campo de entrada, el cual es un código que identifica unívocamente
a un cliente dentro de Iberdrola. **Record Modified Field y Event Time
Field** será el campo auditoría del fichero de entrada y **Organization
Unit Identifier** lo dejaremos vacío.

Para este Data Stream de ingesta de datos de cliente deberemos generar
las siguientes **transformaciones** o creación de nuevos campos de
entrada **(New Formula Field)**:

1.  **Communications Consent Status (Text).** Permite identificar si el
    cliente ha dado o no su consentimiento. Si el campo
    IND_OPOSI_COMUNICACION es igual a "N" el valor será "Opt In" si no
    será "Opt Out".

2.  **Party Consent Name (Text).** Que será texto fijo igual a
    "Communications Consent".

3.  **Effective To Date (Datetime).** Será una fecha fija igual a
    DATE("2999", "12", "31").

Además al configurar el Data Stream introduciremos los siguientes
parámetros de configuración:

-   Modo de refresco: Upsert. Aunque el fichero sea incremental y solo
    venga registros nuevos o modificados.

-   Refrescar solo ficheros nuevos

-   Emitir fallo si no encuentra el fichero cada día

-   Hora de actualización diaria a las 9:00 UTC. Por lo que el fichero
    desde SAS debe depositarse antes en el SFTP.

**Consentimientos Robinson**

Para este tipo de consentimiento no contamos con un texto legal, sin
embargo este indica si una persona se ha opuesto a recibir
comunicaciones publicitarias o comerciales a su número de teléfono y
email.

  ----------------------------------------------------------------------------------------------
  **Bloque de       **Nombre atributo**     **Descripción**   **Tipo**   **Nullable**   **PK**
  información**                                                                         
  ----------------- ----------------------- ----------------- ---------- -------------- --------
  Consentimientos   ID                      Id único          Text       No             x
  robinson                                                                              

  Datos del cliente COD_CLIENTE             Código de cliente Text       No             

  Consentimientos   COD_SOCIEDAD            Código de         Text       No             
  robinson                                  sociedad                                    

  Consentimientos   DNI                     DNI               Text       No             
  robinson                                                                              

  Consentimientos   IND_ROBINSON_ADIGITAL   Indicador de      Text       No             
  robinson                                  oposición a las                             
                                            comunicaciones                              
                                            email / teléfono                            

  Consentimientos   FECHA_CONSULTA          Fecha Auditoria   Datetime   No             
  robinson                                  Adigital                                    

  Consentimientos   PROXIMA_CONSULTA                          Datetime   No             
  robinson                                                                              

  Consentimientos   AUDITORIA               Última            Datetime   No             
  robinson                                  modificación                                
  ----------------------------------------------------------------------------------------------

Se consumirá un fichero **CSV** en formato UTF-8 delimitado por comas y
con comillas dobles en cada valor, con una frecuencia diaria e
incremental, con lo que se solo se depositarán los ficheros con los
cambios en datos de consentimientos de cliente cada día. Estos ficheros
además contendrán una **fecha de modificación del registro** de
consentimientos, para tener una trazabilidad de dichos registros.

El nombre del fichero depositado tendrá la siguiente forma (Entidad +
'\_' + timestamp):

*Consentimientos_Robinson_20230101140000.csv*

En la configuración de entrada del SFTP deberemos indicar:

-   **Conexión**: Indicaremos el nombre de la conexión del SFTP creada.

-   **Directorio**. Guardaremos cada archivo en su carpeta
    correspondiente asociada a la entidad que aplica.

-   **Nombre de fichero el siguiente wildcard**:
    Consentimientos_Robinson\_\*.\*.csv

-   **Source**. Indicaremos el maestro de donde proceden los datos. En
    este caso indicaremos sftp_sas_delta.

La **categoría seleccionada para el DLO** a ingestar en Data Cloud será
**Other**. La **Primary Key** de estos registros será el **Id**, un
nuevo campo de entrada, el cual es un código que identifica unívocamente
a un cliente dentro de Iberdrola. **Record Modified Field** será el
campo auditoría del fichero de entrada y **Organization Unit
Identifier** lo dejaremos vacío.

Para este Data Stream de ingesta de datos de cliente deberemos generar
las siguientes **transformaciones** o creación de nuevos campos de
entrada **(New Formula Field)**:

1.  **Robinson Consent Status (Text).** Permite identificar si el
    cliente ha dado o no su consentimiento. Si el campo
    IND_ROBINSON_ADIGITAL es igual a "N" el valor será "Opt In" si no
    será "Opt Out".

2.  **Party Consent Name (Text).** Que será texto fijo igual a "Robinson
    Consent".

3.  **Effective To Date (Datetime).** Será una fecha fija igual a
    DATE("2999", "12", "31").

Además al configurar el Data Stream introduciremos los siguientes
parámetros de configuración:

-   Modo de refresco: Upsert. Aunque el fichero sea incremental y solo
    venga registros nuevos o modificados.

-   Refrescar solo ficheros nuevos

-   Emitir fallo si no encuentra el fichero cada día

-   Hora de actualización diaria a las 10:00 UTC. Por lo que el fichero
    desde SAS debe depositarse antes en el SFTP.

**Email**

Para este tipo de consentimiento no contamos con un texto legal, sin
embargo este indica si una persona se ha opuesto a recibir correos
electrónicos con fines publicitarios o comerciales.

  ----------------------------------------------------------------------------------------------
  **Bloque de       **Nombre atributo**     **Descripción**   **Tipo**   **Nullable**   **PK**
  información**                                                                         
  ----------------- ----------------------- ----------------- ---------- -------------- --------
  Datos del         ID                      Id único          Text       No             x
  consentimiento                                                                        

  Datos del cliente COD_CLIENTE             Código de cliente Text       No             

  Datos del cliente COD_SOCIEDAD            Código de                                   
                                            sociedad                                    

  Datos del cliente EMAIL                   email             Text       No             

  Consentimientos   IND_ROBINSON_ADIGITAL   Indicador de      Text       No             
  robinson                                  oposición a las                             
                                            comunicaciones                              
                                            email                                       

  Consentimientos   FECHA_CONSULTA          Fecha Auditoria   Datetime   No             
  robinson                                  Adigital                                    

  Consentimientos   PROXIMA_CONSULTA                          Datetime   No             
  robinson                                                                              

  Datos del         AUDITORIA               Última            Datetime   No             
  consentimiento                            modificación                                
  ----------------------------------------------------------------------------------------------

Se consumirá un fichero **CSV** en formato UTF-8 delimitado por comas y
con comillas dobles en cada valor, con una frecuencia diaria e
incremental, con lo que se solo se depositarán los ficheros con los
cambios en datos de consentimientos de cliente cada día. Estos ficheros
además contendrán una **fecha de modificación del registro** de
consentimientos, para tener una trazabilidad de dichos registros.

El nombre del fichero depositado tendrá la siguiente forma (Entidad +
'\_' + timestamp):

*Email_20230101140000.csv*

En la configuración de entrada del SFTP deberemos indicar:

-   **Conexión:** Indicaremos el nombre de la conexión del SFTP creada.

-   **Directorio.** Guardaremos cada archivo en su carpeta
    correspondiente asociada a la entidad que aplica.

-   **Nombre de fichero el siguiente wildcard:** Email\_\*.csv

-   **Source.** Indicaremos el maestro de donde proceden los datos. En
    este caso indicaremos sftp_sas_delta.

La **categoría seleccionada para el DLO** a ingestar en Data Cloud será
**Other**. La **Primary Key** de estos registros será **Id**, un nuevo
campo de entrada, el cual es un código que identifica unívocamente a un
cliente dentro de Iberdrola. **Record Modified Field** será el campo
auditoría del fichero de entrada y **Organization Unit Identifier** lo
dejaremos vacío.

Para este Data Stream de ingesta de datos de cliente deberemos generar
las siguientes **transformaciones** o creación de nuevos campos de
entrada **(New Formula Field)**:

1.  **Consent Email Status** **(Text).** Permite identificar si el
    cliente ha dado o no su consentimiento. Si el campo IND_OPOSI_EMAIL
    es igual a "N" el valor será "Opt In" si no será "Opt Out".

2.  **Contact Point Consent Name** **(Text).** Que será texto fijo igual
    a "Contact Point Email Consent".

3.  **Effective To Date (Datetime).** Será una fecha fija igual a
    DATE("2999", "12", "31").

4.  **Email Domain (Text).** Se extraerá el dominio de correo
    electrónico del atributo de entrada email a través de la siguiente
    expresión regular:
    @(\[a-zA-Z0-9-\]+.)\*\[a-zA-Z0-9-\]+.\[a-zA-Z\]{2,}

Además al configurar el Data Stream introduciremos los siguientes
parámetros de configuración:

-   Modo de refresco: Upsert. Aunque el fichero sea incremental y solo
    venga registros nuevos o modificados.

-   Refrescar solo ficheros nuevos

-   Emitir fallo si no encuentra el fichero cada día

-   Hora de actualización diaria a las 10:00 UTC. Por lo que el fichero
    desde SAS debe depositarse antes en el SFTP.

**Phone**

Para este tipo de consentimiento no contamos con un texto legal, sin
embargo este indica si una persona se ha opuesto a recibir
comunicaciones publicitarias o comerciales a su número de teléfono.

  -------------------------------------------------------------------------------------------------------
  **Bloque de       **Nombre atributo**     **Descripción**            **Tipo**   **Nullable**   **PK**
  información**                                                                                  
  ----------------- ----------------------- -------------------------- ---------- -------------- --------
  Datos del         ID                      Id único                   Text       No             x
  consentimiento                                                                                 

  Datos del cliente COD_CLIENTE             Código de cliente          Text       No             

  Datos del cliente COD_SOCIEDAD            Código de sociedad         Text       No             

  Datos del cliente TELEFONO_CORTO          Teléfono                   Text       Si             

  Datos del cliente TELEFONO_LARGO          FormattedE164PhoneNumber   Text       Si             

  Datos del cliente prefijo_TELEFONO        PhoneCountryCode           Text       Si             

  Datos del cliente pais_TELEFONO           Country Phone              Text       Si             

  Consentimientos   IND_ROBINSON_ADIGITAL   Indicador de oposición a   Text       No             
  robinson                                  las comunicaciones                                   
                                            teléfono                                             

  Consentimientos   FECHA_CONSULTA          Fecha Auditoria Adigital   Datetime   No             
  robinson                                                                                       

  Consentimientos   PROXIMA_CONSULTA                                   Datetime   No             
  robinson                                                                                       

  Datos del         AUDITORIA               Última modificación        Datetime   No             
  consentimiento                                                                                 
  -------------------------------------------------------------------------------------------------------

Se consumirá un fichero **CSV** en formato UTF-8 delimitado por comas y
con comillas dobles en cada valor, con una frecuencia diaria e
incremental, con lo que se solo se depositarán los ficheros con los
cambios en datos de consentimientos de cliente cada día. Estos ficheros
además contendrán una **fecha de modificación del registro** de
consentimientos, para tener una trazabilidad de dichos registros.

El nombre del fichero depositado tendrá la siguiente forma (Entidad +
'\_' + timestamp):

*Phone_20230101140000.csv*

En la configuración de entrada del SFTP deberemos indicar:

-   **Conexión:** Indicaremos el nombre de la conexión del SFTP creada.

-   **Directorio.** Guardaremos cada archivo en su carpeta
    correspondiente asociada a la entidad que aplica.

-   **Nombre de fichero el siguiente wildcard:** Phone\_\*.csv

-   **Source.** Indicaremos el maestro de donde proceden los datos. En
    este caso indicaremos sftp_sas_delta.

La **categoría seleccionada para el DLO** a ingestar en Data Cloud será
**Other**. La **Primary Key** de estos registros será **Id**, un nuevo
campo de entrada, el cual es un código que identifica unívocamente a un
cliente dentro de Iberdrola. **Record Modified Field** será el campo
auditoría del fichero de entrada y **Organization Unit Identifier** lo
dejaremos vacío.

Para este Data Stream de ingesta de datos de cliente deberemos generar
las siguientes transformaciones o creación de nuevos campos de entrada
(New Formula Field):

1.  **Consent Phone Status (Text).** Permite identificar si el cliente
    ha dado o no su consentimiento. Si el campo IND_OPOSI_TELF es igual
    a "N" el valor será "Opt In" si no será "Opt Out".

2.  **Contact Point Consent Name (Text).** Que será texto fijo igual a
    "Contact Point Phone Consent".

3.  **Effective To Date (Datetime).** Será una fecha fija igual a
    DATE("2999", "12", "31").

### CATs (SFTP)

El acuerdo de interfaz comprometido para el fichero CSV que se
depositará en una instancia de SFTP es el siguiente:

  -----------------------------------------------------------------------------------------------------------------
  **Bloque de        **Nombre           **Descripción**   **Tipo**   **Nullable**   **PK**   **Maestro**
  información**      atributo**                                                              
  ------------------ ------------------ ----------------- ---------- -------------- -------- ----------------------
  Comportamiento     URL_FRONTEND       Url del frontend  Text       Si                      CATUSER.CATS_BROWSER
  digital                                                                                    

  Comportamiento     FECHA_NAVEGACIÓN   Datetime del      Datetime   No                      CATUSER.CATS_BROWSER
  digital                               evento                                               

  Comportamiento     NAVEGADOR          Navegador del     Text       Si                      CATUSER.CATS_SESSION
  digital                               evento                                               

  Comportamiento     DISPOSITIVO        Dispositivo       Text       Si                      CATUSER.CATS_SESSION
  digital                                                                                    

  Comportamiento     IP                 Ip                Text       Si                      CATUSER.CATS_SESSION
  digital                                                                                    

  Comportamiento     COD_SESION         Código de sesión  Text       No                      CATUSER.CATS_BROWSER
  digital                                                                                    

  Comportamiento     TRACKING_COOKIE    Cookie de Cats    Text       No                      CATUSER.CATS_SESSION
  digital                                                                                    

  Comportamiento     APPLICATION        Aplicación        Text       Si                      CATUSER.CATS_SESSION
  digital                                                                                    

  Datos del Cliente  COD_USUARIO        Código de usuario Text       Si                      CATUSER.CATS_BROWSER

  Datos del Cliente  COD_CLIENTE        Código de cliente Text       Si                      CATUSER.CATS_BROWSER

  Datos del Cliente  COD_CONTRATO       Código de         Text       Si                      CATUSER.CATS_BROWSER
                                        contrato                                             

  **Comportamiento   **cod_event**      **Código de       **Text**   **No**         **X**    **Calculado**
  digital**                             evento**                                             

  Comportamiento     action_type        Tipo de acción    Text       No                      Calculado
  digital                                                                                    

  Comportamiento     descPaqueteWeb     Descripción del   Text       Si                      Calculado
  digital                               paquete web                                          

  Comportamiento     codPaqueteWeb      Código del        Text       Si                      Calculado
  digital                               paquete web                                          

  Comportamiento     codOfertaWeb       Código de oferta  Text       Si                      Calculado
  digital                               Web                                                  

  Comportamiento     codOfertaDelta     Código de oferta  Text       Si                      Calculado
  digital                               Delta                                                

  Comportamiento     responseCode       Código de         Text       Si                      
  digital                               respuesta del                                        
                                        servicio                                             

  Datos del Cliente  email              email             Text       Si                      Calculado

  Datos del Cliente  telefono           Teléfono          Text       Si                      Calculado

  Datos del Cliente  numDocumento       Número de         Text       Si                      Calculado
                                        documento DNI/CIF                                    
  -----------------------------------------------------------------------------------------------------------------

Se consumirá un fichero **CSV** en formato UTF-8 delimitado por comas y
con comillas dobles en cada valor, con una frecuencia diaria e
incremental, con lo que se solo se depositarán los ficheros con los
cambios en datos de navegación digital (únicamente web para estas
pruebas) cada día. Estos ficheros contienen una fecha

El nombre del fichero depositado tendrá la siguiente forma (Entidad +
'\_' + timestamp):

*Cats_20230101140000.csv*

En la configuración de entrada del SFTP deberemos indicar:

-   **Conexión**: Indicaremos el nombre de la conexión del SFTP creada.

-   **Directorio**. Guardaremos cada archivo en su carpeta
    correspondiente asociada a la entidad que aplica.

-   **Nombre de fichero el siguiente wildcard**: Cats\_\*.csv

-   **Source**. Indicaremos el maestro de donde proceden los datos. En
    este caso indicaremos cats_sidat_sftp.

La **categoría seleccionada para el DLO** a ingestar en Data Cloud será
**Other**. La **Primary Key** de estos registros será el **cod_event**,
que es un código que identifica unívocamente el evento. **Record
Modified Field** será el campo FECHA_NAVEGACIÓN que será un timestamp de
la fecha y hora cuando ocurrió el evento. **Organization Unit
Identifier** lo dejaremos vacío.

Para este Data Stream de ingesta de datos de cliente deberemos generar
las siguientes **transformaciones** o creación de nuevos campos de
entrada **(New Formula Field)**:

1.  **Party Identification Type (Text)**. Que será texto fijo igual a
    "Cookie Identifier".

2.  **Identification Name (Text).** Que será texto fijo igual a "Cookie
    Cats".

3.  **Party Identification Id (Text).** Se calculará a partir de
    concatenar COD_CLIENTE y TRACKING_COOKIE. En caso de no tener el
    COD_CLIENTE, el Party Identification Id será la TRACKING_COOKIE.
    Será la PK del objeto Party Identification.

4.  **Email Domain (Text).** Se extraerá el dominio de correo
    electrónico del atributo de entrada email a través de la siguiente
    expresión regular:
    @(\[a-zA-Z0-9-\]+.)\*\[a-zA-Z0-9-\]+.\[a-zA-Z\]{2,}

5.  **Contact Point Email Id (Text).** Se calculará a partir de la suma
    de COD_CLIENTE más email y será la PK del objeto Contact Point
    Email.

6.  **Formatted E164 Phone Number (Text).** Se calculará a partir de la
    suma del prefijo "34" más teléfono.

7.  **Contact Point Phone Id (Text).** Se calculará a partir de la suma
    de COD_CLIENTE más telefono y será la PK del objeto Contact Point
    Phone.

8.  **Is Anonymous (Text).** Indicará si un perfil es conocido o no. Si
    el campo COD_CLIENTE se encuentra vacío el valor será a "1", si no
    el valor será "0".

9.  **Party (Text).** Si el campo COD_CLIENTE se encuentra vacío tomará
    el valor de la TRACKING_COOKIE, si no el valor será el COD_CLIENTE.
    Será la PK del objeto Individual.

Además al configurar el Data Stream introduciremos los siguientes
parámetros de configuración:

-   Modo de refresco: Upsert. Aunque el fichero sea incremental y solo
    venga registros nuevos o modificados.

-   Refrescar solo ficheros nuevos

-   Emitir fallo si no encuentra el fichero cada día

-   Hora de actualización: No se está depositando datos de CATs en el
    CDP, ya que se están capturando las interacciones web mediante
    Interaction Studio.

### Delio (SFTP)

El acuerdo de interfaz comprometido para el fichero CSV que se
depositará en una instancia de SFTP es el siguiente:

  -------------------------------------------------------------------------------------------------------------------
  **Bloque de      **Nombre atributo**         **Descripción**       **Tipo**   **Nullable**   **PK**   **Maestro**
  información**                                                                                         
  ---------------- --------------------------- --------------------- ---------- -------------- -------- -------------
  Calificación del CATEGORIA_PRODUCTO          Categoría del         Text       Si                      Text
  Lead                                         producto                                                 

  Calificación del COD_CLIENTE                 Código de cliente     Text       Si                      Text
  Lead                                                                                                  

  Calificación del CUALIFICACION_FUNNEL        Cualification Funnel  Text       Si                      Text
  Lead                                                                                                  

  Calificación del ESTADO                      Estado                Text       Si                      Text
  Lead                                                                                                  

  Calificación del GRH_CAMPAIGN                Campaña               Text       Si                      Text
  Lead                                                                                                  

  Calificación del ISCLIENT                    Es cliente            Text       No                      Text
  Lead                                                                                                  

  Calificación del ORIGEN_LEAD                 Origen Lead           Text       No                      Text
  Lead                                                                                                  

  Calificación del PRODUCTO_URL                PRODUCTO_URL          Text       Si                      Text
  Lead                                                                                                  

  Calificación del ULT_CODIFICACION            Última codificación   Text       Si                      Text
  Lead                                         para el estado                                           

  Calificación del URL_SOURCE                  url_source            Text       Si                      Text
  Lead                                                                                                  

  **Calificación   **UTILIDAD_DE_REGISTRO**    **Utilidad de         **Text**   **Si**                  Text
  del Lead**                                   registro para el                                         
                                               estado**                                                 

  Datos de         PAIS_TEL_LLAMADO            País teléfono llamado Text       Si                      Text
  contacto                                                                                              

  Datos de         pais_telefono               País teléfono         Text       Si                      Text
  contacto                                                                                              

  Datos de         PREFIJO_TELEF_LLAMADO       Prefijo teléfono      Text       Si                      Text
  contacto                                     llamado                                                  

  Datos de         prefijo_telefono            Prefijo teléfono      Text       Si                      Text
  contacto                                                                                              

  **Datos de       **SALESFORCE_CATS**         **salesforce_cats**   **Text**   **Si**                  **Text**
  contacto**                                                                                            

  Datos de         SALESFORCE_CATSTEMP         salesforce_catstemp   Text       Si                      Text
  contacto                                                                                              

  Datos de         SALESFORCE_EVGA             salesforce_evga       Text       Si                      Text
  contacto                                                                                              

  Datos de         TELEF_LLAMADO_CORTO         Teléfono llamado      Text       Si                      Text
  contacto                                     corto                                                    

  Datos de         TELEF_LLAMADO_LARGO         Teléfono llamado      Text       Si                      Text
  contacto                                     largo                                                    

  Datos de         TELEFONO_CORTO              Teléfono corto        Text       Si                      Text
  contacto                                                                                              

  Datos de         TELEFONO_LARGO              Teléfono largo        Text       Si                      Text
  contacto                                                                                              

  Datos generales  auditoria                   Última modificación   datetime   No                      datetime
  del Lead                                                                                              

  Datos generales  FECHA_DE_ACTUALIZACION      Fecha de              Datetime   No                      Datetime
  del Lead                                     actualización                                            

  Datos generales  FECHA_DE_ALTA               Fecha de alta         Datetime   No                      Datetime
  del Lead                                                                                              

  **Datos          **LEADID**                  **Id del lead**       **Text**   **No**         **X**    **Text**
  generales del                                                                                         
  Lead**                                                                                                

  Información de   CODIGO_OFERTA_1             Código de oferta 1    Text       Si                      Text
  Producto                                                                                              

  Información de   CODIGO_OFERTA_2             Código de oferta 2    Text       Si                      Text
  Producto                                                                                              

  Información de   CODIGO_OFERTA_3             Código de oferta 3    Text       Si                      Text
  Producto                                                                                              

  Información de   CODIGO_OFERTA_4             Código de oferta 4    Text       Si                      Text
  Producto                                                                                              

  Información de   CODIGO_OFERTA_5             Código de oferta 5    Text       Si                      Text
  Producto                                                                                              

  Información de   CODIGO_OFERTA_6             Código de oferta 6    Text       Si                      Text
  Producto                                                                                              

  Información de   CODIGO_OFERTA_7             Código de oferta 7    Text       Si                      Text
  Producto                                                                                              

  Información de   CONT_CAMBIO_PLAN            Contador de cambio de Text       Si                      Text
  Producto                                     plan                                                     

  Información de   CONT_SMART_CLIMA            Contador smart clima  Number                             Number
  Producto                                                                                              

  Información de   CONT_SMART_MOBILITY         Contador smart        Number                             Number
  Producto                                     mobility                                                 

  Información de   CONT_SMART_SOLAR            Contador smart solar  Number                             Number
  Producto                                                                                              

  Información de   NUM_PRODUCT_CONTRATA        Número de productos   Number                             Number
  Producto                                     contratados                                              

  Información de   NUM_PRODUCT_ENERGETICO      Número de productos   Number                             Number
  Producto                                     energéticos                                              

  Información de   NUM_PRODUCTOS_SMART         Número de productos   Number                             Number
  Producto                                     smart                                                    

  Información de   NUM_PRODUCTOS_Y_SERVICIOS   Número de productos y Number                             Number
  Producto                                     servicios                                                
  -------------------------------------------------------------------------------------------------------------------

Se consumirá un fichero **CSV** en formato UTF-8 delimitado por comas y
con comillas dobles en cada valor, con una frecuencia diaria e
incremental, con lo que se solo se depositarán los ficheros con los
cambios en interacciones en Delio de usuarios (cliente y no cliente)
cada día. Estos ficheros además contendrán una **fecha de modificación
del registro** de delio, para tener una trazabilidad de dichos
registros.

El nombre del fichero depositado tendrá la siguiente forma (Entidad +
'\_' + timestamp):

*Delio_20230101140000.csv*

En la configuración de entrada del SFTP deberemos indicar:

-   **Conexión**: Indicaremos el nombre de la conexión del SFTP creada.

-   **Directorio**. Guardaremos cada archivo en su carpeta
    correspondiente asociada a la entidad que aplica.

-   **Nombre de fichero el siguiente wildcard**: Delio\_\*.\*.csv

-   **Source**. Indicaremos el maestro de donde proceden los datos. En
    este caso indicaremos delio_sidat_sas_sftp.

La **categoría seleccionada para el DLO** a ingestar en Data Cloud será
**Other**. La **Primary Key** de estos registros será el LEADID, que es
un código que identifica unívocamente una interacción en Delio. **Record
Modified Field** será el campo Fecha de actualización del fichero de
entrada y **Organization Unit Identifier** lo dejaremos vacío.

Para este Data Stream de ingesta de datos de Delio de usuarios deberemos
generar las siguientes **transformaciones**:

1.  **Is Anonymous (Text):** permitirá identificar si un cliente es
    conocido o no. Si el campo isClient es igual a "No" el valor será
    "1", si no tomará el valor "0".

2.  **Party (Text):** si el campo isClient es igual a "Si" el valor será
    el cod_client, si no el valor será el LeadId.

3.  **Contact Point Phone Id (Text):** si el campo isClient es igual a
    "Si" el valor será el cod_client, si no el valor será el LeadId +
    telefono.

4.  **Party Identification Type (Text)**: Que será texto fijo igual a
    "Cookie Identifier".

5.  **Identification Name (Text):** Que será texto fijo igual a "Cookie
    Cats".

6.  **Party Identification Id (Text):** si el campo isClient es igual a
    "Si" el valor será el cod_client, si no el valor será el LeadId +
    salesforce_cats.

Además al configurar el Data Stream introduciremos los siguientes
parámetros de configuración:

-   Modo de refresco: Upsert. Aunque el fichero sea incremental y solo
    venga registros nuevos o modificados.

-   Refrescar solo ficheros nuevos

-   Emitir fallo si no encuentra el fichero cada día

-   Hora de actualización diaria a las 10:00 UTC. Por lo que el fichero
    desde SAS debe depositarse antes en el SFTP.F

### Interaction Studio (IS Connector)

Para estos Data Stream de Interaction Studio a día de hoy no se tiene un
acuerdo de interfaz comprometido, al todavía no estar implantada la
herramienta. El IS Bundle estándar contiene 6 Data Streams que
procederán de un único dataset que engloba Web pública, MAC y app de
Iberdrola, pero en el caso de Iberdrola únicamente utilizaremos 2:

1.  Usuarios que consiste en datos de usuario que se mapean contra
    Individual, Email Contact Point y Party Identification. Cuidado que
    por defecto Data Cloud mapea el IS "Profile Id" al CDP "Individual
    ID" y también el "Salesforce Marketing Cloud Contact Key" a "Party
    Identification", pero en el caso de Iberdrola usaremos nuestro
    propio customer ID (Party Identifier) mapeado contra
    PartyIdentifcation.IDentificationNumber

2.  Base Events Data Stream. De donde se recogerán todos los eventos
    base del dataset configurado. Vendrá informado el tipo de acción
    realizada, la url visitada, el email, etc.

3.  Catalog Events Data Stream. De donde se recogerán los eventos
    relacionados a las interacciones de los usuarios con los productos.
    Vendrá informado el producto, fecha de la interacción, la url, etc.

### Marketing Cloud (MKTC Connector)

Desde Salesforce Marketing Cloud se conectarán los dos bundles por
defecto que se tienen:

-   Email Studio

-   Mobile Studio

Estos Data Bundles generan Data Streams por defecto que son:

-   Campaign. Tabla maestra de campañas que engloba los Journeys
    configurados.

-   Journey. Tabla que define los journeys presentes en SFMC. Estos
    engloban varios canales (emails, push, sms)

-   Journey Activity. Cada uno de los pasos dentro del journey.

-   Contact Point Email que relaciona el Subscriber Id con el email del
    cliente.

-   Einstein Email Scoring para categorizar y puntuar la probabilidad de
    apertura, clicks de emails, etc.

-   Email Compliant donde se almacenan todas las comunicaciones de email
    que se han enviado y se han quedado en Spam.

-   Email Engagement Bounce donde se almacenan todos los rebotes, es
    decir las comunicaciones de emails que no han llegado a la bandeja
    de entrada del usuario.

-   Email Engagement Click donde se almacenan todos los clicks sobre los
    emails enviados.

-   Email Engagement Open donde se almacenan todos las aperturas sobre
    los emails enviados.

-   Email Engagement Send donde se almacenan todos los envíos de emails.

-   Email Publication donde se almacenan todas las publicaciones de
    emails desde SFMC.

-   Email Template donde se almacena cada tipo y plantilla de email
    enviado con asunto del email, el nombre etc.

Es importante tener en cuenta que Marketing Cloud utiliza su Id único de
cliente, que se deberá configurar como el cod_cliente (identificador
único de cliente en Iberdrola), por lo que podremos mapear este atributo
contra el Individual Id del objeto Individual de Data Cloud.

## Mappings

En este apartado vamos a detallar y listar cada uno de los mapeos que
realizaremos en los Data Streams de Data Cloud.

Se mostrará una tabla por entidad de ingesta en CDP (Cliente,
Contrato...), dividido por apartados, en la que tendremos las columnas:

1.  Variable. Campo de entrada en el Data Stream

2.  Tipo. Tipo de campo de entrada (Text, Number, Date o Datetime)

3.  Cálculo. Si es un campo calculado a través de la entrada de datos se
    marcará en verde y esta columna vendrá informada con la lógica de
    dicho cálculo

4.  PK. Se marcará con una X la Clave primaria que identifica
    unívocamente al registro de entrada.

5.  Mapping. Que se informa con el DMO.Atributo del DMO, donde si se
    formatea en cursiva el texto indica que es un objeto o atributo
    personalizado. entre paréntesis se marcará con PK si es la clave
    primaria del objeto destino (DMO mapeado).

El modelo de datos a utilizar dentro de Data Cloud es el que se muestra
en la siguiente imagen:

![](media/image18.png){width="6.267716535433071in"
height="3.5277777777777777in"}

Importante destacar que el objeto **Lead** (estándar de Data Cloud) es
de tipo Engagement y lo que representa es una intención de compra de un
usuario (cliente o no) con Iberdrola. Por lo tanto es relación N a 1 con
Individual, pero de manera opcional porque puede haber Leads sin
relación con un cliente.

Por otro lado, todos los atributos que categorizan y segmentan al
individuo irán como atributos personalizados dentro del objeto estándar
**Individual**.

Por último también, se construirá un objeto personalizado **Digital
Asset Engagement** y basado en el objeto estándar Website Engagement que
incluirá todas las interacciones del usuario con los activos digitales
de Iberdrola (Web pública, mac y app).

Por último, para los consentimientos se utilizarán los objetos:

-   **Contact Point Consent**. Para almacenar el consentimiento concreto
    que ha aceptado o no el cliente sobre que se le comunique por un
    punto de contacto concreto como email o teléfono.

-   **Party Consent**. Para almacenar los consentimientos que acepta o
    no el cliente. Este consentimiento va a nivel de cliente y no de
    punto de contacto, ej: Existe un consentimiento expreso de que el
    cliente se opone a que Iberdrola utilice sus datos personales para
    segmentarle en una audiencia y activarle. Este consentimiento irá
    mapeado en esta entidad.

### Cliente

+----------+---------------+----+------------+---+---------------------+
| **Nombre | **            | ** | *          | * | **Mapping**         |
| At       | Descripción** | Ti | *Cálculo** | * |                     |
| ributo** |               | po |            | P |                     |
|          |               | ** |            | K |                     |
|          |               |    |            | * |                     |
|          |               |    |            | * |                     |
+==========+===============+====+============+===+=====================+
| an       | Antiguedad    | Te |            |   | Individual.Client   |
| tiguedad | del cliente   | xt |            |   | Seniority           |
| _cliente |               |    |            |   |                     |
+----------+---------------+----+------------+---+---------------------+
| a        | Apellidos     | Te |            |   | Individual.Last     |
| pellidos |               | xt |            |   | Name                |
| _cliente |               |    |            |   |                     |
+----------+---------------+----+------------+---+---------------------+
| a        | Última        | da |            |   | Individual.Last     |
| uditoria | modificación  | te |            |   | Modified Date       |
|          |               | ti |            |   |                     |
|          |               | me |            |   |                     |
+----------+---------------+----+------------+---+---------------------+
| clase    | Clase de      | Te |            |   | Individual.Customer |
| _cliente | cliente       | xt |            |   | Type                |
+----------+---------------+----+------------+---+---------------------+
| cla      | Posible       | Nu |            |   | Individual.Possible |
| sif_pote | segunda       | mb |            |   | Second Home         |
| ncial_2_ | vivienda      | er |            |   |                     |
| vivienda |               |    |            |   |                     |
+----------+---------------+----+------------+---+---------------------+
| C        | CNAE del      | Te |            |   | Individual.CNAE     |
| NAE_clie | cliente       | xt |            |   |                     |
| nte_desc |               |    |            |   |                     |
+----------+---------------+----+------------+---+---------------------+
| COD      | Cod_cliente   | Te |            | X | In                  |
| _CLIENTE |               | xt |            |   | dividual.Individual |
|          |               |    |            |   | Id (PK)             |
|          |               |    |            |   |                     |
|          |               |    |            |   | I                   |
|          |               |    |            |   | ndividual.*Customer |
|          |               |    |            |   | Id*                 |
|          |               |    |            |   |                     |
|          |               |    |            |   | Contact Point       |
|          |               |    |            |   | Address.Party       |
|          |               |    |            |   |                     |
|          |               |    |            |   | Party               |
|          |               |    |            |   | I                   |
|          |               |    |            |   | dentification.Party |
|          |               |    |            |   |                     |
|          |               |    |            |   | Party               |
|          |               |    |            |   | Identifica          |
|          |               |    |            |   | tion.Identification |
|          |               |    |            |   | Number              |
+----------+---------------+----+------------+---+---------------------+
| DES      | CCAA fiscal   | Te |            |   | Contact Point       |
| _CCAA_FS |               | xt |            |   | Address.Country     |
|          |               |    |            |   | Region              |
+----------+---------------+----+------------+---+---------------------+
| DES_POBL | Poblacion     | Te |            |   | Contact Point       |
| ACION_FS | fiscal        | xt |            |   | Address.City        |
+----------+---------------+----+------------+---+---------------------+
| DES_PROV | Provincia     | Te |            |   | Contact Point       |
| INCIA_FS | fiscal        | xt |            |   | Address.State       |
|          |               |    |            |   | Province            |
+----------+---------------+----+------------+---+---------------------+
| DIRE     | Dirección     | Te |            |   | Contact Point       |
| CCION_FS | fiscal        | xt |            |   | Address.Addresss    |
|          |               |    |            |   | Line 1              |
+----------+---------------+----+------------+---+---------------------+
| EDAD     | Edad          | Te |            |   | Individual.Age      |
|          |               | xt |            |   | Range               |
+----------+---------------+----+------------+---+---------------------+
| FEC_ALTA | Fecha de alta | Da |            |   | Individual.First    |
| _CLIENTE | del cliente   | te |            |   | Contract Date       |
+----------+---------------+----+------------+---+---------------------+
| GRAD     | Grado         | Te |            |   | Indivi              |
| O_DIGITA | d             | xt |            |   | dual.Digitalization |
| LIZACION | igitalización |    |            |   | Degree              |
|          | con Iberdrola |    |            |   |                     |
+----------+---------------+----+------------+---+---------------------+
| ind_baja | Baja anterior | Nu |            |   | Individual.Previous |
| _anterio | por Cambio de | mb |            |   | reduction due to    |
| r_CC_ele | Come          | er |            |   | Change of Supplier  |
|          | rcializador - |    |            |   | Electricity         |
|          | Electricidad  |    |            |   |                     |
+----------+---------------+----+------------+---+---------------------+
| ind_baja | Baja anterior | Nu |            |   | Individual.Previous |
| _anterio | por Cambio de | mb |            |   | reduction due to    |
| r_CC_gas | Come          | er |            |   | Change of Supplier  |
|          | rcializador - |    |            |   | Gas                 |
|          | Gas           |    |            |   |                     |
+----------+---------------+----+------------+---+---------------------+
| IN       | Indicador     | Nu |            |   | In                  |
| D_CLI_VU | cliente       | mb |            |   | dividual.Vulnerable |
| LNERABLE | Vulnerable    | er |            |   | Client              |
+----------+---------------+----+------------+---+---------------------+
| IND_EMPL | Indicador de  | Nu |            |   | I                   |
| EADO_IBD | ser empleado  | mb |            |   | ndividual.Iberdrola |
|          | de Iberdrola  | er |            |   | Employee            |
+----------+---------------+----+------------+---+---------------------+
| IND_VIP  | Indicador     | Nu |            |   | Individual.VIP      |
|          | cliente VIP   | mb |            |   | Client              |
|          |               | er |            |   |                     |
+----------+---------------+----+------------+---+---------------------+
| MERCA    | Mercado       | Te |            |   | Individual.Client   |
| DO_SOLVE | Solvencia     | xt |            |   | Solvency            |
| NCIA_CLI | Cliente       |    |            |   |                     |
+----------+---------------+----+------------+---+---------------------+
| me       | Meses desde   | Nu |            |   | Individual.Months   |
| ses_baja | la última     | mb |            |   | since the last      |
| _anterio | baja por      | er |            |   | reduction due to    |
| r_CC_ele | Cambio de     |    |            |   | Change of Supplier  |
|          | Come          |    |            |   | Electricity         |
|          | rcializador - |    |            |   |                     |
|          | Electricidad  |    |            |   |                     |
+----------+---------------+----+------------+---+---------------------+
| me       | Meses desde   | Nu |            |   | Individual.Months   |
| ses_baja | la última     | mb |            |   | since the last      |
| _anterio | baja por      | er |            |   | reduction due to    |
| r_CC_gas | Cambio de     |    |            |   | Change of Supplier  |
|          | Come          |    |            |   | Gas                 |
|          | rcializador - |    |            |   |                     |
|          | Gas           |    |            |   |                     |
+----------+---------------+----+------------+---+---------------------+
| N/A      | Contact Point | Te | cod        |   | Contact Point       |
|          | Address Id    | xt | _cliente + |   | Address.Contact     |
|          |               |    | Dirección  |   | Point Address Id    |
|          |               |    | fiscal     |   | (PK)                |
+----------+---------------+----+------------+---+---------------------+
| N/A      | I             | Te | MC         |   | Party               |
|          | dentification | xt | Subscriber |   | Identifica          |
|          | Name          |    | Key        |   | tion.Identification |
|          |               |    |            |   | Name                |
+----------+---------------+----+------------+---+---------------------+
| N/A      | Party         | Te | c          |   | Party               |
|          | I             | xt | od_cliente |   | I                   |
|          | dentification |    |            |   | dentification.Party |
|          | Id            |    |            |   | Identification Id   |
|          |               |    |            |   | (PK)                |
+----------+---------------+----+------------+---+---------------------+
| N/A      | Party         | Te | Person     |   | Party               |
|          | I             | xt | Identifier |   | I                   |
|          | dentification |    |            |   | dentification.Party |
|          | Type          |    |            |   | Identification Type |
+----------+---------------+----+------------+---+---------------------+
| NOM      | Nombre        | Te |            |   | Individual.First    |
| _CLIENTE |               | xt |            |   | Name                |
+----------+---------------+----+------------+---+---------------------+
| N        | NIF           | Te |            |   | Individ             |
| UM_DCO_I |               | xt |            |   | ual.Indentification |
| DENTIDAD |               |    |            |   | Number              |
+----------+---------------+----+------------+---+---------------------+
| N        | Reclamaciones | Nu |            |   | Individual.Claims   |
| UM_REC_A | abiertas      | mb |            |   | Past Year           |
| BIERTAS_ | ultimo año    | er |            |   |                     |
| ANIO_CLI |               |    |            |   |                     |
+----------+---------------+----+------------+---+---------------------+
| NUM_     | Reclamaciones | Nu |            |   | Individual.Open     |
| REC_ABIE | abiertas      | mb |            |   | Claims              |
| RTAS_CLI |               | er |            |   |                     |
+----------+---------------+----+------------+---+---------------------+
| NUM_R    | Reclamaciones | Nu |            |   | Individual.Total    |
| ECLAMACI | totales       | mb |            |   | Claims              |
| ONES_CLI |               | er |            |   |                     |
+----------+---------------+----+------------+---+---------------------+
| pais     | País          | Te |            |   | Contact Point       |
|          |               | xt |            |   | Address.Country     |
|          |               |    |            |   |                     |
|          |               |    |            |   | I                   |
|          |               |    |            |   | ndividual.Residence |
|          |               |    |            |   | Country             |
+----------+---------------+----+------------+---+---------------------+
| PERFIL   | Perfil        | Te |            |   | Indivi              |
| _DIGITAL | d             | xt |            |   | dual.Digitalization |
|          | igitalización |    |            |   | Profile             |
+----------+---------------+----+------------+---+---------------------+
| PERF     | Perfil        | Te |            |   | Indiv               |
| IL_PSICO | psicográfico  | xt |            |   | idual.Psychographic |
|          |               |    |            |   | Profile             |
+----------+---------------+----+------------+---+---------------------+
| PROB_    | Probabilidad  | Te |            |   | Individual.Churn    |
| ABANDONO | de abandono   | xt |            |   | Probability         |
| _CAT_CLI |               |    |            |   |                     |
+----------+---------------+----+------------+---+---------------------+
| segmento | Segmento      | Te |            |   | Individual.Segment  |
|          |               | xt |            |   |                     |
+----------+---------------+----+------------+---+---------------------+
| tipo_nba | Tipo NBA      | Te |            |   | Individual.NBA Type |
|          |               | xt |            |   |                     |
+----------+---------------+----+------------+---+---------------------+
| ind_impa | Ha recibido   | Nu |            |   | Individual.Received |
| ctado_MI | c             | mb |            |   | MI Communications   |
|          | omunicaciones | er |            |   |                     |
|          | Mi Iberdrola  |    |            |   |                     |
+----------+---------------+----+------------+---+---------------------+
| ind_adh  | Está adherido | Nu |            |   | Individual.Mi       |
| erido_MI | a Mi          | mb |            |   | Iberdrola Indicator |
|          | Iberdrola     | er |            |   |                     |
+----------+---------------+----+------------+---+---------------------+

### 

Importante señalar que cuando se crean **objetos custom**, primero hay
que mapearlos contra un Data Stream para crear relaciones de estos
objetos con los estándar. El **país** del cliente es importante señalar
que lo mapearemos contra la dirección fiscal del mismo. Además las
**fechas y horas** de entrada en Data Cloud vendrán informadas en
formato ISO indicando el timezone yyyy-MM-dd\'T\'HH:mm:ss.SSSZ. Ej:
2023-01-08T15:30:45.000+0200 si estamos en horario de verano y
2023-01-08T15:30:45.000+0100 cuando es horario de invierno.

### Contrato

  -----------------------------------------------------------------------------------------------------------
  **Nombre Atributo**            **Descripción**   **Tipo**   **Cálculo**   **PK**   **Mapping**
  ------------------------------ ----------------- ---------- ------------- -------- ------------------------
  DES_CANAL_CONTRATACION         Canal de          Text                              Contract.Engagement
                                 contratación                                        Channnel

  DES_CCAA_CO                    CCAA              Text                              Contract.Autonomous
                                 Correspondencia                                     Community

  DES_CCAA_PS                    CCAA Punto        Text                              Contract.Supply Point
                                 Suministro                                          Autonomous Community

  cluster_segm_unica             Cluster           Text                              Contract.Unique Cluster
                                 Segmentacion                                        Segmentation
                                 Unica                                               
                                 Particulares                                        

  COD_CLIENTE                    cod_cliente       Text                              Contract.Individual Id

  COD_CONTRATO                   Cod_contrato      Number                   X        Contract.Contract Id
                                                                                     (PK)

  COD_SOCIEDAD                   cod_sociedad      Number                            Contract.Society Id

  COD_POSTAL_PS                  Codigo Postal                                       Contract.Postal Code
                                 Punto Suministro                                    Supply Point

  VAL_CSU_12_MESES               Consumo últimos   Number                            Contract.Consumption
                                 12 meses                                            Last 12 Months

  CUPS                           CUPS              Text                              Contract.CUPS

  DES_PLAN                       descripcion del                                     Contract.Plan
                                 plan                                                Description

  des_grupo_economico            Descripcion grupo Text                              Contract.Economic Group
                                 economico                                           

  DESC_TIPO_SMART_SOLAR          Descriptivo       Text                              Contract.Smart Solar
                                 Indicador smart                                     Indicator
                                 solar                                               

  DESC_TIPO_VEHICULO_ELECTRICO   Descriptivo       Text                              Contract.Electric
                                 Indicador                                           Vehicle Indicator
                                 vehículo                                            
                                 eléctrico                                           

  DIRECCION_COMPLETA_CORRES      Direccion         Text                              Contract.Mailing Address
                                 Correspondencia                                     

  DIRECCION_COMPLETA_PS          Direccion Punto   Text                              Contract.Supply Point
                                 Suministro                                          Address

  TIP_EST_CONTRATO               Estado contrato   Text                              Contract.Status

  FEC_ALTA_CONTRATO              Fecha alta        Date                              Contract.Start Date
                                 contrato                                            

  FEC_BAJA_CTO                   Fecha de baja     Date                              Contract.Disconnection
                                                                                     Date

  FEC_EGC_INI_CONTTO             Fecha de enganche Date                              Contract.Connection Date

  FEC_ULT_RECLA                  Fecha de la       datetime                          Contract.Last Claim Date
                                 última                                              
                                 reclamación                                         

  FEC_RENOVACION                 Fecha de última   Date                              Contract.Last Renovation
                                 renovación                                          Date

  TIP_IDIOMA_FACTU               Idioma preferente Text                              Contract.Preferred
                                                                                     Language

  IMPORTE_FACTU_12_MESES         Importe factura   Number                            Contract.Invoice Amount
                                 12 meses                                            12 Months

  IMPORTE_FACTU_MEDIA_MENSUAL    Importe factura   Number                            Contract.Average
                                 media mensual                                       Montthly Invoice Amount

  ind_aire_acondicionado         Indicador Aire    Number                            Contract.Air
                                 Acondicionado                                       Conditioning Indicator

  ind_calefaccion_electrica      Indicador         Number                            Contract.Heating
                                 Calefaccion                                         Indicator
                                 electrica                                           

  IND_CLI_MOROSO                 Indicador Cliente Text                              Contract.Past Due
                                 Moroso                                              Cliente Indicator

  IND_DOMICILIADO                Indicador de      Number                            Contract.Domiciliation
                                 domiciliado                                         Indicator

  ind_fe                         Indicador de fact Number                            Contract.Electronic Bill
                                 ele                                                 Indicator

  ind_posible_2v                 Indicador posible Number                            Contract.Possible Second
                                 segunda vivienda                                    Home

  ind_vivienda_alquiler          Indicador         Number                            Contract.Renting
                                 Vivienda alquiler                                   Indicator

  IND_MONOFINCAS                 Indicar Monofinca Number                            Contract.House Indicator

  MERCADO_SOLVENCIA_CTO          Mercado solvencia Text                              Contract.Solvency Cto
                                 Cto                                                 

  pais                           Pais              Text                              Contract.Country

  DES_POBLACION_CO               Poblacion         Text                              Contract.City
                                 Correspondencia                                     Correspondence

  DES_POBLACION_PS               Poblacion Punto                                     Contract.City Supply
                                 Suministro                                          Point

  COD_POSTAL_CO                  Postal            Text                              Contract.Postal
                                 Correspondencia                                     Correspondence

  CLASIF_PROB_ABAND_CAT_CTO      Probabilidad de   Text                              Contract.Churn
                                 abandono                                            Probability

  DES_PDT_PTLAR                  Producto          Text                              Contract.Plan
                                 particular                                          
                                 descripción                                         
                                 (Plan)                                              

  DES_PROVINCIA_PS               Provincia Punto   Text                              Contract.Province Supply
                                 Suministro                                          Point

  DES_PROVINCIA_CO               Provincia         Text                              Contract.Province
                                 Correspondencia                                     Correspondence

  COD_PS                         Punto de          Number                            Contract.Supply Point
                                 suministro                                          

  COD_RS                         Receptor de       Number                            Contract.Supply Receiver
                                 suministro                                          

  NUM_RECLA_ABIERTAS             Reclamaciones     Number                            Contract.Open Claims
                                 abiertas                                            

  DES_TIP_RESUL_CNMC             Resultado         Text                              Contract.Claim CNMC
                                 Reclamacion CNMC                                    Result

  segmento                       Segmento          Text                              Contract.Segment

  DES_TIP_SUB_RECLA              Subtipo de la     Text                              Contract.Last Claim
                                 última                                              Subtypes
                                 reclamación                                         

  tarifa_td                      Tarifa código     Text                              Contract.Tariff Id

  TIP_ALTA_CTO                   Tipo de alta      Number                            Contract.Start Type
                                 contrato                                            

  TIP_BAJA_CTO                   Tipo de baja      Text                              Contract.EndingType
                                 contrato                                            

  DES_TIP_CONTRATO               Tipo de contrato  Text                              Contract.Type Contract
                                 (normal,                                            
                                 eventual,\...)                                      

  DES_TIP_SECTOR_ENE             Tipo de sector    Text                              Contract.Type Energy
                                 energía                                             Sector

  DES_TIP_RECLA                  Tipo de última    Text                              Contract.Type Last Claim
                                 reclamación                                         

  TIPOLOGIA_VIVIENDA             Tipología de      Text                              Contract.Household Types
                                 viviendas                                           

  auditoria                      Última            Text                              Contract.Last Modified
                                 modificación                                        Date

  ZONA_CLIMA_LOCAL               Zona clima local  datetime                          Contract.Local Climate
                                                                                     Area

  ZONA_RATIO                     Zona ratio        Text                              Contract.Ratio Area
  -----------------------------------------------------------------------------------------------------------

### Servicio

  ----------------------------------------------------------------------------------------------------------------
  **Atributo de       **Descripción**   **Tipo**   **Cálculo**         **PK**   **Mapping**
  entrada**                                                                     
  ------------------- ----------------- ---------- ------------------- -------- ----------------------------------
  COD_CONTRATO        Código contrato   Text                                    Service.Contract Id

  COD_EFV             COD_EFV           Text                                    Service.Service Id

  DES_EFV             DES_EFV           Text                                    Service.Description

  COD_GRUPO_EFV       COD_GRUPO_EFV     Text                                    Service.Bundles

  DES_GRUPO_EFV       DES_GRUPO_EFV     Text                                    Service.Bundles Description

  fec_activacion      Fecha de          Date                                    Service.Activation Date
                      activación                                                

  fec_desactivacion   Fecha de          Date                                    Service.Deactivation Date
                      desactivación                                             

  TIP_EST_EFF         Estado servicio   Text                                    Service.Status

  auditoria           Última            datetime                                Service.Last Modified Date
                      modificación                                              

  Id                  Id único          Text                           X        [Service.Id](http://service.id/)
                                                                                (PK)
  ----------------------------------------------------------------------------------------------------------------

### Consentimientos

Consentimientos Terceros

  --------------------------------------------------------------------------------------------------------------------------------------------
  **Atributo de         **Descripción**       **Tipo**   **Cálculo**                                              **PK**   **Mapping**
  entrada**                                                                                                                
  --------------------- --------------------- ---------- -------------------------------------------------------- -------- -------------------
  COD_CLIENTE           Cod_cliente           Text(8)                                                                      Party Consent.Party

  COD_SOCIEDAD          cod_sociedad          Number                                                                       

  IND_CONSEN_TERCEROS   IND_CONSEN_TERCEROS   Text                                                                         

  auditoria             Última modificación   datetime                                                                     Party Consent.Last
                                                                                                                           Modified Date

  **Id**                **Id único**          **Text**                                                            **X**    **Party
                                                                                                                           Consent.Privacy
                                                                                                                           Consent ID (PK)**

  N/A                   Third Party Data      Text       IF(sourceField\[\'IND_CONSEN_TERCEROS\'\]==\'S\',\'Opt            Party
                        Consent Status                   In\',\'Opt Out\')                                                 Consent.Consent
                                                                                                                           Status

  N/A                   Party Consent Name    Text       \"Third Party Data Consent\"                                      Party Consent.Name

  N/A                   Effective To Date     Datetime   DATE(\'2999\',\'12\',\'31\')                                      Party
                                                                                                                           Consent.Effective
                                                                                                                           To Date
  --------------------------------------------------------------------------------------------------------------------------------------------

Consentimientos Perfilado

  --------------------------------------------------------------------------------------------------------------------------------------------
  **Atributo de         **Descripción**       **Tipo**   **Cálculo**                                              **PK**   **Mapping**
  entrada**                                                                                                                
  --------------------- --------------------- ---------- -------------------------------------------------------- -------- -------------------
  COD_CLIENTE           Cod_cliente           Text(8)                                                                      Party Consent.Party

  COD_SOCIEDAD          cod_sociedad          Number                                                                       

  IND_OPOSI_PERFILADO   IND_OPOSI_PERFILADO   Text                                                                         

  auditoria             Última modificación   datetime                                                                     Party Consent.Last
                                                                                                                           Modified Date

  **Id**                **Id único**          **Text**                                                            **X**    **Party
                                                                                                                           Consent.Privacy
                                                                                                                           Consent ID (PK)**

                        Profiling Consent     Text       IF(sourceField\[\'IND_OPOSI_PERFILADO\'\]==\'N\',\'Opt            Party
                        Status                           In\',\'Opt Out\')                                                 Consent.Consent
                                                                                                                           Status

                        Party Consent Name    Text       \"Profiling Consent\"                                             Party Consent.Name

                        Effective To Date     Datetime   DATE(\'2999\',\'12\',\'31\')                                      Party
                                                                                                                           Consent.Effective
                                                                                                                           To Date
  --------------------------------------------------------------------------------------------------------------------------------------------

Consentimientos Comunicaciones

  -----------------------------------------------------------------------------------------------------------------------------------------------------
  **Atributo de entrada**  **Descripción**          **Tipo**   **Cálculo**                                                 **PK**   **Mapping**
  ------------------------ ------------------------ ---------- ----------------------------------------------------------- -------- -------------------
  COD_CLIENTE              Cod_cliente              Text(8)                                                                         Party Consent.Party

  COD_SOCIEDAD             cod_sociedad             Number                                                                          

  IND_OPOSI_COMUNICACION   IND_OPOSI_COMUNICACION   Text                                                                            

  auditoria                Última modificación      datetime                                                                        Party Consent.Last
                                                                                                                                    Modified Date

  **Id**                   **Id único**             **Text**                                                               **X**    **Party
                                                                                                                                    Consent.Privacy
                                                                                                                                    Consent ID (PK)**

                           Communications Consent   Text       IF(sourceField\[\'IND_OPOSI_COMUNICACION\'\]==\'N\',\'Opt            Party
                           Status                              In\',\'Opt Out\')                                                    Consent.Consent
                                                                                                                                    Status

                           Party Consent Name       Text       \"Communications Consent\"                                           Party Consent.Name

                           Effective To Date        Datetime   DATE(\'2999\',\'12\',\'31\')                                         Party
                                                                                                                                    Consent.Effective
                                                                                                                                    To Date
  -----------------------------------------------------------------------------------------------------------------------------------------------------

Consentimientos Winback

  --------------------------------------------------------------------------------------------------------------------------------------------------------
  **Atributo de entrada**   **Descripción**           **Tipo**   **Cálculo**                                                  **PK**   **Mapping**
  ------------------------- ------------------------- ---------- ------------------------------------------------------------ -------- -------------------
  COD_CLIENTE               Cod_cliente               Text(8)                                                                          Party Consent.Party

  COD_SOCIEDAD              cod_sociedad              Number                                                                           

  IND_CONSEN_COMUNICACION   IND_CONSEN_COMUNICACION   Text                                                                             

  auditoria                 Última modificación       datetime                                                                         Party Consent.Last
                                                                                                                                       Modified Date

  **Id**                    **Id único**              **Text**                                                                **X**    **Party
                                                                                                                                       Consent.Privacy
                                                                                                                                       Consent ID (PK)**

  N/A                       Winback Consent Status    Text       IF(sourceField\[\'IND_CONSEN_COMUNICACION\'\]==\'S\',\'Opt            Party
                                                                 In\',\'Opt Out\')                                                     Consent.Consent
                                                                                                                                       Status

  N/A                       Party Consent Name        Text       \"Winback Consent\"                                                   Party Consent.Name

  N/A                       Effective To Date         Datetime   DATE(\'2999\',\'12\',\'31\')                                          Party
                                                                                                                                       Consent.Effective
                                                                                                                                       To Date
  --------------------------------------------------------------------------------------------------------------------------------------------------------

Phone

+--------+-----------+-----+---------------+---+-----------------------+
| **     | **Desc    | **  | **Cálculo**   | * | **Mapping**           |
| Nombre | ripción** | Tip |               | * |                       |
| atri   |           | o** |               | P |                       |
| buto** |           |     |               | K |                       |
|        |           |     |               | * |                       |
|        |           |     |               | * |                       |
+========+===========+=====+===============+===+=======================+
| ID     | Id único  | T   |               | X | Contact Point         |
|        |           | ext |               |   | Phone.Contact Point   |
|        |           |     |               |   | Phone Id (PK)         |
|        |           |     |               |   |                       |
|        |           |     |               |   | Contact Point         |
|        |           |     |               |   | Consent.Contact Point |
|        |           |     |               |   |                       |
|        |           |     |               |   | Contact Point         |
|        |           |     |               |   | Consent.Contact Point |
|        |           |     |               |   | Consent Id (PK)       |
+--------+-----------+-----+---------------+---+-----------------------+
| COD_C  | Co        | T   |               |   | Contact Point         |
| LIENTE | d_cliente | ext |               |   | Phone.Party           |
|        |           | (8) |               |   |                       |
|        |           |     |               |   | Contact Point         |
|        |           |     |               |   | Consent.Party         |
+--------+-----------+-----+---------------+---+-----------------------+
| COD_SO | cod       | Num |               |   |                       |
| CIEDAD | _sociedad | ber |               |   |                       |
+--------+-----------+-----+---------------+---+-----------------------+
| IN     | IND_O     | T   |               |   |                       |
| D_OPOS | POSI_TELF | ext |               |   |                       |
| I_telf |           |     |               |   |                       |
+--------+-----------+-----+---------------+---+-----------------------+
| TE     | telefono  | T   |               |   | Contact Point         |
| LEFONO |           | ext |               |   | Phone.Telephone       |
| _CORTO |           |     |               |   | Number                |
+--------+-----------+-----+---------------+---+-----------------------+
| TE     | Format    | T   |               |   | Contact Point         |
| LEFONO | tedE164Ph | ext |               |   | Phone.Formatted E164  |
| _LARGO | oneNumber |     |               |   | Phone Number          |
+--------+-----------+-----+---------------+---+-----------------------+
| pref   | PhoneCo   | T   |               |   | Contact Point         |
| ijo_TE | untryCode | ext |               |   | Phone.Phone Country   |
| LEFONO |           |     |               |   | Code                  |
+--------+-----------+-----+---------------+---+-----------------------+
| p      | Country   | T   |               |   | Contact Point         |
| ais_TE | Phone     | ext |               |   | Phone.Country         |
| LEFONO |           |     |               |   |                       |
+--------+-----------+-----+---------------+---+-----------------------+
| FE     | Fecha     | da  |               |   | Contact Point         |
| CHA_CO | Auditoria | tet |               |   | Consent.Effective     |
| NSULTA | Adigital  | ime |               |   | From Date             |
+--------+-----------+-----+---------------+---+-----------------------+
| PROX   |           | da  |               |   |                       |
| IMA_CO |           | tet |               |   |                       |
| NSULTA |           | ime |               |   |                       |
+--------+-----------+-----+---------------+---+-----------------------+
| AUD    | Última    | da  |               |   | Contact Point         |
| ITORIA | mod       | tet |               |   | Consent.Consent       |
|        | ificación | ime |               |   | Captured Date Time    |
|        |           |     |               |   |                       |
|        |           |     |               |   | Contact Point         |
|        |           |     |               |   | Phone.Last Modified   |
|        |           |     |               |   | Date                  |
+--------+-----------+-----+---------------+---+-----------------------+
| N/A    | Consent   | T   | IF(sourceF    |   | Contact Point         |
|        | Phone     | ext | ield\[\'IND_O |   | Consent.Consent       |
|        | Status    |     | POSI_telf\'\] |   | Status                |
|        |           |     | ==\'N\',\'Opt |   |                       |
|        |           |     | In\',\'Opt    |   |                       |
|        |           |     | Out\')        |   |                       |
+--------+-----------+-----+---------------+---+-----------------------+
| N/A    | Contact   | T   | \"Contact     |   | Contact Point         |
|        | Point     | ext | Point Phone   |   | Consent.Name          |
|        | Consent   |     | Consent\"     |   |                       |
|        | Name      |     |               |   |                       |
+--------+-----------+-----+---------------+---+-----------------------+
| N/A    | Effective | Da  | DA            |   | Contact Point         |
|        | To Date   | tet | TE(\'2999\',\ |   | Consent.Effective To  |
|        |           | ime | '12\',\'31\') |   | Date                  |
+--------+-----------+-----+---------------+---+-----------------------+

Email

+---------+---------+----+------------------------+---+-----------------+
| *       | *       | ** | **Cálculo**            | * | **Mapping**     |
| *Nombre | *Descri | Ti |                        | * |                 |
| atr     | pción** | po |                        | P |                 |
| ibuto** |         | ** |                        | K |                 |
|         |         |    |                        | * |                 |
|         |         |    |                        | * |                 |
+=========+=========+====+========================+===+=================+
| ID      | Id      | Te |                        | X | Contact Point   |
|         | único   | xt |                        |   | Email.Contact   |
|         |         |    |                        |   | Point Email Id  |
|         |         |    |                        |   | (PK)            |
|         |         |    |                        |   |                 |
|         |         |    |                        |   | Contact Point   |
|         |         |    |                        |   | Consent.Contact |
|         |         |    |                        |   | Point           |
|         |         |    |                        |   |                 |
|         |         |    |                        |   | Contact Point   |
|         |         |    |                        |   | Consent.Contact |
|         |         |    |                        |   | Point Consent   |
|         |         |    |                        |   | Id (PK)         |
+---------+---------+----+------------------------+---+-----------------+
| COD_    | Cod_    | T  |                        |   | Contact Point   |
| CLIENTE | cliente | ex |                        |   | Email.Party     |
|         |         | t( |                        |   |                 |
|         |         | 8) |                        |   | Contact Point   |
|         |         |    |                        |   | Consent.Party   |
+---------+---------+----+------------------------+---+-----------------+
| COD_S   | cod_s   | Nu |                        |   |                 |
| OCIEDAD | ociedad | mb |                        |   |                 |
|         |         | er |                        |   |                 |
+---------+---------+----+------------------------+---+-----------------+
| IND_ROB | IND_ROB | Te |                        |   |                 |
| INSON_A | INSON_A | xt |                        |   |                 |
| DIGITAL | DIGITAL |    |                        |   |                 |
+---------+---------+----+------------------------+---+-----------------+
| EMAIL   | email   | Te |                        |   | Contact Point   |
|         |         | xt |                        |   | Email.Email     |
|         |         |    |                        |   | Address         |
+---------+---------+----+------------------------+---+-----------------+
| FECHA_C | Fecha   | da |                        |   | Contact Point   |
| ONSULTA | Au      | te |                        |   | Co              |
|         | ditoria | ti |                        |   | nsent.Effective |
|         | A       | me |                        |   | From Date       |
|         | digital |    |                        |   |                 |
+---------+---------+----+------------------------+---+-----------------+
| PR      |         |    |                        |   |                 |
| OXIMA_C |         |    |                        |   |                 |
| ONSULTA |         |    |                        |   |                 |
+---------+---------+----+------------------------+---+-----------------+
| AU      | Última  | da |                        |   | Contact Point   |
| DITORIA | modif   | te |                        |   | Consent.Consent |
|         | icación | ti |                        |   | Captured Date   |
|         |         | me |                        |   | Time            |
|         |         |    |                        |   |                 |
|         |         |    |                        |   | Contact Point   |
|         |         |    |                        |   | Email.Last      |
|         |         |    |                        |   | Modified Date   |
+---------+---------+----+------------------------+---+-----------------+
| N/A     | Email   | Te | SELECT(sourceField\[\' |   | Contact Point   |
|         | Domain  | xt | email\'\],\'@(\[a-zA-Z |   | Email.Email     |
|         |         |    | 0-9-\]+.)\*\[a-zA-Z0-9 |   | Domain          |
|         |         |    | -\]+.\[a-zA-Z\]{2,}\') |   |                 |
+---------+---------+----+------------------------+---+-----------------+
| N/A     | Contact | Te | \"Contact Point Email  |   | Contact Point   |
|         | Point   | xt | Consent\"              |   | Consent.Name    |
|         | Consent |    |                        |   |                 |
|         | Name    |    |                        |   |                 |
+---------+---------+----+------------------------+---+-----------------+
| N/A     | Consent | Te | IF(sourceFie           |   | Contact Point   |
|         | Email   | xt | ld\[\'IND_ROBINSON_ADI |   | Consent.Consent |
|         | Status  |    | GITAL\'\]==\'N\',\'Opt |   | Status          |
|         |         |    | In\',\'Opt Out\')      |   |                 |
+---------+---------+----+------------------------+---+-----------------+
| N/A     | Ef      | Da | DATE(\                 |   | Contact Point   |
|         | fective | te | '2999\',\'12\',\'31\') |   | Co              |
|         | To Date | ti |                        |   | nsent.Effective |
|         |         | me |                        |   | To Date         |
+---------+---------+----+------------------------+---+-----------------+

Consentimientos Robinson

  --------------------------------------------------------------------------------------------------------------------------------------------
  **Nombre atributo**     **Descripción**   **Tipo**   **Cálculo**                                                **PK**   **Mapping**
  ----------------------- ----------------- ---------- ---------------------------------------------------------- -------- -------------------
  COD_CLIENTE             Código de cliente Text                                                                           Party Consent.Party

  COD_SOCIEDAD            Código de         Text                                                                           
                          sociedad                                                                                         

  DNI                     DNI               Text                                                                           

  IND_ROBINSON_ADIGITAL   Indicador de      Text                                                                           
                          oposición a las                                                                                  
                          comunicaciones                                                                                   
                          email / teléfono                                                                                 

  FECHA_CONSULTA          Fecha Auditoria   datetime                                                                       
                          Adigital                                                                                         

  PROXIMA_CONSULTA                          datetime                                                                       

  auditoria               Última            datetime                                                                       Party Consent.Last
                          modificación                                                                                     Modified Date

  ID                      Id único          Text                                                                  x        Party Consent.
                                                                                                                           Privacy Consent Id
                                                                                                                           (PK)

  N/A                     Robinson Consent  Text       IF(sourceField\[\'IND_ROBINSON_ADIGITAL\'\]==\'N\',\'Opt            Party
                          Status                       In\',\'Opt Out\')                                                   Consent.Consent
                                                                                                                           Status

  N/A                     Party Consent     Text       \"Robinson Consent\"                                                Party Consent.Name
                          Name                                                                                             

  N/A                     Effective To Date Datetime   DATE(\'2999\',\'12\',\'31\')                                        Party
                                                                                                                           Consent.Effective
                                                                                                                           To Date
  --------------------------------------------------------------------------------------------------------------------------------------------

### CATs

+----------+----------+---+-----------------------+---+-----------------+
| **       | **Descr  | * | **Cálculo**           | * | **Mapping**     |
| Atributo | ipción** | * |                       | * |                 |
| de       |          | T |                       | P |                 |
| e        |          | i |                       | K |                 |
| ntrada** |          | p |                       | * |                 |
|          |          | o |                       | * |                 |
|          |          | * |                       |   |                 |
|          |          | * |                       |   |                 |
+==========+==========+===+=======================+===+=================+
| URL_     |          | T |                       |   | Digital Asset   |
| FRONTEND |          | e |                       |   | Engagement.Page |
|          |          | x |                       |   | URL             |
|          |          | t |                       |   |                 |
+----------+----------+---+-----------------------+---+-----------------+
| FECHA_NA |          | D |                       |   | Digital Asset   |
| VEGACIÓN |          | a |                       |   | Engage          |
|          |          | t |                       |   | ment.Engagement |
|          |          | e |                       |   | Date Time       |
|          |          | t |                       |   |                 |
|          |          | i |                       |   |                 |
|          |          | m |                       |   |                 |
|          |          | e |                       |   |                 |
+----------+----------+---+-----------------------+---+-----------------+
| N        |          | T |                       |   | Digital Asset   |
| AVEGADOR |          | e |                       |   | Eng             |
|          |          | x |                       |   | agement.Browser |
|          |          | t |                       |   | Name            |
+----------+----------+---+-----------------------+---+-----------------+
| DIS      |          | T |                       |   | Digital Asset   |
| POSITIVO |          | e |                       |   | Engage          |
|          |          | x |                       |   | ment.Engagement |
|          |          | t |                       |   | Channel Type    |
+----------+----------+---+-----------------------+---+-----------------+
| IP       |          | T |                       |   | Digital Asset   |
|          |          | e |                       |   | Engagement.IP   |
|          |          | x |                       |   | Address         |
|          |          | t |                       |   |                 |
+----------+----------+---+-----------------------+---+-----------------+
| CO       |          | T |                       |   | Digital Asset   |
| D_SESION |          | e |                       |   | Engagement.Web  |
|          |          | x |                       |   | Session ID      |
|          |          | t |                       |   |                 |
+----------+----------+---+-----------------------+---+-----------------+
| TRACKIN  |          | T |                       |   | Digital Asset   |
| G_COOKIE |          | e |                       |   | Engagement.Web  |
|          |          | x |                       |   | Cookie          |
|          |          | t |                       |   |                 |
|          |          |   |                       |   | Party           |
|          |          |   |                       |   | Identification  |
|          |          |   |                       |   | .Identification |
|          |          |   |                       |   | Number          |
+----------+----------+---+-----------------------+---+-----------------+
| Party    |          | T | \"Cookie Identifier\" |   | Party           |
| Identi   |          | e |                       |   | Ident           |
| fication |          | x |                       |   | ification.Party |
| Type     |          | t |                       |   | Identification  |
|          |          |   |                       |   | Type            |
+----------+----------+---+-----------------------+---+-----------------+
| Identi   |          | T | \"Cookie Cats\"       |   | Party           |
| fication |          | e |                       |   | Identification  |
| Name     |          | x |                       |   | .Identification |
|          |          | t |                       |   | Name            |
+----------+----------+---+-----------------------+---+-----------------+
| Party    |          | T | CONCAT(sou            |   | Party           |
| Identi   |          | e | rceField\[\'COD_CLIEN |   | Ident           |
| fication |          | x | TE\'\],sourceField\[\ |   | ification.Party |
| Id       |          | t | 'TRACKING_COOKIE\'\]) |   | Identification  |
|          |          |   |                       |   | Id (PK)         |
+----------+----------+---+-----------------------+---+-----------------+
| APP      |          | T |                       |   | Digital Asset   |
| LICATION |          | e |                       |   | Engagem         |
|          |          | x |                       |   | ent.Application |
|          |          | t |                       |   |                 |
+----------+----------+---+-----------------------+---+-----------------+
| COD      |          | T |                       |   | Digital Asset   |
| _USUARIO |          | e |                       |   | Engagement.User |
|          |          | x |                       |   | Code            |
|          |          | t |                       |   |                 |
+----------+----------+---+-----------------------+---+-----------------+
| COD      |          | T |                       |   |                 |
| _CLIENTE |          | e |                       |   |                 |
|          |          | x |                       |   |                 |
|          |          | t |                       |   |                 |
+----------+----------+---+-----------------------+---+-----------------+
| COD_     |          | T |                       |   | Digital Asset   |
| CONTRATO |          | e |                       |   | Enga            |
|          |          | x |                       |   | gement.Contract |
|          |          | t |                       |   | Id              |
+----------+----------+---+-----------------------+---+-----------------+
| act      |          | T |                       |   | Digital Asset   |
| ion_type |          | e |                       |   | Engage          |
|          |          | x |                       |   | ment.Engagement |
|          |          | t |                       |   | Channel Action  |
+----------+----------+---+-----------------------+---+-----------------+
| descPa   |          | T |                       |   | Digital Asset   |
| queteWeb |          | e |                       |   | Engagement      |
|          |          | x |                       |   | .descPaqueteWeb |
|          |          | t |                       |   |                 |
+----------+----------+---+-----------------------+---+-----------------+
| codPa    |          | T |                       |   | Digital Asset   |
| queteWeb |          | e |                       |   | Engagemen       |
|          |          | x |                       |   | t.codPaqueteWeb |
|          |          | t |                       |   |                 |
+----------+----------+---+-----------------------+---+-----------------+
| codO     |          | T |                       |   | Digital Asset   |
| fertaWeb |          | e |                       |   | Engageme        |
|          |          | x |                       |   | nt.codOfertaWeb |
|          |          | t |                       |   |                 |
+----------+----------+---+-----------------------+---+-----------------+
| codOfe   |          | T |                       |   | Digital Asset   |
| rtaDelta |          | e |                       |   | Engagement      |
|          |          | x |                       |   | .codOfertaDelta |
|          |          | t |                       |   |                 |
+----------+----------+---+-----------------------+---+-----------------+
| resp     |          | T |                       |   | Digital Asset   |
| onseCode |          | e |                       |   | Enga            |
|          |          | x |                       |   | gement.Response |
|          |          | t |                       |   | Code            |
+----------+----------+---+-----------------------+---+-----------------+
| **cod    |          | * |                       | * | **Digital Asset |
| _event** |          | * |                       | * | Eng             |
|          |          | T |                       | X | agement.Website |
|          |          | e |                       | * | Engagement Id   |
|          |          | x |                       | * | (PK)**          |
|          |          | t |                       |   |                 |
|          |          | * |                       |   |                 |
|          |          | * |                       |   |                 |
+----------+----------+---+-----------------------+---+-----------------+
| email    |          | T |                       |   | Digital Asset   |
|          |          | e |                       |   | E               |
|          |          | x |                       |   | ngagement.Email |
|          |          | t |                       |   |                 |
|          |          |   |                       |   | Contact Point   |
|          |          |   |                       |   | Email.Email     |
|          |          |   |                       |   | Address         |
+----------+----------+---+-----------------------+---+-----------------+
| Email    |          | T | SELE                  |   | Contact Point   |
| Domain   |          | e | CT(sourceField\[\'ema |   | Email.Email     |
|          |          | x | il\'\],\'@(\[a-zA-Z0- |   | Domain          |
|          |          | t | 9-\]+.)\*\[a-zA-Z0-9- |   |                 |
|          |          |   | \]+.\[a-zA-Z\]{2,}\') |   |                 |
+----------+----------+---+-----------------------+---+-----------------+
| Contact  |          | T | sourceField           |   | Contact Point   |
| Point    |          | e | \[\'COD_CLIENTE\'\] + |   | Email.Contact   |
| Email Id |          | x | sou                   |   | Point Email Id  |
|          |          | t | rceField\[\'email\'\] |   | (PK)            |
+----------+----------+---+-----------------------+---+-----------------+
| telefono |          | T |                       |   | Digital Asset   |
|          |          | e |                       |   | Engag           |
|          |          | x |                       |   | ement.Telephone |
|          |          | t |                       |   |                 |
|          |          |   |                       |   | Contact Point   |
|          |          |   |                       |   | Phone.Telephone |
|          |          |   |                       |   | Number          |
+----------+----------+---+-----------------------+---+-----------------+
| F        |          | T | 34\' +                |   | Contact Point   |
| ormatted |          | e | source                |   | Phone.Formatted |
| E164     |          | x | Field\[\'telefono\'\] |   | E164 Phone      |
| Phone    |          | t |                       |   | Number          |
| Number   |          |   |                       |   |                 |
+----------+----------+---+-----------------------+---+-----------------+
| Contact  |          | T | sourceField           |   | Contact Point   |
| Point    |          | e | \[\'COD_CLIENTE\'\] + |   | Phone.Contact   |
| Phone Id |          | x | source                |   | Point Phone Id  |
|          |          | t | Field\[\'telefono\'\] |   | (PK)            |
+----------+----------+---+-----------------------+---+-----------------+
| numD     |          | T |                       |   | Digital Asset   |
| ocumento |          | e |                       |   | Engagement      |
|          |          | x |                       |   | .Identification |
|          |          | t |                       |   | Number          |
+----------+----------+---+-----------------------+---+-----------------+
| Is       |          | T | I                     |   | Individual.Is   |
| A        |          | e | F(ISEMPTY(sourceField |   | Anonymous       |
| nonymous |          | x | \[\'COD_CLIENTE\'\]), |   |                 |
|          |          | t | \"1\", \"0\")         |   |                 |
+----------+----------+---+-----------------------+---+-----------------+
| Party    |          | T | I                     |   | Indivi          |
|          |          | e | F(ISEMPTY(sourceField |   | dual.Individual |
|          |          | x | \[\'COD_CLIENTE\'\]), |   | Id (PK)         |
|          |          | t | sou                   |   |                 |
|          |          |   | rceField\[\'TRACKING_ |   | Contact Point   |
|          |          |   | COOKIE\'\],sourceFiel |   | Phone.Party     |
|          |          |   | d\[\'COD_CLIENTE\'\]) |   |                 |
|          |          |   |                       |   | Contact Point   |
|          |          |   |                       |   | Email.Party     |
|          |          |   |                       |   |                 |
|          |          |   |                       |   | Party           |
|          |          |   |                       |   | Ident           |
|          |          |   |                       |   | ification.Party |
|          |          |   |                       |   |                 |
|          |          |   |                       |   | Digital Asset   |
|          |          |   |                       |   | E               |
|          |          |   |                       |   | ngagement.Party |
+----------+----------+---+-----------------------+---+-----------------+

### Delio

+----------+--------------+----+-----------------+---+----------------+
| **       | **D          | ** | **Cálculo**     | * | **Mapping**    |
| Atributo | escripción** | Ti |                 | * |                |
| de       |              | po |                 | P |                |
| e        |              | ** |                 | K |                |
| ntrada** |              |    |                 | * |                |
|          |              |    |                 | * |                |
+==========+==============+====+=================+===+================+
| a        | Última       | da |                 |   | Lead. SAS      |
| uditoria | modificación | te |                 |   | Extraction     |
|          |              | ti |                 |   | Date           |
|          |              | me |                 |   |                |
+----------+--------------+----+-----------------+---+----------------+
| LEADID   | Id del lead  | Te |                 | X | Lead.Lead Id   |
|          |              | xt |                 |   | (PK)           |
+----------+--------------+----+-----------------+---+----------------+
| TELEFO   | Teléfono     | Te |                 |   | Contact Point  |
| NO_CORTO | corto        | xt |                 |   | P              |
|          |              |    |                 |   | hone.Telephone |
|          |              |    |                 |   | Number         |
+----------+--------------+----+-----------------+---+----------------+
| TELEFO   | Teléfono     | Te |                 |   | Contact Point  |
| NO_LARGO | largo        | xt |                 |   | P              |
|          |              |    |                 |   | hone.Formatted |
|          |              |    |                 |   | E164 Phone     |
|          |              |    |                 |   | Number         |
+----------+--------------+----+-----------------+---+----------------+
| TEL      | Telefono     | Te |                 |   | Lead.Telephone |
| EF_LLAMA | llamado      | xt |                 |   | Number Call    |
| DO_CORTO | corto        |    |                 |   | Center         |
+----------+--------------+----+-----------------+---+----------------+
| TEL      | Telefono     | Te |                 |   | Lead.Formatted |
| EF_LLAMA | llamado      | xt |                 |   | E164 Phone     |
| DO_LARGO | largo        |    |                 |   | Number Call    |
|          |              |    |                 |   | Center         |
+----------+--------------+----+-----------------+---+----------------+
| PAIS_TEL | País         | Te |                 |   | Lead.Country   |
| _LLAMADO | teléfono     | xt |                 |   |                |
|          | llamado      |    |                 |   |                |
+----------+--------------+----+-----------------+---+----------------+
| pais_    | País         | Te |                 |   | Contact Point  |
| telefono | teléfono     | xt |                 |   | Phone.Country  |
+----------+--------------+----+-----------------+---+----------------+
| PREFI    | Prefijo      | Te |                 |   | Lead.Phone     |
| JO_TELEF | teléfono     | xt |                 |   | Country Code   |
| _LLAMADO | llamado      |    |                 |   |                |
+----------+--------------+----+-----------------+---+----------------+
| prefijo_ | Prefijo      | Te |                 |   | Contact Point  |
| telefono | teléfono     | xt |                 |   | Phone.Phone    |
|          |              |    |                 |   | Country Code   |
+----------+--------------+----+-----------------+---+----------------+
| ULT_CODI | Última       | Te |                 |   | Lead.UL        |
| FICACION | codificación | xt |                 |   | T_CODIFICACION |
|          | para el      |    |                 |   |                |
|          | estado       |    |                 |   |                |
+----------+--------------+----+-----------------+---+----------------+
| ESTADO   | Estado       | Te |                 |   | Lead.Lead      |
|          |              | xt |                 |   | Status         |
+----------+--------------+----+-----------------+---+----------------+
| UTIL     | Utilidad de  | Te |                 |   | Lead.Utility   |
| IDAD_DE_ | registro     | xt |                 |   | Register       |
| REGISTRO | para el      |    |                 |   |                |
|          | estado       |    |                 |   |                |
+----------+--------------+----+-----------------+---+----------------+
| CUAL     | C            | Te |                 |   | Lead.Funnel    |
| IFICACIO | ualification | xt |                 |   | Qualification  |
| N_FUNNEL | Funnel       |    |                 |   |                |
+----------+--------------+----+-----------------+---+----------------+
| ISCLIENT | Es cliente   | Te |                 |   |                |
|          |              | xt |                 |   |                |
+----------+--------------+----+-----------------+---+----------------+
| N/A      | Is Anonymous | Te | IF(sourceField\ |   | Individual.Is  |
|          |              | xt | [\'ISCLIENT\'\] |   | Anonymous      |
|          |              |    | == \'No\',      |   |                |
|          |              |    | \'1\', \'0\')   |   |                |
+----------+--------------+----+-----------------+---+----------------+
| COD      | Código de    | Te |                 |   |                |
| _CLIENTE | cliente      | xt |                 |   |                |
+----------+--------------+----+-----------------+---+----------------+
| N/A      | Party        | Te | IF              |   | Individ        |
|          |              | xt | (sourceField\[\ |   | ual.Individual |
|          |              |    | 'ISCLIENT\'\]== |   | Id             |
|          |              |    | \'Si            |   |                |
|          |              |    | \',sourceField\ |   | Contact Point  |
|          |              |    | [\'COD_CLIENTE\ |   | Phone.Party    |
|          |              |    | '\],sourceField |   |                |
|          |              |    | \[\'LEADID\'\]) |   | Lead.Party     |
|          |              |    |                 |   |                |
|          |              |    |                 |   | Party          |
|          |              |    |                 |   | Identi         |
|          |              |    |                 |   | fication.Party |
+----------+--------------+----+-----------------+---+----------------+
| N/A      | Contact      | Te | IF              |   | Contact Point  |
|          | Point Phone  | xt | (sourceField\[\ |   | Phone.Contact  |
|          | Id           |    | 'ISCLIENT\'\]== |   | Point Id (PK)  |
|          |              |    | \'Si\'          |   |                |
|          |              |    | ,sourceField\[\ |   |                |
|          |              |    | 'COD_CLIENTE\'\ |   |                |
|          |              |    | ],sourceField\[ |   |                |
|          |              |    | \'LEADID\'\]) + |   |                |
|          |              |    | sou             |   |                |
|          |              |    | rceField\[\'TEL |   |                |
|          |              |    | EFONO_CORTO\'\] |   |                |
+----------+--------------+----+-----------------+---+----------------+
| SALESFO  | sal          | Te |                 |   | Lead.Cats      |
| RCE_CATS | esforce_cats | xt |                 |   | Cookie         |
|          |              |    |                 |   |                |
|          |              |    |                 |   | Party          |
|          |              |    |                 |   | I              |
|          |              |    |                 |   | dentification. |
|          |              |    |                 |   | Identification |
|          |              |    |                 |   | Number         |
+----------+--------------+----+-----------------+---+----------------+
| SAL      | salesfo      | Te |                 |   | Lead.Session   |
| ESFORCE_ | rce_catstemp | xt |                 |   | Id             |
| CATSTEMP |              |    |                 |   |                |
+----------+--------------+----+-----------------+---+----------------+
| N/A      | Party        | Te | Cookie          |   | Party          |
|          | Id           | xt | Identifier      |   | Identi         |
|          | entification |    |                 |   | fication.Party |
|          | Type         |    |                 |   | Identification |
|          |              |    |                 |   | Type           |
+----------+--------------+----+-----------------+---+----------------+
| N/A      | Id           | Te | Cookie cats     |   | Party          |
|          | entification | xt |                 |   | I              |
|          | Name         |    |                 |   | dentification. |
|          |              |    |                 |   | Identification |
|          |              |    |                 |   | Name           |
+----------+--------------+----+-----------------+---+----------------+
| N/A      | Party        | Te | IF              |   | Party          |
|          | Id           | xt | (sourceField\[\ |   | Identi         |
|          | entification |    | 'ISCLIENT\'\]== |   | fication.Party |
|          | Id           |    | \'Si\'          |   | Identification |
|          |              |    | ,sourceField\[\ |   | Id (PK)        |
|          |              |    | 'COD_CLIENTE\'\ |   |                |
|          |              |    | ],sourceField\[ |   |                |
|          |              |    | \'LEADID\'\]) + |   |                |
|          |              |    | sour            |   |                |
|          |              |    | ceField\[\'SALE |   |                |
|          |              |    | SFORCE_CATS\'\] |   |                |
+----------+--------------+----+-----------------+---+----------------+
| salesfo  | A futuro se  | Te |                 |   | L              |
| rce_evga | mapeará      | xt |                 |   | ead.Salesforce |
|          |              |    |                 |   | IS Cookie      |
|          |              |    |                 |   |                |
|          |              |    |                 |   | Party          |
|          |              |    |                 |   | I              |
|          |              |    |                 |   | dentification. |
|          |              |    |                 |   | Identification |
|          |              |    |                 |   | Number         |
+----------+--------------+----+-----------------+---+----------------+
| N/A      | Party        | Te | Cookie          |   | Party          |
|          | Id           | xt | Identifier      |   | Identi         |
|          | entification |    |                 |   | fication.Party |
|          | Type (A      |    |                 |   | Identification |
|          | futuro se    |    |                 |   | Type           |
|          | mapeará)     |    |                 |   |                |
+----------+--------------+----+-----------------+---+----------------+
| N/A      | Id           | Te | Cookie IS       |   | Party          |
|          | entification | xt |                 |   | I              |
|          | Name (A      |    |                 |   | dentification. |
|          | futuro se    |    |                 |   | Identification |
|          | mapeará)     |    |                 |   | Name           |
+----------+--------------+----+-----------------+---+----------------+
| N/A      | Party        | Te | IF              |   | Party          |
|          | Id           | xt | (sourceField\[\ |   | Identi         |
|          | entification |    | 'isClient\'\]== |   | fication.Party |
|          | Id (A futuro |    | \'Si\'          |   | Identification |
|          | se mapeará)  |    | ,sourceField\[\ |   | Id (PK)        |
|          |              |    | 'cod_cliente\'\ |   |                |
|          |              |    | ],sourceField\[ |   |                |
|          |              |    | \'LeadId\'\]) + |   |                |
|          |              |    | sour            |   |                |
|          |              |    | ceField\[\'sale |   |                |
|          |              |    | sforce_evga\'\] |   |                |
+----------+--------------+----+-----------------+---+----------------+
| ORI      | Origen Lead  | Te |                 |   | Lead.Lead      |
| GEN_LEAD |              | xt |                 |   | Source         |
+----------+--------------+----+-----------------+---+----------------+
| UR       | url_source   | Te |                 |   | Lead.Website   |
| L_SOURCE |              | xt |                 |   |                |
+----------+--------------+----+-----------------+---+----------------+
| PROD     | PRODUCTO_URL | Te |                 |   | Lead.Product   |
| UCTO_URL |              | xt |                 |   | URL            |
+----------+--------------+----+-----------------+---+----------------+
| CA       | Cátegoria    | Te |                 |   | Lead.Product   |
| TEGORIA_ | del producto | xt |                 |   | Category       |
| PRODUCTO |              |    |                 |   |                |
+----------+--------------+----+-----------------+---+----------------+
| FECHA    | Fecha de     | Da |                 |   | Lead.Created   |
| _DE_ALTA | alta         | te |                 |   | Date           |
|          |              | ti |                 |   |                |
|          |              | me |                 |   |                |
+----------+--------------+----+-----------------+---+----------------+
| FECHA_   | Fecha de     | Da |                 |   | Lead.Last      |
| DE_ACTUA | a            | te |                 |   | Modified Date  |
| LIZACION | ctualización | ti |                 |   |                |
|          |              | me |                 |   |                |
+----------+--------------+----+-----------------+---+----------------+
| GRH_     | Campaña      | Te |                 |   | Lead.Campaign  |
| CAMPAIGN |              | xt |                 |   |                |
+----------+--------------+----+-----------------+---+----------------+
| CONT_CAM | Contador de  | Nu |                 |   | Lead.CO        |
| BIO_PLAN | cambio de    | mb |                 |   | NT_CAMBIO_PLAN |
|          | plan         | er |                 |   |                |
+----------+--------------+----+-----------------+---+----------------+
| CONT_SMA | Contador     | Nu |                 |   | Lead.CO        |
| RT_SOLAR | smart solar  | mb |                 |   | NT_SMART_SOLAR |
|          |              | er |                 |   |                |
+----------+--------------+----+-----------------+---+----------------+
| CON      | Contador     | Nu |                 |   | Lead.CONT_     |
| T_SMART_ | smart        | mb |                 |   | SMART_MOBILITY |
| MOBILITY | mobility     | er |                 |   |                |
+----------+--------------+----+-----------------+---+----------------+
| CONT_SMA | Contador     | Nu |                 |   | Lead.CO        |
| RT_CLIMA | smart clima  | mb |                 |   | NT_SMART_CLIMA |
|          |              | er |                 |   |                |
+----------+--------------+----+-----------------+---+----------------+
| NUM_     | Número de    | Nu |                 |   | Lead.NUM_PR    |
| PRODUCT_ | productos    | mb |                 |   | ODUCT_CONTRATA |
| CONTRATA | contratados  | er |                 |   |                |
+----------+--------------+----+-----------------+---+----------------+
| NUM      | Número de    | Nu |                 |   | Lead.NUM_P     |
| _PRODUCT | productos    | mb |                 |   | RODUCTOS_SMART |
| OS_SMART | smart        | er |                 |   |                |
+----------+--------------+----+-----------------+---+----------------+
| N        | Número de    | Nu |                 |   | Le             |
| UM_PRODU | productos y  | mb |                 |   | ad.NUM_PRODUCT |
| CTOS_Y_S | servicios    | er |                 |   | OS_Y_SERVICIOS |
| ERVICIOS |              |    |                 |   |                |
+----------+--------------+----+-----------------+---+----------------+
| NUM_PR   | Número de    | Nu |                 |   | Lead.NUM_PROD  |
| ODUCT_EN | productos    | mb |                 |   | UCT_ENERGETICO |
| ERGETICO | energéticos  | er |                 |   |                |
+----------+--------------+----+-----------------+---+----------------+
| CODIGO_  | Código de    | Te |                 |   | Lead.C         |
| OFERTA_1 | oferta 1     | xt |                 |   | ODIGO_OFERTA_1 |
+----------+--------------+----+-----------------+---+----------------+
| CODIGO_  | Código de    | Te |                 |   | Lead.C         |
| OFERTA_2 | oferta 2     | xt |                 |   | ODIGO_OFERTA_2 |
+----------+--------------+----+-----------------+---+----------------+
| CODIGO_  | Código de    | Te |                 |   | Lead.C         |
| OFERTA_3 | oferta 3     | xt |                 |   | ODIGO_OFERTA_3 |
+----------+--------------+----+-----------------+---+----------------+
| CODIGO_  | Código de    | Te |                 |   | Lead.C         |
| OFERTA_4 | oferta 4     | xt |                 |   | ODIGO_OFERTA_4 |
+----------+--------------+----+-----------------+---+----------------+
| CODIGO_  | Código de    | Te |                 |   | Lead.C         |
| OFERTA_5 | oferta 5     | xt |                 |   | ODIGO_OFERTA_5 |
+----------+--------------+----+-----------------+---+----------------+
| CODIGO_  | Código de    | Te |                 |   | Lead.C         |
| OFERTA_6 | oferta 6     | xt |                 |   | ODIGO_OFERTA_6 |
+----------+--------------+----+-----------------+---+----------------+
| CODIGO_  | Código de    | Te |                 |   | Lead.C         |
| OFERTA_7 | oferta 7     | xt |                 |   | ODIGO_OFERTA_7 |
+----------+--------------+----+-----------------+---+----------------+

[Los mappings señalados en amarillo son los que se intercambiarán en
cuanto a identificador de cookie cuando se tenga implantado Interaction
Studio. Campo Cookie Interaction Studio es "salesforce_evga".]{.mark}

### Users IS

+---------------+----------+------+---------+-----+------------------+
| **Nombre      | **Descr  | **Ti | **Cá    | **P | **Mapping**      |
| Atributo**    | ipción** | po** | lculo** | K** |                  |
+===============+==========+======+=========+=====+==================+
| Apellido      |          |      |         |     | Individual.Last  |
|               |          |      |         |     | Name             |
+---------------+----------+------+---------+-----+------------------+
| Código País   |          |      |         |     | Contact Point    |
|               |          |      |         |     | Phone.Phone      |
|               |          |      |         |     | Country Code     |
+---------------+----------+------+---------+-----+------------------+
| Customer ID   |          |      |         |     | Ind              |
|               |          |      |         |     | ividual.Customer |
|               |          |      |         |     | Id               |
|               |          |      |         |     |                  |
|               |          |      |         |     | Party            |
|               |          |      |         |     | Identificatio    |
|               |          |      |         |     | n.Identification |
|               |          |      |         |     | Number           |
+---------------+----------+------+---------+-----+------------------+
| DNI           |          |      |         |     | Individua        |
|               |          |      |         |     | l.Identification |
|               |          |      |         |     | Number           |
+---------------+----------+------+---------+-----+------------------+
| Email Address |          |      |         |     | Contact Point    |
|               |          |      |         |     | Email.Email      |
|               |          |      |         |     | Address          |
+---------------+----------+------+---------+-----+------------------+
| External User |          |      |         |     |                  |
| ID            |          |      |         |     |                  |
+---------------+----------+------+---------+-----+------------------+
| IS Anonymous  |          |      |         |     | [Indiv           |
|               |          |      |         |     | idual.Is](http:/ |
|               |          |      |         |     | /individual.is/) |
|               |          |      |         |     | Anonymous        |
+---------------+----------+------+---------+-----+------------------+
| Nombre        |          |      |         |     | Individual.First |
|               |          |      |         |     | Name             |
+---------------+----------+------+---------+-----+------------------+
| Phone         |          |      |         |     | Contact Point    |
|               |          |      |         |     | Phone.Telephone  |
|               |          |      |         |     | Number           |
+---------------+----------+------+---------+-----+------------------+
| Profile ID    |          |      |         | X   | Indiv            |
|               |          |      |         |     | idual.Individual |
|               |          |      |         |     | Id (PK)          |
|               |          |      |         |     |                  |
|               |          |      |         |     | In               |
|               |          |      |         |     | dividual.Profile |
|               |          |      |         |     | Id               |
|               |          |      |         |     |                  |
|               |          |      |         |     | Contact Point    |
|               |          |      |         |     | Email.Party      |
|               |          |      |         |     |                  |
|               |          |      |         |     | Contact Point    |
|               |          |      |         |     | Email.Contact    |
|               |          |      |         |     | Point Email Id   |
|               |          |      |         |     |                  |
|               |          |      |         |     | Contact Point    |
|               |          |      |         |     | Phone.Party      |
|               |          |      |         |     |                  |
|               |          |      |         |     | Contact Point    |
|               |          |      |         |     | Phone.Contact    |
|               |          |      |         |     | Point Phone Id   |
|               |          |      |         |     |                  |
|               |          |      |         |     | Party            |
|               |          |      |         |     | Iden             |
|               |          |      |         |     | tification.Party |
|               |          |      |         |     |                  |
|               |          |      |         |     | Party            |
|               |          |      |         |     | Iden             |
|               |          |      |         |     | tification.Party |
|               |          |      |         |     | Identification   |
|               |          |      |         |     | Id               |
+---------------+----------+------+---------+-----+------------------+
| Salesforce    |          |      |         |     |                  |
| Contact ID    |          |      |         |     |                  |
+---------------+----------+------+---------+-----+------------------+
| Salesforce    |          |      |         |     |                  |
| MArketing     |          |      |         |     |                  |
| Cloud Contact |          |      |         |     |                  |
| Key           |          |      |         |     |                  |
+---------------+----------+------+---------+-----+------------------+
| SFMC Party    |          |      |         |     | Party            |
| i             |          |      |         |     | Iden             |
| dentification |          |      |         |     | tification.Party |
| Name          |          |      |         |     | Identification   |
|               |          |      |         |     | Name             |
+---------------+----------+------+---------+-----+------------------+
| SFMC Party    |          |      |         |     | Party            |
| I             |          |      |         |     | Iden             |
| dentification |          |      |         |     | tification.Party |
| Type          |          |      |         |     | Identification   |
|               |          |      |         |     | Type             |
+---------------+----------+------+---------+-----+------------------+
| Telefono      |          |      |         |     | Contact Point    |
| largo         |          |      |         |     | Phone.Formatted  |
|               |          |      |         |     | E164 Phone       |
|               |          |      |         |     | Number           |
+---------------+----------+------+---------+-----+------------------+

### Base Events IS

  -----------------------------------------------------------------------------------------------
  **Nombre        **Descripción**       **Tipo**   **Cálculo**   **PK**   **Mapping**
  Atributo**                                                              
  --------------- --------------------- ---------- ------------- -------- -----------------------
  Action                                                                  Website
                                                                          Engagement.Engagement
                                                                          Channel Action

  Customer ID                                                             

  Email Address                                                           

  Event Date                                                              

  Event Time                                                              Website
                                                                          Engagement.Engagement
                                                                          Date Time

  Id                                                             X        Website
                                                                          Engagement.Website
                                                                          Engagement Id (PK)

  Item Action                                                             

  Page View Flag                                                          

  Profile ID                                                              Website
                                                                          Engagement.Individual

  Salesforce                                                              
  Marketing Cloud                                                         
  Contact Key                                                             

  Source                                                                  
  Application                                                             

  Source                                                                  
  Application                                                             
  Version                                                                 

  Source Channel                                                          Website
                                                                          Engagement.Engagement
                                                                          Channel Type

  Source Device                                                           Website
                                                                          Engagement.Device IP
                                                                          Address

  Source Locale                                                           Website
                                                                          Engagement.Device
                                                                          Locale

  Source                                                                  Website
  Operating                                                               Engagement.Device OS
  System                                                                  Name

  Source OS                                                               
  Version                                                                 

  Source Page                                                             Website
  Type                                                                    Engagement.Engagement
                                                                          Asset

  Source URL                                                              Website Engagement.Page
                                                                          URL

  Source URL                                                              Website
  Referrer                                                                Engagement.Referrer URL
  -----------------------------------------------------------------------------------------------

### Catalog Events IS

+-------------+-------------------+----+---+---+-----------------------+
| **Nombre    | **Descripción**   | ** | * | * | **Mapping**           |
| Atributo**  |                   | Ti | * | * |                       |
|             |                   | po | C | P |                       |
|             |                   | ** | á | K |                       |
|             |                   |    | l | * |                       |
|             |                   |    | c | * |                       |
|             |                   |    | u |   |                       |
|             |                   |    | l |   |                       |
|             |                   |    | o |   |                       |
|             |                   |    | * |   |                       |
|             |                   |    | * |   |                       |
+=============+===================+====+===+===+=======================+
| Action      |                   |    |   |   | Product Browse        |
|             |                   |    |   |   | Engagement.Engagement |
|             |                   |    |   |   | Channel Action        |
+-------------+-------------------+----+---+---+-----------------------+
| Catalog     |                   |    |   |   | Product Browse        |
| Item Id     |                   |    |   |   | Engagement.Product    |
+-------------+-------------------+----+---+---+-----------------------+
| Catalog     |                   |    |   |   |                       |
| Item Type   |                   |    |   |   |                       |
+-------------+-------------------+----+---+---+-----------------------+
| Customer Id |                   |    |   |   |                       |
+-------------+-------------------+----+---+---+-----------------------+
| Email       |                   |    |   |   |                       |
| Address     |                   |    |   |   |                       |
+-------------+-------------------+----+---+---+-----------------------+
| Event Date  |                   |    |   |   |                       |
+-------------+-------------------+----+---+---+-----------------------+
| Event Time  |                   |    |   |   | Product Browse        |
|             |                   |    |   |   | Engagement.Created    |
|             |                   |    |   |   | Date                  |
|             |                   |    |   |   |                       |
|             |                   |    |   |   | Product Browse        |
|             |                   |    |   |   | Engagement.Engagement |
|             |                   |    |   |   | Date Time             |
+-------------+-------------------+----+---+---+-----------------------+
| Id          |                   |    |   | X | Product Browse        |
|             |                   |    |   |   | Engagement.Product    |
|             |                   |    |   |   | Browse Engagement Id  |
|             |                   |    |   |   | (PK)                  |
+-------------+-------------------+----+---+---+-----------------------+
| Item Action |                   |    |   |   |                       |
+-------------+-------------------+----+---+---+-----------------------+
| Page View   |                   |    |   |   |                       |
| Flag        |                   |    |   |   |                       |
+-------------+-------------------+----+---+---+-----------------------+
| Profile Id  |                   |    |   |   | Product Browse        |
|             |                   |    |   |   | Engagement.Individual |
+-------------+-------------------+----+---+---+-----------------------+
| Salesforce  |                   |    |   |   |                       |
| Marketing   |                   |    |   |   |                       |
| Cloud       |                   |    |   |   |                       |
| Contact Key |                   |    |   |   |                       |
+-------------+-------------------+----+---+---+-----------------------+
| Source      |                   |    |   |   |                       |
| Application |                   |    |   |   |                       |
+-------------+-------------------+----+---+---+-----------------------+
| Source      |                   |    |   |   |                       |
| Application |                   |    |   |   |                       |
| Version     |                   |    |   |   |                       |
+-------------+-------------------+----+---+---+-----------------------+
| Source      |                   |    |   |   | Product Browse        |
| Channel     |                   |    |   |   | Engagement.Engagement |
|             |                   |    |   |   | Channel Type          |
+-------------+-------------------+----+---+---+-----------------------+
| Source      |                   |    |   |   |                       |
| Device      |                   |    |   |   |                       |
+-------------+-------------------+----+---+---+-----------------------+
| Source      |                   |    |   |   | Product Browse        |
| Locale      |                   |    |   |   | Engagement.Device     |
|             |                   |    |   |   | Locale                |
+-------------+-------------------+----+---+---+-----------------------+
| Source      |                   |    |   |   |                       |
| Operating   |                   |    |   |   |                       |
| System      |                   |    |   |   |                       |
+-------------+-------------------+----+---+---+-----------------------+
| Source OS   |                   |    |   |   |                       |
| Version     |                   |    |   |   |                       |
+-------------+-------------------+----+---+---+-----------------------+
| Source Page |                   |    |   |   |                       |
| Type        |                   |    |   |   |                       |
+-------------+-------------------+----+---+---+-----------------------+
| Source URL  |                   |    |   |   | Product Browse        |
|             |                   |    |   |   | Engagement.Product    |
|             |                   |    |   |   | View URL              |
+-------------+-------------------+----+---+---+-----------------------+
| Source URL  |                   |    |   |   | Product Browse        |
| Referrer    |                   |    |   |   | Engagement.Referrer   |
|             |                   |    |   |   | URL                   |
+-------------+-------------------+----+---+---+-----------------------+

## Gestión de Consentimientos

Para hacer un correcto modelado de datos de los consentimientos, se debe
entender cada uno de ellos. Se tiene un total de seis consentimientos,
cuatro que pertenecen a Iberdrola y dos de Robinson los cuales obtiene
SAS mediante una llamada API a Adigital.

Consentimientos Iberdrola:

1.  IND_CONSEN_COMUNICACION: Consiento recibir comunicaciones
    comerciales de ofertas y promociones de terceros y, de Iberdrola
    tras la baja del contrato.

2.  IND_OPOSI_COMUNICACION: Me opongo a recibir comunicaciones
    comerciales de Iberdrola. No recibiré ninguna comunicación sobre
    ofertas y promociones de Iberdrola.

3.  IND_OPOSI_PERFILADO: Me opongo a que Iberdrola utilice mi
    información como cliente para que adapten las comunicaciones
    comerciales a mis intereses y necesidades. No recibiré
    comunicaciones que pudieran ser de mi interés (por ejemplo,
    descuentos o promociones que me permitan ahorrar).

4.  IND_CONSEN_TERCEROS: Consiento que Iberdrola utilice información de
    terceras empresas para adaptar las ofertas y promociones a mis
    intereses y necesidades.

Consentimientos Robinson:

1.  IND_ROBINSON_ADIGITAL: indica si una persona se opone a recibir
    publicidad y/o comunicaciones comerciales por correo electrónico o a
    su número de teléfono de empresas a las que no ha dado su
    consentimiento.

Por lo tanto, para hacer los mappings entre consentimientos y los
objetos estándar de privacidad de Data Cloud (Contact Point Consent y
Party Consent) se ingestarán seis tipos de ficheros, cada uno
corresponderá a un tipo de consentimiento:

-   Consentimientos Terceros

-   Consentimientos Winback

-   Consentimientos Perfilado

-   Consentimientos Comunicaciones

-   Consentimientos Robinson

-   Email

-   Phone

Es importante destacar que los consentimientos están asociados a un
cliente y sociedad, porque este puede pertenecer a varias sociedades.

## Reglas de unificación

[Para hacer reglas de unificación entre usuarios conocidos y
desconocidos, leads (sin ser clientes) e Individuals en Data Cloud,
primero se necesita comprender los pasos y conceptos relacionados con la
ingesta y modelado de datos dentro de la herramienta, y la creación de
perfiles unificados. Los pasos a seguir son:]{.mark}

**[Ingesta de datos en bruto desde las fuentes de datos (Apartado Data
Streams)]{.mark}**

1.  [**Mapeo y modelado de datos**. Los datos de los flujos de datos
    (data stream) deben mapearse a objetos como Party Identification e
    Individual para que funcionen los conjuntos de reglas de resolución
    de identidad. En este punto debemos tener en cuenta que cada entidad
    de entrada solo puede tener mapeado un Party Identification, es
    debido a que Salesforce ha optado por un modelo de datos normalizado
    de entrada.]{.mark}

2.  [**Creación de conjuntos de reglas de resolución de identidad**.
    Después de completar los pasos de modelado y mapeado, se deben crear
    conjuntos de reglas de resolución de identidad. Estos conjuntos de
    reglas ayudan a buscar y unificar perfiles en los diferentes flujos
    de datos.]{.mark}

3.  [**Creación de perfiles individuales unificados**. Una vez que se
    han establecido los conjuntos de reglas, el sistema crea perfiles
    individuales unificados que pueden usarse para segmentación y
    activaciones.]{.mark}

  -----------------------------------------------------------------------------------------------------------------------------------
  **Campo        **Cliente**   **Contrato**   **Servicio**   **Consentimientos**   **Cats/IS**   **Delio**   **Comentarios**
  entrada**                                                                                                  
  -------------- ------------- -------------- -------------- --------------------- ------------- ----------- ------------------------
  **First Name** x                                                                                           

  **Last Name**  x                                                                                           

  **Telefono**                                               x                     ?             x           Necesitaremos el
                                                                                                             telefono sin formatear y
                                                                                                             el telefono con el
                                                                                                             prefijo por delante para
                                                                                                             hacer una regla de match

  **Email**                                                  x                     ?                         Necesitaremos el email y
                                                                                                             su dominio para hacer
                                                                                                             una regla de match (ya
                                                                                                             que mínimo son dos
                                                                                                             coincidencias)

  **Cod          x (PI)                                      x                     x             x           
  Cliente**                                                                                                  

  **DNI/CIF**    x                                                                 ?                         No crearemos reglas de
                                                                                                             coincidencia por este
                                                                                                             campo de entrada ya que
                                                                                                             nos sirve para matchear
                                                                                                             Cliente con Cats/IS y
                                                                                                             esto ya lo conseguimos
                                                                                                             con el Telefono o email.

  **Cookie                                                                         x (PI)        x (PI)      Es mejor utilizar como
  Cats**                                                                                                     Party Identification la
                                                                                                             Cookie en la interfaz de
                                                                                                             entrada de Cats ya que
                                                                                                             es la única que siempre
                                                                                                             te viene informada, y
                                                                                                             puedes matchear contra
                                                                                                             Delio
  -----------------------------------------------------------------------------------------------------------------------------------

[Importante destacar que como Party Identification podremos mapear tanto
los DNIs como las cookies, uno por interfaz de flujo de datos de
entrada. En la tala se señala después de cada "x" con (PI)]{.mark}

[Para crear un conjunto de reglas de resolución de identidad, se pueden
seguir estos pasos:]{.mark}

1.  [Haz clic en \"Nuevo\".]{.mark}

2.  [Selecciona la entidad \"Individual\" que en el caso de Iberdrola es
    sobre la que se crearán ya que es B2B.]{.mark}

3.  [Agrega un ID de conjunto de reglas opcional de hasta cuatro
    caracteres de texto. Ten en cuenta que una vez que estableces un ID
    de conjunto de reglas, no puede cambiarse. En el naming convention
    viene indicado este punto.]{.mark}

4.  [Haz clic en \"Siguiente\".]{.mark}

5.  [Nombra tu conjunto de reglas y agrega una descripción opcional para
    ayudar a identificar sus propiedades.]{.mark}

6.  [Haz clic en \"Guardar".]{.mark}

[Para decidir qué reglas de coincidencia tienen sentido para Iberdrola,
se deberían revisar los datos desconectados de tus leads. Por ejemplo,
puedes unificar los perfiles de un lead utilizando las siguientes reglas
de coincidencia:]{.mark}

-   [Primer nombre difuso y dirección normalizada exacta.]{.mark}

-   [Primer nombre difuso e identificador de parte exacta.]{.mark}

-   [Primer nombre exacto y correo electrónico normalizado
    exacto.]{.mark}

-   [Primer nombre exacto y teléfono normalizado exacto.]{.mark}

-   [Custom]{.mark}

Para este proyecto crearemos un único set de reglas de unificación
llamado "Sidat/CATs/IS/Delio Unification" e identificado por id01 donde
crearemos:

-   4 reglas de matcheo:

    -   Exact Cookie Identifier. Donde el criterio será sobre el objeto
        Party Identification, el campo Identification Number y el método
        Exact. Sobre el tipo "Cookie Identifier" y nombre "Cookie cats".

    -   Exact Formatted E164 Phone: Donde el criterio será que sobre el
        objeto Contact Point Phone que el atributo Formatted E164 Phone
        sea exactamente igual.

    -   Exact Contact Point Email: Donde el criterio será que sobre el
        objeto Contact Point Email el atributo Email Address sea
        exactamente igual.

    -   Exact Client Code: Donde el criterio será que el objeto Party
        Identification y atributo Identification Number sean exactos
        donde el Party Identification Type sea Person Identifier y Party
        Identification Name sea MC Subscriber Key.

Además deberemos seleccionar las reglas de reconciliación de cada campo
a unificar de los 5 objetos a crear unificados:

1.  Contact Point Address. Por defecto todos los campos (Most Frequent).

2.  Contact Point Email. Para este seleccionaremos personalizado como
    "Source Priority" donde indicaremos que la fuente de prioridad es la
    interfaz de consentimientos Email y luego SFMC Contact Point Email,
    si no Cats.

3.  Contact Point Phone. Para este seleccionaremos personalizado como
    "Source Priority" donde indicaremos que la fuente de prioridad es la
    interfaz de consentimientos Phone y luego Delio, si no Cats.

4.  Individual. Para este seleccionaremos personalizado como "Source
    Priority" donde indicaremos que la fuente de prioridad es la
    interfaz de clientes procedente de SAS-Sidat.

5.  Party Identification. Por defecto todos los campos (Most Frequent).

Una vez que tengamos configurado los conjuntos de reglas, vamos a
monitorear y editar los conjuntos de reglas desde la pestaña
\"Resoluciones de identidad\". Para aumentar la tasa de consolidación,
intentaremos agregar más reglas de coincidencia si es que tenemos campos
comunes para ello, que en principio no tenemos muchos más.

Algunas de las **buenas prácticas** a tener en cuenta al usar Data Cloud
y las herramientas de resolución de identidad incluyen:

-   Usar conjuntos de reglas para comparar y probar reglas de
    coincidencia y reconciliación. Una vez que hayamos configurado
    nuestro primer conjunto de reglas, crearemos un segundo para
    realizar pruebas A/B.

-   Revisar regularmente los perfiles individuales unificados desde
    Profile Explorer para ver si es necesario hacer ajustes en los
    conjuntos de reglas.

Por último se va a explicar con un ejemplo como se consigue unificar
perfiles por teléfono, ya que con esta regla de unificación nunca se
agruparán en uno perfiles provenientes de SIDAT, ya que es SAS quien
unifica estos teléfonos/emails seleccionando el mejor para activar.
Además en CDP se hace una tercera unificación para nunca tener
duplicados.

Entonces existen tres procesos de unificación (2 ejecutados en entornos
previos al CDP, es decir SAS y otro en CDP):

1.  Proceso de SAS donde cada Cliente solo puede tener un email y
    telefono y cada Telefono e email solo pueden tener un cliente.

2.  Proceso de SAS para dato procedente de Delio donde si un leadId me
    viene con un teléfono que ya existe en la tabla de clientes le
    asigna el cod_cliente existente.

3.  Proceso de CDP donde construye una vista de clientes unificados
    basándose en las reglas explicadas en apartado.

Indicar que de Delio se vuelca todos los días un histórico de datos de 1
mes atrás. Por lo tanto, si un leadId el día N no hace match con un
cliente de iberdrola lo puede hacer el día N+1, porque ese telefono ya
esta dado de alta como cliente en Iberdrola.

En el diagrama que se muestra a continuación se puede ver los 3 procesos
de unificación en recuadros verdes y los flujos de datos con recuadros
blancos.

## ![](media/image19.png){width="6.267716535433071in" height="3.4305555555555554in"}

## Calculated Insights

Con esta funcionalidad se pueden generar métricas calculadas dentro de
Data Cloud. A fecha de diseño de este documento no se tiene ninguna
métrica excepto el Valor de un cliente, aunque de momento no se tenga la
fórmula para calcularlo. Por lo que de momento no se ha diseñado ninguna
métrica.

## Segmentos

En este apartado se pretende enumerar las reglas de segmentación para
cada audiencia a crear en Data Cloud, separadas por caso de uso.

+---+---------+----------+--------------+-----+----+------------------+
| * | **Caso  | **Nombre | *            | **S | *  | **Reglas de      |
| * | de      | del      | *Descripción | egm | *P | Segmentación**   |
| I | Uso**   | Se       | del          | ent | ub |                  |
| d |         | gmento** | Segmento**   | O   | li |                  |
| * |         |          |              | n** | sh |                  |
| * |         |          |              |     | Sc |                  |
|   |         |          |              |     | he |                  |
|   |         |          |              |     | du |                  |
|   |         |          |              |     | le |                  |
|   |         |          |              |     | ** |                  |
+===+=========+==========+==============+=====+====+==================+
| 1 | All     | Clientes | Clientes que | U   | 2  | INCLUDE RULES    |
|   |         | cont     | tengan al    | nif | 4h |                  |
|   |         | actables | menos un     | ied |    | count(Unified    |
|   |         | por      | punto de     | I   |    | Indv Contact     |
|   |         | telefono | contacto de  | ndi |    | Point Phone      |
|   |         |          | teléfono     | vid |    | id01.Telephone   |
|   |         |          | informado    | ual |    | Number matches   |
|   |         |          | c            |     |    | \"\\d{9}\") at   |
|   |         |          | orrectamente |     |    | least 1 AND      |
|   |         |          | (9 dígitos)  |     |    |                  |
|   |         |          | y que tengan |     |    | count(Party      |
|   |         |          | los          |     |    | Consent.Name is  |
|   |         |          | con          |     |    | equal to         |
|   |         |          | sentimientos |     |    | \"Communications |
|   |         |          | de           |     |    | Consent\" AND    |
|   |         |          | perfilado,   |     |    | Party            |
|   |         |          | co           |     |    | Consent.Consent  |
|   |         |          | municaciones |     |    | Status is equal  |
|   |         |          | y de         |     |    | to \"Opt In\")   |
|   |         |          | robinson de  |     |    | at least 1 AND   |
|   |         |          | telefono     |     |    | count(Party      |
|   |         |          | aceptados,   |     |    | Consent.Name is  |
|   |         |          | además se    |     |    | equal to         |
|   |         |          | excluyen     |     |    | \"Profiling      |
|   |         |          | aquellos que |     |    | Consent\" AND    |
|   |         |          | sean         |     |    | Party            |
|   |         |          | clientes     |     |    | Consent.Consent  |
|   |         |          | VIP,         |     |    | Status is equal  |
|   |         |          | vulnerables  |     |    | to \"Opt In\")   |
|   |         |          | o con al     |     |    | at least 1 AND   |
|   |         |          | menos un     |     |    | count(Contact    |
|   |         |          | contrato de  |     |    | Point            |
|   |         |          | morosos.     |     |    | Consent.Name is  |
|   |         |          |              |     |    | equal to         |
|   |         |          |              |     |    | \"Contact Point  |
|   |         |          |              |     |    | Phone Consent\"  |
|   |         |          |              |     |    | AND Contact      |
|   |         |          |              |     |    | Point            |
|   |         |          |              |     |    | Consent.Consent  |
|   |         |          |              |     |    | Status is equal  |
|   |         |          |              |     |    | to \"Opt In\")   |
|   |         |          |              |     |    | at least 1       |
|   |         |          |              |     |    |                  |
|   |         |          |              |     |    | EXCLUDE RULES    |
|   |         |          |              |     |    |                  |
|   |         |          |              |     |    | Unified          |
|   |         |          |              |     |    | Individual.VIP   |
|   |         |          |              |     |    | Client is equal  |
|   |         |          |              |     |    | to 1 OR          |
|   |         |          |              |     |    | Vulnerable       |
|   |         |          |              |     |    | Client is equal  |
|   |         |          |              |     |    | to 1 OR          |
|   |         |          |              |     |    | cou              |
|   |         |          |              |     |    | nt(Contract.Past |
|   |         |          |              |     |    | Due Cliente      |
|   |         |          |              |     |    | Indicator is     |
|   |         |          |              |     |    | equal to 1) at   |
|   |         |          |              |     |    | least 1          |
+---+---------+----------+--------------+-----+----+------------------+
| 2 | All     | Clientes | Clientes que | U   | 2  | INCLUDE RULES    |
|   |         | cont     | tengan al    | nif | 4h |                  |
|   |         | actables | menos un     | ied |    | count(Unified    |
|   |         | por      | punto de     | I   |    | Indv Contact     |
|   |         | email    | contacto de  | ndi |    | Point Email      |
|   |         |          | email        | vid |    | id01.email       |
|   |         |          | informado    | ual |    | matches          |
|   |         |          | c            |     |    | \"\              |
|   |         |          | orrectamente |     |    | ^\[a-zA-Z0-9.!#\ |
|   |         |          | y que tengan |     |    | $%&\'\*+/=?\^\_\ |
|   |         |          | los          |     |    | `{\|}\~-\]+@\[a- |
|   |         |          | con          |     |    | zA-Z0-9-\]+\\.\[ |
|   |         |          | sentimientos |     |    | a-zA-Z0-9-\]+\") |
|   |         |          | de           |     |    | at least 1 AND   |
|   |         |          | perfilado,   |     |    |                  |
|   |         |          | co           |     |    | count(Party      |
|   |         |          | municaciones |     |    | Consent.Name is  |
|   |         |          | y de         |     |    | equal to         |
|   |         |          | robinson de  |     |    | \"Communications |
|   |         |          | email        |     |    | Consent\" AND    |
|   |         |          | aceptados,   |     |    | Party            |
|   |         |          | además se    |     |    | Consent.Consent  |
|   |         |          | excluyen     |     |    | Status is equal  |
|   |         |          | aquellos que |     |    | to \"Opt In\")   |
|   |         |          | sean         |     |    | at least 1 AND   |
|   |         |          | clientes     |     |    | count(Party      |
|   |         |          | VIP,         |     |    | Consent.Name is  |
|   |         |          | vulnerables  |     |    | equal to         |
|   |         |          | o con al     |     |    | \"Profiling      |
|   |         |          | menos un     |     |    | Consent\" AND    |
|   |         |          | contrato de  |     |    | Party            |
|   |         |          | morosos.     |     |    | Consent.Consent  |
|   |         |          |              |     |    | Status is equal  |
|   |         |          |              |     |    | to \"Opt In\")   |
|   |         |          |              |     |    | at least 1 AND   |
|   |         |          |              |     |    | count(Contact    |
|   |         |          |              |     |    | Point            |
|   |         |          |              |     |    | Consent.Name is  |
|   |         |          |              |     |    | equal to         |
|   |         |          |              |     |    | \"Contact Point  |
|   |         |          |              |     |    | Email Consent\"  |
|   |         |          |              |     |    | AND Contact      |
|   |         |          |              |     |    | Point            |
|   |         |          |              |     |    | Consent.Consent  |
|   |         |          |              |     |    | Status is equal  |
|   |         |          |              |     |    | to \"Opt In\")   |
|   |         |          |              |     |    | at least 1       |
|   |         |          |              |     |    |                  |
|   |         |          |              |     |    | EXCLUDE RULES    |
|   |         |          |              |     |    |                  |
|   |         |          |              |     |    | Unified          |
|   |         |          |              |     |    | Individual.VIP   |
|   |         |          |              |     |    | Client is equal  |
|   |         |          |              |     |    | to 1 OR          |
|   |         |          |              |     |    | Vulnerable       |
|   |         |          |              |     |    | Client is equal  |
|   |         |          |              |     |    | to 1 OR          |
|   |         |          |              |     |    | cou              |
|   |         |          |              |     |    | nt(Contract.Past |
|   |         |          |              |     |    | Due Cliente      |
|   |         |          |              |     |    | Indicator is     |
|   |         |          |              |     |    | equal to 1) at   |
|   |         |          |              |     |    | least 1          |
+---+---------+----------+--------------+-----+----+------------------+
| 3 |         | Grupo de | Clientes que | U   | 2  | INCLUDE RULES    |
|   |         | control  | tengan los   | nif | 4h |                  |
|   |         | Smart    | con          | ied |    | count(Party      |
|   |         | Solar    | sentimientos | I   |    | Consent.Name is  |
|   |         |          | de perfilado | ndi |    | equal to         |
|   |         |          | aceptados y  | vid |    | \"Communications |
|   |         |          | pertenezcan  | ual |    | Consent\" AND    |
|   |         |          | al grupo de  |     |    | Party            |
|   |         |          | control      |     |    | Consent.Consent  |
|   |         |          | Smart Solar  |     |    | Status is equal  |
|   |         |          |              |     |    | to \"Opt In\")   |
|   |         |          |              |     |    | at least 1 AND   |
|   |         |          |              |     |    | count(Party      |
|   |         |          |              |     |    | Consent.Name is  |
|   |         |          |              |     |    | equal to         |
|   |         |          |              |     |    | \"Profiling      |
|   |         |          |              |     |    | Consent\" AND    |
|   |         |          |              |     |    | Party            |
|   |         |          |              |     |    | Consent.Consent  |
|   |         |          |              |     |    | Status is equal  |
|   |         |          |              |     |    | to \"Opt In\")   |
|   |         |          |              |     |    | at least 1 AND   |
|   |         |          |              |     |    | Unified          |
|   |         |          |              |     |    | Individual.NBA   |
|   |         |          |              |     |    | Type is equal to |
|   |         |          |              |     |    | \"Grupo Control  |
|   |         |          |              |     |    | Solar\"          |
|   |         |          |              |     |    |                  |
|   |         |          |              |     |    | EXCLUDE RULES    |
|   |         |          |              |     |    |                  |
|   |         |          |              |     |    | Unified          |
|   |         |          |              |     |    | Individual.VIP   |
|   |         |          |              |     |    | Client is equal  |
|   |         |          |              |     |    | to 1 OR          |
|   |         |          |              |     |    | Vulnerable       |
|   |         |          |              |     |    | Client is equal  |
|   |         |          |              |     |    | to 1 OR          |
|   |         |          |              |     |    | cou              |
|   |         |          |              |     |    | nt(Contract.Past |
|   |         |          |              |     |    | Due Cliente      |
|   |         |          |              |     |    | Indicator is     |
|   |         |          |              |     |    | equal to 1) at   |
|   |         |          |              |     |    | least 1          |
+---+---------+----------+--------------+-----+----+------------------+
| 4 |         | Ofrecer  | Clientes que | U   | 2  | INCLUDE RULES    |
|   |         | Smart    | tengan los   | nif | 4h |                  |
|   |         | Solar    | con          | ied |    | count(Party      |
|   |         |          | sentimientos | I   |    | Consent.Name is  |
|   |         |          | de perfilado | ndi |    | equal to         |
|   |         |          | aceptados y  | vid |    | \"Communications |
|   |         |          | pertenezcan  | ual |    | Consent\" AND    |
|   |         |          | al grupo de  |     |    | Party            |
|   |         |          | ofrecer      |     |    | Consent.Consent  |
|   |         |          | Smart Solar  |     |    | Status is equal  |
|   |         |          |              |     |    | to \"Opt In\")   |
|   |         |          |              |     |    | at least 1 AND   |
|   |         |          |              |     |    | count(Party      |
|   |         |          |              |     |    | Consent.Name is  |
|   |         |          |              |     |    | equal to         |
|   |         |          |              |     |    | \"Profiling      |
|   |         |          |              |     |    | Consent\" AND    |
|   |         |          |              |     |    | Party            |
|   |         |          |              |     |    | Consent.Consent  |
|   |         |          |              |     |    | Status is equal  |
|   |         |          |              |     |    | to \"Opt In\")   |
|   |         |          |              |     |    | at least 1 AND   |
|   |         |          |              |     |    | Unified          |
|   |         |          |              |     |    | Individual.NBA   |
|   |         |          |              |     |    | Type is equal to |
|   |         |          |              |     |    | \"Ofrecer Smart  |
|   |         |          |              |     |    | Solar\"          |
|   |         |          |              |     |    |                  |
|   |         |          |              |     |    | EXCLUDE RULES    |
|   |         |          |              |     |    |                  |
|   |         |          |              |     |    | Unified          |
|   |         |          |              |     |    | Individual.VIP   |
|   |         |          |              |     |    | Client is equal  |
|   |         |          |              |     |    | to 1 OR          |
|   |         |          |              |     |    | Vulnerable       |
|   |         |          |              |     |    | Client is equal  |
|   |         |          |              |     |    | to 1 OR          |
|   |         |          |              |     |    | cou              |
|   |         |          |              |     |    | nt(Contract.Past |
|   |         |          |              |     |    | Due Cliente      |
|   |         |          |              |     |    | Indicator is     |
|   |         |          |              |     |    | equal to 1) at   |
|   |         |          |              |     |    | least 1          |
+---+---------+----------+--------------+-----+----+------------------+
| 5 |         | Clientes | Clientes     | U   | 2  | INCLUDE RULES    |
|   |         | solo luz | contactables | nif | 4h |                  |
|   |         | con      | por email    | ied |    | Clientes         |
|   |         | tactable | que tengan   | I   |    | contactables por |
|   |         | email    | contratos de | ndi |    | email AND        |
|   |         |          | luz (1 o     | vid |    | count((          |
|   |         |          | más) y       | ual |    | Contract.Status) |
|   |         |          | exlcuyamos a |     |    | is equal to      |
|   |         |          | los que      |     |    | \"AL\" AND       |
|   |         |          | tengan Gas   |     |    | (Contract.Type   |
|   |         |          |              |     |    | Energy Sector)   |
|   |         |          |              |     |    | is equal to      |
|   |         |          |              |     |    | \                |
|   |         |          |              |     |    | "Electricidad\") |
|   |         |          |              |     |    | at least 1       |
|   |         |          |              |     |    |                  |
|   |         |          |              |     |    | EXCLUDE RULES    |
|   |         |          |              |     |    |                  |
|   |         |          |              |     |    | count((          |
|   |         |          |              |     |    | Contract.Status) |
|   |         |          |              |     |    | is equal to      |
|   |         |          |              |     |    | \"AL\" AND       |
|   |         |          |              |     |    | (Contract.Type   |
|   |         |          |              |     |    | Energy Sector)   |
|   |         |          |              |     |    | is equal to      |
|   |         |          |              |     |    | \"Gas\") at      |
|   |         |          |              |     |    | least 1          |
+---+---------+----------+--------------+-----+----+------------------+
| 6 |         | Clientes | Clientes que | U   | 2  | INCLUDE RULES    |
|   |         | churn    | tengan       | nif | 4h |                  |
|   |         | rate     | definido un  | ied |    | Clientes         |
|   |         | alto     | punto de     | I   |    | contactables por |
|   |         |          | contacto de  | ndi |    | email AND        |
|   |         |          | email        | vid |    | Unified          |
|   |         |          | informado    | ual |    | Individual.Churn |
|   |         |          | c            |     |    | Probability is   |
|   |         |          | orrectamente |     |    | equal to         |
|   |         |          | y cuyo churn |     |    | \"ALTO\"         |
|   |         |          | probability  |     |    |                  |
|   |         |          | sea alto.    |     |    | EXCLUDE RULES    |
|   |         |          |              |     |    |                  |
|   |         |          |              |     |    | Unified          |
|   |         |          |              |     |    | Individual.VIP   |
|   |         |          |              |     |    | Client is equal  |
|   |         |          |              |     |    | to 1 OR          |
|   |         |          |              |     |    | Vulnerable       |
|   |         |          |              |     |    | Client is equal  |
|   |         |          |              |     |    | to 1 OR          |
|   |         |          |              |     |    | cou              |
|   |         |          |              |     |    | nt(Contract.Past |
|   |         |          |              |     |    | Due Cliente      |
|   |         |          |              |     |    | Indicator is     |
|   |         |          |              |     |    | equal to 1) at   |
|   |         |          |              |     |    | least 1          |
+---+---------+----------+--------------+-----+----+------------------+
| 7 | **Ev    | cdp1     | Usuario que  | U   | 2  | INCLUDE RULES    |
|   | olutivo |          | no es        | nif | 4h |                  |
|   | aerot   |          | cliente      | ied |    | Individual.Is    |
|   | ermia** |          |              | I   |    | Anonymous is     |
|   |         |          | Que visita   | ndi |    | equal to 1 AND   |
|   |         |          | la web de    | vid |    | count (Website   |
|   |         |          | aerotermia o | ual |    | Engag            |
|   |         |          | clicka el    |     |    | ement.Engagement |
|   |         |          | banner de    |     |    | Channel Action   |
|   |         |          | aerotermia   |     |    | is equal to      |
|   |         |          |              |     |    | \"Aerotermia\"   |
|   |         |          |              |     |    | AND Website      |
|   |         |          |              |     |    | Engag            |
|   |         |          |              |     |    | ement.Engagement |
|   |         |          |              |     |    | Datetine last    |
|   |         |          |              |     |    | number of days   |
|   |         |          |              |     |    | 60) at least 1   |
+---+---------+----------+--------------+-----+----+------------------+
| 8 | **Ev    | cdp2     | Clientes de  | U   | 2  | INCLUDE RULES    |
|   | olutivo |          | Iberdrola    | nif | 4h |                  |
|   | aerot   |          |              | ied |    | count(Party      |
|   | ermia** |          | Tipo de      | I   |    | Consent.Name is  |
|   |         |          | cliente      | ndi |    | equal to         |
|   |         |          | particular o | vid |    | \"Profiling      |
|   |         |          | autónomo     | ual |    | Consent\" AND    |
|   |         |          |              |     |    | Party            |
|   |         |          | Sin          |     |    | Consent.Consent  |
|   |         |          | oposición a  |     |    | Status is equal  |
|   |         |          | perfilado    |     |    | to \"Opt In\")   |
|   |         |          |              |     |    | at least 1 AND   |
|   |         |          | Que visita   |     |    | count (Website   |
|   |         |          | la web de    |     |    | Enga             |
|   |         |          | aerotermia o |     |    | gment.Engagement |
|   |         |          | clicka en el |     |    | Channel Action   |
|   |         |          | banner de    |     |    | Is equal to      |
|   |         |          | aerotermia   |     |    | \"Aerotermia\")  |
|   |         |          |              |     |    | at least 1 AND   |
|   |         |          | Que no sean  |     |    | Unified          |
|   |         |          | de zona      |     |    | Ind              |
|   |         |          | clima cálida |     |    | ividual.Customer |
|   |         |          | (A y B)      |     |    | Type IS IN       |
|   |         |          |              |     |    | (Particular,     |
|   |         |          | No           |     |    | Autononomo)      |
|   |         |          | vulnerables, |     |    |                  |
|   |         |          | no morosos,  |     |    | EXCLUDE RULES    |
|   |         |          | no vips      |     |    |                  |
|   |         |          |              |     |    | Unified          |
|   |         |          | Que no       |     |    | Individual.VIP   |
|   |         |          | tengan       |     |    | Client is equal  |
|   |         |          | abierta una  |     |    | to 1 OR Unified  |
|   |         |          | oportunidad  |     |    | Indiv            |
|   |         |          | de           |     |    | idual.Vulnerable |
|   |         |          | aerotermia   |     |    | Client is equal  |
|   |         |          |              |     |    | to 1 OR          |
|   |         |          |              |     |    | cou              |
|   |         |          |              |     |    | nt(Contract.Past |
|   |         |          |              |     |    | Due Client       |
|   |         |          |              |     |    | Indicator is     |
|   |         |          |              |     |    | equal to 1) at   |
|   |         |          |              |     |    | least 1 OR       |
|   |         |          |              |     |    | coun             |
|   |         |          |              |     |    | t(Contract.Local |
|   |         |          |              |     |    | Climate Area Is  |
|   |         |          |              |     |    | In (A1, A2, A3,  |
|   |         |          |              |     |    | A4, B1, B2, B3,  |
|   |         |          |              |     |    | B4)) at least 1  |
|   |         |          |              |     |    | OR               |
|   |         |          |              |     |    | Opportunity.Name |
|   |         |          |              |     |    | Is Equal to      |
|   |         |          |              |     |    | aerotermia       |
+---+---------+----------+--------------+-----+----+------------------+
| 9 | **Ev    | cdp3     | Clientes de  | U   | 2  | INCLUDE RULES    |
|   | olutivo |          | Iberdrola    | nif | 4h |                  |
|   | aerot   |          |              | ied |    | Clientes         |
|   | ermia** |          | Que visita   | I   |    | contactables por |
|   |         |          | la web de    | ndi |    | email AND        |
|   |         |          | aerotermia o | vid |    | count(Website    |
|   |         |          | clicka en el | ual |    | Engag            |
|   |         |          | banner de    |     |    | ement.Engagement |
|   |         |          | aerotermia   |     |    | Channel Action   |
|   |         |          |              |     |    | is equal to      |
|   |         |          | Que no sean  |     |    | Aerotermia) AND  |
|   |         |          | de zona      |     |    | Unified          |
|   |         |          | clima cálida |     |    | Ind              |
|   |         |          | (A y B)      |     |    | ividual.Customer |
|   |         |          |              |     |    | Type Is In       |
|   |         |          | No           |     |    | (Particular,     |
|   |         |          | vulnerables, |     |    | Autonomo)        |
|   |         |          | no morosos,  |     |    |                  |
|   |         |          | no vips      |     |    | EXCLUDE RULES    |
|   |         |          |              |     |    |                  |
|   |         |          | Sin          |     |    | coun             |
|   |         |          | oposición a  |     |    | t(Contract.Local |
|   |         |          | co           |     |    | Climate Area Is  |
|   |         |          | municaciones |     |    | In (A1, A2, A3,  |
|   |         |          | comerciales  |     |    | A4, B1, B2, B3,  |
|   |         |          |              |     |    | B4)) at least 1  |
|   |         |          | Sin          |     |    | AND count(Email  |
|   |         |          | oposición a  |     |    | Engag            |
|   |         |          | co           |     |    | ement.Engagement |
|   |         |          | municaciones |     |    | Channel Type is  |
|   |         |          | por email en |     |    | equal to         |
|   |         |          | Adigital     |     |    | \"Email\" AND    |
|   |         |          |              |     |    | Email            |
|   |         |          | Sin          |     |    | Engag            |
|   |         |          | oposición a  |     |    | ement.Engagement |
|   |         |          | perfilado    |     |    | Asset is equal   |
|   |         |          |              |     |    | to \"9149\" AND  |
|   |         |          | No           |     |    | Email            |
|   |         |          | recibieron   |     |    | Engag            |
|   |         |          | el email de  |     |    | ement.Engagement |
|   |         |          | aerotermia   |     |    | Channel Action   |
|   |         |          |              |     |    | is equal to      |
|   |         |          |              |     |    | \"Send\") at     |
|   |         |          |              |     |    | least 1 AND      |
|   |         |          |              |     |    | count(           |
|   |         |          |              |     |    | Opportunity.Name |
|   |         |          |              |     |    | Is Equal To      |
|   |         |          |              |     |    | \"aerotermia\")  |
|   |         |          |              |     |    | at least 1       |
+---+---------+----------+--------------+-----+----+------------------+
| 1 | **Ev    | cdp5     | Clientes de  | U   | 2  | INCLUDE RULES    |
| 0 | olutivo |          | Iberdrola    | nif | 4h |                  |
|   | aerot   |          | que han      | ied |    | count(Party      |
|   | ermia** |          | recibido el  | I   |    | Consent.Name is  |
|   |         |          | email        | ndi |    | equal to         |
|   |         |          |              | vid |    | \"Profiling      |
|   |         |          | No           | ual |    | Consent\" AND    |
|   |         |          | vulnerables, |     |    | Party            |
|   |         |          | no morosos,  |     |    | Consent.Consent  |
|   |         |          | no vips      |     |    | Status Is Equal  |
|   |         |          |              |     |    | To \"Opt In\")   |
|   |         |          | Sin          |     |    | at least 1 AND   |
|   |         |          | oposición a  |     |    | count(Email      |
|   |         |          | co           |     |    | Engag            |
|   |         |          | municaciones |     |    | ement.Engagement |
|   |         |          | comerciales  |     |    | Channel Type is  |
|   |         |          |              |     |    | equal to         |
|   |         |          | Sin          |     |    | \"Email\" AND    |
|   |         |          | oposición a  |     |    | Email            |
|   |         |          | co           |     |    | Engag            |
|   |         |          | municaciones |     |    | ement.Engagement |
|   |         |          | por email en |     |    | Asset is equal   |
|   |         |          | Adigital     |     |    | to \"9149\" AND  |
|   |         |          |              |     |    | Email            |
|   |         |          | Sin          |     |    | Engag            |
|   |         |          | oposición a  |     |    | ement.Engagement |
|   |         |          | perfilado    |     |    | Channel Action   |
|   |         |          |              |     |    | Is In (Open,     |
|   |         |          | Perfil       |     |    | Click)) at least |
|   |         |          | particular o |     |    | 1 AND Unified    |
|   |         |          | autónomo     |     |    | Individual.Is    |
|   |         |          |              |     |    | Anonymous is     |
|   |         |          |              |     |    | equal to \"0\"   |
|   |         |          |              |     |    | AND Unified      |
|   |         |          |              |     |    | Ind              |
|   |         |          |              |     |    | ividual.Customer |
|   |         |          |              |     |    | Type Is In       |
|   |         |          |              |     |    | (Particular,     |
|   |         |          |              |     |    | Autonomo)        |
|   |         |          |              |     |    |                  |
|   |         |          |              |     |    | EXCLUDE RULES    |
|   |         |          |              |     |    |                  |
|   |         |          |              |     |    | Unified          |
|   |         |          |              |     |    | Individual.VIP   |
|   |         |          |              |     |    | Client is equal  |
|   |         |          |              |     |    | to 1 OR          |
|   |         |          |              |     |    | Vulnerable       |
|   |         |          |              |     |    | Client is equal  |
|   |         |          |              |     |    | to 1 OR          |
|   |         |          |              |     |    | cou              |
|   |         |          |              |     |    | nt(Contract.Past |
|   |         |          |              |     |    | Due Cliente      |
|   |         |          |              |     |    | Indicator is     |
|   |         |          |              |     |    | equal to 1) at   |
|   |         |          |              |     |    | least 1 OR       |
|   |         |          |              |     |    | coun             |
|   |         |          |              |     |    | t(Contract.Local |
|   |         |          |              |     |    | Climate Area Is  |
|   |         |          |              |     |    | In (A1, A2, A3,  |
|   |         |          |              |     |    | A4, B1, B2, B3,  |
|   |         |          |              |     |    | B4)) at least 1  |
|   |         |          |              |     |    | OR               |
|   |         |          |              |     |    | count(           |
|   |         |          |              |     |    | Opportunity.Name |
|   |         |          |              |     |    | Is Equal To      |
|   |         |          |              |     |    | \"aerotermia\")  |
|   |         |          |              |     |    | at least 1       |
+---+---------+----------+--------------+-----+----+------------------+
| 1 | **Ev    | cdp7     | Clientes de  | U   | 2  | INCLUDE RULES    |
| 1 | olutivo |          | Iberdrola    | nif | 4h |                  |
|   | aerot   |          | que han      | ied |    | count(Party      |
|   | ermia** |          | recibido el  | I   |    | Consent.Name is  |
|   |         |          | email y no   | ndi |    | equal to         |
|   |         |          | lo han       | vid |    | \"Profiling      |
|   |         |          | abierto      | ual |    | Consent\" AND    |
|   |         |          |              |     |    | Party            |
|   |         |          | No           |     |    | Consent.Consent  |
|   |         |          | vulnerables, |     |    | Status Is Equal  |
|   |         |          | no morosos,  |     |    | To \"Opt In\")   |
|   |         |          | no vips      |     |    | at least 1 AND   |
|   |         |          |              |     |    | Unified          |
|   |         |          | Sin          |     |    | Ind              |
|   |         |          | oposición a  |     |    | ividual.Customer |
|   |         |          | co           |     |    | Type Is In       |
|   |         |          | municaciones |     |    | (Particular,     |
|   |         |          | comerciales  |     |    | Autonomo) AND    |
|   |         |          |              |     |    | count(Email      |
|   |         |          | Sin          |     |    | Engag            |
|   |         |          | oposición a  |     |    | ement.Engagement |
|   |         |          | co           |     |    | Channel Type is  |
|   |         |          | municaciones |     |    | equal to         |
|   |         |          | por email en |     |    | \"Email\" AND    |
|   |         |          | Adigital     |     |    | Email            |
|   |         |          |              |     |    | Engag            |
|   |         |          | Sin          |     |    | ement.Engagement |
|   |         |          | oposición a  |     |    | Asset is equal   |
|   |         |          | perfilado    |     |    | to \"9149\" AND  |
|   |         |          |              |     |    | Email            |
|   |         |          | Perfil       |     |    | Engag            |
|   |         |          | particular o |     |    | ement.Engagement |
|   |         |          | autónomo     |     |    | Channel Action   |
|   |         |          |              |     |    | is equal to      |
|   |         |          |              |     |    | \"Send\") at     |
|   |         |          |              |     |    | least 1 AND      |
|   |         |          |              |     |    | Unified          |
|   |         |          |              |     |    | Ind              |
|   |         |          |              |     |    | ividual.Customer |
|   |         |          |              |     |    | Type Is In       |
|   |         |          |              |     |    | (Particular,     |
|   |         |          |              |     |    | Autonomo)        |
|   |         |          |              |     |    |                  |
|   |         |          |              |     |    | EXCLUDE RULES    |
|   |         |          |              |     |    |                  |
|   |         |          |              |     |    | Unified          |
|   |         |          |              |     |    | Individual.VIP   |
|   |         |          |              |     |    | Client is equal  |
|   |         |          |              |     |    | to 1 OR          |
|   |         |          |              |     |    | Vulnerable       |
|   |         |          |              |     |    | Client is equal  |
|   |         |          |              |     |    | to 1 OR          |
|   |         |          |              |     |    | cou              |
|   |         |          |              |     |    | nt(Contract.Past |
|   |         |          |              |     |    | Due Cliente      |
|   |         |          |              |     |    | Indicator is     |
|   |         |          |              |     |    | equal to 1) at   |
|   |         |          |              |     |    | least 1 OR       |
|   |         |          |              |     |    | coun             |
|   |         |          |              |     |    | t(Contract.Local |
|   |         |          |              |     |    | Climate Area Is  |
|   |         |          |              |     |    | In (A1, A2, A3,  |
|   |         |          |              |     |    | A4, B1, B2, B3,  |
|   |         |          |              |     |    | B4)) at least 1  |
|   |         |          |              |     |    | OR               |
|   |         |          |              |     |    | count(           |
|   |         |          |              |     |    | Opportunity.Name |
|   |         |          |              |     |    | Is Equal To      |
|   |         |          |              |     |    | \"aerotermia\")  |
|   |         |          |              |     |    | at least 1 OR    |
|   |         |          |              |     |    | count(Email      |
|   |         |          |              |     |    | Engag            |
|   |         |          |              |     |    | ement.Engagement |
|   |         |          |              |     |    | Channel Type is  |
|   |         |          |              |     |    | equal to         |
|   |         |          |              |     |    | \"Email\" AND    |
|   |         |          |              |     |    | Email            |
|   |         |          |              |     |    | Engag            |
|   |         |          |              |     |    | ement.Engagement |
|   |         |          |              |     |    | Asset is equal   |
|   |         |          |              |     |    | to \"9149\" AND  |
|   |         |          |              |     |    | Email            |
|   |         |          |              |     |    | Engag            |
|   |         |          |              |     |    | ement.Engagement |
|   |         |          |              |     |    | Channel Action   |
|   |         |          |              |     |    | is equal to      |
|   |         |          |              |     |    | \"Open\") at     |
|   |         |          |              |     |    | least 1          |
+---+---------+----------+--------------+-----+----+------------------+
| 1 | **Smart | cdp8     | Clientes de  | U   |    | INCLUDE RULES    |
| 2 | solar** |          | Iberdrola    | nif |    |                  |
|   |         |          |              | ied |    | Unified          |
|   |         |          | I            | I   |    | Individual.NBA   |
|   |         |          | dentificados | ndi |    | Type is equal to |
|   |         |          | como Smart   | vid |    | \"Ofrecer Smart  |
|   |         |          | Solar (NBA   | ual |    | Solar\" AND      |
|   |         |          | Type =       |     |    | count(Party      |
|   |         |          | Ofrecer      |     |    | Consent.Name is  |
|   |         |          | Smart Solar) |     |    | equal to         |
|   |         |          |              |     |    | \"Profiling      |
|   |         |          | Que sean de  |     |    | Consent\" AND    |
|   |         |          | zona clima   |     |    | Party            |
|   |         |          | cálida (A y  |     |    | Consent.Consent  |
|   |         |          | B)           |     |    | Status Is Equal  |
|   |         |          |              |     |    | To \"Opt In\")   |
|   |         |          | No           |     |    | at least 1 AND   |
|   |         |          | vulnerables, |     |    | Unified          |
|   |         |          | no morosos,  |     |    | Ind              |
|   |         |          | no vips      |     |    | ividual.Customer |
|   |         |          |              |     |    | Type Is In       |
|   |         |          | Sin          |     |    | (Particular,     |
|   |         |          | oposición a  |     |    | Autonomo) AND    |
|   |         |          | perfilado    |     |    | coun             |
|   |         |          |              |     |    | t(Contract.Local |
|   |         |          | Perfil       |     |    | Climate Area Is  |
|   |         |          | particular o |     |    | In (A1, A2, A3,  |
|   |         |          | autónomo     |     |    | A4, B1, B2, B3,  |
|   |         |          |              |     |    | B4) AND          |
|   |         |          |              |     |    | Contract.Status  |
|   |         |          |              |     |    | is equal to      |
|   |         |          |              |     |    | \"AL\") at least |
|   |         |          |              |     |    | 1                |
|   |         |          |              |     |    |                  |
|   |         |          |              |     |    | EXCLUDE RULES    |
|   |         |          |              |     |    |                  |
|   |         |          |              |     |    | Unified          |
|   |         |          |              |     |    | Individual.VIP   |
|   |         |          |              |     |    | Client is equal  |
|   |         |          |              |     |    | to 1 OR          |
|   |         |          |              |     |    | Vulnerable       |
|   |         |          |              |     |    | Client is equal  |
|   |         |          |              |     |    | to 1 OR          |
|   |         |          |              |     |    | cou              |
|   |         |          |              |     |    | nt(Contract.Past |
|   |         |          |              |     |    | Due Cliente      |
|   |         |          |              |     |    | Indicator is     |
|   |         |          |              |     |    | equal to 1) at   |
|   |         |          |              |     |    | least 1          |
+---+---------+----------+--------------+-----+----+------------------+
| 1 | **Nega  | cdp9     | Clientes de  | U   | 2  | INCLUDE RULES    |
| 3 | tivizar |          | Iberdrola    | nif | 4h |                  |
|   | en      |          | que tengan   | ied |    | cou              |
|   | M       |          | al menos un  | I   |    | nt(Contract.Type |
|   | edios** |          | contrato en  | ndi |    | Energy Sector is |
|   |         |          | Alta         | vid |    | equal to         |
|   |         |          |              | ual |    | \"Electricidad\" |
|   |         |          | Tipo de      |     |    | AND              |
|   |         |          | Sector       |     |    | Contract.Status  |
|   |         |          | Energía sea  |     |    | is equal to      |
|   |         |          | igual a      |     |    | \"AL\") AND      |
|   |         |          | Electricidad |     |    | Unified          |
|   |         |          |              |     |    | Individual Is In |
|   |         |          | Perfil       |     |    | (Particular,     |
|   |         |          | particular o |     |    | Autonomo)        |
|   |         |          | autónomo     |     |    |                  |
+---+---------+----------+--------------+-----+----+------------------+
| 1 | **Nega  | cdp10    | Clientes de  | U   | 2  | INCLUDE RULES    |
| 4 | tivizar |          | Iberdrola    | nif | 4h |                  |
|   | en      |          | que tengan   | ied |    | cou              |
|   | M       |          | al menos un  | I   |    | nt(Contract.Type |
|   | edios** |          | contrato en  | ndi |    | Energy Sector is |
|   |         |          | Alta         | vid |    | equal to \"Gas\" |
|   |         |          |              | ual |    | AND              |
|   |         |          | Tipo de      |     |    | Contract.Status  |
|   |         |          | Sector       |     |    | is equal to      |
|   |         |          | Energía sea  |     |    | \"AL\") AND      |
|   |         |          | igual a Gas  |     |    | Unified          |
|   |         |          |              |     |    | Individual Is In |
|   |         |          | Perfil       |     |    | (Particular,     |
|   |         |          | particular o |     |    | Autonomo)        |
|   |         |          | autónomo     |     |    |                  |
+---+---------+----------+--------------+-----+----+------------------+
| 1 | **Nega  | cdp11    | Clientes de  | U   | 2  | INCLUDE RULES    |
| 5 | tivizar |          | Iberdrola    | nif | 4h |                  |
|   | en      |          | que tengan   | ied |    | cou              |
|   | M       |          | al menos un  | I   |    | nt(Contract.Type |
|   | edios** |          | contrato en  | ndi |    | Energy Sector is |
|   |         |          | Alta         | vid |    | equal to         |
|   |         |          |              | ual |    | \                |
|   |         |          | Tipo de      |     |    | "Independiente\" |
|   |         |          | Sector       |     |    | AND AND          |
|   |         |          | Energía sea  |     |    | Contract.Plan is |
|   |         |          | igual a      |     |    | equal to \"Smar  |
|   |         |          | I            |     |    | Solar\" AND      |
|   |         |          | ndependiente |     |    | Contract.Status  |
|   |         |          | y tipo de    |     |    | is equal to      |
|   |         |          | Plan igual a |     |    | \"AL\") AND      |
|   |         |          | Smart Solar  |     |    | Unified          |
|   |         |          |              |     |    | Individual Is In |
|   |         |          | Perfil       |     |    | (Particular,     |
|   |         |          | particular o |     |    | Autonomo)        |
|   |         |          | autónomo     |     |    |                  |
+---+---------+----------+--------------+-----+----+------------------+
| 1 | **Nega  | cdp12    | Clientes de  | U   | 2  | INCLUDE RULES    |
| 6 | tivizar |          | Iberdrola    | nif | 4h |                  |
|   | en      |          | que tengan   | ied |    | cou              |
|   | M       |          | al menos un  | I   |    | nt(Contract.Type |
|   | edios** |          | contrato en  | ndi |    | Energy Sector is |
|   |         |          | Alta         | vid |    | equal to         |
|   |         |          |              | ual |    | \                |
|   |         |          | Tipo de      |     |    | "Independiente\" |
|   |         |          | Sector       |     |    | AND AND          |
|   |         |          | Energía sea  |     |    | Contract.Plan is |
|   |         |          | igual a      |     |    | in (\"Smar       |
|   |         |          | I            |     |    | Mobility         |
|   |         |          | ndependiente |     |    | Hogar\", \"Smart |
|   |         |          | y Plan igual |     |    | Mobility         |
|   |         |          | a Smart      |     |    | Negocios\") AND  |
|   |         |          | Mobility     |     |    | Contract.Status  |
|   |         |          | Hogar o      |     |    | is equal to      |
|   |         |          | Smart        |     |    | \"AL\") AND      |
|   |         |          | Mobilty      |     |    | Unified          |
|   |         |          | Negocios     |     |    | Individual Is In |
|   |         |          |              |     |    | (Particular,     |
|   |         |          | Perfil       |     |    | Autonomo)        |
|   |         |          | particular o |     |    |                  |
|   |         |          | autónomo     |     |    |                  |
+---+---------+----------+--------------+-----+----+------------------+
| 1 | **Nega  | cdp14    | Clientes de  | U   | 2  | INCLUDE RULES    |
| 7 | tivizar |          | Iberdrola    | nif | 4h |                  |
|   | en      |          | que tengan   | ied |    | cou              |
|   | M       |          | al menos un  | I   |    | nt(Contract.Type |
|   | edios** |          | contrato en  | ndi |    | Energy Sector is |
|   |         |          | Alta         | vid |    | equal to         |
|   |         |          |              | ual |    | \                |
|   |         |          | Tipo de      |     |    | "Independiente\" |
|   |         |          | Sector       |     |    | AND AND          |
|   |         |          | Energía sea  |     |    | Contract.Plan is |
|   |         |          | igual a      |     |    | equal to         |
|   |         |          | Indepediente |     |    | \"Aerotermia\"   |
|   |         |          | y Plan igual |     |    | AND              |
|   |         |          | a Aerotermia |     |    | Contract.Status  |
|   |         |          |              |     |    | is equal to      |
|   |         |          | Perfil       |     |    | \"AL\") AND      |
|   |         |          | particular o |     |    | Unified          |
|   |         |          | autónomo     |     |    | Individual Is In |
|   |         |          |              |     |    | (Particular,     |
|   |         |          |              |     |    | Autonomo)        |
+---+---------+----------+--------------+-----+----+------------------+
| 1 | **Opor  | cdp13    | Clientes que | U   | O  | Opportunity      |
| 8 | tunidad |          | tengan una   | nif | ne | Solar.Name is    |
|   | Solar** |          | Oportunidad  | ied | Sh | equal to         |
|   |         |          | Solar        | I   | ot | \"solar\"        |
|   |         |          |              | ndi |    |                  |
|   |         |          |              | vid |    |                  |
|   |         |          |              | ual |    |                  |
+---+---------+----------+--------------+-----+----+------------------+
| 1 | **Cross | cdp15    | Clientes con | U   | O  | INCLUDE RULES    |
| 9 | Sell    |          | las tarifas  | nif | ne |                  |
|   | Luz**   |          | de Gas: RL1, | ied | Sh | cou              |
|   |         |          | RL2 y RL3    | I   | ot | nt(Contract.Type |
|   |         |          |              | ndi |    | Energy Sector is |
|   |         |          | que no       | vid |    | equal to         |
|   |         |          | tengan       | ual |    | \"Gas\") at      |
|   |         |          | contrato de  |     |    | least 1 AND      |
|   |         |          | luz          |     |    | count            |
|   |         |          |              |     |    | (Contract.Tariff |
|   |         |          | No           |     |    | Id is in (RL1,   |
|   |         |          | empleados,   |     |    | Rl2, RL3) at     |
|   |         |          | no vips, no  |     |    | least 1 AND      |
|   |         |          | vulnerables  |     |    | count(C          |
|   |         |          |              |     |    | ontract.Solvency |
|   |         |          | No morosos   |     |    | Cto is equal to  |
|   |         |          |              |     |    | \"RyN\") at      |
|   |         |          | Que          |     |    | least 1          |
|   |         |          | pertenezcan  |     |    |                  |
|   |         |          | al Mercado   |     |    | EXCLUDE RULES    |
|   |         |          | Residencial  |     |    |                  |
|   |         |          | y Negocio.   |     |    | Unified          |
|   |         |          |              |     |    | Individual.VIP   |
|   |         |          |              |     |    | Client is equal  |
|   |         |          |              |     |    | to 1 OR          |
|   |         |          |              |     |    | Vulnerable       |
|   |         |          |              |     |    | Client is equal  |
|   |         |          |              |     |    | to 1 OR          |
|   |         |          |              |     |    | cou              |
|   |         |          |              |     |    | nt(Contract.Past |
|   |         |          |              |     |    | Due Cliente      |
|   |         |          |              |     |    | Indicator is     |
|   |         |          |              |     |    | equal to 1) at   |
|   |         |          |              |     |    | least 1 OR       |
|   |         |          |              |     |    | Unified          |
|   |         |          |              |     |    | Indi             |
|   |         |          |              |     |    | vidual.Iberdrola |
|   |         |          |              |     |    | Employee is      |
|   |         |          |              |     |    | equal to 1 OR    |
|   |         |          |              |     |    | cou              |
|   |         |          |              |     |    | nt(Contract.Type |
|   |         |          |              |     |    | Energy Sector is |
|   |         |          |              |     |    | equal to         |
|   |         |          |              |     |    | \                |
|   |         |          |              |     |    | "Electricidad\") |
|   |         |          |              |     |    | at least 1       |
+---+---------+----------+--------------+-----+----+------------------+
| 2 | **C     | cdp16    | Clientes con | U   | 2  | cou              |
| 0 | lientes |          | Contrato Luz | nif | 4h | nt(Contract.Type |
|   | Identif |          |              | ied |    | Energy Sector is |
|   | icables |          |              | I   |    | equal to         |
|   | Luz -   |          |              | ndi |    | \                |
|   | IS**    |          |              | vid |    | "Electricidad\") |
|   |         |          |              | ual |    | at least 1       |
+---+---------+----------+--------------+-----+----+------------------+
| 2 | **Ad    | A        | Clientes     | U   | 2  | Unified          |
| 1 | heridos | dheridos | Adheridos a  | nif | 4h | Individual.Mi    |
|   | a MI**  | a Mi     | Mi Iberdrola | ied |    | Iberdrola        |
|   |         | I        |              | I   |    | Indicator is     |
|   |         | berdrola |              | ndi |    | equal to 1       |
|   |         |          |              | vid |    |                  |
|   |         |          |              | ual |    |                  |
+---+---------+----------+--------------+-----+----+------------------+

Documento donde se encuentra desarrollado cada caso de uso y segmento
asociado: [[IBD - CDP Segmentos Casos de
uso/audiencias]{.underline}](https://docs.google.com/spreadsheets/d/1ntsM6_M8azqG6jeefpFau6OL8sziKQgyw-UJgZqbmkw/edit#gid=0)

## Activaciones

Previamente a la publicación de un segmento se debe identificar el
destino de activación o activation target, es decir, el lugar donde se
quiere utilizar dicho segmento.

### Marketing Cloud

Data Cloud permite publicar directamente los segmentos a los business
units de **Salesforce Marketing Cloud**, para esto debemos seleccionar
el destino de activación Marketing Cloud. Luego introduciremos un nombre
exclusivo para el destino de activación. Por último, [podemos agregar o
eliminar unidades comerciales (BU) para recibir los segmentos
publicados.]{.mark}

[Los nombres de destino de activación de Marketing Cloud no pueden tener
más de 128 caracteres, comenzar con un guión bajo, ser todo números o
incluir estos caracteres: @ %\^ = \< \' \* + \# \$ / \\\\ ! ? ( ) { } \[
\] , . \\ \\]{.mark}

[Después de crear y activar segmentos en Marketing Cloud, aparecerán en
Contact Builder. Una subcarpeta Segmentos de Data Cloud se crea cuando
se publica la primera activación.]{.mark}

[Después de crear un segmento en Data Cloud, se puede publicar un
segmento en un destino de activación.]{.mark}

-   [En Configuración de Data Cloud, se debe asegurar de que **Permitir
    datos de asignación de unidades comerciales de perfil** está
    activada.]{.mark}

-   [Los segmentos se filtran por la unidad comercial (BU).]{.mark}

[Pasos para crear una activación de Marketing Cloud para un
segmento:]{.mark}

1.  [Seleccionar segmento]{.mark}

2.  [Seleccionar destino de activación]{.mark}

3.  [Seleccionar un DMO desde Pertenencia de Activación]{.mark}

4.  [Seleccionar y configurar puntos de contacto]{.mark}

5.  [Añadir atributos adicionales si es necesario]{.mark}

En el caso de **Marketing Cloud**, el orden de prioridad de origen es el
de cualquier data source como vemos en la siguiente imagen de
configuración de la activación.

![](media/image21.png){width="6.267716535433071in" height="4.25in"}

Los nombres de los campos a enviar en la activación de Marketing Cloud
son los que se ven en la imagen también:

1.  Unified Individual Id. Este es el que cogerá como subscriberKey para
    MKTC, pero no hace falta fijar un nombre en el Preferred Attribute
    Name

2.  customerId

3.  FirstName

4.  emailAddress

![](media/image16.png){width="6.267716535433071in"
height="2.3333333333333335in"}

### Interaction Studio

Al conectar una cuenta de **Interaction Studio** con Data Cloud se crea
automáticamente un destino de activación para cada conjunto de datos o
data set.

Cabe destacar que durante la selección y configuración de puntos de
contacto o contact points, podremos indicar el **o[rden de prioridad de
origen]{.mark}** [o source priority order, este se utiliza para
determinar qué valor de punto de contacto se selecciona para una
activación cuando hay varios valores disponibles. El orden de prioridad
de origen se utilizará para reordenar qué valor de punto de contacto se
selecciona para miembros de segmento con datos desde múltiples orígenes.
Se puede cambiar la prioridad agregando, reordenando o eliminando
orígenes.]{.mark}

Para **Interaction Studio**, el orden de prioridad de origen
predeterminado es el data source de SFTP a continuación el de
Interaction Studio y por último cualquier data source.

![](media/image12.png){width="4.838542213473316in"
height="2.9212128171478566in"}

Además los nombres de los campos a enviar en la activación de
Interaction Studio deben tener un nombre determinado que indicamos a
continuación:

![](media/image22.png){width="6.267716535433071in"
height="2.7777777777777777in"}

Los nombres de los atributos de envío (Preferred Attribute Name) son:

1.  Unified Individual Id

2.  customerId

3.  Aerotermia Savings. Este atributo es para el caso de uso de
    aerotermia, importante destacar que si no se indica un Preferred
    Attribute Name por defecto envía el atributo como Aerotermia_Savings
    (Debe estar definido así en IS)

4.  profileId

5.  emailAddress

Los nombres de los atributos en IS son:

1.  name

2.  customerId \*

3.  dni

4.  emailAddress \*

5.  lastname

6.  name

7.  phone

8.  profileId \*

9.  sfmcContactKey

\*Es importante enviar el máximo número de atributos de identidad a
Interaction Studio ya que en dicha herramienta va a intentar matchear
con sus usuarios, por eso en estas activaciones se mandan el customerId
(Cod_Cliente de Iberdrola), emailAddress y profileID (Cookie de IS).

Por último estas audiencias se pueden escribir en tablas del cdp creando
un activation target que sea Salesforce Data Cloud. Con esta opción
podremos ejecutar consultas SOQL sobre dichas tablas para generar
Insights sobre los datos de las audiencias o exportarlos a sistemas
externos como SAS con un conector JDBC.

### Meta, Amazon Ads y Google Ads

Para publicar segmentos en una cuenta de plataforma externa, se debe
crear un objetivo de activación. Una plataforma externa puede tener
varios objetivos de activación, pero solo se puede adjuntar un objetivo
de activación a una cuenta de plataforma externa.

Solo un administrador o un gerente de marketing pueden crear un objetivo
de activación (activation target) para las plataformas de Google Ads,
Meta y Amazon Ads.

Pasos para crear objetivos de activación:

1.  En la pestaña Objetivos de activación, haz clic en Nuevo.

2.  Selecciona una plataforma de activación externa y haz clic en
    Siguiente.

3.  Ingresa un nombre y una descripción opcional.

4.  Si el tipo de privacidad no está establecido, actualiza la
    plataforma de activación externa para incluir un tipo de privacidad.

5.  Agrega los detalles de la cuenta y el inicio de sesión de la
    plataforma, y haz clic en Siguiente.

6.  Si estás utilizando la plataforma de Google Ads, LinkedIn, Meta o
    Amazon Ads, serás redirigido a su sitio para proporcionar los
    detalles de inicio de sesión.

    a.  Concede permisos de acceso a Data Cloud y acepta los términos de
        servicio.Serás redirigido a Data Cloud y se te proporcionarán
        las cuentas a las que tienes permiso para cargar audiencias.

    b.  Selecciona el espacio de datos con el que deseas asociar tu
        objetivo de activación.

    c.  Selecciona el nombre y el ID de la cuenta.El espacio de datos
        seleccionado determina qué IDs de cuenta están disponibles para
        crear una combinación única para un solo uso.

    d.  Para Amazon Ads, si tienes acceso tanto a una cuenta de Amazon
        DSP como a una instancia de AMC, se te pedirá que selecciones
        una cuenta para cada una.

7.  Si seleccionas tanto DSP como AMC para un nuevo objetivo de
    activación, asegúrate de que tus cuentas estén vinculadas en Amazon
    Ads. Luego, podrás ver los datos de rendimiento de la cuenta DSP en
    la instancia de AMC.

8.  Si estás utilizando una plataforma instalada desde AppExchange,
    ingresa tu ID de cuenta.

9.  Cada ID de cuenta es único y sólo puedes usarlo para un objetivo de
    activación.

10. Selecciona el espacio de datos con el que deseas asociar tu objetivo
    de activación.

11. Haz clic en Guardar.

Luego de crear los objetivos de activación ya se pueden utilizar para
enviar audiencias a estas plataformas.

# Data Space de Integración

El Data Space de integración se utilizará para realizar pruebas entre
las 3 herramientas de Salesforce utilizadas en este proyecto
(Interaction Studio, Marketing Cloud y Data Cloud). Consiste en replicar
el Data Space de producción (default), es decir, recrear todos los data
streams, data mappings y relaciones previamente definidos. Esto
permitirá hacer pruebas de los casos de uso entre las 3 instancias de
integración para posteriormente llevarlos a cabo en las instancias de
producción.

Para crear un nuevo Data Space se deben seguir los siguientes pasos:

1.  Ir a Configuración de Data Cloud.

2.  Bajo Gestión de datos, hacer clic en Espacios de datos.

3.  Hacer clic en Nuevo y asignar al espacio de datos un nombre
    exclusivo que identifique marca, región o departamento.

4.  Introducir un prefijo de espacio de datos exclusivo, comenzando por
    una letra e incluyendo hasta tres caracteres alfanuméricos.\
    Después de guardar el espacio de datos, no se puede cambiar su
    prefijo porque se convierte en parte del nombre de API utilizado
    para diferenciar objetos que existen en múltiples espacios de
    datos.\
    Por ejemplo, el DMO Particular está dividido en dos espacios de
    datos: espacio de datos 1 con prefijo DS1 y espacio de datos 2 con
    prefijo DS2. Cuando el DMO Particular se asigna a los espacios de
    datos, los nombres de API de los objetos son DS1_Individual_dlm y
    DS2_Individual_dlm.

5.  Agregar una descripción opcional acerca del propósito del espacio de
    datos.

6.  Hacer clic en Guardar.

![](media/image7.png){width="3.6302088801399823in"
height="1.9032163167104112in"}

![](media/image5.png){width="6.267716535433071in"
height="0.5972222222222222in"}

Data Space de producción: default

Data Space de integración: int_iberdrola

# Usuarios y Roles

Para crear nuevos usuarios de acuerdo al tipo perfil que se vaya a crear
hay que asignar los siguientes Profiles y Permission Sets:

+---------+----------------------+---------------+--------------------+
| **P     | **Profile Data       | **Permission  | **Funciones en     |
| erfil** | Cloud**              | Set Data      | Data Cloud**       |
|         |                      | Cloud**       |                    |
+=========+======================+===============+====================+
| Data    | Data Cloud Functions | Data Cloud    | Data Space         |
| Expert  | Only User (Custom)   | Admin         | Management         |
|         |                      |               |                    |
|         |                      | Data Cloud    | Data Space Data    |
|         |                      | Marketing     | Addition           |
|         |                      | Admin         |                    |
|         |                      |               | Data Streams       |
|         |                      | default       |                    |
|         |                      |               | Data Shares        |
|         |                      |               |                    |
|         |                      |               | Data Lake Objects  |
|         |                      |               |                    |
|         |                      |               | Data Transforms    |
|         |                      |               |                    |
|         |                      |               | Data Model         |
|         |                      |               |                    |
|         |                      |               | Data Graphs        |
|         |                      |               |                    |
|         |                      |               | Identity           |
|         |                      |               | Resolution         |
|         |                      |               |                    |
|         |                      |               | Data Explorer      |
|         |                      |               |                    |
|         |                      |               | Profile Explorer   |
|         |                      |               |                    |
|         |                      |               | Calculated         |
|         |                      |               | Insights           |
|         |                      |               |                    |
|         |                      |               | Einstein Studio AI |
|         |                      |               | Models             |
|         |                      |               |                    |
|         |                      |               | Data Action and    |
|         |                      |               | Data Action        |
|         |                      |               | Targets            |
|         |                      |               |                    |
|         |                      |               | Segments           |
|         |                      |               |                    |
|         |                      |               | Activation and     |
|         |                      |               | Activation Targets |
+---------+----------------------+---------------+--------------------+
| Use     | Data Cloud Functions | Data Cloud    | Segments           |
| Case    | Only User (Custom)   | Marketing     |                    |
| Fun     |                      | Manager       | Activations        |
| ctional |                      |               |                    |
|         |                      | default       | Activation Targets |
+---------+----------------------+---------------+--------------------+
| Use     | Data Cloud Functions | Data Cloud    | Segments           |
| Case    | Only User (Custom)   | Marketing     |                    |
| Tec     |                      | Manager       | Activations        |
| hnician |                      |               |                    |
|         |                      | default       | Activation Targets |
+---------+----------------------+---------------+--------------------+
| Content | Data Cloud Functions | Data Cloud    | Segments           |
| Creator | Only User (Custom)   | Marketing     |                    |
|         |                      | Specialist    |                    |
|         |                      |               |                    |
|         |                      | default       |                    |
+---------+----------------------+---------------+--------------------+
| Admini  | System Administrator | Data Cloud    | Data Cloud Set Up  |
| strator |                      | Admin         |                    |
|         |                      |               | Data Space         |
|         |                      | Data Cloud    | Management         |
|         |                      | Marketing     |                    |
|         |                      | Admin         | Data Space Data    |
|         |                      |               | Addition           |
|         |                      | default       |                    |
|         |                      |               | Data Streams       |
|         |                      |               |                    |
|         |                      |               | Data Shares        |
|         |                      |               |                    |
|         |                      |               | Data Lake Objects  |
|         |                      |               |                    |
|         |                      |               | Data Transforms    |
|         |                      |               |                    |
|         |                      |               | Data Model         |
|         |                      |               |                    |
|         |                      |               | Data Graphs        |
|         |                      |               |                    |
|         |                      |               | Identity           |
|         |                      |               | Resolution         |
|         |                      |               |                    |
|         |                      |               | Data Explorer      |
|         |                      |               |                    |
|         |                      |               | Profile Explorer   |
|         |                      |               |                    |
|         |                      |               | Calculated         |
|         |                      |               | Insights           |
|         |                      |               |                    |
|         |                      |               | Einstein Studio AI |
|         |                      |               | Models             |
|         |                      |               |                    |
|         |                      |               | Data Action and    |
|         |                      |               | Data Action        |
|         |                      |               | Targets            |
|         |                      |               |                    |
|         |                      |               | Segments           |
|         |                      |               |                    |
|         |                      |               | Activation and     |
|         |                      |               | Activation Targets |
+---------+----------------------+---------------+--------------------+

Hay que tomar en cuenta que al crear un nuevo Data Space habría que
agregarlo a los permission sets del user.

# Naming Convention

Todos los nombres que utilizaremos dentro de Data Cloud deberán ser en
inglés por seguir la forma de Salesforce, aunque los campos de entrada
en los Data Stream se mantendrán con el nombre de cada sistema origen
por mantener la trazabilidad.

En la siguiente tabla se indican el nombre a seguir para cada entidad:

+-----------------+---------------------+-----------------------------+
| **Entidad**     | **Formato Naming**  | **Ejemplo**                 |
+=================+=====================+=============================+
| **File name     | Determinados por    | Cliente_20230101180000.csv  |
| Data Stream**   | los acuerdos de     |                             |
|                 | interfaz            |                             |
+-----------------+---------------------+-----------------------------+
| **Source        | Empieza por         | Delta_Sidat_SAS_SFTP        |
| Details Data    | mayúsculas y se     | (Cliente, Contrato,         |
| Stream**        | separa por \'\_\'.  | Servicio, Consentimientos)  |
|                 | Los nombres deben   |                             |
|                 | indicar desde donde | Delio_Sidat_SAS_SFTP        |
|                 | viaja el dato, por  | (Delio)                     |
|                 | ejemplo             |                             |
|                 | D                   | CATS_Sidat_SFTP (CATs)      |
|                 | elio_Sidat_SAS_SFTP |                             |
|                 |                     | MKTC e IS se generan Auto   |
+-----------------+---------------------+-----------------------------+
| **Data Lake     | Empieza por         | Customer                    |
| Object Name**   | mayúsculas y se     |                             |
|                 | separa por espacios | Contract                    |
|                 |                     |                             |
|                 |                     | Service                     |
|                 |                     |                             |
|                 |                     | Cats Events                 |
|                 |                     |                             |
|                 |                     | Delio Events                |
|                 |                     |                             |
|                 |                     | Consents                    |
+-----------------+---------------------+-----------------------------+
| **Name Data     | Empieza por         | Customer_20230614           |
| Stream**        | mayúsculas y se     |                             |
|                 | separa por \'\_\'   | Cats_Events_20230614        |
|                 |                     |                             |
|                 |                     | Lead_Profiles_20230616      |
|                 |                     |                             |
|                 |                     | Lead_Engagements_20230616   |
+-----------------+---------------------+-----------------------------+
| **Data Model    | Empieza por         | Delio Engagement            |
| Object Label**  | mayúsculas y se     |                             |
|                 | separa por espacios | Contract                    |
|                 |                     |                             |
|                 | Para las            |                             |
|                 | interacciones       |                             |
|                 | utilizar la palabra |                             |
|                 | Engagement          |                             |
+-----------------+---------------------+-----------------------------+
| **Data Model    | Empieza por         | Phone Number                |
| Attributes      | mayúsculas y se     |                             |
| Label**         | separa por espacios |                             |
+-----------------+---------------------+-----------------------------+
| **Ruleset Id**  | Todo minusculas,    | id01                        |
|                 | dos letras para     |                             |
|                 | identificar el      | ac01                        |
|                 | objeto sobre el que |                             |
|                 | hacer la regla y    |                             |
|                 | dos números         |                             |
|                 | secuenciales        |                             |
+-----------------+---------------------+-----------------------------+
| **Ruleset       | Empieza por         | Phone Formatted             |
| Name**          | mayúsculas y se     |                             |
|                 | separa por          |                             |
|                 | espacios. La        |                             |
|                 | descripción va en   |                             |
|                 | otro inbox así que  |                             |
|                 | tiene que ser       |                             |
|                 | autodescriptivo     |                             |
+-----------------+---------------------+-----------------------------+
| **Segment       | Empieza por         |                             |
| Name**          | mayúsculas y se     |                             |
|                 | separa por espacios |                             |
+-----------------+---------------------+-----------------------------+
| **Activation    | Empieza por         |                             |
| Targets Name**  | mayúsculas y se     |                             |
|                 | separa por espacios |                             |
+-----------------+---------------------+-----------------------------+
| **Customer Data | Empieza por         | Segment Electric Power      |
| Platform Object | mayúsculas y se     | Customer                    |
| Name para las   | separa por espacios |                             |
| activaciones    | y debe tener un     |                             |
| que se generan  | prefijo Segment     |                             |
| en tablas de    |                     |                             |
| Data Cloud**    |                     |                             |
+-----------------+---------------------+-----------------------------+
| **Para Testing  | Añadir a todos los  | TEST - Customer             |
| y               | nombres TEST - o    |                             |
| validaciones**  | VALIDATION - por    | TEST - Contract             |
|                 | delante             |                             |
|                 |                     | \...                        |
|                 | Si no se pueden     |                             |
|                 | añadir guiones      |                             |
|                 | pongamos TEST\_ y   |                             |
|                 | si no TEST          |                             |
+-----------------+---------------------+-----------------------------+

# Pruebas de carga

De cara a diseñar los horarios de carga de ficheros y borrado de los
mismos en el SFTP se han realizado pruebas de carga en el CDP de la
siguiente forma:

1.  Carga (Primera carga Upsert) de 1M de registros de clientes con una
    estructura similar a la que recibirá el CDP desde SAS según el
    acuerdo de interfaz. Tiempo de procesamiento: 7min

2.  Carga (Full Refresh) de 1M de registros de clientes con una
    estructura similar a la que recibirá el CDP desde SAS según el
    acuerdo de interfaz. Tiempo de procesamiento: 14min

3.  Carga (Primera carga Upsert) de 8M de registros de clientes con una
    estructura similar a la que recibirá el CDP desde SAS según el
    acuerdo de interfaz. Tiempo de procesamiento: 22min

# Borrado de datos en CDP

-   Borraremos clientes, servicios y consentimientos

-   Delio se va acumulando mediante ficheros incrementales

-   Contrato se hará full refresh

# Glosario de términos

CDP: Customer Data Platform

DSO: Data Stream Object

DLO: Data Lake Object

PK: Primary Key
