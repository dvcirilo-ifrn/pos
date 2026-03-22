---
theme: default
size: 4:3
marp: true
paginate: true
_paginate: false
title: Aula 05: XML
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

**Aula 05**: XML

---
![bg 70%](../img/xml.svg)

---
# XML
- *eXtensible Markup Language*
- Linguagem de marcação
    - Conjunto de diretivas que definem estrutura e relacionamento entre as partes.
- Desenvolvida pelo W3C (World Wide Web Consortium)
- Padrão aberto e gratuito.
- Importante no contexto de SOA para troca de dados entre serviços.
    - Serialização

---
# XML
- Baseado no SGML (Standard Generalized Markup Language)
- HTML
- Extensível: permite a definição de marcadores (tags)
- Legível
- Fácil de criar
- Auto-descritivo.

---
<style scoped>section { font-size: 22px; }</style>
# Exemplo
```xml
<?xml version="1.0" encoding="utf-8"?>
<livraria>
    <livro id="L01" ano="1997">
        <autor>
            <nome>Marie</nome>
            <sobrenome>Buretta</sobrenome>
        </autor>
        <titulo>Data Replication</titulo>
        <editora>Wiley</editora>
    </livro>
    <livro id="L02" ano="2000">
        <autor>
            <nome>Ramez</nome>
            <sobrenome>Elmasri</sobrenome>
        </autor>
        <autor>
            <nome>Shamkant</nome>
            <sobrenome>Navathe</sobrenome>
        </autor>
        <titulo>Fundamentals of Database Systems</titulo>
        <editora>Addison Wesley</editora>
    </livro>
</livraria>
```

---
# Estrutura
- Declaração XML.
- Elemento raiz único.
- Elementos.
- Atributos.

---
# Elementos
- Os elementos possuem uma tag de início e uma de fim.
- Um elemento pode conter outros elementos.
- Um elemento pode ser vazio.
```xml
<vazio/>
<vazio></vazio>
```

---
# Tipos de elementos
- Composto
```xml
<autor>
    <nome>Jose</nome>
    <sobrenome>Silva</sobrenome>
</autor>
```

- Texto
```xml
<autor>Jose Silva</autor>
```

---
# Tipos de elementos
- Misto
```xml
<autor>Jose Silva
    <email>joesilva@silva.com</email>
</autor>
```

- Vazio
```xml
<autor/>
```

---
# Atributos
- Associados a um elemento com dados ou vazio.
- Valores entre aspas
- Atributo ou elemento?
    - Atributos são únicos
    - A ordem dos atributos não importa
    - Atributos não tem estrutura
```xml
<nota autor="Diego" data="23-08-2034" assunto="Atributos" texto="Tá certo isso?"></nota>
```

---
# Comentários
- Igual em HTML
```xml
<!-- Informações úteis e relevantes -->
```

---
# Caracteres
- O XML é case-sensitive
    - `<nome>` é diferente de `<Nome>`
- Caracteres especiais:
    - `&lt;` - <
    - `&amp;` - &
- Seção CDATA para textos com muitos caracteres especiais:
```xml
<![CDATA[texto com <><<#$%^]]>
```
---
<style scoped>section { font-size: 24px; }</style>
# Exemplo

- Crie um XML para o gerenciamento de computadores de uma empresa com a seguinte estrutura:
    - Equipamentos (raiz):
        - Computador:
            - Ano de aquisição (atributo)
            - Modelo
            - Processador
            - Memórias
            - Armazenamento
            - Ano de fabricação
            - Quantidade

---
<style scoped>section { font-size: 24px; }</style>
# Exercício
- Crie um XML (`cardapio.xml`) para um cardápio de pelo menos 5 pratos com a seguinte estrutura:
    - Cardápio (raiz):
        - Prato:
            - ID (atributo)
            - Nome do prato
            - Descrição
            - Ingredientes
            - Preço
            - Calorias
            - Tempo de preparo (estimado)

---
# <!--fit--> Dúvidas? 🤔

