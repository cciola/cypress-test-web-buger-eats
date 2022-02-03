## Automação de testes web com Cypress
Projeto do curso Cypress Discovery, da QA Ninja.

-----------------------------------------
### O que este script faz?
Este script efetua alguns testes na aplicação [Buger Eats](https://buger-eats-qa.vercel.app).

-----------------------------------------
### Instalação e uso da arquitetura

**Atenção:** O Cypress somente funciona em computadores 64-bits (MacOS, Linux ou Windows).

- Instale as ferramentas:
  - [NodeJS](https://nodejs.org/en/download/ "NodeJS")
  - [NPM](https://www.npm.com/ "NPM")
  - [Cypress](https://www.npmjs.com/package/cypress/ "Cypress")
  - [Git for Windows](https://gitforwindows.org/)
  - [cmder](https://cmder.net/): optar pela instalação full
  - [VS Code](https://code.visualstudio.com/ "VS Code"): após a instalação, vamos utilizar a  extensão *Material Icon Theme*, do Philipp Kief

- Abra um terminal que aceite comandos git (exemplo: Powershell, git bash, cmder, etc.)
- Baixe este repositório ou faça um git clone (HTTPS/SSH)
- Abra o diretório do projeto via terminal e execute o comando abaixo para instalar as dependências:
```
npm install --save -dev
```
- Para verificar se possui as versões instaladas, digite no terminal:
```
npm -v && node -v
```

- Para abrir a interface gráfica do Cypress e escolher quais testes deseja executar, digite no terminal:
```
npx cypress open
```

- Para executar todos os testes em modo headless, digite no terminal:
```
npx cypress run
```

-----------------------------------------
### Arquitetura do projeto

```
📂 cypress-test-web/
  ├─ 📂 cypress/
  │        │
  │        ├── 📂 fixtures/ (pasta que contém arquivos com a massa de dados utilizada nos testes)
  │        │   └── 📂 images/ (contém as imagens utilizadas nos testes)
  |        |       └── 📜 nome-do-arquivo.jpg
  |        |
  |        ├── 📂 integration/ (pasta que contém os testes)
  │        │   └── 📜 cadastro.spec.js
  |        |   └── 📜 home.spec.js
  │        │
  │        ├── 📂 plugins/
  │        │   └── 📜 index.js
  │        │
  │        ├── 📂 support/
  │        │   ├── 📜 commands.js
  │        │   └── 📜 index.js
  │        │
  ├── 📂 node_modules/
  ├── 📜 .gitignore
  ├── 📜 cypress.json
  ├── 📜 package-lock.json
  ├── 📜 package.json
  └── 📜 README.md
```

-----------------------------------------
### Camadas da arquitetura

- **fixtures:** contém arquivos com a massa de dados utilizada nos testes
- **integration:** contém os testes
- **plugins:** plugins que são utilizados na solução ficam dentro do arquivo "plugins/index.js"
- **support:** camada com comandos Cypress customizados e sobrescritas globais:
  - Arquivo <i>commands.js</i> para comandos específicos
  - Arquivo <i>index.js</i> responsável por receber as importações dos comandos Cypress
- **node_modules:** arquivos ou diretórios que podem ser carregados pelo NodeJS
- **cypress.json:** arquivo de configuração dos Cypress
- **package-lock.json:** gerado automaticamente com as instalações e atualizações de pacotes

---

## Iniciando um novo projeto

- Crie uma pasta, acesse a pasta via linha de comando, digite `npm init -y`
- Abra o projeto no VS Code, com o comando `code .`
- Instale o Cypress, com o comando `npm install cypress --save-dev`
- Altere a linha `test` no arquivo *package.json*, para que fique da seguinte forma:
```javascript
  "scripts": {
    "test": "npx cypress open"
  },
```
- Digite o comando `npm run test` para inicializar o painel do Cypress pela primeira vez, e criar a estrutura do Cypress no nosso projeto.

Acesse *cypress > integration*, apague as pastas *1-getting-started* e *2-advanced-examples*. São pastas exemplo, não utilizaremos nos projetos.

## Primeiro script
Dentro da pasta *integration*, crie o arquivo *home.spec.js*, contendo o comando `cy:viewport` para configurar a resolução da janela exibida dentro do painel do Cypress, e `cy:visit` para acessar a URL:
```javascript
describe ('Home page', () => {
	it('app deve estar online', () => {
		cy.viewport(1920, 1080)
		cy.visit('https://buger-eats.vercel.app/')
	})
})
```

## Não use o Selector Playground
O *selector playground* do Cypress facilita bastante na busca pelos elementos, mas muitas vezes o caminho apresentado por ele não é uma das melhores formas de busca. O ideal é analisar a estrutura HTML da página para elaborar manualmente uma busca pelo 'pai' ou 'avô' do elemento, fazendo com que fique mais assertivo.

## Funções no Javascript
`function()` é uma palavra reservada no Javascript para criarmos funções de expressão. No Cypress, utilizaremos *arrow function*, representada por `=> ()` ou `=>()` (sem espaço).

Basicamente, as *arrow functions* são **simplificações** para as *functions expression*. Imagine um exemplo onde temos a seguinte função de expressão:
```javascript
const numeroAleatorio = function() {
    return Math.random()
}
```

Tivemos que escrever bastante coisa, para apenas criar uma função que devolve um número aleatório. Agora vamos transformar essa função de expressão em uma *arrow function*:
```javascript
const numeroAleatorio = () => {
    return Math.random()
}
```

*Saiba mais sobre funções no Javascript no [Blog do Matheus Castiglioni](https://blog.matheuscastiglioni.com.br/definindo-funcoes-em-javascript/).*

## Checkpoints na automação web
É recomendado que façamos uma técnica chamada *checkpoint*, para saber se estamos no ponto correto do teste.

Dentro da pasta *integration*, crie o arquivo *cadastro.spec.js*, contendo uma verificação se estamos na página correta após o clique no botão:
```javascript
describe ('Cadastro', () => {
	it('Usuário deve se tornar um entregador', () => {
		cy.viewport(1440, 900)
		cy.visit('https://buger-eats.vercel.app')

		cy.get('a[href="/deliver"]').click()
		cy.get('#page-deliver form h1').should('have.text', 'Cadastre-se para  fazer entregas')
	}
})
```

## Combinando CSS Selector com texto
Utilizando a função `contains` podemos juntar a estratégia de busca pelo locator e também pelo texto desejado. No exemplo abaixo, será efetuada uma busca no seletor (que é do tipo lista) pelo texto da variável, para então saber onde será executada a ação de clique:
```javascript
	var entregador = {
		nome: 'Carol Ciola',
		cpf: '45918628363',
		email: 'carol@email.com',
		whatsapp: '11922223333',
		endereco: {
			cep: '04534011',
			rua: 'Rua Rua Joaquim Floriano',
			numero: '1000',
			complemento: 'Ap 142',
			bairro: 'Itaim Bibi',
			cidade_uf: 'São Paulo'
		},
		metodo_entrega: 'Moto'
	}

	cy.get('input[name="name"]').type(entregador.nome)
	cy.get('input[name="cpf"]').type(entregador.cpf)
	cy.get('input[name="email"]').type(entregador.email)
	cy.get('input[name="whatsapp"]').type(entregador.whatsapp)

	cy.get('input[name="name"]').should('have.value', entregador.nome)
	cy.get('input[name="cpf"]').should('have.value', entregador.cpf)
	cy.get('input[name="email"]').should('have.value', entregador.email)
	cy.get('input[name="whatsapp"]').should('have.value', entregador.whatsapp)

	cy.get('input[name="postalcode"]').type(entregador.endereco.cep)
	cy.get('input[type=button][value="Buscar CEP"]').click()

	cy.get('input[name="address-number"]').type(entregador.endereco.numero)
	cy.get('input[name="address-details"]').type(entregador.endereco.complemento)

	cy.get('input[name="address"]').should('have.value', entregador.endereco.rua)
	cy.get('input[name="district"]').should('have.value', entregador.endereco.bairro)
	cy.get('input[name="city-uf"]').should('have.value', entregador.endereco.cidade_uf)

	cy.contains('.delivery-method li', entregador.metodo_entrega).click()
```

## Upload de arquivos
**Observação:** até a versão 9 do Cypress, não existe nenhum recurso nativo para efetuar upload de arquivos. Utilizaremos um pacote npm para realizar o upload:
`npm install cypress-file-upload --save-dev`

Na pasta *cypress > support*, temos os arquivos:
- *index.js* é um arquivo principal, em que qualquer comando informado nele executa antes do script de teste (suite ou caso de teste).
- *commands.js* nos permite criar comandos personalizados.

No *index.js*, digite abaixo do comando `import '/.commands'` a configuração para que o recurso de upload seja importado (o Cypress não faz isso nativamente):
```javascript
import '/.commands'
import 'cypress-file-upload'
```

Para o teste, tenha um arquivo no formato *.jpg*. Crie uma nova pasta *fixtures > images*, e mova o arquivo para a nova pasta. A pasta *fixtures* é onde definimos uma massa de dados estática.

Em *integration > cadastro.spec.js*, declare o nome do arquivo em uma variável:
```javascript
//[...]
	},
	metodo_entrega: 'Moto',
	arqv: 'nome-do-arquivo.jpg'
}
```

Para importar o arquivo, faremos da seguinte forma:
```javascript
var entregador = {
	nome: 'Carol Ciola',
	cpf: '45918628363',
	email: 'carol@email.com',
	whatsapp: '11922223333',
	endereco: {
		cep: '04534011',
		rua: 'Rua Rua Joaquim Floriano',
		numero: '1000',
		complemento: 'Ap 142',
		bairro: 'Itaim Bibi',
		cidade_uf: 'São Paulo'
	},
	metodo_entrega: 'Moto',
	arqv: 'nome-do-arquivo.jpg'
}

//[...]

cy.contains('.delivery-method li', entregador.metodo_entrega).click()

cy.get('input[accept^="image"]').attachFile('/images/' + entregador.arqv)
```

## Submetendo forms e validando modal
Para concluir o envio do formulário da aplicação, vamos clicar no botão de envio e validar o modal de confirmação exibido:
```javascript
cy.get('form button[type="submit"]').click()

const expectedMessage = 'Recebemos os seus dados. Fique de olho na sua caixa de e-mail, pois e em breve retornaremos o contato.'

cy.get('.swal2-container .swal2-html-container')
	.should('have.text', expectedMessage)
```

## Validando span de alerta
Copie o teste *Usuário deve se tornar um entregador* para um novo bloco, renomeando o teste para *CPF incorreto*.

Na nossa massa de teste, informe duas letras no CPF, tornando-o inválido.
```javascript
// [...]
	var entregador = {
		nome: 'Carol Ciola',
		cpf: '459186283XX',
```

Vamos também apagar o trecho do código que valida o fluxo de submissão do formulário, pois o intuito deste teste é validar o CPF informado:
```javascript
const expectedMessage = 'Recebemos os seus dados. Fique de olho na sua caixa de email, pois e em breve retornamos o contato.'

cy.get('.swal2-container .swal2-html-container')
	.should('have.text', expectedMessage)
```

Ao executar o teste com `npm run test`, após submeter o formulário, o campo CPF apresentará uma mensagem sobre o CPF inválido. Vamos inspecionar o elemento, e incrementar nosso script:
```javascript
cy.get('.alert-error').should('have.text', 'Oops! CPF inválido')
```

## Refatorando a massa de testes
Os nomes dos elementos estão em Inglês. Vamos então refatorar nossa massa de testes, alterando os nomes das variáveis do Português para o Inglês, lembrando também de alterar as chamadas.
```javascript
var deliver = {
	name: 'Carol Ciola',
	cpf: '45918628363',
	email: 'carol@email.com',
	whatsapp: '11922223333',
	address: {
		postalcode: '04534011',
		street: 'Rua Joaquim Floriano',
		number: '1000',
		details: 'Ap 142',
		district: 'Itaim Bibi',
		city_state: 'São Paulo/SP'
	},
	delivery_method: 'Moto',
	arqv: 'nome-do-arquivo.jpg'
}
```

## Padrão de projeto com Page Objects (POM)
Para não termos um projeto com uma manutenção custosa, vamos implementar a estrutura de Page Objects.

Dentro da pasta *cypress*, crie uma nova pasta *pages*. Dentro dela, crie o arquivo *SignupPage.js*, contendo uma classe que leva o mesmo nome, `class SignupPage{}`.

O padrão de escrita de classes no Javascript é o **PascalCase** (primeira letra de cada palavra iniciando em maiúsculo). Nomes de variáveis e funções devem estar no padrão **camelCase** (primeira letra da primeira palavra iniciando em minúsculo, demais iniciando em maiúsculo).

Vamos criar uma função dentro desta classe. Neste caso, não é necessário utilizarmos a palavra reservada `function` nem mesmo *arrow function*, basta declarar os parêntesis e abrir o bloco de chaves:
```javascript
class SignupPage {
    go() {

    }
}
```

A função `go()` vai acessar a página do formulário de cadastro. Recorte as linhas abaixo do arquivo *cadastro.spec.js*, e cole na função:
```javascript
class SignupPage {
    go() {
        cy.viewport(1440, 900)
		cy.visit('https://buger-eats.vercel.app')

		cy.get('a[href="/deliver"]').click()
		cy.get('#page-deliver form h1').should('have.text', 'Cadastre-se para  fazer entregas')
    }
}
```

Vamos criar uma segunda função, que receberá uma massa de teste como argumento: `fillForm(deliver)`. Vamos acrescentar o seguinte código também do arquivo *cadastro.spec.js*:
```javascript
class SignupPage {
//[...]

fillForm(deliver) {
	cy.get('input[name="name"]').type(deliver.name)
	cy.get('input[name="cpf"]').type(deliver.cpf)
	cy.get('input[name="email"]').type(deliver.email)
	cy.get('input[name="whatsapp"]').type(deliver.whatsapp)

	cy.get('input[name="name"]').should('have.value', deliver.name)
	cy.get('input[name="cpf"]').should('have.value', deliver.cpf)
	cy.get('input[name="email"]').should('have.value', deliver.email)
	cy.get('input[name="whatsapp"]').should('have.value', deliver.whatsapp)

	cy.get('input[name="postalcode"]').type(deliver.address.postalcode)
	cy.get('input[type=button][value="Buscar CEP"]').click()

	cy.get('input[name="address-number"]').type(deliver.address.number)
	cy.get('input[name="address-details"]').type(deliver.address.details)

	cy.get('input[name="address"]').should('have.value', deliver.address.street)
	cy.get('input[name="district"]').should('have.value', deliver.address.district)
	cy.get('input[name="city-uf"]').should('have.value', deliver.address.city_state)

	cy.contains('.delivery-method li', deliver.delivery_method).click()

	cy.get('input[accept^="image"]').attachFile('/images/' + deliver.arqv)
	}
}
```

Note que não copiamos para o método `fillForm(deliver)` a linha que clica no botão apra submeter o formulário. Esta linha deverá permanecer no arquivo *cadastro.spec.js*.

Crie uma nova função, `submit()`, incluindo então a linha de código que submete o formulário:
```javascript
//[...]
submit() {
	cy.get('form button[type="submit"]').click()
}
```

Crie uma nova função, `modalContentShouldBe()`, incluindo a linha de código que valida a mensagem do modal, mantendo a variável `expectedMessage` no arquivo *cadastro.spec.js*. Vamos passar a variável como um argumento para a função, ficando `modalContentShouldBe(expectedMessage)`:
```javascript
modalContentShouldBe(expectedMessage) {
	cy.get('.swal2-container .swal2-html-container')
		.should('have.text', expectedMessage)
}
```

Assim, teremos um encapsulamento de todos os steps dentro de cada função.

Ao final da classe, vamos exportar a página para depois importá-la na camada de testes:
```javascript
//[...]
    modalContentShouldBe() {
        cy.get('.swal2-container .swal2-html-container')
            .should('have.text', expectedMessage)
    }
}

export default SignupPage;
```

No início do arquivo *cadastro.spec.js*, inclua a linha para importar nossa classe:
```javascript
import SignupPage from '../pages/SignupPage'

describe ('Cadastro', () => {
//[...]
```

Logo depois da definição da massa de teste, vamos instanciar na variável `signup` a classe `SignupPage()`. Desta forma, a variável terá acesso a todas as funções da classe `SignupPage`.
```javascript
var signup = new SignupPage()
```

Vamos agora declarar as funções:
```javascript
//[...]
		arqv: 'nome-do-arquivo.jpg'
	}

	var signup = new SignupPage()

	signup.go()
	signup.fillForm(deliver)
	signup.submit()

	const expectedMessage = 'Recebemos os seus dados. Fique de olho na sua caixa de email, pois e em breve retornamos o contato.'
	signup.modalContentShouldBe(expectedMessage)
})
```

Para o teste de CPF inválido, criaremos mais uma função, contendo:
```javascript
alertMessageShouldBe(expectedMessage) {
	cy.get('.alert-error').should('have.text', expectedMessage)
}
```

A declaração ficará da seguinte forma:
```javascript
//[...]
            delivery_method: 'Moto',
            arqv: 'nome-do-arquivo.jpg'
        }

        var signup = new SignupPage()

        signup.go()
        signup.fillForm(deliver)
        signup.submit()
        signup.alertMessageShouldBe('Oops! CPF inválido')
    })
})
```

## vieport e baseUrl

### viewport
Execute um script de teste. No canto superior direito da janela do Cypress, temos o ícone "i". Ao clicar nele, temos uma descrição da configuração padrão de `viewport` do Cypress, que é de 100 x 660px.

É possível deixar configurado no Cypress as dimensões do `viewport` que queremos utilizar, ao invés de informar isso no script de teste.

Copie o trecho de código do exemplo, e cole no arquivo *cypress.json* do projeto. Altere para a dimensão que queremos, 1440 x 900px.
```javascript
{
    "viewportWidth": 1440,
    "viewportHeight": 900
}
```

Remova também a linha abaixo, do arquivo *SignupPage.js*:
```javascript
class SignupPage {
    go() {
        cy.viewport(1440, 900) //remova esta linha
```

Ao executar o teste novamente, ao lado do ícone "i" da janela do Cypress, será exibido o valor da nossa dimensão configurada.

### baseUrl
Abaixo da configuração da `viewport`, vamos configurar nossa URL base do projeto.
```javascript
{
    "viewportWidth": 1440,
    "viewportHeight": 900,
    "baseUrl": "https://buger-eats.vercel.app"
}
```

No arquivo *SignupPage.js*, deixaremos da seguinte forma:
```javascript
class SignupPage {
   go() {
       cy.visit('/')
```

## Entendendo os ganchos (hooks) do Cypress
Os ganchos (hooks) do Cypress são:

- `before()` executa as instruções sempre antes de todos os casos de teste
- `beforeEach()` executa as instruções sempre antes de cada um dos casos de teste
- `after()` executa as instruções sempre depois da execução de todos os casos de teste
- `afterEach()` executa as instruções sempre depois da execução de cada um dos casos de teste

São declarados após o `describe`. Veremos a implementação de cada um nas próximas aulas.
```javascript
describe ('Cadastro', () => {

    before(function() {
        cy.log('Tudo aqui é executado uma única vez ANTES DE TODOS os casos de teste.')
    })

    beforeEach(function() {
        cy.log('Tudo aqui é executado sempre ANTES DE CADA caso de teste.')
    })

    after(function() {
        cy.log('Tudo aqui é executado uma única vez DEPOIS DE TODOS os casos de teste.')
    })

    afterEach(function() {
        cy.log('Tudo aqui é executado sempre DEPOIS DE CADA caso de teste.')
    })
```

## Export default new Page
Podemos melhorar a implementação da instância da classe, reaproveitando o código. Existem duas formas válidas:

1) Colocando o trecho de código referente à instância dentro do `describe`, tornando-o assim disponível para todos os testes (isso vale para todas as demais classes que queremos instanciar no futuro):
```javascript
import SignupPage from '../pages/SignupPage'
describe ('Cadastro', () => {

	var signup = new SignupPage()

	it('Usuário deve se tornar um entregador', () => {
		//[...]
	})

	it('CPF incorreto', () => {
		//[...]
	})
})
```

2) Exportar já instanciando, indicando o `new` no `export`, e depois informando o nome da variável no `import`:

*SignupPage.js*
```javascript
export default new SignupPage;
```

*cadastro.spec.js*
```javascript
import signup from '../pages/SignupPage'
```

## Trbalhando com Fixtures
Vamos remodelar a massa de teste dos nossos dois cenários. Como temos uma massa de teste com muitos campos, vamos passar a armazená-la na pasta *fixtures*: renomeie o arquivo *example.json* para *deliver.json*, e substitua o conteúdo pela massa de teste, alterando para o formato json. E ainda neste contexto, vamos separar a massa de teste por cenários: `signup` e `cpf_inv`:
```json
{
  "signup": {
    "name": "Carol Ciola",
    "cpf": "45918628363",
    "email": "carol@email.com",
    "whatsapp": "11922223333",
    "address": {
        "postalcode": "04534011",
        "street": "Rua Joaquim Floriano",
        "number": "1000",
        "details": "Ap 142",
        "district": "Itaim Bibi",
        "city_state": "São Paulo/SP"
    },
    "delivery_method": "Moto",
    "arqv": "nome-do-arquivo.jpg"
  },
  "cpf_inv": {
    "name": "Ana Teste",
    "cpf": "459186283XX",
    "email": "ana@email.com",
    "whatsapp": "11988885555",
    "address": {
        "postalcode": "09520070",
        "street": "Rua Amazonas",
        "number": "2000",
        "details": "Ap 200",
        "district": "Centro",
        "city_state": "São Caetano do Sul/SP"
    },
    "delivery_method": "Moto",
    "arqv": "nome-do-arquivo.jpg"
  }
}
```

Apague a massa de dados `var deliver` dos dois testes.

No arquivo *cadastro.spec.js*, para obter as informações do arquivo de fixture, vamos criar um gancho `beforeEach()` após a declaração do `describe`:
```javascript
describe ('Cadastro', () => {

    beforeEach(() => {
        cy.fixture('deliver').then((d) => {
            this.deliver = d
        })
    })
```

O `cy.fixture` faz uma *promise*, onde é necessário informar um `.then` após a declaração. Esta subfunção indica `d` para receber os dados da massa de teste do arquivo `deliver.json`.

**Atenção:** para utilizar a variável de contexto desta forma, ocasionará um erro ao executar o teste. É necessário alterar as arrow functions do `beforeEach()` e também dos testes, substituindo por `function()`.

```javascript
describe ('Cadastro', () => {

    beforeEach(function() {
        cy.fixture('deliver').then((d) => {
            this.deliver = d
        })
    })

	it('Usuário deve se tornar um entregador', function() {
		//[...]
    })

    it('CPF incorreto', function() {
		//[...]
    })
})
```

Nos testes, vamos informar nas functions o contexto para receber a massa de teste:
```javascript
signup.fillForm(this.deliver.signup)
//[...]
signup.fillForm(this.deliver.cpf_inv)
```

## Padrão de nomenclatura (Inglês)
Vamos refatorar a padronização de nomenclatura da suite de Cadastro. É preferível deixar em inglês, pois os nomes ficam mais enxutos, e fica mais fácil de se analisar. No entanto, é um gosto pessoal.

```javascript
describe ('Signup', () => {
//[...]
	it('User should be deliver', function() {
	//[...]
	it('Incorrect document', function() {
	//[...]
```

O arquivo *cadastro.spec.js* será renomeado para *signup.spec.js*.

## O reuso do código é real
**Observação:** a URL da aplicação foi alterada para https://buger-eats-qa.vercel.app. Altere o `baseUrl` na configuração.

Vamos automatizar mais um cenário, desta vez o de e-mail incorreto. Para isso, basta reutilizar o código do teste do CPF incorreto, informando um e-mail incorreto.

*deliver.json*
```javascript
  "email_inv": {
    "name": "José Teste",
    "cpf": "45918628363",
    "email": "jose.com.br",
    "whatsapp": "11966664444",
    "address": {
        "postalcode": "09520070",
        "street": "Rua Amazonas",
        "number": "3000",
        "details": "Ap 300",
        "district": "Centro",
        "city_state": "São Caetano do Sul/SP"
    },
    "delivery_method": "Moto",
    "arqv": "nome-do-arquivo.jpg"
  }
```

*signup.js*
```javascript
    it('Incorrect e-mail', function() {
        signup.go()
        signup.fillForm(this.deliver.email_inv)
        signup.submit()
        signup.alertMessageShouldBe('Oops! Email com formato inválido.')
    })
```

## Factory e Faker com dados e CPFs dinâmicos
Estamos utilizando uma massa de teste estática/fixa via camada de *fixture* nativa do Cypress. Vamos aprender outra forma de se utilizar a massa de teste:

Vamos criar uma nova pasta *cypress > factories*, contendo o arquivo *SignupFactory.js*. O arquivo conterá:
```javascript
export default {
    deliver: function() {
        var data = {
            name: 'Carol Ciola',
            cpf: '45918628363',
            email: 'carol@email.com',
            whatsapp: '11922223333',
            address: {
                postalcode: '04534011',
                street: 'Rua Joaquim Floriano',
                number: '1000',
                details: 'Ap 142',
                district: 'Itaim Bibi',
                city_state: 'São Paulo/SP'
            },
        delivery_method: 'Moto',
        arqv: 'nome-do-arquivo.jpg'
        }

        return data
    }
}
```

No arquivo *signup.spec.js*, vamos incluir o `import` abaixo, importanto o módulo:
```javascript
import signupFactory from '../factories/SignupFactory'
```

E vamos criar dentro dos testes uma variável para importar nossa massa, alterando a função `signup.fillForm(deliver)` e comentando o `beforeEach()`, pois não estamos mais utilizando *fixture*, e sim, *factory*:
```javascript
import signup from '../pages/SignupPage'
import signupFactory from '../factories/SignupFactory'

describe ('Signup', () => {

    // beforeEach(function() {
    //     cy.fixture('deliver').then((d) => {
    //         this.deliver = d
    //     })
    // })

	it('User should be deliver', function() {

        var deliver = signupFactory.deliver()

        signup.go()
        signup.fillForm(deliver)
        signup.submit()
    //[...]
```

`deliver` lê os dados da massa de teste do cenário positivo. Para os testes negativos de CPF e e-mail, vamos criar uma massa inválida dentro do cenário de teste:
```javascript
    it('Incorrect document', function() {

        var deliver = signupFactory.deliver()

        deliver.cpf = '459186283XX'

        signup.go()
        signup.fillForm(deliver)
        //[...]

    it('Incorrect e-mail', function() {

        var deliver = signupFactory.deliver()

        deliver.email = 'jose.com.br'

        signup.go()
        signup.fillForm(deliver)
        //[...]
```

A grande vantagem de utilizarmos o *factory* ao invés do *fixture* neste caso, é termos uma única modelagem de massa de teste. E toda alteração de massa necessária deixamos no próprio cenário de teste.

No entanto, a massa de teste ainda está estática. Feche a interface do Cypress, e execute o seguinte comando: `npm install faker --save-dev faker@5.5.3`. Esta biblioteca nos permite gerar dados dinâmicos para os testes.

**Observação: estamos instalando a versão específica 5.5.3, pois a versão 6.6.6 está com erro atualmente.**

Na primeira linha do arquivo *SignupFactory.js*, vamos importar a biblioteca *faker*:
```javascript
var faker = require('faker')
```

E depois vamos declaras as funções que nos trarão os dados dinâmicos:
```javascript
var firstName = faker.name.firstName()
```

O formato para chamar a variável faker é ``${nomeDaVariavelFaker}``. Se precisarmos chamar mais de uma, basta utilizar espaço:
```javascript
name: `${firstName} ${lastName}`
```

O e-mail utilizará o primeiro nome gerado, e o restante será gerado dinamicamente:
```javascript
email: faker.internet.email(firstName)
```

Para termos CPF dinâmico, vamos instalar: `npm install gerador-validador-cpf --save-dev`.

Vamos importar a biblioteca:
```javascript
var cpf = require('gerador-validador-cpf')
```

E a declaração ficará assim:
```javascript
cpf: cpf.generate(),
```

## Campos obrigatórios
No arquivo *signup.spec.js*, vamos criar um novo teste, contendo:
```javascript
it('Required fields'), () => {
	signup.go()
	signup.submit()

	signup.alertMessageShouldBe('É necessário informar o nome')
	signup.alertMessageShouldBe('É necessário informar o CPF')
	signup.alertMessageShouldBe('É necessário informar o email')
	signup.alertMessageShouldBe('É necessário informar o CEP')
	signup.alertMessageShouldBe('É necessário informar o número do endereço')
	signup.alertMessageShouldBe('Selecione o método de entrega')
	signup.alertMessageShouldBe('Adicione uma foto da sua CNH')
}
```

No entanto, se deixarmos desta forma, a função `alertMessageShouldBe` constante no arquivo *SignupPage.js* verifica apenas um elemento `.alert-error`, e na página queremos fazer a asserção em sete elementos diferentes, que possuem este mesmo locator.

Para resolver isso, vamos ajustar nossa função para:
```javascript
alertMessageShouldBe(expectedMessage) {
	//cy.get('.alert-error').should('have.text', expectedMessage)
	cy.contains('.alert-error', expectedMessage).should('be.visible')
}
```

Com isso, temos uma verificação que não dá problema de ambiguidade por conta do seletor, pois estamos validadno o texto também.

## Execução de teste Step by Step x dinâmico
Todo e qualquer framework de teste faz uma execução procedural, ou seja, execução step by step. Se por ventura um step falar, os próximos testes serão abortados.

Vamos criar um contexto, substituindo o teste `it('Required fields')` anterior:
```javascript
context('Required fields', function() {
	const messages = [
		{field: 'name', output: 'É necessário informar o nome'},
		{field: 'cpf', output: 'É necessário informar o CPF'},
		{field: 'email', output: 'É necessário informar o e-mail'}, //erro intencional, palavra com hífen
		{field: 'postalcode', output: 'É necessário informar o CEP'},
		{field: 'number', output: 'É necessário informar o número do endereço'},
		{field: 'delivery_method', output: 'Selecione o método de entrega'},
		{field: 'cnh', output: 'Adicione uma foto da sua CNH'}
	]

	before(function() {
		signup.go()
		signup.submit()
	})

	messages.forEach(function(msg) {
		it(`${msg.field} is required`, function() {
			signup.alertMessageShouldBe(msg.output)
		})
	})
})
```

Ao executarmos o teste desta forma, o teste vai falhar, porém o script continuará a execução dos demais testes. Isso fará com que o teste fique dinâmico, não abortando a execução do script todo se um dos asserts falhar.

## Cypress Run com evidências em vídeos
O comando `npx cypress run` executa todos os testes em modo headless. Com isso, é criada automaticamente a pasta *videos*, contendo arquivos de vídeo para cada um dos testes executados.

## Screenshots quando um teste falha
Ao executar os testes em modo headless, o Cypress sempre captura automaticamente um screenshot quando encontra uma falha na execução e gera a pasta *screenshots*.

Ao executar novamente o teste em modo headless, caso o teste passe, o conteúdo da pasta *screenshots* é deletado, e a pasta é mantida.

## Executando em outros navegadores
O **Electron** é uma webview muito utilizada por desenvolvedores para fazer implementação de aplicações desktop embarcadas em HTML, Javascript e CSS. Um exemplo disso é o VS Code e o Slack.

Os desenvolvedores do Cypress optaram por ter o Electron como navegador padrão nas execuções em modo headless.

Se quiser excolher um navegador específico, basta informar por parâmetro no momento da execução:

- `npx cypress run -b chrome`
- `npx cypress run -b firefox`
- `npx cypress run -b edge`

Lembrando que você deve possuir a **versão mais atual do Edge** para conseguir executar o Cypress, ele não funciona nas versões mais antigas.

## Git na prática
O Git é uma ferramenta para efetuar controle versionamento de código. O GitHub é um hub de armazenamento de repositórios.

Acessando a pasta do projeto via linha de comando, digite o comando `git init`. Este comando inicializa um repositório Git para termos controle de versão do projeto. Note que a linha de comando exibirá a brnach `master`.

No VS Code, as pastas com a cor de fonte em verde indicam "untracked", ou seja, a pasta é um repositório Git, e os arquivos ainda não foram adicionados ao repositório.

Descrevemos os nomes dos arquivos e pastas que não devem listados para inclusão no repositório (ou seja, serão ignorados). Este arquivo deve ser criado na raiz do projeto.

Ao clicar no ícone de Source Control (Controle de Versão), são listados os arquivos alterados que ainda não foram adicionados ao repositório. Estes arquivos possuem um "U" à direta, indicam que estão "untracked".

Clique no "+" para selecionar os arquivos que vamos adicionar ao projeto.

No prompt, o comando `git status` lista os arquivos que serão adicionados ao projeto.

O comando `git add .` confirma e adiciona no projeto **todos** os arquivos listados pelo `git status`.

O comando `git commit -m "mensagem do commit"` efetiva a alteração, e nele informamos um comentário sobre o commit.

Caso o Git exiba a mensagem *Aythor identity unknown*, é necessário executar os comandos para criação do usuário e nome:

`git config --global user.email "you@example.com"`
`git config --global user.name "Seu Nome"`

O comando `git push` confirma a ação do commit.

O comando `git pull` baixa a versão constante no repositório remoto para o repositório local.

No GitHub, crie um novo repositório.

No Git, a branch principal é a `master`. O GitHub trabalha com a branch `main`como sendo a branch principal. É a mesma coisa, somente muda o rótulo. Execute o comando `git branch -M main` para criar a branch `main`.

git remote add origin https://github.com/cciola/cypress-test-web.git
git push -u origin main


## Masterclasses complementares
Links das masterclasses complementares do curso:

- [Masterclass #1](https://www.youtube.com/watch?v=UfGROGLyqZ0&list=LL&index=1) (20/01/2021)
- [Masterclass #2](https://www.youtube.com/watch?v=4lkbUVmanpg&list=PLn2i8I7W73irYXZJvjLxAU1179dzHbvAB&index=1) (27/01/2021)


## Dicas
- *npm* significa Node Package Manager, ou gerenciador de pacotes do NodeJS.

- Comunidade de Cypress no Telegram: https://t.me/joinchat/J2lmnhiUztr7_Hjes40Wgw