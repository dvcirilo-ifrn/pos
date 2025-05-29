---
theme: default
size: 4:3
marp: true
paginate: true
_paginate: false
title: Aula 07: XML Namespaces
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

**Aula 07**: XML Namespaces

---
<style scoped>section { font-size: 20px; }</style>
# Problemas
- As tags XML são definidas pelo usuário.
- Nomes de elementos iguais são ambíguos.

```xml
<louça>
    <prato>
        <ano>1833</ano>
        <origem>China</origem>
        <valor>143</valor>
    </prato>
</louça>

```
```xml
<cardápio>
    <prato>
        <nome>Bauru</nome>
        <descrição>Sanduíche tradicional</descrição>
        <valor>6</valor>
    </prato>
</cardápio>

```
---
# XML Namespaces
- *Espaços de nome*.
- Definição de um prefixo que identifica a origem dos elementos.
- Reduz a chance de haver ambiguidade.
- Definido com o atributo `xmlns:prefixo="URI"`.
- URI - *Uniform Resource Identifier*, ex. URL.
    - A URI não precisa ser válida, apenas única.
- Os elementos filhos herdam os namespaces.
- Pode ser definido em qualquer elemento, inclusive o raiz.

---
# Exemplo
```xml
<l:louça xmlns:l="http://meusite.com/louca">
    <l:prato>
        <l:ano>1833</l:ano>
        <l:origem>China</l:origem>
        <l:valor>143</l:valor>
    </l:prato>
</l:louça>

<c:cardápio xmlns:c="cardapions">
    <c:prato>
        <c:nome>Bauru</c:nome>
        <c:descrição>Sanduíche tradicional</c:descrição>
        <c:valor>6</c:valor>
    </c:prato>
</c:cardápio>

```

---
# Exemplo
```xml
<raiz xmlns:l="http://meusite.com/louca" xmlns:c="cardapions">
    <l:louça>
        <l:prato>
            <l:ano>1833</l:ano>
            <l:origem>China</l:origem>
            <l:valor>143</l:valor>
        </l:prato>
    </l:louça>
    <c:cardápio>
        <c:prato>
            <c:nome>Bauru</c:nome>
            <c:descrição>Sanduíche tradicional</c:descrição>
            <c:valor>6</c:valor>
        </c:prato>
    </c:cardápio>
</raiz>

```
---
# Exemplo
- Se o prefixo não for definido, o namespace é padrão para todos elementos.
- [SVG](../img/xml.svg)
- [Nota Fiscal Eletrônica](https://www.webdanfe.com.br/danfe/exemplos/NFe_assinada.xml) [(pdf)](https://www.webdanfe.com.br/danfe/exemplos/35080599999090910270550010000000015180051273.pdf)
- Detalhe: O DTD não dá suporte a namespaces...


---
# <!--fit--> Dúvidas? 🤔
