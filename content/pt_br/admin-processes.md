## XII. Processos de administração
### Rode tarefas de administração/gestão como processos pontuais

O [processo de formação](./concurrency) é o conjunto de processos que são usados para fazer as negociações regulares da app como ela é executada (tais como manipulação de requisições web). Separadamente, os desenvolvedores, muitas vezes desejam fazer tarefas pontuais de administração ou manutenção para a app, tais como:

* Executando migrações de base de dados (ex: `manage.py migrate` no Django, `rake db:migrate` no Rails).
* Executando um console (também conhecido como um [REPL](http://en.wikipedia.org/wiki/Read-eval-print_loop) shell) para rodar código arbitrário ou inspecionar os modelos da app ao vivo no banco de dados. A maioria das linguagens fornece um REPL para rodar o interpretador sem nenhum argumento (ex: `python` or `perl`) ou em alguns casos tem um comando separado (ex: `irb` para Ruby, `rails console` para Rails).
* Executando uma vez os scripts comitados no repositório da app (ex: `php scripts/fix_bad_records.php`).     

Processos de administração pontuais devem ser executados em um ambiente idêntico, como os [processos regulares de longa execução](./processes) da app. Eles rodam contra uma [versão](./build-release-run), usando a mesma [base de código](./codebase) e [configuração](./config) como qualquer processo executado contra essa versão. Códigos de administração devem ser fornecidos com o código da aplicação para evitar problemas de sincronização.

A mesma técnica de [isolamento de dependência](./dependencies) deve ser usada em todos tipos de processos. Por exemplo, se o processo web Ruby usa o comando `bundle exec thin start`, então uma migração de base de dados deve usar `bundle exec rake db:migrate`. Da mesma forma, um programa Python usando Virtualenv deve usar `bin/python` vendorizado para executar tanto o servidor web Tornado e qualquer processo de administração `manage.py`.

Dozes-fatores favorece fortemente linguagens que fornecem um shell REPL fora da caixa, e que tornam mais fácil executar scripts pontuais. Em um deploy local, os desenvolvedores invocam processos de administração pontuais por um comando shell direto dentro do diretório de checkout da app. Em um deploy de produção, desenvolvedores podem usar ssh ou outro mecanismo de execução de comandos remoto fornecido por aquele ambiente de execução do deploy para executar tal processo.
