# 🚀 Trilha de Conhecimento DevOps
Repositório criado para documentar minha evolução prática em Linux, Git, GitHub, redes e troubleshooting.

---

## 📌 Mission 1 — Git, GitHub e Linux

### Introdução
Projeto focado em aprender os fundamentos de Git, GitHub e Linux.

### O que aprendi

| Comando | O que faz |
|---|---|
| `git init` | Inicializa um repositório Git na pasta atual |
| `git add` | Prepara os arquivos para serem commitados |
| `git commit` | Salva um ponto na história do projeto com uma mensagem |
| `git push` | Envia os commits para o GitHub |
| `git pull` | Baixa as atualizações do GitHub para o computador |
| `branches` | Cria ramificações para desenvolver sem afetar o código principal |
| `merge` | Une duas branches em uma só |
| `clone` | Copia um repositório do GitHub para o computador |

### Tecnologias utilizadas
- Git
- GitHub
- Ubuntu Linux

### Comando utilizado para descobrir o IP
```bash
hostname -I
# Exibe o endereço IP privado da máquina na rede local
```

### Como resolvi o erro de permissão do GitHub
```bash
gh auth login
# Autentica a conta do GitHub pelo terminal, permitindo fazer push sem erro de permissão
```

### Evidências
Os prints estão organizados na pasta `evidencias/mission-1`.

---

## 📌 Mission 2 — Investigação de Redes e Processos no Linux

### Introdução
Nessa missão aprendi a investigar processos, monitorar portas, analisar conexões de rede e trabalhar com troubleshooting no Linux.

### Listagem de processos
```bash
ps aux
# Lista todos os processos rodando no sistema com detalhes de CPU e memória
```

### Encontrando processos do Nginx
```bash
ps aux | grep nginx
# Filtra a lista de processos mostrando apenas os relacionados ao Nginx
# O | (pipe) passa a saída de um comando para o outro
```

### Descobrindo qual PID estava usando a porta 80
```bash
sudo ss -tulnp | grep :80
# ss -tulnp → mostra todas as portas abertas e seus processos
# grep :80  → filtra apenas a porta 80
```

### Monitoramento de recursos
```bash
top
# Exibe em tempo real o uso de CPU e memória de cada processo
# É o gerenciador de tarefas do Linux
```

### Verificando conexões de rede
```bash
sudo ss -tanp | grep :80
# Mostra conexões ativas na porta 80
# ESTABLISHED = conexão aberta com algum cliente
```

### Simulação de incidente
```bash
sudo kill -9 PID
# Força o encerramento imediato do processo pelo seu PID
# Substitua PID pelo número do processo encontrado no ss ou ps
```

### Diferença entre kill normal e kill -9

| Comando | Comportamento |
|---|---|
| `kill PID` | Pede ao processo que encerre corretamente, liberando recursos |
| `kill -9 PID` | Força encerramento imediato, sem permitir resposta do processo |

### Acompanhamento de logs em tempo real
```bash
journalctl -f
# Exibe os logs do sistema em tempo real
# Útil para monitorar erros e eventos enquanto acontecem
```

### Reload do Nginx sem derrubar o serviço
```bash
sudo nginx -s reload
# Recarrega as configurações do Nginx sem interromper conexões ativas
# Sites em uso não percebem nada durante o reload
```

### O que aprendi na Mission 2
- Investigação de processos no Linux
- Monitoramento de portas
- Uso do comando ss
- Troubleshooting básico
- Gerenciamento de serviços
- Uso de sinais no Linux
- Logs do sistema
- Observabilidade básica

### Tecnologias utilizadas
- Ubuntu Linux
- Nginx
- Apache2
- Git
- GitHub

---

## 📌 Mission 3 — Segmentação de Redes e Protocolos de Roteamento

### Introdução
Nessa missão aprendi a identificar interfaces de rede, interpretar a tabela de roteamento, rastrear o caminho de pacotes pela internet e calcular sub-redes usando CIDR.

### Identificando interfaces de rede
```bash
ip a
# Lista todas as interfaces de rede da máquina com seus endereços IP e máscaras
```

Saída da minha máquina:
```
1: lo       → 127.0.0.1/8      (loopback, a máquina falando consigo mesma)
2: enp0s3   → 192.168.15.7/24  (interface real, IP atribuído via DHCP)
3: docker0  → 172.17.0.1/16    (interface virtual do Docker, sem containers ativos)
```

### Informações da interface principal

| Campo | Valor |
|---|---|
| Interface | `enp0s3` |
| IP | `192.168.15.7` |
| Máscara | `/24` = `255.255.255.0` |
| Broadcast | `192.168.15.255` |
| Hosts disponíveis na sub-rede | 254 |
| Origem do IP | DHCP (atribuído automaticamente pelo roteador) |

O **broadcast** é o endereço usado para enviar um pacote a todos os dispositivos da rede ao mesmo tempo. O roteador usa isso no DHCP para encontrar os clientes.

### Visualizando a tabela de roteamento
```bash
ip route show
# Exibe todas as rotas configuradas na máquina
# Cada linha representa um caminho que o Linux usa para decidir por onde mandar um pacote
```

Saída da minha máquina:
```
default via 192.168.15.1 dev enp0s3 proto dhcp src 192.168.15.7 metric 100
172.17.0.0/16 dev docker0 proto kernel scope link src 172.17.0.1 linkdown
192.168.15.0/24 dev enp0s3 proto kernel scope link src 192.168.15.7 metric 100
```

| Rota | O que significa |
|---|---|
| `default via 192.168.15.1` | Rota padrão — pacotes com destino desconhecido vão pro roteador doméstico |
| `172.17.0.0/16 via docker0` | Tráfego pra rede Docker vai pela interface interna do Docker |
| `192.168.15.0/24 via enp0s3` | Tráfego pra rede local vai direto, sem passar pelo roteador |

A **rota padrão** (default gateway) é o que salva o pacote quando nenhuma outra rota específica combina com o destino — ele vai pro roteador e o roteador decide o resto.

### Adicionando e removendo uma rota estática
```bash
sudo ip route add 192.168.100.0/24 via 192.168.15.1
# Adiciona manualmente uma rota estática temporária
# Pacotes com destino 192.168.100.x serão enviados para o gateway 192.168.15.1

ip route show
# Confirma que a rota foi adicionada

sudo ip route del 192.168.100.0/24
# Remove a rota criada

ip route show
# Confirma que a rota foi removida
```

### Testando conectividade com ping
```bash
ping -c 4 8.8.8.8
# Envia 4 pacotes ICMP para o DNS público do Google
# -c 4 → limita para 4 pacotes e encerra automaticamente
```

Saída:
```
64 bytes from 8.8.8.8: icmp_seq=1 ttl=119 time=18.7 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=119 time=20.0 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=119 time=20.3 ms
64 bytes from 8.8.8.8: icmp_seq=4 ttl=119 time=21.4 ms
0% packet loss
```

- `ttl=119` → TTL (Time To Live) indica quantos roteadores o pacote ainda pode passar. Começou em 128 e chegou com 119, ou seja, passou por **9 roteadores**
- `~20ms` → latência excelente
- `0% packet loss` → nenhum pacote perdido, conexão estável

### Rastreando o caminho do pacote com traceroute
```bash
sudo traceroute -I 8.8.8.8
# Rastreia cada roteador (hop) pelo qual o pacote passa até chegar ao destino
# -I → usa ICMP em vez de UDP para melhor compatibilidade
```

Saída:
```
 1   192.168.15.1      0,518ms  → roteador doméstico (gateway local)
 2   * * *             bloqueado → roteador do provedor não responde ICMP
 3   201.1.224.64      5,200ms  → rede interna do provedor
 4   * * *             bloqueado
 5   187.100.83.141   19,022ms  → backbone do provedor
 6   * * *             bloqueado
 7   72.14.220.222    20,926ms  → infraestrutura do Google
 8   142.251.238.15   21,608ms  → infraestrutura do Google
 9   192.178.253.77   20,512ms  → infraestrutura do Google
10   8.8.8.8          21,502ms  → destino final ✅
```

- Total de hops: **10**
- Primeiro gateway identificado: `192.168.15.1` (roteador doméstico)
- Os `* * *` são roteadores que bloqueiam resposta ICMP mas deixam o pacote passar normalmente
- A partir do hop 7 já é infraestrutura interna do Google

### Calculando sub-redes com CIDR

**Exercício 1:** Dividindo `192.168.1.0/24` em sub-redes `/26`

- `/24` tem 8 bits disponíveis para hosts
- `/26` usa 2 bits a mais para identificar sub-redes → **2² = 4 sub-redes**
- Sobram 6 bits para hosts → **2⁶ - 2 = 62 hosts por sub-rede** (descontando rede e broadcast)

| Sub-rede | Range de IPs | Broadcast |
|---|---|---|
| `192.168.1.0/26` | `192.168.1.1` até `192.168.1.62` | `192.168.1.63` |
| `192.168.1.64/26` | `192.168.1.65` até `192.168.1.126` | `192.168.1.127` |
| `192.168.1.128/26` | `192.168.1.129` até `192.168.1.190` | `192.168.1.191` |
| `192.168.1.192/26` | `192.168.1.193` até `192.168.1.254` | `192.168.1.255` |

**Exercício 2:** Dividindo `10.0.0.0/24` em 4 sub-redes iguais

| Sub-rede | Range de IPs | Gateway | Broadcast |
|---|---|---|---|
| `10.0.0.0/26` | `10.0.0.1` até `10.0.0.62` | `10.0.0.1` | `10.0.0.63` |
| `10.0.0.64/26` | `10.0.0.65` até `10.0.0.126` | `10.0.0.65` | `10.0.0.127` |
| `10.0.0.128/26` | `10.0.0.129` até `10.0.0.190` | `10.0.0.129` | `10.0.0.191` |
| `10.0.0.192/26` | `10.0.0.193` até `10.0.0.254` | `10.0.0.193` | `10.0.0.255` |

### IP Aliasing — dois IPs na mesma interface
```bash
sudo ip addr add 192.168.15.100/24 dev enp0s3
# Adiciona um segundo IP (IP aliasing) na interface enp0s3
# Útil para simular múltiplas interfaces ou testar isolamento de rede

ip a show enp0s3
# Confirma que o segundo IP aparece como "secondary" na interface

ping -c 4 192.168.15.100
# Testa conectividade entre os dois IPs da mesma interface

sudo ip addr del 192.168.15.100/24 dev enp0s3
# Remove o IP secundário após o teste
```

Saída do ping entre os dois IPs:
```
64 bytes from 192.168.15.100: icmp_seq=1 ttl=64 time=0.036 ms
64 bytes from 192.168.15.100: icmp_seq=2 ttl=64 time=0.038 ms
64 bytes from 192.168.15.100: icmp_seq=3 ttl=64 time=0.037 ms
64 bytes from 192.168.15.100: icmp_seq=4 ttl=64 time=0.040 ms
0% packet loss
```

A latência de `0.036ms` confirma que a comunicação aconteceu internamente, sem sair pela rede física.

### O que aprendi na Mission 3
- Identificação e leitura de interfaces de rede no Linux
- Interpretação da tabela de roteamento
- Diferença entre rota estática e rota dinâmica (DHCP)
- Criação e remoção manual de rotas estáticas
- Uso do ping para teste de conectividade e análise de TTL
- Uso do traceroute para rastrear o caminho de pacotes pela internet
- Conceito de CIDR e cálculo de sub-redes
- Planejamento de endereçamento IP
- IP Aliasing — múltiplos IPs em uma única interface

### Tecnologias utilizadas
- Ubuntu Linux
- Git
- GitHub

---

## 📂 Organização do Projeto
```
evidencias/
├── mission-1
├── mission-2
└── mission-3
```

---

## 🎯 Objetivo
Evoluir continuamente meus conhecimentos em Linux, redes, Git, GitHub, DevOps e infraestrutura.
