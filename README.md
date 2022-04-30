# Dicas para trabalhar com docker

docker run -it IMAGEM COMANDO
//executa a imagem e entra no modo //interativo

docker start NOME_CONTAINER
//starta o container

 docker run -it --rm ubuntu bash
//roda a imagem e quando o processo morrer, já remove do historico

docker run -d nginx
//desacopla o processo do container do terminar, ou seja, nesse caso não vai ficar com o terminal travado

 docker run -d --name container_nginx nginx
//o comando --name é para dar nome ao container

docker exec container_nginx ls
// docker exec é para executar um comando dentro do container, nesse caso eu estou executando o comando ls dentro do container container_nginx

docker exec -it container_gnix bash
//aqui eu estou executando o bash em modo iterativo dentro do container container_nginx

Em uma imagem nova do ubuntu provavelmente não vai existir o vim, então precisamos baixar, para isso basta usar o apt-get install vim, porém, antes precisamos rodar o apt-get update

Quando damos o comando ->  vim nome_arquivo, é aberto o arquivo em modo leitura, para conseguir editar é necessario digitar "i" e para salvar precisamos sair do modo de inserção 
apertando ESC, para salvar basta digitar ":w", para sair do vim basta digitar ":q"
sair sem salvar ":q!"
sair e salvar ":wq"


docker run -d --name container_nginx -p 8080:80 -v ~/html/:/usr/share/nginx/html  nginx
// usando -v conseguimos criar um volume do TIPO BIND(não é um volume de verdade é somente um bind de uma pasta para dentro do container) da nossa pasta para o container, esse caso eu estou apontando minha pasta html para a pasta html dentro do container, e toda alteração feita nos arquivos dentro dessa pasta permaneceram mesmo se o container morrer.

 echo $(pwd)
// esse comando mostra a pasta atual em que você está, então podemos usa-lo junto com o comando acima. Ex: docker run -d --name container_nginx -p 8080:80 -v "$(pwd)"/:/usr/share/nginx/html  nginx

Vimos que utilizamos o -v nas linhas acima, porém hoje em dia o mais recomendado é utilizar o --mount. EX: --mount type=bind,source="$(pwd)",target=/usr/share/nginx/html

comando completo: docker run -d --name container_nginx -p 8080:80  --mount type=bind,source="$(pwd)",target=/usr/share/nginx/html nginx

uma das diferença entre -v e --mount é que a primeira cria a pasta mesmo que ela não exista no source, já o segundo comando tem a validação e dá erro caso a pasta não existe no source.

docker rmi <NAME OU ID_IMAGE>
// remover uma imagem

Para criar um volume:

docker volume ls -> lista todos os volumes que temos
docker volume create meuvolume -> comando para criar um volume 
docker volume inspect meuvolume -> para inspecionar um volume
criando um container com a imagem do nginx usando o volume-> docker run -d --name nginx1 --mount type=volume,source=meuvolume,target=/app nginx

Dessa forma também conseguimos utilizar o mesmo volume para diferente containers. EX:
docker run -d --name nginx1 --mount type=volume,source=meuvolume,target=/app nginx
docker run -d --name nginx2 --mount type=volume,source=meuvolume,target=/app nginx

Nesse caso os dois containers está apontando para o volume chamado de meuvolume e compartilham tudo o que tem dentro desse volume, então se entrarmos dentro da pasta app em qualquer um dos container e criarmos um arquivo, automaticamente os dois containers terão acesso.

Utilizando o -v também conseguimos utilizar o volume, EX:
docker run -d --name nginx3 -v meuvolume:/app nginx



