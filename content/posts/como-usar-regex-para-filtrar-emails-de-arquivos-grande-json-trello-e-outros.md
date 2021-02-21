# Como Puxar todos os E-mails de um arquivo grande como da exportação das boards do Trello em JSON ou Outros tipos

Recentemente estava organizando alguns contatos antigos para recriar minha newsletter e como eu tinha um "funil" de vendas no Trello, resolvi exportar os contatos antigos daquela ferramenta que estavam organizados em cartões, como é o padrão comum do Trello.

A única forma de exportação não paga é através da criação de arquivo [JSON](link), entretanto, como eram diversos cartões separados não foi possível organiza-los. Tentei algumas formas de utilização do arquivo através de JavaScript mas como o padrão estava muito desorganizado, este método não se mostrou produtivo.

Recorri ao bom e velho [REGEX](link), expressões regulares, que sempre me auxiliam a manipular e organizar documentos.

## Buscar a Expressão

A primeira coisa a fazer é buscar uma expressão noa internet que corresponda aos emails, depois adapta-la a sua necessidade. Cuidado que a maioria irá fazer uma consulta de um e-mail único, utilizando alguns parâmetros que podem não ser produtivos, como início de linha "^" e fim de linha "$". Também é comum que nos exemplos que você receba não contemplem os endereços nacionais, ficando a expressão limitada ao ".com" o que vai fazer você perder muitos emails.

A expressão que eu achei e já corrigida para o padrão nacional foi foi essa, ela contempla pontos ".", traços "-" e underline "_", endereços ".com" e ".com.br" e ".gov.br": [a-z.A-Z0-9_-]{1,}@[a-zA-Z0-9_]{1,}.[a-zA-Z]{1,}.[a-zA-Z]{1,}.[a-zA-Z]{1,}

## Buscando a Expressão

No aplicativo de texto [Sublime Text](link) e muitos outros aplicativos como [Notepad++](..) é possível escolher a busca por expressões regulares, basta selecionar a opção com o Ctrl + Alt + F. Depois você cola o texto da busca e ele vai atrás de todas as referências.

No meu caso eu consegui 1447 endereços de email.

Depois disso, crie um documento novo vazio e cole a busca, ele já vai adicionar o seu endereço linha a linha.

## Tratamento dos Dados

Antes de terminar a busca, dê uma verificada nos emails coletados, se não houve nenhum ponto, vírgula ou alguma expressão que prejudicou a coleta dos emails, como o .com.br como já mencionado acima, não precisa olhar todas as entradas, basta alguma amostragem.

Outra questão é que vieram muitos dados repetidos e muitos emails de @boards.trello.com que eu não sei exatamente o que seria, então eu adaptei o Regex para buscar estes e-mails e excluí-los da minha lista. Seguindo o mesmo procedimento adicionei o seguint REGEX: ```[a-z.A-Z0-9_-]{1,}@boards.trello.com```

Finalmente eu organizei as entradas por ordem e depois limpei as linhas repetidas-> ```Sublime Text -> Edit Sort Lines -> Edit Permute Lines -> Unique```.


## Resumo Final Dos Resultados

```regex
1447  - [a-z.A-Z0-9_-]{1,}@[a-zA-Z0-9_]{1,}.[a-zA-Z]{1,}.[a-zA-Z]{1,}.[a-zA-Z]{1,}
775   - [a-z.A-Z0-9_-]{1,}@boards.trello.com
206   - Sublime Text -> Edit Sort Lines -> Edit Permute Lines -> Unique
```

Bom, tive apenas 206 resultados positivos depois de filtrar os e-amails, o que foi um pouco decepcionante. De qualquer forma, já são "leads" qualificados e prontos para entrar na newsletter.
