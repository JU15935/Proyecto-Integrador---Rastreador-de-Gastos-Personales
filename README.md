# Proyecto-Integrador---Rastreador-de-Gastos-Personales

Participantes: Juan Miguel Sanchez y Samuel Cadavid.

 Descripión del Proyecto
Este proyecto implementa un sistema de rastreo y gestión de gastos personales que permite a los usuarios controlar sus finanzas, establecer presupuestos, categorizar transacciones y generar reportes financieros detallados.

  Objetivos del Sistema

Registrar y categorizar todos los ingresos y gastos
Establecer y monitorear presupuestos por categoría
Generar reportes financieros y análisis de tendencias
Gestionar múltiples cuentas bancarias y métodos de pago
Establecer metas de ahorro y seguimiento de progreso
Crear alertas y notificaciones por límites de gasto

  Modelo Entidad-Relación
![Captura de pantalla 2025-05-27 144446](https://github.com/user-attachments/assets/4564afc8-9bb2-472c-a332-6eabce58cc6f)

Escenarios de Uso
Escenario 1: Registro de Gasto Diario

Usuario registra compra de almuerzo ($15.00)
Selecciona cuenta "Tarjeta Débito Principal"
Categoriza como "Alimentación → Restaurantes"
Sistema actualiza saldo de cuenta automáticamente
Verifica si excede presupuesto mensual de alimentación
Genera alerta si supera el 80% del límite

Escenario 2: Transferencia Entre Cuentas

Usuario transfiere $500 de cuenta corriente a cuenta de ahorros
Sistema crea dos transacciones vinculadas
Actualiza saldos de ambas cuentas
Registra comisión si aplica
Actualiza progreso de meta de ahorro asociada

Escenario 3: Análisis de Presupuesto Mensual

Usuario consulta gastos del mes actual
Sistema genera reporte por categorías
Compara con presupuestos establecidos
Muestra porcentajes de cumplimiento
Sugiere ajustes basados en patrones de gasto

Escenario 4: Seguimiento de Meta de Ahorro

Usuario tiene meta "Vacaciones 2025" ($3,000)
Cada ahorro se vincula automáticamente a la meta
Sistema calcula progreso y tiempo restante
Genera alertas de cumplimiento o retraso
Sugiere monto mensual necesario para alcanzar la meta

Reglas de Negocio Detalladas
Transacciones

Gastos: siempre se registran como montos positivos
Ingresos: se registran como montos positivos
Transferencias: crean dos transacciones relacionadas (débito y crédito)
Eliminación: no se permite, solo cancelación con justificación

Presupuestos

Período mínimo: 1 día
Período máximo: 1 año
Alertas: automáticas al 80% (configurable)
Sobregiro: permitido con notificación

Cuentas

Tarjetas de crédito: pueden tener saldo negativo hasta el límite
Cuentas de ahorro: no permiten saldo negativo
Efectivo: no puede tener saldo negativo
Cuentas cerradas: solo consulta, no transacciones

Categorías

Jerarquía: máximo 3 niveles de profundidad
Predeterminadas: disponibles para todos los usuarios
Personalizadas: específicas por usuario
Eliminación: solo si no tiene transacciones asociadas

Metas de Ahorro

Múltiples metas: permitidas simultáneamente
Priorización: alta, media, baja
Fecha límite: opcional pero recomendada
Progreso: calculado automáticamente

Consideraciones Técnicas
Índices Sugeridos

-- Índices para optimizar consultas frecuentes
CREATE INDEX idx_usuario_email ON USUARIO(email);
CREATE INDEX idx_transaccion_fecha ON TRANSACCION(fecha_transaccion);
CREATE INDEX idx_transaccion_usuario_fecha ON TRANSACCION(id_usuario, fecha_transaccion);
CREATE INDEX idx_transaccion_cuenta ON TRANSACCION(id_cuenta);
CREATE INDEX idx_transaccion_categoria ON TRANSACCION(id_categoria);
CREATE INDEX idx_presupuesto_usuario ON PRESUPUESTO(id_usuario);
CREATE INDEX idx_cuenta_usuario ON CUENTA(id_usuario);

Triggers Sugeridos

-- Actualizar saldo de cuenta después de cada transacción
CREATE TRIGGER actualizar_saldo_cuenta 
AFTER INSERT ON TRANSACCION;

-- Actualizar monto gastado en presupuesto
CREATE TRIGGER actualizar_presupuesto 
AFTER INSERT ON TRANSACCION;

-- Generar alertas automáticas
CREATE TRIGGER generar_alertas 
AFTER UPDATE ON PRESUPUESTO;

-- Validar saldo antes de transacción
CREATE TRIGGER validar_saldo 
BEFORE INSERT ON TRANSACCION;

Vistas Útiles

-- Resumen financiero por usuario
CREATE VIEW vista_resumen_financiero;

-- Gastos por categoría del mes actual
CREATE VIEW vista_gastos_mensuales;

-- Estado de presupuestos activos
CREATE VIEW vista_estado_presupuestos;

-- Progreso de metas de ahorro
CREATE VIEW vista_progreso_metas;

Procedimientos Almacenados

-- Generar reporte mensual de gastos
DELIMITER //
CREATE PROCEDURE sp_reporte_mensual(IN usuario_id INT, IN mes INT, IN año INT)

-- Calcular tendencias de gasto
CREATE PROCEDURE sp_analisis_tendencias(IN usuario_id INT, IN meses INT)

-- Sugerir presupuesto basado en historial
CREATE PROCEDURE sp_sugerir_presupuesto(IN usuario_id INT, IN categoria_id INT)

Categorías Predeterminadas

Gastos

Vivienda: Renta, Servicios, Mantenimiento
Alimentación: Supermercado, Restaurantes, Comida rápida
Transporte: Combustible, Transporte público, Mantenimiento vehículo
Salud: Medicina, Doctor, Seguro médico
Entretenimiento: Cine, Streaming, Deportes
Educación: Cursos, Libros, Material educativo
Ropa: Vestimenta, Calzado, Accesorios
Tecnología: Software, Hardware, Servicios digitales
Otros: Gastos varios, Emergencias

Ingresos

Salario: Sueldo principal, Bonificaciones
Freelance: Trabajos independientes, Consultorías
Inversiones: Dividendos, Intereses, Ganancias de capital
Otros: Regalos, Reembolsos, Ingresos varios

Funcionalidades Futuras
Fase 2

Importación de transacciones desde archivos CSV/Excel
Conexión con APIs bancarias para sincronización automática
Reportes avanzados con gráficos y análisis predictivo
Compartir presupuestos con familia/pareja

Fase 3

Aplicación móvil para registro rápido de gastos
Reconocimiento de recibos con OCR
Inteligencia artificial para categorización automática
Comparación con promedios nacionales/regionales

Este proyecto demuestra conocimientos avanzados en modelado de 
datos para sistemas financieros, incluyendo consideraciones de 
seguridad, integridad de datos y análisis financiero personal.
