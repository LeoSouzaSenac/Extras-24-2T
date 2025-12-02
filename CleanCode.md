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

