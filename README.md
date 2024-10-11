# Parcial-2_Sistemas-Expertos_USPG
Este proyecto es un sistema de banca en línea con un chatbot basado en inteligencia artificial (OpenAI) que permite realizar consultas de saldo, transferencias y gestión de cuentas. Desarrollado con Node.js y MySQL, ofrece un sistema eficiente y seguro para operaciones bancarias en tiempo real.

Codigó HTML:
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <title>Banca en Línea - Sistema Experto</title>
    <link rel="stylesheet" href="style.css"> <!-- Vinculación del archivo CSS -->
</head>
<body>
    <header>
        <nav class="navbar">
            <div class="logo">
                <h1>BAM Online</h1>
            </div>
            <ul class="nav-links">
                <li><a href="#">Inicio</a></li>
                <li><a href="#">Cuentas</a></li>
                <li><a href="#">Transacciones</a></li>
                <li><a href="#">Préstamos</a></li>
                <li><a href="#">Contacto</a></li>
                <li><a href="#" class="btn">Cerrar Sesión</a></li>
            </ul>
        </nav>
    </header>

    <main>
        <section class="banner">
            <h2>Bienvenido a la Banca en Línea</h2>
            <p>Accede a tus cuentas, realiza transacciones y más desde la comodidad de tu hogar.</p>
        </section>

        <section class="account-section">
            <h2>Tus Cuentas</h2>
            <div class="account-info">
                <div class="account-card">
                    <h3>Cuenta de Ahorros</h3>
                    <p>Saldo: <span class="saldo">$10,000.00</span></p>
                    <button class="btn">Ver Detalles</button>
                </div>
                <div class="account-card">
                    <h3>Cuenta Corriente</h3>
                    <p>Saldo: <span class="saldo">$5,500.00</span></p>
                    <button class="btn">Ver Detalles</button>
                </div>
            </div>
        </section>

        <section class="actions-section">
            <h2>Acciones Rápidas</h2>
            <div class="actions-grid">
                <div class="action-card">
                    <h3>Transferencia</h3>
                    <button class="btn">Iniciar</button>
                </div>
                <div class="action-card">
                    <h3>Pagar Servicios</h3>
                    <button class="btn">Iniciar</button>
                </div>
                <div class="action-card">
                    <h3>Solicitar Préstamo</h3>
                    <button class="btn">Iniciar</button>
                </div>
                <div class="action-card">
                    <h3>Historial de Transacciones</h3>
                    <button class="btn">Ver</button>
                </div>
            </div>
        </section>
    </main>

    <!-- Chatbot UI -->
    <div class="chatbot-container">
        <div class="chatbot-header">Asistente Virtual</div>
        <div class="chatbot-body" id="chatbot-body">
            <!-- Aquí se agregarán los mensajes del usuario y del bot -->
        </div>
        <div class="chatbot-input">
            <input type="text" id="chatbot-input" placeholder="Escribe tu pregunta...">
            <button onclick="enviarMensaje()">Enviar</button>
        </div>
    </div>

    <footer>
        <p>&copy; BAM Online - Todos los derechos reservados</p>
    </footer>

    <script src="script.js"></script> <!-- Vinculación del archivo JavaScript -->
</body>
</html>

Código CSS:

/* Estilos para la barra de navegación */
.navbar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    background-color: #0077cc;
    padding: 10px 20px;
}

.logo h1 {
    color: white;
    font-size: 24px;
}

.nav-links {
    list-style: none;
    display: flex;
    gap: 20px;
}

.nav-links a {
    color: white;
    text-decoration: none;
    font-size: 16px;
}

.nav-links .btn {
    background-color: #ff5722;
    padding: 8px 16px;
    border-radius: 5px;
    color: white;
    text-decoration: none;
}

/* Estilos generales */
.banner {
    text-align: center;
    padding: 50px 0;
    background-color: #f4f4f4;
}

.account-section, .actions-section {
    padding: 20px;
    text-align: center;
}

.account-card, .action-card {
    background-color: #e0e0e0;
    padding: 20px;
    border-radius: 8px;
    margin: 10px;
}

.actions-grid {
    display: flex;
    justify-content: center;
    gap: 20px;
}

.btn {
    background-color: #0077cc;
    color: white;
    padding: 10px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

/* Estilos del chatbot */
.chatbot-container {
    position: fixed;
    bottom: 20px;
    right: 20px;
    width: 300px;
    max-height: 500px;
    background-color: white;
    border: 1px solid #ccc;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    border-radius: 10px;
    overflow: hidden;
    display: flex;
    flex-direction: column;
}

.chatbot-header {
    background-color: #0077cc;
    color: white;
    padding: 10px;
    text-align: center;
}

.chatbot-body {
    flex: 1;
    padding: 10px;
    overflow-y: auto;
}

.chatbot-input {
    display: flex;
    padding: 10px;
    border-top: 1px solid #ccc;
}

.chatbot-input input {
    flex: 1;
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 5px;
}

.chatbot-input button {
    padding: 8px 12px;
    background-color: #0077cc;
    color: white;
    border: none;
    border-radius: 5px;
    margin-left: 10px;
}

.message {
    margin-bottom: 10px;
}

.message.user {
    text-align: right;
    color: blue;
}

.message.bot {
    text-align: left;
    color: green;
}

Código Javascript:
const express = require('express');
const bodyParser = require('body-parser');
const sqlite3 = require('sqlite3').verbose();
const { Configuration, OpenAIApi } = require('openai');

// Configuración de OpenAI
const configuration = new Configuration({
    apiKey: 'TU_API_KEY_AQUI', //  API key de OpenAI
});
const openai = new OpenAIApi(configuration);

const app = express();
const db = new sqlite3.Database('./banco.db'); // Base de datos SQLite

app.use(bodyParser.json());
app.use(express.static('public')); // Servir archivos estáticos como HTML, CSS y JS

// Función de chatbot con IA para interactuar con los usuarios
async function chatbot(usuarioPregunta) {
    try {
        const completion = await openai.createCompletion({
            model: "text-davinci-003",
            prompt: usuarioPregunta,
            max_tokens: 150,
        });
        return completion.data.choices[0].text.trim();
    } catch (error) {
        console.error("Error al interactuar con el chatbot: ", error.message);
        return "Lo siento, no puedo responder a esa solicitud en este momento.";
    }
}

// Función para gestionar solicitudes del chatbot e interactuar con la base de datos
async function gestionarSolicitudChatBot(pregunta, datosCliente) {
    const respuestaIA = await chatbot(pregunta);
    console.log("Chatbot IA: ", respuestaIA);

    if (respuestaIA.includes('saldo')) {
        await consultarSaldo(datosCliente.email);
    } else if (respuestaIA.includes('transferencia')) {
        detallesTransferencia(datosCliente.email, datosCliente.beneficiario, datosCliente.monto);
    } else if (respuestaIA.includes('reclamo')) {
        iniciarReclamo('transaccionNoReconocida', datosCliente.monto);
    } else if (respuestaIA.includes('apertura de cuenta')) {
        abrirCuentaDigital();
    } else if (respuestaIA.includes('tarjeta')) {
        verificarEstadoTarjeta(datosCliente.email);
    } else {
        console.log("No se pudo identificar la solicitud. Prueba con otra pregunta.");
    }
}

// Función para consultar saldo
async function consultarSaldo(email) {
    if (!await clienteExiste(email)) {
        console.log(`El cliente con email ${email} no existe.`);
        return;
    }
    db.get(`
        SELECT c.saldo 
        FROM cuentas c 
        JOIN clientes cl ON cl.id = c.id_cliente 
        WHERE cl.email = ?
    `, [email], (err, row) => {
        if (err) {
            return console.error("Error al consultar saldo: ", err.message);
        }
        if (row) {
            console.log(`El saldo del cliente con email ${email} es: Q${row.saldo}`);
        } else {
            console.log(`No se encontró ninguna cuenta asociada al cliente con email ${email}.`);
        }
    });
}

// Función para verificar si un cliente existe en la base de datos
function clienteExiste(email) {
    return new Promise((resolve, reject) => {
        db.get(`
            SELECT id 
            FROM clientes 
            WHERE email = ?
        `, [email], (err, row) => {
            if (err) {
                console.error("Error al verificar cliente: ", err.message);
                return reject(false);
            }
            resolve(!!row);
        });
    });
}

// Funciones adicionales para realizar operaciones bancarias
function detallesTransferencia(email, beneficiario, monto) {
    if (monto <= 0) {
        console.log("El monto de la transferencia debe ser mayor a cero.");
        return;
    }
    console.log(`Iniciando transferencia de Q${monto} al beneficiario ${beneficiario}.`);
}

function iniciarReclamo(tipoReclamo, monto) {
    if (tipoReclamo === 'transaccionNoReconocida') {
        console.log(`Iniciando el proceso de reclamo por una transacción no reconocida de Q${monto}.`);
        console.log("El departamento de riesgo operacional ha sido notificado.");
    } else {
        console.log("Iniciando el proceso de reclamo por otro motivo.");
    }
}

function abrirCuentaDigital() {
    console.log("Redirigiendo al cliente al proceso de apertura de cuenta digital en línea.");
}

function verificarEstadoTarjeta(email) {
    console.log("Verificando el estado actual de la tarjeta de crédito para el cliente con email: " + email);
}

// Ruta para manejar las solicitudes del chatbot
app.post('/chatbot', async (req, res) => {
    const { pregunta, datosCliente } = req.body;
    
    try {
        // Gestionar la solicitud del usuario y obtener la respuesta
        await gestionarSolicitudChatBot(pregunta, datosCliente);
        res.json({ respuesta: "Solicitud gestionada con éxito." });
    } catch (error) {
        console.error("Error al gestionar la solicitud del chatbot: ", error.message);
        res.status(500).json({ respuesta: "Error al procesar tu solicitud. Intenta nuevamente más tarde." });
    }
});

// Iniciar el servidor
const port = 3000;
app.listen(port, () => {
    console.log(`Servidor corriendo en http://localhost:${port}`);
});


Código SQL:
--  base de datos
CREATE DATABASE IF NOT EXISTS banco_experto;

-- Usar la base de datos
USE banco_experto;

--  tabla de clientes
CREATE TABLE IF NOT EXISTS clientes (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    direccion VARCHAR(255),
    telefono VARCHAR(20),
    fecha_registro TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- tabla de cuentas
CREATE TABLE IF NOT EXISTS cuentas (
    id INT AUTO_INCREMENT PRIMARY KEY,
    id_cliente INT NOT NULL,
    tipo_cuenta ENUM('corriente', 'ahorros', 'inversion') NOT NULL,
    saldo DECIMAL(15, 2) NOT NULL,
    fecha_apertura DATE NOT NULL,
    FOREIGN KEY (id_cliente) REFERENCES clientes(id) ON DELETE CASCADE
);

--  tabla de transacciones
CREATE TABLE IF NOT EXISTS transacciones (
    id INT AUTO_INCREMENT PRIMARY KEY,
    id_cuenta INT NOT NULL,
    tipo_transaccion ENUM('deposito', 'retiro', 'transferencia', 'pago_servicio') NOT NULL,
    monto DECIMAL(15, 2) NOT NULL,
    fecha TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (id_cuenta) REFERENCES cuentas(id) ON DELETE CASCADE
);

--  tabla de préstamos
CREATE TABLE IF NOT EXISTS prestamos (
    id INT AUTO_INCREMENT PRIMARY KEY,
    id_cliente INT NOT NULL,
    monto_prestamo DECIMAL(15, 2) NOT NULL,
    saldo_pendiente DECIMAL(15, 2) NOT NULL,
    fecha_inicio DATE NOT NULL,
    fecha_fin DATE NOT NULL,
    FOREIGN KEY (id_cliente) REFERENCES clientes(id) ON DELETE CASCADE
);

-- tabla de reclamos (opcional, si deseas gestionar reclamos)
CREATE TABLE IF NOT EXISTS reclamos (
    id INT AUTO_INCREMENT PRIMARY KEY,
    id_cliente INT NOT NULL,
    tipo_reclamo ENUM('transaccion_no_reconocida', 'servicio', 'otros') NOT NULL,
    descripcion TEXT,
    estado ENUM('pendiente', 'resuelto', 'en_proceso') DEFAULT 'pendiente',
    fecha_reclamo TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (id_cliente) REFERENCES clientes(id) ON DELETE CASCADE
);

--  tabla de pagos de servicios
CREATE TABLE IF NOT EXISTS pagos_servicios (
    id INT AUTO_INCREMENT PRIMARY KEY,
    id_cuenta INT NOT NULL,
    servicio VARCHAR(100) NOT NULL,
    monto DECIMAL(15, 2) NOT NULL,
    fecha TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (id_cuenta) REFERENCES cuentas(id) ON DELETE CASCADE
);

-- Crear algunos índices para optimizar las consultas
CREATE INDEX idx_cliente_email ON clientes(email);
CREATE INDEX idx_cuenta_id_cliente ON cuentas(id_cliente);
CREATE INDEX idx_transaccion_id_cuenta ON transacciones(id_cuenta);
CREATE INDEX idx_prestamo_id_cliente ON prestamos(id_cliente);
