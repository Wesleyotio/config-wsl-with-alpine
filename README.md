# Configurando o WSL2 com Alpine Linux

Neste tutorial usaremos a [Alpine Linux](https://www.alpinelinux.org) dentro do WSL2, caso nunca tenha configurado o wsl2 em sua maquina deixo esse outro tutorial [Configurando o WSL com o Ubuntu 22.04 + ZSH](https://github.com/Wesleyotio/config-wsl-with-ubuntu-zsh) para que tenha por onde começar, aqui vou considerar que já tenha WSL2 funcionando plenamente em sua maquina.


Escolhi essa distro por querer uma distro diferente da minha principal o [Manjaro Linux](https://manjaro.org/download/), que também tenho um [tutorial de instalação](https://github.com/Wesleyotio/mymanjaroconfig#instalação-do-so), e experimentar o famoso [neovim](https://github.com/neovim) como ferramenta de desenvolvimento.

# Índice

- [Instalando o Alpine Linux no WSL2](#instalando-o-alpine-linux)
  - [Instalando o básico](#instalando-o-basico)
  - [Criando um usuário não root](#criando-um-usuario-nao-root)
- [ZSH](#zsh)
  - [Nerds Fonts](#nerds-fonts)
  - [powerlevel10k](#powerlevel10k)
  - [plugins zsh](#plugins-zsh)
    - [zsh-autosuggestions](#zsh-autosuggestions)
    - [zsh-syntax-highlighting](#zsh-syntax-highlighting)
- [Docker](#docker)
  - [Usando allias](#usando-allias)
  

# Instalando o Alpine Linux no WSL2
Importante lembar que por padrão o Alpine Linux não é suportado pelo WSL2, por isso usaremos o de um projeto da [yuk7](https://github.com/yuk7) que criou um modelo simples e fácil de ser seguido ela tem projetos para as distros:
 
 - [ArchWSL](https://github.com/yuk7/ArchWSL)  
 - [AlpineWSL](https://github.com/yuk7/AlpineWSL)

Aqui deixo o link com a [lista das distros](https://wsldl-pg.github.io/docs/Using-wsldl/#distros) que fazem parte teste projeto usando wsl2 diferentes da pase do Linux Ubuntu
