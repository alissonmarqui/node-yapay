# node-yapay
Biblioteca de integração do intermediador Yapay para transações em cartão de credito e boleto em Node.js 

## Instalação
`npm install node-yapay`

## Como Usar

### Configuração
Para configurar o objeto, informe o token de cliente nos parâmetros do construtor. Opcionalmente, é possível habilitar o modo sandbox

```javascript
const Yapay = require('node-yapay');

let yapay = new Yapay({
   token: 'ABCDEFGH12345678ABCDEFGH12345678',
   sandbox: true, //opcional, default = false
   reseller: 'ABCDEFGH12345678ABCDEFGH12345678' //opcional
})
```

### Simular Parcelamento
```javascript
yapay.simulateSplitting(value, (err, data) => {

});
```

### Consultar Cliente
Faz a consulta do cliente a partir do CPF ou e-mail
```javascript
yapay.getPerson({ email: String, cpf: String}, (err, data) => {

})
```

### Processar Transação
Para envio da transação com cartão de crédito, é preciso definir alguns dados obrigatórios:

#### Definir Cliente
```javascript
yapay.setCustomer({
    email: String,
    name: String,
    cpf: String,
    phone_number: String
});
```

#### Definir Endereço
```javascript
yapay.setAddress({
    type_address: String, //'B' para Entrega, 'D' para Cobrança
    postal_code: String,
    street: String,
    number: String,
    neighborhood: String,
    city: String,
    state: String
})
```

#### Definir Dados de Entrega
```javascript
yapay.setShipping({
    price: Number,
    type: String
})
```

#### Definir URL de notificação
```javascript
yapay.setUrlNotification({
    url: String
})
```

#### Atribuir Valor de Desconto
```javascript
yapay.setDiscount(Number)
```

#### Adicionar Produto
```javascript
yapay.addProduct({
    description: String,
    quantity: Number,
    price: Number
})
```

#### Enviar Transação de Cartão
```javascript
yapay.payment({
    card_number: String,
    card_name: String,
    card_expire_month: String,
    card_expire_year: String,
    card_cvv: String,
    split: Number
}, (err, result) => {

})
```

#### Enviar Transação de Boleto
```javascript
yapay.payment({
    payment_method_id: '6'
}, (err, result) => {

})
```

#### Consultar Transação
```javascript
yapay.getTransaction(token_transaction, (err, result) => {

})
```
#### Gerar codigo de autorização e token de acesso
Algumas funções dependem do token de acesso para serem executadas.
Para conseguir este token deve ser primeiro gerado o codigo de autenticação para que depois possa ser chamado o metodo para geração do token de acesso.

#### Gerar codigo de autorização
```javascript
yapay.createResellerCode(consumerKey, consumerSecret, (err, result) => {

})
```

#### Gerar Token de acesso
```javascript
yapay.generateAccessToken(consumerKey, consumerSecret, (err, result) => {

})
```

#### Cancelar Transação
```javascript
yapay.cancelTransaction(transaction_id, (err, result) => {

})
```