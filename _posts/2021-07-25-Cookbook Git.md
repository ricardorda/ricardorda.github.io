---
layout: post
title: "GIT"
date: 2021-07-25
tags: [git]
categories: [git]

---
# GIT

## **Fontes de estudo**

- <https://git-scm.com/book/pt-br/v2>
- <https://www.youtube.com/watch?v=0MVXlGQe9nU>
- <https://ohshitgit.com/pt_BR>

## **Comandos Úteis**

### **Debug**

\
Comandos para auxiliar no debug.

Visualizar os últimos commits em uma linha:

``` bash
 git log -QtdCommits --oneline
```

\
Visualizar últimas alterações no HEAD:

``` bash
 git reflog -QtdRegistros ou git log -g -QtdRegistros
```

\
Encontrar o commit que gerou um bug:

``` bash
 git bisect ...
```

_Bisect realiza uma busca binária procurando o commit que iniciou o problema. Se informar um script que detecta o problema a identificação é automática, senão usuário precisa testar a cada fase._  

\
Visualizar diferenças nos últimos commits:

``` bash
git log -p --QtdCommits
```

\
Visualizar detalhes de um commit:

``` bash
git show [codigo comit]
```

\
Criar uma branch para um commit:

``` bash
git checkout -b NomeBranch IdCommit
```

\
Pesquisar termo nos arquivos atuais do git:

``` bash
git grep -n TermoPesquisado
```

\
Visualizar quais commits afetaram um trecho do código:

``` bash
git blame -L linhaInicial, linhaFinal -C nomeArquivo
```

\
Encontrar no histórico os commits que modificaram o termo pesquisado:

``` bash
git log -S ... ou git log -L ...
```

\
Visualizar diferenças entre branches:

``` bash
git log branch1..branch2
git log branch1 --not branch2
git log branch1 ^branch2
git log branch1...branch2 (mostra tudo que está de diferente nos 2 sentidos)
```

\
Visualizar as branches que foram ou não mergeadas ao branch atual:

``` bash
git branch --merged OU git branch --no-merged
```

### **Branch e Merge**

\
Adicionando um repositório remote:

``` bash
git remote add nome-remote url-remote
```

\
Fazendo push para o remote de uma branch local:

``` bash
git push -u nome-remote NomeBranch
git push nome-remote nomeBranchLocal:NovoNomeBranchRemote
```

\
Visualizando informações do remote :

``` bash
git ls-remote ou git remote show [nome-remote]
```

\
Fazendo merge do branch remoto com o local:

``` bash
git merge [nome-remote]/NomeBranchRemote
```

\
Visualizando a ligação das branchs locais com as remotas desde o último fetch:

``` bash
git branch -vv
```

\
Apagar branch no remote:

``` bash
git push --delete origin NomeBranch
```

\
Fazendo commit de trechos específicos do arquivo:

``` bash
git add --patch
```

\
Desfazendo as alterações de um arquivo:

``` bash
git checkout -- nomeArquivo
```

\
Desfazendo a aplicação de um commit:

``` bash
git reset [hash commit] --hard (working-directory not safe)
git reset [hash commit] (working-directory safe)
```

\
Reescrever histórico dos commits:

``` bash
git rebase -i head~[numero commits]
```

\
Fazendo rebase a partir de um commit específico:

``` bash
git rebase --onto hash-nova-head hash-inicio-comparacao
```

\
Fazer o merge de um commit específico para branch atual:

``` bash
git cherry-pick [hash-commit]
```

\
Realizando alteração em todos os commits do histórico:

``` bash
git filter-branch --tree-filter '(comando git, ex: rm -f NomeArquivo)' HEAD
```

\
Compactar database interno do git:

``` bash
git gc
```

### **Tags**

Pesquisando todas as tags existentes:

``` bash
git tag
```

Pesquisando as tags com um padrão especifico:

``` bash
git tag -l "v1.1.*"
```

Criando uma tag na branch atual:

``` bash
git tag -a v1.0 -m "Descrição da tag"
```

Criando uma tag para um commit específico:

``` bash
git tag -a v1.0 IdCommit -m "Descrição da tag"
```

Fazendo push da tag para remote:

``` bash
git push origin --tags OU git push origin NomeTag
```

Criando uma branch a partir de uma tag:

``` bash
git checkout -b NovaBranch TagName
```

## **Atributos**

O uso de atributos diversas customizações, como por exemplo:

- Ignorar uma pasta/arquivo quando for gerar arquivo de export
- Definir arquivos como binários evitando comparações para merge
- Definir programas específicos para ler o contéudo de arquivos binários, possibilitando assim extrair as informações para uso do git (ex: ler texto de pdfs, word e metadados)
- Definir que estratégia de merge usar em arquivos/pastas especificas (ex: sempre considerar versão local como correta)

## **Hooks**

Permite disparar scripts customizados em certos eventos. Esses scripts podem ser executados no client ou servidor e podem cancelar o commit. Alguns exemplos de uso:

- Garantir que mensagems de commits estejam em um padrão desejado
- Definir ACLs
- Envio de emails
- Rodar testes
- Garantir policies definidas
