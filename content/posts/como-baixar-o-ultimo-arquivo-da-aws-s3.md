---
title: Como baixar o ultimo arquivo da aws s3
date: "January 01, 2021"
image: ''
imageAlt: Github
---

# Como baixar o ultimo arquivo da aws s3

Para testar a automação do fluxo de documentação estou utilizando a linguagem de programação Ruby na plataforma Rails, ou seja, Ruby on Rails e utilizando os serviços de nuvem da Amazon - AWS, mais espcificamente, para armazenar os arquivos, na AWS S3.

Quando eu estou fazendo o teste dos arquivos o sistema envia os arquivos finalizados lá para a AWS e eu tenho que fazer o login na plataforma, buscar os serviços S3 (a AWS tem uma porrada de serviços) e depois fazer o download do arquivo para verificar se o teste ocorreu bem.

Percebi que isso estava demandando tempo e esforço excessivo e como sempre, decidi automatizar isso, veja como fiz:

# Listar e Baixar todos os arquivos

Achei este link com uma mistura de código de AWS com Linux feito para buscar o último arquivo da S3, eu já tinha o AWS Client configurado, então ficou mais fácil ainda. Na sequência ele usa vários comandos de Linux para salvar uma variável chamada $KEY que é simplesmente o nome do último arquivos, confira:

```shell
$ KEY=`aws s3 ls $BUCKET --recursive | sort | tail -n 1 | awk '{print $4}'`
$ aws s3 cp s3://$BUCKET/$KEY ~/aws_test
```

Não esqueça de substituir o $BUCKET pelo caminho do seu Bucket.

Também deixei pronto a pasta aws_test, para salvar todos os arquivos neste diretório e ficar mais fácil de localizar posteriormente.

# Abrindo o Arquivo

Não basta apenas baixar o arquivo, eu quero, no mesmo comando, abri-lo com meu editor de "docx", atualmente estou usando o WPS Writer. Já descobri que para abri-lo basta digitar `wps` no terminal. Vou utilizar as mesmas técnias do item anterior, usando o sort - tail e awk com o wps.

```shell
$ cd ~/aws_test
$ KEY_F=`ls -alt -A | tail -n 1 | awk '{print $NF}'`

```
`ls -alt -A`: Utilizei essa forma de LS para dar organizado por datas (-alt) e também para tirar o . e o .. que usualmente vêm junto, utilizando o comando (-A), depois utilizei os mesmos comandos do item anterior, exceto o aws que eu utilizei na última coluna ($NF).

Para abrir o arquivo utilizei o comando para abrir o WPS com a minha chave escolhida `wps $KEY_F`.

# Testando

```shell
$ KEY=`aws s3 ls $BUCKET --recursive | sort | tail -n 1 | awk '{print $4}'`
$ aws s3 cp s3://$BUCKET/$KEY ~/aws_test
$ cd ~/aws_test
$ KEY_F=`ls -alt -A | tail -n 1 | awk '{print $NF}'`
$ wps $KEY_F
```

1. A primeira tentativa não deu certo: _Esqueci de alterar o $BUCKET para o meu bucket_.
2. A segunda tentativa também não deu certo: _O código rodou muito rápido e não deu tempo de abrir o arquivo .docx_.
3. Na terceira, adicionei um ```sleep 10``` entre o download e a abertura do arquivo e aí deu certo.

# Colocando tudo em um script no ZSH

Finalmente, arrumei o script, coloquei tudo entre ponto e vírgula ';' que é o padrão do Zsh e depois colei com o comando ```docl``` para me lembrar de documento last e ficou assim:

```shell
docl(){KEY=`aws s3 ls $BUCKET --recursive | sort | tail -n 1 | awk '{print $4}'`; aws s3 cp s3://$BUCKET/$KEY ~/aws_test; sleep 10; cd ~/aws_test; KEY_F=`ls -alt -A | tail -n 1 | awk '{print $NF}'`; wps $KEY_F}
```
