# testing

## Setup do Projeto
```
npm install
```

### Ambiente de Desenvolvimento
```
npm run serve
```

#### Configurando a api Fake

Instalar o json-server globalmente:

```
npm install -g json-server
```

Iniciando a api fake de leilões;

```
json-server --watch db.json

# Conceitos de Testes Unitários em Vue Baseados nos Exemplos

# Objetivo Geral dos Testes

Os testes mostrados possuem como objetivo verificar:

* comportamento dos componentes
* renderização da interface
* comunicação entre componentes
* integração com API
* emissão de eventos
* validação de dados
* comportamento assíncrono

---

# Bibliotecas Utilizadas

## Vue Test Utils

Vue Test Utils

Serve para:

* montar componentes Vue
* simular interações
* acessar DOM
* verificar comportamento do componente

---

## Jest

Jest

Serve para:

* executar testes
* criar mocks
* fazer assertions (`expect`)
* simular funções

---

## flush-promises

Biblioteca usada para esperar promises terminarem.

Muito útil quando:

* componente faz requisições
* hooks async executam
* API é chamada

---

# Conceitos Fundamentais

---

# 1. `mount()`

```js
const wrapper = mount(Componente)
```

## O que faz?

Cria uma versão do componente para teste.

---

## Essência

Simula o componente funcionando de verdade.

---

## O que o wrapper possui?

```js
wrapper.vm
wrapper.find()
wrapper.text()
wrapper.emitted()
```

---

# 2. `wrapper`

Representa o componente montado.

---

## Exemplo

```js
wrapper.find('button')
```

Procura botão dentro do componente.

---

# 3. `find()` e `findAll()`

---

## `find()`

Busca UM elemento.

```js
wrapper.find('input')
```

---

## `findAll()`

Busca vários elementos.

```js
wrapper.findAll('.leilao')
```

---

# 4. `trigger()`

```js
wrapper.trigger('submit')
```

## O que faz?

Simula evento do usuário.

---

## Exemplos

```js
trigger('click')
trigger('submit')
trigger('keyup')
```

---

## Essência

O teste age como usuário.

---

# 5. `setValue()`

```js
input.setValue(100)
```

## O que faz?

Simula usuário digitando.

---

## Importante

Ele:

* altera valor
* dispara evento input automaticamente

---

# 6. `expect()`

Usado para validações.

---

## Exemplos

```js
expect(valor).toBe(100)
expect(lista).toHaveLength(1)
expect(texto).toContain('Vue')
```

---

# Conceitos de Mock

---

# 7. `jest.mock()`

```js
jest.mock('@/http')
```

## O que faz?

Substitui módulo real por versão falsa.

---

## Essência

Evita:

* chamadas reais
* API real
* dependências externas

---

# Exemplo Mental

Sem mock:

```txt
teste chama internet real
```

Com mock:

```txt
teste controla resposta
```

---

# 8. `mockResolvedValueOnce()`

```js
getLeiloes.mockResolvedValueOnce(leiloes)
```

## O que faz?

Define retorno fake de Promise.

---

## Equivale a:

```js
Promise.resolve(leiloes)
```

---

## Muito usado para:

* APIs
* Axios
* Fetch
* chamadas async

---

# Conceitos Assíncronos

---

# 9. `async/await`

Usado porque:

* Vue atualiza async
* APIs retornam Promise

---

## Exemplo

```js
await flushPromises()
```

---

# 10. `flushPromises()`

Espera todas promises terminarem.

---

## Muito importante quando:

```txt
mounted()
↓
faz API
↓
atualiza componente
```

---

# Sem flushPromises()

O teste pode verificar DOM antes da atualização.

---

# 11. `$nextTick()`

```js
await wrapper.vm.$nextTick()
```

## O que faz?

Espera Vue atualizar DOM.

---

## Fluxo

```txt
dados mudam
↓
Vue agenda renderização
↓
nextTick espera terminar
```

---

# Conceitos de Eventos

---

# 12. `wrapper.emitted()`

```js
wrapper.emitted('novo-lance')
```

## O que faz?

Verifica eventos emitidos pelo componente.

---

## Essência

Testa comunicação componente -> pai.

---

# Exemplo

```js
this.$emit('novo-lance', valor)
```

---

# Estrutura do retorno

```js
[
  [100]
]
```

---

## Significa

```txt
evento chamado 1 vez
↓
com argumento 100
```

---

# Conceitos de Props

---

# 13. `propsData`

```js
propsData: {
  lanceMinimo: 300
}
```

## O que faz?

Passa props para componente.

---

## Simula

```html
<Lance :lanceMinimo="300" />
```

---

# Conceitos de Router

---

# 14. `RouterLinkStub`

```js
stubs: {
  RouterLink: RouterLinkStub
}
```

## O que faz?

Substitui RouterLink real.

---

## Por quê?

Evita necessidade do Vue Router real.

---

## Essência

Simplifica teste.

---

# 15. `$router` mockado

```js
const $router = {
  push: jest.fn()
}
```

## O que faz?

Simula Vue Router.

---

# Muito usado para testar:

```js
this.$router.push('/home')
```

---

# Conceitos dos Testes do Avaliador

---

# Objetivo

Testar integração com API.

---

# Conceitos usados

| Conceito              | Função            |
| --------------------- | ----------------- |
| jest.mock             | mockar API        |
| mockResolvedValueOnce | controlar retorno |
| flushPromises         | esperar API       |
| findAll               | contar elementos  |

---

# O que o teste verifica?

```txt
API retorna dados
↓
componente renderiza leilões
```

---

# Conceitos dos Testes do Lance

---

# Objetivo

Testar:

* validação
* emissão de eventos
* regras de negócio

---

# Conceitos usados

| Conceito  | Função                  |
| --------- | ----------------------- |
| setValue  | preencher input         |
| trigger   | enviar formulário       |
| emitted   | verificar evento        |
| propsData | configurar lance mínimo |

---

# O que é validado?

---

## Lance inválido

```txt
não emite evento
```

---

## Lance válido

```txt
emite evento
```

---

## Valor emitido

```txt
evento envia valor correto
```

---

# Conceitos dos Testes do Leiloeiro

---

# Objetivo

Testar renderização baseada em API.

---

# Conceitos usados

| Conceito              | Função               |
| --------------------- | -------------------- |
| mockResolvedValueOnce | simular dados        |
| flushPromises         | esperar carregamento |
| find                  | procurar elementos   |
| textContent           | validar texto        |

---

# O que é testado?

---

## Sem lances

```txt
mostra alerta
```

---

## Com lances

```txt
mostra maior lance
mostra menor lance
```

---

# Conceitos dos Testes do NovoLeilao

---

# Objetivo

Testar criação de leilão.

---

# Conceitos usados

| Conceito         | Função               |
| ---------------- | -------------------- |
| mocks            | mockar router        |
| setValue         | preencher formulário |
| trigger          | submit               |
| toHaveBeenCalled | verificar chamada    |

---

# O que o teste valida?

```txt
usuário preenche formulário
↓
submit ocorre
↓
API createLeilao é chamada
```

---

# Conceitos Importantes de Arquitetura

---

# Teste Unitário

Testa:

* componente isolado
* comportamento específico

---

# Isolamento

Mocks garantem que:

* API real não execute
* router real não execute
* dependências externas não interfiram

---

# Reatividade Vue

Vue atualiza DOM assincronamente.

Por isso aparecem:

* await
* flushPromises
* nextTick

---

# Fluxo Geral Mental dos Testes Vue

```txt
mount()
↓
simula usuário
↓
Vue reage
↓
teste verifica resultado
```

---

# Resumo Geral dos Principais Métodos

| Método                | Função                 |
| --------------------- | ---------------------- |
| mount                 | monta componente       |
| wrapper.find          | busca elemento         |
| wrapper.findAll       | busca vários elementos |
| trigger               | simula evento          |
| setValue              | altera input           |
| emitted               | verifica eventos       |
| jest.mock             | mocka dependências     |
| mockResolvedValueOnce | define retorno fake    |
| flushPromises         | espera promises        |
| nextTick              | espera DOM atualizar   |

---

# Essência Final dos Testes Vue

O objetivo dos testes Vue é verificar:

```txt
se o componente reage corretamente
às ações do usuário
e às mudanças de dados
```

sem depender:

* da API real
* do navegador real
* do backend real
* de outras partes do sistema

```

