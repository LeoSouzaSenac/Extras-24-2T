# Clean Code em TypeScript — Explicação Completa

Clean Code é um conjunto de boas práticas para escrever código **limpo, claro, legível e fácil de manter**.

O objetivo é simples:
**“Escrever código que outra pessoa consiga entender rapidamente — inclusive você mesmo no futuro.”**

TypeScript é perfeito para Clean Code porque oferece **tipos**, **interfaces**, **enums**, **generics** e **modularização**, que ajudam a organizar melhor os projetos.

Esse guia explica cada conceito com exemplos.

---

# 1. Nomes descritivos

## O que é?
Dar nomes claros para **variáveis**, **funções**, **classes** e **arquivos**.

## Por que é importante?
Evita confusão e torna o código autoexplicativo — sem precisar de comentários extras.

## Exemplos

**Ruim (não dá pra entender o propósito)**
```ts
let x = 10;
function calc(d: number) { ... }
````

**Bom**

```ts
let quantidadeDeVidas = 10;
function calcularDesconto(valor: number) { ... }
```

---

# 2. Use os tipos do TypeScript

## O que é?

Definir o tipo das variáveis, parâmetros e retornos.

## Por que é importante?

* evita erros
* deixa claro o que a função espera
* melhora a inteligência do editor (auto complete)

## Exemplos

**Ruim**

```ts
function salvarUser(user: any) {
  // qualquer coisa funciona, inclusive errado
}
```

**Bom**

```ts
interface Usuario {
  nome: string;
  email: string;
  idade: number;
}

function salvarUsuario(usuario: Usuario) {
  // agora o código está protegido
}
```

---

# 3. Funções pequenas (SRP — Single Responsibility Principle)

## O que é?

Cada função deve fazer **apenas uma coisa**.

## Por que é importante?

* facilita testes
* facilita manutenção
* facilita leitura

## Exemplos

**Ruim (função gigante que faz tudo)**

```ts
function processar(user: Usuario) {
  validar(user);
  salvarNoBanco(user);
  enviarEmail(user);
}
```

**Bom**

```ts
function validarUsuario(user: Usuario) { ... }
function salvarUsuarioNoBanco(user: Usuario) { ... }
function enviarEmailDeBoasVindas(user: Usuario) { ... }
```

---

# 🟦 4. Use interfaces e types para evitar repetição (DRY)

## O que é?

DRY = Don’t Repeat Yourself
Significa evitar código duplicado.

## Por que é importante?

* facilita mudanças
* deixa o código mais limpo
* evita erros ao editar informações repetidas

## Exemplos

**Ruim**

```ts
function cadastrar(nome: string, email: string, idade: number) { ... }
function atualizar(nome: string, email: string, idade: number) { ... }
```

**Bom**

```ts
interface Usuario {
  nome: string;
  email: string;
  idade: number;
}

function cadastrar(usuario: Usuario) { ... }
function atualizar(usuario: Usuario) { ... }
```

---

# 5. Use Enums para evitar "magic numbers" e "magic strings"

## O que é?

Um enum cria uma lista de valores com nomes claros.

## Por que é importante?

Evita erros de digitação e melhora a leitura.

## Exemplos

**Ruim**

```ts
if (status === "PAGO") { ... }
```

**Bom**

```ts
enum StatusPagamento {
  PAGO = "PAGO",
  PENDENTE = "PENDENTE",
  FALHOU = "FALHOU"
}

if (status === StatusPagamento.PAGO) { ... }
```

---

# 6. Early Return

## O que é?

Sair da função mais cedo quando algo está errado.

## Por que é importante?

* reduz blocos aninhados
* deixa o código mais direto

## Exemplos

**Ruim**

```ts
function login(user?: Usuario) {
  if (user) {
    if (user.email && user.senha) {
      return autenticar(user);
    }
  }
  return "Erro";
}
```

**Bom**

```ts
function login(user?: Usuario) {
  if (!user || !user.email || !user.senha) return "Erro";
  return autenticar(user);
}
```

---

# 7. Use Generics

## O que é?

Uma forma de criar funções **flexíveis**, que funcionam com vários tipos.

## Por que é importante?

Evita o uso de `any` e deixa funções reutilizáveis.

## Exemplos

**Ruim**

```ts
function pegarItem(item: any) {
  return item;
}
```

**Bom**

```ts
function pegarItem<T>(item: T): T {
  return item;
}
```

---

# 8. Arquivos pequenos e organização por responsabilidade

## O que é?

Cada arquivo deve conter **apenas uma responsabilidade**.

## Por que é importante?

* facilita encontrar código
* melhora a manutenção
* deixa o projeto profissional

## Estrutura recomendada

```
src/
│── modules/          # regras de negócio
│── services/         # comunicação com APIs/BD
│── utils/            # funções auxiliares
│── types/            # tipos globais
│── interfaces/
│── index.ts
```

---

# 9. Comentários apenas quando necessário

## O que é?

Escrever apenas comentários úteis.

## Por que é importante?

Código limpo evita comentários desnecessários.

## Exemplo

**Ruim**

```ts
// soma 1
i = i + 1;
```

**Bom**

```ts
// Explicando regra de negócio complexa
aplicarBonusMensal(usuario);
```

---

# 10. Tratamento de erros

## O que é?

Usar `try/catch` e erros personalizados.

## Por que é importante?

* evita falhas inesperadas
* melhora a experiência do usuário
* facilita debug

## Exemplos

**Ruim**

```ts
const data = JSON.parse(json);
```

**Bom**

```ts
function parseJSON(json: string) {
  try {
    return JSON.parse(json);
  } catch {
    throw new Error("JSON inválido");
  }
}
```

---

# Benefícios do Clean Code em TypeScript

* menos bugs
* mais clareza
* mais organização
* equipe trabalha melhor
* código mais profissional
* fácil de testar
* fácil de evoluir

---

## Exercício de Refatoração de Código

```ts

/* codigo propositalmente ruim
   - nomes curtos e sem sentido
   - sem tipos
   - funcoes gigantes
   - misto de responsabilidades
   - repeticao de codigo
   - magic numbers e strings
   - estruturas confusas
   - sem interfaces
   - sem enums
   - sem early return
   - variaveis globais
*/

// dica: isso deveria estar dentro de uma classe "sistema" ou "repositorio"
// dica: variáveis globais dificultam testes e manutenção
var a = [] // dica: renomear para "usuarios" ou "listaUsuarios"
var u = null // dica: renomear para "usuarioAtual"
var t = "tokenfixo" // dica: evitar magic string; criar método para gerar token
var x = 7 // dica: renomear para algo com significado real

function f(d, k) {
    // dica: nome da função deveria explicar o que ela faz (ex: salvarUsuario)
    // dica: "d" deveria ser renomeado para "usuario"
    // dica: separar validação em função própria

    if (d) {
        if (!d.n) {
            d.n = "x" // dica: porque não criar uma varável para este valor?
        }
    } else {
        d = { n: "x", e: "x@x.com", i: 0 } // dica: criar função "criarUsuarioPadrao"
    }

    for (var i = 0; i < x; i++) {
        if (i === 3) {
            // dica: remover loops inúteis que não fazem nada
        }
    }

    var r = Math.random()
    if (r > 0.1) {
        a.push(d) // dica: extrair para método "adicionarUsuario"
        u = d // dica: atualizar usuário atual deveria ser responsabilidade separada
        console.log("ok", d)
    } else {
        console.log("falhou")
        // dica: retornar cedo (early return) aqui
    }

    var y = []
    // dica: este loop deveria virar uma função "buscarUsuariosPorNome"
    for (var j = 0; j < a.length; j++) {
        if (a[j].n == d.n) {
            y.push(a[j])
        }
    }

    if (k) {
        // dica: callbacks deveriam ser evitados — prefira funções puras ou retorno direto
        k("feito", y)
    }

    var zz = 0
    // dica: este loop não tem objetivo — remover ou dar nome para deixar claro
    for (var q = 0; q < a.length; q++) {
        zz += q
    }
    if (zz > 50) {
        console.log("nao deveria acontecer, mas acontece")
        // dica: remover comportamentos "mágicos" sem propósito
    }
}

function h(n, s) {
    // dica: nome deveria explicar comportamento (ex: autenticarUsuario ou login)
    // dica: parâmetros n e s → renomear para "nome" e "senha"

    if (!n) n = "x" // dica: validação deveria ter função própria
    if (!s) s = "123"

    // dica: criar função "gerarToken"
    t = "t" + Math.floor(Math.random() * 999)

    var achou = false
    // dica: transformar este loop em "buscarUsuarioPorNome"
    for (var i = 0; i < a.length; i++) {
        if (a[i].n == n) {
            achou = true
            u = a[i]
        }
    }

    if (!achou) {
        // dica: criar função "criarUsuario"
        var novo = { n: n, s: s, c: new Date(), ativo: "sim" }
        a.push(novo) // dica: responsabilidade separada
        u = novo
    }

    var soma = 0
    // dica: este loop não tem relação com login → deve ser removido ou isolado
    for (var i = 0; i < 10; i++) {
        soma += i * x
    }

    if (soma > 100) {
        console.log("alerta")
        // dica: alertas genéricos devem ser removidos ou padronizados
    }
}

function z(i) {
    // dica: nome deveria ser "removerUsuario" ou "deletarPorIndice"

    if (i >= 0) {
        var w = []
        // dica: substituir por filter
        for (var j = 0; j < a.length; j++) {
            if (j !== i) {
                w.push(a[j])
            }
        }
        a = w
    } else {
        console.log("nao removeu")
        // dica: usar early return
    }

    // dica: esta verificação deveria ser responsabilidade de outra função
    if (a.length === 0) {
        console.log("vazio")
    } else {
        console.log("tem itens")
    }
}

function p() {
    // dica: nome deveria explicar (ex: gerarRelatorioUsuarios)
    var r = []
    // dica: este loop deveria estar em função "formatarUsuario"
    for (var i = 0; i < a.length; i++) {
        r.push(a[i].n + "-" + a[i].e + "-" + a[i].i)
        // dica: logs "par/impar" não fazem sentido na lógica
        if (i % 2 === 0) {
            console.log("par")
        } else {
            console.log("impar")
        }
    }

    var final = r.join("|") // dica: formato deveria ser constante (ex: separador "|")
    return final
}

function main() {
    // dica: main deveria apenas chamar métodos claros com nomes semânticos

    f({ n: "leo", e: "leo@a", i: 20 }, function (msg, lista) {
        console.log(msg, lista)
    })

    h("leo", "123")

    f({ n: "ana", e: "ana@a", i: 22 }, null)

    z(0)

    var rel = p()
    console.log(rel)
}

main()


```
