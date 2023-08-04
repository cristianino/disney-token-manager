Sure! Below is the information for your README file in Markdown format:

---

# Disney Token Manager with ERC-20 (no-official)

## Introduction

This repository contains a basic implementation of a Disney Token Manager using the ERC-20 standard on the Ethereum blockchain. The smart contract allows the creation, activation, and deactivation of attractions in a Disney theme park, as well as facilitating payments for food and providing a client's transaction history. The contract utilizes modifiers for implementing access control and events to emit information about attraction and food-related activities.

## ERC-20 Token

The smart contract implements the ERC-20 standard, which is a widely adopted token standard on the Ethereum blockchain. It ensures that the Disney tokens have a consistent interface, allowing them to be managed and traded on various decentralized applications (DApps) and exchanges that support ERC-20 tokens.

## Functionality

### Creating Attractions

The contract provides a function to create new attractions within the Disney theme park. The creator of the contract or an authorized entity can add new attractions along with their relevant details, such as name, description, and activation status.

### Activating/Deactivating Attractions

Attractions can be activated and deactivated by the contract owner or authorized parties. Activated attractions are accessible to visitors, while deactivated attractions are temporarily unavailable.

### Food Payment

The contract allows users to make payments for food using the Disney tokens. Visitors can interact with the contract to purchase food items within the theme park.

### Transaction History

The contract keeps track of all client transactions involving attraction activation, deactivation, and food purchases. Clients can view their transaction history to keep track of their token usage within the park.

## Access Control with Modifiers

Modifiers are used to enforce access control restrictions within the contract. Certain functions can only be executed by the contract owner or other authorized addresses. Modifiers help ensure that critical functions are protected from unauthorized access.

## Events

The contract emits events to notify external applications and interfaces about important activities within the theme park. Events are emitted when attractions are activated or deactivated, and when food purchases are made.

## How to Use

To deploy and interact with the Disney Token Manager smart contract, follow these steps:

1. Install the required dependencies (list the dependencies and versions if applicable).

2. Deploy the smart contract to the Ethereum blockchain (provide deployment instructions).

3. Interact with the contract using the provided API or smart contract functions (document the available functions and their parameters).

4. Access the emitted events to stay informed about attraction and food-related activities (subscribe to events and interpret event data).

## Examples

Here are some code snippets to demonstrate how to interact with the contract:

1. **Creating an Attraction:**
```solidity
// Crear nuevas atracciones para DISNEY (SOLO es ejecutable por Disney)
    function NuevaAtraccion(string memory _nombreAtraccion, uint _precio) public Unicamente (msg.sender) {
        // Creacion de una atraccion en Disney 
        MappingAtracciones[_nombreAtraccion] = atraccion(_nombreAtraccion,_precio, true);
        // Almacenamiento en un array el nombre de la atraccion 
        Atracciones.push(_nombreAtraccion);
        // Emision del evento para la nueva atraccion 
        emit nueva_atraccion(_nombreAtraccion, _precio);
    }
```

2. **Activating an Attraction:**
```solidity
// Funcion para subirse a una atraccion de disney y pagar en tokens 
    function SubirseAtraccion (string memory _nombreAtraccion) public {
        // Precio de la atraccion (en tokens)
        uint tokens_atraccion = MappingAtracciones[_nombreAtraccion].precio_atraccion;
        // Verifica el estado de la atraccion (si esta disponible para su uso)
        require (MappingAtracciones[_nombreAtraccion].estado_atraccion == true, 
                    "La atraccion no esta disponible en estos momentos.");
        // Verifica el numero de tokens que tiene el cliente para subirse a la atraccion 
        require(tokens_atraccion <= MisTokens(), 
                "Necesitas mas Tokens para subirte a esta atraccion.");
        
        /* El cliente paga la atraccion en Tokens:
        - Ha sido necesario crear una funcion en ERC20.sol con el nombre de: 'transferencia_disney'
        debido a que en caso de usar el Transfer o TransferFrom las direcciones que se escogian 
        para realizar la transccion eran equivocadas. Ya que el msg.sender que recibia el metodo Transfer o
        TransferFrom era la direccion del contrato.
        */
        token.transferencia_disney(msg.sender, address(this),tokens_atraccion);
        // Almacenamiento en el historial de atracciones del cliente 
        HistorialAtracciones[msg.sender].push(_nombreAtraccion);
        // Emision del evento para disfrutar de la atraccion 
        emit disfruta_atraccion(_nombreAtraccion, tokens_atraccion, msg.sender);
    }
```

3. **Paying for Food:**
```solidity
// Funcion para comprar comida con tokens
    function ComprarComida (string memory _nombreComida) public {
        // Precio de la comida (en tokens)
        uint tokens_comida = MappingComida[_nombreComida].precio_comida;
        // Verifica el estado de la comida (si esta disponible)
        require (MappingComida[_nombreComida].estado_comida == true, 
                    "La comida no esta disponible en estos momentos.");
        // Verifica el numero de tokens que tiene el cliente para comer esa comida
        require(tokens_comida <= MisTokens(), 
                "Necesitas mas Tokens para comer esta comida.");
        
        /* El cliente paga la atraccion en Tokens:
        - Ha sido necesario crear una funcion en ERC20.sol con el nombre de: 'transferencia_disney'
        debido a que en caso de usar el Transfer o TransferFrom las direcciones que se escogian 
        para realizar la transccion eran equivocadas. Ya que el msg.sender que recibia el metodo Transfer o
        TransferFrom era la direccion del contrato.
        */
        token.transferencia_disney(msg.sender, address(this),tokens_comida);
        // Almacenamiento en el historial de comidas del cliente 
        HistorialComidas[msg.sender].push(_nombreComida);
        // Emision del evento para disfrutar de la comida 
        emit disfruta_comida(_nombreComida, tokens_comida, msg.sender);
    }
```

4. **Viewing Transaction History:**
```solidity
// Visualiza el historial completo de atracciones disfrutadas por un cliente 
    function Historial() public view returns (string [] memory) {
        return HistorialAtracciones[msg.sender];
    }
    
    // Visualiza el historial completo de comidas disfrutadas por un cliente 
    function HistorialComida() public view returns (string [] memory) {
        return HistorialComidas[msg.sender];
    }
```

