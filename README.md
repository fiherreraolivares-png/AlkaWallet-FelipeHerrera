# AlkaWallet-FelipeHerrera
1. Estructura y Atributos
Primero, se define la clase Cuenta. Tiene dos componentes clave:
•	saldo: Una variable privada que guarda el dinero del usuario en Pesos Chilenos (CLP).
•	TASA_CLP_A_USD: Una constante (valor que no cambia) que define cuánto vale un peso en dólares. Actualmente está configurada en 0.00105.
2. El Corazón del Programa (Método main)
Es donde ocurre la interacción con el usuario:
•	Inicialización: El programa pide al usuario que ingrese cuánto dinero tiene para empezar y crea el objeto miCuenta.
•	El Bucle do-while: Es el menú que se repite una y otra vez. Permite que el usuario realice múltiples operaciones sin que el programa se cierre después de la primera acción.
•	El switch: Es como un interruptor que decide qué parte del código ejecutar según el número (1 al 5) que el usuario elija.
3. Las Operaciones (Métodos de Lógica)
Para que el código sea ordenado, las acciones están separadas en funciones específicas:
•	depositar: Suma dinero al saldo, pero primero verifica que el monto sea mayor a 0 para evitar errores.
•	retirar: Resta dinero, pero incluye una regla de negocio: no puedes retirar más de lo que tienes (evita saldos negativos).
•	convertirAUsd: Es la función matemática. Multiplica el saldo actual por la tasa de cambio para mostrar el valor en dólares.
4. Detalles de Formato y UX
•	printf: Verás que se usa %.0f o %.2f. Esto sirve para que los números se vean bonitos (sin decimales para los pesos y con 2 decimales para los dólares).
•	Método pausar: Un pequeño truco para que el menú no aparezca de golpe. Obliga al usuario a presionar "Enter" para que pueda leer el resultado de su transacción.
"Es un sistema que gestiona un saldo bancario. Permite sumar, restar y consultar dinero, con la funcionalidad extra de convertir ese saldo a dólares automáticamente usando una tasa predefinida, todo controlado mediante un menú interactivo en la consola."




Informe Técnico: Clase Cuenta
1. Objetivo del Software:
Proporcionar una interfaz de consola para la gestión de finanzas personales, permitiendo depósitos, retiros y la conversión de divisa de Pesos Chilenos (CLP) a Dólares (USD).
2. Estructura de la Clase:
•	Atributos:
o	saldo: Almacena el capital disponible en tipo double.
o	TASA_CLP_A_USD: Constante estática (0.00105) que define el factor de conversión.
•	Métodos Principales:
o	depositar(double): Incrementa el saldo validando montos positivos.
o	retirar(double): Disminuye el saldo previa verificación de fondos suficientes.
o	convertirAUsd(): Calcula la equivalencia internacional del saldo actual.
3. Lógica de Negocio:
El sistema utiliza un ciclo do-while para mantener la sesión activa. La tasa de cambio está proyectada para el año 2026, lo que automatiza el cálculo financiero sin requerir entradas externas de divisas.

Explicación Simple (Para el usuario)
Imagina que este código es el cerebro de un cajero automático de Alke Wallet. Así es como funciona paso a paso:
1.	Inicio de Sesión: Lo primero que hace el programa es preguntarte con cuánto dinero estás empezando.
2.	El Menú de Opciones: Se te presenta una lista de 5 acciones posibles. El programa se queda "esperando" a que elijas un número.
3.	Movimientos de Dinero:
Si depositas, el programa suma ese monto a tu saldo.
Si retiras, el programa primero revisa si tienes suficiente dinero. Si intentas sacar más de lo que tienes, te avisa que no se puede.
4.	La Calculadora de Dólares: Esta es la función especial. El programa toma tus pesos chilenos y los multiplica por una tasa fija para decirte cuánto dinero tendrías si viajaras a Estados Unidos (USD).
5.	Persistencia: El ciclo se repite una y otra vez hasta que elijas la opción 5 (Salir), momento en el cual se cierra la aplicación.
Diagrama de flujo

    class BilleteraOperaciones {
        <<interface>>
        +depositar(double cantidad)*
        +retirar(double cantidad)*
        +convertirMoneda(double tasaCambio)* double
    }

    class Cuenta {
        -double saldo
        -static final double TASA_CLP_A_USD
        +Cuenta(double inicial)
        +main(String[] args)$
        +getSaldo() double
        +depositar(double monto)
        +retirar(double monto)
        +convertirAUsd() double
        +convertirMoneda(double tasa) double
        -pausar(Scanner scanner) void$
    }

    BilleteraOperaciones <|.. Cuenta : implementa


Stack Tecnológico
Lenguaje de Programación: Java SE (Standard Edition).
Versión recomendada: Java 17 (LTS) o superior. Se eligió una versión Long-Term Support por su estabilidad y por ser el estándar actual en entornos corporativos.
Paradigma: Programación Orientada a Objetos (POO). Se utilizaron conceptos de encapsulamiento (atributos privados), constructores y métodos de instancia.
API de Entrada/Salida: java.util.Scanner. Utilizada para la captura de flujos de datos desde la consola del sistema.
Entorno de Ejecución: JRE (Java Runtime Environment) compatible con la versión del compilador utilizada.
Documentación: Javadoc. Se integraron etiquetas estándar (@param (Parámetro)
Se utiliza para explicar qué información necesita recibir un método para funcionar, @return (Retorno)
Se utiliza para explicar qué resultado entrega el método después de ejecutarse) para la generación automática de manuales técnicos en formato HTML.

 Informe de Errores y Observaciones Técnicas durante las pruebas realizadas
1.	Inconsistencia de Paquete (Lógica vs. Test):
El test está en com.alkewallet.service. Si la clase Cuenta es una entidad (POJO), usualmente debería estar en com.alkewallet.model. Si el test no compila, verifica que la clase Cuenta sea pública y esté correctamente importada o en el mismo paquete.
2.	Falta de Validación de Casos de Borde (Edge Cases):
1.	Valores Negativos: El test testDepositar no verifica qué pasa si envías un monto negativo (e.g., cuenta.depositar(-500)). Según las guías de JUnit 5 sobre aserciones, deberías probar si el sistema lanza una excepción o ignora la operación.
2.	Retiro de Saldo Exacto: No hay un test que verifique cuenta.retirar(1000.0). Es vital confirmar si el saldo puede quedar en 0.
3.	Error de Lógica en testRetirarInsuficiente:
El test solo verifica que el saldo no cambie, pero no valida si el método retirar devuelve un boolean (false) o lanza una excepción. Si el método falla silenciosamente, el test pasa, pero el usuario de la aplicación no sabrá por qué falló la transacción.
4.	Uso de Delta en Comparación de Decimales (double):
En JUnit, comparar double con assertEquals puede dar errores de precisión por cómo Java maneja los decimales. Se recomienda usar un tercer parámetro (delta):
assertEquals(1500.0, cuenta.getSaldo(), 0.001, "Mensaje");
5.	Acoplamiento en testConvertirMoneda:
El test asume que convertirMoneda solo multiplica el saldo actual. Si el requerimiento es que el método actualice el saldo interno de la cuenta, el test está incompleto. Si es solo un cálculo externo, el nombre del método en una clase de servicio podría ser confuso.


