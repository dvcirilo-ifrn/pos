---
theme: default
size: 4:3
marp: true
paginate: true
_paginate: false
title: Aula 09: JSON
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

**Aula 09**: JSON

---
# JSON
- *JavaScript Object Notation*
- Formato de texto para armazenamento e transporte de dados.
- Desenvolvida no início dos anos 2000 por Douglas Crockford.
- Padrão aberto, leve, simples e legível.
- Apesar do nome, é independente do JS.
- É suportada pela maioria das linguagens atuais.

---
# JSON *vs.* XML
- Não podem ser comparadas diretamente, JSON não é uma linguagem de marcação.
- Ambos são autodescritivos: legíveis.
- Ambos têm hierarquia (elementos contém/podem conter elementos)
- Ambos podem ser utilizados em diversas linguagens.
- JSON é mais rápido de ler e escrever.
- JSON pode usar *arrays*.
- JSON não tem atributos.

---
# Sintaxe
- Segue o padrão `{"chave1": valor1, "chave2": valor2}`.
- As chaves devem ser *strings* entre aspas duplas.
- Elementos são separados por vírgula.
- Os valores podem ser:
    - *string* (entre aspas duplas)
    - número
    - objeto (entre `{}`)
    - *array* (lista entre `[]`)
    - *boolean* (`true`/`false`)
    - *null*
---
<style scoped>section { font-size: 18px; }</style>
# Exemplo
```json
{
"funcionários":
    [
        {"nome":"José Dantas",
        "função":"Presidente",
        "endereço":{
            "rua":"Rua de Baixo",
            "número":33,
            "cidade":"Lajes"
            }
        },
        {"nome":"Maria Silva",
        "função":"Gerente",
        "endereço":{
            "rua":"Rua de Cima",
            "número":75,
            "cidade":"Macau"
            }
        },
        {"nome":"Sinto Muito",
        "função":"Programador",
        "endereço":{
            "rua":"Rua Principal",
            "número":42,
            "cidade":"Bom Jesus"
            }
        }
    ]
}
```
---
# Tarefa 07
- Converta os 3 XMLs gerados nas atividades anteriores para JSON
    - `cardapio.json`, `imobiliaria.json` e `quiz.json`.
- Converta os atributos para elementos.
- [JSON Lint](https://jsonlint.com/)

---
# <!--fit--> Dúvidas? 🤔
