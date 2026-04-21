# Projeto conversão de temperatura

### Sobre o projeto
O projeto conversão de temperatura é um projeto desenvolvido em NodeJS. O projeto tem como objetivo ser um exemplo para a criação de ambiente com containers usando NodeJS.

Etapas para Criar a Imagem:

Criar um Dockerfile:

O que é? Um arquivo de texto que contém todos os comandos que o Docker executará para construir a imagem.
Exemplo:
Dockerfile
FROM node:18-alpine  # Base da imagem (Node.js 18 com Alpine Linux)
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["node", "index.js"]

----
Entendendo o Dockerfile:

1. FROM node:22.6.0:
Significado: Indica que a imagem será criada com base na imagem oficial do Node.js na versão 22.6.0. Essa imagem base já vem com o Node.js pré-instalado e outras ferramentas essenciais.
2. WORKDIR /app:
Significado: Define o diretório de trabalho dentro do container como /app. Todas as próximas instruções serão executadas nesse diretório.
3. COPY package.json ./:*
Significado: Copia os arquivos package.json e package-lock.json (se existir) do seu diretório local para o diretório /app dentro do container. Esses arquivos contêm as dependências do seu projeto.
4. RUN npm install:
Significado: Executa o comando npm install dentro do container para instalar todas as dependências listadas no package.json.
5. COPY . .:
Significado: Copia todos os outros arquivos e diretórios do seu projeto local para o diretório /app do container. Isso inclui seu código fonte e outros recursos necessários.
6. EXPOSE 3000:
Significado: Indica que a porta 3000 dentro do container estará disponível para conexão externa. No entanto, isso não significa que a porta já esteja mapeada para uma porta específica na sua máquina.
7. CMD ["node", "server.js"]:
Significado: Define o comando padrão que será executado quando um container for iniciado a partir dessa imagem. Neste caso, o comando node server.js inicia seu servidor Node.js.


Abra o terminal e navegue até o diretório onde está o Dockerfile.
Execute o comando:
Bash
docker build -t minha-imagem-node .
Use o código com cuidado.

-t: Define um nome para a imagem (substitua por um nome mais descritivo).
.: Indica que o Dockerfile está no diretório atual.
Verificando a Imagem:

Listar as imagens:
Bash
docker images
Use o código com cuidado.

Inspecionar uma imagem:
Bash
docker inspect minha-imagem-node
Use o código com cuidado.

Executando a Imagem em um Container:

Criar um container interativo:
Bash
docker run -it --rm -p 3000:3000 minha-imagem-node
Use o código com cuidado.

-it: Permite interagir com o container.
--rm: Remove o container quando ele é encerrado.
-p 3000:3000: Mapeia a porta 3000 do container para a porta 3000 da sua máquina.
Testando a Aplicação:

Abra um navegador e acesse http://localhost:3000.
Se tudo estiver configurado corretamente, você verá a saída da sua aplicação Node.js.
Dicas Adicionais:

Multi-stage builds: Para criar imagens mais leves, você pode utilizar multi-stage builds para separar as etapas de construção e execução.
.dockerignore: Crie um arquivo .dockerignore para especificar quais arquivos e diretórios não devem ser copiados para a imagem.
Volumes: Utilize volumes para persistir dados entre execuções do container.
Docker Compose: Para gerenciar múltiplos containers, utilize o Docker Compose.
Considerações:

Ambiente de desenvolvimento: Certifique-se de que o ambiente de desenvolvimento (Node.js, npm) esteja configurado corretamente antes de criar a imagem.
Dependências: Verifique se todas as dependências do seu projeto estão listadas no package.json.
Porta: A porta exposta (3000 no exemplo) pode ser alterada de acordo com a sua necessidade.
Nome da imagem: Escolha um nome claro e conciso para a sua imagem.
Recursos Adicionais:

Documentação oficial do Docker: https://docs.docker.com/
Tutorial DigitalOcean: [URL inválido removido]
Com este guia, você estará pronto para criar e gerenciar suas aplicações Node.js utilizando Docker.

## instalação do Helm:

Entendendo os componentes:
Helm: É o "Instalador de Aplicativos" do Kubernetes. Sem ele, você teria que criar dezenas de arquivos YAML manualmente.

Prometheus: O banco de dados que armazena as métricas (o "cérebro" do monitoramento).

Grafana: A interface visual que transforma os dados do Prometheus em gráficos bonitos.

Passo a Passo Detalhado
1. Instalação do Helm
O comando baixa o script oficial de instalação e o executa no seu Ubuntu.

Bash
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
2. Adição dos Repositórios
O Helm precisa saber "onde buscar" os pacotes do Prometheus.

Bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
3. Instalação da Stack (Kube-Prometheus-Stack)
Este comando instala o Prometheus e o Grafana já configurados para conversar entre si.

Bash
kubectl create namespace monitoring
helm install monitoramento prometheus-community/kube-prometheus-stack -n monitoring
4. Acesso ao Grafana
Por padrão, o serviço é do tipo ClusterIP. Usamos o port-forward para mapear a porta do cluster para o seu navegador no Windows.

Bash
kubectl port-forward -n monitoring service/monitoramento-grafana 3000:80
Conteúdo para o seu arquivo README.md
Abaixo está o texto formatado. Você pode criar um arquivo chamado MONITORAMENTO.md no seu repositório projeto-devops-fase-2.

# 📊 Monitoramento de Cluster Kubernetes (Prometheus & Grafana)

Este guia descreve o processo de instalação e configuração da stack de monitoramento utilizando Helm em um ambiente Kubernetes (Minikube/WSL2).

## 🛠️ Pré-requisitos
- Cluster Kubernetes rodando (Minikube).
- `kubectl` configurado.

## 🚀 Passo a Passo

### 1. Instalar o Helm
O Helm é o gerenciador de pacotes para Kubernetes. Ele facilita a implantação de aplicações complexas.
```bash
curl [https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3](https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3) | bash
````

2. Configurar Repositórios do Helm
Adicionamos o repositório da comunidade que mantém as imagens oficiais do Prometheus.
# Adiciona o repositório
helm repo add prometheus-community [https://prometheus-community.github.io/helm-charts](https://prometheus-community.github.io/helm-charts)

# Atualiza a lista de pacotes locais
helm repo update

3. Instalar Kube-Prometheus-Stack
Utilizamos um "Chart" completo que já instala Prometheus, Grafana e Alertmanager em um único comando.

# Criar um namespace isolado para organização
kubectl create namespace monitoring

# Instalar a stack
helm install monitoramento prometheus-community/kube-prometheus-stack -n monitoring

4. Acessar o Painel do Grafana
O Grafana é instalado com o tipo de serviço ClusterIP. Para acessar do navegador local:

Obter a senha do usuário admin:
kubectl get secret --namespace monitoring monitoramento-grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo

Criar o Túnel de Acesso (Port-Forward):

Bash
kubectl port-forward -n monitoring service/monitoramento-grafana 3000:80

🗑️ Como Remover a Stack
Para limpar o ambiente e remover todos os recursos instalados:
# Desinstalar via Helm
helm uninstall monitoramento -n monitoring

# Remover o namespace
kubectl delete namespace monitoring

# Remover os CRDs (Custom Resource Definitions)
- kubectl delete crd alertmanagerconfigs.monitoring.coreos.com alertmanagers.monitoring.coreos.com podmonitors.monitoring.coreos.com probes.monitoring.coreos.com prometheuses.monitoring.coreos.com prometheusrules.monitoring.coreos.com servicemonitors.monitoring.coreos.com
