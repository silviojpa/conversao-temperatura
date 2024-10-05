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

Entendendo o Dockerfile:

FROM node:18-alpine: Define a imagem base.
WORKDIR /app: Define o diretório de trabalho dentro do container.
COPY package*.json ./: Copia os arquivos package.json e package-lock.json para o diretório de trabalho.
RUN npm install: Instala as dependências do projeto.
COPY . .: Copia o restante do projeto para o container.
EXPOSE 3000: Indica que a porta 3000 do container estará disponível para conexões externas.
CMD ["node", "index.js"]: Define o comando a ser executado quando o container iniciar.
Construir a Imagem:

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
