# ☸️ Módulo: Orquestração de Contêineres com Kubernetes (K8s)

O **Kubernetes**, carinhosamente chamado de **K8s**, é uma plataforma de código aberto projetada para automatizar a implantação, o dimensionamento e o gerenciamento de aplicações em contêineres.

---

## 📋 Resumo Teórico

### O que é o Kubernetes?
Originalmente desenvolvido pelo Google, o K8s é o padrão da indústria para gerenciar infraestruturas complexas de contêineres em ambientes de Cloud Pública, Privada ou Híbrida.

### Por que utilizar?
* **Migração de Monólitos:** Suporte total à arquitetura de microsserviços.
* **Alta Disponibilidade:** Auto-cura (self-healing) e baixa taxa de erro.
* **Performance:** Escalabilidade horizontal automática.

---

## 🏗️ Arquitetura Básica

| Componente | Função |
| :--- | :--- |
| **Cluster** | Conjunto de máquinas que trabalham como uma única unidade. |
| **Control Plane** | O gerenciador central que toma decisões sobre o cluster. |
| **Node** | Máquina (VM ou Física) que executa os aplicativos. |
| **Pod** | Menor unidade gerenciável; uma abstração sobre o contêiner. |

---

## 🚀 Fluxo de Trabalho
1. **Criar** o Cluster.
2. **Implantar** o Aplicativo.
3. **Explorar** recursos.
4. **Expor** o serviço publicamente.
5. **Escalar** instâncias.
6. **Atualizar** a aplicação.

---
*Este material faz parte dos estudos de Orquestração de Contêineres.*