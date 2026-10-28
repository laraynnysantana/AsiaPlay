AsiaPlay — Banco de Dados Relacional
Projeto acadêmico da disciplina Banco de Dados, com planejamento de um banco relacional para uma plataforma fictícia de streaming de filmes e séries asiáticos. O SGBD escolhido é o MySQL.
1. Domínio do banco de dados
A AsiaPlay é uma plataforma digital de filmes e séries asiáticos, incluindo produções da Coreia do Sul, Japão, China, Tailândia e Taiwan. O banco organiza informações sobre países, usuários, planos e assinaturas, produções audiovisuais, gêneros, temporadas, episódios e avaliações.
2. Contextualização e problema
Sem um SGBD relacional, a empresa poderia manter os dados em planilhas e arquivos separados. Isso aumentaria o risco de:
- duplicação de informações de usuários, produções e planos;
- inconsistências quando uma informação fosse atualizada em apenas um arquivo;
- dificuldade para consultar as produções mais bem avaliadas e os planos mais utilizados;
- registros inválidos, como avaliações ligadas a usuários inexistentes;
- manutenção difícil à medida que o catálogo crescesse;
- controle de acesso e proteção de dados limitados.
3. Proposta de melhoria
A proposta é centralizar as informações em um banco relacional MySQL, utilizando chaves primárias e estrangeiras para identificar os registros e manter a integridade dos relacionamentos.

Benefícios esperados:
- centralização e melhor organização das informações;
- redução de redundâncias;
- maior consistência e integridade referencial;
- facilidade para inserir, atualizar, excluir e consultar dados;
- geração de relatórios por meio de SQL;
- realização de backups e restauração do banco.
4. Modelagem lógica
O projeto contempla dez tabelas. A lista abaixo registra os campos descritos na documentação do projeto.
Tabela	Campos
pais	id_pais, nome, codigo_iso
usuario	id_usuario, nome, email, data_cadastro, data_nascimento
plano	id_plano, nome, preco_mensal, telas_simultaneas, qualidade_maxima
assinatura	id_assinatura, id_usuario, id_plano, data_inicio, data_fim, status
producao	id_producao, titulo, tipo, ano_lancamento, classificacao, id_pais, sinopse
genero	id_genero, nome
producao_genero	id_producao, id_genero
temporada	id_temporada, id_producao, numero_temporada, ano_lancamento
episodio	id_episodio, id_temporada, numero_episodio, titulo, duracao_minutos
avaliacao	id_avaliacao, id_usuario, id_producao, nota, comentario, data_avaliacao


Atenção sobre os tipos de dados: os arquivos enviados para esta documentação não incluem os scripts .sql de criação das tabelas. Por isso, os tipos SQL, as restrições (NOT NULL, UNIQUE, CHECK) e a definição exata das chaves devem ser conferidos no script de criação antes da entrega final. Não presuma que esta lista de campos substitui o script SQL.

Relacionamentos e cardinalidades
- País — Produção (1:N): um país pode estar associado a várias produções; cada produção possui um país de origem cadastrado.
- Usuário — Assinatura (1:N): um usuário pode possuir várias assinaturas ao longo do tempo.
- Plano — Assinatura (1:N): um plano pode estar associado a várias assinaturas.
- Produção — Gênero (N:N): uma produção pode ter vários gêneros e um gênero pode classificar várias produções. A tabela producao_genero resolve esse relacionamento.
- Produção — Temporada (1:N): uma série pode possuir várias temporadas.
- Temporada — Episódio (1:N): uma temporada pode conter vários episódios.
- Usuário — Avaliação (1:N): um usuário pode avaliar diferentes produções.
- Produção — Avaliação (1:N): uma produção pode receber avaliações de diferentes usuários.
A modelagem visual deve ser exportada ou capturada no MySQL Workbench e incluída na entrega.
5. Formas normais
1ª Forma Normal (1FN)
Cada campo armazena um único valor, sem listas ou grupos repetidos. Os gêneros não ficam agrupados em uma coluna de texto: cada associação é registrada em producao_genero.
2ª Forma Normal (2FN)
A tabela deve estar na 1FN e cada atributo não-chave deve depender da chave primária completa. Em producao_genero, a chave composta é formada por id_producao e id_genero, e a tabela não possui atributos adicionais que dependam apenas de parte dessa chave.
3ª Forma Normal (3FN)
A tabela deve estar na 2FN e os atributos não-chave não devem depender transitivamente da chave primária. Por exemplo, os dados dos países ficam em pais, enquanto producao guarda a referência id_pais; os preços são mantidos em plano, enquanto assinatura referencia o plano.
4ª Forma Normal (4FN)
A 4FN evita redundâncias causadas por dependências multivaloradas independentes. Na AsiaPlay, as associações entre produções e gêneros ficam separadas das temporadas e dos episódios. Assim, não é necessário repetir combinações de gêneros e episódios na mesma tabela.
6. Implementação SQL e consultas
A documentação do projeto prevê três scripts SQL:
1. 01_criacao_tabelas.sql — criação do banco e das dez tabelas;
2. 02_insercao_dados.sql — inserção de dados de exemplo;
3. 03_consultas.sql — quinze consultas contextualizadas.
Importante: esses três scripts não estavam entre os arquivos disponibilizados para preparar este README. Adicione-os ao repositório e valide os comandos no MySQL antes de considerar o projeto completo. A atividade exige pelo menos cinco registros em cada tabela, inclusive producao_genero.
Consultas previstas
Nº	Objetivo	Recursos SQL esperados
1	Listar produções e seus países de origem	INNER JOIN, ORDER BY
2	Pesquisar títulos por trecho de texto	LIKE, WHERE
3	Buscar produções por intervalo de anos	BETWEEN, ORDER BY
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
15	Calcular quantidade e duração acumulada dos episódios	COUNT, SUM, JOIN, GROUP BY


Os resultados exatos dependem dos registros existentes nos scripts de inserção. Após executar as consultas, registre no relatório ou nos comentários do script a resposta observada no MySQL Workbench; não informe resultados sem conferir a execução.
Como executar no MySQL Workbench
1. Abra o MySQL Workbench e conecte-se ao servidor.
2. Abra 01_criacao_tabelas.sql e execute o script.
3. Abra 02_insercao_dados.sql e execute o script.
4. Abra 03_consultas.sql e execute as consultas.
5. Confira a lista de tabelas com SHOW TABLES;.
6. Confira se todas as tabelas possuem ao menos cinco registros. Exemplo:
USE AsiaPlay;

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
7. Verifique se as consultas executam sem erro e se as chaves estrangeiras impedem referências inválidas.
7. Exportação e restauração do dump
Um dump é um arquivo que pode conter a estrutura e os dados do banco, facilitando backup e restauração.
Opção A — MySQL Workbench
1. Conecte-se ao servidor.
2. Abra Administration → Data Export.
3. Selecione o schema AsiaPlay e marque todas as tabelas.
4. Escolha Dump Structure and Data.
5. Selecione Export to Self-Contained File.
6. Informe o nome AsiaPlay_dump.sql.
7. Clique em Start Export e confirme que o arquivo foi criado.
Opção B — Terminal
Exportar estrutura e dados:
mysqldump -u root -p --databases AsiaPlay > AsiaPlay_dump.sql
Restaurar o dump:
mysql -u root -p < AsiaPlay_dump.sql
Após restaurar, confira as tabelas, a quantidade de registros e a execução das consultas.
8. Versionamento com Git e GitHub
Inicialize o repositório na pasta do projeto:
git init
git status
git add README.md 01_criacao_tabelas.sql 02_insercao_dados.sql 03_consultas.sql
git commit -m "Adiciona projeto de banco de dados AsiaPlay"
git branch -M main
git remote add origin https://github.com/SEU-USUARIO/AsiaPlay-Banco-de-Dados.git
git push -u origin main
Substitua SEU-USUARIO pelo nome real da conta GitHub e configure o nome/e-mail do Git, se necessário:
git config --global user.name "Seu Nome"
git config --global user.email "seu-email@example.com"
