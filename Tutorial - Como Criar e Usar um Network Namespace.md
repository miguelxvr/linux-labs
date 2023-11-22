# 1 - Introdução

Network namespace é uma funcionalidade do Linux que permite isolar a rede de um sistema. Com isso, é possível ter interfaces de rede, tabelas de roteamento, firewalls, e outras configurações de rede independentes para diferentes processos. Vamos aprender como criar e usar um network namespace para isolar esses recursos de rede.

## 1.1 - Pré-requisitos

- Ter uma máquina com sistema operacional Linux
- Ter conhecimentos básicos de terminal e comandos Linux

# 2 - Tutorial

## 2.1 - Criando um Novo Network Namespace

   Use o seguinte comando para criar um novo network namespace:

   ```
   sudo ip netns add netns1
   ```

   Isso cria um namespace chamado `netns1`.

## 2.2 - Verificando os Namespaces de Rede

   Para listar os namespaces de rede existentes, use:

   ```
   ip netns list
   ```

## 2.3 - Configurando Interfaces de Rede no Namespace

   - **Adicionar uma Interface Virtual:** Crie uma veth pair (par de interfaces virtuais) e adicione ao namespace:

	 ```
     sudo ip link add veth0 type veth peer name veth1
     sudo ip link set veth1 netns netns1
     ```

   - **Configuração de IP:** Atribua endereços IP às interfaces:

	 ```
     sudo ip addr add 192.168.1.1/24 dev veth0
     sudo ip netns exec netns1 ip addr add 192.168.1.2/24 dev veth1
     ```

   - **Ativar Interfaces:** Levante as interfaces:

	 ```
     sudo ip link set veth0 up
     sudo ip netns exec netns1 ip link set veth1 up
     sudo ip netns exec netns1 ip link set lo up
     ```

## 2.4 - Testando a Conectividade

   Verifique a conectividade entre as interfaces:

   ```
   ping -c 3 192.168.1.2
   sudo ip netns exec netns1 ping -c 3 192.168.1.1
   ```

## 2.5 - Visualizando Tabelas de Roteamento e Configurações de Rede

   - **Dentro do Namespace:** Use o comando a seguir para ver as configurações de rede dentro do namespace:

	 ```
     sudo ip netns exec netns1 ip route
     ```

   - **Fora do Namespace:** Visualize as configurações de rede do sistema principal:

	 ```
     ip route
     ```

# 3 - Conclusão

Com este tutorial, você aprendeu a criar e configurar um network namespace no Linux, permitindo isolar e gerenciar as configurações de rede de maneira independente. Essa habilidade é especialmente útil para testes de rede, desenvolvimento de aplicações de rede, e para garantir isolamento e segurança em ambientes de contêineres.

## Nota Adicional

Ao terminar de usar o namespace, você pode removê-lo com `sudo ip netns del netns1`. Lembre-se de que as configurações e interfaces alocadas ao namespace são específicas a ele e não afetam o namespace de rede padrão do sistema.
