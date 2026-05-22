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

## 📂 Organização do Projeto
evidencias/
├── mission-1
└── mission-2

---

## 🎯 Objetivo
Evoluir continuamente meus conhecimentos em Linux, redes, Git, GitHub, DevOps e infraestrutura.
