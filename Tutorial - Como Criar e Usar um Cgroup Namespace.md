## 0.1 - Introdução

Cgroup (Control Group) namespaces no Linux são usados para isolar a visualização dos cgroups, permitindo que processos em diferentes namespaces vejam diferentes hierarquias de cgroups. Isso é especialmente útil para contêineres e para sistemas que necessitam de isolamento de recursos. Neste tutorial, vamos focar em como criar e usar um cgroup namespace para gerenciar e isolar a visão dos cgroups.

## 0.2 - Pré-requisitos

- Ter um sistema operacional Linux instalado.
- Conhecimento básico sobre terminal Linux.

# 1 - Tutorial

## 1.1 - Criando um Novo Cgroup Namespace

O cgroup namespace é geralmente criado junto com outros namespaces, como o PID ou mount namespace. Aqui, criaremos um novo namespace com isolamento de cgroup junto com um PID namespace:

```
sudo unshare --cgroup --pid --fork bash
```

Isso cria um novo cgroup e PID namespace e inicia um shell dentro deste novo ambiente.

# 2 - Tutorial sobre Cgroup Namespace no Linux

## 2.1 - Introdução

Cgroup (Control Group) namespaces no Linux são usados para isolar a visualização dos cgroups, permitindo que processos em diferentes namespaces vejam diferentes hierarquias de cgroups. Isso é especialmente útil para contêineres e para sistemas que necessitam de isolamento de recursos. Neste tutorial, vamos focar em como criar e usar um cgroup namespace para gerenciar e isolar a visão dos cgroups.

## 2.2 - Pré-requisitos

- Ter um sistema operacional Linux instalado.
- Conhecimento básico sobre cgroups e terminal Linux.

# 3 - Tutorial

## 3.1 - Criando um Novo Cgroup Namespace

Para criar um novo cgroup namespace, você pode usar o comando `unshare`. No entanto, note que a partir da minha última atualização de conhecimento (abril de 2023), não é possível criar um cgroup namespace isoladamente com o comando `unshare`. O cgroup namespace é geralmente criado junto com outros namespaces, como o PID ou mount namespace. Aqui, criaremos um novo namespace com isolamento de cgroup junto com um PID namespace:

```
sudo unshare --cgroup --pid --fork bash
```

Isso cria um novo cgroup e PID namespace e inicia um shell dentro deste novo ambiente.

### 3.1.1 - Trabalhando com Cgroups no Namespace

Dentro do novo namespace, você pode criar e gerenciar cgroups como faria normalmente. Os cgroups criados ou visualizados aqui serão isolados do resto do sistema.

Por exemplo, para visualizar a hierarquia de cgroups:

```
ls /sys/fs/cgroup
```

### 3.1.2 - Verificando a Isolamento de Cgroup

Para entender o isolamento, compare a visualização dos cgroups dentro do namespace com a visualização no namespace global. No namespace global (outro terminal), execute:

```
ls /sys/fs/cgroup
```

Você notará diferenças na estrutura de cgroups exibida.

### 3.1.3 - Utilizando Cgroups para Gerenciamento de Recursos

Você pode criar novos cgroups dentro do namespace para gerenciar recursos como CPU, memória e I/O para processos específicos. A configuração de cgroups permite um controle refinado sobre a alocação de recursos do sistema.

## 3.2 - Configurando Cgroups para Gerenciar CPU

### 3.2.1 - Criando um Cgroup para CPU

Dentro do namespace, crie um novo cgroup para gerenciar a CPU:

```
sudo mkdir /sys/fs/cgroup/cpu/test_cpu
```

### 3.2.2 - Limitando o Uso da CPU

Defina um limite de uso da CPU para o cgroup. Por exemplo, limitar a 20% da CPU:

```
echo 20000 | sudo tee /sys/fs/cgroup/cpu/test_cpu/cpu.cfs_quota_us
```

### 3.2.3 - Adicionando um Processo ao Cgroup

Inicie um processo que consome CPU (como `yes > /dev/null`) e adicione-o ao cgroup:

```
yes > /dev/null &
echo $! | sudo tee /sys/fs/cgroup/cpu/test_cpu/cgroup.procs
```

### 3.2.4 - Configurando Afinidade de CPU

Para designar o processo a uma CPU específica (por exemplo, CPU 0):

`echo 0 | sudo tee /sys/fs/cgroup/cpu/test_cpu/cpuset.cpus`

Este comando configura a afinidade de CPU para o cgroup, restringindo os processos neste cgroup à CPU designada.

## 3.3 - Configurando Cgroups para Gerenciar Memória

Vamos explor a configuração de cgroups para o gerenciamento eficiente da memória. O uso de cgroups para controlar a alocação de memória é crucial em ambientes onde a otimização de recursos é essencial, como em servidores com múltiplas aplicações ou em sistemas de contêineres. A habilidade de limitar o uso de memória de processos específicos previne que um único processo consuma recursos excessivos, garantindo uma distribuição mais equilibrada e evitando situações de esgotamento de memória (OOM). Além disso, esta seção orienta sobre como criar um cgroup dedicado para memória e como definir e aplicar limites de uso, proporcionando uma ferramenta poderosa para administração de sistemas e otimização de performance.

### 3.3.1 - Criando um Cgroup para Memória

Crie um cgroup para gerenciar a memória:

```
sudo mkdir /sys/fs/cgroup/memory/test_memory
```

### 3.3.2 - Limitando o Uso da Memória

Defina um limite de uso de memória. Por exemplo, limitar a 100MB:

```
echo 100M | sudo tee /sys/fs/cgroup/memory/test_memory/memory.limit_in_bytes
```

### 3.3.3 - Adicionando um Processo ao Cgroup

Inicie um processo que consome memória (como um script Python que aloca memória) e adicione-o ao cgroup:

```
python -c "a = ' ' * 1024 * 1024 * 100" &
echo $! | sudo tee /sys/fs/cgroup/memory/test_memory/cgroup.procs
```

## 3.4 - Verificando o Funcionamento dos Cgroups

Monitore o uso de recursos pelos processos nos cgroups criados para confirmar que os limites estão sendo aplicados corretamente.

# 4 - Conclusão

Neste tutorial, você aprendeu a criar um cgroup namespace no Linux e realizar testes práticos para limitar o uso de CPU e memória. Essas técnicas são essenciais para o gerenciamento de recursos em ambientes isolados, proporcionando um controle mais preciso sobre o consumo de recursos do sistema.

## 4.1 - Nota Adicional

Ao finalizar os testes, lembre-se de remover os cgroups criados e encerrar os processos para evitar o consumo desnecessário de recursos. As configurações de cgroup são específicas ao namespace e são revertidas ao sair dele.
