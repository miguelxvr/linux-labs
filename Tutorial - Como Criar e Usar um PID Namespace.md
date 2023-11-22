# 1 - Introdução

Namespaces são uma funcionalidade do Linux que permitem isolar recursos do sistema operacional, como o espaço de IDs de processos, interfaces de rede, e sistemas de arquivos, para criar ambientes isolados semelhantes a contêineres. Neste tutorial, vamos focar em como criar e usar um namespace PID para isolar o espaço de IDs de processos.

## 1.1 - Pré-requisitos

- Ter uma máquina com sistema operacional Linux
- Ter conhecimentos básicos de terminal e comandos Linux

# 2 - Tutorial

## 2.1 - Criando um Novo Namespace PID

Para criar um novo namespace PID, execute o seguinte comando no terminal:

```
sudo unshare --pid --fork bash
```

Isso criará um novo namespace PID e iniciará um shell dentro desse namespace.

## 2.2 - Verificando os Processos no Novo Namespace PID

Agora que você está dentro do novo namespace PID, você pode executar o comando "ps" para ver os processos em execução. Você verá apenas os processos que pertencem ao namespace atual, e o shell que você iniciou terá o PID 1.

```
ps
```

## 2.3 - Trabalhando com Processos no Namespace

Dentro do novo namespace, você pode iniciar processos como de costume. Por exemplo, iniciar um simples processo `sleep`:

`sleep 100 &`

Este processo terá um PID dentro do namespace, que pode ser diferente do seu PID no namespace global.

## 2.4 - Visualizando Processos Fora do Namespace PID

Se você quiser ver os processos fora do namespace atual, pode usar o comando "nsenter" para entrar no namespace do processo 1, que é o namespace pai, e listar todos os processos no sistema.

```
sudo nsenter --target 1 --pid -- bash
ps
```

# 3 - Conclusão

Neste tutorial, você aprendeu a criar e usar um namespace PID para isolar o espaço de IDs de processos. Isso é útil para criar ambientes isolados, como contêineres, onde cada contêiner tem seu próprio PID 1, que é o processo inicial. Além disso, você aprendeu a visualizar processos dentro e fora do namespace PID usando os comandos "ps" e "nsenter".
