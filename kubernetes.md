Se a aplicação falhar?
No kubernetes consigo contornar o problema com o self-healing

Se precisar escalar?
Temos a escala horizontal

Se tiver muitos serviços?
Service Discovery, DNS dentro do cluster para aplicações internas

E o loadbalancing?
Kubernetes possui esse balanceador, por padrão

Se houver dados sensiveis
Kubernetes possui as secrets

---

Arquitetura: Control Plane
Control Plane e Data Plane

Control Plane é onde tem os componentes criticos do cluster, todo gerenciamento dele: Cloud Controller,Controller Manager, Api Server, etcd, Scheduler
As aplicações/serviços rodam no Data Plane 

Cloud gerencia o control plane e os usuarios cuidam do data plane (PAAS)

API Server: expões essa API do Kubernetes, GET, POST, etc..., um pod/container, dentro do k8s que vai rodar na porta 6443 e escutando as requisições de API, tudo passa por ele, quando quisermos interagir com o cluster. Os nodes tbm falam com o API Server.

etcd: Componente mais critico do cluster, banco de dados de chave e valor, base de dados do cluster k8s, quando criamos um pod, salva dentro dele, se ele der problema, pausa o cluster inteiro, tudo para de funcionar, armazena tudo que é criado no cluster. (Da pra fazer snapshot). Pode rodar como um serviço(systemd) ou um pod.

Scheduler: Ele olha containers que ainda não tem um node, e encontra um node pra eles, sabe em qual vm esta sendo menos utilizada para colocar o POD. (balanceador?)

Controller Manager: Aplicação que vai rodar e ler alguma regra que criamos, e quando algo acontecer, ele entra em ação, quando criamos um cron-job(pesquisar), ele cria um job e cria um pod, tem um controller dentro do cluster que quando criar um job, precisa criar um POD. Pequenos controllers que tomam ação automatica. 

Cloud Controller: Interagir com as APIs de cloud, cria um ALB para expor um loadbalancer, programado dentro do Cloud Controller (AWS, AZURE, GOOGLE CLOUD, etc...)

---

Data Plane é as Vms que vão rodar as aplicações, tudo vai ser distribuidos entre elas. 

Kubelet: O capitão do navio, a VM vai ser o navio, um agente do cluster, que roda como um serviço e se comunica com o API Server, ele cria o pod e o container e garante que ele continue rodando. (pesquisar: probess e health checks). Faz o bootstrap do node

Kube-Proxy: Roda em todos os nodes, a ideia é gerenciar as regras de iptables e ipvs, e garantir que consiga usar os services do k8s, para usar esse service, DNS/balanceador de carga, precisa gerenciar regras de IPTables das vms onde as aplicações rodam. Temos a CNA tambem (pesquisar)

---

Precisamos de um container runtime:
docker (não mais utilizado)
ContainerD (https://github.com/containerd/containerd/blob/main/docs/getting-started.md / https://docs.docker.com/engine/install/ubuntu/)

Kubeadm: ferramenta de linha de comando para criar o cluster
Kubelet: Componente que vai ser instalado nas maquinas e vai falar com o API Server
kubectl: ferramenta de nivel de cliente, que usamos pra bater no API Server e comunicar com o cluster

