# Solana

Solana es una de las tantas blockchains que existen. Su programming model, algoritmo de consenso y particularidades la diferencian de otros blockchains y lo hacen nuna de las plataformas mas interesantes sobre la que desarrollar.

### Como funciona un blockchain

Una blockchain es para todos los efectos practicos una base de datos. La caracteristica innovadora de las blockchains es que las modificaciones que se hacen sobre esa base de datos son descentralizadas y cualquier puede hacerlas, al contrario de las bases de datos tradicionales donde solo un determinado grupo de personas con privilegios especiales puede hacer modificaciones.

##### Estado actual

Como toda base de datos, la blockchain tiene un estado actual. En el caso de Solana, Ethereum y Bitcoin consultar el estado actual del blockchain siempre es gratis. Modificar el estado del blockchain siempre tiene un costo.

##### Transacciones

Para modificar el estado del blockchain se deben introducir transacciones. Cada blockchain tiene sus particularidades en cuanto a como estan compuestas las transacciones, como se procesan y cuanto cuesta a quien las introduce hacerlas procesar.

Se pueden crear transacciones que sean erroneas y no sean procesables, por ejemplo si intentara enviar un monto en SOL desde una billetera que no tiene saldo. Es por esta razon que las transacciones tienen que ser verificadas antes de que se incorporen al estado del blockchain.

##### Algoritmos de consenso

Cada blockchain tiene como proceso de verificacion de transacciones un [algoritmo de conseso](https://www.geeksforgeeks.org/consensus-algorithms-in-blockchain/). A traves de este algoritmo es que todas las computadoras en la red se ponen de acuerdo (de ahi el "consenso") en que transacciones incorporar al blockchain. Solana utiliza un algoritmo llamado [Proof of History](https://solana.com/solana-whitepaper.pdf).

### Solana programming model
