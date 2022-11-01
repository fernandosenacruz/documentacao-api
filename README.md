# eGestor API

O [eGestor](https://www.egestor.com.br) disponibiliza uma API RESTful que permite o acesso aos módulos do sistema.

Dúvidas e solicitações relacionadas a integração e API, devem ser enviadas para o e-mail kasper@zipline.com.br.

Recursos disponíveis para acesso via API:
* [**Dados da empresa**](#reference/recursos/empresa)
* [**Contatos**](#reference/recursos/contatos)
* [**Categoria de produtos**](#reference/recursos/categorias)
* [**Produtos**](#reference/recursos/produtos)
* [**Ajuste de estoque**](#reference/recursos/ajuste-de-estoque)
* [**Serviços**](#reference/recursos/servicos)
* [**Disponíveis**](#reference/recursos/disponiveis)
* [**Formas de pagamento**](#reference/recursos/formas-de-pagamento)
* [**Plano de contas**](#reference/recursos/plano-de-contas)
* [**Grupo de tributos**](#reference/recursos/grupo-de-tributos)
* [**Recebimentos**](#reference/recursos/recebimentos)
* [**Pagamentos**](#reference/recursos/pagamentos)
* [**Compras**](#reference/recursos/compras)
* [**Vendas / Ordens de serviço / OS**](#reference/recursos/vendas)
* [**Boletos**](#reference/recursos/boletos)
* [**Relatórios**](#reference/recursos/relatorios)
* [**NFSe**](#reference/recursos/nfse)
* [**NFe**](#reference/recursos/nfe)
* [**Disco virtual**](#reference/recursos/disco-virtual)
* [**Usuários**](#reference/recursos/usuarios)
* [**Webhooks**](#reference/recursos/webhooks)

# Biblioteca
Integre seu sistema de forma rápida e fácil utilizando nossa [biblioteca para PHP](https://github.com/eGestor/egestor-sdk-php).

## URLs de acesso
O eGestor não possui sandbox (ambiente de homologação). Cada conta do eGestor é isolada das outras (multi-tenant), sugerimos aos desenvolvedores que criem uma conta de testes, e depois utilizem a conta de produção com os dados dos clientes.

Para testar a API, crie uma conta gratuitamente, acesse o sistema e clique no menu configurações. Na aba API você gera o personal_token.

URL homologação/produção (access_token): https://api.egestor.com.br/api/oauth/access_token

URL homologação/produção (ex: contatos): https://api.egestor.com.br/api/v1/contatos


## Métodos
Requisições para a API devem seguir os padrões:
| Método | Descrição |
|---|---|
| `GET` | Retorna informações de um ou mais registros. |
| `POST` | Utilizado para criar um novo registro. |
| `PUT` | Atualiza dados de um registro ou altera sua situação. |
| `DELETE` | Remove um registro do sistema. |


## Respostas

| Código | Descrição |
|---|---|
| `200` | Requisição executada com sucesso (success).|
| `400` | Erros de validação ou os campos informados não existem no sistema.|
| `401` | Dados de acesso inválidos.|
| `404` | Registro pesquisado não encontrado (Not found).|
| `405` | Método não implementado.|
| `410` | Registro pesquisado foi apagado do sistema e não esta mais disponível.|
| `422` | Dados informados estão fora do escopo definido para o campo.|
| `429` | Número máximo de requisições atingido. (*aguarde alguns segundos e tente novamente*)|


## Limites (Throttling)
Existe o limite de `60` requisições por minuto por aplicação+usuário.

Você pode acompanhar esses limites nos `headers`: `X-RateLimit-Limit`, `X-RateLimit-Remaining` enviados em todas as respostas da API.

Ações de `listar` exibem `50` registros por página. Não é possível alterar este número.

Por questões de segurança, todas as requisições serão feitas através do protocolo `HTTPS`.

## Listar
As ações de `listar` permitem o envio dos seguintes parâmetros:

| Parâmetro | Descrição |
|---|---|
| `filtro` | Filtra dados pelo valor informado. |
| `page` | Informa qual página deve ser retornada. |


# Group Autenticação - OAuth

Nossa API utiliza [OAuth2](https://oauth.net/2/) como forma de autenticação/autorização.



## Solicitando tokens de acesso [/oauth/access_token]

### Utilizando personal_token [POST]

O `personal_token` é do formato JWT e contém informações da empresa (subdomínio) e do usuário. Este é o token utilizado em sistemas de e-Commerce e sites integrados ao eGestor.

#### Dados para envio no POST
| Parâmetro | Descrição |
|---|---|
| `grant_type` | Informar: `personal` |
| `personal_token` | Token JWT com informações da aplicação cliente. |


+ Request (application/json)

    + Body

            {
              "grant_type": "personal"
              "personal_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhcHAiOiIzNDhlNTM4MzQ2M2Y3MzZjMGExZTJhNTFmNjYwZjA5NCIsInN1YmRvbWluaW8iOiJleGVtcGxvIiwiY2xpZW50IjoiMTU0ZDZlZGQ4YmQzMDEwYzQ4NjBkN2E5Yzk1NzNmYmVmZTUyNGRlZiJ9.JJNs0bFtGOtwyJy_r-eefsvkd387M_x7zpucE1m4WIw",
            }

+ Response 200 (application/json)

    + Body

            {
                "access_token": "[access_token]",
                "token_type": "Bearer",
                "expires_in": 900,
                "refresh_token": "[refresh_token]"
            }

# Group Recursos

# Dados da empresa [/empresa]

Buscar detalhes da conta.

### Listar (List) [GET /empresa]

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]


+ Response 200 (application/json)

          {
    "subdom": "zipline",
    "nome": "ZIPLINE TECNOLOGIA LTDA",
    "fantasia": "",
    "cpfcnpj": "04693497000121",
    "fones": "5530263336",
    "emails": "suporte@zipline.com.br",
    "end": "Rua do Acampamento",
    "num": "380",
    "compl": "SALA  1 - 2 E 3",
    "bairro": "Centro",
    "cidadeNome": "Santa Maria",
    "cidadeCod": "4316907",
    "uf": "RS",
    "cep": "97050002",
    "cifrao": "",
    "crt": 10,
    "atividade": 10,
    "pSimples": 4,
    "pICMS": "",
    "pPIS": "",
    "pCOFINS": "",
    "baseIRPJ": "",
    "baseCSLL": "",
    "pIRPJ": "",
    "pCSLL": "",
    "inscMunicipal": "4628202",
    "tipoIE": "",
    "inscEstadual": "",
    "cnae": "6203100",
    "codTributMunicipio": "724",
    "regimeTributacao": "0",
    "cmc": "",
    "RNTRC": ""
}



# Contatos [/contatos]

Os contatos podem ser clientes, fornecedores e transportadores.

### Listar (List) [GET /contatos{?filtro,endereco,telefone,email,clienteFinal,indIE,IE,IM,suframa,obs,fields,orderBy}]
+ Parameters
    + filtro (string, optional) - Busca a string informada nos campos: nome, fantasia, código, contato, email, telefone e tags.
    + endereco (string, optional) - Busca a string informada no endereço do contato (rua, cep, bairro, cidade e estado)
    + telefone (string, optional) - Busca o valor informado no campo "Telefones" do contato
    + email (string, optional) - Busca o valor informado no campo "E-mails" do contato
    + clienteFinal (number, optional) - Filtrar por cliente final. Valores possíveis:
        * 1 - Sim
        * 2 - Não
    + indIE (number, optional) - Filtrar por indicador de IE. Valores possíveis:
        * 1 - Contribuinte
        * 2 - Isento de IE
        * 9 - Não contribuinte
    + IE (string, optional) - Filtra por inscrição estadual 
    + IM (string, optional) - Filtra por inscrição municipal
    + suframa (string, optional) - Filtra pelo código SUFRAMA 
    + obs (string, optional) - Busca a string informada nas observações do contato
    + fields (string, optional) - Permite definir quais os campos serão retornados pela api. Informe separado por vírgula. Valores possíveis:
        * codigo, nome, fantasia, nomeParaContato, cpfcnpj, tipo, dtNasc, dtCad, emails, fones, logradouro, numero, complemento, bairro, cep, cidade, uf, clienteFinal, indicadorIE, inscricaoMunicipal, inscricaoEstadual, obs, tags
        * ex: &fields=nome,fantasia
        * Padrão: codigo, nome, tipo, emails, fones, cidade, uf, clienteFinal, tags
    + orderBy (string, optional) - Permite definir a ordenação da listagem. Informe o campo e a forma de ordenação (ascendente ou descendente) separados por vírgula. Só é possível definir uma ordenação por requisição. Valores possíveis:
        * codigo, tipo, nome, fantasia, nomeParaContato, cpfcnpj, clienteFinal, indicadorIE, inscricaoEstadual, inscricaoMunicipal, suframa, emails, cidade
        * ex: &orderBy=nome,desc
        * ex: &orderBy=fantasia,asc
        

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)

          {
              "total": 1,
              "per_page": 50,
              "current_page": 1,
              "last_page": 4,
              "next_page_url": null,
              "prev_page_url": null,
              "from": 1,
              "to": 50,
              "data": [
                {
                  "codigo": "1",
                  "tipo": ["cliente", "fornecedor"],
                  "nome": "Nome do contato",
                  "emails": [],
                  "fones": [],
                  "clienteFinal": false,
                  "tags": [],
                  "cidade": "Nome da cidade",
                  "uf": "UF"
                }
              ]
          }

+ Response 401 (application/json)

          {
              "errCode": 401,
              "errMsg": "Não foi possível acessar o sistema. Verifique seu \"access_token\".",
              "errObs": "access_denied",
              "errFields": null,
              "errUrl": "/v1/contatos"
          }

### Novo (Create) [POST]

+ Attributes (object)

    + nome: nome do contato (string, required) - limite 60 caracteres
    + fantasia (string, optional) - limite de 60 caracteres
    + tipo (array, required) - Tipo
        + cliente
        + fornecedor
        + transportadora
    + nomeParaContato (string, optional) - limite de 60 caracteres
    + cpfcnpj (string, optional)
    + dtNasc (string, optional) - formato: YYYY-MM-DD
    + emails (array)
    + fones (array)
    + cep (number, optional)
    + logradouro (string, optional)
    + numero (string, optional)
    + complemento (string, optional)
    + bairro (string, optional)
    + cidade (string, optional)
    + codIBGE (string, optional) - Informe um código IBGE válido
    + uf (string, optional) - O UF deverá ser referente ao código do IBGE informado
    + pais (string, optional)
    + clienteFinal (optional)
    + indicadorIE (enum[number], optional)
        + Members
          + 1 - Contribuinte
          + 2 - Isento de IE
          + 9 - Não contribuinte
    + inscricaoMunicipal (string, optional)
    + inscricaoEstadual (string, optional)
    + inscricaoEstadualST (string, optional)
    + suframa (string, optional)
    + obs (string, optional)
    + tags (array, optional)
    + cepEntrega (number, optional)
    + cpfCnpjEntrega (number, optional)
    + logradouroEntrega (string, optional)
    + numeroEntrega (number, optional)
    + complementoEntrega (string, optional)
    + bairroEntrega (string, optional)
    + codIBGEEntrega (number, optional) - Informe um código IBGE válido
    + ufEntrega (string, optional) - O UF deverá ser referente ao código do IBGE informado
    + pontoRefEntrega (string, optional)

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
              "nome": "Kaya Labadie",
              "fantasia": "",
              "nomeParaContato": "Elfrieda Labadie",
              "cpfcnpj": "00000000000191",
              "tipo": [
                "cliente"
              ],
              "dtNasc": "1992-02-13",
              "emails": [
                "exemplo@example.com.br"
              ],
              "fones": [],
              "cep": ,
              "logradouro": "Rua Exemplo lado ímpar",
              "numero": "999",
              "complemento": "",
              "bairro": "",
              "codIBGE": "3550308",
              "uf": "SP",
              "pais": "",
              "clienteFinal": true,
              "indicadorIE": 1,
              "inscricaoMunicipal": "",
              "inscricaoEstadual": "",
              "inscricaoEstadualST": "",
              "cpfCnpjEntrega": "",
              "logradouroEntrega": "",
              "numeroEntrega": "",
              "complementoEntrega": "",
              "bairroEntrega": "",
              "codIBGEEntrega": "",
              "ufEntrega": "",
              "cepEntrega": "",
              "pontoRefEntrega": ""
              "suframa": "",
              "obs": "",
              "tags": ["cliente bom", "especial"]
            }

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
                "codigo": 1,
                "nome": "Nome do contato"
            }

### Detalhar (Read) [GET /contatos/{codigo}]

+ Parameters
    + codigo (required, number, `1`) ... Código do contato

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)
  Todos os dados do contato
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 58

    + Body

            {
              "codigo": "1",
              "tipo": ["cliente", "fornecedor", "transportadora"],
              "nome": "Nome do novo contato",
              "fantasia": "",
              "nomeParaContato": "",
              "cpfcnpj": "",
              "clienteFinal": true,
              "indicadorIE": 9,
              "inscricaoEstadual": "",
              "inscricaoEstadualST": "",
              "inscricaoMunicipal": "",
              "suframa": "",
              "emails": [],
              "logradouro": "",
              "numero": "",
              "complemento": "",
              "bairro": "",
              "cidade": "",
              "codIBGE": "",
              "uf": "",
              "cep": "",
              "pais": "",
              "fones": [],
              "tags": [],
              "obs": "",
              "dtNasc": "1990-05-12",
              "dtCad": "2017-01-15 11:20:15",
              "cpfCnpjEntrega": "",
              "logradouroEntrega": "",
              "numeroEntrega": "",
              "complementoEntrega": "",
              "bairroEntrega": "",
              "codIBGEEntrega": "0",
              "ufEntrega": "",
              "cepEntrega": 0,
              "pontoRefEntrega": ""
            }

+ Response 404 (application/json)
  Quando registro não for encontrado.
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
              "errCode": 404,
              "errMsg": "Nenhum registro com código 1 econtrado",
              "errObs": null,
              "errFields": null
            }

+ Response 410 (application/json)
  Quando registro foi apagado do sistema, o código de retorno é 410.
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59
    + Body

            {
              "errCode": 410,
              "errMsg": "Registro com código 1 não existe.",
              "errObs": null,
              "errFields": null
            }

### Editar (Update) [PUT  /contatos/{codigo}]

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
              "nome": "Exemplo Company LTDA",
              "fantasia": "",
              "nomeParaContato": "Elfrieda",
              "cpfcnpj": "83294489654",
              "tipo": [
                "cliente"
              ],
              "dtNasc": "1992-02-13",
              "emails": [
                "exemplo@example.com.br"
              ],
              "fones": [],
              "cep": 4320040,
              "logradouro": "Rua Exemplo lado ímpar",
              "numero": "999",
              "complemento": "",
              "bairro": "",
              "codIBGE": "355030",
              "uf": "SP",
              "pais": "",
              "clienteFinal": true,
              "indicadorIE": 1,
              "inscricaoMunicipal": "",
              "inscricaoEstadual": "",
              "inscricaoEstadualST": "",
              "cpfCnpjEntrega": "",
              "logradouroEntrega": "",
              "numeroEntrega": "",
              "complementoEntrega": "",
              "bairroEntrega": "",
              "codIBGEEntrega": "",
              "ufEntrega": "",
              "cepEntrega": "",
              "pontoRefEntrega": "",
              "suframa": "",
              "obs": "",
              "tags": []
            }

+ Response 200 (application/json)
  Todos os dados do contato
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 58

    + Body

            {
              "codigo": "1",
              "tipo": ["cliente", "fornecedor", "transportadora"],
              "nome": "Nome do novo contato",
              "fantasia": "",
              "nomeParaContato": "",
              "cpfcnpj": "",
              "clienteFinal": true,
              "indicadorIE": 9,
              "inscricaoEstadual": "",
              "inscricaoEstadualST": "",
              "inscricaoMunicipal": "",
              "suframa": "",
              "emails": [],
              "logradouro": "",
              "numero": "",
              "complemento": "",
              "bairro": "",
              "cidade": "",
              "codIBGE": "",
              "uf": "",
              "cep": "",
              "pais": "",
              "fones": [],
              "tags": [],
              "obs": "",
              "dtNasc": "1990-05-12",
              "dtCad": "2017-01-15 11:20:15",
              "cpfCnpjEntrega": "",
              "logradouroEntrega": "",
              "numeroEntrega": "",
              "complementoEntrega": "",
              "bairroEntrega": "",
              "codIBGEEntrega": "0",
              "cidadeEntrega": "",
              "ufEntrega": "",
              "cepEntrega": 0,
              "pontoRefEntrega": ""
            }

### Remover (Delete) [DELETE  /contatos/{codigo}]

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
                "code": "200",
                "msg": "Contato com código 317 excluído com sucesso!",
                "obs": null,
                "fields": null
            }

# Categorias de produtos [/categorias]

As categorias são utilizadas para agrupar produtos, podendo filtrar posteriormente por produtos de determinada categoria.


### Listar (List) [GET]

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)

          {
              "total": 1,
              "per_page": 50,
              "current_page": 1,
              "last_page": 1,
              "next_page_url": null,
              "prev_page_url": null,
              "from": 1,
              "to": 50,
              "data": [
                {
                  "codigo": "1",
                  "nome": "Material de escritório"
                }
              ]
          }

+ Response 401 (application/json)

          {
              "errCode": 401,
              "errMsg": "Não foi possível acessar o sistema. Verifique seu \"access_token\".",
              "errObs": "access_denied",
              "errFields": null,
              "errUrl": "/v1/categorias"
          }

### Novo (Create) [POST]

+ Attributes (object)

    + nome (string, required)

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
              "nome": "Fios e arames"
            }

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
                "codigo": 2,
                "nome": "Fios e arames"
            }

### Detalhar (Read) [GET /categorias/{codigo}]

+ Parameters
    + codigo (required, number, `1`) ... Código da categoria

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)
  Todos os dados do categoria
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 58

    + Body

            {
              "codigo": "1",
              "nome": "Material de escritório"
            }

+ Response 404 (application/json)
  Quando registro não for encontrado.
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
              "errCode": 404,
              "errMsg": "Nenhum registro com código 1 encontrado",
              "errObs": null,
              "errFields": null
            }

+ Response 410 (application/json)
  Quando registro foi apagado do sistema, o código de retorno é 410.
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59
    + Body

            {
              "errCode": 410,
              "errMsg": "Registro com código 1 não existe.",
              "errObs": null,
              "errFields": null
            }

### Editar (Update) [PUT  /categorias/{codigo}]

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
              "nome": "Materiais de escritório"
            }

+ Response 200 (application/json)
  Todos os dados do categoria
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 58

    + Body

            {
              "codigo": "1",
              "nome": "Materiais de escritório"
            }

### Remover (Delete) [DELETE  /categorias/{codigo}]

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
                "code": "200",
                "msg": "Categoria com código 1 excluído com sucesso!",
                "obs": null,
                "fields": null
            }


# Produtos [/produtos]

Produtos são utilizados nas vendas e controle de estoque.


### Listar (List) [GET /produtos{?filtro,codCategoria,controlarEstoque,tipoUso,arquivado,anotInternas,Norig,INCM,codConfigTrib,IcProd,updatedBefore,updatedAfter,fields,orderBy}]

+ Parameters
    + filtro (optional) - Busca a string informada nos campos: código do produto, código próprio e nome do produto.
    + codCategoria (optional, integer) - Código da categoria do produto.
    + controlarEstoque (optional, integer) - Filtra por produtos que controlam/não controlam estoque. Valores possíveis:
      * 0 - Não controla estoque
      * 1 - Controla estoque
    + tipoUso (number, optional) - Tipo de uso do produto
        * 0 - Mercadoria para revenda
        * 1 - Matéria-prima
        * 2 - Embalagem
        * 3 - Produtos em processo
        * 4 - Produto final (Produção)
    + arquivado (boolean, optional) - Filtra apenas por produtos arquivados
    + Norig (optional, integer) - Origem do produto. Valores possíveis:
      * 0 - Nacional, exceto as indicadas nos códigos de 3, 4, 5 e 8',
      * 1 - Estrangeira, importação direta, exceto a indicada no código 6',
      * 2 - Estrangeira, adquirida no mercado interno, exceto a indicada no código 7',
      * 3 - Nacional, mercadoria ou bem com Conteúdo de Importação maior que 40% e menor ou igual a 70%',
      * 4 - Nacional, produção em conformidade com processos básicos que tratam as legisl. dos Ajustes',
      * 5 - Nacional, mercadoria ou bem com Conteúdo de Importação inferior ou igual a 40%',
      * 6 - Estrangeira, importação direta, sem similar nacional, constante na lista da CAMEX e gás natural',
      * 7 - Estrangeira, adquir. merc. interno, sem similar nacional, na lista da CAMEX e gás natural',
      * 8 - Nacional, mercadoria ou bem com Conteúdo de Importação superior a 70%'
    + INCM (optional, integer) - NCM dos produtos
    + codConfigTrib (optional, integer) - Código do grupo de tributos vinculado.
    + IcProd (optional, string) - Código interno do produto
    + anotInternas (optional, string) - Filtro baseado nas observações internas do produto
    + updatedBefore (optional, string) - Lista apenas produtos que sofreram alterações antes da data informada. Formato "YYYY-MM-DD HH:MM:SS"
    + updatedAfter (optional, string) - Lista apenas produtos que sofreram alterações após a data informada. Formato "YYYY-MM-DD HH:MM:SS"
    + fields (optional) - Permite definir quais os campos serão retornados pela api. Informe separado por vírgula.
      * ex: &fields=descricao,codigoProprio
    + orderBy (optional) - Permite definir a ordenação da listagem. Informe o campo e a forma de ordenação (ascendente ou descendente) separados por vírgula. Só é possível definir uma ordenação por requisição. Valores possíveis:
        * codigo, codCategoria, descricao, codigoProprio, estoque, estoqueMinimo, controlarEstoque, margemLucro, precoCusto, precoVenda, origemFiscal, unidadeTributada, refEanGtin, ncm, excecaoIPI, codigoCEST, pesoBruto, pesoLiquido, anotacoesNFE, anotacoesInternas, tags, dtCad
        * ex: &orderBy=margemLucro,desc
        * ex: &orderBy=descricao,asc

+ Request (application/json)

  + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)

            {
                "total": 1,
                "per_page": 50,
                "current_page": 1,
                "last_page": 4,
                "next_page_url": null,
                "prev_page_url": null,
                "from": 1,
                "to": 50,
                "data": [
                    {
                        "codigo": 1,
                        "descricao": "Caneca Preta",
                        "codigoProprio": "",
                        "estoque": 60,
                        "estoqueMinimo": 10,
                        "controlarEstoque": false,
                        "margemLucro": 0,
                        "precoCusto": 100,
                        "precoVenda": 200,
                        "origemFiscal": 0,
                        "unidadeTributada": "UN",
                        "refEanGtin": "",
                        "ncm": "",
                        "excecaoIPI": 1,
                        "codigoCEST": "",
                        "pesoBruto": 0,
                        "pesoLiquido": 0,
                        "codigoGrupoTributos": 1,
                        "anotacoesNFE": "",
                        "anotacoesInternas": "",
                        "tags": [ "café", "utilidades" ],
                        "dtCad": "2017-03-10 14:10:37",
                        "codCategoria": 0,
                        "updatedAt": "2019-09-20 16:56:39",
                        "listaImagens": [
                            {
                                "foto": {
                                    "link": "https://730590973010370e1726.cdn.com/010-2154900.png",
                                    "padrao": 1
                                },
                                "thumb": {
                                    "link": "https://730590973010370e1726.cdn.com/010-2154900_tmb.png",
                                }
                            },
                            {
                                "foto": {
                                    "link": "https://730590973010370e1726.cdn.com/011-2111100.png",
                                },
                                "thumb": {
                                    "link": "https://730590973010370e1726.cdn.com/011-2111100_tmb.png",
                                }
                            }
                        ]
                    }
                ]
            }

### Novo (Create) [POST]

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

                {
                  "descricao": "Novo produto",
                  "codigoProprio": "001",
                  "codCategoria": 1,
                  "estoque": 100,
                  "estoqueMinimo": 0,
                  "controlarEstoque": true,
                  "margemLucro": 0.00,
                  "precoCusto": 5.00,
                  "precoVenda": 10.37,
                  "origemFiscal": 0,
                  "unidadeTributada": "UN",
                  "refEanGtin": "",
                  "ncm": "",
                  "codigoCEST": "0107400",
                  "excecaoIPI": 7,
                  "codigoGrupoTributos": 0,
                  "anotacoesNFE": "",
                  "anotacoesInternas": "",
                  "pesoBruto": 0,
                  "pesoLiquido": 0,
                  "tags": ['exemplo', 'modelo']
                }

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
                "codigo": 1,
                "descricao": "Novo produto"
            }

### Detalhar (Read) [GET /produtos/{codigo}]

+ Request (application/json)

  + Headers

            Authorization: Bearer [access_token]

  + Parameters
      + codigo (required, number, `1`) ... Código do contato

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 58

    + Body

            {
              "codigo": "1",
              "descricao": "Novo produto",
              "codigoProprio": "",
              "codCategoria": 1,
              "estoque": 100,
              "estoqueMinimo": 0,
              "controlarEstoque": true,
              "margemLucro": 0.00,
              "precoCusto": 0.0000,
              "precoVenda": 0.0000,
              "origemFiscal": 0,
              "unidadeTributada": "UN",
              "refEanGtin": "",
              "ncm": "",
              "codigoCEST": "",
              "excecaoIPI": "",
              "codigoGrupoTributos": 0,
              "anotacoesNFE": "",
              "anotacoesInternas": "",
              "pesoBruto": "0.000",
              "pesoLiquido": "0.000",
              "tags": [],
              "dtCad": "2017-05-24 17:32:18",
              "codCategoria": 0,
              "updatedAt": "2019-10-30 14:55:13"
            }

+ Response 410 (application/json)
 Quando o registro foi apagado do sistema.

    + Body

            {
              "errCode": 410,
              "errMsg": "Registro com código 1 não existe.",
              "errObs": null,
              "errFields": null
            }

+ Response 404 (application/json)
 Quando o registro não foi encontrado.

    + Body

            {
              "errCode": 404,
              "errMsg": "Nenhum registro com código 1 econtrado",
              "errObs": null,
              "errFields": null
            }

### Editar (Update) [PUT /produtos/{codigo}]

+ Request (application/json)

  + Headers

            Authorization: Bearer [access_token]

  + Parameters
      + codigo (required, number, `1`) ... Código do contato
      
  + Body
        
            {
                  "descricao": "Novo produto editado",
                  "codigoProprio": "007",
                  "codCategoria": 1,
                  "estoque": 50,
                  "estoqueMinimo": 25,
                  "controlarEstoque": true,
                  "margemLucro": 50.00,
                  "precoCusto": 5.00,
                  "precoVenda": 7.50,
                  "origemFiscal": 3,
                  "unidadeTributada": "UN",
                  "refEanGtin": "7895548123584",
                  "ncm": "12345678",
                  "codigoCEST": "0107400",
                  "excecaoIPI": 7,
                  "codigoGrupoTributos": 2,
                  "anotacoesNFE": "",
                  "anotacoesInternas": "",
                  "pesoBruto": 1.70,
                  "pesoLiquido": 1.22,
                  "tags": ["exemplo", "modelo"]
            }

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 58

    + Body

            {
                "codigo": 1,
                "descricao": "Novo produto editado"
            }

+ Response 410 (application/json)
 Quando o registro foi apagado do sistema.

    + Body

            {
              "errCode": 410,
              "errMsg": "Registro com código 1 não existe.",
              "errObs": null,
              "errFields": null
            }

+ Response 404 (application/json)
 Quando registro não foi encontrado.

    + Body

            {
              "errCode": 404,
              "errMsg": "Nenhum registro com código 1 econtrado",
              "errObs": null,
              "errFields": null
            }

### Remover (Delete) [DELETE  /produtos/{codigo}]

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
              "code": "200",
              "msg": "Produto com código 1 excluído com sucesso.",
              "obs": null,
              "fields": null
            }

# Ajuste de estoque [/ajusteEstoque]

Permite definir o estoque atual dos produtos. 
Os ajustes de estoque não podem ser editados nem excluídos, caso seja necessário realize um novo ajuste.

| motivo | Descrição |
|------|------------|
| `10` | Ajuste geral. |
| `20` | Envio de RMA. |
| `30` | Retorno de RMA. |
| `40` | Devolução de venda. |
| `50` | Devolução de compra. |


### Listar (List) [GET]

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)

          {
              "total": 1,
              "per_page": 50,
              "current_page": 1,
              "last_page": 1,
              "next_page_url": null,
              "prev_page_url": null,
              "from": 1,
              "to": 50,
              "data": [
                {
                  "codigo": 1,
                  "codContato": 1,
                  "nomeContato": "Cliente padrão",
                  "motivo": 10,
                  "dtCad": "2019-05-15 10:56:58",
                  "obs": "Ajusta por quebra",
                  "tags": [
                      "api,teste"
                  ]
                }
              ]
          }

+ Response 401 (application/json)

          {
              "errCode": 401,
              "errMsg": "Não foi possível acessar o sistema. Verifique seu \"access_token\".",
              "errObs": "access_denied",
              "errFields": null,
              "errUrl": "/v1/ajusteEstoque"
          }

### Novo (Create) [POST]

+ Attributes (object)

    + motivo (number, required) - Motivo do ajuste (ver tabela)
    + codContato (number, optional) - Código do responsável pelo ajuste
    + obs (string, optional) - Observações gerais
    + tags (array, optional) - Palavras-chave
    + produtos (array, required) - Produtos que serão ajustados

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
              "codContato": 1,
              "motivo": 10,
              "obs": "",
              "tags": ["api", "teste"],
              "produtos": [
                {
                  "codProduto": 1,
                  "estoqueFinal": 300
                },
                {
                  "codProduto": 2,
                  "estoqueFinal": 50
                }
              ]
            }

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
                "codigo": 1
            }

### Detalhar (Read) [GET /ajusteEstoque/{codigo}]

+ Parameters
    + codigo (required, number, `1`) ... Código do ajuste

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)
  Todos os dados do ajuste
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 58

    + Body

            {
              "codigo": 1,
              "codContato": 1,
              "nomeContato": "Cliente padrão",
              "motivo": 10,
              "dtCad": "2019-05-15 10:56:58",
              "obs": "",
              "tags": [
                  "api,teste"
              ],
              "produtos": [
                  {
                      "estoqueFinal": 300,
                      "quantidade": -402,
                      "codProduto": 1,
                      "nomeProduto": "Produto padrão 1",
                      "codProprio": ""
                  },
                  {
                      "estoqueFinal": 50,
                      "quantidade": -602,
                      "codProduto": 2,
                      "nomeProduto": "Produto padrão 2",
                      "codProprio": ""
                  }
              ]
            }

+ Response 404 (application/json)
  Quando registro não for encontrado.
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
              "errCode": 404,
              "errMsg": "Nenhum registro com código 1 econtrado",
              "errObs": null,
              "errFields": null
            }

+ Response 410 (application/json)
  Quando registro foi apagado do sistema, o código de retorno é 410.
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59
    + Body

            {
              "errCode": 410,
              "errMsg": "Registro com código 1 não existe.",
              "errObs": null,
              "errFields": null
            }

# Serviços [/servicos]


### Listar (List) [GET /servicos{?filtro,fields,orderBy}]
+ Parameters
    + filtro (optional) - Busca a string informada nos campos código e descrição do serviço.
    + fields (optional) - Permite definir quais os campos serão retornados pela api. Informe separado por vírgula.
      * ex: &fields=codigo,descricao
    + orderBy (optional) - Permite definir a ordenação da listagem. Informe o campo e a forma de ordenação (ascendente ou descendente) separados por vírgula. Só é possível definir uma ordenação por requisição. Valores possíveis:
        * codigo, codCategoria, descricao, precoVenda, dtCad
        * ex: &orderBy=descricao,desc
        * ex: &orderBy=precoVenda,asc

+ Request (application/json)

  + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)

            {
                "total": 1,
                "per_page": 50,
                "current_page": 1,
                "last_page": 1,
                "next_page_url": null,
                "prev_page_url": null,
                "from": 1,
                "to": 50,
                "data": [
                    {
                        "codigo": 3,
                        "descricao": "",
                        "precoVenda": 0,
                        "codigoGrupoTributos": 0,
                        "tags": [],
                        "dtCad": "2017-04-24 11:44:35",
                        "itemListaServico": "",
                        "cnae": ""
                    }
                ]
            }

### Novo (Create) [POST]

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
              "descricao": "Manuteção agendada",
              "precoVenda": 10,
              "codigoGrupoTributos": 1,
              "tags": [],
              "itemListaServico": "14.01",
              "cnae": "1234567",
              "codTributacaoMun": "987654",
              "anotacoesInternas": ""
            }

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
                "codigo": 10,
                "descricao": "Manuteção agendada"
            }

### Detalhar (Read) [GET /servicos/{codigo}]

+ Request (application/json)

  + Headers

            Authorization: Bearer [access_token]

  + Parameters
      + codigo (required, number, `1`) ... Código do serviço

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 58

    + Body

            {
              "codigo": 10,
              "descricao": "Manuteção agendada",
              "precoVenda": 10,
              "codigoGrupoTributos": 1,
              "anotacoesInternas": "",
              "dtCad": "2017-04-24 11:44:35",
              "tags": [],
              "itemListaServico": "14.01",
              "cnae": "1234567",
              "codTributacaoMun": "987654"
            }

+ Response 410 (application/json)
 Quando o registro foi apagado do sistema.

    + Body

            {
              "errCode": 410,
              "errMsg": "Registro com código 1 não existe.",
              "errObs": null,
              "errFields": null
            }

+ Response 404 (application/json)
 Quando o registro não foi encontrado.

    + Body

            {
              "errCode": 404,
              "errMsg": "Nenhum registro com código 1 econtrado",
              "errObs": null,
              "errFields": null
            }

### Editar (Update) [PUT /servicos/{codigo}]

+ Request (application/json)

  + Headers

            Authorization: Bearer [access_token]

  + Parameters
      + codigo (required, number, `1`) ... Código do serviço
      
  + Body 
            
            {
                "descricao": "Editado API",
                "precoVenda": 399,
                "codigoGrupoTributos": 1,
                "tags": ["Exemplo", "API"],
                "itemListaServico": "1.03",
                "cnae": "1234567",
                "codTributacaoMun": "112233",
                "anotacoesInternas": "Anotação"
            }

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 58

    + Body

            {
              "codigo": 1,
              "descricao": "Editado API"
            }

+ Response 410 (application/json)
 Quando o registro foi apagado do sistema.

    + Body

            {
              "errCode": 410,
              "errMsg": "Registro com código 1 não existe.",
              "errObs": null,
              "errFields": null
            }

+ Response 404 (application/json)
 Quando o registro não foi encontrado.

    + Body

            {
              "errCode": 404,
              "errMsg": "Nenhum registro com código 1 econtrado",
              "errObs": null,
              "errFields": null
            }

### Remover (Delete) [DELETE  /servicos/{codigo}]

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
              "code": "200",
              "msg": "Serviço com código 1 excluído com sucesso.",
              "obs": null,
              "fields": null
            }


# Disponiveis [/disponiveis]

Configuração das contas caixa da empresa.


### Listar (List) [GET /disponiveis{?fields,orderBy}]
+ Parameters
    + fields (optional) - Permite definir quais os campos serão retornados pela api. Informe separado por vírgula. Valores possíveis:
        * codigo, nome, saldo, visivel
        * ex: &fields=nome,saldo
        * Padrão: codigo, nome, saldo, visivel
    + orderBy (optional) - Permite definir a ordenação da listagem. Informe o campo e a forma de ordenação (ascendente ou descendente) separados por vírgula. Só é possível definir uma ordenação por requisição. Valores possíveis:
        * codigo, nome, saldo, visivel
        * ex: &orderBy=nome,asc
        * ex: &orderBy=saldo,desc

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)

          {
              "total": 1,
              "per_page": 50,
              "current_page": 1,
              "last_page": 4,
              "next_page_url": null,
              "prev_page_url": null,
              "from": 1,
              "to": 50,
              "data": [
                {
                  "codigo": 1,
                  "nome": "Caixa Interno",
                  "saldo": 15.5,
                  "visivel": true
                }
              ]
          }

+ Response 401 (application/json)

          {
              "errCode": 401,
              "errMsg": "Não foi possível acessar o sistema. Verifique seu \"access_token\".",
              "errObs": "access_denied",
              "errFields": null,
              "errUrl": "/v1/disponiveis"
          }

### Novo (Create) [POST]

+ Attributes (object)

    + nome (string, required)
    + saldo (number, required)

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
              "nome": CEF,
              "saldo": 5000.35
            }

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
              "codigo": 1,
              "nome": "CEF",
              "saldo": 5000.35
            }

### Detalhar (Read) [GET /disponiveis/{codigo}]

+ Parameters
    + codigo (required, number, `1`) ... Código do disponivel

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)
  Todos os dados do disponivel
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 58

    + Body

            {
              "codigo": 1,
              "nome": "CEF",
              "saldo": 5000.35,
              "visivel": true
            }

+ Response 404 (application/json)
  Quando registro não for encontrado.
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
              "errCode": 404,
              "errMsg": "Nenhum registro com código 1 econtrado",
              "errObs": null,
              "errFields": null
            }

+ Response 410 (application/json)
  Quando registro foi apagado do sistema, o código de retorno é 410.
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59
    + Body

            {
              "errCode": 410,
              "errMsg": "Registro com código 1 não existe.",
              "errObs": null,
              "errFields": null
            }

### Editar (Update) [PUT  /disponiveis/{codigo}]

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
              "nome": "CEF 2",
              "saldo": 6000.35
            }

+ Response 200 (application/json)
  Todos os dados do disponivel
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 58

    + Body

            {
              "codigo": "1",
              "nome": "CEF 2",
              "saldo": 6000.35,
              "visivel": true
            }

### Remover (Delete) [DELETE  /disponiveis/{codigo}]

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
                "code": "200",
                "msg": "Conta caixa com código 1 excluído com sucesso!",
                "obs": null,
                "fields": null
            }

# Formas de pagamento [/formasPagamento]

 | tipoCusto | Descrição |
 |------|------------|
 | `0` | Sem custo. |
 | `1` | Porcentagem. |
 | `2` | Fixo por total. |
 | `3` | Fixo por parcela. |

 | formaPgtoNFCe | Descrição |
 |------|------------|
 | `0` | Nenhum |
 | `1` | Dinheiro |
 | `2` | Cheque |
 | `3` | Cartão de Crédito |
 | `4` | Cartão de Débito |
 | `5` | Crédito Loja |
 | `10` | Vale Alimentação |
 | `11` | Vale Refeição |
 | `12` | Vale Presente |
 | `13` | Vale Combustível |
 | `99` | Outros |

### Listar (List) [GET /formasPagamento{?fields,orderBy}]
+ Parameters
    + fields (optional) - Permite definir quais os campos serão retornados pela api. Informe separado por vírgula. Valores possíveis:
        * codigo, codDisponivel, nome, formaPgtoNFCe, tipoCusto, valorCusto
        * ex: &fields=nome,tipoCusto,valorCusto
        * Padrão: codigo, codDisponivel, nome, formaPgtoNFCe, tipoCusto, valorCusto
    + orderBy (optional) - Permite definir a ordenação da listagem. Informe o campo e a forma de ordenação (ascendente ou descendente) separados por vírgula. Só é possível definir uma ordenação por requisição. Valores possíveis:
        * codigo, codDisponivel, nome, formaPgtoNFCe, tipoCusto, valorCusto
        * ex: &orderBy=nome,asc
        * ex: &orderBy=codigo,desc

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)

          {
              "total": 1,
              "per_page": 50,
              "current_page": 1,
              "last_page": 4,
              "next_page_url": null,
              "prev_page_url": null,
              "from": 1,
              "to": 50,
              "data": [
                {
                  "codigo": 1,
                  "codDisponivel": 1,
                  "nome": "Dinheiro",
                  "formaPgtoNFCe": 1,
                  "tipoCusto": 0,
                  "valorCusto": 0
                }
              ]
          }

+ Response 401 (application/json)

          {
              "errCode": 401,
              "errMsg": "Não foi possível acessar o sistema. Verifique seu \"access_token\".",
              "errObs": "access_denied",
              "errFields": null,
              "errUrl": "/v1/formaspagamento"
          }

### Novo (Create) [POST]

+ Attributes (object)

    + nome (string, required)
    + codDisponivel (number, optional)
    + tipoCusto (number, optional)
    + valorCusto (number, optional)
    + formaPgtoNFCe (number, optional)

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
              "nome": "Boleto",
              "codDisponivel": 1,
              "tipoCusto": 2,
              "valorCusto": 2.59,
              "formaPgtoNFCe": 99
            }

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
                "codigo": 1,
                "codDisponivel": 1,
                "nome": "Boleto",
                "formaPgtoNFCe": 99,
                "tipoCusto": 2,
                "valorCusto": 2.59
            }

### Detalhar (Read) [GET /formasPagamento/{codigo}]

+ Parameters
    + codigo (required, number, `1`) ... Código da forma de pagamento

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)
  Todos os dados da forma de pagamento
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 58

    + Body

            {
              "codigo": 1,
              "codDisponivel": 1,
              "nome": "Boleto",
              "formaPgtoNFCe": 99,
              "tipoCusto": 2,
              "valorCusto": 2.59
            }

+ Response 404 (application/json)
  Quando registro não for encontrado.
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
              "errCode": 404,
              "errMsg": "Nenhum registro com código 1 encontrado",
              "errObs": null,
              "errFields": null
            }

+ Response 410 (application/json)
  Quando registro foi apagado do sistema, o código de retorno é 410.
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59
    + Body

            {
              "errCode": 410,
              "errMsg": "Registro com código 1 não existe.",
              "errObs": null,
              "errFields": null
            }

### Editar (Update) [PUT  /formasPagamento/{codigo}]

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
                "nome": "Boleto editado",
                "codDisponivel": 2,
                "tipoCusto": 1,
                "valorCusto": 2.59,
                "formaPgtoNFCe": 99
            }

+ Response 200 (application/json)
  Todos os dados da forma de pagamento
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 58

    + Body

            {
              "codigo": 1,
              "codDisponivel": 2,
              "nome": "Boleto editado",
              "formaPgtoNFCe": 99,
              "tipoCusto": 1,
              "valorCusto": 2.59
            }

### Remover (Delete) [DELETE  /formasPagamento/{codigo}]

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
                "code": "200",
                "msg": "Forma de pagamento com código 1 excluído com sucesso!",
                "obs": null,
                "fields": null
            }

# Plano de contas [/planoContas]

| tipo | Descrição |
|------|------------|
| `1` | Receita. |
| `2` | Despesa. |

Todo plano de conta precisa estar vinculado a um dos planos de conta padrão já existentes no sistema, identificados pelo atributo `registroPadrao` = true. O atributo `tipo` (receita ou despesa) é obtido automaticamente, de acordo com o registro pai. 


### Listar (List) [GET /planoContas{?fields,orderBy}]
+ Parameters
    + fields (optional) - Permite definir quais os campos serão retornados pela api. Informe separado por vírgula. Valores possíveis:
        * codigo, codPai, nome, tipo, debitar, creditar, registroPadrao
        * ex: &fields=nome,registroPadrao
        * Padrão: codigo, codPai, nome, tipo, debitar, creditar, registroPadrao
    + orderBy (optional) - Permite definir a ordenação da listagem. Informe o campo e a forma de ordenação (ascendente ou descendente) separados por vírgula. Só é possível definir uma ordenação por requisição. Valores possíveis:
        * codigo, codPai, nome, tipo, debitar, creditar, registroPadrao
        * ex: &orderBy=nome,asc
        * ex: &orderBy=codigo,desc

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)

          {
              "total": 1,
              "per_page": 50,
              "current_page": 1,
              "last_page": 4,
              "next_page_url": null,
              "prev_page_url": null,
              "from": 1,
              "to": 50,
              "data": [
                {
                  "codigo": 1,
                  "codPai": 0,
                  "nome": "Receitas diversas",
                  "tipo": 1,
                  "debitar": 0,
                  "creditar": 0,
                  "registroPadrao": true
                }
              ]
          }

+ Response 401 (application/json)

          {
              "errCode": 401,
              "errMsg": "Não foi possível acessar o sistema. Verifique seu \"access_token\".",
              "errObs": "access_denied",
              "errFields": null,
              "errUrl": "/v1/planoContas"
          }

### Novo (Create) [POST]

+ Attributes (object)

    + nome (string, required)
    + codPai (number, required) - Código do registro pai
    + debitar (number, optional) - Conta contábil para débito
    + creditar (number, optional) - Conta contábil para crédito

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
              "nome": "Venda de produtos",
              "codPai": 1
            }

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
                "codigo": 99,
                "codPai": 1,
                "nome": "Venda de produtos",
                "tipo": 1
            }

### Detalhar (Read) [GET /planoContas/{codigo}]

+ Parameters
    + codigo (required, number, `99`) ... Código do plano de conta

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)
  Todos os dados do plano de conta
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 58

    + Body

            {
              "codigo": 99,
              "codPai": 1,
              "nome": "Venda de produtos",
              "tipo": 1,
              "debitar": 0,
              "creditar": 0,
              "registroPadrao": false
            }

+ Response 404 (application/json)
  Quando registro não for encontrado.
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
              "errCode": 404,
              "errMsg": "Nenhum registro com código 99 encontrado",
              "errObs": null,
              "errFields": null
            }

+ Response 410 (application/json)
  Quando registro foi apagado do sistema, o código de retorno é 410.
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59
    + Body

            {
              "errCode": 410,
              "errMsg": "Registro com código 99 não existe.",
              "errObs": null,
              "errFields": null
            }

### Editar (Update) [PUT  /planoContas/{codigo}]
+ Parameters
    + codigo (required, number, `99`) ... Código do plano de conta

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
                "nome": "Venda de produtos editado",
                "codPai": 2,
                "debitar": 2000,
                "creditar": 1000
            }

+ Response 200 (application/json)
  Todos os dados do plano de conta
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 58

    + Body

            {
              "codigo": 99,
              "codPai": 2,
              "nome": "Venda de produtos editado",
              "tipo": 1,
              "debitar": 2000,
              "creditar": 1000,
              "registroPadrao": false
            }

### Remover (Delete) [DELETE  /planoContas/{codigo}]

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
                "code": "200",
                "msg": "Plano de conta com código 99 excluído com sucesso!",
                "obs": null,
                "fields": null
            }

# Grupo de tributos [/configTrib]

Permite consultar os grupos tributários existentes. Não é possível cadastrar nem editar grupos.

| icmsModBc | Modalidade BC ST |
|------|------------|
| `0` | Tabelado ou máx. sugerido |
| `1` | Lista negativa (valor) |
| `2` | Lista positiva (valor) |
| `3` | Lista neutra (valor) |
| `4` | Margem valor agregado (%) |
| `5` | Pauta (Valor) |
| `6` | Valor da operação |

| pisTipoCalcSt | PIS - Tipo de cálculo ST |
|------|------------|
| `0` | Não usar |
| `1` | Porcentagem |
| `2` | Em valor |

| cofinsTipoCalcSt | COFINS - Tipo de cálculo ST |
|------|------------|
| `0` | Não usar |
| `1` | Porcentagem |
| `2` | Em valor |

| icmsMotivDeson | Motivo desoneração |
|------|------------|
| `0` | Não desejo usar |
| `1` | Táxi |
| `3` | Produtor agropecuário |
| `4` | Frotista/locadora |
| `5` | Diplomático/consular |
| `6` | Util. e Motoc. Amazônia Oc. e  Livre Com. |
| `7` | SUFRAMA |
| `8` | Venda a Órgãos Públicos |
| `9` | Outros |
| `10` | Deficiente condutor |
| `11` | Deficiente não condutor |
| `90` | Solicitada pelo Fisco |

### Listar (List) [GET /configTrib{?fields}]
+ Parameters
    + fields (optional) - Permite definir quais os campos serão retornados pela api. Informe separado por vírgula.
        * ex: &fields=codigo,pisAliquotaSt,ipiAliquota
+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)

          {
              "total": 1,
              "per_page": 50,
              "current_page": 1,
              "last_page": 4,
              "next_page_url": null,
              "prev_page_url": null,
              "from": 1,
              "to": 50,
              "data": [
                {
                    "codigo": 1,
                    "descricao": "Tributação padrão",
                    "cfop": "x102",
                    "tipo": "produto",
                    "padrao": true,
                    "pisSitTrib": "99",
                    "cofinsSitTrib": "99",
                    "pisAliquota": "0",
                    "pisValor": "0",
                    "pisAliquotaSt": "0",
                    "pisValorSt": "0",
                    "cofinsAliquota": "0",
                    "cofinsValor": "0",
                    "cofinsAliquotaSt": "0",
                    "cofinsValorSt": "0",
                    "pisTipoCalcSt": "2",
                    "cofinsTipoCalcSt": "1",
                    "icmsSitTrib": "202",
                    "icmsSitTribNFCe": "",
                    "icmsModBc": "0",
                    "icmsModBcSt": "4",
                    "icmsRedBc": "0",
                    "icmsRedBcSt": "0",
                    "icmsAliquota": "0",
                    "icmsAliquotaSt": "0",
                    "icmsMotivDeson": "",
                    "icmsCalcCred": "0",
                    "icmsMvaSt": "0",
                    "icmsBcOpPropriaSt": "0",
                    "icmsPrecoUnitPautaSt": "0",
                    "icmsUfPagSt": "",
                    "ipiSitTrib": "55",
                    "ipiCnpjProdutor": "92156290000185",
                    "ipiCodSelo": "2",
                    "ipiQtdSelo": "3",
                    "ipiCodEnquadramento": "1",
                    "ipiAliquota": "0",
                    "ipiPrecoUnit": "0",
                    "anpCodigo": "",
                    "anpDesc": "",
                    "anpPercentualGLP": "",
                    "anpPercentualGasNac": "",
                    "anpPercentualGasImp": "",
                    "anpValorPartida": ""
                }
              ]
          }

+ Response 401 (application/json)

          {
              "errCode": 401,
              "errMsg": "Não foi possível acessar o sistema. Verifique seu \"access_token\".",
              "errObs": "access_denied",
              "errFields": null,
              "errUrl": "/v1/configTrib"
          }

### Detalhar (Read) [GET /configTrib/{codigo}]

+ Parameters
    + codigo (required, number, `1`) ... Código do grupo de tributo

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)
  Todos os dados do configtrib
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 58

    + Body

            {
              "codigo": 1,
              "descricao": "Tributação padrão",
              "cfop": "x102",
              "tipo": "produto",
              "padrao": true,
              "pisSitTrib": "99",
              "cofinsSitTrib": "99",
              "pisAliquota": "0",
              "pisValor": "0",
              "pisAliquotaSt": "0",
              "pisValorSt": "0",
              "cofinsAliquota": "0",
              "cofinsValor": "0",
              "cofinsAliquotaSt": "0",
              "cofinsValorSt": "0",
              "pisTipoCalcSt": "2",
              "cofinsTipoCalcSt": "1",
              "icmsSitTrib": "202",
              "icmsSitTribNFCe": "",
              "icmsModBc": "0",
              "icmsModBcSt": "4",
              "icmsRedBc": "0",
              "icmsRedBcSt": "0",
              "icmsAliquota": "0",
              "icmsAliquotaSt": "0",
              "icmsMotivDeson": "",
              "icmsCalcCred": "0",
              "icmsMvaSt": "0",
              "icmsBcOpPropriaSt": "0",
              "icmsPrecoUnitPautaSt": "0",
              "icmsUfPagSt": "",
              "ipiSitTrib": "55",
              "ipiCnpjProdutor": "92156290000185",
              "ipiCodSelo": "2",
              "ipiQtdSelo": "3",
              "ipiCodEnquadramento": "1",
              "ipiAliquota": "0",
              "ipiPrecoUnit": "0",
              "anpCodigo": "",
              "anpDesc": "",
              "anpPercentualGLP": "",
              "anpPercentualGasNac": "",
              "anpPercentualGasImp": "",
              "anpValorPartida": "",
              "issqnBc": 100,
              "pisBc": 100,
              "cofinsBc": 100,
              "csllBc": 100,
              "irBc": 100,
              "inssBc": 100
            }

+ Response 404 (application/json)
  Quando registro não for encontrado.
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
              "errCode": 404,
              "errMsg": "Nenhum registro com código 1 econtrado",
              "errObs": null,
              "errFields": null
            }

+ Response 410 (application/json)
  Quando registro foi apagado do sistema, o código de retorno é 410.
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59
    + Body

            {
              "errCode": 410,
              "errMsg": "Registro com código 1 não existe.",
              "errObs": null,
              "errFields": null
            }


# Recebimentos [/recebimentos]
 Cadastro de contas a receber.

 | Situação | Descrição | Data |
 |------|------------|---------|
 | `20` | Em aberto. | DT de vencimento |
 | `40` | Recebido. | DT de pagamento |


### Listar (List) [GET /recebimentos{?filtro,dtTipo,dtIni,dtFim,caixa,origem,conciliacao,planoContas,obs,formaPgto,numDoc,situFin,boleto,recibo,fields,orderBy}]

+ Parameters
    + filtro (optional) - Busca a string informada nos campos: código do recebimento, descrição, palavras-chave e nome do cliente. Pode-se informar vários códigos, separados por virgula.
    + dtTipo (optional, string) - Define a data que será utilizada pelos filtros dtIni e dtFim. Valores possíveis: 
      * dtVencPgto(default) - Data de venc/pgto (caso em aberto, filtra por vencimento, caso pago filtra por pagamento)
      * dtVenc - Data de vencimento
      * dtPgto - Data de pagamento
      * dtComp - Data de competência
      * dtCad - Data de cadastro
    + dtIni (optional, date) - Data inicial, no formato yyyy-mm-dd
    + dtFim (optional, date) - Data final, no formato yyyy-mm-dd
    + caixa (optional, integer) - Código interno da conta caixa
    + origem (optional, string) - Origem do recebimento. Valores possíveis:
      * 'vendas'
      * 'compras'
      * 'financeiro'
      * 'recorrencia'
    + conciliacao (optional, string) - Filtrar pela situação da conciliação bancária. Valores possíveis:
      * 'com' - Somento registros com conciliação bancária
      * 'sem' - Somento registros sem conciliação bancária
    + planoContas (optional, integer) - Código interno do plano de contas vinculado
    + obs (optional, string) - Pesquisa nas observações adicionais do recebimento.
    + formaPgto (optional, integer) - Código interno da forma de pagamento
    + numDoc (optional, string) - Número do documento
    + situFin (optional, integer) - Situação atual do recebimento. Valores possíveis:
      * 20 - A receber
      * 40 - Recebida
    + boleto (optional, string) - Filtra por recebimento com/sem boletos. Valores possíveis:
      * 'com' - Com boleto vinculado
      * 'sem' - Sem boleto vinculado
    + recibo (optional, string) - Filtra por recebimento com/sem recibos. Valores possíveis:
      * 'com' - Com recibo vinculado
      * 'sem' - Sem recibo vinculado
    + fields (optional) - Permite definir quais os campos serão retornados pela api. Informe separado por vírgula. Valores possíveis:
        * codigo, numDoc, descricao, valor, taxa, data, dtVenc, dtPgto, dtCad, dtComp, situacao, codContato, nomeContato, origem, codDisponivel, codModulo, obs, tags 
        * ex: &fields=nomeContato,valor
        * Padrão: codigo,descricao,valor,data,situacao,nomeContato
    + orderBy (optional) - Permite definir a ordenação da listagem. Informe o campo e a forma de ordenação (ascendente ou descendente) separados por vírgula. Só é possível definir uma ordenação por requisição. Valores possíveis:
        * codigo, codContato, nomeContato, codFormaPgto, codDisponivel, codPlanoContas, codConciliacao, codBoleto, codRecibo, codModulo, origem, descricao, numDoc, tags, valor, valorPago, taxa, dtComp, dtVenc, dtPgto, dtCad, dtDel, motivoDel, obs, previsao, situacao
        * ex: &orderBy=situacao,desc
        * ex: &orderBy=nomeContato,asc

+ Request (application/json)

  + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)
  + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

  + Body

            {
                "total": 1,
                "per_page": 50,
                "current_page": 1,
                "last_page": 4,
                "next_page_url": null,
                "prev_page_url": null,
                "from": 1,
                "to": 50,
                "data": [
                  {
                    "codigo": "1",
                    "descricao": "Venda direta editada",
                    "situacao": "40",
                    "data": "2017-04-24",
                    "valor": "50.00",
                    "nomeContato": "Nome do cliente"
                  }
                ]
            }
            
### Novo (Create) [POST]

+ Request (application/json)

  + Headers

            Authorization: Bearer [access_token]

  + Body

            {
              "codPlanoContas": 1,
              "codFormaPgto": 0,
              "numDoc": "",
              "descricao": "Recebimento parcela 1/1",
              "valor": 500,
              "dtVenc": "2017-09-02",
              "dtPgto": "0000-00-00",
              "dtComp": "2017-09-02",
              "recebido": false,
              "codContato": 7,
              "codDisponivel": 1,
              "obs": "",
              "tags": [ "tag1", "tag2" ]
            }

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 58

    + Body

            {
              "codigo": 1,
              "descricao": "Recebimento parcela 1/1",
              "situacao": 20,
              "origem": "financeiro",
              "codModulo": 1
            }
            


### Detalhar [GET /recebimentos/{codigo}]


+ Parameters
    + codigo (required, number, `1`) ... Código do recebimento

+ Request (application/json)

    + Headers

              Authorization: Bearer [access_token]

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
                "codigo": 2430,
                "codPlanoContas": 1,
                "codFormaPgto": 0,
                "numDoc": "",
                "descricao": "Recebimento parcela 1/1",
                "valor": 500,
                "taxa": 0,
                "dtVenc": "2019-04-15",
                "dtPgto": "",
                "dtCad": "2019-05-08 11:06:50",
                "dtComp": "2019-04-10",
                "situacao": 20,
                "codContato": 7,
                "codDisponivel": 6,
                "origem": "financeiro",
                "codModulo": 2430,
                "obs": "",
                "tags": [
                    "tag1,tag2"
                ]
            }

+ Response 410 (application/json)
  Quando o registro foi apagado do sistema, o código de retorno é 410.

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
              "errCode": 410,
              "errMsg": "Registro com código 1 não existe.",
              "errObs": null,
              "errFields": null
            }

+ Response 404 (application/json)
  Quando registro não for encontrado.

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
              "errCode": 404,
              "errMsg": "Nenhum registro com código 1 econtrado",
              "errObs": null,
              "errFields": null
            }


### Editar [PUT /recebimentos/{codigo}]

+ Parameters
    + codigo (required, number, `1`) ... Código do recebimento

+ Request (application/json)

  + Headers

            Authorization: Bearer [access_token]

  + Body

            {
              "codPlanoContas": 1,
              "codFormaPgto": 0,
              "numDoc": "",
              "descricao": "Recebimento parcela 1/1",
              "valor": 500,
              "dtVenc": "2017-09-02",
              "dtComp": "2017-09-02",
              "codContato": 7,
              "codDisponivel": 1,
              "obs": "",
              "tags": [ "tag1", "tag2" ]
            }

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 58

    + Body

            {
              "codigo": 1,
              "descricao": "Recebimento parcela 1/1",
              "situacao": 20,
              "origem": "financeiro",
              "codModulo": 1
            }


### Efetivar [PUT /recebimentos/{codigo}/receber]

+ Parameters
    + codigo (required, number, `1`) ... Código do recebimento

+ Request (application/json)
    + Body

            {
              "valor": 200,
              "dtPgto": "2017-04-25",
              "dtComp": "2019-05-25"
            }

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 58

    + Body

            {
              "code": 200,
              "msg": "Recebimento efetuado com sucesso.",
              "obs": null,
              "fields": null
            }


### Remover [DELETE /recebimentos/{codigo}]

+ Parameters
    + codigo (required, number, `1`) ... Código do recebimento

+ Request (application/json)

+ Response 200 (application/json)

    + Body

            {
              "code": 200,
              "msg": "Recebimento com código 1 excluído com sucesso!",
              "obs": null,
              "fields": null
            }


# Pagamentos [/pagamentos]
 Cadastro de contas a pagar.

 | Situação | Descrição |
 |------|------------|
 | `10` | Em aberto. |
 | `30` | Pago. |


### Listar (List) [GET /pagamentos{?filtro,dtTipo,dtIni,dtFim,caixa,origem,conciliacao,planoContas,obs,formaPgto,numDoc,situFin,boleto,recibo,fields,orderBy}]
+ Parameters
    + filtro (optional) - Busca a string informada nos campos: código do pagamento, descrição, palavras-chave e nome do cliente. Pode-se informar vários códigos, separados por virgula.
    + dtTipo (optional, string) - Define a data que será utilizada pelos filtros dtIni e dtFim. Valores possíveis: 
      * dtVencPgto(default) - Data de venc/pgto (caso em aberto, filtra por vencimento, caso pago filtra por pagamento)
      * dtVenc - Data de vencimento
      * dtPgto - Data de pagamento
      * dtComp - Data de competência
      * dtCad - Data de cadastro
    + dtIni (optional) - Data inicial, no formato yyyy-mm-dd
    + dtFim (optional) - Data final, no formato yyyy-mm-dd
    + caixa (optional, integer) - Código interno da conta caixa
    + origem (optional, string) - Origem do pagamento. Valores possíveis:
      * 'vendas'
      * 'compras'
      * 'financeiro'
      * 'recorrencia'
    + conciliacao (optional, string) - Filtrar pela situação da conciliação bancária. Valores possíveis:
      * 'com' - Somento registros com conciliação bancária
      * 'sem' - Somento registros sem conciliação bancária
    + planoContas (optional, integer) - Código interno do plano de contas vinculado
    + obs (optional, string) - Pesquisa nas observações adicionais do pagamento.
    + formaPgto (optional, integer) - Código interno da forma de pagamento
    + numDoc (optional, string) - Número do documento
    + situFin (optional, integer) - Situação atual do pagamento. Valores possíveis:
      * 20 - A receber
      * 40 Recebida
    + boleto (optional, string) - Filtra por pagamento com/sem boletos. Valores possíveis:
      * 'com' - Com boleto vinculado
      * 'sem' - Sem boleto vinculado
    + recibo (optional, string) - Filtra por pagamento com/sem recibos. Valores possíveis:
      * 'com' - Com recibo vinculado
      * 'sem' - Sem recibo vinculado
    + fields (optional) - Permite definir quais os campos serão retornados pela api. Informe separado por vírgula. Valores possíveis:
        * codigo, numDoc, descricao, valor, taxa, data, dtVenc, dtPgto, dtCad, dtComp, situacao, codContato, nomeContato, origem, codDisponivel, codModulo, obs, tags 
        * ex: &fields=nomeContato,valor
        * Padrão: codigo,descricao,valor,data,situacao,nomeContato
    + orderBy (optional) - Permite definir a ordenação da listagem. Informe o campo e a forma de ordenação (ascendente ou descendente) separados por vírgula. Só é possível definir uma ordenação por requisição. Valores possíveis:
        * codigo, codContato, nomeContato, codFormaPgto, codDisponivel, codPlanoContas, codConciliacao, codBoleto, codRecibo, codModulo, origem, descricao, numDoc, tags, valor, valorPago, taxa, dtComp, dtVenc, dtPgto, dtCad, dtDel, motivoDel, obs, previsao, situacao
        * ex: &orderBy=situacao,desc
        * ex: &orderBy=nomeContato,asc

+ Request (application/json)

  + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)

          {
              "total": 1,
              "per_page": 50,
              "current_page": 1,
              "last_page": 4,
              "next_page_url": null,
              "prev_page_url": null,
              "from": 1,
              "to": 50,
              "data": [
                {
                    "codigo": 1,
                    "descricao": "Compra de produtos, cód 1, parcela 1/2",
                    "valor": 780,
                    "data": "2018-11-23",
                    "situacao": 30,
                    "nomeContato": "Fornecedor padrão"
                }
              ]
          }

### Novo (Create) [POST]

+ Request (application/json)

  + Headers

            Authorization: Bearer [access_token]

  + Body

            {
              "codPlanoContas": 1,
              "codFormaPgto": 0,
              "numDoc": "",
              "descricao": "Pagamento de Luz",
              "valor": 123,
              "dtVenc": "2019-09-02",
              "dtComp": "2019-04-20",
              "dtPgto": "2019-04-21",
              "pago": true,
              "codContato": 7,
              "codDisponivel": 1,
              "obs": "",
              "tags": [ "tag1", "tag2" ]
            }

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 58

    + Body

            {
              "codigo": 1,
              "descricao": "Pagamento de Luz",
              "situacao": 10,
              "origem": "financeiro",
              "codModulo": 1
            }


### Detalhar [GET /pagamentos/{codigo}]

+ Request (application/json)

  + Headers

            Authorization: Bearer [access_token]

+ Parameters
    + codigo (required, number, `1`) ... Código do pagamento

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 58

    + Body

            {
                "codigo": 1,
                "codPlanoContas": 1,
                "codFormaPgto": 0,
                "numDoc": "",
                "descricao": "Recebimento parcela 1/1",
                "valor": 500,
                "taxa": 0,
                "dtVenc": "2019-04-15",
                "dtPgto": "",
                "dtCad": "2019-05-08 11:45:53",
                "dtComp": "2019-04-20",
                "situacao": 10,
                "codContato": 7,
                "codDisponivel": 2,
                "origem": "financeiro",
                "codModulo": 2431,
                "obs": "",
                "tags": [
                    "tag1,tag2"
                ]
            }

+ Response 410 (application/json)
 Quando o registro foi apagado do sistema, o código de retorno é 410.

    + Body

            {
              "errCode": 410,
              "errMsg": "Registro com código 1 não existe.",
              "errObs": null,
              "errFields": null
            }

+ Response 404 (application/json)
 Quando o registro não foi encontrado (Not found).

    + Body

            {
              "errCode": 404,
              "errMsg": "Nenhum registro com código 1 econtrado",
              "errObs": null,
              "errFields": null
            }

### Detalhar contato do pagamento [GET /pagamentos/{codigo}/contato]

+ Request (application/json)

  + Headers

            Authorization: Bearer [access_token]

+ Parameters
    + codigo (required, number, `1`) ... Código do pagamento

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 58

    + Body

            {
              "codigo": 1,
              "nome": "Contato de uma conta a receber",
              "fantasia": "",
              "nomeContato": "",
              "cpfcnpj": "",
              "tipo": [
                "cliente"
              ],
              "dtNasc": "",
              "dtCad": "2016-04-19 11:07:58",
              "emails": [],
              "fones": [],
              "cep": "",
              "logradouro": "",
              "numero": "",
              "complemento": "",
              "bairro": "",
              "cidade": "",
              "codIBGE": "",
              "uf": "RS",
              "pais": "",
              "clienteFinal": true,
              "indicadorIE": 9,
              "inscricaoMunicipal": "",
              "inscricaoEstadual": "",
              "inscricaoEstadualST": "",
              "suframa": "",
              "obs": "",
              "tags": []
            }

### Editar [PUT /pagamentos/{codigo}]

+ Parameters
    + codigo (required, number, `1`) ... Código do pagamento

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
                "codPlanoContas": 1,
                "codFormaPgto": 0,
                "numDoc": "",
                "descricao": "Recebimento parcela 1/1",
                "codDisponivel": 2,
                "valor": 500,
                "dtComp": "2019-04-20",
                "dtVenc": "2019-04-15",
                "codContato": 7,
                "obs": "",
                "tags": [
                    "tag1",
                    "tag2"
                ]
            }

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 58

    + Body

              {
                    "codigo": 1,
                    "descricao": "Recebimento parcela 1/1",
                    "situacao": 10,
                    "origem": "financeiro",
                    "codModulo": 1
              }


### Efetuar pagamento  [PUT /pagamentos/{codigo}/pagar]

+ Parameters
    + codigo (required, number, `1`) ... Código do pagamento

+ Request (application/json)

    + Headers

              Authorization: Bearer [access_token]
    + Body

            {
              "valor": 200,
              "dtPgto": "2017-04-25",
              "dtComp": "2019-11-10"
            }

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 58

    + Body

            {
              "code": 200,
              "msg": "Pagamento efetuado com sucesso.",
              "obs": null,
              "fields": null
            }


### Remover [DELETE /pagamentos/{codigo}]

+ Request (application/json)

  + Headers

            Authorization: Bearer [access_token]

+ Parameters
    + codigo (required, number, `1`) ... Código do pagamento

+ Response 200 (application/json)

    + Body

            {
              "code": 200,
              "msg": "Pagamento com código 1 excluído com sucesso!",
              "obs": null,
              "fields": null
            }

# Compras [/compras]

As compras não podem ser editadas via API. Caso seja necessário, apague-a e crie uma nova.


### Listar (List) [GET /compras{?filtro,dtTipo,dtIni,dtFim,situacao,observacoes,nota,fields,orderBy}]

+ Parameters
    + filtro (optional, string) - Busca a string informada nos campos: código da compra, palavras-chave e nome do fornecedor.
    + dtTipo (optional, string) - Define a data que será utilizada pelos filtros dtIni e dtFim. Valores possíveis: 
        * dtCompra(default) - Data da compra.
        * dtCad - Data de cadastro.
    + dtIni (optional, date) - Data inicial, no formato yyyy-mm-dd
    + dtFim (optional, date) - Data final, no formato yyyy-mm-dd
    + situacao (optional, integer) - Filtra por compra ou orçamento. Valores possíveis: 
        * 10 - Orçamento.
        * 50 - Compra.
    + observacoes (optional, string) - Pesquisa nas observações da compra.
    + nota (optional, string) - Pesquisa no número da nota.
    + fields (optional) - Permite definir quais os campos serão retornados pela api. Informe separado por vírgula. Valores possíveis:
        * codigo, codContato, nomeContato, numNota, dtCompra, valorTotal, obs, tags, situacao, ativo
        * ex: &fields=nomeContato,valorTotal
    + orderBy (optional) - Permite definir a ordenação da listagem. Informe o campo e a forma de ordenação (ascendente ou descendente) separados por vírgula. Só é possível definir uma ordenação por requisição. Valores possíveis:
        * codigo, codContato, nomeContato, numNota, dtCompra, valorTotal, obs, tags, situacao, ativo
        * ex: &orderBy=nomeContato,desc
        * ex: &orderBy=numNota,asc
    

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)

          {
              "total": 1,
              "per_page": 50,
              "current_page": 1,
              "last_page": 4,
              "next_page_url": null,
              "prev_page_url": null,
              "from": 1,
              "to": 50,
              "data": [
                {
                    "codigo": 1,
                    "codContato": 20,
                    "nomeContato": "Fornecedor padrão",
                    "numNota": "775",
                    "dtCompra": "2019-05-03",
                    "valorTotal": 494.76,
                    "obs": "",
                    "tags": [],
                    "situacao": 50,
                    "ativo": 1
                }
              ]
          }

+ Response 401 (application/json)

          {
              "errCode": 401,
              "errMsg": "Não foi possível acessar o sistema. Verifique seu \"access_token\".",
              "errObs": "access_denied",
              "errFields": null,
              "errUrl": "/v1/compras"
          }

### Novo (Create) [POST]

+ Attributes (object)

    + codContato (number, required)
    + numNota (number, optional) - Número do documento fiscal
    + dtCompra (string, required) - Data da realização da compra, formato YYYY-MM-DD
    + extFrete (number, optional) - Valor do frete externo
    + extST (number, optional) - Valor da substituição tributária
    + extDesp (number, optional) - Valor de outras despesas
    + obs (string, optional) - Observações gerais
    + tags (array, optional)
    + situacao (enum[number], required) - Situação da compra
      + Members
        + 10 - Orçamento
        + 50 - Compra efetivada
    + atualizarPrecoVenda (boolean, required) - Define se o preço de venda dos produtos na compra serão atualizados, de acordo com preço de compra + margem definida.

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
                "codContato": 20,
                "numNota": "775",
                "dtCompra": "2019-05-03",
                "extFrete": 0,
                "extST": 0,
                "extDesp": 0,
                "obs": "",
                "tags": [],
                "situacao": 10,
                "atualizarPrecoVenda": false,
                "produtos": [
                    {
                        "codProduto": 10,
                        "custoBruto": 499.99,
                        "quant": 1000,
                        "vDesc": 0,
                        "valorIPI": 0,
                        "valorST": 0
                    },
                    {
                        "codProduto": 12,
                        "custoBruto": 399.98,
                        "quant": 12,
                        "vDesc": 0,
                        "valorIPI": 0,
                        "valorST": 0
                    }
                ],
                "financeiros": [
                    {
                        "descricao": "Compra parcela 1/3",
                        "codCaixa": 5,
                        "codRecibo": 0,
                        "dtComp": "2019-05-03",
                        "tags": [],
                        "dtVenc": "2019-06-03",
                        "valor": 164.92
                    },
                    {
                        "descricao": "Compra parcela 2/3",
                        "codCaixa": 5,
                        "codRecibo": 0,
                        "dtComp": "2019-05-03",
                        "tags": [],
                        "dtVenc": "2019-07-03",
                        "valor": 164.92
                    },
                    {
                        "descricao": "Compra parcela 3/3",
                        "codCaixa": 5,
                        "codRecibo": 0,
                        "dtComp": "2019-05-03",
                        "tags": [],
                        "dtVenc": "2019-08-03",
                        "valor": 164.92
                    }
                ]
            }

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
                "codigo": 173,
                "codContato": 20,
                "nomeContato": "Fornecedor padrão",
                "dtCompra": "2019-05-03",
                "valorTotal": 504789.76
            }

### Detalhar (Read) [GET /compras/{codigo}]

+ Parameters
    + codigo (required, number, `1`) ... Código da venda

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)
  Todos os dados do contato
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 58

    + Body

            {
                "codigo": 173,
                "codContato": 20,
                "nomeContato": "Fornecedor padrão",
                "codCompraXML": 0,
                "numNota": "775",
                "dtCompra": "2019-05-03",
                "dtCad": "2019-05-03 17:41:31",
                "valorTotal": 504789.76,
                "extFrete": 0,
                "extST": 0,
                "extDesp": 0,
                "obs": "",
                "tags": [],
                "situacao": 10,
                "ativo": 1,
                "produtos": [
                    {
                        "codigo": 10,
                        "codProduto": 5553,
                        "custoBruto": 499.99,
                        "quant": 1000,
                        "vDesc": 0,
                        "valorIPI": 0,
                        "valorST": 0,
                        "descricao": "Produto 10",
                        "codigoProprio": "10prop"
                    },
                    {
                        "codigo": 20,
                        "codProduto": 12,
                        "custoBruto": 399.98,
                        "quant": 12,
                        "vDesc": 0,
                        "valorIPI": 0,
                        "valorST": 0,
                        "descricao": "Produto 20",
                        "codigoProprio": "20prop"
                    }
                ],
                "financeiros": [
                    {
                        "codigo": 2326,
                        "codBoleto": 0,
                        "descricao": "Compra parcela 1/3",
                        "codCaixa": 5,
                        "codRecibo": 0,
                        "situacao": 10,
                        "dtComp": "2019-05-03",
                        "tags": [],
                        "dtVenc": "2019-06-03",
                        "valor": 164.92
                    },
                    {
                        "codigo": 2327,
                        "codBoleto": 0,
                        "descricao": "Compra parcela 2/3",
                        "codCaixa": 5,
                        "codRecibo": 0,
                        "situacao": 10,
                        "dtComp": "2019-05-03",
                        "tags": [],
                        "dtVenc": "2019-07-03",
                        "valor": 164.92
                    },
                    {
                        "codigo": 2328,
                        "codBoleto": 0,
                        "descricao": "Compra parcela 3/3",
                        "codCaixa": 5,
                        "codRecibo": 0,
                        "situacao": 10,
                        "dtComp": "2019-05-03",
                        "tags": [],
                        "dtVenc": "2019-08-03",
                        "valor": 164.92
                    }
                ]
            }

+ Response 404 (application/json)
  Quando o registro não foi encontrado.
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
              "errCode": 404,
              "errMsg": "Nenhum registro com código 1 econtrado",
              "errObs": null,
              "errFields": null
            }

+ Response 410 (application/json)
  Quando o registro foi apagado do sistema.
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59
    + Body

            {
              "errCode": 410,
              "errMsg": "Registro com código 1 não existe.",
              "errObs": null,
              "errFields": null
            }

### Efetivar compra [GET  /compras/{codigo}/efetivar]
Efetiva uma compra que esteja na situação "Orçamento", atualizando-a para "Concluída".

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
                "code": 200,
                "msg": "Compra com código 1 efetivada com sucesso.",
                "obs": null,
                "fields": null
            }

### Reabrir compra [GET  /compras/{codigo}/reabrir]
Altera a situação de uma compra que esteja na situação "Concluída", atualizando-a para "Orçamento". 

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
                "code": 200,
                "msg": "Compra com código 1 reaberta com sucesso.",
                "obs": null,
                "fields": null
            }


### Remover (Delete) [DELETE  /compras/{codigo}]
Somente podem ser excluídas compras na situação de `Orçamento`. Ao excluir a compra, todos os financeiros associados a mesma serão também excluídos.
+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
                "code": "200",
                "msg": "Compra com código 173 excluída com sucesso!",
                "obs": null,
                "fields": null
            }
            

### Detalhar XML NFE (Read) [GET /compras/{codigo}/nfe]

+ Parameters
    + codigo (required, number, `1`) ... Código da compra

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)
Todos os XMLs associados a venda, e suas respectivas situações.
| Situação | Descrição |
|------|--------|
| `10` | Orçamento |
| `20` | Compra |

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 58

    + Body

        [
            {
                "codigo": "100",
                "chave": "43191014911562000100550010000150951741512345", 
                "situacao": "20",
                "nota": "<?xml version=\"1.0\"?><nfeProc xmlns=\"http://www.portalfiscal.inf.br/nfe\" versao=\"4.00\"><NFe xmlns=\"http://www.portalfiscal.inf.br/nfe\"><infNFe versao=\"4.00\"<ide><cUF>43</cUF><natOp>...</NFe>"
            }
            ,
            {
                "codigo": "101",
                "chave": "43191014911562000100550010000150951741512346", 
                "situacao": "20",
                "nota": "<?xml version=\"1.0\"?><nfeProc xmlns=\"http://www.portalfiscal.inf.br/nfe\" versao=\"4.00\"><NFe xmlns=\"http://www.portalfiscal.inf.br/nfe\"><infNFe versao=\"4.00\"<ide><cUF>43</cUF><natOp>...</NFe>"
            }
        ]


# Vendas / Ordens de serviço / OS [/vendas]

As vendas não podem ser editadas pela API. Caso seja necessário, apague-a e crie uma nova.

No eGestor, vendas podem ser de produtos e serviços. Vendas de serviços são consideradas OS. 

Para as OS valem os mesmos endpoint das vendas. Lembrando que é possível gerar notas fiscais de serviços (NFSe)

|Situação|Descrição|
|--------|---------|
|10|Orçamento|
|50|Venda|

### Listar (List) [GET /vendas{?filtro,dtTipo,dtIni,dtFim,vendedor,tipo,formaPgto,contaDest,situOS,buscaObs,fiscal,listarCanceladas,fields,orderBy}]

+ Parameters
    + filtro (optional, string) - Busca a string informada nos campos: código da venda, palavras-chave e nome do cliente.
    + dtTipo (optional, string) - Define a data que será utilizada pelos filtros dtIni e dtFim. Valores possíveis: 
        * dtVenda(default) - Data da venda.
        * dtCad - Data de cadastro.
        * dtEntrega - Data de entrega
    + dtIni (optional, date) - Data inicial, no formato yyyy-mm-dd
    + dtFim (optional, date) - Data final, no formato yyyy-mm-dd
    + vendedor (optional, integer) - Código do vendedor
    + tipo (optional, integer) - Filtra por venda ou orçamento. Valores possíveis: 
        * 10 - Orçamento.
        * 50 - Venda.
    + formaPgto (optional, integer) - Filtra pela forma de pagamento dos financeiros da venda.
    + contaDest (optional, integer) - Filtra pela conta destino dos financeiros da venda.
    + situOS (optional, string) - Situação da venda.
    + buscaObs (optional, string) - Pesquisa nas observações da venda.
    + fiscal (optional, string) - Permite filtrar por vendas com ou sem nota fiscal. Valores possíveis:
        * comNFe
        * semNFe
    + listarCanceladas (optional, boolean) - Define se as vendas canceladas também serão listadas
    + fields (optional) - Permite definir quais os campos serão retornados pela api. Informe separado por vírgula. Valores possíveis:
        * codigo, codContato, nomeContato, codVendedor, dtVenda, dtEntrega, dtCad, valorTotal, valorFrete, valorFinanc, valorEntrada, numParcelas, codsNFe, clienteFinal, situacao, situacaoOS, tags
        * ex: &fields=nomeContato,valorTotal
        * Padrão: codigo, codContato, nomeContato, codVendedor, dtVenda, valorTotal, valorFinanc, codsNFe, situacao, tags, ativo
    + orderBy (optional) - Permite definir a ordenação da listagem. Informe o campo e a forma de ordenação (ascendente ou descendente) separados por vírgula. Só é possível definir uma ordenação por requisição. Valores possíveis:
        * codigocodigo, codContato, codVendedor, clienteFinal, dtVenda, dtEntrega, dtCad, dtDel, valorTotal, valorFinanc, valorEntrada, numParcelas, tags, situOS, situacao, nomeContato, nomeVendedor
        * ex: &orderBy=nomeVendedor,desc
        * ex: &orderBy=dtVenda,asc
    

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)

          {
              "total": 1,
              "per_page": 50,
              "current_page": 1,
              "last_page": 4,
              "next_page_url": null,
              "prev_page_url": null,
              "from": 1,
              "to": 50,
              "data": [
                {
                    "codigo": 4,
                    "codContato": 7,
                    "nomeContato": "Cliente",
                    "codVendedor": 1,
                    "dtVenda": "2017-04-01",
                    "valorTotal": 500,
                    "valorFinanc": 500,
                    "codsNFe": [],
                    "situacao": 50,
                    "clienteFinal": 1,
                    "tags": [],
                    "ativo": true
                }
              ]
          }

+ Response 401 (application/json)

          {
              "errCode": 401,
              "errMsg": "Não foi possível acessar o sistema. Verifique seu \"access_token\".",
              "errObs": "access_denied",
              "errFields": null,
              "errUrl": "/v1/vendas"
          }

### Novo (Create) [POST]

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
              "codContato":1,
              "codVendedor":12,
              "dtVenda":"2022-09-14",
              "dtEntrega": "2022-09-14",
              "situacao": 50,
              "tags": ["VENDA_VIA_API" , "Tag2"],
              "valorFrete": 15.45,
              "valorDesc": 7.51,
              "valorDespesasAcessorias": 12.50,
              "clienteFinal":1,
              "enderecoEntrega":1,
              "situacaoOS": "Em espera",
              "customizado": {
                "xCampo1": "Mensagem do campo observações gerais",
                "xCampo2": "Mensagem do campo personalizado 2",
                "xCampo3": "Mensagem do campo personalizado 3",
                "xCampo4": "Mensagem do campo personalizado 4"
              },
              "produtos":[
                {
                "codProduto":17,
                "quant":1,
                "vDesc":0,
                "deducao":0,
                "preco":50.00,
                "obs":"SN: XYZ1020"
                },
                {
                "codProduto":22,
                "quant":10,
                "vDesc":0,
                "deducao":0,
                "preco":200.00,
                "obs":"Entregar em mãos"
                }
              ],
              "financeiros": [
                {
                "dtVenc":"2022-09-18",
                "dtComp":"2022-09-20",
                "pago": true,
                "codFormaPgto": 2,
                "codPlanoContas": 1,
                "codCaixa": 1,
                "valor": 100.00,
                "tags": ["API", "Parc1", "tag_do_financeiro"],
                "descricao": "Cobrança da venda de produtos, parcela 1/2"
                },
                {
                "dtVenc":"2022-09-18",
                "dtComp":"2022-09-20",
                "pago": false,
                "codFormaPgto": 4,
                "codPlanoContas": 1,
                "codCaixa": 1,
                "valor": 150.00,
                "tags": ["API", "Parc2", "tag_do_financeiro"],
                "descricao": "Cobrança da venda de produtos, parcela 2/2"
                }
              ],
              "despesas":[
               {
                "codFormaPgto": 4,
                "descricao": "Pagamento instalador",
                "codCaixa": 1,
                "pago": 1,
                "dtComp": "2022-09-30",
                "codPlanoContas": 25,
                "tags": ["instalador"],
                "codContato": "66",
                "numDoc": "doc001",
                "valor": 150
               }
              ]
            }

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
                "codigo": 111,
                "codVendedor": 1,
                "dtVenda": "2017-07-18",
                "valorTotal": 3453.45
            }

### Detalhar (Read) [GET /vendas/{codigo}]

+ Parameters
    + codigo (required, number, `1`) ... Código da venda

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)
  Todos os dados do contato
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 58

    + Body

            {
              "codigo": 1,
              "codContato": 1,
              "nomeContato": "Marcelo",
              "codVendedor": 2,
              "nomeVendedor": "João",
              "dtVenda": "2017-07-11",
              "dtEntrega": "2017-07-12",
              "dtCad": "2017-06-11 14:15:20",
              "valorTotal": 500,
              "valorFinanc": 500,
              "valorFrete": 15.45,
              "valorEntrada": 0,
              "numParcelas": 0,
              "codsNFe": [],
              "customizado": {
                "xCampo1": "Venda direta."
              },
              "clienteFinal": true,
              "situacao": 50,
              "situacaoOS": "Em espera",
              "tags": [],
              "publicURL": "https://v4.egestor.com.br/vendas/print.php?key=10540.626.3588.2457.d1432",
              "produtos": [
                {
                    "codigo": 26,
                    "codProduto": 17,
                    "tipo": "produto",
                    "preco": 0,
                    "custo": 110,
                    "custoCadProd": 110,
                    "quant": 1,
                    "vDesc": 0,
                    "valorIPI": 0,
                    "valorST": 0,
                    "valorFCPST": 0,
                    "obs": "",
                    "descricao": "Caneca preta",
                    "codigoProprio": "204"
                },
                {
                    "codigo": 267,
                    "codProduto": 17,
                    "tipo": "produto",
                    "preco": 500,
                    "custo": 110,
                    "custoCadProd": 110,
                    "quant": 1,
                    "vDesc": 0,
                    "valorIPI": 5,
                    "valorST": 0,
                    "valorFCPST": 0,
                    "obs": "",
                    "descricao": "Caneca preta",
                    "codigoProprio": "204"
                }
              ],
              "financeiros": [
                {
                    "codigo": 337,
                    "codBoleto": 0,
                    "codFormaPgto": 0,
                    "codPlanoContas": 1,
                    "descricao": "Cobrança da venda de produtos, parcela 1/1",
                    "codCaixa": 1,
                    "codRecibo": 0,
                    "situacao": 20,
                    "dtComp": "2017-07-11",
                    "tags": [],
                    "dtVenc": "2017-07-11",
                    "valor": 505,
                    "nomeFormaPgto": "Boleto"
                }
              ]
            }

+ Response 404 (application/json)
  Quando o registro não foi encontrado.
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
              "errCode": 404,
              "errMsg": "Nenhum registro com código 1 encontrado",
              "errObs": null,
              "errFields": null
            }

+ Response 410 (application/json)
  Quando o registro foi apagado do sistema.
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59
    + Body

            {
              "errCode": 410,
              "errMsg": "Registro com código 1 não existe.",
              "errObs": null,
              "errFields": null
            }

### Editar (Update) [PUT /vendas/{codigo}]
Só é possível alterar a situação (Venda/Orçamento) e a situação da O.S.
+ Request (application/json)

  + Headers

            Authorization: Bearer [access_token]

  + Parameters
      + codigo (required, number, `1`) ... Código da venda
      
  + Body 
            
            {
                "situacao": 50,
                "situacaoOS": "Entregue"
            }

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 58

    + Body

            {
              "codigo": 1,
              "codVendedor": 12,
              "dtVenda": "2022-10-31",
              "valorTotal": 74.73
            }

+ Response 400 (application/json)
 Quando o registro não foi encontrado.

    + Body

            {
              "errCode": 400,
              "errMsg": "Impossível salvar a venda.",
              "errObs": null,
              "errFields": null
            }

### Detalhar XML NFE (Read) [GET /vendas/{codigo}/nfe]

+ Parameters
    + codigo (required, number, `1`) ... Código da venda

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)
Todos os XMLs associados a venda, e suas respectivas situações.
| Situação | Descrição |
|------|--------|
| `5` | Criada |
| `10` | Em digitação |
| `15` | Rejeitada |
| `20` | Assinada |
| `30` | Validada |
| `35` | Form. Segur. |
| `40` | Enviada |
| `50` | Autorizada |
| `80` | Denegada |
| `90` | Cancelada |
| `91` | Inutilizada |

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 58

    + Body

        [
            {
                "id": "100",
                "situacao": "5",
                "nota": "<NFe xmlns=\"http://www.portalfiscal.inf.br/nfe\"><infNFe><ide><cUF>43</cUF><natOp>...</NFe>"
            }
            ,
            {
                "id": "101",
                "situacao": "50",
                "nota": "<NFe xmlns=\"http://www.portalfiscal.inf.br/nfe\"><infNFe><ide><cUF>43</cUF><natOp>...</NFe>"
            }

        ]

### Remover (Delete) [DELETE  /vendas/{codigo}]

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
                "code": "200",
                "msg": "Venda com código 317 excluída com sucesso!",
                "obs": null,
                "fields": null
            }


### Detalhar contato da venda [GET  /vendas/{codigo}/contato]

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
              "codigo": 324,
              "nome": "Kaya Labadie",
              "fantasia": "",
              "nomeParaContato": "Elfrieda Labadie",
              "cpfcnpj": "83294489654",
              "tipo": [
                "cliente"
              ],
              "dtNasc": "1992-02-13",
              "emails": [
                "exemplo@example.com.br"
              ],
              "fones": [],
              "cep": 4320040,
              "logradouro": "Rua Exemplo lado ímpar",
              "numero": "999",
              "complemento": "",
              "bairro": "",
              "codIBGE": "355030",
              "uf": "SP",
              "pais": "",
              "clienteFinal": true,
              "indicadorIE": 1,
              "inscricaoMunicipal": "",
              "inscricaoEstadual": "",
              "inscricaoEstadualST": "",
              "suframa": "",
              "obs": "",
              "tags": []
            }

### Gerar NFSe da venda [GET  /vendas/{codigo}/gerarNfse{?reterIssqn}]
Emite uma NFSe a partir de uma venda que possua serviços.
Não envia o documento automaticamente para o cliente.

+ Parameters
    + codigo (required, number, `1`) ... Código da venda
    + reterIssqn (optional, boolean) - Define se o ISSQN será retido na NFSe emitida

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
              "descricao": "Autorizado o uso da NFSe.",
              "numeroNfse": "1",
              "protocolo": "a8c217c3-7450-405b-9bd0-4bbee4c1e27e",
              "codVerificacao": "",
              "codNota": 335,
              "situacao": 10,
              "urlImpressao": "http://fiscal/nfse/imprimir.php?key=00.133.5a9b.614c.fe6",
              "emailCliente": "zipline@zipline.com.br"
            }
            
### Gerar NFSe da venda e enviar [GET  /vendas/{codigo}/gerarNfseEnviarEmail]
Emite uma NFSe a partir de uma venda que possua serviços.
Envia o documento automaticamente para o cliente por email.

+ Parameters
    + codigo (required, number, `1`) ... Código da venda

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
              "descricao": "Autorizado o uso da NFSe.",
              "numeroNfse": "1",
              "protocolo": "a8c217c3-7450-405b-9bd0-4bbee4c1e27e",
              "codVerificacao": "",
              "codNota": 335,
              "situacao": 10,
              "urlImpressao": "http://fiscal/nfse/imprimir.php?key=00.133.5a9b.614c.fe6",
              "emailCliente": "zipline@zipline.com.br",
              "emailEnviado": true
            }

### Buscar dados Mercado Livre [GET  /vendas/{codigo}/mercadoLivre]
Retorna os dados vindos da integração com o mercado livre, baseado no código da venda do eGestor.

+ Parameters
    + codigo (required, number, `1`) ... Código da venda
    
+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            [
               {
                    "codVenda": "1",
                    "dados": "{\"idOrder\": 1234567890, \"date_closed\": \"15\\/03\\/2022 12:58:00\", \"total_amount\": 20, \"paid_amount\": 20, \"payments\": [{\"transaction_amount\": 20, \"shipping_cost\": 0, \"payment_method_id\": \"elo\", \"date_created\": \"2022-03-15T11:57:59.000-04:00\"}], \"order_items\": [{\"idProduto\": \"MLB0000000000\", \"quantity\": 1, \"price\": 20, \"sale_fee\": 7.4, \"idProdutoEgestor\": \"123\"}], \"idBuyer\": 123456789, \"tracking_number\": \"RN123123333BR\", \"idBuyerEgestor\": \"99999\"}"
               }
            ]


### Gerar NFCe [POST  /vendas/{codigo}/gerarNfce]
Permite gerar uma NFCe a partir de determinada venda. Informe o campo `cpfcnpj` quando o contato da venda não possuir um CPF ou CNPJ definido no cadastro.
+ Parameters
    + codigo (required, number, `1`) ... Código da venda
    + indPres (required, number) ... Indicador de presença do consumidor
        + 0 = Não se aplica
        + 1 = Operação presencial
        + 2 = Operação não presencial, pela internet
        + 3 = Operação não presencial, teleatendimento
        + 4 = NFC-e em operação com entrega a domicílio
        + 5 = Operação presencial, fora do estabelecimento
        + 9 = Operação não presencial, outros

        

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body
            
            {
                "cpfcnpj": 12345678912,
                "indPres": 1,
                "codTransportadora": 0
            }


+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
              "codNFe": 123,
              "cStat": 100,
              "xMotivo": "Autorizado o uso da NF-e",
              "chaveAcesso": "1231231231231231231231231231231231",
              "linkDANFCe": "http://v4.egestor.com.br/painel/danfe.php?key=00.1148.6386.23b3.1ef",
              "autorizada": true,
              "ambiente": 2
            }

### Campos customizados [GET  /config/vendas/customizado]

O campo `customizado` possui até quatro índices:
* `xCampo1`;
* `xCampo2`;
* `xCampo3`;
* `xCampo4`;

Acesse este recurso (endpoint) para saber qual a descrição (label) de cada campo apresentado ao usuário.


+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
              "xCampo1": "Observa\u00e7\u00f5es gerais",
              "xCampo2": "Laudo técnico"
            }

# Boletos [/boletos]

Boletos não podem ser **editados** ou **excluídos** via API.

| Situação | Descrição |
|------|------------|
| `10` | Em aberto. |
| `50` | Recebido. |

### Listar (List) [GET /boletos{?filtro,dtTipo,dtIni,dtFim,codigosFin,situacaoBoleto,situacaoRemessa,situacaoFin,formaPgto,fields,orderBy}]
+ Parameters
    + filtro (optional, string) - Busca a string informada nos campos: código do boleto, nome do cliente ou descrição do financeiro.
    + dtTipo (optional, string) - Define a data que será utilizada pelos filtros dtIni e dtFim. Valores possíveis: 
        * dtVenc (default) - Data de vencimento.
        * dtEmissao - Data da emissão.
    + dtIni (optional, date) - Data inicial, no formato yyyy-mm-dd
    + dtFim (optional, date) - Data final, no formato yyyy-mm-dd
    + codigosFin (optional, string) - Lista de códigos dos financeiros, separados por virgula (ex.: 1,2,3)
    + situacaoBoleto (optional, integer) - Filtra pela situação atual do boleto. Valores possíveis:
        * 10 - Em aberto
        * 50 - Recebido
        * 70 - Boleto de teste
    + situacaoRemessa (optional, integer) - Filtra pela situação atual da remessa do boleto. Valores possíveis:
        * 5 - Pendência
        * 10 - Sem registro
        * 17 - Aguarda Retorno
        * 20 - Registrado
        * 50 - Pago
        * 105 - Pago (Pendência)
        * 110 - Pago (Sem registro)
        * 117 - Pago (Aguarda Retorno)
    + situacaoFin (optional, integer) - Filtra pela situação do financeiro vinculado ao boleto. Valores possíveis:
        * 20 - Financeiro a receber
        * 40 - Financeiro recebido
        * 80 - Financeiros em diferentes situações (quando boleto composto por mais de um financeiro)
        * 90 - Financeiro cancelado
    + formaPgto (optional, integer) - Filtra pela forma de pagamento do financeiro vinculado ao boleto.
    + fields (optional) - Permite definir quais os campos serão retornados pela api. Informe separado por vírgula.
        * ex: &fields=contatoNome,dtEmissao
    + orderBy (optional) - Permite definir a ordenação da listagem. Informe o campo e a forma de ordenação (ascendente ou descendente) separados por vírgula. Só é possível definir uma ordenação por requisição. Valores possíveis:
        * codigo, codRemessa, codContato, contatoNome, dtEmissao, dtVenc, dtPgto, dtCancelado, valorPago, situacao, sitRemessa
        * ex: &orderBy=contatoNome,desc
        * ex: &orderBy=dtEmissao,asc

+ Request (application/json)

  + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)

            {
                "total": 1,
                "per_page": 50,
                "current_page": 1,
                "last_page": 1,
                "next_page_url": null,
                "prev_page_url": null,
                "from": 1,
                "to": 50,
                "data": [
                  {
                      "codigo": 1,
                      "contatoNome": "",
                      "dtVenc": "2017-06-01",
                      "dtEmissao": "2017-06-07",
                      "valor": 100,
                      "valorPago": 0,
                      "codRecebimentos": [
                          "4"
                      ],
                      "situacao": "10"
                  }
                ]
            }

### Novo (Create) [POST]

+ Attributes (object)

    + codContato (number, required) - código do contato
    + dtVenc (string, required) - data de vencimento
    + codRecebimentos (array, required) - codigos dos recebimentos


+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
              "codContato" : 1,
              "dtVenc": "2017-10-14",
              "codRecebimentos" : [ "445" ]
            }

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
              "codigo": 195,
              "codContato": 1,
              "contatoNome": "Hugo",
              "contatoEmail": "hugo@example.com",
              "dtVenc": "2017-01-14",
              "dtEmissao": "2016-12-28",
              "valor": 100,
              "valorPago": 0,
              "codRecebimentos": [
                "445"
              ],
              "link": "http://link_para_arquivo_pdf",
              "situacao": "10"
            }


### Detalhar (Read) [GET /boletos/{codigo}]

+ Request (application/json)

  + Headers

            Authorization: Bearer [access_token]

  + Parameters
      + codigo (required, number, `1`) ... Código do boleto

+ Response 200 (application/json)
Quando o boleto está em aberto (ainda não pago).
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 58

    + Body

            {
              "codigo": 10,
              "codContato": 1,
              "contatoNome": "Hugo",
              "contatoEmail": "hugo@example.com",
              "dtVenc": "2016-04-14",
              "dtEmissao": "2016-03-28",
              "valor": 201,
              "valorPago": 0,
              "codRecebimentos": [
                "14", "15", "16"
              ],
              "link": "http://link_para_arquivo_pdf",
              "situacao": "10"
            }
            
+ Response 200 (application/json)
Quando o boleto já estiver pago.
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 58

    + Body

            {
              "codigo": 10,
              "codContato": 1,
              "contatoNome": "Hugo",
              "contatoEmail": "hugo@example.com",
              "dtVenc": "2016-04-14",
              "dtEmissao": "2016-03-28",
              "dtCredito": "2016-04-16",
              "valor": 201.00,
              "valorPago": 201.00,
              "codRecebimentos": [
                "14", "15", "16"
              ],
              "link": "http://link_para_arquivo_pdf",
              "situacao": "50"
            }

+ Response 410 (application/json)
 Quando o registro foi apagado do sistema.

    + Body

            {
              "errCode": 410,
              "errMsg": "Registro com código 10 não existe.",
              "errObs": null,
              "errFields": null
            }

+ Response 404 (application/json)
 Quando o registro não foi encontrado.

    + Body

            {
              "errCode": 404,
              "errMsg": "Nenhum registro com código 10 econtrado",
              "errObs": null,
              "errFields": null
            }
            
# Boleto eGestor - Verificar retorno  [/verificarRetornoBoletos]

### Verificar Retorno (Read) [GET /verificarRetornoBoletos]
Atualiza os recebimentos associados ao Boleto eGestor.

+ Request (application/json)

  + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)

    + Body

            {
              "ok": 1
            }



# Relatórios [/relatorios]

### Extrato Financeiro [POST /relatorios/extratoFinanceiro]
  Este relatório mostra a movimentação financeira já confirmada (já paga e já recebida) com o saldo.

  + Attributes (object)
      + de (string, required) - Data inicial, formato YYYY-MM-DD
      + ate (string, required) - Data final, formato YYYY-MM-DD
      + disponivel (number, optional) - Código da conta disponível
      + formaPgto (enum[number], optional) - Código da forma de pagamento
        + Members
          + `-1` - Forma de pagamento não definida
          + 0 - Não filtrar por forma de pagamento
          + 1,2,3, etc - Código da forma de pagamento
      + tags (string, optional) - Filtrar por palavras-chave do financeiro
      + comContato (boolean, optional) - Mostrar nome do contato
      + semSaldoAnterior (boolean, optional) - Ocultar saldo anterior
      + ordem (enum[number], optional) - Ordenação
        + Members
          + 1 - Data de pagamento
          + 2 - Código interno
          + 3 - Descrição
          + 4 - Valor
          + 5 - Contato
      
  + Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
                "de": "2018-07-01",
                "ate": "2018-09-21",
                "disponivel": 0,
                "formaPgto": -1,
                "tags": "",
                "comContato": true,
                "semSaldoAnterior": false
            }
              

  + Response 200 (application/json)

    + Headers

              X-RateLimit-Limit: 60
              X-RateLimit-Remaining: 59

    + Body

            {
                "dtPgto": "2018-07-04",
                "codigo": "1000",
                "descricao": "Venda código 246, parc 2/3",
                "contato": "João da Silva",
                "valor": 319.63,
                "saldo": 16944.80000000002
            }


### Fluxo de caixa periódico [POST /relatorios/fluxoDeCaixa]
  Fluxo de caixa separado em períodos.
  
  Os campos relacionados a data se comportam de forma diferente de acordo com o agrupamento definido.
| Agrupamento | Campos obrigatórios |
|-------------|---------------------|
| `dia` | `de` e `ate` |
| `semana` e `mes` | `mesIni`, `anoIni`, `mesFim` e `anoFim` |
| `bimestre`, `trimestre`, `semestre` e `ano` | `anoIni` e `anoFim` |

  + Attributes (object)
      + agrupamento (enum[string], required) - Agrupamento das informações do relatório
        + Members
          + dia
          + semana
          + mes
          + bimestre
          + trimestre
          + semestre
          + ano
      + tipoData (enum[string], required) - Tipo de data que será considerada para realizar os filtros
        + Members
          + dtPgto - Data de pagamento
          + dtCad - Data de cadastro
          + dtVenc - Data de vencimento
          + dtComp - Data de competência
      + mesIni (string, required) - Mês inicial
      + anoIni (string, required) - Ano inicial
      + mesFim (string, required) - Mês final
      + anoFim (string, required) - Ano final
      + de (string, optional) - Data inicial, formato YYYY-MM-DD
      + ate (string, optional) - Data final, formato YYYY-MM-DD
      + tags (string, optional) - Filtrar por palavras-chave do financeiro
      + semPlano (boolean, optional) - Lançamentos sem Plano de Contas
      + comTransf (boolean, optional) - Transferências entre contas
      + mostrarRecorrencia (boolean, optional) - Incluir recorrências
      
  + Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
                "agrupamento": "trimestre",
                "tipoData": "dtPgto",
                "anoIni": 2017,
                "anoFim": 2018,
                "tags": "",
                "semPlano": true,
                "comTransf": false,
                "mostrarRecorrencia": true
            }
            {
                "agrupamento": "dia",
                "tipoData": "dtCad",
                "de": "2018-12-01",
                "ate": "2018-12-31",
                "tags": "",
                "semPlano": true,
                "comTransf": false,
                "mostrarRecorrencia": true
            }
              

  + Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
                "receitas": [
                    {
                        "descr": "Receitas diversas",
                        "aJan2017": 5.29,
                        "aAbr2017": 4308.06,
                        "aJul2017": 0,
                        "aOut2017": 0,
                        "aJan2018": 0,
                        "aAbr2018": 0,
                        "aJul2018": 0,
                        "aOut2018": 0
                    }
                ],
                "despesas": [
                    {
                        "descr": "Gastos gerais",
                        "aJan2017": 0,
                        "aAbr2017": 1158.4,
                        "aJul2017": 89.9,
                        "aOut2017": 89.9,
                        "aJan2018": 0,
                        "aAbr2018": 4560,
                        "aJul2018": 1500,
                        "aOut2018": 10
                    }
                ],
                "resumo": [
                    {
                        "descr": "SALDO ANTERIOR",
                        "aJan2017": -53043.39,
                        "aAbr2017": -52111.009999999995,
                        "aJul2017": -47729.579999999994,
                        "aOut2017": -31688.409999999996,
                        "aJan2018": -1128.6999999999916,
                        "aAbr2018": -459.40999999999167,
                        "aJul2018": 1724.5600000000086,
                        "aOut2018": -2438.2299999999914
                    },
                    {
                        "descr": "TOTAL RECEITAS",
                        "aJan2017": 6271.799999999999,
                        "aAbr2017": 8557.93,
                        "aJul2017": 16675.129999999997,
                        "aOut2017": 41086.770000000004,
                        "aJan2018": 669.29,
                        "aAbr2018": 7221.63,
                        "aJul2018": 1399.71,
                        "aOut2018": 3098.14
                    },
                    {
                        "descr": "TOTAL DESPESAS",
                        "aJan2017": -5339.42,
                        "aAbr2017": -4176.5,
                        "aJul2017": -633.96,
                        "aOut2017": -10527.06,
                        "aJan2018": 0,
                        "aAbr2018": -5037.66,
                        "aJul2018": -5562.5,
                        "aOut2018": -5323
                    }
                ]
            }


### Fluxo financeiro [POST /relatorios/fluxoFinanceiro]
  Fluxo financeiro baseado no plano de contas.

  + Attributes (object)
      + tipoData (enum[string], required) - Tipo de data será considerada para realizar os filtros
        + Members
          + dtPgto - Data de pagamento
          + dtCad - Data de cadastro
          + dtVenc - Data de vencimento
          + dtComp - Data de competência
      + de (string, required) - Data inicial, formato YYYY-MM-DD
      + ate (string, required) - Data final, formato YYYY-MM-DD
      + tags (string, optional) - Filtrar por palavras-chave do financeiro
      + disponivel (number, optional) - Código da conta disponível
      + semPlano (number, optional) - Mostrar lançamentos sem Plano de Contas
      + comTransf (number, optional) - Mostrar transferências entre contas
      + comSomaGrupos (number, optional) - Mostrar somatório dos grupos
      + ordem (enum[number], optional) - Ordenação
        + Members
          + 1 - Ordem configurada
          + 2 - Descrição
          + 3 - Valor

  + Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
                "de": "2018-11-17",
                "ate": "2018-12-17",
                "tipoData": "dtPgto",
                "disponivel": 0,
                "tags": "",
                "semPlano": true,
                "comTransf": true,
                "comSomaGrupos": true
            }
              

  + Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
                "semPlano": {
                    "valorIn": null,
                    "valorOut": null
                },
                "planoCancelado": {
                    "valorIn": null,
                    "valorOut": null
                },
                "transfEntreContas": {
                    "valorIn": null,
                    "valorOut": null
                },
                "receitas": {
                    "Receitas diversas": [
                        {
                            "codigo": "9",
                            "nome": "Rendimentos financeiros",
                            "tipo": "receitas",
                            "pai": "Receitas diversas",
                            "valorIn": null,
                            "valorOut": null
                        }
                    ],
                },
                "despesas": {
                    "Gastos gerais": [
                       {
                            "codigo": "12",
                            "nome": "Água",
                            "tipo": "despesas",
                            "pai": "Gastos gerais",
                            "valorIn": null,
                            "valorOut": null
                        }
                    ]
                }
            }


### DRE [POST /relatorios/dre]
  Demonstrativo do Resultado do Exercício.

  + Attributes (object)
      + tipoData (enum[string], required) - Tipo de data será considerada para realizar os filtros
        + Members
          + dtPgto - Data de pagamento
          + dtVenc - Data de vencimento
          + dtComp - Data de competência
      + de (string, required) - Data inicial, formato YYYY-MM-DD
      + ate (string, required) - Data final, formato YYYY-MM-DD

  + Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body
            
                {
                    "tipoData": "dtComp",
                    "de": "2018-10-29",
                    "ate": "2018-12-17"
                }
              

  + Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
                {
                    "dre": "+ Receita bruta",
                    "valor": 25490.87
                },
                {
                    "dre": "- Impostos e tributos",
                    "valor": 0
                },
                {
                    "dre": "= Lucro líquido",
                    "valor": 25490.87
                },
                {
                    "dre": "- Despesas de venda",
                    "valor": 0
                },
                {
                    "dre": "- Despesas administrativas",
                    "valor": -10
                },
                {
                    "dre": "- Custo dos produtos (CMV)",
                    "valor": -35845.265514
                },
                {
                    "dre": "= Lucro operacional",
                    "valor": -10364.395514
                },
                {
                    "dre": "+/- Receitas/despesas diversas",
                    "valor": 0
                },
                {
                    "dre": "= Lucro/prejuízo",
                    "valor": -10364.395514
                }
            }
          

### Fluxo futuro [POST /relatorios/fluxoFuturoDetalhado]
  Este relatório mostra o fluxo de entrada e saída de dinheiro de acordo com as datas de vencimento dos lançamentos entre a data inicial e a data final. Todos os lançamentos já pagos e também os lançamentos com data de vencimento menor que a data inicial entram no "Saldo anterior".

  + Attributes (object)
      + de (string, required) - Data de vencimento inicial, formato YYYY-MM-DD
      + ate (string, required) - Data de vencimento final, formato YYYY-MM-DD
      + tags (string, optional) - Filtrar por palavras-chave do financeiro
      + disponivel (number, optional) - Código da conta disponível
      + semSaldoAnterior (number, optional) - Ocultar saldos e pgtos antes do início
      + semAtrasadasAnterior (number, optional) - Ocultar lanç. atrasados antes da data inícial

  + Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
                "disponivel": 0,
                "de": "2019-01-01",
                "ate": "2019-01-31",
                "tags": "",
                "semSaldoAnterior": true
            }
              

  + Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
                [
                    {
                        "dtVenc": "2019-01-02",
                        "aReceber": "17.92",
                        "aPagar": null,
                        "saldo": 17.92
                    },
                    {
                        "dtVenc": "2019-01-03",
                        "aReceber": "5.07",
                        "aPagar": null,
                        "saldo": 22.990000000000002
                    }
                ]
            }


### Fluxo futuro detalhado [POST /relatorios/fluxoFuturo]
  Este relatório mostra o fluxo de entrada e saída de dinheiro, detalhando cada lançamento cadastrado no financeiro, de acordo com as datas de vencimento dos lançamentos entre a data inicial e a data final. Todos os lançamentos já pagos e também os lançamentos com data de vencimento menor que a data inicial entram no "Saldo anterior".

  + Attributes (object)
      + de (string, required) - Data de vencimento inicial, formato YYYY-MM-DD
      + ate (string, required) - Data de vencimento final, formato YYYY-MM-DD
      + tags (string, optional) - Filtrar por palavras-chave do financeiro
      + disponivel (number, optional) - Código da conta disponível
      + comContato (number, optional) - Mostrar nome do contato associado ao lançamento
      + semSaldoAnterior (number, optional) - Ocultar saldos e pgtos antes do início
      + semAtrasadasAnterior (number, optional) - Ocultar lanç. atrasados antes da data inícial

  + Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
                "disponivel": 0,
                "de": "2019-01-01",
                "ate": "2019-01-31",
                "tags": "",
                "comContato": 1,
                "semSaldoAnterior": true
            }
              

  + Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
                [
                    {
                        "dtVenc": "2018-12-17",
                        "codigo": "1312",
                        "descricao": "Venda código 264, parc 3/3",
                        "contato": "Cliente Revenda XX",
                        "aReceber": "567.50",
                        "aPagar": "",
                        "saldo": 567.5
                    }
                ]
            }


### Lançamentos cancelados [POST /relatorios/lancamentosCancelados]
  Mostra a listagem de lançamentos cancelados no financeiro.

  + Attributes (object)
      + de (string, required) - Data de cancelamento inicial, formato YYYY-MM-DD
      + ate (string, required) - Data de cancelamento final, formato YYYY-MM-DD
      + ordem (enum[number], optional) - Ordenação
        + Members
          + 1 - Código interno
          + 2 - Descrição
          + 3 - Data cadastro
          + 4 - Data cancelamento
          + 5 - Valor
          + 6 - Usuário

  + Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body
        
            {
                "de": "2018-01-01",
                "ate": "2018-12-31"
            }
          

  + Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            [
                {
                    "codigo": "826",
                    "descricao": "Venda a prazo, código 184, parc 1/1",
                    "dtCad": "2018-01-12",
                    "dtDel": "2018-01-12",
                    "valor": "0.00",
                    "motivoDel": "",
                    "situacao": "20",
                    "usuario": "jean"
                }
            ]


### Financeiro Personalizado [POST /relatorios/personalizadoFinanceiro]
  Lista o financeiro com os campos escolhidos. O código e a descrição sempre serão listados.

  + Attributes (object)
      + tipoData (enum[string], required) - Tipo de data será considerada para realizar os filtros
        + Members
          + dtPgto - Data de pagamento
          + dtCad - Data de cadastro
          + dtVenc - Data de vencimento
          + dtComp - Data de competência
      + tipoFin (enum[number], required) - Tipo do financeiro
        + Members
          + 0 - Pagamentos e recebimento
          + 10 - Pagamentos
          + 20 - Recebimentos
          + 90 - Lançamentos cancelados
      + situacao (enum[number], required) - Situação atual dos financeiros
        + Members
          + 0 - Ambos (abertos e resolvidos)
          + 10 - Abertos (a pagar/a receber)
          + 20 - Já resolvidos (pagos/recebidos)
      + busca (string, optional) - Filtra por descrição os código dos finaceiros
      + contato (number, optional) - Código do contato
      + planoContas (number, optional) - Código do plano de contas
      + negativar (boolean, optional) - Valores negativos nas contas a pagar
      + semTransf (boolean, optional) - Esconder transferência entre contas
      + apenasFornTransport (boolean, optional) - Mostrar apenas registros de fornecedores e transportadoras
      + de (string, required) - Data inicial, formato YYYY-MM-DD
      + ate (string, required) - Data final, formato YYYY-MM-DD
      + tags (string, optional) - Filtrar por palavras-chave do financeiro
      + tagsContatos (string, optional) - Filtrar por palavras-chave do contato
      + formaPgto (enum[number], optional) - Código da forma de pagamento
        + Members
          + `-1` - Forma de pagamento não definida
          + 0 - Não filtrar por forma de pagamento
          + 1,2,3, etc - Código da forma de pagamento
      + disponivel (number, optional) - Código da conta disponível
      + ordem (enum[string], optional) - Campo para realizar o ordenação (ASC)
        + Members
            + `codigo`
            + `descricao`
            + `disponivel`
            + `contato`
            + `formaPgto`
            + `codRecibo`
            + `modulo`
            + `numDoc`
            + `situacao`
            + `valor`,
            + `taxa`
            + `dtCad`
            + `dtComp`
            + `dtVenc`
            + `dtPgto`
            + `obs`
            + `motivo`
            + `previsao`
            + `tags`
      + mostrar_disponivel (boolean, optional) - Mostrar conta disponível
      + mostrar_planoContas (boolean, optional) - Mostrar plano de contas
      + mostrar_contato (boolean, optional) - Mostrar dados do contato
      + mostrar_fantasia (boolean, optional) - Mostrar nome fantasia do contato
      + mostrar_cpfcnpj (boolean, optional) - Mostrar CPF/CNPJ do contato
      + mostrar_fones (boolean, optional) - Mostrar telefones do contato
      + mostrar_emails (boolean, optional) - Mostrar emails do contato
      + mostrar_endereco (boolean, optional) - Mostrar endereço do contato
      + mostrar_formaPgto (boolean, optional) - Mostrar forma de pagamento
      + mostrar_codRecibo (boolean, optional) - Mostrar código do recibo
      + mostrar_modulo (boolean, optional) - Mostrar módulo relacionado
      + mostrar_numDoc (boolean, optional) - Mostrar o número do documento relacionado
      + mostrar_situacao (boolean, optional) - Mostrar situação
      + mostrar_valor (boolean, optional) - Mostrar valor
      + mostrar_dtComp (boolean, optional) - Mostrar data de competência
      + mostrar_dtCad (boolean, optional) - Mostrar data de cadastro
      + mostrar_dtVenc (boolean, optional) - Mostrar data de vencimento
      + mostrar_dtPgto (boolean, optional) - Mostrar data de pagamento
      + mostrar_obs (boolean, optional) - Mostrar observações
      + mostrar_motivoDel (boolean, optional) - Mostrar motivo de cancelamento
      + mostrar_tags (boolean, optional) - Mostrar palavras-chave
      + mostrar_tagsContato (boolean, optional) - Mostrar palavras chave do contato

  + Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
                "tipoFin": 0,
                "situacao": 0,
                "busca": "",
                "contato": "",
                "contatoText": "",
                "disponivel": 0,
                "formaPgto": -1,
                "planoContas": 0,
                "tags": "",
                "tagsContatos": "",
                "tipoData": "dtVenc",
                "de": "2018-11-17",
                "ate": "2018-12-17",
                "negativar": true,
                "semTransf": false,
                "ordem": "contato",
                "comSaldoInicial": false,
                "mostrar_disponivel": true,
                "mostrar_planoContas": true,
                "mostrar_contato": true,
                "mostrar_fantasia": false,
                "mostrar_cpfcnpj": false,
                "mostrar_fones": false,
                "mostrar_emails": false,
                "mostrar_endereco": false,
                "mostrar_formaPgto": false,
                "mostrar_codRecibo": false,
                "mostrar_modulo": false,
                "mostrar_numDoc": false,
                "mostrar_situacao": false,
                "mostrar_valor": true,
                "mostrar_dtComp": true,
                "mostrar_dtCad": false,
                "mostrar_dtVenc": false,
                "mostrar_dtPgto": false,
                "mostrar_obs": true,
                "mostrar_motivoDel": true,
                "mostrar_tags": false,
                "mostrar_tagsContato": false
            }
              

  + Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            [
                {
                    "codigo": "1638",
                    "codContato": "0",
                    "codDisponivel": "1",
                    "codPlanoContas": "11",
                    "descricao": "Venda código 320, parc 1/1",
                    "valor": "856.00",
                    "dtComp": "2018-11-19",
                    "dtDel": "0000-00-00 00:00:00",
                    "motivoDel": "",
                    "obs": "",
                    "disponivel": "Caixa Interno",
                    "planoContas": "Venda de produtos/serviços",
                    "contato": "João da Silva"
                }
            ]


### Financeiro por vendedor [POST /relatorios/financeiroPorVendedor]
  Lista de lançamentos financeiros gerados a partir de vendas e seus vendedores.

  + Attributes (object)
      + responsavel (number, optional) - Código do vendedor 
      + de (string, required) - Data de vencimento/pagamento inicial, formato YYYY-MM-DD
      + ate (string, required) - Data de vencimento/pagamento final, formato YYYY-MM-DD
      + situacaoFinanceiro (enum[number], required) - Situação do financeiro
        + Members
          + 20 - A receber
          + 40 - Recebidos
      + ordem (enum[string], required) - Ordenar por
        + Members
          + nomeCliente - Nome do cliente
          + responsavel - Nome do vendedor
          + valorTotal - Valor total
          + dtVenc - Data do Vencimento
          + dtPgto - Data do pagamento

  + Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body
            
            {
                "responsavel": 0,
                "de": "2018-11-01",
                "ate": "2018-11-30",
                "situacaoFinanceiro": 20,
                "ordem": "nomeCliente"
            }
              

  + Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            [
                {
                    "codigo": "13",
                    "nomeCliente": "Ervilha Emilio",
                    "responsavel": "João da Silva",
                    "operacao": "Vendas: Cód. 266",
                    "situFinanc": "A receber",
                    "dtVenc": "2018-11-05",
                    "valorTotal": "22.00"
                }
            ]


### Comissões por financeiro [POST /relatorios/comissoesFinanceiro]
  Relatório de comissionamento baseado nos lançamentos financeiros já recebidos relativos às vendas.

  + Attributes (object)
      + de (string, required) - Data de pagamento inicial, formato YYYY-MM-DD
      + ate (string, required) - Data de pagamento final, formato YYYY-MM-DD
      + vendedor (number, optional) - Código do vendedor 
      + tags (string, optional) - Filtrar por palavras-chave do financeiro
      + ordem (enum[number], optional) - Ordenação
        + Members
          + 1 - Data pagamento
          + 2 - Código interno
          + 3 - Descrição
          + 4 - Cliente
          + 5 - Base de cálculo
          + 6 - Valor comissão
          + 7 - Data venda

  + Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body
    
            {
                "de": "2018-11-01",
                "ate": "2018-11-30",
                "vendedor": "",
                "tags": ""
            }
              

  + Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            [
                {
                    "codFinanc": "1625",
                    "descricao": "Venda código 316, parc 1/4",
                    "cliente": "João da Silva",
                    "data": "2018-11-14",
                    "baseCalc": "142.51",
                    "percComissao": "0.000",
                    "valorComissao": "0.000000000"
                }
            ]


### Custos das cobranças [POST /relatorios/custosDasCobrancas]
  Detalhamento dos custos relacionados às cobranças de acordo com a forma de pagamento utilizada.

  + Attributes (object)
      + tipoData (enum[string], required) - Tipo de data será considerada para realizar os filtros
        + Members
          + dtPgto - Data de pagamento
          + dtCad - Data de cadastro
          + dtVenc - Data de vencimento
          + dtComp - Data de competência
      + de (string, required) - Data inicial, formato YYYY-MM-DD
      + ate (string, required) - Data final, formato YYYY-MM-DD
      + formaPgto (enum[number], optional) - Código da forma de pagamento
        + Members
          + `-1` - Forma de pagamento não definida
          + 0 - Não filtrar por forma de pagamento
          + 1,2,3, etc - Código da forma de pagamento
      + situacao (enum[number], required) - Situação dos financeiros
        + Members
          + 0 - Recebidas e a receber
          + 10 - Apenas já recebidas
          + 20 - Apenas a receber
      + tags (string, optional) - Filtrar por palavras-chave do financeiro
      + ocultarZeradas (boolean, optional) - Ocultar cobranças com custo zerado
      + ordem (enum[number], optional) - Ordenação
        + Members
          + 1 - Código interno
          + 2 - Descrição
          + 3 - Forma de pgto
          + 4 - Data cadastro
          + 5 - Data vencimento
          + 6 - Valor
          + 7 - Taxa
          + 8 - Saldo

  + Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
                "tipoData": "dtCad",
                "de": "2018-11-01",
                "ate": "2018-11-30",
                "formaPgto": -1,
                "situacao": 0,
                "tags": "",
                "ocultarZeradas": 0
            }
              

  + Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            [
                {
                    "codigo": "1432",
                    "descricao": "Financeiro 123",
                    "dtCad": "2018-11-05",
                    "dtVenc": "2018-11-10",
                    "valor": "48.90",
                    "taxa": "0.00000",
                    "saldo": "48.90000",
                    "situacao": "20",
                    "formaPgto": "- sem forma pgto -"
                }
            ]


### Conciliação bancária [POST /relatorios/conciliacaoBancaria]
  Este relatório mostra os registros do banco conciliados ao financeiro.

  + Attributes (object)
    + de (string, required) - Data inicial, formato YYYY-MM-DD
    + ate (string, required) - Data final, formato YYYY-MM-DD
    + disponivel (number, optional) - Código da conta disponível
    + ordem (enum[number], optional) - Ordenação
        + Members
          + 1 - Data
          + 2 - Desc. conciliação
          + 3 - Desc. financeiro
          + 4 - Cód. conciliação
          + 5 - Cód. financeiro
          + 6 - Caixa
          + 7 - Transfer
          + 8 - Valor
          + 9 - Valor financeiro

  + Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
                "disponivel": "",
                "de": "2018-06-01",
                "ate": "2018-12-31"
            }
              

  + Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            [
                {
                    "cod": "48",
                    "data": "2018-09-03",
                    "codConc": "3",
                    "descrConc": "IOF",
                    "valor": "-6.89",
                    "codFin": "435",
                    "codDisponivel": "CEF",
                    "descrFin": "Teste",
                    "valorFin": "1000.00",
                    "codDisp2": null
                }
            ]


### Boletos [POST /relatorios/boletos]
  Relatório de Boletos.

  + Attributes (object)
    + contato (number, optional) - Código do contato
    + situacao (enum[number], required) - Situação do boleto
      + Members
        + 0 - Mostrar Todos
        + 10 - Não pago
        + 50 - Pago
        + 70 - Teste
        + 90 - Cancelado
    + sitRemessa (enum[number], required) - Situação da remessa
      + Members
        + 0 - Mostrar Todas
        + 5 - Pendência
        + 10 - Sem Registro
        + 17 - Aguarda Retorno
        + 20 - Registrado
        + 50 - Pago
        + 105 - Pago (Pendência)
        + 110 - Pago (Sem Registro)
        + 117 - Pago (Aguarda Retorno)
    + dtVenc (enum[number], required) - Define se irá filtrar os boletos com base na data de vencimento
     + Members
      + 1 - Filtrar
      + 2 - Não filtrar
    + dtVenc_de (string, optional) - Data de vencimento inicial, no formato YYYY-MM-DD. Obrigatório caso definido o atributo dtVenc como 1
    + dtVenc_ate (string, optional) - Data de vencimento final, no formato YYYY-MM-DD. Obrigatório caso definido o atributo dtVenc como 1
    + dtPgto (enum[number], required) - Define se irá filtrar os boletos com base na data de pagamento
     + Members
      + 1 - Filtrar
      + 2 - Não filtrar
    + dtPgto_de (string, optional) - Data de pagamento inicial, no formato YYYY-MM-DD. Obrigatório caso definido o atributo dtPgto como 1
    + dtPgto_ate (string, optional) - Data de pagamento final, no formato YYYY-MM-DD. Obrigatório caso definido o atributo dtPgto como 1
    + dtCred (enum[number], required) - Define se irá filtrar os boletos com base na data de crédito
     + Members
      + 1 - Filtrar
      + 2 - Não filtrar
    + dtCred_de (string, optional) - Data de crédito inicial, no formato YYYY-MM-DD. Obrigatório caso definido o atributo dtCred como 1
    + dtCred_ate (string, optional) - Data de crédito final, no formato YYYY-MM-DD. Obrigatório caso definido o atributo dtCred como 1
    + dtCanc (enum[number], required) - Define se irá filtrar os boletos com base na data de cancelamento
     + Members
      + 1 - Filtrar
      + 2 - Não filtrar
    + dtCanc_de (string, optional) - Data de cancelamento inicial, no formato YYYY-MM-DD
    + dtCanc_ate (string, optional) - Data de cancelamento final, no formato YYYY-MM-DD. Obrigatório caso definido o atributo dtCanc como 1
    + ordem (enum[string], required) - Ordenar por
      + Members
        + nomeCliente - Nome do cliente
        + codigo - Código do boleto
        + dtVenc - Data de vencimento
        + dtPgto - Data de pagamento
        + dtCancelado - Data de cancelamento
        + situacao - Situação do boleto
        + sitRemessa - Situação da remessa
    + valorDiferente (boolean, optional) - Valor pago diferente do valor do boleto


  + Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body
    
            {
                "contato": "",
                "situacao": 0,
                "sitRemessa": 0,
                "dtVenc": 1,
                "dtVenc_de": "2018-12-01",
                "dtVenc_ate": "2018-12-31",
                "dtPgto": 2,
                "dtCanc": 2,
                "ordem": "nomeCliente",
                "valorDiferente": false
            }
              

  + Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            [
                {
                  "codigo": "2122",
                  "nomeCliente": "João da Silva",
                  "dtVenc": "2018-12-28",
                  "dtPgto": null,
                  "dtCancelado": null,
                  "situacao": "Não pago",
                  "sitRemessa": "Registrado",
                  "valorPago": null,
                  "valor": "56.40"
                }
            ]
            
### Vendas detalhadas [POST /relatorios/vendasDetalhadas]
  Mostra a lista das vendas, com detalhes como quantidade, valor de venda, valor de custo, totais.

  + Attributes (object)
      + tipoData (enum[string], required) - Tipo de data que será considerada para realizar os filtros
        + Members
          + dtVenda - Data da venda
          + dtCad - Data de cadastro
          + dtEntrega - Data de entrega
      + de (string, required) - Data inicial, formato YYYY-MM-DD
      + ate (string, required) - Data final, formato YYYY-MM-DD
      + vendedor (number, optional) - Código do vendedor
      + contato (number, optional) - Código do cliente
      + codProduto (number, optional) - Filtra por vendas com o produto informado
      + codServico (number, optional) - Filtra por vendas com o serviço informado
      + situOS (enum[string], optional) - Situação da venda
        + Members
          + 'Em espera'
          + 'Em execução'
          + 'Finalizada'
          + 'Entregue'
      + tipoProd (enum[string], optional) - Filtrar apenas produtos ou serviços
        + Members
          + 'produto'
          + 'servico'
      + tags (string, optional) - Filtrar por palavras-chave da venda
      + ordem (enum[string], optional) - Ordenação padrão
        + Members
          + 'codigo'
          + 'cliente'
          + 'dtVenda'
          + 'dtCad'
          + 'vendedor'
      + formaPgto (number, optional) - Filtrar por forma de pagamento
      + contaDestino (number, optional) - Filtrar por conta destino
      + buscaEnd (string, optional) - Buscar no endereço do cliente
      + buscar_xCampos (string, optional) - Buscar nas observações da venda
      + mostrarvendasConcluidas (boolean, optional) - Filtrar apenas vendas concluídas
      + mostrarOrcamentos (boolean, optional) - Listar também orçamentos
      + mostrarObsProd (boolean, optional) - Mostrar as observações dos itens da venda
      + mostrarMLucro (boolean, optional) -  Mostrar margem de lucro
      + mostrarEndContato (boolean, optional) - Mostrar o endereço do contato
      + mostrarDesconto (boolean, optional) -  Mostrar desconto nos itens
      + listarIcProd (boolean, optional) - Listar o código próprio dos produtos
      + ocultarCustoLucro (boolean, optional) - Ocultar lucro e preço de custo
      + mostrarMarkup (boolean, optional) - Mostrar Markup
      + mostrarFoneContato (boolean, optional) - Mostrar telefone do cliente
      + mostrarVOutros (boolean, optional) - Mostrar despesas acessórias

  + Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
                "tipoData": "dtVenda",
                "de": "2019-03-01",
                "ate": "2019-03-31",
                "vendedor": "1",
                "contato": "",
                "codProduto": "",
                "codServico": "",
                "situOS": "",
                "tipoProd": "",
                "tags": "",
                "ordem": "cliente",
                "formaPgto": "",
                "contaDestino": "",
                "buscaEnd": "",
                "mostrarvendasConcluidas": 1,
                "mostrarOrcamentos": 1,
                "mostrarObsProd": 1,
                "mostrarMLucro": 1,
                "mostrarVOutros": 1,
                "mostrarEndContato": 1,
                "mostrarDesconto": 1,
                "listarIcProd": 1,
                "ocultarCustoLucro": 0,
                "mostrarMarkup": 1,
                "mostrarFoneContato": 1,
                "buscar_xCampos": ""
            }
              

  + Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            [
                {
                    "codVenda": "447",
                    "dtCad": "25/03/2019 11:28:40",
                    "dtVenda": "25/03/2019",
                    "cliente": " José da Silva",
                    "cpfcnpj": "12345678912",
                    "vendedor": "João",
                    "endereco": "Rua Marquês do Herval, 0, Nossa Senhora de Lourdes, Santa Maria - RS",
                    "fone": "",
                    "vendasItens": [
                        {
                            "codPerson": "AP",
                            "produto": "Arame farpado",
                            "tipoProd": "produto",
                            "quant": "10.0000",
                            "custo": 5,
                            "venda": 559.9,
                            "desconto": "0.0000",
                            "outros": "0.0000",
                            "obsProd": "",
                            "lucro": 554.9,
                            "markup": 11097.999999999998,
                            "mlucro": 99.10698338989106
                        },
                        {
                            "codPerson": "",
                            "produto": "Formatação de computadores 775",
                            "tipoProd": "servico",
                            "quant": "3.0000",
                            "custo": 0,
                            "venda": 272.70000000000005,
                            "desconto": "0.0000",
                            "outros": "0.0000",
                            "obsProd": "",
                            "lucro": 272.70000000000005,
                            "markup": 0,
                            "mlucro": 100
                        }
                    ]
                }
            ]
            
### Detalhes dos produtos vendidos [POST /relatorios/detalhesProdutosVendidos]
  Mostra cada produto vendido em uma linha, mesmo que repita o número da venda, nome do cliente, etc.

  + Attributes (object)
      + tipoData (enum[string], required) - Tipo de data que será considerada para realizar os filtros
        + Members
          + dtVenda - Data da venda
          + dtCad - Data de cadastro
          + dtEntrega - Data de entrega
      + de (string, required) - Data inicial, formato YYYY-MM-DD
      + ate (string, required) - Data final, formato YYYY-MM-DD
      + contato (number, optional) - Código do cliente
      + situOS (enum[string], optional) - Situação da venda
        + Members
          + 'Em espera'
          + 'Em execução'
          + 'Finalizada'
          + 'Entregue'
      + tags (string, optional) - Filtrar por palavras-chave da venda
      + tagsProduto (string, optional) - Filtrar por palavras-chave dos produtos
      + ordem (enum[string], optional) - Ordenação padrão
        + Members
          + 'categoria'
          + 'dtVenda'
          + 'dtCad'
          + 'produto'
          + 'tags'
          + 'tagsProduto'
          + 'vendedor'
          + 'codigo'
      + mostrarObs (boolean, optional) - Mostrar observação do produto
      + mostrarCategorias (boolean, optional) - Mostrar categorias do produto
      + mostrarCpfCnpj (boolean, optional) - Mostrar CPF/CNPJ do cliente
      + mostrarTags (boolean, optional) - Mostrar palavra chave das vendas
      + mostrarTagsProd (boolean, optional) - Mostrar pal. chave dos produtos
      + mostrarVendedor (boolean, optional) - Mostrar vendedor
      + mostrarDtCad (boolean, optional) - Mostrar data de cadastro da venda
      + mostrarDtVenda (boolean, optional) - Mostrar data da venda
      + mostrarCusto (boolean, optional) - Mostrar custo unitário do produto
      + mostrarApenasComNFE (boolean, optional) - Mostrar apenas vendas com NFE
      + mostrarCustoTotal (boolean, optional) - Mostrar total custo
      + mostrarVendaTotal (boolean, optional) - Mostrar total venda
      + mostrarLucro (boolean, optional) - Mostrar lucro
      + mostrarMarkup (boolean, optional) - Mostrar markup
      + mostrarMLucro (boolean, optional) - Mostrar margem lucro
      + mostrarCEST (boolean, optional) - Mostrar CEST
      + mostrarCEAN (boolean, optional) - Mostrar Referência EAN/GTIN
      + mostrarCFOP (boolean, optional) - Mostrar CFOP
      + mostrarGrupoTrib (boolean, optional) - Mostrar grupo tributário
      + mostrarNCM (boolean, optional) - Mostrar NCM
      + mostrarIcProd (boolean, optional) - Mostrar cód. próprio
      + listarServicos (boolean, optional) - Listar serviços
      + mostrarAbertas (boolean, optional) - Listar orçamentos
      + mostrarVOutros (boolean, optional) - Mostrar despesas acessórias

  + Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
                "de": "2019-03-01",
                "ate": "2019-03-31",
                "tipoData": "dtVenda",
                "contato": "",
                "tags": "",
                "tagsProd": "",
                "situOS": "",
                "ordem": "codigo",
                "mostrarObs": 1,
                "mostrarCategorias": 1,
                "mostrarCpfCnpj": 1,
                "mostrarTags": 1,
                "mostrarTagsProd": 1,
                "mostrarVendedor": 1,
                "mostrarDtCad": 1,
                "mostrarDtVenda": 1,
                "mostrarCusto": 1,
                "mostrarApenasComNFE": 0,
                "mostrarCustoTotal": 1,
                "mostrarVendaTotal": 1,
                "mostrarLucro": 1,
                "mostrarMarkup": 1,
                "mostrarMLucro": 1,
                "mostrarCEST": 1,
                "mostrarCEAN": 1,
                "mostrarCFOP": 1,
                "mostrarGrupoTrib": 1,
                "mostrarNCM": 1,
                "mostrarIcProd": 1,
                "listarServicos": 1,
                "mostrarVOutros": 1,
                "mostrarAbertas": 0
            }
              

  + Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            [
                {
                    "venda": "427",
                    "dtVenda": "2019-03-06",
                    "dtEntrega": "0000-00-00",
                    "dtCad": "2019-03-06 10:56:12",
                    "tags": "",
                    "situacao": "50",
                    "custo": "15.0000000000",
                    "quant": "1.0000",
                    "preco": "1.0000000000",
                    "vFrete": "0.0000",
                    "vOutro": 
                    "vDesc": "0.0000",
                    "vIPI": "0.0000",
                    "vST": "0.0000",
                    "total": "1.00000000000000",
                    "obs": "",
                    "tipoProd": "produto",
                    "custoTotal": "15.00000000000000",
                    "vendaTotal": "1.00000000000000",
                    "lucro": "-14.00000000000000",
                    "markup": "-93.33333333333333",
                    "mlucro": "-1400.00000000000000",
                    "produto": "Caneca Black",
                    "tagsProd": "todos",
                    "IcEAN": "1234567890128",
                    "INCM": "70131000",
                    "ICEST": "1400100",
                    "IcProd": "can001Preta",
                    "categoria": "Canecas",
                    "cliente": "José da Silva",
                    "cpfcnpj": "12345678912",
                    "vendedor": "João",
                    "cfop": "x102",
                    "codGrupTrib": "12"
                }
            ]
            
### Comissões de vendas [POST /relatorios/comissoesVendas]
  Lista as vendas por vendedor destacando a base dos produtos (valor bruto menos desconto), a porcentagem e o valor de comissão.

  + Attributes (object)
      + tipoData (enum[string], required) - Tipo de data que será considerada para realizar os filtros
        + Members
          + dtVenda - Data da venda
          + dtCad - Data de cadastro
          + dtEntrega - Data de entrega
      + de (string, required) - Data inicial, formato YYYY-MM-DD
      + ate (string, required) - Data final, formato YYYY-MM-DD
      + tagsContatos (string, optional) - Palavras-chave do cliente
      + tagsVendas (string, optional) - Palavras-chave das vendas
      + vendedor (number, optional) - Código do vendedor
      + situOS (enum[string], optional) - Situação da venda
        + Members
          + 'Em espera'
          + 'Em execução'
          + 'Finalizada'
          + 'Entregue'
      + ordem (enum[number], optional) - Ordenação
        + Members
          + 1 - Data
          + 2 - Código venda
          + 3 - Cliente
          + 4 - Base serviços
          + 5 - Base produtos
          + 6 - Valor comissão
      

  + Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
                "tipoData": "dtVenda",
                "de": "2019-03-01",
                "ate": "2019-03-31",
                "tagsContatos": "",
                "tagsVendas": "",
                "vendedor": "",
                "situOS": ""
            }
              

  + Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            [
                {
                    "vendedor": "José",
                    "comissaoServico": "5,00%",
                    "comissaoProduto": "5,00%",
                    "tags": "",
                    "vendas": [
                        {
                            "codVenda": "438",
                            "cliente": "João da Silva",
                            "data": "2019-03-15",
                            "totalServicos": "0.00",
                            "totalProdutos": "91.00000000000000",
                            "valorComissao": 4.55
                        }
                    ]
                }
            ]
            
### ABC de produtos vendidos [POST /relatorios/abcProdutosVendidos]
  Lista os itens vendidos em um determinado período ordenando-os do mais vendido ao menos, em quantidade, valor bruto ou lucratividade.

  + Attributes (object)
      + tipoData (enum[string], required) - Tipo de data que será considerada para realizar os filtros
        + Members
          + dtVenda - Data da venda
          + dtCad - Data de cadastro
          + dtEntrega - Data de entrega          
      + de (string, required) - Data inicial, formato YYYY-MM-DD
      + ate (string, required) - Data final, formato YYYY-MM-DD
      + contato (number, optional) - Código do cliente
      + vendedor (number, optional) - Código do vendedor
      + tagsVendas (string, optional) - Filtrar por palavras-chave da venda
      + tagsProdutos (string, optional) - Filtrar por palavras-chave dos produtos
      + tipoProd (enum[string], optional) - Filtrar apenas produtos ou serviços
        + Members
          + 'produto'
          + 'servico'
      + situOS (enum[string], optional) - Situação da venda
        + Members
          + 'Em espera'
          + 'Em execução'
          + 'Finalizada'
          + 'Entregue'
      + base (enum[string], optional) - Valor base
        + Members
          + 'precoTotal' - Total da venda
          + 'quantTotal' - Quantidade
          + 'lucroTotal' - Lucratividade
      + categoria (number, optional) - Filtrar por categoria de produto
      + tipoVenda (enum[string], optional) - Tipo da venda
        + Members
          + 'venda'
          + 'orcamento'
      + codsVenda (string, optional) - Filtrar por vendas específicas
      + mostrarApenasQuant (boolean, optional) - Mostrar apenas as quantidades
      + mostrarComDesconto (boolean, optional) - Considerar desconto nos itens vendidos
      + mostrarEstoque (boolean, optional) - Mostrar estoque atual
      + totalizarPeso (boolean, optional) -  Mostrar pesos dos produtos
      + listarIcProd (boolean, optional) - Listar cód. próprio
      + listarTags (boolean, optional) -  Mostrar palavras-chave dos itens

  + Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
                "tipoData": "dtCad",
                "de": "2019-03-01",
                "ate": "2019-03-31",
                "contato": "",
                "vendedor": "",
                "tagsVendas": "",
                "tagsProdutos": "",
                "tipoProd": "produto",
                "situOS": "",
                "base": "precoTotal",
                "categoria": "",
                "tipoVenda": "",
                "codsVenda": "",
                "mostrarApenasQuant": 0,
                "mostrarComDesconto": 1,
                "mostrarEstoque": 1,
                "totalizarPeso": 1,
                "listarIcProd": 1,
                "listarTags": 1
            }
              

  + Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            [
                {
                    "codPerson": "can001Preta",
                    "produto": "Caneca Black",
                    "precoMedio": 51,
                    "quant": "9.0000",
                    "estoque": "12",
                    "pesoB": "0.0000000",
                    "pesoL": "0.0000000",
                    "preco": "459.00000000000000",
                    "custo": "75.00000000000000",
                    "lucro": "384.00000000000000",
                    "margem": 512,
                    "share": 5.326061731260153,
                    "tagsItens": "todos"
                }
            ]
            
### ABC de vendas por cliente [POST /relatorios/abcClientesVendas]
  Lista os maiores clientes em valores vendidos em um determinado período ordenando-os do mais vendido ao menos.

  + Attributes (object)
      + tipoData (enum[string], required) - Tipo de data que será considerada para realizar os filtros
        + Members
          + dtVenda - Data da venda
          + dtCad - Data de cadastro
          + dtEntrega - Data de entrega          
      + de (string, required) - Data inicial, formato YYYY-MM-DD
      + ate (string, required) - Data final, formato YYYY-MM-DD
      + situOS (enum[string], optional) - Situação da venda
        + Members
          + 'Em espera'
          + 'Em execução'
          + 'Finalizada'
          + 'Entregue'
      + tagsVendas (string, optional) - Filtrar por palavras-chave da venda
      + tagsContatos (string, optional) - Filtrar por palavras-chave dos clientes
      + vendedor (number, optional) - Código do vendedor
      + base (enum[string], optional) - Valor base
        + Members
          + 'valor' - Valor
          + 'quant' - Quantidade
          + 'media' - Média por venda

  + Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
                "de": "2019-03-01",
                "ate": "2019-03-31",
                "tipoData": "dtVenda",
                "situOS": "",
                "tagsVendas": "", 
                "tagsContatos": "",
                "vendedor": "",
                "base": "valor"
            }
              

  + Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            [
                {
                    "codContato": "306",
                    "contato": "João da Silva",
                    "quant": "1",
                    "valor": "51946.00",
                    "media": 51946,
                    "share": 45.41143127396959
                }
            ]
            
### Vendas por tipo de documento [POST /relatorios/vendasPorTipoDocumento]
  Lista as vendas por vendedor, divididos por tipo de documento.

  + Attributes (object)
      + de (string, required) - Data inicial, formato YYYY-MM-DD
      + ate (string, required) - Data final, formato YYYY-MM-DD
      + vendedor (number, optional) - Código do vendedor
      + recebidas (boolean, optional) - Apenas com financeiro recebido
      + tags (string, optional) - Filtrar por palavras-chave da venda
      + situOS (enum[string], optional) - Situação da venda
        + Members
          + 'Em espera'
          + 'Em execução'
          + 'Finalizada'
          + 'Entregue'
      + tipoDoc (number, optional) - Tipo de documento, informe -1 para mostrar todos
      + ordem (enum[number], optional) - Ordenação
        + Members
          + 1 - Data da venda
          + 2 - Vendedor

  + Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
                "de": "2019-03-01",
                "ate": "2019-03-31",
                "vendedor": "",
                "recebidas": 0,
                "tags": "",
                "situOS": "",
                "tipoDoc": -1
            }
              

  + Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            [
                {
                    "data": "2019-03-26",
                    "vendedor": "José",
                    "Dinheiro 1X": 7502,
                    "Dinheiro 2X": 182,
                    "Dinheiro 4X": 0,
                    "total": 7684
                }
            ]
            
### Vendas sem valores [POST /relatorios/vendasSemValores]
  Mostra a lista de produtos vendidos, separados por vendas, sem informação de valores monetários, apenas a quantidade vendida de cada produto.

  + Attributes (object)
      + tipoData (enum[string], required) - Tipo de data que será considerada para realizar os filtros
        + Members
          + dtVenda - Data da venda
          + dtCad - Data de cadastro
          + dtEntrega - Data de entrega          
      + de (string, required) - Data inicial, formato YYYY-MM-DD
      + ate (string, required) - Data final, formato YYYY-MM-DD
      + codVendas (string, optional) - Código das vendas, separados por vírgula
      + tipoVenda (enum[string], optional) - Tipo da venda
        + Members
          + 'venda'
          + 'orcamento'
          + 'ambos'
      + vendedor (number, optional) - Código do vendedor
      + contato (number, optional) - Código do cliente
      + tags (string, optional) - Filtrar por palavras-chave da venda
      + situOS (enum[string], optional) - Situação da venda
        + Members
          + 'Em espera'
          + 'Em execução'
          + 'Finalizada'
          + 'Entregue'
      + mostrarObsAdic (boolean, optional) - Mostrar obs adicionais do produto
      + ordem (enum[number], optional) - Ordenação
        + Members
          + 1 - Produto
          + 2 - Tipo
          + 3 - Quantidade
          + 4 - Unidade

  + Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
                "tipoData": "dtVenda",
                "de": "2019-03-01",
                "ate": "2019-03-31",
                "codVendas": "",
                "tipoVenda": "venda",
                "vendedor": "",
                "contato": "",
                "tags": "",
                "situOS": "",
                "mostrarObsAdic": 1
            }
              

  + Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            [
                {
                    "codVenda": 427,
                    "vendasItens": [
                        {
                            "codProd": "9",
                            "codPerson": "can001Preta",
                            "produto": "Caneca Black",
                            "obsAdic": "",
                            "tipoProd": "produto",
                            "quant": "1.0000",
                            "unidade": "PC"
                        },
                        {
                            "codProd": "24",
                            "codPerson": "",
                            "produto": "Formatação de computadores",
                            "obsAdic": "",
                            "tipoProd": "servico",
                            "quant": "1.0000",
                            "unidade": ""
                        }
                    ]
                }
            ]

### Compras detalhadas [POST /relatorios/comprasDetalhadas]
  Mostra a lista das compras, com detalhes como quantidade, valor de compra e totais.

+ Attributes (object)
    + tipoData (enum[string], required) - Tipo de data que será considerada para realizar os filtros
        + Members
            + dtCompra - Data da compra
            + dtCad - Data de cadastro
    + de (string, required) - Data inicial, formato YYYY-MM-DD
    + ate (string, required) - Data final, formato YYYY-MM-DD
    + contato (number, optional) - Código do fornecedor
    + codProduto (number, optional) - Filtra por compras com o produto informado
    + situacao (enum[string], optional) - Situação da compra
        + Members
            + '10' - Orçamento
            + '50' - Concluída
    + tags (string, optional) - Filtrar por palavras-chave da compra
    + ordem (enum[string], optional) - Ordenação padrão
      + Members
        + 'codigo'
        + 'nome'
        + 'dtCompra'
        + 'dtCad'
        + 'valorTotal'
    + formaPgto (number, optional) - Filtrar por forma de pagamento
    + contaDestino (number, optional) - Filtrar por conta destino
    + mostrarFoneContato (boolean, optional) - Mostrar o telefone do fornecedor
    + mostrarEndContato (boolean, optional) - Mostrar o endereço do fornecedor
    + mostrarDesconto (boolean, optional) -  Mostrar desconto nos itens
    + listarIcProd (boolean, optional) - Listar o código próprio dos produtos
    + mostrarSaldoAtual(boolean, optional) - Mostrar o saldo atual dos produtos
    + mostrarTags(boolean, optional) - Mostrar palavra-chave das compras
    + mostrarFormaPgto(boolean, optional) - Mostrar forma de pagamento das compras

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            [
                {
                    "tipoData": "dtCompra",
                    "de": "2019-04-01",
                    "ate": "2019-12-30",
                    "contato": 0,
                    "situacao": 50,
                    "codProduto": 0,
                    "tags": "",
                    "formaPgto": 0,
                    "contaDestino": 0,
                    "ordem": "codigo",
                    "mostrarFormaPgto": 1,
                    "mostrarTags": 1,
                    "mostrarDesconto": 1,
                    "listarIcProd": 1,
                    "mostrarEndContato": 1,
                    "mostrarFoneContato": 1,
                    "mostrarSaldoAtual": 1
                }
            ]

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            [
                {
                    "codCompra": "1",
                    "dtCad": "2019-08-02 17:11:14",
                    "dtCompra": "2019-08-02",
                    "fornecedor": "Zipline Tecnologia Ltda",
                    "cpfcnpj": "25098282000132",
                    "endereco": "Rua Doutor Alberto Pasqualini, 111, Centro, Santa Maria - RS",
                    "fone": "5530263336",
                    "tags": [
                        "compra",
                        "zip"
                    ],
                    "formaPgto": "Não informada.",
                    "comprasItens": [
                        {
                            "codPerson": "KB123",
                            "produto": "Teclado 107 teclas padrão ABNT",
                            "quant": "5.0000",
                            "valorTotal": 150,
                            "desconto": "0.00",
                            "saldoAtual": "111.0000"
                        }
                    ]
                }
            ]
            
### Detalhes dos produtos comprados [POST /relatorios/detalhesProdutosComprados]
  Mostra cada produto comprado em uma linha, mesmo que repita o número da compra, nome do fornecedor, etc.

  + Attributes (object)
      + tipoData (enum[string], required) - Tipo de data que será considerada para realizar os filtros
        + Members
          + dtCompra - Data da compra
          + dtCad - Data de cadastro
      + de (string, required) - Data inicial, formato YYYY-MM-DD
      + ate (string, required) - Data final, formato YYYY-MM-DD
      + contato (number, optional) - Código do fornecedor
      + numNota (string, optional) - Número da nota fiscal
      + tags (string, optional) - Filtrar por palavras-chave da compra
      + observacoes (string, optional) - Filtrar nas observações gerais da compra
      + mostrarAbertas (boolean, optional) - Mostrar orçamentos
      + mostrarCodigo (boolean, optional) - Mostrar código interno do produto
      + mostrarIcProd (boolean, optional) - Mostrar código personalizado
      + mostrarCategorias (boolean, optional) - Mostrar categorias do produto
      + mostrarNumNota (boolean, optional) - Mostrar número da nota
      + mostrarCpfCnpj (boolean, optional) - Mostrar CPF/CNPJ do fornecedor
      + mostrarDtCad (boolean, optional) - Mostrar data de cadastro
      + mostrarDtCompra (boolean, optional) - Mostrar data da compra
      + mostrarDtNf (boolean, optional) - Mostrar data da Nota Fiscal
      + mostrarCustoBruto (boolean, optional) - Mostrar custo bruto do produto
      + mostrarCustoLiquido (boolean, optional) - Mostrar custo calculado do produto
      + mostrarTotalBruto (boolean, optional) - Total bruto do custo
      + mostrarTotalLiquido (boolean, optional) - Total calculado do custo
      + mostrarPrecoSugerido (boolean, optional) - Mostrar preço sugerido do produto
      + mostrarPrecoVenda (boolean, optional) - Mostrar preço atual do produto
      + mostrarTags (boolean, optional) - Mostrar palavras-chave
      + mostrarObserv (boolean, optional) - Mostrar observações compra
      + ordem (enum[number], optional) - Ordenação
        + Members
          + 1 - Código compra
          + 2 - Produto
          + 3 - Categoria
          + 4 - Fornecedor
          + 5 - Quantidade
          + 6 - Total bruto

  + Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
                "tipoData": "dtCad",
                "de": "2019-04-01",
                "ate": "2019-04-30",
                "contato": "",
                "numNota": "",
                "tags": "",
                "observacoes": "",
                "mostrarAbertas": true,
                "mostrarCodigo": true,
                "mostrarIcProd": true,
                "mostrarCategorias": true,
                "mostrarNumNota": true,
                "mostrarCpfCnpj": true,
                "mostrarDtCad": true,
                "mostrarDtCompra": true,
                "mostrarDtNf": true,
                "mostrarCustoBruto": true,
                "mostrarCustoLiquido": true,
                "mostrarTotalBruto": true,
                "mostrarTotalLiquido": true,
                "mostrarPrecoSugerido": true,
                "mostrarPrecoVenda": true,
                "mostrarTags": true,
                "mostrarObserv": true
            }
              

  + Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            [
                {
                    "compra": "1",
                    "dtCompra": "2019-04-04",
                    "dtCad": "2019-04-04",
                    "numNota": "",
                    "tags": "",
                    "obs": "",
                    "codProd": "1",
                    "quant": "500.0000",
                    "custoBruto": "7.5500000000",
                    "totalBruto": "3775.00000000000000",
                    "custoCalculado": "7.5500000000",
                    "precoSugerido": "14.0000000000",
                    "totalLiquido": "3775.00000000000000",
                    "produto": "Produto padrão",
                    "IcProd": "codigo interno",
                    "precoVenda": "15.0000000000",
                    "categoria": "Material de escritório",
                    "fornecedor": "Fornecedor padrão",
                    "cpfcnpj": "1234567891011"
                }
            ]

### Produções detalhadas [POST /relatorios/producoesDetalhadas]
  Lista de produções abertas ou concluídas.

  + Attributes (object)
      + tipoData (enum[string], required) - Tipo de data que será considerada para realizar os filtros
        + Members
          + dtInicio - Data de início
          + dtConclusao - Data de conclusão
      + de (string, required) - Data inicial, formato YYYY-MM-DD
      + ate (string, required) - Data final, formato YYYY-MM-DD
      + tags (string, optional) - Filtrar por palavras-chave da produção
      + situacao (enum[number], required) - Situação da produção
        + Members
          + 0 - Todas as situações
          + 10 - Somente em aberto
          + 50 - Somente concluidas
      + cods (string, optional) - Código das produções, separados por virgula
      + esconderValores (boolean, optional) - Esconder valor monetário
      + mostrarQntPerda (boolean, optional) - Mostrar quantidade da perda

  + Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
                "tipoData": "dtInicio",
                "de": "2019-04-01",
                "ate": "2019-04-30",
                "tags": "",
                "situacao": 0,
                "cods": "",
                "esconderValores": false,
                "mostrarQntPerda": true
            }
              

  + Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            [
                {
                    "cod": "12",
                    "insumos": [
                      {
                        "tipo": "Insumo",
                        "codProduto": "5",
                        "IxProd": "Insumo 1",
                        "IcProd": "ins",
                        "IuCom": "GR",
                        "quant": 0.12,
                        "pPerda": 0,
                        "qntPerda": 0,
                        "custoInsumo": 6.12,
                        "custoUnit": 51,
                        "custoExtra": 0,
                        "custo": 6.12
                      },
                      {
                        "tipo": "Insumo",
                        "codProduto": "6",
                        "IxProd": "Insumo 2",
                        "IcProd": "ins2",
                        "IuCom": "Quilo",
                        "quant": 3.6,
                        "pPerda": 0.02,
                        "qntPerda": 0.0007,
                        "custoInsumo": 32.04,
                        "custoUnit": 8.9,
                        "custoExtra": 0,
                        "custo": 32.04
                      }
                    ],
                    "produto": [
                      {
                        "tipo": "Produto",
                        "codProduto": "2",
                        "IxProd": "Produto final",
                        "IcProd": "prodFin",
                        "IuCom": "UN",
                        "quant": 12,
                        "pPerda": 0,
                        "qntPerda": 0,
                        "custoInsumo": 62.16,
                        "custoUnit": 5.18,
                        "custoExtra": 222,
                        "custo": 284.15999999999997
                      }
                    ]
                }
            ]

### Composições [POST /relatorios/composicoes]
  Lista as composições cadastradas para cada produto final.

  + Attributes (object)
      + codProds (string, optional) - Código dos produtos, separados por vírgula
      + categoria (number, optional) - Código da categoria de produto
      + tags (string, optional) - Filtrar por palavras-chave dos produtos

  + Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
                "codProds": "",
                "categoria": "",
                "tags": ""
            }
              

  + Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            [
                {
                  "cod": "2",
                  "produto": "Produto final",
                  "insumos": [
                    {
                        "codInsumo": "5",
                        "insumo": "Insumo 1",
                        "codProprio": "ins",
                        "unidade": "GR",
                        "quant": "20.12",
                        "pPerda": "0"
                    },
                    {
                        "codInsumo": "6",
                        "insumo": "Insumo 2",
                        "codProprio": "ins2",
                        "unidade": "Quilo",
                        "quant": "3.6",
                        "pPerda": "0.02"
                    }
                  ]
                }
            ]

### Histórico de um contato [POST /relatorios/historicoContato]
  Mostra o histórico de um contato (cliente ou fornecedor) nos módulos do sistema.

  + Attributes (object)

      + contato (number, required) - Código do contato
      + de (string, required) - Data inicial, formato YYYY-MM-DD
      + ate (string, required) - Data final, formato YYYY-MM-DD
      + dtFinanceiro (enum[string], required) - Tipo de data do financeiro que será considerada para realizar os filtros
        + Members
          + dtVenc - Data de vencimento
          + dtCad - Data de cadastro
          + dtPgto - Data de pagamento
          + naoMostrar - Não listar financeiros
      + dtVendas (enum[string], required) - Tipo de data da venda que será considerada para realizar os filtros
        + Members
          + dtVenda - Data da venda
          + dtEntrega - Data da entrega
          + naoMostrar - Não listar vendas
      + dtCompras (enum[string], required) - Tipo de data da compra que será considerada para realizar os filtros
        + Members
          + dtCompra - Data da compra
          + dtCad - Data de cadastro
          + naoMostrar - Não listar compras
      + ordem  (enum[string], required) - Ordenação dos registros
        + Members
          + data
          + codigo
          + descricao
          + valor
          + situacao

  + Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
                "contato": "109",
                "de": "2019-04-16",
                "ate": "2019-05-16",
                "dtFinanceiro": "dtVenc",
                "dtVendas": "dtVenda",
                "dtCompras": "dtCompra",
                "ordem": "valor"
            }
              

  + Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            [
              "Financeiro": [
                {
                  "data": "2019-05-17",
                  "codigo": "2471",
                  "descricao": "Compra 206",
                  "valor": "477.00",
                  "situacao": "Pago"
                },
                {
                  "data": "2019-05-15",
                  "codigo": "2467",
                  "descricao": "Venda código 586, parc 1/1 nota ",
                  "valor": "73.50",
                  "situacao": "A receber"
                },
              ],
              "Vendas": [
                {
                  "data": "2019-05-15",
                  "codigo": "586",
                  "descricao": "Venda teste",
                  "valor": "73.50",
                  "situacao": "Concluída"
                }
              ],
              "Compras": [
                {
                  "data": "2019-05-17",
                  "codigo": "206",
                  "descricao": "",
                  "valor": "477.00",
                  "situacao": "Concluída"
                }
              ]
            ]

### Personalizado de contatos [POST /relatorios/personalizadoContatos]
  Lista os contatos com os campos escolhidos. O nome do contato e seu código interno sempre são listados.

  + Attributes (object)

      + contato (number, required) - Código do contato
      + tipoContato (enum[number], optional) - Tipo de contato
        + Members
          + 1 - Clientes
          + 2 - Fornecedores
          + 4 - Transportadoras
      + tags (string, optional) - Filtrar por palavras-chave dos contatos
      + tipoData (enum[string], required) - Tipo de data que será considerada para realizar os filtros
        + Members
          + dtCad - Data de cadastro
          + dtNasc - Data de nascimento
      + de (string, required) - Data inicial, formato YYYY-MM-DD
      + ate (string, required) - Data final, formato YYYY-MM-DD
      + ordenarPor (enum[string], required) - Ordenação dos registros
        + Members
          + codigo
          + nome
          + fantasia
          + dtNasc
      + mostrar_tipo (boolean, optional) - Mostrar tipo
      + mostrar_fantasia (boolean, optional) - Mostrar nome fantasia
      + mostrar_cpfcnpj (boolean, optional) - Mostrar CPF/CNPJ
      + mostrar_ie (boolean, optional) - Mostrar IE
      + mostrar_indIE (boolean, optional) - Mostrar indicador de IE 
      + mostrar_iest (boolean, optional) - Mostrar IE ST
      + mostrar_im (boolean, optional) - Mostrar IM
      + mostrar_suframa (boolean, optional) - Mostrar Suframa
      + mostrar_emails (boolean, optional) - Mostrar e-mails
      + mostrar_end (boolean, optional) - Mostrar endereço
      + mostrar_num (boolean, optional) - Mostrar número
      + mostrar_compl (boolean, optional) - Mostrar complemento
      + mostrar_bairro (boolean, optional) - Mostrar bairro
      + mostrar_cidadeNome (boolean, optional) - Mostrar cidade
      + mostrar_cidadeCod (boolean, optional) - Mostrar cód. IBGE
      + mostrar_uf (boolean, optional) - Mostrar UF
      + mostrar_cep (boolean, optional) - Mostrar CEP
      + mostrar_pais (boolean, optional) - Mostrar país
      + mostrar_fones (boolean, optional) - Mostrar fones
      + mostrar_dtNasc (boolean, optional) - Mostrar data de nascimento
      + mostrar_dtCad (boolean, optional) - Mostrar data de cadastro
      + mostrar_tags (boolean, optional) - Mostrar palavras-chave
      + mostrar_obs (boolean, optional) - Mostrar anotações

  + Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
              "contato": "",
              "tipoContato": "",
              "tags": "",
              "tipoData": "dtCad",
              "de": "",
              "ate": "",
              "ordenarPor": "codigo",
              "mostrar_tipo": false,
              "mostrar_fantasia": true,
              "mostrar_cpfcnpj": true,
              "mostrar_ie": true,
              "mostrar_indIE": true,
              "mostrar_iest": true,
              "mostrar_im": true,
              "mostrar_suframa": true,
              "mostrar_emails": true,
              "mostrar_end": true,
              "mostrar_num": true,
              "mostrar_compl": true,
              "mostrar_bairro": true,
              "mostrar_cidadeNome": true,
              "mostrar_cidadeCod": true,
              "mostrar_uf": true,
              "mostrar_cep": true,
              "mostrar_pais": true,
              "mostrar_fones": true,
              "mostrar_dtNasc": true,
              "mostrar_dtCad": true,
              "mostrar_tags": true,
              "mostrar_obs": true
            }
              

  + Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            [
              "codigo": "1",
              "nome": "Cliente padrão",
              "fantasia": "",
              "cpfcnpj": "",
              "ie": "",
              "indIE": "Não informado",
              "iest": "",
              "im": "",
              "suframa": "",
              "emails": "",
              "end": "",
              "num": "",
              "compl": "",
              "bairro": "",
              "cidadeNome": "Santa Maria",
              "cidadeCod": "",
              "uf": "RS",
              "cep": "",
              "pais": "",
              "fones": "",
              "dtNasc": "0000-00-00",
              "dtCad": "2016-10-17 15:40:50",
              "tags": "",
              "obs": ""
            ]

### Aniversariantes do mês [POST /relatorios/aniversariantesDoMes]
  Lista dos contatos aniversariantes do mês selecionado.

  + Attributes (object)

      + mes (number, required) - Mês de referência, iniciando em 0 (Janeiro)
      + mostrarEmails (boolean, optional) - Mostrar email do contato
      + mostrarFones (boolean, optional) - Mostrar fone do contato
      + ordem (enum[number], optional) - Ordenação
        + Members
          + 1 - Código
          + 2 - Nome
          + 3 - Data de nascimento

  + Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
              "mes": 2,
              "mostrarEmails": true,
              "mostrarFones": true
            }
              

  + Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            [
              "codigo": "16",
              "nome": "Cliente padrão",
              "emails": "cliente@padrao.com",
              "fones": "12345677",
              "dtNasc": "1981-03-29"
            ]

### Inatividade de clientes [POST /relatorios/inatividadeClientes]
  Lista de clientes inativos.

  + Attributes (object)

      + tipoInatividade (enum[string], required) - Tipo do período de inatividade
        + Members
          + maiorque
          + menorque
      + diasInativos (number, required) - Período de inatividade (dias)
      + tags (string, optional) - Filtrar por palavras-chave dos clientes
      + tipoDataVendas (enum[string], required) - Data utilizada na venda
        + Members
          + dtVenda
          + dtCad
          + dtEntrega
      + ordem (enum[string], required) - Ordenar por
        + Members
          + nomeContato
          + inatividade
      + considerarOrcamentos (boolean, optional) - Considera orçamentos

  + Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
              "tipoInatividade": "maiorque",
              "diasInativos": 50,
              "tags": "",
              "tipoDataVendas": "dtVenda",
              "ordem": "inatividade",
              "considerarOrcamentos": false
            }
              

  + Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            [
              {
                "codigo": "1",
                "nome": "Cliente padrão",
                "dataUltOper": "2019-03-22",
                "codUltOper": "Venda: 445",
                "inatividade": 56
              }
            ]

### Contatos duplicados [POST /relatorios/contatosDuplicados]
  Este relatório mostra todos os contatos que estão cadastrados em duplicidade no sistema.

  + Attributes (object)

      + ordem (enum[string], required) - Ordenar por
        + Members
          + nome
          + quantidade - Quantidade de vezes que o registro se repete
      + usarCpfCnpj (boolean, optional) - Considerar o CNPJ ao comparar
      + usarIe (boolean, optional) - Considerar a Inscrição estadual ao comparar

  + Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
              "ordem": "quantidade",
              "usarCpfCnpj": true,
              "usarIe": true
            }
              

  + Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            [
              {
                "vezes": "4",
                "nome": "Cliente modelo",
                "cpfcnpj": "12345678912345",
                "ie": "ISENTO",
                "codigos": "56, 57, 58, 55"
              }
            ]

### Produtos por fornecedor [POST /relatorios/produtosPorFornecedor]
  Relatório de produtos por fornecedor com preço de compra.

  + Attributes (object)
      + tipoData (enum[string], required) - Tipo de data que será considerada para realizar os filtros
        + Members
          + dtCompra - Data da compra
          + dtCad - Data de cadastro
      + de (string, required) - Data inicial, formato YYYY-MM-DD
      + ate (string, required) - Data final, formato YYYY-MM-DD
      + categoria (number, optional) - Código da categoria
      + produto (number, optional) - Código do produto
      + contato (number, optional) - Código do fornecedor
      + agruparForn (boolean, optional) - Agrupar por fornecedor
      + agruparProd (boolean, optional) - Juntar produtos iguais e fazer preço médio
      + apresentarArquivados (boolean, optional) - Apresentar produtos arquivados
      + ordem (enum[number], optional) - Ordenação
        + Members
          + 1 - Fornecedor
          + 2 - Produto
          + 3 - Código compra
          + 4 - Data compra
          + 5 - Custo bruto
          + 6 - Quantidade
          + 7 - Desconto
          + 8 - Despesas
          + 9 - IPI
          + 10 - ST
          + 11 - Total

  + Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
                "tipoData": "dtCad",
                "de": "2019-02-01",
                "ate": "2019-04-30",
                "categoria": "",
                "produto": "",
                "contato": "",
                "agruparProd": false,
                "agruparForn": false,
                "apresentarArquivados": false
            }
              

  + Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            [
                {
                    "codigo": "128",
                    "fornec": "Fornecedor padrão S.A.",
                    "codProduto": "12",
                    "produto": "Produto padrão",
                    "data": "2019-04-11",
                    "custoOuMedia": "25.0000000000",
                    "vDesc": "0.00",
                    "vDesp": "0.00",
                    "vIPI": "0.00",
                    "vST": "0.00",
                    "quant": "100.0000",
                    "total": "2500.00000000000000"
                },
            ]
            
### Estoque mínimo [POST /relatorios/estoqueMinimo]
  Mostra a lista de produtos que estão com o estoque mínimo abaixo do valor definido em seu cadastro. Para um produto não aparecer nessa lista, seu estoque mínimo deve estar definido com 0 (zero).

  + Attributes (object)
      + buscar (string, optional) - Apenas produtos que contenham a(s) palavra(s)
      + categoria (number, optional) - Código da categoria
      + mostrarIgual (boolean, optional) - Também mostrar com estoque igual ao mínimo
      + apresentarArquivados (boolean, optional) - Apresentar produtos arquivados
      + ordem (enum[number], optional) - Ordenação
        + Members
          + 1 - Código
          + 2 - Categoria
          + 3 - Produto
          + 4 - Estoque
          + 5 - Mínimo
          + 6 - Custo unit.

  + Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
                "buscar": "",
                "categoria": "",
                "mostrarIgual": false,
                "apresentarArquivados": false
            }
              

  + Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            [
                {
                    "codigo": "20",
                    "produto": "Produto padrão",
                    "estoque": "1.0000",
                    "minimo": "2.0000",
                    "reservado": "0.0000",
                    "oc": "0.0000",
                    "custo": "12.5500000000",
                    "saldo": "1.0000",
                    "categoria": "Arames"
                },
            ]
            
### Estoque em data específica [POST /relatorios/estoqueDoDia]
  Saldo em estoque em uma determinada data.

  + Attributes (object)
      + dia (string, required) - Data desejada, formato YYYY-MM-DD
      + categoria (number, optional) - Código da categoria
      + tags (string, optional) - Filtrar por palavras-chave dos produtos
      + semExcluidos (boolean, optional) - Ocultar produtos excluídos
      + semEstNaoControl (boolean, optional) - Ocultar produtos marcados para não controlar estoque
      + mostrarEstoqueNegativo (boolean, optional) - Mostrar estoque negativo
      + mostrarCodProprio (boolean, optional) - Mostrar código próprio do produto
      + apresentarArquivados (boolean, optional) - Apresentar produtos arquivados
      + ordem (enum[number], optional) - Ordenação
        + Members
          + 1 - Produto
          + 2 - Código
          + 3 - Estoque
          + 4 - Custo
          + 5 - Total
      + mostrar_categNome (boolean, optional) - Mostrar categoria
      + mostrar_codConfigTrib (boolean, optional) - Mostrar código grupo tributo
      + mostrar_precoCusto (boolean, optional) - Mostrar preço custo
      + mostrar_precoVenda (boolean, optional) - Mostrar preço venda
      + mostrar_margem (boolean, optional) - Mostrar margem cadastrada
      + mostrar_margemAtual (boolean, optional) - Mostrar margem atual
      + mostrar_controlarEstoque (boolean, optional) - Mostrar controla estoque
      + mostrar_estoque (boolean, optional) - Mostrar quant. estoque
      + mostrar_totalCusto (boolean, optional) - Mostrar total (custo x quant)
      + mostrar_totalVenda (boolean, optional) - Mostrar total (venda x quant)
      + mostrar_estoqueMinimo (boolean, optional) - Mostrar estoque mínimo
      + mostrar_tipoUso (boolean, optional) - Mostrar tipo de uso
      + mostrar_anotacoes (boolean, optional) - Mostrar anotações gerais
      + mostrar_VinfAdProd (boolean, optional) - Mostrar anotações NFe
      + mostrar_tags (boolean, optional) - Mostrar palavras-chave
      + mostrar_IcEAN (boolean, optional) - Mostrar EAN/GTIN
      + mostrar_INCM (boolean, optional) - Mostrar NCM
      + mostrar_ICEST (boolean, optional) - Mostrar CEST
      + mostrar_IEXTIPI (boolean, optional) - Mostrar EXTIPI
      + mostrar_IuCom (boolean, optional) - Mostrar unidade
      + mostrar_pesoB (boolean, optional) - Mostrar peso bruto
      + mostrar_pesoL (boolean, optional) - Mostrar peso líquido
      + mostrar_Norig (boolean, optional) - Mostrar origem
      + mostrar_cfop (boolean, optional) - Mostrar CFOP
      + mostrar_NCST (boolean, optional) - Mostrar código ICMS
      + mostrar_NpICMS (boolean, optional) - Mostrar alíquota ICMS
      + mostrar_NpICMSST (boolean, optional) - Mostrar alíquota ICMS ST
      + mostrar_NpMVAST (boolean, optional) - Mostrar MVA ST
      + mostrar_OCST (boolean, optional) - Mostrar código IPI
      + mostrar_OpIPI (boolean, optional) - Mostrar alíquota IPI
      + mostrar_QCST (boolean, optional) - Mostrar código PIS
      + mostrar_QpPIS (boolean, optional) - Mostrar alíquota PIS
      + mostrar_SCST (boolean, optional) - Mostrar código COFINS
      + mostrar_SpCOFINS (boolean, optional) - Mostrar alíquota COFINS

  + Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
                "dia": "2019-04-30",
                "categoria": "",
                "tags": "",
                "semExcluidos": false,
                "semEstNaoControl": false,
                "mostrarEstoqueNegativo": false,
                "mostrarCodProprio": false,
                "apresentarArquivados": false
            }
              

  + Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            [
                {
                    "codigo": 20,
                    "produto": "Produto padrão",
                    "estoque": 2,
                    "custo": "12.5500000000",
                    "total": 25.10
                }
            ]
            
### Estoque histórico por produto [POST /relatorios/estoqueHistorico]
  Histórico de movimentação de um produto.

  + Attributes (object)
      + produto (number, optional) - Código do produto

  + Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
                "produto": 20
            }
              

  + Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            [
                {
                    "codigo": "20",
                    "data": "2019-04-10",
                    "estoque": "4.0000",
                    "precoCusto": "12.5500000000",
                    "precoVenda": "29.9000000000",
                    "custoTotal": "50.20000000000000",
                    "vendaTotal": "119.60000000000000"
                }
            ]

### Sugestão de compra [POST /relatorios/sugestaoCompra]
  Relatório de sugestão de compra baseado na média da venda dos produtos.

  + Attributes (object)
      + dataCompra (string, required) - Data da compra, formato YYYY-MM-DD
      + estoqueAte (string, required) - Desejo comprar para ter estoque até, formato YYYY-MM-DD
      + de (string, required) - Data inicial para calculo da média de vendas, formato YYYY-MM-DD
      + ate (string, required) - Data final para calculo da média de vendas, formato YYYY-MM-DD
      + categoria (number, optional) - Código da categoria
      + apenasAComprar (boolean, optional) - Mostrar apenas produtos necessários
      + apresentarArquivados (boolean, optional) - Apresentar produtos arquivados
      + apresentarCodProprio (boolean, optional) - Apresentar código próprio
      + ordem (enum[number], optional) - Ordenação
        + Members
          + 1 - Produto
          + 2 - Código
          + 3 - Últ. custo
          + 4 - Total vendas
          + 5 - Giro diário
          + 6 - Estoque atual
          + 7 - Dias estoque
          + 8 - Estoque mín.
          + 9 - Sug. Compra

  + Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
                "dataCompra": "2019-04-30",
                "estoqueAte": "2019-05-30",
                "de": "2019-03-01",
                "ate": "2019-03-31",
                "categoria": "",
                "apenasAComprar": false,
                "apresentarArquivados": false,
                "apresentarCodProprio": false
            }
              

  + Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            [
                {
                    "codigo": "20",
                    "produto": "Produto padrão",
                    "IcProd": "AP",
                    "ultCusto": "12.5500000000",
                    "totVendas": "10.0000",
                    "giroDiario": 0.3333333333333333,
                    "estoqueAtual": "1.0000",
                    "diasEstoque": 0,
                    "estoqueMin": "2.0000",
                    "sugestao": 10
                }
            ]
            
### Produtos duplicados [POST /relatorios/produtosDuplicados]
  Mostra todos os produtos que estão cadastrados em duplicidade no sistema.

  + Attributes (object)
      + categoria (number, optional) - Código da categoria
      + ordem (enum[string], required) - Tipo de ordenação que será utilizada
        + Members
          + repeticoes - Quantidade de registros duplicados
          + nome
          + categoria
      + apresentarArquivados (boolean, optional) - Apresentar produtos arquivados
      + usarIcProd (boolean, optional) - Considerar código personalizado na diferença dos produtos

  + Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
                "categoria": "",
                "ordem": "nome",
                "apresentarArquivados": false,
                "usarIcProd": false
            }
              

  + Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            [
                {
                    "vezes": "6",
                    "nomeCateg": null,
                    "IxProd": "Tinta",
                    "IcProd": "035250519",
                    "IcEAN": "7891153010113",
                    "codigos": "5688, 5630, 5689, 5685, 5686, 5687"
                }
            ]
            
### Movimentação de um produto [POST /relatorios/movimentacaoProduto]
  Histórico de movimentação de um produto.

  + Attributes (object)
      + produto (number, required) - Código do produto
      + tipoData (enum[string], required) - Tipo de data que será considerada para realizar os filtros
        + Members
          + dtVenda - Data da venda
          + dtCad - Data de cadastro
      + de (string, required) - Data inicial, formato YYYY-MM-DD
      + ate (string, required) - Data final, formato YYYY-MM-DD
      + mostrarPrecoCusto (boolean, optional) - Mostrar preço de custo

  + Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
                "produto": 10,
                "tipoData": "dtVenda",
                "de": "2019-02-01",
                "ate": "2019-04-30",
                "mostrarPrecoCusto": false
            }
              

  + Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            [
                {
                    "tipo": "entrada",
                    "quantidade": 915.8,
                    "data": "",
                    "descricao": "Saldo anterior",
                    "saldo": 915.8
                },
                {
                    "tipo": "saida",
                    "quantidade": "1.0000",
                    "data": "2019-02-05",
                    "descricao": "Venda cód. 414. Catadores Ltda.",
                    "saldo": 914.8
                }
            ]
            
### Personalizado de produtos [POST /relatorios/personalizadoProdutos]
  Lista os produtos com os campos escolhidos. O nome do produto e seu código interno sempre são listados.

  + Attributes (object)
      + buscar (string, optional) - Apenas produtos que contenham a(s) palavra(s)
      + tags (string, optional) - Filtrar por palavras-chave dos produtos
      + categoria (number, optional) - Código da categoria
      + estoque (enum[string], optional) - Filtrar por situação do estoque
        + Members
          + ignoraZerado - Não mostrar produtos com estoque zerado
          + somenteNegativo - Mostrar apenas produtos com estoque negativo
      + ordem (enum[string], optional) - Ordenação que será utilizada
        + Members
          + nomeProduto
          + codInterno
          + codPersonalizado
          + nomeCategoria
          + precoVenda
          + precoCusto
          + estoque
          + palavrasChave
          + ean
      + tipoProduto (enum[string], optional) - Filtrar apenas produtos ou serviços
        + Members
          + produto
          + servico
      + ordemDesc (boolean, optional) - Ordenação decrescente
      + arquivados (boolean, optional) - Apresentar produtos arquivados
      + mostrar_IcProd (boolean, optional) - Mostrar código personalizado
      + mostrar_categNome (boolean, optional) - Mostrar categoria
      + mostrar_codConfigTrib (boolean, optional) - Mostrar código grupo tributo
      + mostrar_precoCusto (boolean, optional) - Mostrar preço custo
      + mostrar_precoVenda (boolean, optional) - Mostrar preço venda
      + mostrar_margem (boolean, optional) - Mostrar margem cadastrada
      + mostrar_margemAtual (boolean, optional) - Mostrar margem atual
      + mostrar_controlarEstoque (boolean, optional) - Mostrar controla estoque
      + mostrar_estoque (boolean, optional) - Mostrar quant. estoque
      + mostrar_totalCusto (boolean, optional) - Mostrar total (custo x quant)
      + mostrar_totalVenda (boolean, optional) - Mostrar total (venda x quant)
      + mostrar_estoqueMinimo (boolean, optional) - Mostrar estoque mínimo
      + mostrar_tipoUso (boolean, optional) - Mostrar tipo de uso
      + mostrar_anotacoes (boolean, optional) - Mostrar anotações gerais
      + mostrar_VinfAdProd (boolean, optional) - Mostrar anotações NFe
      + mostrar_tags (boolean, optional) - Mostrar palavras-chave
      + mostrar_IcEAN (boolean, optional) - Mostrar EAN/GTIN
      + mostrar_INCM (boolean, optional) - Mostrar NCM
      + mostrar_ICEST (boolean, optional) - Mostrar CEST
      + mostrar_IEXTIPI (boolean, optional) - Mostrar EXTIPI
      + mostrar_IuCom (boolean, optional) - Mostrar unidade
      + mostrar_pesoB (boolean, optional) - Mostrar peso bruto
      + mostrar_pesoL (boolean, optional) - Mostrar peso líquido
      + mostrar_Norig (boolean, optional) - Mostrar origem
      + mostrar_cfop (boolean, optional) - Mostrar CFOP
      + mostrar_NCST (boolean, optional) - Mostrar código ICMS
      + mostrar_NpICMS (boolean, optional) - Mostrar alíquota ICMS
      + mostrar_NpICMSST (boolean, optional) - Mostrar alíquota ICMS ST
      + mostrar_NpMVAST (boolean, optional) - Mostrar MVA ST
      + mostrar_OCST (boolean, optional) - Mostrar código IPI
      + mostrar_OpIPI (boolean, optional) - Mostrar alíquota IPI
      + mostrar_QCST (boolean, optional) - Mostrar código PIS
      + mostrar_QpPIS (boolean, optional) - Mostrar alíquota PIS
      + mostrar_SCST (boolean, optional) - Mostrar código COFINS
      + mostrar_SpCOFINS (boolean, optional) - Mostrar alíquota COFINS

  + Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
                "buscar": "",
                "tags": "",
                "categoria": "",
                "estoque": "",
                "ordem": "nomeProduto",
                "tipoProduto": "",
                "ordemDesc": false,
                "arquivados": false,
                "mostrar_IcProd": false,
                "mostrar_categNome": false,
                "mostrar_codConfigTrib": false,
                "mostrar_precoCusto": false,
                "mostrar_precoVenda": false,
                "mostrar_margem": false,
                "mostrar_margemAtual": false,
                "mostrar_controlarEstoque": false,
                "mostrar_estoque": false,
                "mostrar_totalCusto": false,
                "mostrar_totalVenda": false,
                "mostrar_estoqueMinimo": false,
                "mostrar_tipoUso": false,
                "mostrar_anotacoes": false,
                "mostrar_VinfAdProd": false,
                "mostrar_tags": false,
                "mostrar_IcEAN": false,
                "mostrar_INCM": false,
                "mostrar_ICEST": false,
                "mostrar_IEXTIPI": false,
                "mostrar_IuCom": false,
                "mostrar_pesoB": false,
                "mostrar_pesoL": false,
                "mostrar_Norig": false,
                "mostrar_cfop": false,
                "mostrar_NCST": false,
                "mostrar_NpICMS": false,
                "mostrar_NpICMSST": false,
                "mostrar_NpMVAST": false,
                "mostrar_OCST": false,
                "mostrar_OpIPI": false,
                "mostrar_QCST": false,
                "mostrar_QpPIS": false,
                "mostrar_SCST": false,
                "mostrar_SpCOFINS": false
            }
              

  + Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            [
                {
                    "codigo": "20",
                    "IxProd": "Produto padrão"
                }
            ]
            
### Impostos NFSe [POST /relatorios/nfseImpostos]
  Relatório que detalha os impostos das notas fiscais de serviço

+ Attributes (object)
    + de (string, required) - Data de emissão inicial, formato YYYY-MM-DD
    + ate (string, required) - Data de emissão final, formato YYYY-MM-DD
    + cliente (number, optional) - Código do cliente
    + tags (string, optional) - Filtrar por palavras-chave da NFSe
    + order (enum[string], optional) - Ordenação padrão
      + Members
        + 'codigo'
        + 'nome' - Nome do cliente
        + 'numeroNFSe'
        + 'dtEmissao'
    + retido (enum[string], optional) - Filtrar por retenção
      + Members
        + '1' - Somente notas com retenção
        + '2' - Somente notas sem retenção
    + situacao (enum[string], optional) - Situação da NFSe
      + Members
        + '10' - Autorizada
        + '20' - Ag. prefeitura/Envio
        + '25' - Ag. prefeitura/Cancelamento
        + '30' - Rejeitada
        + '40' - Denegada
        + '90' - Cancelada
        + '99' - Não enviada
        + '100' - Todas
+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            [
                {
                    "de": "2019-12-01",
                    "ate": "2019-12-31",
                    "cliente": 0,
                    "tags": "",
                    "retido": 0,
                    "order": "numeroNFSe",
                    "situacao": 30
                }
            ]

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            [
                {
                    "codigo": "1",
                    "dtEmissao": "2019-12-10 14:45:15",
                    "numeroNFSe": "1",
                    "valorLiquido": "34.68",
                    "nome": "Cliente teste",
                    "issqn": 0,
                    "inss": 1.61,
                    "pis": 0.24,
                    "cofins": 1.1,
                    "csll": 0.37,
                    "irrf": 0.37
                }
            ]

### Registro de eventos [POST /relatorios/registroEventos]
  Lista os eventos ocorridos no sistema.

  + Attributes (object)

      + de (string, required) - data inicial, formato YYYY-MM-DD
      + ate (string, required) - data final, formato YYYY-MM-DD
      + buscar (string, optional) - Apenas eventos contendo a string informada
      + usuario (number, optional) - Filtrar por usuário que realizou a ação
      + modulo (enum[string], optional) - Filtrar apenas por registros de determinado módulo
        + Members
          + boletos
          + produtosCateg - Categoria de produtos
          + composicao
          + compras
          + conciliacao
          + configBoletos - Configuração dos boletos
          + config - Configurações gerais do sistema
          + contatos
          + configTrib - Dados tributários
          + disponiveis - Disponíveis (Contas Caixa)
          + financeiro
          + formasPgto - Formas de pagamento
          + nfse
          + planoContas - Plano de Contas
          + produtos
          + producao
          + recibos
          + recorrencia
          + relatorios
          + configTemplates - Templates de impressão
          + transferencia
          + usuarios
          + vendas
      + codModulo (number, optional) - Filtrar apenas por registro com o código interno informado.
      + ordem (enum[number], optional) - Ordenação
        + Members
          + 1 - Código
          + 2 - Usuário
          + 3 - Evento
          + 4 - Módulo
          + 5 - Cód. módulo
          + 6 - Data

  + Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
              "de": "2019-05-17",
              "ate": "2019-05-17",
              "buscar": "",
              "usuario": "",
              "modulo": "vendas",
              "codModulo": "5"
            }
              

  + Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            [
              {
                  "codigo": "1",
                  "codModulo": "5",
                  "modulo": "vendas",
                  "usuario": "userPadrao",
                  "msg": "Adicionou item",
                  "obs": "codVenda: 5; codProduto: 24; tipoProd: servico; preco: 90.9; quant: 1; vDesc: 0; vIPI: 0; vST: 0; vFCPST: 0; vFrete: 0; vRetencao: 0; vDeducao: ; obs: ; codConfigTrib: 8; custo: 0.0000000000; codItem: 12; ",
                  "data": "2019-05-17 11:38:08"
              },
              {
                  "codigo": "2",
                  "codModulo": "5",
                  "modulo": "vendas",
                  "usuario": "userPadrao",
                  "msg": "Alterou",
                  "obs": "valorTotal: 105.9; ",
                  "data": "2019-05-17 11:38:08"
              }
            ]

### Registro de acessos [POST /relatorios/registroAcessos]
  Registro de acessos ao sistema.

  + Attributes (object)

      + de (string, required) - data inicial, formato YYYY-MM-DD
      + ate (string, required) - data final, formato YYYY-MM-DD
      + usuario (number, optional) - Filtrar por usuário
      + ordem (enum[number], optional) - Ordenação
        + Members
          + 1 - Entrada
          + 2 - Código
          + 3 - Usuário
          + 4 - IP
          + 5 - Saída
          + 6 - Duração

  + Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
              "de": "2019-05-15",
              "ate": "2019-05-17",
              "usuario": ""
            }
              

  + Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            [
              {
                "codigo": "1",
                "usuario": "Usuario padrão",
                "ip": "192.168.0.12",
                "entrada": "15/05/2019 09:13",
                "saida": "15/05/2019 09:22",
                "duracao": 9
              },
              {
                "codigo": "2",
                "usuario": "Usuario dois",
                "ip": "192.168.0.13",
                "entrada": "15/05/2019 09:37",
                "saida": "15/05/2019 17:59",
                "duracao": 503
              }
            ]

# NFSe [/nfse]
Para utilizar o módulo de emissão de notas fiscal de serviço (NFSe) é preciso primeiro realizar a ativação do mesmo em conjuto com o suporte eGestor, e configurar os parâmetros básicos. Entre em contato com o suporte para mais informações.
Notas fiscais de serviço (NFSe) não podem ser excluídas nem canceladas via API.

| Situação | Descrição |
|----------|-----------|
| `10` | Autorizada |
| `20` | Ag. retorno |
| `25` | Ag. cancelamento |
| `30` | Rejeitada |
| `40` | Denegada |
| `90` | Cancelada |
| `99` | Não enviada |


| Enviar para prefeitura (enviarPrefeitura) | Descrição |
|-------------------------------------------|-----------|
| `0` | Não |
| `1` | Sim |

| Município de incidência do imposto (munIncidencia) | Descrição |
|----------------------------------------------------|-----------|
| `1` | Meu município |
| `2` | Município do cliente |

| Local da prestação do serviço (localPrest) | Descrição |
|--------------------------------------------|-----------|
| `1` | No município |
| `3` | Fora do município |
| `6` | Outro (Exterior) |

### Listar (List) [GET /nfse{?filtro,buscaObs,dtTipo,dtIni,dtFim,origNota,situacao,fields}]

+ Parameters
    + filtro (optional, string) - Busca a string informada nos campos: código da nota, código da venda, palavras-chave e nome do cliente.
    + buscaObs (optional, string) - Busca a string informada nas observações da nota fiscal
    + dtTipo (optional, string) - Define a data que será utilizada pelos filtros dtIni e dtFim. Valores possíveis: 
        * dtEmissao(default) - Data da emissão da nota fiscal.
        * dtCad - Data de cadastro.
    + dtIni (optional, date) - Data inicial, no formato yyyy-mm-dd
    + dtFim (optional, date) - Data final, no formato yyyy-mm-dd
    + origNota (optional, string) - Origem da nota fiscal. Valores possíveis: 
        * 'venda'
        * 'avulsa'
    + situacao (optional, integer) - Situação atual da nota fiscal. Valores possíveis:
        * 10 - Autorizada
        * 20 - Ag. retorno
        * 25 - Ag. cancelamento
        * 30 - Rejeitada
        * 40 - Denegada
        * 90 - Cancelada
        * 99 - Não enviada
    + fields (optional, string) - Permite definir quais os campos serão retornados pela api. Informe separado por vírgula. 
        * ex: &fields=codigo,dtEmissao,valorBruto

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)

          {
              "total": 1,
              "per_page": 50,
              "current_page": 1,
              "last_page": 4,
              "next_page_url": null,
              "prev_page_url": null,
              "from": 1,
              "to": 50,
              "data": [
                {
                    "codigo": 171,
                    "codContato": 152,
                    "nomeContato": "Cliente padrão",
                    "codVenda": 339,
                    "numeroRPS": 160034,
                    "numeroNFSe": 10,
                    "chaveRPS": "",
                    "serie": "7",
                    "ambiente": 2,
                    "dtEmissao": "2018-11-30 17:16:30",
                    "dtCad": "2018-11-30 09:56:26",
                    "valorBruto": 90,
                    "valorLiquido": 90,
                    "obs": "",
                    "situacao": 10,
                    "situRetorno": "30/11/2018 17:16:30 - Mensagem retornada pela prefeitura: Código: L018, Mensagem: A empresa nao esta habilitada no ambiente de integracao.  Numero do RPS em que ocorreu o erro: 160034",
                    "tags": ""
                }
              ]
          }

+ Response 401 (application/json)

          {
              "errCode": 401,
              "errMsg": "Não foi possível acessar o sistema. Verifique seu \"access_token\".",
              "errObs": "access_denied",
              "errFields": null,
              "errUrl": "/v1/nfse"
          }
          
### Novo (Create) [POST]

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
              "enviarPrefeitura": 1,
              "codContato": 1,
              "numeroRPS": "",
              "munIncidencia": 1,
              "localPrest": 1,
              "obs": "Obs geral",
              "issRetido": 0,
              "inssRetido": 0,
              "pisRetido": 0,
              "cofinsRetido": 0,
              "csllRetido": 0,
              "irrfRetido": 0,
              "itens": [
                  {
                      "cod": 24,
                      "obs": "item adicionado via API",
                      "quant": 1,
                      "preco": 90.9,
                      "vDesc": 0,
                      "vDeducao": 0
                  }
              ]
            }

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
              "descricao": "Mensagem retornada pela prefeitura: Código: E100, Mensagem: Documento autorizado",
              "numeroNfse": "1",
              "protocolo": "1",
              "codVerificacao": "1231332",
              "codNota": 1,
              "situacao": 10,
              "urlImpressao": "https://v4.egestor.com.br/fiscal/print.php?chave=123031fs03.j42fk4mlg.2hbrx10029",
              "emailCliente": "email@contato.com.br"
            }
            
### Detalhar (Read) [GET /nfse/{codigo}]

+ Parameters
    + codigo (required, number, `1`) ... Código da NFSe

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 58

    + Body

            {
              "codigo": 1,
              "codContato": 1,
              "codVenda": 1,
              "numeroRPS": 1,
              "numeroNFSe": 1,
              "chaveRPS": "",
              "serie": "20",
              "ambiente": 2,
              "dtEmissao": "2018-12-11 17:55:41",
              "dtCad": "2018-12-11 17:55:38",
              "valorBruto": 90.9,
              "valorLiquido": 90.9,
              "obs": "",
              "situacao": 10,
              "situRetorno": "11/12/2018 17:55:41 - Mensagem retornada pela prefeitura: Código: E100, Mensagem: Documento autorizado.",
              "ativo": true,
              "nfseItens": [
                  {
                      "codigo": "1",
                      "codNfse": "1",
                      "codProduto": "24",
                      "codListaServico": "01.06",
                      "servico": "Formatação de computadores 775",
                      "codConfigTrib": "8",
                      "quant": "1",
                      "unitario": "90.9000000000",
                      "vDesc": "0.0000",
                      "total": "90.9000000000",
                      "configTrib": {
                          "iss": {
                              "aliquota": "2",
                              "baseCalculo": 90.9,
                              "imposto": 1.82,
                              "retido": 0
                          },
                          "inss": {
                              "aliquota": 0,
                              "baseCalculo": 90.9,
                              "imposto": 0,
                              "retido": 0
                          },
                          "pis": {
                              "aliquota": 0,
                              "baseCalculo": 90.9,
                              "imposto": 0,
                              "retido": 0
                          },
                          "cofins": {
                              "aliquota": 0,
                              "baseCalculo": 90.9,
                              "imposto": 0,
                              "retido": 0
                          },
                          "csll": {
                              "aliquota": 0,
                              "baseCalculo": 90.9,
                              "imposto": 0,
                              "retido": 0
                          },
                          "irrf": {
                              "aliquota": 0,
                              "baseCalculo": 90.9,
                              "imposto": 0,
                              "retido": 0
                          }
                      },
                      "configGeral": {},
                      "obs": "servico",
                      "descConfigTrib": "ISS",
                      "cnae": "5920100",
                      "camposExtras": {},
                      "codTributacaoMun": "724",
                      "vDeducao": 0.00
                  }
              ],
              "munIncidencia": 1,
              "localPrest": 1,
              "tags": "",
            }

+ Response 404 (application/json)
  Quando o registro não foi encontrado.
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
              "errCode": 404,
              "errMsg": "Nenhum nfse encontrado para o código 1",
              "errObs": null,
              "errFields": null
            }

+ Response 410 (application/json)
  Quando o registro foi apagado do sistema.
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59
    + Body

            {
              "errCode": 410,
              "errMsg": "Registro com código 1 não existe.",
              "errObs": null,
              "errFields": null
            }
            
# NFe [/nfe]
Módulo para a consultar a situação de NFe e NFCe, e realizar o download do XML de documentos autorizados.

| Situação | Descrição |
|------|--------|
| `5` | Criada |
| `10` | Em digitação |
| `15` | Rejeitada |
| `20` | Assinada |
| `30` | Validada |
| `35` | Form. Segur. |
| `40` | Enviada |
| `50` | Autorizada |
| `80` | Denegada |
| `90` | Cancelada |
| `91` | Inutilizada |

| Ambiente | Descrição |
|---------|-----------|
| `1` | Produção |
| `2` | Homologação |

### Listar (List) [GET /nfe{?dtIni,dtFim,fields,orderBy}]
+ Parameters
    + dtIni (date, required) - Data inicial, no formato yyyy-mm-dd
    + dtFim (date, required) - Data final, no formato yyyy-mm-dd
    + fields (string, optional) - Permite definir quais os campos serão retornados pela api. Informe separado por vírgula. Valores possíveis:
        * codigo, numero, codVenda, cliente, chave, email, ambiente, serie, modelo, dtEmissao, cfop, valor, situacao
        * ex: &fields=codigo,cliente
        * Padrão: codigo
    + orderBy (string, optional) - Permite definir a ordenação da listagem. Informe o campo e a forma de ordenação (ascendente ou descendente) separados por vírgula. Só é possível definir uma ordenação por requisição. Valores possíveis:
        * codigo, numero, codVenda, cliente, chave, email, ambiente, serie, modelo, dtEmissao, cfop, valor, situacao
        * ex: &orderBy=cliente,desc
        * ex: &orderBy=email,asc
        

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)

          {
              "total": 1,
              "per_page": 50,
              "current_page": 1,
              "last_page": 4,
              "next_page_url": null,
              "prev_page_url": null,
              "from": 1,
              "to": 50,
              "data": [
                 {
                   "codigo": 282,
                   "dtEmissao": "2021-01-08",
                   "situacao": 50,
                   "valor": 10.45
                 }
              ]
          }

+ Response 401 (application/json)

          {
              "errCode": 401,
              "errMsg": "Não foi possível acessar o sistema. Verifique seu \"access_token\".",
              "errObs": "access_denied",
              "errFields": null,
              "errUrl": "/v1/nfe"
          }

### Detalhar (Read) [GET /nfe/{codigo}]

+ Parameters
    + codigo (required, number, `1`) ... Código da NFe

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 58

    + Body

            {
              "codigo": "1",
              "numero": "1",
              "chave": "43123456789123456789123456789123456789123456",
              "modelo": "65",
              "dt_emissao": "2019-12-19",
              "situacao": "50"
            }

+ Response 404 (application/json)
  Quando o registro não foi encontrado.
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
              "errCode": 404,
              "errMsg": "Nenhum nfe encontrado para o código 1",
              "errObs": null,
              "errFields": null
            }
            
### Download XML (Read) [GET /nfe/{codigo}/xml]
    Retorna o XML (binário) de um documento autorizado.
+ Parameters
    + codigo (required, number, `1`) ... Código da NFe

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 58

    + Body

            {
              
            }

+ Response 404 (application/json)
  Quando o registro não foi encontrado.
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
              "errCode": 404,
              "errMsg": "Nenhum nfe encontrado para o código 1",
              "errObs": null,
              "errFields": null
            }

+ Response 404 (application/json)
  Quando o documento ainda não foi autorizado.
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
              "errCode": 404,
              "errMsg": "Documento não se encontra autorizado, impossível fazer download do XML.",
              "errObs": null,
              "errFields": null
            }

# Disco virtual [/discoVirtual]
Módulo para upload de arquivos, que podem ser vinculados a registros de outros módulo, ou serem enviados de maneira avulsa.
Não é possível editar um registro no disco virtual. A URL para download é obtida através do método Detalhar, e tem validade de 10 minutos após a requisição.

### Listar (List) [GET /discoVirtual{?filtro,dtIni,dtFim,modulo,codModulo,fields,orderBy}]

+ Parameters
  + filtro (optional) - Busca a string informada nos campos: código, nome e descrição do arquivo.
  + dtIni (optional, date) - Data inicial do upload, no formato yyyy-mm-dd
  + dtFim (optional, date) - Data final do upload, no formato yyyy-mm-dd
  + modulo (optional, string) - Filtrar apenas por registros de determinado módulo. Valores possíveis:
        * financeiro
        * vendas
        * usuarios
        * produtos
        * contatos
        * producao
        * composicao
        * compras
        * recibos
  + codModulo (number, optional) - Filtrar apenas por registro com o código interno informado.
  + fields (optional) - Permite definir quais os campos serão retornados pela api. Informe separado por vírgula. Valores possíveis:
      * codigo, codModulo, modulo, nome, descricao, tags, tipo, dtEnvio, tamanho 
      * ex: &fields=nomeContato,valor
  + orderBy (optional) - Permite definir a ordenação da listagem. Informe o campo e a forma de ordenação (ascendente ou descendente) separados por vírgula. Só é possível definir uma ordenação por requisição. Valores possíveis:
      * codigo, codModulo, modulo, nome, descricao, tags, tipo, dtEnvio, tamanho
      * ex: &orderBy=situacao,desc
      * ex: &orderBy=nomeContato,asc

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)

          {
              "total": 1,
              "per_page": 50,
              "current_page": 1,
              "last_page": 4,
              "next_page_url": null,
              "prev_page_url": null,
              "from": 1,
              "to": 50,
              "data": [
                {
                  "codigo": 1,
                  "codModulo": 12,
                  "modulo": "financeiro",
                  "nome": "credit-card.png",
                  "descricao": "Dados do cartão",
                  "tags": [],
                  "tipo": "image/png",
                  "dtEnvio": "2019-05-31 10:43:44",
                  "tamanho": 11477
                }
              ]
          }

+ Response 401 (application/json)

          {
              "errCode": 401,
              "errMsg": "Não foi possível acessar o sistema. Verifique seu \"access_token\".",
              "errObs": "access_denied",
              "errFields": null,
              "errUrl": "/v1/discoVirtual"
          }

### Novo (Create) [POST]
ATENÇÃO! Para utilizar esse método não deve ser enviado no header da requisição o "application-type: json", pois o mesmo não funciona em conjunto com o envio de arquivos. Nesse caso, apenas iniba o envio dessa propriedade.
+ Attributes (object)

    + arquivo (required) - Arquivo que será enviado
    + tags (array, optional) - Palavras-chave do arquivo
    + modulo (string, optional) - Módulo a qual o pertence o arquivo
    + codModulo (number, optional) - Código do registro a qual pertence o arquivo. Deve ser informado caso o `modulo` seja informado
    + descricao (string, optional) - Observações do arquivo

+ Request (multipart/form-data)

    + Headers

            Authorization: Bearer [access_token]

    + Body

            {
              "arquivo": "comprovante.pfd",
              "tags": [],
              "modulo": "vendas",
              "codModulo": 200,
              "descricao": "Comprovante de pagamento",
            }

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
                "codigo": 2,
                "codModulo": 200,
                "modulo": "vendas",
                "nome": "comprovante.pfd",
                "descricao": "Comprovante de pagamento",
                "tags": [],
                "tipo": "application/pdf",
                "dtEnvio": "2019-05-31 14:21:52",
                "tamanho": 101477
            }

### Detalhar (Read) [GET /discoVirtual/{codigo}]

+ Parameters
    + codigo (required, number, `1`) ... Código do arquivo

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)
  Todos os dados do arquivo
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 58

    + Body

            {
              "codigo": 2,
              "codModulo": 200,
              "modulo": "vendas",
              "nome": "comprovante.pfd",
              "descricao": "Comprovante de pagamento",
              "tags": [],
              "tipo": "image/png",
              "dtEnvio": "2019-05-31 14:21:52",
              "tamanho": 101477,
              "linkDireto": "https://storage101.dfw1.clouddrive.com/v1/MossoCloudFS_ea4cd452-057c-46e4-b58f-aef2c5e52dfd/newgestor/new_1/discoVirtual/credit-card-vector2.png?temp_url_sig=37487e1d0a8fabeb6dc69acebd653ec6e982162a&temp_url_expires=1559324049"
            }

+ Response 404 (application/json)
  Quando registro não for encontrado.
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
              "errCode": 404,
              "errMsg": "Nenhum registro com código 1 encontrado",
              "errObs": null,
              "errFields": null
            }

+ Response 410 (application/json)
  Quando registro foi apagado do sistema, o código de retorno é 410.
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59
    + Body

            {
              "errCode": 410,
              "errMsg": "Registro com código 1 não existe.",
              "errObs": null,
              "errFields": null
            }

### Remover (Delete) [DELETE  /discoVirtual/{codigo}]

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
                "code": "200",
                "msg": "Arquivo com código 2 excluído com sucesso!",
                "obs": null,
                "fields": null
            }

# Usuários [/usuarios]
Módulo permite listar e detalhar os usuários cadastrados no sistema.

### Listar (List) [GET /usuarios{?filtro,vendedor,usuarioSistema,fields,orderBy}]
+ Parameters
    + filtro (string, optional) - Busca a string informada nos campos: nome, login e email.
    + vendedor (boolean, optional) - Filtrar apenas usuários vendedores.
    + usuarioSistema (boolean, optional) - Filtrar apenas usuários com acesso ao sistema.
    + fields (string, optional) - Permite definir quais os campos serão retornados pela api. Informe separado por vírgula. Valores possíveis:
        * ex: &fields=login,email
    + orderBy (string, optional) - Permite definir a ordenação da listagem. Informe o campo e a forma de ordenação (ascendente ou descendente) separados por vírgula. Só é possível definir uma ordenação por requisição. Valores possíveis:
        * ex: &orderBy=nome,desc
        * ex: &orderBy=codigo,asc
        

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)

          {
              "total": 1,
              "per_page": 50,
              "current_page": 1,
              "last_page": 4,
              "next_page_url": null,
              "prev_page_url": null,
              "from": 1,
              "to": 50,
              "data": [
                {
                  "codigo": 1,
                  "nome": "Usuario 1",
                  "login": "user1",
                  "email": "user1@user1.com",
                  "tags": [],
                  "usuarioSistema": true,
                  "vendedor": true,
                  "comisProd": 7.35,
                  "comisServ": 5.00,
                  "comisFin": 9.90
                },
              ]
          }

+ Response 401 (application/json)

          {
              "errCode": 401,
              "errMsg": "Não foi possível acessar o sistema. Verifique seu \"access_token\".",
              "errObs": "access_denied",
              "errFields": null,
              "errUrl": "/v1/contatos"
          }

### Detalhar (Read) [GET /usuarios/{codigo}]

+ Parameters
    + codigo (required, number, `1`) ... Código do usuário

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)
  Todos os dados do contato
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 58

    + Body

            {
              "codigo": 1,
              "nome": "Usuario 1",
              "login": "user1",
              "email": "user1@user1.com",
              "tags": [],
              "permissoes": [
                  "acessoCompleto"
              ],
              "usuarioSistema": true,
              "vendedor": true,
              "comisProd": 7.35,
              "comisServ": 5,
              "comisFin": 9.9
            }

+ Response 404 (application/json)
  Quando registro não for encontrado.
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59

    + Body

            {
              "errCode": 404,
              "errMsg": "Nenhum registro com código 1 econtrado",
              "errObs": null,
              "errFields": null
            }

+ Response 410 (application/json)
  Quando registro foi apagado do sistema, o código de retorno é 410.
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59
    + Body

            {
              "errCode": 410,
              "errMsg": "Registro com código 1 não existe.",
              "errObs": null,
              "errFields": null
            }

            
# Webhooks [/webhooks]
O eGestor conta com uma estrutura de webhooks. Através dela, será disparada uma requisição para determinada URL do seu sistema sempre que algum registro for criado, editado ou excluído. 

Importante: Enviaremos um parâmetro chave na requisição, chamado de "securityToken". Esta é uma chave única e diferente de seu token. Você deve usá-la para autenticar o webhook recebido, confirmando a autenticidade da requisição.

Para testes dos webhooks, sugerimos a utilização do site https://webhook.site/

Os módulos disponíveis são os seguintes:
* Produtos
* Contatos
* Vendas
* Usuários

Sempre que um produto, cliente, venda ou usuário for criado, editado ou excluído, você receberá um webhook no endpoint cadastrado, no seguinte formato (form-data):
    
    {
    
        "action"        => "updated",
        "codigo"        => "1",
        "date"          => "2019-01-11 14:55:19",
        "module"        => "contatos",
        "securityToken" => "01a161daaaf265968c1f36a19ef8f4"
    
    

+ action: Ação realizada. Valores possíveis: "created", "updated" ou "deleted".
+ codigo: Código do registro
+ date: Data que a alteração ocorreu no eGestor
+ module: Módulo do registro alterado. Valores possíveis: "produtos", "contatos", "vendas" ou "usuarios"
+ securityToken: Sua chave de segurança

*ATENÇÃO: O timeout para resposta ao POST do webhook é de 3 segundos. Serão feitas 5 tentativas em caso de falha. Sugerimos a utilização de queues para processamento dos webhooks enviados pelo eGestor, a fim de evitar timeouts.*

No momento do cadastro do webhook, é necessário definir quais os módulos serão ativados através dos parâmetros "produtos", "contatos", "vendas" e "usuarios".

### Detalhar [GET /webhooks]

+ Request (application/json)

    + Headers

            Authorization: Bearer [access_token]

+ Response 200 (application/json)
    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 59


    + Body
            
            {
              "endpoint": "http://www.example.com/webhookEgestor"
              "securityToken": "01a161daaaf265968c1f36a19ef8f4",
              "produtos": true,
              "contatos": false,
              "vendas": true,
              "usuarios": true
            }

### Cadastrar [POST /webhooks]

+ Request (application/json)

  + Headers

            Authorization: Bearer [access_token]

  + Body

            {
              "endpoint": "http://www.example.com/webhookEgestor",
              "produtos": true,
              "contatos": false,
              "vendas": true,
              "usuarios": false
            }

+ Response 200 (application/json)

    + Headers

            X-RateLimit-Limit: 60
            X-RateLimit-Remaining: 58

    + Body

            {
              "endpoint": "http://www.example.com/webhookEgestor",
              "securityToken": "b7e7d8d9185313c8738636c6bfc57d",
              "produtos": true,
              "contatos": false,
              "vendas": true,
              "usuarios": false
            }
