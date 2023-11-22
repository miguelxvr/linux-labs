### Pré-requisitos

- Um sistema Linux com suporte a cgroups.
- Privilegios de superusuário (root) para criar e gerenciar cgroups.

### Passo 1: Instalar e Configurar Cgroups

1. **Instale as ferramentas necessárias (se ainda não estiverem instaladas):**

   ```bash
   sudo apt-get update
   sudo apt-get install cgroup-tools
   ```

2. **Crie um novo grupo cgroup:**

   ```bash
   sudo cgcreate -g cpu:/mycgroup
   ```

### Passo 2: Definir a Política de Escalonamento para o Cgroup

1. **Defina a política `SCHED_FIFO` para o cgroup:**
   - Primeiro, mude para o diretório do cgroup:

	 ```bash
     cd /sys/fs/cgroup/cpu/mycgroup
     ```

   - Em seguida, defina a política de escalonamento (substitua `[pid]` pelo ID do processo que você quer adicionar ao cgroup):

	 ```bash
     echo "fifo [pid]" > cpu.rt_runtime_us
     ```

### Passo 3: Adicionar Processos ao Cgroup

1. **Adicione um processo ao cgroup:**
   - Encontre o ID do processo (PID) que você deseja adicionar. Por exemplo, use `ps` para encontrar o PID de um processo:

	 ```bash
     ps aux | grep [nome_do_processo]
     ```

   - Adicione o processo ao cgroup (substitua `[pid]` pelo PID real):

	 ```bash
     sudo cgclassify -g cpu:/mycgroup [pid]
     ```

### Passo 4: Testando o Cgroup

1. **Teste o cgroup com um script simples:**
   - Crie um script que consuma CPU e observe o comportamento dele sob a política `SCHED_FIFO`. Por exemplo, um script Python que executa um loop infinito.
   - Adicione o script ao cgroup `mycgroup` e monitore seu comportamento usando ferramentas como `htop` ou `top`.

### Verificando o Funcionamento

1. **Monitore o Uso da CPU:**
   - Use `htop` ou `top` para monitorar o uso da CPU pelo seu script.
   - Você deve observar que o processo no cgroup `mycgroup` com a política `SCHED_FIFO` tem prioridade sobre outros processos.

### Limpeza

1. **Remova o cgroup após os testes (opcional):**

   ```bash
   sudo cgdelete cpu:/mycgroup
   ```

### Nota Importante

- O uso de `SCHED_FIFO` pode levar a condições de starvation (inanição) para outros processos se não for gerenciado cuidadosamente, pois processos com essa política podem monopolizar a CPU.
- Teste sempre em um ambiente seguro, como uma máquina virtual, para evitar afetar o sistema principal.
