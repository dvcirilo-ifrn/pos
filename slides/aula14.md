---
theme: default
size: 4:3
marp: true
paginate: true
_paginate: false
title: Aula 14: Clientes JavaScript
author: Diego Cirilo

---
<style>
img {
  display: block;
  margin: 0 auto;
}
</style>

# <!-- fit --> Programação Orientada a Serviços

### Prof. Diego Cirilo

**Aula 14**: Clientes JavaScript

---
# Introdução
- Usualmente as funcionalidades de um sistema web estão no servidor
- O servidor recebe as requisições, processa/acessa dados e *monta* o HTML
- As páginas HTML são enviadas *prontas* para o cliente (navegador)
- Depois de enviado ao cliente, o servidor não tem mais controle sobre a página
- *Front-end* - interface gráfica do usuário
- Como *desacoplar* a interface de usuário do servidor?

---
# JavaScript

- Linguagem *interpretada*, com tipagem dinâmica e multi-paradigma
- Desenvolvida nos anos 90 para dinamizar páginas web
- Permite alterar o conteúdo da página no lado do cliente
- É executada por uma *engine* no navegador
- Em meados dos anos 2000 surgiram os *runtimes* nativos, como o Node.js

---
<style scoped>section { font-size: 26px; }</style>
# Runtimes do JS

- Node.js
    - Ambiente de execução de JavaScript *server-side*.
    - Utiliza a *engine* V8 do Chrome.
    - Oferece APIs para acessar o sistema de arquivos, redes e outras funcionalidades do servidor.
- Browser Engines
    - V8 (Chrome, Edge), SpiderMonkey (Firefox), JavaScriptCore (Safari).
    - Executam JavaScript diretamente nos navegadores, oferecendo suporte para aplicações web interativas.

---
# Declarações
- Variáveis podem ser declaradas com:
    - Automaticamente (não recomendado)
    - `var` - Escopo global com *hoisting*.
    - `let` - Variável com escopo de bloco.
    - `const` - Constantes, o valor/tipo não pode mudar.

---
# *Hoisting*
- Joga as declarações automaticamente para o topo do script.
- Permite usar variáveis/funções que ainda serão declaradas.
- Funciona com `var` e declaração de funções.
- Pode ser uma fonte de *bugs* se não for tratado com cuidado.

---
# Tipo de dados
- O JavaScript tem tipagem *dinâmica* e *fraca*.
- `var` e `let` podem receber tipos de dados diferentes
- Tipos primitivos:
    - String, Number, Bigint, Boolean, Undefined, Null, Symbol
- O resto é objeto (*Object*)

---
# Objetos JS
- JavaScript Object Notation
```js
const bejeto = {
    nome: "Ana",
    idade: 20,
    profissao: "Desenvolvedora",
    saudacao: function() {
        return `Olá, meu nome é ${this.nome}.`;
    }
};
```

---
# Strings
- Podem ser delimitadas com:
    - `` `string` ``
    - `"string"`
    - `'string'`
- O `` ` `` é chamado de *string literal* e permite interpolação, múltiplas linhas, etc.
```js
const cor = "azul";
const informacao = `O display é ${cor}.`;
```

---
# Manipulação da DOM
- *Document Object Model*
- Uma das principais funções do JS é manipular a DOM
- Criar/remover elementos, substituir conteúdo, alterar atributos, etc

---
# Manipulação da DOM
- Selecionar elementos:
    - `document.getElementById('id')`
    - `document.querySelector('.classe')`
    - `document.querySelectorAll('tag')`

- Modificar Conteúdo
    - `element.textContent = 'Novo texto'`
    - `element.innerHTML = '<p>Novo HTML</p>'`

---
# Manipulação da DOM
- Alterar Estilos
    - `element.style.color = 'red'`
    - `element.classList.add('nova-classe')`
    - `element.classList.remove('classe-existente')`

- Criar e Inserir Elementos
    - `document.createElement('div')`
    - `parentElement.appendChild(novoElemento)`

---
# Manipulação da DOM
- Exemplo:
```js
const paragrafo = document.createElement('p');
paragrafo.textContent = 'Este é um novo parágrafo.';
document.body.appendChild(paragrafo);
```

---
# Eventos JS
- Os eventos reagem a ações do usuário, servidor ou temporizadas
- Permitem a execução de funções quando algo acontece
- Ex. `click`, `mouseover`, etc.
```js
elemento.addEventListener('click', function() {
    alert('Elemento clicado!');
});
```

---
<style scoped>section { font-size: 24px; }</style>
# Funções no JS
- Funções padrão:
```js
function somar(a, b) {
    return a + b;
}
```

- Funções anônimas:
```js
const saudacao = function(nome) {
    return `Olá, ${nome}!`;
};
```

- *Arrow functions*
```js
const multiplicar = (a, b) => {
    const resultado = a * b;
    return resultado;
};
```

---
# *Arrow functions*
- Retornam o valor por padrão
```js
hello = () => "Hello World!";
```

- Se houver apenas um parâmetro
```js
hello = val => "Hello " + val;
```

---
# *Promises*
- Algumas operações não devem bloquear a execução do código
- Esse é o princípio de operações *assíncronas*
- O JavaScript pode retornar *promessas* em uma função que pode demorar
- O código continua sua execução.

---
# *Promises*
- Uma Promise pode estar em um dos três estados:
    - Pendente (*pending*): Estado inicial, ainda não resolvida ou rejeitada.
    - Resolvida (*fulfilled*): A operação foi completada com sucesso.
    - Rejeitada (*rejected*): A operação falhou.

---
# *Promises*
- Podemos consumir as promessas com:
    - `.then(callback)` - função executada se der certo
    - `.catch(callback)` - função executada se falhar

---
# *Promises*
```js
promessa
    .then(resultado => {
        console.log(resultado);  // "Operação bem-sucedida!"
    })
    .catch(erro => {
        console.error(erro);  // "Falha na operação."
    });
```

---
# Acessando os Dados (.then)
- Relembrando: o valor resolvido só está disponível **dentro do callback** passado para `.then()` (ou encadeando outro `.then()`).

```js
// Errado: data não existe fora do .then()
let data;
fetch('https://jsonplaceholder.typicode.com/todos/1')
    .then(response => response.json())
    .then(result => { data = result; });

console.log(data); // undefined, o fetch ainda não terminou

// Certo: use o valor dentro do callback
fetch('https://jsonplaceholder.typicode.com/todos/1')
    .then(response => response.json())
    .then(data => console.log(data)); // objeto com os dados
```

---
# *Async/Await*
- No ES8 surge a sintaxe de *async* e *await* para Promises
- Torna o código um pouco mais legível
- As funções assíncronas são declaradas com *async* e a chamada de funções assíncronas com *await*
```js
async function minhaFuncaoAssincrona() {
    try {
        const resultado = await promessa;
        console.log(resultado);  // "Operação bem-sucedida!"
    } catch (erro) {
        console.error(erro);  // "Erro na operação."
    }
}

minhaFuncaoAssincrona();
```


---
# Fetch API
- A Fetch API é usada para fazer requisições HTTP no navegador.
- Substitui o antigo XMLHttpRequest
- Retorna uma *Promise*

---
# Exemplo (async/await)
- `await` fora de uma função só funciona no console do navegador ou em um *módulo* (`<script type="module">`).

```js
async function buscarDados() {
    try {
        const response = await fetch('https://jsonplaceholder.typicode.com/todos/1');
        if (!response.ok) {
            throw new Error('Erro: ' + response.status);
        }
        return await response.json();
    } catch (error) {
        console.error('Erro:', error);
    }
}

const data = await buscarDados();
console.log(data);
```

---
# Exemplo (async/await)
- Outras opções (ex. `PUT`, `headers`, `body`):
```js
async function atualizarUsuario() {
    try {
        const response = await fetch('https://api.exemplo.com/usuario/1', {
            method: 'PUT',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({ nome: 'Maria', idade: 28 })
        });
        return await response.json();
    } catch (error) {
        console.error('Erro:', error);
    }
}

const data = await atualizarUsuario();
console.log('Atualizado:', data);
```

---
# Acessando os Dados (async/await)
- `data` só existe **dentro** da função `async`.
- A função retorna uma *Promise*, então usar o resultado fora dela sem `await` não funciona.

```js
// Errado: data não existe fora da função assíncrona
async function buscarDados() {
    const response = await fetch('https://jsonplaceholder.typicode.com/todos/1');
    return await response.json();
}

const data = buscarDados();
console.log(data); // Promise { <pending> }
```

---
# Acessando os Dados (async/await)
- Para acessar o valor fora, é preciso usar `await` também na chamada, dentro de outra função `async`.

```js
// Certo
async function main() {
    const data = await buscarDados();
    console.log(data); // objeto com os dados
}

main();
```

---
# Módulos

- Permitem a melhor organização do código
- Reuso e escopo
- Implementados no ES6
- *export* e *import*

---
# ESM
```js
// Exportando (arquivo myModule.js)
export const myFunction = () => { ... };

// Importando
import { myFunction } from './myModule.js';
```

---
# Bundling

- Processo de combinar múltiplos arquivos JS em um único arquivo
- Necessário devido à divisão de código em múltiplos módulos
- Benefícios:
    - Reduz o número de requisições HTTP
    - Melhora o desempenho do site
    - Facilita a minificação e otimização do código
- Exemplos:
    - Webpack, Rollup, Parcel

---
# Bundling

- Módulos são combinados em um único arquivo (ou múltiplos, dependendo da configuração)
- Ferramentas de bundling resolvem dependências e geram um arquivo otimizado
- Exemplo:
    - Arquivos de entrada: main.js, utils.js, app.js
    - Saída: bundle.js

---
# Vite.js
- Ferramenta de desenvolvimento de front-end
- Funciona como bundler e servidor de desenvolvimento
- *Lembra* o que o `django-admin` faz no back-end
- Utiliza o *Rollup* para bundling
- [Documentação](https://vite.dev/)

---
<style scoped>section { font-size: 26px; }</style>

# Comandos *Vite*
- Criar um projeto *vanilla* na pasta atual
```
npm create vite@latest . -- --template vanilla
```
- Rodar o servidor de desenvolvimento
```
npx vite dev
```
- Fazer a *build* (*bundling*)
```
npx vite build
```
- Testar a *build* (arquivos estáticos em `dist/`)
```
npx vite preview
```
---
<style scoped>section { font-size: 26px; }</style>

# Projeto 01
- Crie um fork do repositório disponibilizado no GSA.
- Inicialize um projeto Vite.js com o template *vanilla* na raiz do repositório.
- Desenvolva a interface e crie um cliente web para uma API aberta usando a estrutura do template JS Vanilla do Vite.js.
- O cliente deve listar mais de um nível de informações, ex. usuários e to-dos do usuário, fabricante e modelos e veículos.
- Separe o .js que se comunica com a API em um *wrapper* e o .js que manipula a DOM usando módulos
- Exemplos de APIs abertas:
    - [JSON Placeholder](https://jsonplaceholder.typicode.com/), [PokeAPI](https://pokeapi.co/), [SWAPI](https://swapi.dev/),  [Tabela FIPE](https://deividfortuna.github.io/fipe/), etc.

---
# Referências
- https://javascript.info/
- https://developer.mozilla.org/pt-BR/docs/Web/JavaScript
- https://developer.mozilla.org/pt-BR/docs/Learn/JavaScript
- https://vite.dev/

---
# <!--fit--> Dúvidas? 🤔
