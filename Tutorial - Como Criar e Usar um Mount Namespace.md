## 1 - Introdução

Os mount namespaces são uma funcionalidade poderosa do Linux que permitem isolar os pontos de montagem de sistemas de arquivos. Isso significa que você pode ter diferentes visões de sistema de arquivos em diferentes namespaces. Neste tutorial, focaremos em como criar e usar um mount namespace para isolar pontos de montagem de sistemas de arquivos.

## 1.1 - Pré-requisitos

- Ter um sistema operacional Linux instalado.
- Conhecimento básico de comandos Linux e gerenciamento de sistemas de arquivos.

# 2 - Tutorial

## 2.1 - Criando um Novo Mount Namespace

Para criar um novo mount namespace, execute o seguinte comando no terminal:

```
sudo unshare --mount --fork bash
```

Este comando cria um novo mount namespace e inicia um shell dentro deste namespace. Aqui, as modificações nos pontos de montagem serão isoladas do restante do sistema.

## 2.2 - Montando um Sistema de Arquivos no Namespace

No novo namespace, você pode montar um sistema de arquivos sem afetar o sistema global. Por exemplo, para montar um sistema de arquivos temporário (`tmpfs`):

```
sudo mount -t tmpfs tmpfs /mnt
```

Agora, `/mnt` no namespace atual contém um sistema de arquivos `tmpfs`, isolado dos outros namespaces.

## 2.3 - Verificando os Pontos de Montagem

Para verificar os pontos de montagem dentro do namespace, use o comando `mount` ou `df`. Você notará que as mudanças feitas estão visíveis apenas dentro do namespace.

```
mount
```

ou

```
df -h
```

## 2.4 - Saindo do Namespace e Comparando

Após sair do namespace (fechando o shell ou usando `exit`), você pode verificar que os pontos de montagem do sistema principal não foram alterados. Execute `mount` ou `df -h` no shell original para confirmar.

# 3 - Conclusão

Neste tutorial, você aprendeu como criar e usar um mount namespace no Linux. Essa funcionalidade permite isolar e modificar pontos de montagem de sistemas de arquivos sem impactar o sistema como um todo. Isso é extremamente útil para testar novos sistemas de arquivos, manipular ambientes de teste, ou em cenários de contêineres onde o isolamento de recursos é crítico.

## Nota Adicional

Lembre-se de que as alterações feitas em um mount namespace são temporárias e existem apenas enquanto o namespace está ativo. Ao encerrar a sessão do namespace, todas as montagens exclusivas desse namespace são desfeitas.
