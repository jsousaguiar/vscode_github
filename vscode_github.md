# Instalação Git

Instalar o Git: [Baixe Aqui](https://github.com/git-for-windows/git/releases/download/v2.32.0.windows.2/Git-2.32.0.2-64-bit.exe)

Instale com as opções padrão

## Configurar usuário no git:

No terminal, digite:
```sh
git config --global user.name "Nome Completo Usuário"
git config --global user.email "usuario@provedor.com"
```

## Criar chave SSH

O github mudou recentemente para não permitir modificação nos repositórios para acesso via usuário e senha, somente com chave SSH. Então vamos criar uma:
Criar chave ssh para acesso ao github (necessário uma chave por computador):

```sh
ssh-keygen.exe -t ed25519 -C "usuario@provedor.com"
```

Pode-se deixar a senha em branco.

O comando acima cria um par de chaves, uma pública e outra privada. A chave pública vamos colocar no github, a chave privada fica guardada no computador.

Copia a chave pública para a área de transferência:
 
No Terminal do Linux ou no Powershell do Windows, digite:
```sh
cat ~/.ssh/id_ed25519.pub | clip
```
Se preferir usar o Prompt de Comando do Windows, digite:

```sh
type .ssh\id_ed25519.pub | clip
```

Adiciona a chave no github:

Settings -> SSH and GPG keys -> New SSH key

Dê o nome do computador e cole a chave (tem que ter uma chave por computador)

git e github configurados!

# Principais comandos Git

## Clonar o repositório
A pasta será clonada para o diretório atual, criando uma pasta para para cada projeto clonado, então, antes de dar o comando, navegue até o diretório onde ficarão os projetos.
```sh
git clone git@github.com:usuario/repositorio.git
```



## Puxar as mudanças do repositório origem (github)

```sh
git pull
```

## Verificar status do repositório local

```sh
git status
```

## Verificar histórico de commits

```sh
git log
```

## Adicionar arquivos para preparar o commit (staging)

```sh
git add arquivo1.py arquivo2.py ...
```

Para os comandos em que se relaciona o nome dos arquivos, pode-se adicionar um arquivo por vez, ou usar caracteres especiais, como \*.py, ou \* para todos

## Fazer o commit

Faz o commit das mudanças adicionadas ao stage

```sh
git commit -m "Mensagem de commit"
```

## Envia mudanças para o repositório de origem (github)

```sh
git push
```

## Descartar mudanças locais

-   Antes do stage

Caso as mudanças a serem desfeitas ainda não tenham sido adicionadas ao estage (antes do comando `git add`), usa-se o commando `git checkout`:

```sh
git checkout arquivo1.py arquivo2.py ...
```

-   Após o stage

Caso as mudanças a serem desfeitas já tenha sido adicionadas ao estage (após o comando `git add`), usa-se o commando `git reset`:

```sh
git reset arquivo1.py arquivo2.py ...
```

-   Após o commit, mas antes do push

Caso o commit a ser desfeito ainda não tenha sido enviado para o repositório remoto (antes do comando `git push`), pode-se, ao invés de criar um commit para desfazer as mudanças, simplesmente descartar o commit indesejardo. Desta forma, o commit não aparecerá no histórico. Para tal, usa-se o commando `git reset`:

```sh
git reset [--soft|--hard] HEAD~1
```

O comando acima retorna o branch local para o último commit (`HEAD~1` significa um commit antes do commit corrente). A opção `--soft` faz com que as mudanças feitas pelo commit descartado sejam colocadas na área de stage (ou seja, apesar do commit ser desfeito, será possível modificar e refazer as mudanças pretendidas). Já a opção `--hard` descarta totalmente as mudanças do último commit (deve ser usado com cuidado).

## Desfazer mudanças no repositório(github)

Para se reverter o último commit:

```sh
git revert HEAD [--no-edit]
git push
```

Isto criará um novo commit desfazendo as mudanças do commit indicado. Se a opção `--no-edit` for passada, a mensagem de commit será simplesmente "Revert {mensagem_commit_anterior}".

Para se reverter qualquer commit:

```sh
git revert {número-do-commit} [--no-edit]
git push
```

Esta operação só terá sucesso se não houver conflitos. Não abordaremos os casos de resolução de conflitos, pois torna a operação mais complexa, fugindo ao escopo de um documento introdutório.

Caso haja conflito e queira voltar o estado anterior, basta dar o seguinte comando:

```sh
git reset *
```

## Trabalhar com stash
O stash é um local onde podem ser guardadas versões locais que estão sendo trabalhadas. Ajuda a salvar um trabalho e voltar a trabalhar na última versão do projeto.

Criar um stash:
```sh
git stash save "Detalhes para lembrar em que estava trabalhando"
```
Verificar os stashes salvos:
```sh
git stash list
```
Voltar a trabalhar em um stash específico:
```sh
git stash {pop/appy} {id_do_stash}
```
A id do stash pode ser encontrada observando na lista de stashes o número dentro dos colchetes "stash@{0}"

Ao usar o comando `pop` o stash volta para a área de trabalho atual e é retirado da lista de stashes, sendo necessário salvá-lo novamente caso não termine o trabalho.

Ao usar o comando `apply`o stash continua na lista com as altereações salvas anteriormente. Para salvar a nova versão, será necessário excluir o stash anterior ou salvar um novo stash e manter ambas as versões.

Excluir um stash específico:
```sh
git stash drop {id_do_stash}
```
Para excluir todos os stashes:
```sh
git stash clear
```

# Extensões do VSCode recomendadas

-   Python (`ms-python.python`): Extensão necessária para trabalhar com arquivos python
-   Pylance (`ms-python.vscode-pylance`): Melhora o IntelliSense para python (sugestões e detecção de erros)
-   Jupyter (`ms-toolsai.jupyter`): parta trabalhar com arquivos do Jupyter Noteook
-   Python Auto Venv (`whinarn.python-auto-venv`): Ativa o virtual environment automaticamente
-   GitLens (`eamodio.gitlens`): Recursos adicionais para trabalhar com Git
-   Live Share Extension Pack (`ms-vsliveshare.vsliveshare-pack`): para colaboração em tempo real
-   Notepad++ keymap (`ms-vscode.notepadplusplus-keybindings`): para utilizar os atalhos do Notepad++ no VSCode
- Bracket Pair Colorized 2 (`coenraads.bracket-pair-colorizer-2`): para melhorar a visulização de parêntesis no código
-   Hjson (`laktak.hjson`): para trabalhar com arquivos no formato hjson

# Pacotes python para instalar no ambiente

O VSCode utiliza alguns pacotes python para melhorar a experiência, como o formatador de código, linter (busca de erros durante a edição do programa), etc.

Os seguites pacotes são recomendados, e podem ser instalados via pip:

-   `black`: Formatador de código
-   `flake8`: Corrige e sugere modificações para estilo do código fonte
-   `mypy`: Checagem de tipos de variáveis (static type checker)

Para instalar, pode-se usar o comando abaixo, ou instalar todos os pacotes no ambiente virtual, conforme descrito em [Criação de ambiente virtual para instalação de dependências](#criação-de-ambiente-virtual-para-instalação-de-dependências):

```powershell
pip install --upgrade black flake8 mypy
```

# Criação de ambiente virtual para instalação de dependências

É interessante o uso do ambiente virtual para evitar mistuara as dependências entre projetos.

Para criar um ambiente virutal, no terminal, dentro da pasta do projeto, digite:

```powershell
python -m venv venv
```

Uma pasta com nome venv será criada, onde serão armazenados os pacotes instalados via pip.

Para ativar o ambiente virutal, pode-se utilizar a extensão "Python Auto Venv", que detecta o ambiente virutal e já ativa, ou pode-se ativar manualmente. Para a extensão funcionar na primeira vez, deve-se fechar o VSCode e abrir novamente. Para ativação manual, utiliza-se o seguinte comando (dentro da pasta do projeto):

```powershell
venv\Scripts\Activate.ps1
```

Ao ativar o ambiente virutal, no prompt será possível ver a mensagem "(venv)" no começo da linha, indicando que o ambiente virtual está ativado.

Após a ativação do ambiente virtual, todos os pacotes instalados via _pip_ serão instalados dentro do ambiente. Caso queira somente executar o projeto, apenas a primeira instalação é necessária.

Para instalar as dependências necessárias para a execução do projeto, digite:

```powershell
pip install -r requirements.txt --upgrade
```

Para instalar as dependências necessárias para o desenvolvimnento do projeto, digite:

```powershell
pip install -r dev-requirements.txt --upgrade
```

# Outras recomendações

-   Ativar a opção "formatar ao salvar" no VSCode (Arquivo, Preferências, "Format on Save")
-   Escolher o "black" como provedor de formatação (Arquivo, Preferências, "python formatting provider")
-   Aumentar o tamanho máximo da linha para o flake8 (Arquivo, Preferências, "flake8 args": adicionar o parâmetro: `--max-line-length=120`)
-   Citar na mensagem o número da issue fixada, caso exista, ao efetuar um commit ("Fix # - descrição da Issue")
-   Fazer um "git pull" ao iniciar os trabalhos para ter certeza de que está trabalhando com a versão mais atual
-   Quando um commit for uma sugestão ou ainda precisar ser testado melhor, não efetuar a alteração diretamente no "banch master", mas utilizar um "branch" separado

# Configuração de fontes personalizadas no Powershell

Ficará com uma aparência semelhante a esta, facilitando a identificação do branch e da versão do Python utilizada:
![comandline](https://user-images.githubusercontent.com/68362578/128211876-931ef8fb-3c20-47b4-a2ba-ea81d695d491.png)

## Instalar e configurar os pacotes necessários:

Abra o Powershell como admnistrador e digite o seguinte comando:

```powershell
Set-ExecutionPolicy unrestricted
```

Depois, instale os seguintes pacontes (confirme com "Sim" para instalar o NuGet e também para PSGallery, que é tratado como repositório não confiável):

```powershell
Install-Module posh-git -Scope CurrentUser
Install-Module oh-my-posh -Scope CurrentUser
```

Se você estiver usando o PowerShell Core, instale o PSReadline:

```powershell
Install-Module -Name PSReadLine -Scope CurrentUser -Force -SkipPublisherCheck
```

Depois, para editar o profile digite:

```powershell
notepad $PROFILE
```

Após aberto, cole os comandos abaixo e salve o arquivo:

```powershell
Import-Module posh-git
Import-Module oh-my-posh
Set-Theme Paradox
```

## Instalar uma fonte personalizada:

Por exemplo, Fira Code, disponível em <https://github.com/ryanoasis/nerd-fonts/tree/master/patched-fonts/FiraCode>

Após baixar e descompatar a pasta das fontes, acesse a pasta "ttf", clique com o botão direito em cada fonte e clique em "Instalar"

## Configurar a fonte no Powershell

Após instalado, abra o Powershell, clique com o botão direito sobre a barra superior e em "Propriedades", "Fonte" e escolha "Fira Code"

## Definir o Powershell como terminal padrão do VSCode

Digite "Shift+Ctrl+P" e, na barra de pesquisas digite "select default profile" e selecione o Powershell

Para informações adicionais acesse:

-   <https://docs.microsoft.com/pt-br/windows/terminal/tutorials/powerline-setup>
-   <https://www.hanselman.com/blog/how-to-make-a-pretty-prompt-in-windows-terminal-with-powerline-nerd-fonts-cascadia-code-wsl-and-ohmyposh>
