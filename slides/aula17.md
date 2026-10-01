---
theme: default
size: 4:3
marp: true
paginate: true
_paginate: false
title: Aula 17: React Native
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

**Aula 17**: React Native

---
# React Native

- Framework para criar aplicativos nativos (iOS e Android) usando React.
- "Aprenda uma vez, escreva em qualquer lugar": o mesmo JavaScript produz uma interface nativa de verdade, não uma página web dentro de um app.
- [Expo](https://expo.dev/) é a ferramenta mais usada para criar e rodar projetos React Native, parecida com o papel do Vite no React web.

---
# O que Não Muda

- JSX, componentes, props e state funcionam exatamente como no React web.
- `useState`, `useEffect`, Context API, hooks customizados: tudo igual.
- A diferença está no que cada componente *renderiza* e em algumas APIs específicas da plataforma — é isso que esta aula cobre.

---
<style scoped>section { font-size: 24px; }</style>

# Sem DOM, Sem HTML

- Não existe `<div>`, `<p>`, `<img>` ou `<button>`: o React Native compila para *views* nativas, não para HTML.
- Em vez de tags HTML, usa-se componentes importados de `react-native`:

| Web (HTML)       | React Native        |
|-------------------|----------------------|
| `<div>`           | `View`               |
| `<p>`, `<span>`   | `Text`               |
| `<img>`           | `Image`              |
| `<input>`         | `TextInput`          |
| `<button>`        | `Pressable`          |

---

# Componentes Nativos

```jsx
import { Image, Text, View } from 'react-native'

export function Cartao({ titulo, foto }) {
  return (
    <View>
      <Image source={{ uri: foto }} style={{ width: 80, height: 80 }} />
      <Text>{titulo}</Text>
    </View>
  )
}
```

- Todo texto precisa estar dentro de um `Text`: diferente da web, `<View>texto</View>` dá erro.

---
<style scoped>section { font-size: 24px; }</style>

# Estilização com `StyleSheet`

- Não há CSS, classes nem cascata: cada componente recebe um objeto de estilos na prop `style`.
- `StyleSheet.create` organiza esses objetos e otimiza o desempenho.

```jsx
import { StyleSheet, Text, View } from 'react-native'

export function Cartao({ titulo }) {
  return (
    <View style={styles.caixa}>
      <Text style={styles.titulo}>{titulo}</Text>
    </View>
  )
}

const styles = StyleSheet.create({
  caixa: { padding: 16, borderRadius: 8 },
  titulo: { fontSize: 18, fontWeight: 'bold' },
})
```

---

# Combinando Estilos

- A prop `style` também aceita um *array*: os estilos são aplicados em ordem, como uma mesclagem.

```jsx
<Text style={[estilos.secundario, styles.descricao]}>
  Aulas de violão, piano e canto.
</Text>
```

- Útil para combinar um estilo de layout (local) com um estilo de texto compartilhado (ex. cor do tema).

---

# Interação: `Pressable` e `onPress`

- Não existe `onClick`: todo toque usa `onPress`.
- `Pressable` é o componente genérico para qualquer área tocável (o `Button` do React Native Paper já usa `Pressable` por baixo).

```jsx
import { Pressable, Text } from 'react-native'

<Pressable onPress={() => console.log('tocado!')}>
  <Text>Toque aqui</Text>
</Pressable>
```

---
<style scoped>section { font-size: 24px; }</style>

# Formulários: `TextInput` e `onChangeText`

- Não existe elemento `<input>`. O componente `TextInput` chama `onChangeText` já com o texto novo — não um `Event` como no `onChange` da web.

```jsx
// Web
<input value={nome} onChange={e => setNome(e.target.value)} />

// React Native
<TextInput value={nome} onChangeText={setNome} />
```

- O padrão de formulário controlado (estado + função que atualiza) continua o mesmo.

---

# Bibliotecas de Componentes

- Sem CSS, bibliotecas prontas de UI são ainda mais usadas. A mais popular é o [React Native Paper](https://reactnativepaper.com/), com os componentes do Material Design.
- Papel equivalente ao do React-Bootstrap no projeto web: `Button`, `TextInput`, `Card`, `Banner`, `Chip`, `Dialog`...

```jsx
import { Button, TextInput } from 'react-native-paper'

<TextInput mode="outlined" label="E-mail" value={email} onChangeText={setEmail} />
<Button mode="contained" onPress={entrar}>Entrar</Button>
```

---
<style scoped>section { font-size: 24px; }</style>

# Tema do Paper

- As cores, em vez de variáveis Sass do Bootstrap, ficam em um objeto JavaScript, aplicado por um `Provider`.

```jsx
import { PaperProvider, MD3LightTheme } from 'react-native-paper'

const tema = {
  ...MD3LightTheme,
  colors: { ...MD3LightTheme.colors, primary: '#0a2f5f' },
}

export default function App() {
  return (
    <PaperProvider theme={tema}>
      {/* resto do app */}
    </PaperProvider>
  )
}
```

---

# Expo Router

- Em vez de declarar `<Route>` manualmente (React Router), cada arquivo dentro de `src/app/` já é uma rota — como no Next.js.
- `src/app/perfil.jsx` → rota `/perfil`.
- `src/app/aulas/index.jsx` → rota `/aulas`.

---
<style scoped>section { font-size: 24px; }</style>

# Rotas Dinâmicas e Grupos

- Colchetes `[id]` criam uma **rota dinâmica**, como `/posts/:id` no React Router:
    - `src/app/aulas/[id].jsx` → rota `/aulas/123`.
- Parênteses `(nome)` criam um **grupo**: organiza os arquivos em pastas sem aparecer no endereço:
    - `src/app/(app)/(abas)/perfil.jsx` → continua sendo a rota `/perfil`.

---

# Lendo Parâmetros de Rota

- No lugar do `useParams` do React Router, usa-se `useLocalSearchParams`.

```jsx
import { useLocalSearchParams } from 'expo-router'

export default function Aula() {
  const { id } = useLocalSearchParams()
  return <Text>Aula número {id}</Text>
}
```

- Todo parâmetro de rota chega como texto — mesmo se for um número.

---
<style scoped>section { font-size: 24px; }</style>

# Navegando entre Telas

- `Link`, como no React Router, mas a propriedade é `href`:

```jsx
import { Link } from 'expo-router'

<Link href="/login">Entrar</Link>
```

- Para navegar via código (no lugar do `useNavigate`), usa-se o objeto `router`:

```jsx
import { router } from 'expo-router'

router.push('/aulas/123')
router.back()
```

---
<style scoped>section { font-size: 24px; }</style>

# Layouts: `Stack` e `Tabs`

- Um arquivo `_layout.jsx` define como as telas de uma pasta se conectam — substitui o componente `Layout` com `<Outlet>` do React Router.
- `Stack`: telas empilhadas, com cabeçalho e botão de voltar.
- `Tabs`: abas na parte inferior da tela.

```jsx
import { Stack } from 'expo-router'

export default function Layout() {
  return (
    <Stack>
      <Stack.Screen name="index" options={{ title: 'Início' }} />
      <Stack.Screen name="perfil" options={{ title: 'Meu perfil' }} />
    </Stack>
  )
}
```

---
<style scoped>section { font-size: 24px; }</style>

# Rotas Protegidas

- No React web, uma rota protegida é um componente que lê o contexto e decide entre `<Outlet>` ou `<Navigate>`.
- No Expo Router, isso é declarativo: `Stack.Protected`/`Tabs.Protected` com uma condição (`guard`).

```jsx
<Stack.Protected guard={!usuario}>
  <Stack.Screen name="login" />
</Stack.Protected>
<Stack.Protected guard={!!usuario}>
  <Stack.Screen name="(app)" />
</Stack.Protected>
```

- Ao entrar ou sair, o app troca de telas sozinho — sem precisar navegar manualmente.

---
<style scoped>section { font-size: 22px; }</style>
<style scoped>pre { font-size: 16px; }</style>

# Armazenamento Seguro

- Não existe `localStorage` no celular. O [`expo-secure-store`](https://docs.expo.dev/versions/latest/sdk/securestore/) guarda dados de forma criptografada — útil para tokens de autenticação.
- Diferença importante: a API é **assíncrona** (`await`), enquanto o `localStorage` é síncrono.

```jsx
import * as SecureStore from 'expo-secure-store'

await SecureStore.setItemAsync('access', token)
const token = await SecureStore.getItemAsync('access')
await SecureStore.deleteItemAsync('access')
```

---

# Variáveis de Ambiente

- No lugar do prefixo `VITE_` e de `import.meta.env`, o Expo usa o prefixo `EXPO_PUBLIC_` e `process.env`.

```
# .env
EXPO_PUBLIC_API_URL=https://minha-api.com/api
```

```jsx
const API_URL = process.env.EXPO_PUBLIC_API_URL
```

---
<style scoped>section { font-size: 24px; }</style>

# Escolhendo Imagens

- Não existe `<input type="file">`. A escolha de imagem (galeria ou câmera) é feita com o [`expo-image-picker`](https://docs.expo.dev/versions/latest/sdk/imagepicker/).

```jsx
import * as ImagePicker from 'expo-image-picker'

async function escolherImagem() {
  const resultado = await ImagePicker.launchImageLibraryAsync({
    mediaTypes: ['images'],
    quality: 0.7,
  })
  if (!resultado.canceled) {
    return resultado.assets[0] // { uri, fileName, mimeType, ... }
  }
}
```

---

# Enviando a Imagem com `FormData`

- O `FormData` continua existindo, mas o arquivo não é um `File` do navegador: é um objeto `{ uri, name, type }`.

```jsx
const imagem = await escolherImagem()
const dados = new FormData()
dados.append('foto', {
  uri: imagem.uri,
  name: imagem.fileName ?? 'foto.jpg',
  type: imagem.mimeType ?? 'image/jpeg',
})
await fetch(`${API_URL}/auth/eu/`, { method: 'PATCH', body: dados })
```

---
<style scoped>section { font-size: 22px; }</style>
<style scoped>pre { font-size: 16px; }</style>

# Listas Performáticas: `FlatList`

- Mapear uma lista grande com `.map()` dentro de uma `ScrollView` renderiza *todos* os itens de uma vez.
- `FlatList` renderiza só os itens visíveis na tela (virtualização) — importante em listas longas no celular.

```jsx
import { FlatList } from 'react-native'

<FlatList
  data={posts}
  keyExtractor={post => String(post.id)}
  renderItem={({ item }) => <Text>{item.title}</Text>}
  ListEmptyComponent={<Text>Nenhum post.</Text>}
  ListFooterComponent={<Button onPress={carregarMais}>Carregar mais</Button>}
/>
```

---
<style scoped>section { font-size: 24px; }</style>

# `useFocusEffect`

- Na web, a página recarrega (ou o componente é desmontado) ao trocar de rota.
- No app, as telas ficam empilhadas: voltar para uma tela não a remonta, então o `useEffect` não dispara de novo.
- O `useFocusEffect` do Expo Router roda sempre que a tela **volta a ficar visível** — útil para atualizar dados após voltar de outra tela.

```jsx
import { useFocusEffect } from 'expo-router'
import { useCallback } from 'react'

useFocusEffect(useCallback(() => {
  buscarDados()
}, []))
```

---

# Diálogos

- No lugar do `Modal` do React-Bootstrap, o React Native Paper usa `Dialog`, dentro de um `Portal` (para ficar por cima de toda a tela).

```jsx
import { Button, Dialog, Portal, Text } from 'react-native-paper'

<Portal>
  <Dialog visible={visivel} onDismiss={fechar}>
    <Dialog.Title>Cancelar aula</Dialog.Title>
    <Dialog.Content><Text>Deseja mesmo cancelar?</Text></Dialog.Content>
    <Dialog.Actions>
      <Button onPress={fechar}>Voltar</Button>
      <Button onPress={confirmar}>Cancelar aula</Button>
    </Dialog.Actions>
  </Dialog>
</Portal>
```

---
<style scoped>section { font-size: 24px; }</style>

# Rodando o Projeto

```bash
npx create-expo-app@latest . --example with-router
npm install
npx expo start
```

- Abra no celular com o app [Expo Go](https://expo.dev/go), lendo o QR code exibido no terminal.
- Ou aperte `a` (emulador Android), `i` (simulador iOS) ou `w` (navegador, via `react-native-web`).
- O mesmo código React Native também roda na web — por isso o `armazenamento.js` precisa de um caminho alternativo com `localStorage` (próximo slide).

---
<style scoped>section { font-size: 20px; }</style>

# Resumo: Web → React Native

| Web (React) | React Native |
|---|---|
| `div`, `p`, `img`, `input`, `button` | `View`, `Text`, `Image`, `TextInput`, `Pressable` |
| CSS / classes | `StyleSheet.create` + prop `style` |
| React Bootstrap | React Native Paper |
| React Router (`Route`, `useParams`, `useNavigate`) | Expo Router (arquivos em `src/app/`, `useLocalSearchParams`, `router`) |
| `localStorage` | `expo-secure-store` (assíncrono) |
| `import.meta.env.VITE_*` | `process.env.EXPO_PUBLIC_*` |
| `<input type="file">` | `expo-image-picker` |
| `useEffect` | `useFocusEffect` |
| `Modal` do Bootstrap | `Dialog` + `Portal` do Paper |

---
<style scoped>section { font-size: 26px; }</style>

# Tarefa 16

- Pegue o cliente web que você já fez (JSONPlaceholder ou similar) e recrie as mesmas telas em um app Expo.
- Reaproveite o máximo de lógica possível: o *wrapper* da API (`fetch`) muda muito pouco — a diferença está nos componentes visuais e na navegação.
- Troque: `<div>`/`<input>`/`<button>` pelos componentes do React Native; o React Router pelo Expo Router; o `localStorage` (se usado) pelo `expo-secure-store`.
- Teste no navegador (`w`) e, se possível, no celular com o Expo Go.

---
# Referências
- https://docs.expo.dev/
- https://docs.expo.dev/router/introduction/
- https://reactnative.dev/
- https://reactnativepaper.com/
- https://docs.expo.dev/router/advanced/authentication/

---

# <!--fit--> Dúvidas? 🤔
