# Presentamos “Baking como Servicio” - BaaS
En mis artículos anteriores acerca de las diferentes posibilidades para ganar ingresos por los servicios en Tezos, he presentado tres opciones que los poseedores de tokens Tezos pueden elegir: solo-baking (ustedes realizan su propio horneo/baking, offering delegation services (oferta de servicios de delegación), o delegating (delegación). Hoy, presentamos el Baking/horneo Como Servicio (BaaS) como cuarta opción para un poseedor de tokens Tezos. Luego de varios meses de testeos internos de nuestro BaaS en beta, estamos orgullosos de lanzarlo como servicio público.

## ¿Qué es el Baking as a Service?
A pesar de siempre recomendar en primer lugar que los usuarios ganen experiencia “horneando, esta no es una labor poco importante: requiere tomarse una cantidad significativa de tiempo; este puede ser menor si se lo toma como un hobby, e incrementa significativamente cuando se hace esta labor de manera profesional. Por otro lado, delegar es mucho más fácil pero puede tener recompensas mucho más bajas debido a las tarifas del servicio. 
Presentamos entonces el Baking as a Service (BaaS) como la cuarta oferta de Cryptium Labs. Este servicio está inspirado por los pedidos de algunos de nuestros delegantes, quienes demostraron interés en hornear, pero no pudieron hacerlo o tuvieron que detener su labor debido a falta de tiempo o recursos. En resumen, Baking as a Service es el equivalente a tener su propia “Panadería”/Bakery de Tezos con un 5% de tarifa por servicio - y sin las molestias. 

## ¿Cuales son las recompensas?
BaaS tiene principalmente tres beneficios: 
1. Ahorrarles las tarifas de servicio que se abonan por delegar de manera regular. Esencialmente, ustedes estarían delegando a un baker/panadero que cobra 0% en tarifas de servicio, o como si estuvieran horneando ustedes mismos.
2. 50% en recompensas: ustedes podrán acceder a 50% de las recompensas por las tarifas de servicios generadas por la delegación regular que su cantidad de BaaS les permita. Como nuestra tarifa de servicio es de 10% a todas nuestras delegaciones, en práctica, esto lleva al mismo resultado que si ustedes ofrecieran un servicio de baking cuya tarifa de servicio es del 5% a los delegantes. Para más ejemplos, aquí hay una tabla comparativa de expectativas de ganancias:

![Comparativa de ganancias esperadas entre la delegación normal y nuestro BaaS](https://github.com/cryptiumlabs/library/blob/master/tezos/figures/BaaS-spanish.png "Comparativa de ganancias esperadas entre la delegación normal y nuestro BaaS")
(Comparativa de ganancias esperadas entre la delegación normal y nuestro BaaS)

3. Cobertura en vivo (liveness faults): Esto funciona como si ustedes fueran bakers que nunca pierden un horneo o un turno aprobaciones (endorsements) y ser recompensados por las mismas, debido a inactividad. 

## ¿Cuáles son los Riesgos? 
BaaS tiene en principio dos riesgos: 
1. Custodia: BaaS requiere que el usuario transfiera sus XTZ a nuestra dirección TZ1. Actualmente, esta es la única manera en la cual podemos hacer posible el BaaS. Hemos considerado contratos inteligentes o hasta ofrecer un token. Sin embargo, ninguna de estas opciones evitaría el requisito de la custodia. Para poder remover este requisito, el protocolo Tezos debería ser modificado de manera tal que los depósitos de seguridad sólo puedan ser tomados de cuentas diferentes, y no de la cuenta del panadero solamente.
2. Fallas de seguridad (safety faults): de manera similar al riesgo que un panadero solitario o un panadero público se encuentra expuesto, los fondos de los usuarios de BaaS se encuentran en las mismas condiciones debido a que se utilizarán como depósitos de seguridad. Las pérdidas por double-baking y double-endorsing afectarán las contribuciones de los usuarios de BaaS y no se encuentran aseguradas. De todas maneras, Cryptium Labs toma todas las medidas posibles y necesarias para mitigar este riesgo. Al día de la fecha, Cryptium Labs no ha tenido fallas de seguridad. 

## ¿Cómo comenzar a utilizar BaaS?
BaaS se recomienda para los poseedores de XTZ que tienen experiencia con el protocolo Tezos, de manera tal que comprendan de antemano los potenciales riesgos y recompensas. Esta experiencia puede ser adquirida horneando o delegando activamente, así como también siguiendo los desarrollos del protocolo. 
A pesar de que ofrecemos BaaS como servicio público, queremos asegurarnos que nuestros usuarios de BaaS estén tan informados como sea posible y tengan el tiempo necesario para considerar en detalle si esta oferta es la más interesante para ellos. Si ustedes se encuentran interesados, por favor envíenos un mensaje por e-mail o Telegram, nuestros datos de contacto se encuentran aquí: hello@cryptium.ch.
 
## Consideraciones Finales 
En primer lugar, no recomendamos BaaS a principiantes en el espacio de las criptomonedas o blockchain. En segundo lugar, si deciden participar de nuestro BaaS, recomendamos diversificar dividiendo la cantidad: una porción en BaaS y otra en delegación normal. Finalmente, tengan en cuenta que pueden existir “exit scams” o ataques de phishing, debido a que existen otros servicios de delegación pública ofreciendo servicios similares a BaaS y siempre cabe la posibilidad que surjan sitios web intentando personificar panaderos. Aseguren interactuar de manera suficiente con su panadero, y tener confianza en la relación con el mismo. 
