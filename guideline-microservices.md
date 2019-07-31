## Guideline de Microsserviços

Este documento deve ser usado como refência na criação de novos microsserviços.

**SIGA ESTE REPOSITÓRIO PARA MANTER-SE ATUALIZADO!!!**

### Introdução

Os microserviços desenvolvidos devem ter foco na tarefa delegada, não resolvendo outros problemas como autenticação, permisses, etc. Também deve ser mantido um código limpo, bem documentado e de fácil manutenção, pois pode ocorrer de o responsável por certa modificação não dominar a linguagem ou a tecnologia.

### Configuração

Os microsserviços devem ser configuráveis exclusivamente via variáveis de ambiente. Configurações como Banco de dados, chave privada, etc no formato `ENV_VAR_NAME`.

Frameworks costumam suportar nativamente este meio de configuração, pesquise sobre a que for usar.

### Banco de Dados

Quando o microsserviço necessitar acesso a um banco de dados, deve auto-configurá-lo, criando suas coleções ou tabelas automaticamente, sem necessidade de iteração manual como imports etc. Usar a abordagem de ORM e migrations é recomendado. Ao usar uma framework, esta costuma dar suporte ao mesmo.

### Nomenclaturas

Usar sempre nomenclaturas em inglês seguindo os padres definidos. Ser objetivo e evitar redundância (ex: rota `/user` retornar `{"userName": "max", "userEmail"...}` ao invés de apenas `name e email`.

### API RESTful

 - Seguir padrões e boas práticas HTTP/RESTful.
 - Sempre que necessário, aplicar CRUD completo (`GET, POST, PUT, DELETE`). 
    - GET users/POST new user: `/user`
    - PUT/DELETE an user: `/user/:id`
 - Simplificar rotas. Se um recurso pode ser ativado via parâmetros, fazer uma rota apenas (obter todos os users ou algos via filtragem com parâmetros `GET`
 - Suportar CORS e pre-flight (`OPTIONS`)
 - Usar status HTTP em respostas adequadamente (`200, 401, 404`, etc)
 - Manter o MS stateless (não usar sessão, qualquer dado externo deve ser recebido em ENV ou na requisição)
 - manter nomenclatura de rotas com `slash-case` (`/sua-rota/da-api`).
 - Retorno: usar `camelCase` (ex: `{ addressCode, fatherName }`)

#### Validação / Erros

As rotas devem fazer as validaçes necessárias para seu funcionamento e os erros devem ser padronizados, usando codigos HTTP adequados:

**Erro de validação:**

```json
{
  "invalidField": "Mensagem de erro para campo invalidField.",
  "email": "Email inválido."
}
```

**Erro geral / Crítico (ainda pensando):** usar chave `error`

```json
{
  "error": ["Erro crtico A", "Erro crtico B"]
}
```

### Futuro

Pode ser necessário manter controle nos MS que apresentam CRUDs, registro do usuário logado para saber quem criou ou editou algum conteúdo. É necessário pensar em uma boa abordagem para isto.

No futuro, todos os MS deverão ter uma rota padrão de configuração (`/setup`). Esta rota irá informar um mapeamento de todas as rotas disponíveis no MS, informando suas configuraçes, parâmetros e permissões necessárias (`READ:POST, WRITE:EVENT`).

Os MS devem também aceitar um parâmetro ENV (`ACCESS_SECRET`) o qual é uma chave secreta. Os MS devem bloquear qualquer acesso caso a requisição não apresente por cabeçalho a chave correta.
