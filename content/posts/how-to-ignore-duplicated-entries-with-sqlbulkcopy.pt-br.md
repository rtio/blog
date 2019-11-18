+++
title = "Ignorar registros duplicados usando SqlBulkCopy do C#"
date = "2017-03-09"
author = "rtio"
cover = "sql-server.jpeg"
tags = ["csharp", "sqlserver"]
keywords = ["sqlbulkcopy", "csharp", "sql"]
description = "Eu estava construindo um importador de arquivos, porém em determinado momento me deparei com o seguinte cenário, eu tinha que inserir os dados de um mesmo arquivo várias vezes durante um determinado período. Os dados necessitavam estar atualizados o máximo possível e durante esse período o Windows Service tinha que ler o mesmo arquivo várias vezes e atualizar esses dados."
showFullContent = false
+++

Eu estava construindo um importador de arquivos, porém em determinado momento me deparei com o seguinte cenário, eu tinha que inserir os dados de um mesmo arquivo várias vezes durante um determinado período. Os dados necessitavam estar atualizados o máximo possível e durante esse período o Windows Service tinha que ler o mesmo arquivo várias vezes e atualizar esses dados.

Um dos pronto primordiais desse serviço é a performance, então ele foi desenhado para trabalhar em Multithreading para ganhar agilidade e usava uma classe chamada SqlBulkCopy do C# para inserir os dados de um arquivo em apenas uma execução.

O grande problema surgiu devido à necessidade de ler um mesmo arquivo mais de uma vez, sendo necessário consultar os dados antes de inserir, remover os dados duplicados da lista e só depois usar o “Bulk”. Esse processo adicionaria tempo e uma carga maior no serviço, que eu não gostaria que existisse.

A solução que encontrei foi executar um pequeno script SQL, para que o banco de dados trate de forma diferente os registros duplicados na inserção de dados. Segue o script:

```sql
CREATE UNIQUE INDEX not_duplicate_data_index
    ON Table (field_one, field_two, field_three)
WITH IGNORE_DUP_KEY
```

Vamos falar um pouco mais sobre esse script:

### O argumento `UNIQUE`

Cria um índice exclusivo em uma tabela ou exibição. Um índice exclusivo é aquele no qual duas linhas não podem ter o mesmo valor de chave de índice.

### O argumento `IGNORE_DUP_KEY = { ON | OFF }`

Especifica a resposta de erro quando uma operação de inserção tenta inserir valores da chave duplicada em um índice exclusivo. A opção IGNORE_DUP_KEY aplica-se apenas a operações de inserção depois que o índice é criado ou recriado. A opção não tem nenhum efeito ao executar `CREATE INDEX`, `ALTER INDEX` ou `UPDATE`. O padrão é `OFF`.

`ON`

Uma mensagem de aviso será exibida quando valores de chave duplicados forem inseridos em um índice exclusivo. Ocorrerá falha somente nas linhas que violarem a restrição de exclusividade.

`OFF`

Ocorrerá uma mensagem de erro quando os valores de chave duplicados forem inseridos em um índice exclusivo. Toda a operação `INSERT` será revertida.

Na sintaxe compatível com versões anteriores, `WITH IGNORE_DUP_KEY` é equivalente a `WITH IGNORE_DUP_KEY = ON`.