# Tutorial sobre User Namespace no Linux

## 1 - Introdução

User namespaces são uma funcionalidade avançada no Linux que permite isolar IDs de usuários e grupos. Isso significa que um processo pode ter um UID (User ID) e GID (Group ID) dentro do namespace que é diferente do UID e GID no sistema hospedeiro. Este tutorial focará em como criar e usar um user namespace para gerenciar e isolar IDs de usuários e grupos.

## 1.1 - Pré-requisitos

- Ter uma máquina com sistema operacional Linux.
- Conhecimento básico de terminal, comandos Linux e gerenciamento de usuários/grupos.

# 2 - Tutorial

## 2.1 - Criando um Novo User Namespace

Para criar um novo user namespace, execute o seguinte comando no terminal:

```
sudo unshare --user --map-root-user --fork bash
```

Este comando cria um novo user namespace e inicia um shell dentro deste namespace. Neste namespace, o usuário que executou o comando tem privilégios de root.

## 2.2 - Trabalhando com IDs de Usuário e Grupo no Namespace

No novo namespace, você pode criar usuários e grupos locais que são isolados do sistema principal. Por exemplo, para adicionar um novo usuário:

```
useradd localuser
```

Este usuário `localuser` terá um UID/GID local dentro do namespace, diferente do sistema principal.

## 2.3 - Verificando IDs de Usuário e Grupo no Namespace

Para verificar os IDs de usuário e grupo dentro do namespace, use os comandos `id` ou `cat /etc/passwd`:

```
id localuser
```

ou

```
cat /etc/passwd
```

Isso mostrará os IDs atribuídos dentro do namespace, que podem diferir do sistema global.

## 2.4 - Comparando com o User ID do Sistema Principal

Após sair do namespace (fechando o shell ou usando `exit`), você pode verificar que os IDs de usuário e grupo do sistema principal não foram alterados. Execute `id` ou `cat /etc/passwd` no shell original para confirmar.

# 3 - Conclusão

Neste tutorial, você aprendeu a criar e usar um user namespace no Linux. Isso é particularmente útil para testes e isolamento de ambientes em cenários multiusuários ou de contêineres, permitindo operações com diferentes IDs de usuário e grupo sem impactar o sistema hospedeiro. User namespaces fornecem uma camada adicional de isolamento e segurança, especialmente útil em ambientes de desenvolvimento e produção.

## Nota Adicional

É importante notar que as mudanças feitas dentro de um user namespace são locais a esse namespace e não afetam o sistema como um todo. Ao sair do namespace, todas as configurações e usuários adicionados são revertidos ao estado original do sistema.
