---
theme: default
size: 4:3
marp: true
paginate: true
_paginate: false
title: Aula 06: DTD
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

**Aula 06**: DTD

---
# XML DTD
- *Document Type Definition*
- Define a estrutura e os elementos e atributos permitidos no documento.
- Padronização.
- Validação de documentos externos.

---
# Validação de XML
- Um XML que segue os padrões W3C é considerado *well-formed*, ou bem formado.
- Exemplo:
    - Possui um, e apenas um, elemento raiz.
    - Todos os elementos possuem uma tag de fim `</fim>`.
    - Respeitam a diferença de maiúsculas/minúsculas.
    - Aninhados corretamente `<b><i>texto</i></b>`.
    - Atributos são definidos entre aspas.
- Para ser considerado **válido**, deve atender ao DTD definido.

---
# Exemplo
- O DTD pode ser interno (no mesmo arquivo) ou externo.
- Interno:
```xml
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE nota [
<!ELEMENT nota (para,de,cabecalho,corpo)>
<!ELEMENT para (#PCDATA)>
<!ELEMENT de (#PCDATA)>
<!ELEMENT cabecalho (#PCDATA)>
<!ELEMENT corpo (#PCDATA)>
]>
<nota>
    <para>Chucky</para>
    <de>Annabelle</de>
    <cabecalho>Bilhetinho</cabecalho>
    <corpo>Oi sumido, rs</corpo>
</nota>
```

---
# Exemplo
- Externo:
```xml
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE nota SYSTEM "definicoes.dtd">
<nota>
    <para>Chucky</para>
    <de>Annabelle</de>
    <cabecalho>Bilhetinho</cabecalho>
    <corpo>Oi sumido, rs</corpo>
</nota>
```
- `definicoes.dtd`
```xml
<?xml version="1.0" encoding="utf-8"?>
<!ELEMENT nota (para,de,cabecalho,corpo)>
<!ELEMENT para (#PCDATA)>
<!ELEMENT de (#PCDATA)>
<!ELEMENT cabecalho (#PCDATA)>
<!ELEMENT corpo (#PCDATA)>
```

---
# Sintaxe

- `!DOCTYPE nomeraiz` -  Define o DTD e qual o elemento raiz.
- `<!ELEMENT ...>` - Informações do elemento.
- `<!ATTLIST ...>` - Informações dos atributos.
- `<!ENTITY ...>` - Informações da entidade.
- Tipos de dados:
    - PCDATA - *Parsed Character Data*
    - CDATA - *Character Data*
- `SYSTEM "nomedoarquivo.dtd"` - Arquivos externos (locais ou URL)

---
# Elementos
- `<!ELEMENT nome (tipos-de-dados)>`
- Tipos de dados:
    - `#PCDATA`
    - EMPTY
    - Sub-elementos na forma de lista:
        - `(sub-elem1, sub-elem2)`
        - `(sub-elem1|sub-elem2)`
        - `((sub-elem1|sub-elem2), sub-elem3)`
        - `(#PCDATA|sub-elem1|sub-elem2)`
- `|` - operador "ou"
- A ordem dos sub-elementos importa!

---
# Elementos
- Cardinalidade (quantidade de elementos):
    - `+` um ou mais
    - `*` zero ou mais
    - `?` zero ou um
    - Se não houver símbolo de cardinalidade ao lado do elemento, ele é obrigatório e único.
    - Pode ser aplicado em parênteses.

---
# Exemplos
```xml
<!ELEMENT usuario(nome, cpf, endereco)>
```
```xml
<!ELEMENT usuario(nome, cpf?, endereco*)>
```
```xml
<!ELEMENT usuario(nome, (email|cpf)?, endereco*)>
```
```xml
<!ELEMENT livro(autor+, titulo, num_paginas?)>
```
```xml
<!ELEMENT livro(autor+, titulo, subtitulo?, editora)>
```

---
# Atributos
- `<!ATTLIST nomeelemento nomeatributo tipo valor>`
- Tipo
    - CDATA, ID, etc.
    - Lista. Ex. (valor1|valor2|valor3)
- Valor
    - Padrão "valor-padrao"
    - `#REQUIRED`, `#IMPLIED`, `#FIXED valor`

---
# Exemplos
```xml
<!ATTLIST livro isbn CDATA #REQUIRED>
```
```xml
<!ATTLIST ingrediente unidade (g|l|kg) #REQUIRED>
```
```xml
<!ATTLIST pagamento tipo (credito|debito|pix) "pix">
```

---
# Entidades
- Atalhos para caracteres especiais ou textos.
- Entidades padrão ([lista](https://en.wikipedia.org/wiki/List_of_XML_and_HTML_character_entity_references))
- `<!ENTITY nome "valor">`
```xml
<!ENTITY escritor "Pato Donald">
<!ENTITY copyright "Copyright Disney">
```
- No XML chamamos com `&`nome e `;`, ex:
    - `<autor>&escritor;&copyright;</autor>`


---
<style scoped>section { font-size: 22px; }</style>
# Tarefa
- Crie um DTD `definicoes.dtd` para o XML da aula passada, os requisitos são os seguintes:
    - Elemento raiz é `cardapio` e deve conter um ou mais elementos `prato`.
    - Elemento `prato` deve conter obrigatoriamente um atributo `id` do tipo ID;
    - Elemento `prato` deve conter os sub-elementos `nome`, `descricao`, `ingredientes`, `preco`, `calorias` e `tempoPreparo`;
    - Elemento `ingredientes` deve conter 1 ou mais sub-elementos `ingrediente`;
    - Os demais elementos são do tipo PCDATA;
    - O elemento `preco` deve conter um atributo `moeda`, com as opções `BRL` e `USD`, o padrão deve ser `BRL`.
    - Defina uma entidade chamada `reais` com o valor `R$`.
- Atualize o seu XML para o padrão desse DTD e coloque a tag `<!DOCTYPE...` , indicando o elemento raiz `cardapio` e apontando para o arquivo `definicoes.dtd`

---

# <!--fit--> Dúvidas? 🤔
