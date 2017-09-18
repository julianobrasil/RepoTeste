# RepoTeste
o objetivo deste repositório é somente aprender como o github trabalha.

## Instruções iniciais

Este guia supõe que você tem algum conhecimento básico sobre o GIT:
1. Conhece os seguintes conceitos:
    - O que é um *_branch_*
    - Uso básico do *_checkout_*
    - Uso básico do *_commit_*
    - Uso básico do *_pull_*
    - Uso básico do *_push_*
    - Uso básico do *_log_*
    - Uso básico do editor de textos *_vi_*
    
2. Tem instalado o [GIT](https://git-scm.com/downloads) em seu computador.

### <span id="clonando_o_projeto">Clonando o projeto</span>

1. Antes de qualquer coisa, é preciso fazer um **fork** do projeto na sua conta do github para trabalhar. Isso pode ser feito no canto superior direito da sua tela (em baixo da foto do seu perfil).

![Fork](/assets/img/trabalhando_com_git/fork.png)

2. Depois de ter feito o **fork**, você terá uma cópia do projeto na sua conta do github. **Da sua conta do github** clone o projeto no **seu HD local**:

**`git clone https://github.com/sua_conta_do_github/nome_do_repositorio.git`**

#### Se o projeto já existir no seu HD (repositório local)...

Ao longo do processo de desenvolvimento, após a primeira clonagem, é provavel que você não precise repetiro que foi feito acima. Em vez disso, sempre que for fazer qualquer alteração (passos abaixo), antes você deve trazer, para o seu HD (repositório local) as últimas modificações feitas no **repositório original**:

**`git pull https://github.com/conta_original_do_projeto_no_github/nome_do_repositorio.git master:master`**

Como essa é uma operação que costuma ser feita com certa frequência, você pode configurar um `remote` para reduzir o comando acima. Por padrão, ao <a href="#clonando_o_projeto">clonar o projeto no seu HD</a> (`git clone...`) você já criou um `remote` chamado `origin`, "ligando" o ramo `master` remoto da *SUA CONTA* do github . Assim, toda vez que você estiver no ramo master e fizer um `git pull`, você irá sincronizar o seu master com o master remoto (da SUA CONTA). O que vamos fazer agora é criar um `remote` de ligação com o ramo master da conta original. Vamos chamar nosso novo remote de **`upstream`**: 

**`git remote add upstream https://github.com/conta_original_do_projeto_no_github/nome_do_repositorio.git`**

Agora, quando quiser sincronizar o seu ramo master com o ramo remoto ORIGINAL, basta:

**`git pull upstream master:master`** ou, se já estiver no seu ramo master local: **`git pull upstream master`**

Se desejar sincronizar o repositório remoto do github (clonado na sua conta) com o repositório remoto original, após o comando acima, você deve fazer:

**`git push -f`**

### Trabalhando com o projeto em seu computador

#### Regra 1: Toda modificação deve ser feita em um ramo (_branch_) específico

Não tenha receio de criar _branches_ em sua cópia local do repositório. O trabalho com o GIT pressupõe que você irá criar, localmente, _branches_ para tudo o que desejar. É comum, em uma fábrica de software, que um desenvolvedor crie vários _branches_ locais em um único dia.

Para criar um _branch_ e poder trabalhar, é simples:

**`git checkout -b nome_do_meu_novo_branch`**

Trabalhe à vontade no seu novo _branch_, fazendo quantos _commits_ você precisar.

### Antes de finalizar o seu trabalho...

Uma vez que a sua funcionalidade esteja pronta, após você ter feito vários _commits_ locais, você deve arrumar o seu trabalho antes de enviá-lo para a **sua conta do github**.

No seu processo de trabalho você terá feito muitos commits no ramo local criado para implementar uma funcionalidade ou corrigir um  bug qualquer. Esse processo gera um histórico de alterações que não precisa (apesar de poder) ficar disponível para consultas. Muitos commits são extremamente desnecessários, como nesse exemplo:
  1. Você faz um _commit_ e percebe, logo depois que o programa não está compilando porque "Faltou um ponto-e-vírgula"
  2. Então você faz outro _commit_ com a adição do ponto-e-vírgula e a seguinte mensagem: "Adição de ponto-e-vírgula"
  
Esse _commit_ é interessante no seu repositório local _durante_ o seu trabalho. Depois de finalizado, é algo completamente dispensável saber que existe um _commit_ no histórico que guarda o código com a adição do ponto-e-vírgula e que, antes dele, o _commit_ anterior não tem o referido ponto-e-vírgula.

Para resolver isso, temos a regra do _squash_

#### Regra 2: Faça _squash_ antes de enviar o seu projeto para o github

O _squash_ é uma operação do git que permite a você colabar os _N_ últimos _commits_ em um único _commit_, resumindo assim a história detalhada dos vários commits intermediários que foram feitos durante o seu trabalho.

Exemplo: Suponha que em determinado momento seu histórico esteja assim:

```
e3c060b adicionei outro ponto-e-vírgula
5ca981c adicionei o ponto-e-vírgula
1453368 alteração importante
```
Note que a intenção acima era fazer um único _commit_ com a mensagem "alteração importante" (_commit_ **1453368**). Por um acidente você foi obrigado a fazer mais 2 commits com alterações mínimas (_commits_ **5ca981c** e **e3c060b**). Então, estando no seu ramo de trabalho, você deseja que as alterações feitas nos _commits_ extras estejam no commit "alteração importante": os **3 últimos _commits_** precisam ser colabados. O comando que você deseja é:

`git rebase -i HEAD~3`

Ao executar esse comando, você entrará em um **modo interativo de ajustes**. A primeira coisa que o GIT irá exigir é que você escolha o que deseja fazer (quais commits devem ser preservados - **_pick_** - e quais devem ser colabados - **_squash_**).

Isso (resultado do comando `git rebase -i HEAD~3`):
```
pick 1453368 alteração importante
pick 5ca981c adicionei o ponto-e-vírgula
pick e3c060b outro ponto-e-vírgula
```

deve ficar assim:
```
pick 1453368 alteração importante
squash 5ca981c adicionei o ponto-e-vírgula
squash e3c060b outro ponto-e-vírgula
```

Após fazer as alterações conforme as instruções, salvar e sair, o GIT continuará o processo de squash, te dando uma oportunidade de editar uma mensagem de commit:

```
alteração importante

# This is the commit message #2:

 adicionei o ponto-e-vírgula

# This is the commit message #3:

outro ponto-e-vírgula

```

Se quiser alterar, faça-o. Senão, salve e saia do vi para que o GIT finalize o processo. Após isso, o comando git log irá mostrar o seu histórico assim:

`862c326 alteração importante`

### Enviando o seu ramo para o github

Neste ponto você já:

1. Criou um ramo
2. Trabalho no seu ramo, fazendo várias alterações e vários _commits_
3. Finalizou o trabalho fazendo o _squash_ dos _commits_, se necessário

E agora precisa enviar o resultado para o github:

**`git push -u origin nome_do_meu_novo_branch`**

Esse comando cria uma cópia do seu ramos local (chamado "nome_do_meu_novo_branch") na sua conta do github, em um _branch_ de igual nome (ou seja, não altera o _branch master_ no seu repositório remoto).

O próximo e último passo agora é fazer um *_pull request_* (PR) para o projeto original, com a intenção de fundir suas alterações ao ramo _master_ (ou outro definido pela equipe) do projeto original.

![Pull Request](/assets/img/trabalhando_com_git/PullRequest.png)

