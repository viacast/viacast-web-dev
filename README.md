# Teste de conhecimento em tecnologias web

O projeto consiste numa plataforma de monitoramento de estatísticas de dispositivos.
Usuários podem se cadastrar na plataforma, e em seguida registrar novos dispositivos a serem monitorados.
As estatísticas geradas são exibidas para o usuário através de gráficos.
Cada usuário só pode monitorar as estatísticas dos próprios dispositivos.

O desafio possui duas partes: desenvolvimento do backend, e desenvolvimento do frontend.
Crie um repositório `git` para cada uma. Ambas devem ser obrigatoriamente desenvolvidas utilizando TypeScript.

# Etapa 1: Backend

O servidor consiste em um único projeto com três partes principais: banco de dados, servidor HTTP, e comunicação em tempo real.
O projeto deve ser obrigatoriamente em Node, usando o framework [`express`](https://expressjs.com/) para o servidor HTTP, e o framework [TypeORM](https://typeorm.io/) para o banco de dados. Para fins de simplicidade, utilize o `sqlite` como driver para o banco de dados.

O uso de outras tecnologias fica a seu critério, porém são sugeridas as que seguem:

- [`socket.io`](https://socket.io/) para comunicação em tempo real com os clientes (aplicação web)

## Parte 1: Banco de dados

O banco de dados consistirá de duas entidades: usuários e dispositivos. Com exceção da senha do usuário e relação 1:N (um usuário pode possuir N dispositivos), fica a seu critério guardar outras informações. A senha do usuário não deve ser armazenada como texto simples. 

A estatísticas geradas pelos dispositivos não precisam ser armazenadas.

## Parte 2: Servidor HTTP

O servidor HTTP deverá ter rotas para as operações de registro de usuários e dispositivos, autenticação, e listagem de dispositivos de um usuário. Fica a seu critério a criação de outras rotas com mais funcionalidades.

## Parte 3: Comunicação em tempo real

O servidor deve permitir comunicação em tempo real entre os dispositivos e os usuários. Caso opte por utilizar `socket.io`, isso pode ser feito, por exemplo, com um evento em que o dispositivo submete as informações de latência para o servidor, e um evento em que o servidor repassa essas informações para o usuário.

# Etapa 2: Frontend

A interface consiste em um único projeto com três partes principais: registro de usuários, painel de controle do usuário, e painel de controle do dispositivo.
O projeto deve ser obrigatoriamente desenvolvido com o framework React, utilizando a funcionalidade de hooks (`useState()`, `useRef()`, ...), ao invés de componentes de classe, como o declarado a seguir:

```js
/* ❌ don't use these!! */
class MyComponent extends React.Component
```

O uso de outras tecnologias fica a seu critério, porém são sugeridas as que seguem:

- [`axios`](https://axios-http.com/docs/intro) para requisições HTTP (registro de usuários e dispositivos, autenticação, e listagem de dispositivos registrados)
- [`socket.io`](https://socket.io/) para comunicação em tempo real com o servidor (monitoração das estatísticas dos dispositivos)

## Parte 1: Registro de usuários

O registro pode ser simplificado, como descrito [acima](#parte-1-banco-de-dados).

## Parte 2: Painel de controle do usuário

No painel de controle do usuário, deverá haver alguma forma de registrar novos dispositivos, constando no mínimo alguma informação como número de série (uso de informações adicionais fica a seu critério). Ao ser registrado, o dispositivo passa a aparecer no painel de controle do usuário, com um gráfico referente às suas estatísticas.

No gráfico de estatísticas será exibida a informação de latência enviadas pelo dispositivo ao servidor. O valor da latência corresponde ao eixo vertical, com o horário que a informação foi gerada pelo dispositivo no eixo horizontal.

## Parte 3: Painel de controle do dispositivo

Para simular um dispositivo gerando informações de latência, a interface deverá ter uma página em que é possível modificar as configurações dos dispositivos. A aplicação web será responsável pela geração dos dados.

Fica a seu critério se será uma página acessada de forma exclusiva para cada dispositivo, ou se, por exemplo, a página faz parte do acesso de um usuário, onde é possível configurar todos os dispositivos daquele usuário simultaneamente.

Devem constar as seguintes opções, configuráveis independentemente para cada dispositivo:

- Latência base (1~1000 ms)
- Jitter (0~100 ms)
- Habilitar/desabilitar transmissão de estatísticas
- Frequência de envio das estatísticas (1~30 Hz)

As informações de latência base e jitter devem ser usadas para gerar o valor de latência que será enviado para o servidor. Parta da seguinte operação para geração desse valor:

```js
const latency = baseLatency + (Math.random() - 0.5) * jitter;
```

Atente-se que existem situações em que essa operação não irá gerar valores válidos de latência. Portanto, busque uma alternativa viável.

Ajustes feitos nessas configurações devem refletir imediatamente nos dados enviados ao servidor (não deve existir botão de confirmar).

# Etapa 3: Deployment

Ambas partes do projeto deverão ser lançadas independentemente utilizando algum serviço de deployment. O único critério é que tanto o frontend quanto o backend sejam acessíveis pela internet.

# Orientações gerais

## Qualidade do código

### Linting

Siga [essas orientações](https://www.notion.so/EditorConfig-5f494ae4b47248c1b16681ff74d6766c) para configuração das extensões EditorConfig, ESLint, e Prettier para o VSCode.

### Código em inglês

Opte por comentários e nomes de variáveis em inglês.

```js
/* ✅ ok */
const latency = ...

/* ❌ not ok */
const latencia = ...
```

### Bons comentários

Escreva comentários descritivos apenas quando necessário. Se o código pode ser entendido rapidamente sem necessidade de esclarecimento, ele dispensa comentários.

```js
/* ✅ good comment. explains something that might not be obvious */
// accounting for pixel offset
const adjustedWidth = width + 1;

/* ❌ bad comment. states the obvious and doesn't explain what is being done */
// adjusting width by 1
const adjustedWidth = width + 1;

/* ❌ bad comment. states the obvious */
// calculating the area
const area = Math.PI * radius**2;
```

### Versionamento

Utilize o `git` para controle de versão do seu código, fazendo commits frequentes.
