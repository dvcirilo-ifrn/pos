---
theme: default
size: 4:3
marp: true
paginate: true
_paginate: false
title: Aula 15: React
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

**Aula 15**: React

---
# React JS

- https://nextjs.org/learn/react-foundations
- https://react.dev/learn

![bg right w:400px](../img/React-icon.svg)

---

# React

- **Biblioteca** JavaScript;
- Criada para construir interfaces de usuário;
- Eficiente, modular e reativa.

---

# React

- Criado em 2011 pelo engenheiro Jordan Walke, do Facebook.  
- Lançado publicamente em 2013;
- Visa resolver problemas de performance e complexidade na construção de interfaces dinâmicas.  
- Uma das bibliotecas mais populares para *web*.

---

# Características Principais do React

- Baseado em componentes reutilizáveis  
- Renderização declarativa  
- Utiliza o Virtual DOM para otimizar atualizações na interface  
- Compatível com diferentes frameworks e bibliotecas  
- Pode ser usado para aplicações web, mobile (React Native) e desktop (Electron)

---

# JavaScript Vanilla

- Manipulação direta do DOM;
- Cada mudança exige:
    - selecionar elementos
    - alterar atributos
    - atualizar manualmente a interface.

---

# JavaScript Vanilla

```html
<div id="app"></div>
<script>
  const app = document.getElementById('app')
  const button = document.createElement('button')
  button.textContent = 'Clique aqui'
  button.onclick = () => {
    const p = document.createElement('p')
    p.textContent = 'Você clicou no botão!'
    app.appendChild(p)
  }
  app.appendChild(button)
</script>
```

---

# Problemas do Modo Tradicional

- Código extenso e repetitivo  
- Dificuldade de manutenção  
- Atualizações manuais no DOM  
- Estado da aplicação espalhado em várias partes do código  
- Dificuldade para testar e reaproveitar componentes

---

# Construção Imperativa

- Descrição passo a passo o que deve ser feito para atualizar a interface.  
- A lógica é voltada ao "como" fazer.
- Exemplo:

```javascript
button.onclick = () => {
  const p = document.createElement('p')
  p.textContent = 'Você clicou!'
  app.appendChild(p)
}
```

---

# Construção Declarativa

- Descrição do que deseja ver na interface, e o React cuida do "como" atualizar.  
- A interface é uma função do estado.
- Exemplo:

```jsx
function App() {
  const [count, setCount] = React.useState(0)
  return (
    <div>
      <p>Você clicou {count} vezes</p>
      <button onClick={() => setCount(count + 1)}>Clique</button>
    </div>
  )
}
```

---

# Imperativo vs. Declarativo

- Imperativo: o foco está em manipular o DOM diretamente.  
- Declarativo: o foco está no estado e no resultado desejado.  
- React é declarativo e reativo, atualizando automaticamente a interface conforme o estado muda.

---

# Conceito de Componentes

- Unidades independentes e reutilizáveis que compõem a interface.  
- Podem ser funções ou classes que retornam elementos visuais.
- Podem ser combinados para formar interfaces complexas.
- Exemplo:

```jsx
function Saudacao(props) {
  return <h1>Olá, {props.nome}!</h1>
}
```

---

# Benefícios dos Componentes

- Reutilização de código  
- Facilidade de manutenção  
- Isolamento de responsabilidades  
- Melhor testabilidade  
- Separação entre lógica e apresentação


---

# JSX

- JSX (JavaScript XML)
- Extensão da linguagem JavaScript
- Usada pelo React para descrever a interface do usuário.
- Permite escrever código que se parece com HTML dentro do JavaScript
- Exemplo:

```jsx
const element = <h1>Olá, mundo!</h1>
```

---

# JSX

- O JSX é transformado em código JavaScript puro pelo compilador (como Babel ou o próprio Vite).
- O resultado é um objeto JavaScript que descreve o elemento e que será renderizado pelo React no DOM.
- Exemplo equivalente:

```jsx
const element = React.createElement('h1', null, 'Olá, mundo!')
```

---

# Expressões em JSX

- Dentro de JSX é possível usar expressões JavaScript entre chaves `{}`.

```jsx
const nome = 'Diego'
const element = <h1>Olá, {nome}</h1>
```

- Podem ser usadas funções, cálculos, condicionais e chamadas de métodos:

```jsx
const element = <p>O dobro de 4 é {2 * 4}</p>
```

---

# Atributos em JSX

- Seguem a convenção camelCase em vez dos nomes em minúsculas do HTML.

```jsx
const element = <img src="foto.jpg" alt="Descrição" className="foto" />
```

- Diferenças importantes:
    - `class` vira `className`
    - `for` vira `htmlFor`
    - propriedades podem receber valores JavaScript entre `{}`

```jsx
const element = <input type="text" value={nome} onChange={atualizarNome} />
```

---

# Componentes em JSX

- Funções ou classes que retornam elementos JSX.

```jsx
function Saudacao() {
  return <h1>Bem-vindo!</h1>
}
```

- Podem ser compostos:

```jsx
function App() {
  return (
    <div>
      <Saudacao />
      <p>Essa é a página inicial</p>
    </div>
  )
}
```

---

# Componentes em JSX

- Deve retornar **apenas um elemento**.

```jsx
return (
  <>
    <h1>Título</h1>
    <p>Texto</p>
  </>
)
```

---

# Componentes em JSX

- Elementos devem ser sempre fechados.

```jsx
<img src="foto.jpg" />
<input type="text" />
```


---

# Componentes com Props

- Props (propriedades) são argumentos passados aos componentes.

```jsx
function Saudacao(props) {
  return <h1>Olá, {props.nome}</h1>
}

function App() {
  return <Saudacao nome="Ana" />
}
```

- Também podem ser desestruturadas:

```jsx
function Saudacao({ nome }) {
  return <h1>Olá, {nome}</h1>
}
```

---

# Condicionais em JSX

- Podemos usar operadores condicionais dentro do JSX.

```jsx
function Mensagem({ logado }) {
  return <p>{logado ? 'Bem-vindo!' : 'Faça login'}</p>
}
```

- Ou criar uma variável antes do retorno:

```jsx
function App() {
  let conteudo
  if (logado) conteudo = <p>Bem-vindo!</p>
  else conteudo = <p>Faça login</p>

  return <div>{conteudo}</div>
}
```

---

# Listas em JSX

- Podemos renderizar listas usando `map`.

```jsx
function Lista({ itens }) {
  return (
    <ul>
      {itens.map(item => (
        <li key={item.id}>{item.nome}</li>
      ))}
    </ul>
  )
}
```

- O atributo `key` é obrigatório para ajudar o React a identificar cada item da lista.

---

# Fragmentos

- Fragmentos (`<></>`) permitem agrupar múltiplos elementos sem criar uma nova tag no DOM.

```jsx
function App() {
  return (
    <>
      <h1>Título</h1>
      <p>Texto de exemplo</p>
    </>
  )
}
```

---

# Comentários em JSX

- Comentários dentro de JSX devem ser colocados entre chaves e dentro de `{/* ... */}`.

```jsx
function App() {
  return (
    <div>
      {/* Comentário JSX */}
      <p>Olá!</p>
    </div>
  )
}
```

---
<style scoped>section { font-size: 24px; }</style>

# Importação e Exportação de Componentes

- Cada componente pode ser exportado e importado em outros arquivos.

```jsx
// Saudacao.jsx
export function Saudacao() {
  return <h1>Oi!</h1>
}

// App.jsx
import { Saudacao } from './Saudacao'
```

- Para exportar um único componente como padrão:

```jsx
export default function App() {
  return <h1>Aplicação</h1>
}
```

<!--
---

# Início do Cliente React para a API JSONPlaceholder

Vamos construir um cliente simples para consumir a API JSONPlaceholder, que simula recursos como posts e usuários.

---

# Etapa 1: Criando o Projeto

Crie o projeto com Vite:

```bash
npm create vite@latest cliente-react
cd cliente-react
npm install
npm run dev
```

---

# Etapa 2: Estrutura do Projeto

O projeto conterá:

- src/api para comunicação com a API  
- src/components para os componentes visuais  
- src/App.jsx para a aplicação principal

---

# Etapa 3: Cliente da API

Crie o arquivo src/api/client.js

```javascript
const BASE_URL = 'https://jsonplaceholder.typicode.com'

export async function getPosts() {
  const res = await fetch(`${BASE_URL}/posts`)
  return res.json()
}
```

---

# Etapa 4: Componente de Lista de Posts

Crie o arquivo src/components/PostList.jsx

```jsx
import { useEffect, useState } from 'react'
import { getPosts } from '../api/client'

export function PostList() {
  const [posts, setPosts] = useState([])

  useEffect(() => {
    getPosts().then(setPosts)
  }, [])

  return (
    <div>
      <h2>Posts</h2>
      <ul>
        {posts.slice(0, 10).map(post => (
          <li key={post.id}>{post.title}</li>
        ))}
      </ul>
    </div>
  )
}
```

---

# Etapa 5: Integrando no App

Edite src/App.jsx

```jsx
import { PostList } from './components/PostList'

function App() {
  return (
    <div>
      <h1>Cliente JSONPlaceholder</h1>
      <PostList />
    </div>
  )
}

export default App
```

---

# Etapa 6: Estilizando com MUI

Instale MUI:

```bash
npm install @mui/material @emotion/react @emotion/styled
```

Modifique o componente PostList:

```jsx
import { useEffect, useState } from 'react'
import { getPosts } from '../api/client'
import { Card, CardContent, Typography } from '@mui/material'

export function PostList() {
  const [posts, setPosts] = useState([])

  useEffect(() => {
    getPosts().then(setPosts)
  }, [])

  return (
    <>
      <Typography variant="h4">Posts</Typography>
      {posts.slice(0, 10).map(post => (
        <Card key={post.id} sx={{ margin: 2 }}>
          <CardContent>
            <Typography variant="h6">{post.title}</Typography>
            <Typography variant="body2">{post.body}</Typography>
          </CardContent>
        </Card>
      ))}
    </>
  )
}
```
-->

---

# Bibliotecas de Componentes Populares

- O React permite integrar bibliotecas de componentes prontos para acelerar o desenvolvimento.
- Exemplos:
    - [Bootstrap]()
    - [Material UI (MUI)]()
    - [shadcn/ui]()

---

# Bootstrap para React

```bash
npm install react-bootstrap bootstrap
```

```jsx
import { Button } from 'react-bootstrap'

function App() {
  return <Button variant="primary">Clique</Button>
}
```

---

# Material UI (MUI)

- Baseada no *Material Design* do Google.  

```bash
npm install @mui/material @emotion/react @emotion/styled
```

```jsx
import Button from '@mui/material/Button'

function App() {
  return <Button variant="contained">Clique</Button>
}
```

---

# shadcn/ui

- Construída sobre o Tailwind CSS.  
- Os componentes são copiados para o projeto, permitindo customização completa.

```bash
npx shadcn-ui@latest init
```

---
# Projeto React com Vite

- Para criar o projeto:

```bash
npm create vite@latest meu-projeto -- --template react
cd meu-projeto
npm install
npm run dev
```

---

# Estrutura Padrão do Projeto

```
meu-projeto/
├── index.html
├── package.json
├── vite.config.js
└── src/
    ├── main.jsx
    ├── App.jsx
    └── assets/
```

- **index.html**: ponto de entrada HTML
- **main.jsx**: inicializa o React e renderiza o App
- **App.jsx**: componente principal
- **assets/**: imagens e estilos

---

# Arquivo index.html

- O React será renderizado dentro da div com id="root".

```html
<!DOCTYPE html>
<html lang="pt-BR">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>React com Vite</title>
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/main.jsx"></script>
  </body>
</html>
```


---

# Arquivo main.jsx

- Inicializa a aplicação e renderiza o componente principal `App`.

```jsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App'

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
)
```

---

# Arquivo App.jsx

```jsx
function App() {
  return (
    <div>
      <h1>Olá, React!</h1>
      <p>Meu primeiro app com Vite</p>
    </div>
  )
}

export default App
```

---

# Estilos e Imagens

Você pode importar arquivos CSS diretamente dentro de componentes:

```jsx
import './App.css'
```

E usar imagens:

```jsx
import logo from './assets/logo.png'

function Header() {
  return <img src={logo} alt="Logo" />
}
```

---

# Estado no React

- Representa dados dinâmicos que determinam o comportamento e a aparência de um componente.  
- Sempre que o estado muda, o React atualiza automaticamente a interface.

---

# Estado no React
- Exemplo: (`count` é o estado do componente e `setCount` é a função que o altera):

```jsx
import { useState } from 'react'

function Contador() {
  const [count, setCount] = useState(0)

  return (
    <div>
      <p>Você clicou {count} vezes</p>
      <button onClick={() => setCount(count + 1)}>Clique</button>
    </div>
  )
}
```

---

# Como o Estado Funciona

- Cada componente pode ter seu próprio estado.  
- O React mantém o estado entre renderizações.  
- Quando o estado muda, o componente é renderizado novamente.  
- O estado é isolado: alterar o estado em um componente não afeta outro, a menos que seja compartilhado.

---

# Hooks e useState

- O [hook](https://pt-br.react.dev/reference/react/hooks) `useState` é usado para criar e gerenciar estado em componentes funcionais.  
- O React garante que o valor da variável seja preservado entre renderizações.
- Sintaxe:

```jsx
const [variavel, setVariavel] = useState(valorInicial)
```

---

# Estado e Eventos

- Costuma ser atualizado em resposta a eventos;
- Cliques, digitação ou resultados de requisições.

```jsx
function InputNome() {
  const [nome, setNome] = useState('')

  return (
    <div>
      <input value={nome} onChange={(e) => setNome(e.target.value)} />
      <p>Olá, {nome}</p>
    </div>
  )
}
```

---

# Atualizações de Estado

- As atualizações são assíncronas e podem ser agrupadas.  
- Nunca se deve modificar o estado diretamente.
- Errado:

```jsx
count = count + 1
```

- Certo:

```jsx
setCount(count + 1)
```

---

# Estado Derivado

- Evite calcular valores diretamente no estado quando podem ser derivados de outros valores.
- Errado:

```jsx
const [dobro, setDobro] = useState(count * 2)
```
- Certo:

```jsx
const dobro = count * 2
```

---

# Estado em Componentes Pai e Filho

- O estado pode ser levantado para um componente pai e compartilhado com filhos via props.

```jsx
function Pai() {
  const [mensagem, setMensagem] = useState('Olá')
  return <Filho msg={mensagem} />
}

function Filho({ msg }) {
  return <p>{msg}</p>
}
```

---

# Estado Global

- Quando o estado precisa ser acessado por muitos componentes, ele pode ser movido para um contexto global.

---
<style scoped>section { font-size: 24px; }</style>

# Context API

- A Context API permite criar um estado acessível por toda a árvore de componentes.

```jsx
import { createContext, useContext, useState } from 'react'

const TemaContext = createContext()

export function TemaProvider({ children }) {
  const [tema, setTema] = useState('claro')
  return (
    <TemaContext.Provider value={{ tema, setTema }}>
      {children}
    </TemaContext.Provider>
  )
}

export function useTema() {
  return useContext(TemaContext)
}
```

---

# Usando o Contexto

```jsx
function BotaoTema() {
  const { tema, setTema } = useTema()

  return (
    <button onClick={() => setTema(tema === 'claro' ? 'escuro' : 'claro')}>
      Alternar tema: {tema}
    </button>
  )
}
```

---

# Estado Assíncrono

- O estado pode ser atualizado após operações assíncronas, como chamadas à API.

```jsx
import { useEffect, useState } from 'react'

function Posts() {
  const [posts, setPosts] = useState([])

  useEffect(() => {
    fetch('https://jsonplaceholder.typicode.com/posts')
      .then(res => res.json())
      .then(setPosts)
  }, [])

  return <p>Total de posts: {posts.length}</p>
}
```

---

# Rotas

- Permitem que uma aplicação React tenha múltiplas páginas e URLs diferentes, sem recarregar o navegador.
- O React Router é a biblioteca mais usada para isso.
```bash
npm install react-router-dom
```

---

# Estrutura de Rotas

- O React Router define rotas que associam URLs a componentes.

```jsx
import { BrowserRouter, Routes, Route } from 'react-router-dom'
import Home from './pages/Home'
import Sobre from './pages/Sobre'

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/sobre" element={<Sobre />} />
      </Routes>
    </BrowserRouter>
  )
}
```

---

# Criando Páginas

```jsx
// src/pages/Home.jsx
export default function Home() {
  return <h1>Página Inicial</h1>
}

// src/pages/Sobre.jsx
export default function Sobre() {
  return <h1>Sobre o Projeto</h1>
}
```

---

# Links de Navegação

- Para navegar entre páginas sem recarregar, use o componente Link.

```jsx
import { Link } from 'react-router-dom'

function Menu() {
  return (
    <nav>
      <Link to="/">Início</Link>
      <Link to="/sobre">Sobre</Link>
    </nav>
  )
}
```

---

# Componente de Layout

- Um layout permite incluir elementos comuns em todas as páginas, como cabeçalhos ou rodapés.

```jsx
import { Outlet, Link } from 'react-router-dom'

function Layout() {
  return (
    <div>
      <nav>
        <Link to="/">Início</Link>
        <Link to="/sobre">Sobre</Link>
      </nav>
      <hr />
      <Outlet />
    </div>
  )
}
```

---

# Integrando Layout às Rotas

```jsx
<BrowserRouter>
  <Routes>
    <Route path="/" element={<Layout />}>
      <Route index element={<Home />} />
      <Route path="sobre" element={<Sobre />} />
    </Route>
  </Routes>
</BrowserRouter>
```

---

# Rotas com Parâmetros

- Rotas podem conter parâmetros dinâmicos.

```jsx
<Route path="/posts/:id" element={<Post />} />
```

- No componente:

```jsx
import { useParams } from 'react-router-dom'

function Post() {
  const { id } = useParams()
  return <h2>Post número {id}</h2>
}
```

---

# Navegação Programática

- Para navegar via código, use o hook useNavigate.

```jsx
import { useNavigate } from 'react-router-dom'

function BotaoVoltar() {
  const navigate = useNavigate()
  return <button onClick={() => navigate(-1)}>Voltar</button>
}
```

---

# Tratamento de Rotas Não Encontradas

- Adicione uma rota padrão para exibir uma página 404.

```jsx
<Route path="*" element={<h1>Página não encontrada</h1>} />
```

---
# <!--fit--> Dúvidas? 🤔
