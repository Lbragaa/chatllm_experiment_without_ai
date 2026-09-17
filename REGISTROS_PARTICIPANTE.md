# Meu registro

O projeto usa FastAPI no backend, React no frontend e SQLite para salvar os dados.

O `models.py` tem as tabelas do banco e o `auth.py` cuida do login. As rotas ficam dentro da pasta `routers`. O `openrouter.py` manda as mensagens para a IA. No frontend, a tela do chat fica no `App.jsx` e a comunicação com o backend fica no `api.js`.

O prompt da IA está fixo no `openrouter.py` e é igual para todo usuário.

Meu plano:

- adicionar as instruções no usuário do banco;
- criar uma rota para buscar e salvar elas;
- usar as instruções do usuário nas respostas da IA;
- fazer uma parte da tela para editar e salvar;
- testar se o texto fica salvo e se cada usuário só acessa as próprias instruções.

Meu rascunho do armazenamento:

```text
ir no models.py

na classe User:
    colocar custom_instructions como campo de texto
    deixar esse campo aceitar NULL

se estiver NULL:
    usar o prompt padrão que já existe
senão:
    usar o texto salvo pelo usuário
```

Também precisa atualizar o banco que já existe. Só colocar o campo no `models.py` não cria a coluna dentro do SQLite antigo.

Mais ou menos assim:

```sql
ALTER TABLE users ADD COLUMN custom_instructions TEXT;
```

Antes disso, conferir se a coluna já existe para não tentar criar duas vezes e manter os dados que já estão salvos.

No `openrouter.py` tem a função `_build_messages()`. Ela monta a lista de mensagens que vai para a IA e coloca o `_SYSTEM_PROMPT` logo no começo.

Agora falta deixar essa função receber as instruções personalizadas também:

```text
se receber custom_instructions:
    usar esse texto como system prompt
senão:
    continuar usando o _SYSTEM_PROMPT normal
```

Depois, mudar o `generate_reply()` e o `stream_reply()` para receber esse valor e passar para o `_build_messages()`. Por enquanto dá para preparar essa parte no OpenRouter e ligar com o usuário depois.
