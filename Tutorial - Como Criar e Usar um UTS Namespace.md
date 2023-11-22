## 1 - Introdução

UTS (UNIX Time-sharing System) namespaces são uma funcionalidade do Linux que permite isolar informações como hostname e NIS (Network Information Service) domain name. Esta isolamento é crucial para aplicações que precisam operar com seus próprios hostnames sem afetar o sistema hospedeiro. Neste tutorial, focaremos em como criar e usar um UTS namespace para modificar e gerenciar o hostname de forma isolada.

## 1.1 - Pré-requisitos

- Ter uma máquina com sistema operacional Linux.
- Conhecimento básico de terminal e comandos Linux.

# 2 - Tutorial

## 2.1 - Criando um Novo UTS Namespace

Para criar um novo UTS namespace, execute o seguinte comando no terminal:

```
sudo unshare --uts --fork bash
```

Esse comando cria um novo UTS namespace e inicia um shell dentro desse namespace. Alterações feitas ao hostname neste namespace não afetarão o hostname do sistema principal.

## 2.2 - Alterando o Hostname no Namespace

Dentro do novo namespace, você pode alterar o hostname utilizando o comando `hostname`. Por exemplo:

```
sudo hostname novo-hostname
```

Agora, o hostname dentro deste namespace é `novo-hostname`. Esta mudança é visível apenas dentro do namespace.

## 2.3 - Verificando o Hostname no Namespace

Para verificar o hostname dentro do namespace, use o comando:

```
hostname
```

Isso mostrará `novo-hostname`, refletindo a mudança feita no namespace.

## 2.4 - Comparando com o Hostname do Sistema Principal

Após sair do namespace (fechando o shell ou usando `exit`), você pode verificar que o hostname do sistema principal permanece inalterado. Execute `hostname` no shell original para confirmar que o hostname original ainda está em vigor.

# 3 - Conclusão

Neste tutorial, você aprendeu a criar e usar um UTS namespace no Linux. Isso é útil para testar aplicações e serviços que dependem do hostname sem afetar o hostname do sistema hospedeiro. O UTS namespace oferece uma maneira segura e isolada de manipular informações de identificação do sistema, como hostname e NIS domain name, essencial em ambientes de teste ou em cenários de contêinerização.

## Nota Adicional

Assim como outros namespaces, as alterações feitas em um UTS namespace são temporárias e limitadas ao escopo do namespace. Ao sair do namespace, as configurações retornam ao seu estado original.
