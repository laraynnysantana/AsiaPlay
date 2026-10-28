AsiaPlay — Plataforma de Filmes e Séries Asiáticos
Projeto acadêmico de banco de dados relacional para organizar a operação de uma plataforma fictícia de streaming de produções asiáticas, incluindo países de origem, usuários, planos, assinaturas, produções, gêneros, temporadas, episódios e avaliações.

SGBD: MySQL
Modelagem: MySQL Workbench
Banco de dados: AsiaPlay
📌 Sumário
1. Domínio do banco de dados
2. Contextualização
3. Proposta de melhoria
4. Modelagem lógica
5. Relacionamentos e cardinalidades
6. Formas normais
7. Implementação SQL e dados de teste
8. Consultas SQL
9. Testes
10. Exportação e restauração do dump
11. Como executar o projeto
12. Estrutura sugerida do repositório
13. Entrega na Sala de Aula
1. Domínio do banco de dados
O domínio escolhido é a administração da AsiaPlay, uma empresa fictícia de streaming de filmes e séries asiáticos. O banco de dados organiza informações sobre:
- países de origem das produções;
- usuários cadastrados;
- planos e assinaturas;
- filmes e séries;
- gêneros audiovisuais;
- temporadas e episódios;
- avaliações dos usuários.
O objetivo é centralizar essas informações e permitir consultas confiáveis sobre o catálogo, os países de origem, os planos contratados e as avaliações das produções.
2. Contextualização
Sem um SGBD relacional, a plataforma poderia manter seus registros em planilhas, documentos e arquivos separados. Esse método pode causar:
- Duplicação: os dados de usuários, produções e planos podem aparecer em vários arquivos.
- Inconsistência: uma alteração pode ser feita em um arquivo e não nos demais.
- Consultas demoradas: localizar as produções mais bem avaliadas ou os planos mais utilizados exigiria trabalho manual.
- Falhas de integridade: podem surgir avaliações relacionadas a usuários inexistentes ou episódios sem uma temporada válida.
- Dificuldade de manutenção: o crescimento do catálogo aumenta a complexidade da organização.
- Segurança limitada: pode ser mais difícil controlar o acesso e proteger as informações.
3. Proposta de melhoria
A solução proposta é utilizar o MySQL para centralizar os dados em tabelas relacionadas. As chaves primárias identificam os registros e as chaves estrangeiras conectam as tabelas, ajudando a preservar a integridade referencial.
Benefícios
- centralização das informações;
- redução de redundâncias;
- maior consistência dos registros;
- facilidade para cadastrar, atualizar e consultar dados;
- geração de relatórios por meio de SQL;
- melhor organização do catálogo de filmes e séries;
- possibilidade de criar cópias de segurança e restaurar o banco.
4. Modelagem lógica
O projeto utiliza dez tabelas. Os campos abaixo seguem a documentação do projeto. Os tipos SQL e as restrições exatas devem ser confirmados no script de criação do banco, pois os arquivos SQL não foram disponibilizados junto à documentação consultada.
4.1. pais
Armazena os países de origem das produções.
Campo	Tipo	Restrições/finalidade
id_pais	Conferir no script SQL	Chave primária
nome	Conferir no script SQL	Nome do país
codigo_iso	Conferir no script SQL	Código ISO do país


4.2. usuario
Armazena os dados dos usuários cadastrados.
Campo	Tipo	Restrições/finalidade
id_usuario	Conferir no script SQL	Chave primária
nome	Conferir no script SQL	Nome do usuário
email	Conferir no script SQL	E-mail do usuário
data_cadastro	Conferir no script SQL	Data do cadastro
data_nascimento	Conferir no script SQL	Data de nascimento


4.3. plano
Registra os planos oferecidos pela plataforma.
Campo	Tipo	Restrições/finalidade
id_plano	Conferir no script SQL	Chave primária
nome	Conferir no script SQL	Nome do plano
preco_mensal	Conferir no script SQL	Preço mensal
telas_simultaneas	Conferir no script SQL	Quantidade de telas simultâneas
qualidade_maxima	Conferir no script SQL	Qualidade máxima oferecida


4.4. assinatura
Registra as assinaturas dos usuários.
Campo	Tipo	Restrições/finalidade
id_assinatura	Conferir no script SQL	Chave primária
id_usuario	Conferir no script SQL	Chave estrangeira para usuario.id_usuario
id_plano	Conferir no script SQL	Chave estrangeira para plano.id_plano
data_inicio	Conferir no script SQL	Data de início da assinatura
data_fim	Conferir no script SQL	Data de término, quando aplicável
status	Conferir no script SQL	Situação da assinatura


4.5. producao
Armazena os filmes e as séries disponíveis no catálogo.
Campo	Tipo	Restrições/finalidade
id_producao	Conferir no script SQL	Chave primária
titulo	Conferir no script SQL	Título da produção
tipo	Conferir no script SQL	Tipo de produção
ano_lancamento	Conferir no script SQL	Ano de lançamento
classificacao	Conferir no script SQL	Classificação indicativa
id_pais	Conferir no script SQL	Chave estrangeira para pais.id_pais
sinopse	Conferir no script SQL	Resumo da produção


4.6. genero
Registra os gêneros disponíveis no catálogo.
Campo	Tipo	Restrições/finalidade
id_genero	Conferir no script SQL	Chave primária
nome	Conferir no script SQL	Nome do gênero


4.7. producao_genero
Associa as produções aos respectivos gêneros.
Campo	Tipo	Restrições/finalidade
id_producao	Conferir no script SQL	Chave estrangeira para producao.id_producao; participa da chave primária composta
id_genero	Conferir no script SQL	Chave estrangeira para genero.id_genero; participa da chave primária composta


4.8. temporada
Armazena as temporadas das séries.
Campo	Tipo	Restrições/finalidade
id_temporada	Conferir no script SQL	Chave primária
id_producao	Conferir no script SQL	Chave estrangeira para producao.id_producao
numero_temporada	Conferir no script SQL	Número da temporada
ano_lancamento	Conferir no script SQL	Ano de lançamento da temporada


4.9. episodio
Registra os episódios de cada temporada.
Campo	Tipo	Restrições/finalidade
id_episodio	Conferir no script SQL	Chave primária
id_temporada	Conferir no script SQL	Chave estrangeira para temporada.id_temporada
numero_episodio	Conferir no script SQL	Número do episódio
titulo	Conferir no script SQL	Título do episódio
duracao_minutos	Conferir no script SQL	Duração em minutos


4.10. avaliacao
Armazena as avaliações feitas pelos usuários.
Campo	Tipo	Restrições/finalidade
id_avaliacao	Conferir no script SQL	Chave primária
id_usuario	Conferir no script SQL	Chave estrangeira para usuario.id_usuario
id_producao	Conferir no script SQL	Chave estrangeira para producao.id_producao
nota	Conferir no script SQL	Nota atribuída à produção
comentario	Conferir no script SQL	Comentário do usuário
data_avaliacao	Conferir no script SQL	Data da avaliação


Observação: os tipos de dados, os valores permitidos, as restrições (NOT NULL, UNIQUE, CHECK) e a definição das chaves devem ser conferidos no modelo do MySQL Workbench e no script SQL definitivo. Esta documentação não inventa definições que não estavam presentes nos arquivos consultados.

5. Relacionamentos e cardinalidades
Relacionamento	Cardinalidade	Explicação
pais → producao	1:N	Um país pode estar associado a várias produções; cada produção possui um país de origem cadastrado.
usuario → assinatura	1:N	Um usuário pode possuir várias assinaturas ao longo do tempo.
plano → assinatura	1:N	Um plano pode estar associado a várias assinaturas.
producao ↔ genero	N:N	Uma produção pode possuir vários gêneros e um gênero pode classificar várias produções. A tabela producao_genero implementa essa relação.
producao → temporada	1:N	Uma série pode possuir várias temporadas.
temporada → episodio	1:N	Uma temporada pode conter vários episódios.
usuario → avaliacao	1:N	Um usuário pode avaliar diferentes produções.
producao → avaliacao	1:N	Uma produção pode receber avaliações de diferentes usuários.


6. Formas normais
6.1. Primeira Forma Normal (1FN)
Cada campo deve armazenar um único valor, sem listas ou grupos repetidos. Na AsiaPlay, os gêneros não são armazenados em uma coluna separados por vírgulas; cada associação entre produção e gênero é registrada individualmente em producao_genero.
6.2. Segunda Forma Normal (2FN)
A tabela deve estar na 1FN e todos os atributos não-chave devem depender da chave primária completa. Em producao_genero, a chave primária é composta por id_producao e id_genero, e a tabela contém apenas esses campos. Assim, não há atributos adicionais que dependam somente de parte dessa chave.
6.3. Terceira Forma Normal (3FN)
A tabela deve estar na 2FN e os atributos não-chave não devem depender transitivamente da chave primária. Os dados dos países ficam em pais, enquanto producao armazena a referência id_pais. Da mesma forma, os preços ficam em plano, enquanto assinatura referencia o plano contratado.
6.4. Quarta Forma Normal (4FN)
A 4FN busca evitar redundâncias causadas por dependências multivaloradas independentes. Na AsiaPlay, as associações entre produções e gêneros ficam separadas das temporadas e dos episódios. Dessa forma, não é necessário repetir todas as combinações de gêneros e episódios em uma mesma tabela.
7. Implementação SQL e dados de teste
A documentação do projeto prevê três scripts SQL:
- 01_criacao_tabelas.sql — criação do banco e das tabelas;
- 02_insercao_dados.sql — inserção dos dados de exemplo;
- 03_consultas.sql — consultas SQL do projeto.
A atividade exige no mínimo cinco registros em cada tabela, incluindo a tabela associativa producao_genero.
Os scripts SQL não estavam entre os arquivos disponíveis para preparar este README. Portanto, confirme que eles existem no repositório, que contêm os comandos descritos e que foram executados com sucesso antes de entregar o trabalho.

8. Consultas SQL
O projeto prevê 15 consultas contextualizadas, utilizando comandos e funções como INNER JOIN, LEFT JOIN, WHERE, LIKE, BETWEEN, GROUP BY, ORDER BY, COUNT, SUM e AVG.
Nº	Objetivo da consulta	Recursos SQL
1	Listar produções e países de origem	INNER JOIN, ORDER BY
2	Pesquisar títulos por trecho do nome	LIKE, WHERE
3	Pesquisar produções por intervalo de anos	BETWEEN, ORDER BY
4	Listar produções de determinado gênero	JOIN, WHERE
5	Contar produções por país	COUNT, GROUP BY
6	Calcular a média das avaliações	AVG, GROUP BY
7	Listar usuários com assinaturas ativas	JOIN, WHERE
8	Somar os preços dos planos	SUM
9	Calcular o preço médio dos planos	AVG
10	Pesquisar planos por faixa de preço	BETWEEN, ORDER BY
11	Contar produções por gênero	JOIN, GROUP BY
12	Listar episódios por duração	WHERE, ORDER BY
13	Consultar usuários e suas avaliações	JOIN, ORDER BY
14	Contar temporadas por série	LEFT JOIN, GROUP BY
15	Calcular o total e a duração acumulada dos episódios	COUNT, SUM, JOIN, GROUP BY


As respostas exatas dependem dos dados inseridos no banco. Execute as consultas no MySQL Workbench e registre os resultados obtidos; não informe respostas presumidas sem validar a execução.
9. Testes
Depois de executar os scripts, verifique:
- se as dez tabelas foram criadas;
- se todas as tabelas possuem pelo menos cinco registros;
- se as chaves primárias identificam os registros;
- se as chaves estrangeiras mantêm os relacionamentos;
- se as 15 consultas são executadas sem erros;
- se os resultados correspondem aos dados cadastrados;
- se o dump pode ser restaurado corretamente.
Para listar as tabelas:
USE AsiaPlay;
SHOW TABLES;
Para conferir a quantidade de registros por tabela:
SELECT 'pais' AS tabela, COUNT(*) AS total FROM pais
UNION ALL SELECT 'usuario', COUNT(*) FROM usuario
UNION ALL SELECT 'plano', COUNT(*) FROM plano
UNION ALL SELECT 'assinatura', COUNT(*) FROM assinatura
UNION ALL SELECT 'producao', COUNT(*) FROM producao
UNION ALL SELECT 'genero', COUNT(*) FROM genero
UNION ALL SELECT 'producao_genero', COUNT(*) FROM producao_genero
UNION ALL SELECT 'temporada', COUNT(*) FROM temporada
UNION ALL SELECT 'episodio', COUNT(*) FROM episodio
UNION ALL SELECT 'avaliacao', COUNT(*) FROM avaliacao;
Cada linha retornada deve apresentar um total igual ou superior a cinco.
10. Exportação e restauração do dump
Um dump é um arquivo que contém a estrutura e os dados do banco de dados, permitindo realizar backups e restaurar o projeto em outro ambiente.
10.1. Exportação pelo MySQL Workbench
1. Abra o MySQL Workbench e conecte-se ao servidor.
2. Acesse Administration → Data Export.
3. Selecione o banco AsiaPlay.
4. Marque todas as tabelas.
5. Selecione Dump Structure and Data.
6. Escolha Export to Self-Contained File.
7. Defina o nome AsiaPlay_dump.sql.
8. Clique em Start Export.
9. Verifique se o arquivo foi criado corretamente.
10.2. Exportação pelo terminal
mysqldump -u root -p --databases AsiaPlay > AsiaPlay_dump.sql
O comando exporta a estrutura e os dados para o arquivo AsiaPlay_dump.sql.
10.3. Restauração do dump
mysql -u root -p < AsiaPlay_dump.sql
Após a restauração, verifique as tabelas, as quantidades de registros e a execução das consultas.
11. Como executar o projeto
1. Instale e abra o MySQL Server e o MySQL Workbench.
2. Conecte-se ao servidor MySQL.
3. Abra e execute 01_criacao_tabelas.sql.
4. Execute 02_insercao_dados.sql.
5. Execute 03_consultas.sql.
6. Confira os resultados das consultas e os registros de todas as tabelas.
7. Exporte o banco para AsiaPlay_dump.sql.
8. Salve a imagem da modelagem para incluir na entrega.
Execute os scripts na ordem indicada para que as tabelas existam antes da inserção de dados e das consultas.

