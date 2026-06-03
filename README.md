Objetivo General

Desarrollar una aplicación web utilizando Spring Boot y Thymeleaf que permita la gestión de pedidos de una empresa de repostería de manera eficiente, facilitando el control de productos, usuarios, pagos y reportes mediante una plataforma moderna y segura.

Objetivos Específicos

1.	Identificar las necesidades del negocio para la definición de los requerimientos del sistema.
2.	Diseñar una base de datos en MySQL que permita el almacenamiento de la información de usuarios, productos y pedidos. 
3.	Crear un sistema web por medio de Spring Tools que permita al administrador el registro de los pedidos, así como su estado, y el pago.
4.	Implementar una plataforma que sea responsive y compatible con dispositivos móviles y computadores.
5.	Generar reportes administrativos que permitan el control de ventas y pedidos.


Justificación

El desarrollo de una plataforma web para la gestión de pedidos en una empresa de repostería surge de la necesidad de optimizar los procesos de registro, seguimiento y administración de pedidos, los cuales generalmente se realizan de manera manual, generando posibles errores, pérdida de información y retrasos en la atención.
La implementación de este sistema permitirá al administrador registrar pedidos de manera organizada, consultar el historial de solicitudes, actualizar el estado de los pedidos y visualizar reportes relacionados con las ventas y el control de productos. Esto contribuirá a mejorar la eficiencia operativa del negocio, reducir errores en el manejo de la información y facilitar el acceso a los datos en tiempo real.
Asimismo, la plataforma busca fortalecer la gestión administrativa de la empresa mediante la centralización de la información, permitiendo un mayor control sobre los pedidos realizados, los métodos de pago y el estado de entrega de los productos, favoreciendo así una mejor organización interna y una toma de decisiones más eficiente.


Entorno de Trabajo

<img width="908" height="433" alt="Captura de pantalla 2026-06-02 090406" src="https://github.com/user-attachments/assets/9b427c79-9710-4f95-a3f9-942ed9bc2aeb" />

Requerimientos Funcionales

<img width="904" height="646" alt="Captura de pantalla 2026-06-02 090549" src="https://github.com/user-attachments/assets/8629a1e2-15d2-45a8-8c84-bcf456afaf98" />

<img width="911" height="410" alt="image" src="https://github.com/user-attachments/assets/bd969efd-8476-413b-b0c0-302885e5781d" />



Requerimientos No Funcionales

<img width="910" height="507" alt="image" src="https://github.com/user-attachments/assets/f719c6de-052d-4864-a855-93d26436f53c" />



Diagramas del Sistema

Diagramas de Casos de Uso

Actores principales:

• Cliente (actor indirecto)

• Administrador (actor principal)


Casos de uso:

Administrador

• Iniciar sesión

• Gestionar productos

• Gestionar categorías

• Registrar pedidos de clientes

• Gestionar estados de pedidos

• Registrar pagos

• Consultar historial de pedidos

• Generar reportes administrativos

• Consultar ventas realizadas


Cliente (participación indirecta)

• Solicitar productos

• Proporcionar información para el pedido

• Elegir método de pago

CODIGO 

CREATE TABLE IF NOT EXISTS usuarios (
    id_usuario INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    telefono VARCHAR(20),
    direccion VARCHAR(150),
    contrasena VARCHAR(255) NOT NULL,
    rol ENUM('cliente', 'administrador') DEFAULT 'cliente'
);

CREATE TABLE IF NOT EXISTS categorias (
    id_categoria INT AUTO_INCREMENT PRIMARY KEY,
    nombre_categoria VARCHAR(100) NOT NULL,
    descripcion TEXT
);

CREATE TABLE IF NOT EXISTS productos (
    id_producto INT AUTO_INCREMENT PRIMARY KEY,
    id_categoria INT,
    nombre VARCHAR(100) NOT NULL,
    descripcion TEXT,
    precio DECIMAL(10,2) NOT NULL,

    FOREIGN KEY (id_categoria)
    REFERENCES categorias(id_categoria)
);


CREATE TABLE IF NOT EXISTS pedidos (
    id_pedido INT AUTO_INCREMENT PRIMARY KEY,

    cliente VARCHAR(100) NOT NULL,
    telefono VARCHAR(20),

    id_producto INT NOT NULL,
    cantidad INT NOT NULL,

    observaciones TEXT,
    metodo_pago VARCHAR(50),

    total DECIMAL(10,2),

    estado ENUM(
        'tomado',
        'en proceso',
        'entregado',
        'cancelado'
    ) DEFAULT 'tomado',

    fecha_pedido DATETIME DEFAULT CURRENT_TIMESTAMP,

    FOREIGN KEY (id_producto)
    REFERENCES productos(id_producto)
);

CREATE TABLE IF NOT EXISTS detalle_pedidos (
    id_detalle INT AUTO_INCREMENT PRIMARY KEY,
    id_pedido INT,
    id_producto INT,
    cantidad INT NOT NULL,
    subtotal DECIMAL(10,2),

    FOREIGN KEY (id_pedido)
    REFERENCES pedidos(id_pedido),

    FOREIGN KEY (id_producto)
    REFERENCES productos(id_producto)
);

CREATE TABLE IF NOT EXISTS pagos (
    id_pago INT AUTO_INCREMENT PRIMARY KEY,
    id_pedido INT UNIQUE,
    metodo_pago VARCHAR(50),
    fecha_pago DATETIME DEFAULT CURRENT_TIMESTAMP,

    estado_pago ENUM('pendiente', 'pagado', 'cancelado') DEFAULT 'pendiente',

    FOREIGN KEY (id_pedido)
    REFERENCES pedidos(id_pedido)
);

CREATE TABLE IF NOT EXISTS calificaciones (
    id_calificacion INT AUTO_INCREMENT PRIMARY KEY,
    id_usuario INT,
    id_producto INT,

    puntuacion INT CHECK (puntuacion BETWEEN 1 AND 5),
    comentario TEXT,

    fecha_calificacion DATETIME DEFAULT CURRENT_TIMESTAMP,

    FOREIGN KEY (id_usuario)
    REFERENCES usuarios(id_usuario),

    FOREIGN KEY (id_producto)
    REFERENCES productos(id_producto)
);

Diagrama entidad relación 
<img width="921" height="614" alt="image" src="https://github.com/user-attachments/assets/a855dd94-1448-4e5c-ade7-c0e3d4538612" />

