# Nucont Benchmarking API

## Sobre o projeto

A API do Nucont Benchmarking será responsável por fornecer os dados da revista digital para o frontend desenvolvido em React.

Nesta primeira versão, os dados serão armazenados em planilhas estruturadas que funcionarão como um CMS simplificado. O backend, desenvolvido em Python utilizando FastAPI, será responsável por ler essas planilhas, transformar os dados em objetos Python e disponibilizá-los através de endpoints REST.

O objetivo é permitir a navegação pelas edições da revista, capítulos, indicadores, análises setoriais e regionais, além da captura de leads para acesso ao conteúdo.

---

## Padrões da API

Todas as rotas utilizarão o prefixo:

```text
/api/v1
```

As respostas serão retornadas em formato JSON.

As coleções utilizarão nomes no plural, como:

```text
/edicoes
/setores
/regioes
```

E os recursos específicos serão identificados por IDs ou slugs:

```text
/edicoes/q1-2026
/indicadores/receita-mediana
/setores/industria
```

---

## Endpoints

### Listar edições

```http
GET /api/v1/edicoes
```

Retorna todas as edições disponíveis.

Exemplo de resposta:

```json
[
  {
    "id_edicao": "q1-2026",
    "titulo": "Q1 2026",
    "subtitulo": "Observatório da Economia Real",
    "trimestre": 1,
    "ano": 2026,
    "status": "publicado"
  }
]
```

---

### Listar capítulos de uma edição

```http
GET /api/v1/edicoes/{id_edicao}/capitulos
```

Retorna a estrutura editorial da edição.

Exemplo:

```json
{
  "id_edicao": "q1-2026",
  "capitulos": [
    {
      "id_capitulo": 2,
      "titulo": "Raio-X Brasil",
      "slug": "raio-x-brasil",
      "ordem": 1
    }
  ]
}
```

---

### Consultar indicador

```http
GET /api/v1/edicoes/{id_edicao}/indicadores/{slug_indicador}
```

Retorna os dados completos de um indicador, incluindo percentis e comparações por setor e região.

Exemplo:

```json
{
  "nome": "Receita Mediana",
  "valor_principal": 148000,
  "variacao": 6.2,
  "percentis": {
    "p25": 68000,
    "p50": 148000,
    "p75": 320000,
    "p90": 610000
  },
  "setores": [
    {
      "setor": "Indústria",
      "valor": 238000
    }
  ],
  "regioes": [
    {
      "regiao": "Sudeste",
      "valor": 158000
    }
  ]
}
```

---

### Consultar setor

```http
GET /api/v1/edicoes/{id_edicao}/setores/{slug_setor}
```

Retorna os dados e textos editoriais de um setor específico.

Exemplo:

```json
{
  "nome": "Indústria",
  "empresas_analisadas": 1250,
  "texto_editorial": "Análise do setor industrial...",
  "alerta": "Redução da margem operacional.",
  "oportunidade": "Crescimento da demanda regional."
}
```

---

### Consultar região

```http
GET /api/v1/edicoes/{id_edicao}/regioes/{slug_regiao}
```

Retorna os dados e textos editoriais de uma região específica.

Exemplo:

```json
{
  "nome": "Sudeste",
  "empresas_analisadas": 5400,
  "texto_editorial": "Análise da região Sudeste...",
  "alerta": "Queda na lucratividade média.",
  "oportunidade": "Mercado aquecido."
}
```

---

### Consultar rankings

```http
GET /api/v1/edicoes/{id_edicao}/rankings
```

Retorna os rankings apresentados na edição.

Exemplo:

```json
{
  "titulo": "Setores que mais cresceram",
  "itens": [
    {
      "posicao": 1,
      "nome_item": "Tecnologia",
      "valor": 24.5
    },
    {
      "posicao": 2,
      "nome_item": "Saúde",
      "valor": 18.2
    }
  ]
}
```

---

### Consultar alertas e oportunidades

```http
GET /api/v1/edicoes/{id_edicao}/alertas-oportunidades
```

Retorna os principais alertas e oportunidades destacados na publicação.

Exemplo:

```json
{
  "alertas": [
    {
      "titulo": "Queda da margem operacional",
      "descricao": "Empresas estão crescendo receita, mas reduzindo rentabilidade."
    }
  ],
  "oportunidades": [
    {
      "titulo": "Expansão do setor de serviços",
      "descricao": "Setor apresenta crescimento acima da média."
    }
  ]
}
```

---

### Consultar matriz crescimento x saúde

```http
GET /api/v1/edicoes/{id_edicao}/matriz-crescimento-saude
```

Retorna os dados utilizados para compor a matriz da publicação.

Exemplo:

```json
{
  "quadrantes": [
    {
      "nome": "Cresce e está saudável",
      "itens": [
        "Tecnologia",
        "Serviços"
      ]
    },
    {
      "nome": "Cresce, mas está pressionado",
      "itens": [
        "Varejo"
      ]
    }
  ]
}
```

---

### Consultar guia de interpretação

```http
GET /api/v1/edicoes/{id_edicao}/guia-interpretacao
```

Retorna conteúdos de apoio para interpretação dos indicadores.

Exemplo:

```json
{
  "conteudos": [
    {
      "titulo": "Como interpretar a mediana",
      "texto": "A mediana representa o valor central da distribuição."
    }
  ]
}
```

---

### Consultar conteúdo editorial

```http
GET /api/v1/edicoes/{id_edicao}/editorial/{tipo}
```

Retorna cartas, apresentações e demais conteúdos editoriais da edição.

Exemplo:

```json
{
  "titulo": "Carta da CEO",
  "autor": "Fernanda",
  "cargo": "CEO",
  "texto": "Mensagem editorial da edição."
}
```

---

### Consultar metodologia

```http
GET /api/v1/edicoes/{id_edicao}/metodologia
```

Retorna a metodologia utilizada na construção dos indicadores da publicação.

Exemplo:

```json
{
  "fontes": [
    "Fiscal",
    "Financeiro",
    "Contábil"
  ],
  "criterios": [
    "Uso da mediana",
    "Tratamento de outliers",
    "Anonimização dos dados"
  ]
}
```

---

### Cadastro de usuários

```http
POST /api/v1/cadastro
```

Responsável pela captura de leads e liberação de acesso à revista.

Body:

```json
{
  "nome": "Nome do Usuário",
  "email": "usuario@exemplo.com",
  "empresa": "Empresa X",
  "cargo": "Diretor",
  "perfil": "Empresário"
}
```

Resposta:

```json
{
  "status": "success",
  "access_token": "..."
}
```

---

## Estrutura dos dados

A aplicação será alimentada inicialmente por planilhas organizadas em abas, representando entidades como:

* edicoes
* capitulos
* indicadores
* setores
* regioes
* rankings
* alertas_oportunidades
* metodologia
* editoriais
* leads

O backend será responsável por converter essas informações em estruturas Python para consumo dos endpoints.

---

## Requisitos técnicos

* Backend em Python utilizando FastAPI.
* Leitura de dados a partir de planilhas estruturadas.
* Retorno em JSON.
* Cache simples em memória para melhorar desempenho.
* Omissão de campos vazios ou retorno de `null` quando não houver dados disponíveis.
* Compatibilidade com futuras edições da revista sem necessidade de alterações no frontend.

---

## Próximos passos

* Validar os endpoints com a equipe de Front-end.
* Definir a estrutura final das planilhas.
* Implementar o parser de dados.
* Desenvolver os endpoints da API.
* Gerar documentação automática com Swagger/OpenAPI.
