---
marp: true
theme: default
paginate: true
backgroundColor: #ffffff
style: |
  section {
    font-family: 'Segoe UI', Arial, sans-serif;
    font-size: 22px;
    color: #1a1a2e;
    padding: 40px 60px;
  }
  h1 {
    color: #16213e;
    font-size: 42px;
    border-bottom: 3px solid #0f3460;
    padding-bottom: 10px;
  }
  h2 {
    color: #0f3460;
    font-size: 32px;
    border-left: 5px solid #e94560;
    padding-left: 15px;
  }
  h3 {
    color: #e94560;
    font-size: 24px;
  }
  ul {
    line-height: 1.8;
  }
  li {
    margin-bottom: 6px;
  }
  strong {
    color: #0f3460;
  }
  .capa {
    text-align: center;
    margin-top: 60px;
  }
  section.capa h1 {
    border: none;
    font-size: 48px;
    color: #0f3460;
  }
  footer {
    font-size: 14px;
    color: #888;
  }
  code {
    background: #f0f4f8;
    padding: 2px 8px;
    border-radius: 4px;
    font-size: 18px;
  }
---

<!-- _class: capa -->

# Virtualização e Arquiteturas de Hardware

**Grupo 1 — Dia 1**

Vinicius · Thiago · Vitor · Jeann
Amaro · Dani · Miguel · Alec

*Sistemas Operacionais — 2025*

---

## O que é Virtualização?

**Apresentação: Vinicius**

- Permite rodar **múltiplos sistemas operacionais** em uma única máquina física
- O hardware real é **abstraído** e compartilhado entre ambientes isolados
- Cada ambiente virtual acredita ter acesso exclusivo ao hardware
- Base de toda infraestrutura de **cloud computing** moderna

> "A virtualização cria uma camada de abstração entre o hardware e o software que o utiliza."

---

## História da Virtualização

**Apresentação: Thiago**

- **Anos 60/70** — IBM CP/CMS: primeiro sistema com VMs no mainframe IBM 360
- **Anos 80/90** — Declínio com a ascensão dos PCs; foco em hardware dedicado
- **1999** — VMware traz virtualização para arquitetura x86
- **Anos 2000** — Xen, KVM, Hyper-V: virtualização se torna mainstream
- **Hoje** — Containers (Docker), cloud (AWS, Azure, GCP) são a norma

---

## As 3 Exigências de Popek & Goldberg (1974)

**Apresentação: Vitor**

### Fidelidade
O programa deve se comportar **da mesma forma** que no hardware real

### Segurança (Isolamento)
O hypervisor mantém **controle total** — a VM não pode comprometer o sistema

### Eficiência
A maioria das instruções deve executar **diretamente no hardware**, sem overhead significativo

---

## Hipervisor Tipo 1 vs Tipo 2

**Apresentação: Jeann**

| | **Tipo 1 (Bare-metal)** | **Tipo 2 (Hosted)** |
|---|---|---|
| Roda sobre | Hardware diretamente | Sistema operacional host |
| Desempenho | ✅ Alto | ⚠️ Menor overhead |
| Exemplos | VMware ESXi, Hyper-V, Xen | VirtualBox, VMware Workstation |
| Uso comum | Servidores, datacenter | Desenvolvimento, desktop |

---

## Trap-and-Emulate

**Apresentação: Amaro**

- A VM roda em **modo usuário** (ring 1), não em modo kernel
- Quando executa instrução privilegiada → **trap** (armadilha) para o hypervisor
- O hypervisor **emula** o efeito da instrução e retorna o controle
- Problema: arquitetura **x86 original não era virtualizável** (instruções sensíveis sem trap)

### Tradução Binária
- Solução da VMware para x86 não-virtualizável
- Hypervisor **recompila** blocos de código em tempo real
- Substitui instruções problemáticas por chamadas seguras ao hypervisor

---

## Paravirtualização

**Apresentação: Dani**

- Em vez de **emular** o hardware, o SO convidado é **modificado**
- O kernel da VM sabe que está virtualizado e **coopera** com o hypervisor
- Usa **hypercalls** (como syscalls, mas para o hypervisor) no lugar de instruções privilegiadas

### Vantagens vs Desvantagens
- ✅ **Mais eficiente** que trap-and-emulate puro
- ✅ Sem necessidade de tradução binária
- ❌ Requer **modificação do SO** convidado (impossível com Windows proprietário)
- **Exemplo:** Xen com Linux paravirtualizado

---

## Virtualização de Memória

**Apresentação: Miguel**

### O problema
Cada VM acredita ter acesso ao espaço de endereçamento completo

### Tabelas de Páginas Sombra (Shadow Page Tables)
- Hypervisor mantém uma tabela **extra** que mapeia endereços virtuais da VM → físicos reais
- Toda modificação da VM é interceptada e sincronizada

### Tabelas Aninhadas (Hardware-Assisted)
- **Intel EPT** / **AMD NPT**: hardware gerencia dois níveis de tradução
- VM→físico-virtual → físico-real, **sem intervenção do hypervisor**
- Muito mais eficiente: reduz drasticamente os exits de VM

---

## Virtualização de E/S

**Apresentação: Alec**

### Emulação completa
- Hypervisor emula dispositivos (placa de rede, disco) via software
- Simples, mas **lento** — muitas interrupções e context switches

### VirtIO
- Interface **paravirtualizada** para E/S
- VM e hypervisor compartilham filas de memória (virtqueues)
- Muito mais eficiente que emulação completa

### Passthrough (IOMMU / SR-IOV)
- Dispositivo físico é **atribuído diretamente** à VM
- Desempenho quase nativo
- Usa **Intel VT-d** ou **AMD-Vi** para isolamento seguro

---

## Conclusão

**Apresentação: Vinicius**

- Virtualização resolve o problema de **compartilhar hardware** com segurança e eficiência
- Evolução: emulação → trap-and-emulate → paravirtualização → assistência por hardware
- Hoje o hardware (Intel VT-x, AMD-V, EPT, IOMMU) foi **redesenhado para virtualizar melhor**
- Base de toda infraestrutura moderna de cloud e containers

---

<!-- _class: capa -->

# Obrigado!

**Dúvidas e perguntas?**

Vinicius · Thiago · Vitor · Jeann · Amaro · Dani · Miguel · Alec
