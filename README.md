# **Proyecto de Análisis de Datos de Recursos Humanos**

Este repositorio contiene la estructura de la base de datos diseñada para un proyecto de análisis de datos de Recursos Humanos. El objetivo principal es comprender y mejorar la rotación y retención de empleados, analizando aspectos clave como la demografía, el rendimiento, la satisfacción, la compensación y los beneficios.

La estructura se ha optimizado para un máximo de 7 tablas, consolidando la información de manera eficiente para el análisis.

## **Estructura Simplificada de la Base de Datos de Recursos Humanos (Máximo 7 Tablas)**

Este diseño consolida la información para un análisis efectivo de RRHH, enfocándose en la rotación y retención de empleados, su demografía, rendimiento, satisfacción, compensación y beneficios, limitando el número de tablas a siete.

### **Tablas Propuestas y sus Columnas:**

#### **1\. Empleados**

Esta tabla central contiene la información demográfica, de empleo básica y de compensación actual de cada persona.

* empleado\_id (INT, PRIMARY KEY): Identificador único del empleado.  
* nombre (VARCHAR(100)): Nombre del empleado.  
* apellido (VARCHAR(100)): Apellido del empleado.  
* fecha\_nacimiento (DATE): Fecha de nacimiento del empleado.  
* genero (VARCHAR(10)): Género del empleado (e.g., 'Masculino', 'Femenino', 'No binario').  
* estado\_civil (VARCHAR(20)): Estado civil del empleado (e.g., 'Soltero', 'Casado', 'Divorciado').  
* nacionalidad (VARCHAR(50)): Nacionalidad del empleado.  
* direccion (VARCHAR(255)): Dirección de residencia del empleado.  
* telefono (VARCHAR(20)): Número de teléfono de contacto.  
* email (VARCHAR(100) UNIQUE): Correo electrónico corporativo del empleado.  
* fecha\_contratacion (DATE): Fecha en que el empleado se unió a la empresa.  
* tipo\_contrato (VARCHAR(50)): Tipo de contrato (e.g., 'Indefinido', 'Temporal', 'Servicios profesionales').  
* salario\_actual (DECIMAL(10, 2)): Cantidad del salario actual.  
* moneda\_salario (VARCHAR(5)): Moneda del salario actual (e.g., 'USD', 'EUR', 'PEN').  
* departamento\_id (INT, FOREIGN KEY): Referencia al departamento al que pertenece el empleado.  
* puesto\_id (INT, FOREIGN KEY): Referencia al puesto que ocupa el empleado.  
* supervisor\_id (INT, FOREIGN KEY, NULLABLE): Identificador del supervisor directo del empleado (puede ser NULL si no tiene).  
* activo (BOOLEAN): Indica si el empleado está actualmente activo en la empresa (útil para rotación).  
* fecha\_fin\_empleo (DATE, NULLABLE): Fecha en que el empleado dejó la empresa (NULL si activo).  
* razon\_salida (VARCHAR(100), NULLABLE): Razón principal de salida si el empleado ya no está activo.

#### **2\. Departamentos**

Almacena la información de los diferentes departamentos de la organización.

* departamento\_id (INT, PRIMARY KEY): Identificador único del departamento.  
* nombre\_departamento (VARCHAR(100) UNIQUE): Nombre del departamento (e.g., 'Ventas', 'Marketing', 'RRHH', 'TI').  
* ubicacion (VARCHAR(100)): Ubicación física del departamento.  
* descripcion (TEXT): Breve descripción del departamento.

#### **3\. Puestos**

Define los diferentes puestos de trabajo dentro de la empresa.

* puesto\_id (INT, PRIMARY KEY): Identificador único del puesto.  
* nombre\_puesto (VARCHAR(100) UNIQUE): Nombre del puesto (e.g., 'Analista de Datos', 'Gerente de Proyectos', 'Desarrollador Senior').  
* nivel\_puesto (VARCHAR(50)): Nivel jerárquico del puesto (e.g., 'Junior', 'Mid', 'Senior', 'Gerencia').  
* descripcion\_puesto (TEXT): Descripción detallada de las responsabilidades del puesto.

#### **4\. Beneficios**

Define los tipos de beneficios que ofrece la empresa.

* beneficio\_id (INT, PRIMARY KEY): Identificador único del beneficio.  
* nombre\_beneficio (VARCHAR(100) UNIQUE): Nombre del beneficio (e.g., 'Seguro de Salud', 'Plan de Pensiones', 'Vales de Comida').  
* descripcion\_beneficio (TEXT): Descripción del beneficio.  
* costo\_empresa (DECIMAL(10, 2)): Costo promedio del beneficio para la empresa.

#### **5\. Empleado\_Beneficios (Tabla de Relación Muchos a Muchos)**

Vincula a los empleados con los beneficios que reciben.

* empleado\_id (INT, FOREIGN KEY): Referencia al empleado.  
* beneficio\_id (INT, FOREIGN KEY): Referencia al beneficio.  
* fecha\_inscripcion (DATE): Fecha en que el empleado se inscribió en el beneficio.  
* estado\_beneficio (VARCHAR(50)): Estado actual del beneficio para el empleado (e.g., 'Activo', 'Inactivo', 'Pendiente').  
* PRIMARY KEY (empleado\_id, beneficio\_id)

#### **6\. Evaluaciones**

Combina las evaluaciones de rendimiento y los resultados de encuestas de satisfacción.

* evaluacion\_id (INT, PRIMARY KEY): Identificador único de la evaluación/encuesta.  
* empleado\_id (INT, FOREIGN KEY): Referencia al empleado evaluado.  
* fecha\_evaluacion (DATE): Fecha en que se realizó la evaluación o encuesta.  
* tipo\_evaluacion (VARCHAR(50)): Tipo de evaluación (e.g., 'Rendimiento Anual', 'Encuesta Satisfacción Q1', 'Evaluación 360').  
* puntuacion\_general (DECIMAL(4, 2), NULLABLE): Puntuación numérica general (puede ser de rendimiento o satisfacción).  
* puntuacion\_clima\_laboral (INT, NULLABLE): Puntuación específica de la encuesta de satisfacción.  
* puntuacion\_equilibrio (INT, NULLABLE): Puntuación específica de la encuesta de satisfacción.  
* puntuacion\_compensacion (INT, NULLABLE): Puntuación específica de la encuesta de satisfacción.  
* nps\_empleado (INT, NULLABLE): Net Promoter Score de empleado.  
* objetivos\_cumplidos (INT, NULLABLE): Para evaluaciones de rendimiento.  
* objetivos\_totales (INT, NULLABLE): Para evaluaciones de rendimiento.  
* comentarios (TEXT, NULLABLE): Comentarios cualitativos de la evaluación o encuesta.

#### **7\. Historial\_Laboral**

Registra los cambios de puesto o departamento de un empleado, incluyendo las razones de salida si no se registra en la tabla Empleados. Esta tabla es crucial para rastrear la trayectoria del empleado y los puntos de inflexión que podrían llevar a la rotación.

* historial\_id (INT, PRIMARY KEY): Identificador único del registro de historial.  
* empleado\_id (INT, FOREIGN KEY): Referencia al empleado.  
* fecha\_inicio\_periodo (DATE): Fecha de inicio de este periodo en el historial.  
* fecha\_fin\_periodo (DATE, NULLABLE): Fecha de fin de este periodo (NULL si es el puesto/departamento actual).  
* departamento\_id (INT, FOREIGN KEY, NULLABLE): Departamento asociado a este periodo.  
* puesto\_id (INT, FOREIGN KEY, NULLABLE): Puesto asociado a este periodo.  
* razon\_cambio (VARCHAR(100)): Razón del cambio (e.g., 'Promoción', 'Transferencia', 'Reestructuración', 'Renuncia', 'Despido', 'Cambio salarial'). Este campo puede incluir cambios salariales significativos para trazar el historial de compensación.  
* detalles\_cambio (TEXT, NULLABLE): Descripción más detallada del evento.

![graf_RRHH](https://github.com/user-attachments/assets/d5899f71-0c1c-4025-9b8e-38e14cd7d864)
