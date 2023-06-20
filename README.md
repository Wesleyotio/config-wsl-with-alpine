# Configurando o WSL2 com Alpine Linux

Neste tutorial usaremos a [Alpine Linux](https://www.alpinelinux.org) dentro do WSL2, caso nunca tenha configurado o wsl2 em sua maquina deixo esse outro tutorial [Configurando o WSL com o Ubuntu 22.04 + ZSH](https://github.com/Wesleyotio/config-wsl-with-ubuntu-zsh) para que tenha por onde começar, aqui vou considerar que já tenha WSL2 funcionando plenamente em sua maquina.


Escolhi essa distro por querer uma distro diferente da minha principal o [Manjaro Linux](https://manjaro.org/download/), que também tenho um [tutorial de instalação](https://github.com/Wesleyotio/mymanjaroconfig#instalação-do-so) e experimentar o famoso [neovim](https://github.com/neovim) como ferramenta de desenvolvimento.

# Índice

- [Configurando o WSL2 com Alpine Linux](#configurando-o-wsl2-com-alpine-linux)
- [Índice](#índice)
- [Instalando o Alpine Linux no WSL2](#instalando-o-alpine-linux-no-wsl2)
  - [Instalando o básico](#instalando-o-básico)
  - [Criando um usuário não root](#criando-um-usuário-não-root)
- [Nerd Fonts](#nerd-fonts)
  - [powerlevel10k](#powerlevel10k)
  - [plugins zsh](#plugins-zsh)
    - [zsh-autosuggestions](#zsh-autosuggestions)
    - [zsh-syntax-highlighting](#zsh-syntax-highlighting)
- [Docker](#docker)
  - [Iniciando o docker](#iniciando-o-docker)
    - [ALIAS](#alias)
- [Tmux](#tmux)
  - [Instalando o Tmux](#instalando-o-tmux)
  - [Personalizando o Tmux](#personalizando-o-tmux)
  

# Instalando o Alpine Linux no WSL2
Importante lembar que por padrão o Alpine Linux não é suportado pelo WSL2, por isso usaremos o de um projeto da [yuk7](https://github.com/yuk7) que criou um modelo simples e fácil de ser seguido ela tem projetos para as distros:
 
 - [ArchWSL](https://github.com/yuk7/ArchWSL)  
 - [AlpineWSL](https://github.com/yuk7/AlpineWSL)

Aqui deixo o link com a [lista das distros](https://wsldl-pg.github.io/docs/Using-wsldl/#distros) que fazem parte deste projeto usando wsl2 diferentes da base do Linux Ubuntu.

Seguindo as recomendações do link basta baixar o arquivo descompactar onde achar melhor, eu fiz no diretório raiz. Agora dentro da pasta descompactada execute o terminal como Administrador do sistema e execute o seguinte comando

```sh
.\ Alpine.exe
```
Feito isso o Alpine Linux será instalado no WSL2, agora feche o terminal e abrindo novamente você verá o sistema instalado, quando tiver feito isso estará logado como root do sistema e daqui a pouco resolvemos isso.
## Instalando o básico

Uma coisa interesante sobre o alpine  é que ele vem sem nada praticamente, precisamos instalar tudo e vamos ao comando pra isso, mas antes vou deixar a [lista de comandos do alpine](https://pt.linux-console.net/?p=2606#gsc.tab=0) para que possa entender como manipular o sistema.

```sh
apk add zsh curl wget git htop nano doas neofetch
```
Nesse pacote de instalações estamos colocando o zsh que será nosso shell padrão do sistema, mas antes disso vamos criar nosso usuário nào root do sistema. 

## Criando um usuário não root

Precisamos criar o usuário com sua respectiva senha, vamos precisar conceder para ele o uso de comandos de root e para isso temos que instalar e configurar sudo e fazemos isso pelo comando.  

```sh
apk add sudo 
NEWUSER='<username>' 
adduser -g "${NEWUSER}" $NEWUSER 
echo "$NEWUSER ALL=(ALL) ALL" > /etc/sudoers.d/$NEWUSER && chmod 0440 /etc/sudoers.d/ $NEWUSER
```
agora adicionamos nosso usuário ao grupo wheel pelo comando

```sh
echo '%wheel ALL=(ALL) ALL' > /etc/sudoers.d/wheel 
adduser NEWUSER wheel
```
Legal, mas ainda não estamos usando o zsh e toda vez que entramos no sistema estamos como root. Para corrigir isso precisamos primeiro editar nosso arquivo ***passwd***

```sh
nano etc/passwd  
```
do resultado apresentado só é importante a primeira e ultima linha como exemplo:

```sh
root:x:0:0:root:/root:/bin/ash
.
.
username:x:1000:1000:wesleyotio:/home/username:/bin/ash
```

agora só mudamos de /ash pra /zsh e salvamos o arquivo, feche o terminal e abra novamente. O zsh estará em pleno funcionamento. Agora execute o comando:

```sh
whoami
```
se sua resposta for ***root*** é pelo fato de que o alpine ainda não está configurado para iniciar a partir de seu usuário criado, para corrigir isso feche o terminal e execute novamente a pasta descompactada do alpine linux como administrador e nela execute o seguinte comando passando o nome do seu usuário

```sh
.\ Alpine.exe config --default-user <username>
```
agora quando iniciamos o sistema o comando **whoami** deve apresentar o nome de seu usuário. 

# Nerd Fonts

Antes de deixar nosso terminal bonito como fizemos no [Configurando o WSL com o Ubuntu 22.04 + ZSH](https://github.com/Wesleyotio/config-wsl-with-ubuntu-zsh) vamos instalar uma fonte do tipo [nerd fonts](https://www.nerdfonts.com), aqui deixo os links da que tenho usado para você baixar e instalar em seu sistema.
- [MesloLGS NF Regular.ttf](https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Regular.ttf) 
 - [MesloLGS NF Bold.ttf](https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Bold.ttf) 
 - [MesloLGS NF Italic.ttf](https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Italic.ttf) 
 - [MesloLGS NF Bold Italic.ttf](https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Bold%20Italic.ttf)

Feito isso agora abra novamente o terminal e em configurações -> padrões -> fonts selecione _MesloLGS NF_ .
## powerlevel10k

Tudo isso foi necessário para agora instalar o [powerlevel10k](https://github.com/romkatv/powerlevel10k) que é um tema para o shell ZSH e deixar nosso terminal mais bonito e performático quando somado aos plugins. Eu sugiro que use a instalação manual que tem certeza de funcionar.
Fazendo uso da instalação manual usamos o comando.

```sh
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ~/powerlevel10k
echo 'source ~/powerlevel10k/powerlevel10k.zsh-theme' >>~/.zshrc
```
Feche o terminal e abra novamente, assim você inicia a configuração do tema, siga as instruções e customize seu terminal da forma que mais te agrada. Caso queira mudar o estilo apresentado basta usar o comando.

```sh
  p10k configure
```
e fazer o processo novamente.

## plugins zsh

Bonito o terminal já está, mas precisamos deixar performático e para isso vamos usar os seguintes plugins:
  - zsh-autosuggestions
  - zsh-syntax-highlighting

### zsh-autosuggestions
Esse plugin é responsável por usar nosso histórico de comandos para sugerir qual proximo comando você deseja usar e autocompleta o comado quando usamos TAB ou seta direita. Para instalar executamos.

```sh
 git clone https://github.com/zsh-users/zsh-autosuggestions ~/.zsh/zsh-autosuggestions
```
Agora adicionamos os caminho do plugin em nosso arquivo `/.zshrc`

```sh
source ~/.zsh/zsh-autosuggestions/zsh-autosuggestions.zsh
```
Feche e abar novamente o terminal para usar o plugin

### zsh-syntax-highlighting
Esse plugin é responsável dar realce/cores nos comandos, diretórios e arquivos no sistema no terminal.
```sh
sudo git clone https://github.com/zsh-users/zsh-syntax-highlighting.git
echo "source ${(q-)PWD}/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh" >> ${ZDOTDIR:-$HOME}/.zshrc
```
Para ativar o plugin em nosso Shell executamos o comando.
```sh
 source ./zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
```

# Docker
Agora vamos pra instalação que deu mais trabalho para realizar, vale lembrar que estou usando WSL2 com docker nativo rodando na própria distro linux. Então caso seja usuário do **[docker desktop](https://www.docker.com/products/docker-desktop/)** acho que este passo não seja necessário. Esses dois artigos aqui me ajudaram muito nessa configuração.
  - [Install Docker on Windows (WSL) without Docker Desktop](https://dev.to/bowmanjd/install-docker-on-windows-wsl-without-docker-desktop-34m9)
  - [DOCKER IN WSL2 ALPINE WITHOUT DOCKER DESKTOP-DOCKER](https://www.appsloveworld.com/docker/100/87/docker-in-wsl2-alpine-without-docker-desktop)

Inicialmente atualizamos o sistema pelo comando

```sh
sudo apk upgrade -U
```

agora instalamos o docker

```sh
sudo apk add docker
```
feito isso vamos adicionar nosso usuário ao grupo docker para não ser necessário usar **sudo docker** toda vez que quisermos usa-lo

```sh
sudo addgroup $USER docker  
```
precisamos definir um ID para nosso grupo docker que deve ser um valor acima de 1000 e menor que 65534, para ver quais ids já estão atribuídos a grupos use o comando.

```sh
getent group | cut -d: -f3 | grep -E '^[0-9]{4}' | sort -g
```
escolhido o ID agora podemos atribuir ao grupo docker de duas maneiras usando o comando.

```sh
sudo sed -i -e 's/^\(docker:x\):[^:]\+/\1:<ID>/' /etc/group
```
ou caso tenha instalado o shadow

```sh
sudo apk add shadow
sudo groupmod -g <ID> docker
```
Pronto temos o docker instalado em sua distro
## Iniciando o docker

No alpine Linux foi bem tenso até encontrar uma configuração que fizesse isso funcionar corretamente, foi aí que encontrei o comando que adiciona o serviço do docker a execução padrão

```sh
rc-update add docker default
```
Agora para iniciar o docker temos que fazer o comando 

```sh
sudo openrc default
```
para finalizar a execução do docker usamos o mesmo comando do ubuntu

```sh
 sudo service docker stop
```
se desejar iniciar novamente usamos o mesmo comando do ubuntu também

```sh
 sudo service docker start
```
o problema é que para executar o serviço pela primeira vez somos obrigados a usar o **sudo openrc default** a solução para esse impasse é usar ALIAS

### ALIAS
Alias são  apelidos que damos aos nossos comando de terminal com objetivo de torna-los mais simples e para isso precisamos editar o nosso arquivo `.zshrc` ou criar um arquivo `.zshrc_aliases`  em separado somente com seus alias. Vou optar pela segunda opção pois em uma nova instalação do sistema teremos somente que referenciar o arquivo novamente.

Iniciemos editando nosso arquivo `.zshrc`
```sh
sudo  nano ~/.zshrc
```
nesse arquivo vamos adicionar o caminho, caso exista, para nosso arquivo de aliases
```sh
if [ -f ~/.zshrc_aliases ]; then
      . ~/.zshrc_aliases
fi
```
agora vamos criar nosso aquivo de aliases e adicionar nossos apelidos

```sh
sudo nano ~/.zshrc_aliases
```
Meu arquivo ficou assim, defini **docker-init** para toda vez que iniciar os sistema e precisar ligar o docker
```sh
#Config for Docker
alias docker-init='sudo openrc default'
alias docker-start='sudo service docker start'
alias docker-stop='sudo service docker stop'
```
agora para garantir o funcionamento usamos o comando **source** para recarregar nossos arquivos
```sh
source ~/.zshrc_aliases
source ~/.zshrc
```
para realizar a testagem basta executar 
```
docker-init
docker run --rm hello-world
```
Se sua saída obtiver o **Hello from dorcker!** sua configuração está completa. Parabéns!!

# Tmux

O tmux é um [multiplexador de terminal](https://opensource.com/article/21/5/linux-terminal-multiplexer) que permite que por meio de uma única entrada ( você com seu terminal aberto) enviar comandos para qualquer uma de suas saídas( um grupo de servidores ou  partes diferentes de um mesmo sistema).

Para deixar mais claro, imagine um sistema que só pode ser acessado via terminal. Nesse sistema é necessário fazer a edição de um arquivo de configuração, mas é preciso também monitorar os logs que estão sendo gerados naquele mesmo instante.

Para esta situação precisaríamos que nosso terminal fosse capaz de enviar comandos para duas saídas distintas, nesse momento que o  **multiplexador de terminal** entra pois ele permite criar varias conexões/janelas de acesso em um mesmo terminal tornando essa tarefa possível.

## Instalando o Tmux

Simplesmente usamos o comando 

```sh
sudo apk add tmux
```
e pronto o tmux está pronto para uso e deixo neste link a [lista de comandos](https://tmuxcheatsheet.com/) dessa ferramenta incrível.

## Personalizando o Tmux

Como você ja deve ter percebido a ferramenta é feia que dói, além de ter alguns comandos bem difíceis de serem executados, a boa noticia é que podemos personalizar a interface como os comandos da ferramenta, neste link deixo uma [lista personalizações](https://www.trackawesomelist.com/rothgar/awesome-tmux/readme/#themes) possíveis para torna-la mais agradável.

Inicialmente vamos instalar o [TMP](https://github.com/tmux-plugins/tpm) que é um gerenciador de plugins do Tmux para isso executamos o comando

```sh
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
```

em seguida crie o arquivo de configuração do tmux o `tmux.conf` usando comando

```sh
sudo nano ~/.tmux.conf
```
escreva a seguinte configuração

```
# List of plugins
set -g @plugin 'tmux-plugins/tpm'
set -g @plugin 'tmux-plugins/tmux-sensible'

# Initialize TMUX plugin manager (keep this line at the very bottom of tmux.conf)
run '~/.tmux/plugins/tpm/tpm'
```
salve o arquivo e em seguida execute o comando 

```sh
source ~/.tmux.conf
```
agora inicie o tmux usando 

```sh
tmux new
```
Se tudo estiver saindo como o esperado nada deve ter acontecido, isso acontece pelo fato de que o arquivo está pronto, mas não iniciamos a instalação ou atualização do plugin.

Para isso executamos o prefixo ou leader do tmux que por padrão é o `Ctrl B` + **I**( isso mesmo tem que ser `i` maiúsculo ) ou   `Ctrl B` + **U** para atualização dos plugins.

Sei que é bem chato, mas isso só será feito apenas para podermos adicionar novos plugins ao tmux. Minha sugestão é que olhe a lista de plugins disponíveis assim como os mapeamentos de teclas possíveis e deixe com sua cara, mas caso queria seguir minha configuração deixo aqui registrado como ela se encontra no momento.

```sh
# Set colors terminal overrides
set-option -sa terminal-overrides ",xterm*:Tc"

# Enable mouse mode
set -g mouse on

# Start counting pane and window number at 1
set -g base-index 1
setw -g pane-base-index 1

# Use xclip to copy and paste with the system clipboard
bind C-c run "tmux save-buffer - | xclip -i -sel clip"
bind C-v run "tmux set-buffer $(xclip -o -sel clip); tmux paste-buffer"

# count again windows when a window is close
set -g renumber-windows on

# increase history limit
set-option -g history-limit 10000

# For create split on horizontal or vertical
bind | split-window -hc "#{pane_current_path}"
bind - split-window -vc "#{pane_current_path}"

# change posision between windows
bind -r "<" swap-window -d -t -1
bind -r ">" swap-window -d -t +1

# create windom the same directory
bind c new-window -c "#{pane_current_path}"

# List of plugins
set -g @plugin 'tmux-plugins/tpm'
set -g @plugin 'tmux-plugins/tmux-sensible'
set -g @plugin "janoamaral/tokyo-night-tmux"


# Initialize TMUX plugin manager (keep this line at the very bottom of tmux.conf)
run  '~/.tmux/plugins/tpm/tpm'
```
