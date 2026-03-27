O [Syncthing](https://syncthing.net/) é um projeto open source que permite a sincronização de arquivos em diferentes dispositivos.

### Como rodar o Syncthing.
 Seguir os passos abaixo para utilizar o repositório corretamente.
 - `mkdir -p ~/syncthing/{config,data} && cd ~/syncthing` - para criar as pastas que serão utilizadas nos volumes do docker dentro de `docker-compose.yml`.
 - `docker compose --project-name syncthing up -d` - para subir o serviço do syncthing configurado  no `docker-compose.yml`.

> Antes de seguir com a utilização do Syncthing na VPS, configure o firewall para liberar a porta 22000 para o TCP E UDP.
> ```bash
> # Libera apenas a porta de sync (8384 fica bloqueada — acesso só via SSH tunnel)
> ufw allow 22000/tcp
> ufw allow 22000/udp
> ufw reload 
> ```

### Configurando o Syncthing (via Web UI)
#### Como acessar a interface web 
 Ao subir esse serviço em qualquer computador a porta `:8384` fica exposta localmente  na máquina para o acesso a interface web, mas no caso da VPS como essa porta está disponível somente localmente então para acessar é preciso de algum computador para que através dele consigamos entrar no painel do Syncthing da VPS, para isso precisamos rodar o comando ssh com a flag `-L`, que basicamente vai nos permitir acessar a porta 8384 da VPS localmente - *ao usar este comando utilize uma porta local que não seja a `:8384` (ex: `:8385`) para que não dê conflito com o Syncthing que estará rodando localmente*.
 
#### Desabilitar o discovery global (opcional, mais privado)
 Seguir caminho abaixo para realizar configuração:
 `Actions → Settings → Connections → desmarcar "Global Discovery"`
 
#### Pegar o Device ID da VPS
 Vá em `Actions → Settings → Connections → desmarcar "Global Discovery"` - copie esse ID, vai precisar em cada dispositivo para adicionar um dispositivo remoto.

### Adicionando pastas
Os dados que que serão sincronizados entre os dispositivos estarão dentro do diretório `data`, para criar uma nova pasta a ser sincronizada, faça isso através da interface web e especifique o caminho desta maneira: `var/syncthing/[pasta_a_ser_sincronizada]`, este é o caminho dentro do container que será refletido no diretório `data` localmente.
 
 As pastas de sincronização são adicionadas via interface mas os arquivos e subpastas são feitos diretamente no sistema de diretórios do sistema. Para sincronizar a nova pasta adicionada clique na pasta em si depois vá em `editar -> compartilhamento` e então selecione os dispositivos que você quer que aquela pasta esteja sincronizada.
