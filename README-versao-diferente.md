# Blocos básicos do Node-RED

Exercício prático: um flow com seis linhas independentes (cada uma vai de um `inject` até um `debug`), na aba **Blocos básicos**. Usa só nós nativos do Node-RED.

Arquivo do flow: [`blocos-basicos-node-red-versao-diferente.json`](blocos-basicos-node-red-versao-diferente.json)

## Como importar

1. Abra o Node-RED em <http://localhost:1880>.
2. Menu (☰) → **Import** → **select a file to import** → escolha o `.json` → **Import**.
3. Clique em **Deploy**, abra a aba **debug** e clique no botão de cada `inject`.

## Atividades e saídas esperadas

| # | Nó | Entrada (inject) | Saída esperada (debug) |
|---|----|------------------|------------------------|
| 1 | function | número `9` | `{"numero":9,"resultado":18,"ehPar":false}` |
| 2 | change | `{"nome":"Carlos","idade":21}` | mensagem completa com `topic: "cadastro"` e `payload: {"nome":"Carlos","status":"ativo"}` |
| 3 | switch | `temperatura` 35 e 25 | 35 → saída 1 (ALERTA); 25 → saída 2 (normal) |
| 4 | template | `{"nome":"Carlos","nota":8.7}` | `O(a) aluno(a) Carlos obteve nota 8.7.` |
| 5 | split + function + join | `["diego","erica","felipe"]` | `["DIEGO","ERICA","FELIPE"]` |
| 6 | delay | string `processando` | a mesma mensagem, 2 s depois do clique |

## Como cada atividade foi feita

1. **function**: devolve `{numero, resultado, ehPar}` a partir de `msg.payload`, gerando o número original, o dobro e a indicação de paridade.
2. **change**: três regras, sem JavaScript: *set* `msg.topic`, *set* `msg.payload.status`, *delete* `msg.payload.idade`.
3. **switch**: testa `msg.payload.temperatura`; regra 1 `>= 30`, regra 2 `otherwise`; para na primeira regra que casar.
4. **template**: mustache `O(a) aluno(a) {{payload.nome}} obteve nota {{payload.nota}}.`, com saída em string.
5. **split → function → join**: o join em modo *automatic* usa `msg.parts` para remontar a lista na ordem original.
6. **delay**: *Delay each message* por 2 segundos; o payload não é alterado.
