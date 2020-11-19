# Documentación del ERC-1497: Evidence Standard — ERC-792: Arbitration Standard 1.0.0 

[https://developer.kleros.io/en/latest/erc-1497.html](https://developer.kleros.io/en/latest/erc-1497.html)

Advertencia

Los contratos inteligentes de este tutorial han sido creados con intenciones educativas y no están pensados para ser producidos. Cuidado con usarlos en la red principal.

En el contrato `SimpleEscrow` anterior, usamos un `string` para almacenar el acuerdo entre las partes. El desarrollador podría usar cualquier formato que quisiera para ese `string`, ya que hay muchas formas posibles de transmitir la información sobre el acuerdo.

En lugar de eso, un estándar ofrece interoperabilidad. Por eso el *ERC-1497: Evidence Standard* describe un enfoque estándar para esto. Tiene dos categorías de información: prueba (evidence) y meta-prueba (meta-evidence).

Prueba, como el nombre sugiere, es un pedazo de información para apoyar una propuesta. Meta-prueba es la información sobre la disputa en sí misma: el acuerdo, las partes involucradas, la cuestión que tiene que ser decidida, las posibles decisiones, etc...

ERC-1497 introduce tres eventos: MetaEvidence, Evidence y Dispute.

- `event MetaEvidence` aporta el contexto de la disputa, la pregunta que los árbitros tienen que responder, los significados de las decisiones comprensibles por humanos, y modos específicos de visualización para las pruebas. Usamos `_metaEvidenceID` para identificar una meta-prueba de forma única. Esto es necesario cuando hay multiples casos de uso para meta-pruebas. `_evidence` contiene el URI para el archivo JSON de la meta-prueba.
- `event Evidence` vincula una prueba con un árbitro, parte emisora y disputa. `_evidence` contiene la URI para el archivo JSON de la prueba.
- `event Dispute` es lanzado en cuanto una disputa es creada, para así vincular la meta-prueba y el grupo de pruebas apropiados a la disputa. El evento incluye una referencia al árbitro, un identificador único para la disputa en sí misma, el identificador para comprobar el registro de eventos de la meta-prueba y el idenficador del grupo de pruebas que puede ser usado para comprobar todas las pruebas subidas a ese grupo.

Vamos a volver a `SimpleEscrow` y lo vamos a refactorizar para implementar la interfaz del ERC-1797. Recordemos `SimpleEscrow`:

Ahora, primero vamos a implementar IEvidence:

Lo siguiente es librarnos de `string agreement`. En su lugar, necesitamos `uint metaevidenceID` y `string _metaevidence`, que contiene la URI para el JSON con la meta-prueba cuyo formato ha sido realizado según el estándar, y también tenemos que emitir el evento `MetaEvidence`.

Establecemos el identificador de la meta-prueba a la constante cero, ya que no habrá múltiples meta-pruebas en este contrato. Cualquier constante puede servir. Después, emitimos `MetaEvidence` con la `_metaevidence` provista. Este string contiene la URI desde la cual el contenido de la meta-prueba puede ser obtenido.

También necesitamos emitir `Dispute` cuando creamos una nueva disputa:

Solo habrá una disputa en este contrato, por lo que podemos usar una constante cero para `evidenceGroupID`.

Por último, necesitamos una función para permitir que las partes presenten sus pruebas:

¡Felicidades, ahora tu arbitrable es compatible con ERC-1497!
