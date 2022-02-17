# Solana

Solana es una de las tantas blockchains que existen. Su programming model, algoritmo de consenso y particularidades la diferencian de otros blockchains y lo hacen nuna de las plataformas mas interesantes sobre la que desarrollar.

### Como funciona un blockchain

Una blockchain es para todos los efectos practicos una base de datos. La caracteristica innovadora de las blockchains es que las modificaciones que se hacen sobre esa base de datos son descentralizadas y cualquier puede hacerlas, al contrario de las bases de datos tradicionales donde solo un determinado grupo de personas con privilegios especiales puede hacer modificaciones.

##### Estado actual

Como toda base de datos, la blockchain tiene un estado actual. En el caso de Solana, Ethereum y Bitcoin [consultar el estado actual del blockchain siempre es gratis](https://explorer.solana.com/). Modificar el estado del blockchain siempre tiene un costo.

##### Transacciones

Para modificar el estado del blockchain se deben introducir transacciones. Cada blockchain tiene sus particularidades en cuanto a como estan compuestas las transacciones, como se procesan y cuanto cuesta a quien las introduce hacerlas procesar.

Se pueden crear transacciones que sean erroneas y no sean procesables, por ejemplo si intentara enviar un monto en SOL desde una billetera que no tiene saldo. Es por esta razon que las transacciones tienen que ser verificadas antes de que se incorporen al estado del blockchain.

##### Algoritmos de consenso

Cada blockchain tiene como proceso de verificacion de transacciones un [algoritmo de conseso](https://www.geeksforgeeks.org/consensus-algorithms-in-blockchain/). A traves de este algoritmo es que todas las computadoras en la red se ponen de acuerdo (de ahi el "consenso") en que transacciones incorporar al blockchain. Solana utiliza un algoritmo llamado [Proof of History](https://solana.com/solana-whitepaper.pdf).



### Solana programming model

El [programming model](https://docs.solana.com/developing/programming-model/overview) refiere a la arquitectura que se usa para organizar la informacion en Solana. Muy parecido a Unix donde [todo es un archivo](https://en.wikipedia.org/wiki/Everything_is_a_file), en Solana, todo lo que forma parte del estado del blockchain es un [account](https://docs.solana.com/developing/programming-model/accounts).

##### Accounts

Igual que los archivos en cualquier sistema operativo, una account en Solana es la abstraccion que se utiliza para almacenar informacion. Un account tiene las siguientes propieadades:

* Address: Es la manera de identificar el account, igual que un archivo tiene un `path` unico en el disco, un account tiene un address unico.
* Data: El contenido de la account, igual que el contenido de un archivo.
* Balance: A diferencia de un sistema operativo comun y corriente donde uno puede tener tantos archivos como quiera, en Solana las accounts necesitan pagar para justificar su estadia en la blockchain. Cuando uno crea un account, deposita en ella una cantidad determinada de SOL para que el blockchain se cobre su renta. El monto de la renta es directamente proporcional al espacio reservado para la `data` del account. Si el balance depositado en la account es suficiente, el blockchain le perdona la renta y la account pasa a ser permanente.
* Owner: El programa que tiene permiso para editar la account.
