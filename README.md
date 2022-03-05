<h1 align="center">
  <a href="https://idopterlabs.com.br">IdopterLabs</a> Onboarding: Git workflow
  <b>Inspirado no <a href="https://github.com/TallerWebSolutions/git-workflow/">repositório da Taller</a></b>
</h1>

<p align="center">
  <a href="https://git-scm.com/">
    <img src="https://git-scm.com/images/logo@2x.png" />
  </a>
</p>

> Esse é o onboarding do workflow de git aqui na Idopterlabs, inspirado no repositório de onboarding da Taller, decidimos deixar aberto para comunidade.

## Índice

1. [Introdução](#introduction)
2. [Padrão do nome das branches](#branch-name-pattern)
3. [Workflow](#workflow)
   1. [Atualizando a branch main](#att-branch-main)
   2. [Criar uma nova branch](#new-branch)
4. [Em desenvolvimento](#dev)
   1. [Passos após você terminar o desenvolvimento da sua história](#finish-dev)
5. [Enviando seu código para Stage](#stage)
   1. [Primeira forma: rebase e Merge](#rebase-merge-stage)
   2. [Segunda forma: reset hard e rebase](#reset-rebase-stage)
6. [Enviando seu código para a Main](#main)
   1. [Primeira forma: rebase e Merge](#rebase-merge-main)
   2. [Segunda forma: reset hard e rebase](#reset-rebase-main)

<dl>
  <a name="introduction"></a>
  <dt>Seu primeiro passo é fazer as seguintes configurações:</dt>
  <br>
  <dd>1. Configure seu email da taller no seu github, caso você for usar seu perfil pessoal. Tem um tutorial bacana <a href="https://docs.github.com/pt/github/setting-up-and-managing-your-github-user-account/setting-your-commit-email-address#">nesse link.</a></dd><br>
  <dd>2. Configure a conexão com o gitlab por ssh, uma vez ativa a autenticação por dois fatores você não conseguirá realizar clone dos repositórios por https. Tem um tutorial bacana para gerar a chave ssh <a href="https://docs.github.com/pt/github/authenticating-to-github/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent">nesse link</a> e um outro tutorial supimpa para vincular a sua chave ssh com sua conta do gitlab <a href="https://docs.gitlab.com/ee/ssh/">nesse link</a></dd>
</dl>

Tudo configurado e organizado? Bora entender nosso fluxo e nossos padrões!

> Em cada passo descrito nesse onboarding, o mais importante é você entender o que está fazendo e o motivo de fazer ele. Se algo não estiver claro, crie uma issue, abra um PR e nos ajude a melhorar o onboarding para as pessoas que entrarão depois de você.

## Padrão do nome das branches <a name="branch-name-pattern"></a>

Tanto o nome das branches quanto as mensagens de commit deverão ser em inglês. O nome das nossas branchs são baseadas nos nomes das stories criadas no <a href="https://app.shortcut.com/">Shortchut</a> com isso conseguimos automatizar o fluxo e manter o board atualizado (importante pois nossos clientes acompanham o board também).

Para ficar melhor de entender como nomear nossas branches, bora criar três histórias fictícias:

**Primeira história:** Uma feature, no qual o título é "Permitir que os usuários possam deslogar do app" e o número da story é 14412.

Quais são os nomes de branch desejáveis para essas histórias?

| número   | Barra | Descrição curta |
| -------- | ----- | --------------- |
| sc-14412 | /     | user-logout     |

Para criar uma branch, com a branch `main` atualizada com o origin do projeto, use o seguinte comando trocando as variáveis dentro de placeholders(`${placeholder}`):

`git checkout -b sc-${número da história}/${descrição-curta}`

## Padrão das mensagens de commit <a name="branch-name-pattern"></a>

Nós presamos por um changelog fácil de ler e entender o que aconteceu no passado, então hoje adotamos algumas regrinhas que nos ajuda a manter isso, e nas nossas pesquisas encontramos o padrão de Semantic Commit Messages proposto pelo [Karma](http://karma-runner.github.io/4.0/dev/git-commit-msg.html) e também vimos esse [artigo](https://sparkbox.com/foundry/semantic_commit_messages) que inspirou muito nossos padrões e também é uma das nossas bases hoje.

Então todas as mensagens de commit devem seguir o seguinte padrão:

`categoria: Verbo e descrição no presente.`

Os tipos são: feat, fix, docs, style, refactor, test e chore. Já os verbos mais usados hoje são: update, add, remove, improve, mas você não precisa se limitar a isso. Uma dica é você baixar um dos nossos projetos ativos, e usar o tig para ler uma parte do histórico dos commits e ver os padrões "na prática".

Exemplo e explicação é tudo nessa vida, né? Aqui embaixo você terá mais detalhes sobre ao se refere cada categoria de commit citado anteriormente.

| Tipo          | Descrição                                                                                                                                                                | Exemplo:                                                  |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------- |
| **feat:**     | Implementação de alguma novidade: Uma feature, funcionalidade, tela, opção de ação, etc.                                                                                 | `feat: add node cachetag to ensure content invalidation`  |
| **fix:**      | Implementação em que a única finalidade é corrigir algum comportamento errôneo do sistema.                                                                               | `fix: improve url object verification`                    |
| **docs:**     | Alterações ou incrementos na documentação do projeto.                                                                                                                    | `docs: add instructions to force new drupal installation` |
| **style:**    | Para alterar identação, vírgulas(retirar ou colocar), etc. Um exemplo é se você está adicionando o prettier ao projeto. Pense nele como estilo de código e não como CSS. | `style: convert tabs to spaces`                           |
| **refactor:** | Refatoração/melhoria de código sem alterar funcionalidade.                                                                                                               | `refactor: extract data provider method`                  |
| **test:**     | Refatoração ou adição de testes. Apenas testes.                                                                                                                          | `test: update test to add scenario for columnist article` |
| **chore:**    | Alterações em arquivos de configuração do projeto ou do ambiente. Logs, pacotes, dependências e etc.                                                                     | `chore: add scripts to deploy on vercel`                  |

<br>Caso você queira já deixar configurado na sua máquina, existe a [git-semantic-commits](https://github.com/fteem/git-semantic-commits) que já faz a magia da mensagem do commit pra ti. Essa lib é escolha sua usar ou não, o importante é você seguir os nossos padrões.

## Workflow <a name="workflow"></a>

### Atualizando a branch main <a name="att-branch-main"></a>

A _fonte da verdade_ é a branch `main`. Todas as branchs devem ser criadas a partir da `main` e deverão ser mergeadas na `main` através de um merge request.

Quando você estiver desenvolvendo, certifique-se que sua branch `main` local esteja atualizada com o `origin` executando:

```
git remote update
git reset --hard origin/main
```

`git remote update`: esse comando pega todas as atualizações do origin(desde a branch que você está até as outras branchs do projeto), mas não fara o merge das alterações em sua branch local.

`git reset --hard origin/main`: esse comando reseta tua branch local com a origin. Ou seja, tua branch local ficará idêntica ao origin.

Se voce estiver se perguntando "o que é essa porra de `origin` que tanto falam aqui?", o origin é um termo default que o git usa para se referir ao repositório(`remote`) de onde você clonou o projeto(seja ele github, gitlab ou bitbucket). Você pode mudar esse nome default, mas não há necessidade. Você pode acessar a [documentação oficial ](https://git-scm.com/book/en/v2/Git-Basics-Working-with-Remotes) para ter mais detalhes.

### Criar uma nova branch <a name="new-branch"></a>

Atualize sua branch `main` conforme explicamos no ítem acima e execute o comando `git checkout -b` + o nome da branch(com o padrão explicado na sessão Padrão do nome das branches)

```
git checkout -b sc-${número da história}/${descrição da issue ou história}
```

### Em desenvolvimento <a name="dev"></a>

> ⚠️ INFORMAÇÃO IMPORTANTE: a branch que você estiver trabalhando deve SEMPRE estar atualizada com a `main`, para isso usamos o rebase. É sempre bom você ir fazendo rebase durante durante o desenvolvimento, porque se você deixar pra hora de entregar, existe a possibilidade de você passar muito tempo resolvendo conflito.
> Para fazer rebase da `main`, execute:

```
git remote update
git rebase origin/main
```

Fique atento porque você não vai conseguir fazer rebase se não tiver commitado ou usado um `git stash` no que você estava trabalhando.

_**Terminou sua história?**_ <a name="finish-dev"></a>

Abra um pull request para a `main` e solicite o code review de uma pessoas do time.

<br>
<p align="center">⚠️ AVISO: Nunca faça push de commits diretamente na branch <b>main</b>. Seu código só vai pra main depois do merge request, code review e homologação.</p>
<p align="center"><img src="https://i.imgflip.com/5643lw.jpg" alt="imagem com a frase: proibido push de commits diretamente na branch main na frente das crianças, sujeito a paulada"/></p>
<p align="center"><i>é meme(zoeira), ninguém vai te bater ou dar pauladas.</i></p>

Terminou sua história, code review feito, alterações ok é hora de enviar sua história para `stage`.

### Enviando seu código para a Main <a name="main"></a>

História homologada, tudo certo e é hora de mandar pra main! Como falamos lá em cima, a ideia é que tua branch esteja sempre atualizada com o origin/main e também temos duas formas de fazer a mesma coisa

Vá para a sua branch, caso você não esteja nela.

1 - Faz o rebase de main pra ficar tudo atualizado

```
git remote update
git rebase origin/main
```

se ela já estiver atualizada, você pode ir direto para o proximo passo, caso ela não esteja atualizada, faça o push com os commits atualizados

```
*** resolve conflitos ***
git push origin sc-numero/my-branch
```

**Primeira forma: rebase e Merge** <a name="rebase-merge-main"></a>

2 - Vai pra main e atualiza main com a origin

```
git checkout main
git remote update
git reset --hard origin/main
```

3 - Adicione seus commits na main com o merge da sua branch:

```
git merge my-branch
```

<p align="center">⚠️ <b>Atenção:</b> verifique se seu merge foi <b>fast-forward</b>. Se e quando estiver tudo certo, faça o push.</p>

```
git push origin main
```

**Segunda forma: reset hard e rebase** <a name="reset-rebase-main"></a>

Assim como para enviar para stage, você pode fazer um hard reset em stage com sua branch e depois fazer rebase com o origin stage para não perder nenhum commit que já estava lá.

```
git checkout main
git remote update
```

esse primeio passo é para termos certeza que você tem todos commits do origin localmente.

```
git reset --hard fs/my-branch
git rebase origin/main
git push origin main
```

## Importante: não fique com o código só em sua máquina

É uma grande fragilidade quando ficamos com o código somente em
