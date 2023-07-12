# Gerenciamento de contratos: modelagem de banco de dados documental

**Prefácio:** Neste projeto, desenvolvi uma solução para o gerenciamento e armazenamento de contratos e seus respectivos fornecedores, utilizando uma abordagem de modelagem de banco de dados. Ao longo do processo, foram seguidas as três etapas principais de modelagem de banco de dados: modelo conceitual, modelo lógico e modelo físico.

### Introdução
O gerenciamento eficiente de contratos é crucial para empresas de todos os portes. No entanto, lidar com a complexidade de contratos de fornecimento pode ser desafiador, especialmente para grandes empresas multinacionais.

Um dos principais desafios é a gestão de contratos com fornecedores, que envolvem uma variedade de serviços e produtos. Nesse contexto, a escolha adequada de um sistema de banco de dados se torna essencial para garantir a eficiência do gerenciamento. Duas opções em consideração são o banco de dados documental e o banco de dados de série temporal.

O banco de dados documental, como o MongoDB, oferece flexibilidade e estrutura semiestruturada, enquanto o banco de dados de série temporal é útil para análise histórica e previsões futuras.

Este projeto explora a problemática do gerenciamento de contratos com fornecedores em uma empresa multinacional, analisando as opções de banco de dados documental e de série temporal. Será proposta uma metodologia para projetar um banco de dados documental, considerando o modelo conceitual, lógico e físico.

### Questão a ser resolvida
Uma empresa multinacional de grande porte deseja gerenciar os contratos firmados com seus fornecedores. O Setor de Patrimônio é o responsável pelo gerenciamento e cadastramento de contratos, e de seus respectivos fornecedores. Esses contratos de fornecimento de produtos e de prestação de serviços são realizados com vários fornecedores dos mais variados ramos de atividade.

Um determinado contrato pode ser feito com vários fornecedores, como por exemplo, um contrato de fornecimento de suprimentos de informática onde cada fornecedor entrega determinado tipo de produto (um entrega papel, outro cartucho de impressora jato de tinta, etc.). 

Os contratos são classificados em vários tipos, tais como: consultoria, aluguel de equipamento, fornecimento de material/serviço, etc. Cada contrato possui uma forma de pagamento específica estabelecida na sua inclusão. Entretanto, todos os fornecedores enviam boletas de cobrança que são registradas, e na sua respectiva data de vencimento são atualizadas para o status de “paga”, e uma autorização para crédito ao fornecedor é enviada para o Sistema Bancário. 

O Setor de Patrimônio também é responsável pelo cadastramento dos materiais/serviços consumidos pela empresa, e também pelo cadastramento de seus respectivos fornecedores. Esses materiais/serviços podem ser contratados em vários contratos.

Além do mais, foi proposto duas alternativas como opção de tipo de banco de dados para solucinar a questão, o banco de dados documental e de série temporal.

### Metodologia
A seguir, pode-se observar o fluxograma da metologia utilziada para a criação da modelagem através da Figura 1.

Figura 1 – Fluxograma da metodologia.
<p align="center">
<img src="https://github.com/LucasTWJT/LucasTWJT-Modelagem-de-Banco-de-Dados-Documental_MongoDB/assets/103520318/a1c5e320-11c9-4756-9dd9-5a2f5fdb5dfc" alt="Fluxograma da metodologia utilizada" width="400" />
</p>
<p align="center">
Fonte: Autor (2023)
</p>

### Compreensão do negócio
De antemão, ao realizar a leitura e compreensão do problema proposto, é perceptível que não se trata de uma demanda complexa de banco de dados, principalmente por poder resumir o cadastramento dos materiais e serviços consumidos pela empresa e seus fornecedores relacionados, anexando-os à proposta principal, que são os contratos, pois todos estão correlacionados.

Como o banco de dados é direcionado aos contratos, o processo de envio dos boletos pelos fornecedores não é de interesse para a empresa, apenas os registros e anexos aos seus respectivos contratos. A principal escolha que é feita nessa etapa é entre o tipo de banco de dados que será utilizado, tendo como opção o documental e o de série temporal. Dessa forma, ao analisar o contexto e as características de cada um, o tipo documental foi escolhido, principalmente devido à sua flexibilidade, visto que não necessitaria seguir um esquema rígido.

Dessa forma, entidades como contratos ou boletos, na forma de documentos, podem ser alteradas e modificadas com o tempo, conforme a necessidade. Além disso, isso torna possível consultas mais complexas e flexíveis. Em oposição, o banco de dados de série temporal poderia ser utilizado caso fosse de extrema importância acompanhar o consumo de materiais e serviços ou pagamentos de boletos, por exemplo. No entanto, como o objetivo da modelagem não é esse e o foco são os contratos, o banco de dados documental se torna ideal.

### Modelo conceitual
O modelo cocenceitual foi baseado na compreensão e análise do problema proposto e construiído através do BR Modelo. O resultado da construção do diagrama de entidade relacionamento pode ser visualizado na Figura 2.

Figura 2 – Modelo conceitual.
<p align="center">
<img src="https://github.com/LucasTWJT/LucasTWJT-Modelagem-de-Banco-de-Dados-Documental_MongoDB/assets/103520318/5a3ceb86-b570-4479-9c03-e83c648ac369" alt="Modelo conceitual" width="820" />
</p>
<p align="center">
Fonte: Autor (2023)
</p>

Na Figura 2, é possível observar a relação entre os contratos e os boletos de pagamento. Um contrato pode não ter nenhum boleto quando, por exemplo, o fornecedor está no início do contrato e ainda não emitiu nenhum boleto, ou pode ter vários boletos que se acumularam ao longo do tempo de contrato. Por outro lado, um boleto está relacionado com no mínimo um e apenas um contrato.

Além disso, observa-se que os contratos são de prestação de materiais e serviços. Um contrato pode estar associado a no mínimo um material e serviço, caso contrário ele não existe, mas não há um limite máximo de associações. Por sua vez, um material e serviço não precisa necessariamente ter um contrato para existir em um determinado momento, mas pode estar associado a diversos contratos distintos.

Por fim, é possível observar que os materiais e serviços são fornecidos pelos fornecedores. Um material e serviço pode ser fornecido por nenhum ou muitos fornecedores, e um fornecedor pode estar fornecendo nenhum ou muitos materiais e serviços.

Além das próprias entidades e relacionamentos, pode-se visualizar os atributos e atributos chaves relacionados a cada um deles.

### Modelo lógico

O modelo lógico é estruturado em JSON, onde no total existem cinco documentos, incluindo o relacionamento "fornecimento". No entanto, serão criados três documentos distintos.

O primeiro documento é referente aos fornecedores e deve ser exclusivo, pois contém os registros dos fornecedores e não está integrado a outros documentos. Esse documento é composto apenas por seus atributos e chaves, conforme mostrado no Código 1.

Código 1 – Modelo lógico documento “fornecedor”.
```json
  {
    "id_fornecedor": "atributo_chave",
    "nome": "valor",
    "endereco": "valor",
    "telefone": "valor"
  }
```
<p align="center">
Fonte: Autor (2023)
</p>

O segundo documento é sobre os materiais e serviços e também deve ser independente. Ele faz referência aos fornecedores que fornecem esses materiais e serviços. No entanto, essa referência não é direta. Ela é colocada em um array chamado "fornecedores", que pode ter várias referências em formato de objetos JSON. Além disso, cada objeto "fornecedor" dentro do array "fornecedores" contém o relacionamento correspondente ao fornecimento. Esse esquema JSON é ilustrado no Código 2.

Código 2 – Modelo lógico documento “material_serviço”.
```json
  {
    "id_material": "atributo_chave",
    "nome": "valor",
    "descricao": "valor",
    "fornecedores": [
      {
        "$ref": "fornecedor",
        "$id_fornecedor": "referência_atributo_chave",
        "fornecimentos": [
          {
            "data_fornecimento": "valor",
            "quantidade": "valor",
            "valor_unitario": "valor"
          }
        ]
      }
    ]
  }
```
<p align="center">
Fonte: Autor (2023)
</p>

O terceiro e último documento é o mais importante, que são os contratos propriamente ditos. Nesses contratos, os boletos são integrados, uma vez que o boleto não existe sem um contrato associado. Portanto, não é necessário ter um documento separado para os boletos. A integração é feita através de um array de objetos chamado "boletos", onde cada objeto se refere a um boleto emitido para o mesmo contrato, mas em períodos diferentes. Isso ocorre porque um pagamento de contrato pode ser dividido em vários boletos. Além disso, um array de objetos é usado para referenciar os "materiais" (também serviços) da mesma forma, pois um contrato pode envolver vários materiais e serviços. Esse esquema é visualizado no Código 3.

Código 3 – Modelo lógico documento “contrato”.
```json
  {
    "id_contrato": "atributo_chave",
    "data_inicio": "valor",
    "data_fim": "valor",
    "tipo_contrato": "valor",
    "forma_pagamento": "valor",
    "boletos": [
      {
        "id_boleto": "atributo_chave",
        "data_emissao": "valor",
        "data_vencimento": "valor",
        "valor": "valor",
        "status": "valor",
        "data_pagamento": "valor"
      }
    ],
    "materiais": [
      {
        "$ref": "material_servico",
        "$id_material": "referência_atributo_chave"
      }
    ]
  }
```
<p align="center">
Fonte: Autor (2023)
</p>

### Modelo físico

A última etapa da metodologia envolve a construção do modelo físico no MongoDB Community Server versão 6.0.7 para Windows. Com base no modelo lógico, utiliza-se a linguagem DML para criar os documentos no console Mongosh do MongoDB. Ao utilizar a função use no console, é possível criar ou selecionar o banco de dados.

Os documentos são inseridos individualmente usando a função ```db.NomeDaColeção.insertMany()```, passando os valores correspondentes para cada atributo. Cada documento possui um atributo chave chamado "ID", cujo valor é gerado pelo próprio MongoDB usando a função ```ObjectID()```, garantindo sua exclusividade no banco de dados.

Além disso, é possível utilizar a interface gráfica do MongoDB Compass para visualizar, modificar e manipular o banco de dados de forma intuitiva. Utilizando a função ```use```, o banco de dados é criado com o nome "gerenciamento_contratos". Em seguida, com a função ```insertMany```, os documentos são inseridos seguindo a ordem descrita no modelo lógico, de acordo com a estrutura JSON.

Além de realizar alterações e modificações no banco de dados usando a linguagem DML, é possível efetuar essas mudanças manualmente por meio da interface gráfica do MongoDB Compass, tornando o processo mais intuitivo e menos técnico.

Figura 3 – Interface inicial MongoDB Compass e Mongosh.
<p align="center">
<img src="https://github.com/LucasTWJT/LucasTWJT-Modelagem-de-Banco-de-Dados-Documental_MongoDB/assets/103520318/38bf8e2c-1c4c-49f5-92c9-cdd436badd4f" alt="Interface inicial MongoDB Compass e Mongosh" width="800" />
</p>
<p align="center">
Fonte: Autor (2023)
</p>

 Na Figura 3 é possível observar a esquerda os banco de dados existentes. Ao centro, é possível observar a lista das três coleções de documentos de “contrato”, “fornecedor” e “material_serviço”. Ademais, na partir inferior da imagem é possível visualizar o console Mongosh.

Figura 4 – a) Documentos lista, b) Documento JSON, c) Documento tabela.
<p align="center">
(a)
</p>
<p align="center">
<img src="https://github.com/LucasTWJT/LucasTWJT-Modelagem-de-Banco-de-Dados-Documental_MongoDB/assets/103520318/c5e8b192-2667-44cf-83d2-a6010949c900" alt="Interface inicial MongoDB Compass e Mongosh" width="220" />
</p>

<p align="center">
(b)
</p>
<p align="center">
<img src="https://github.com/LucasTWJT/LucasTWJT-Modelagem-de-Banco-de-Dados-Documental_MongoDB/assets/103520318/8f319d85-9b68-495e-87d7-d26f47b27deb" alt="Interface inicial MongoDB Compass e Mongosh" width="150" />
</p>

<p align="center">
(c)
</p>
<p align="center">
<img src="https://github.com/LucasTWJT/LucasTWJT-Modelagem-de-Banco-de-Dados-Documental_MongoDB/assets/103520318/40a2c6dd-3f65-40c3-8c9b-c1d9c854a2da" alt="Interface inicial MongoDB Compass e Mongosh" width="420" />
</p>

<p align="center">
Fonte: Autor (2023)
</p>

Na Figura 4 é possível visualizar as três possíveis formas de visualizar o modelo físico implementado no banco de dados na interface gráfica do MongoDB. Na Figura 4a em lista em estrutura similar ao JSON, na Figura 4b em estrutura JSON e na Figura 4c numa estrutura de tabela.

As coleções criada com o projeto físico podem ser encontradas em formato ```.json``` dentro desse repositório, que são: [Coleção de fornecedores](geranciamento_contratos.fornecedor.json), [Coleção de materiais e serviços](geranciamento_contratos.material_servico.json) e o [Coleção de contratos](geranciamento_contratos.contrato.json)

### Conclusões

Este projeto abordou o gerenciamento de contratos com fornecedores em empresas multinacionais, com foco no uso do banco de dados documental, como o MongoDB. Apresentou-se uma metodologia de projeto, envolvendo as etapas de modelagem conceitual, lógica e física. Essa abordagem proporciona flexibilidade e adaptabilidade na gestão dos contratos, permitindo modificações e atualizações de forma eficiente. O uso do MongoDB oferece recursos avançados, como consultas flexíveis e análise de dados, tornando-o uma opção viável para empresas que lidam com contratos complexos, além de proporcionar uma interface gráfica simples e intuitiva.

Ademais, o presente projeto proporcionou o desenvolvimento pessoal no conhecimento de dados, estrutura de armazenamento de dados e banco de dados não relacionais, em especial o documental.
