# BetBlocker-DNS

Um Serviço de DNS para Bloqueio de Sites de Apostas

> [Você pode ler mais sobre aqui](https://www.andrealtoe.me/blog/bet-block-dns)

## Utilização

> Atualmente não há servidor disponibilizado para este projeto, sinta-se livre para hospedar um.

~Para utilizar o serviço disponibilizado, configure o DNS primário de sua máquina para `174.138.124.37`, ou, configur o provedor de DNS privado `betblock.andrealtoe.me`~

> Não se esqueça de configurar um DNS secundário de sua preferência, para o caso de indisponibilidade do serviço primário.

## Descrição

Este serviço fornece um bloqueador de sites de apostas operando em nível de DNS, com o objetivo de auxiliar no bloqueio de acessos a sites de apostas. Pode ser instalado em praticamente qualquer dispositivo conectado à internet através das configurações de rede.

## Funcionamento

O serviço é implementado utilizando [Unbound](https://www.nlnetlabs.nl/projects/unbound/about/). A configuração atual funciona como um proxy para o DNS da [Cloudflare](https://www.cloudflare.com/pt-br/learning/dns/what-is-1.1.1.1/), bloqueando os domínios listados no arquivo `blocks.txt`. Este arquivo é convertido em um arquivo de configuração do Unbound no momento da inicialização do serviço, por meio de um script em Python.

## Instruções de Execução

**Pré-requisitos**

- Docker
- Git

---

Para executar este serviço como um resolvedor de DNS, siga os seguintes passos:

1. Clone o repositório via Git:

```sh
git clone https://github.com/faakit/bet-blocker-dns.git
```

2. Construa a imagem Docker:

```sh
docker build -t unbound-dns .
```

3. Verifique se a porta 53 está sendo utilizada:

```sh
sudo ss -tuln | grep :53
```

4. Caso a porta 53 esteja em uso, interrompa o serviço correspondente (exemplo com systemd-resolved):

```sh
sudo systemctl stop systemd-resolved
```

5. Execute o container do Unbound
   5.1 Caso você não possua um domínio:

```sh
docker run -d --name unbound-dns -p 53:53/udp -p 53:53/tcp unbound-dns
```

5.2 Caso você possua um domínio e queira utilizar o hostname

```sh
docker run -d --name unbound-dns -p 53:53/udp -p 53:53/tcp -p 853:853/udp -p 853:853/tcp -p 80:80 -e DOMAIN=<Seu dominio aqui> -e EMAIL=<Seu email aqui> unbound-dns
```

## Próximos Passos

Ainda há muitos aspectos a serem desenvolvidos. A segurança é um dos principais desafios a serem enfrentados.

## Créditos

Grande parte do trabalho realizado foi inspirado pelos seguintes repositórios:

- [Unbound Docker - Configuração do Unbound](https://github.com/MatthewVance/unbound-docker)
- [Bet-blocker - Fonte inicial da lista de bloqueio](https://github.com/bet-blocker/bet-blocker)
