# 🧩 Log-Chain

**Log-Chain** is a blockchain-based logging system built on **Hyperledger Fabric**, using **TypeScript / Node.js** and **CouchDB** for rich log queries.
The goal is to provide **immutable, verifiable, and queryable log storage** for audit and security-critical environments.

---

## 🚀 Project Overview

Log-Chain records every log entry as a blockchain transaction.
Each peer in the Fabric network maintains a CouchDB world state, allowing indexed and structured queries on logs while preserving immutability through Fabric’s ledger.

The project is designed with clear separation of concerns:

| Layer      | Purpose                                                                                                                                                      |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `Network/` | Defines and operates the **Hyperledger Fabric network** (organizations, peers, orderers, CAs, channels, chaincode, and CouchDB).                             |
| `Apps/`    | Contains the **application layer**, including REST APIs and log ingestion services built with Node.js and the Fabric Gateway SDK.                            |
| `Infra/`   | Provides **orchestration and infrastructure tooling**, managing how the network and apps run together through Docker Compose or future Kubernetes manifests. |

---

## 💇️ Repository Structure

```
Log-Chain/
├─ Documentation/                # Documentation, diagrams, architecture notes
│  ├─ architecture.md
│  └─ sequences.puml
│
├─ Network/                      # Hyperledger Fabric network layer
│  ├─ config/                    # configtx.yaml, crypto-config.yaml, connection profiles
│  ├─ chaincode/                 # TypeScript chaincode (LogContract)
│  ├─ crypto-material/           # Generated certificates and MSPs (gitignored)
│  ├─ channels/                  # Channel artifacts (.tx, .block)
│  ├─ compose/                   # Docker Compose files for Fabric nodes
│  └─ scripts/                   # Fabric lifecycle scripts (generate, up, down, deploy)
│
├─ Apps/                         # Application layer (API, agents, gateways)
│  ├─ api-gateway/               # REST API built with Node.js / Fabric SDK
│  └─ log-shipper/               # [Optional] Log ingestion service
│
├─ Infra/                        # Infrastructure and orchestration
│  ├─ docker-compose.yml         # Global compose to run network + apps
│  └─ reverse-proxy.yml          # [Optional] Traefik/Nginx setup for routing
│
├─ .env                          # Environment configuration
├─ .gitignore
├─ README.md
├─ run.sh                        # Launch the whole stack (network + apps)
└─ stop.sh                       # Stop and clean containers
```

---

## ⚙️ Quick Start

```bash
# 1. Generate crypto materials and channel artifacts
./Network/scripts/generate-artifacts.sh

# 2. Start the Fabric network
./Network/scripts/network-up.sh

# 3. Deploy the LogChain chaincode
./Network/scripts/deploy-chaincode.sh

# 4. Start the full stack (network + API)
./run.sh

# 5. Stop everything
./stop.sh
```

---

## 🔑 Key Components

* **Hyperledger Fabric 2.x**

  * Modular, permissioned blockchain framework.
  * Uses **CouchDB** as the state database for JSON-based rich queries.
  * Smart contracts (chaincode) written in **TypeScript**.

* **API Gateway (Node.js / TypeScript)**

  * Exposes endpoints to insert and query logs.
  * Connects to the Fabric network using the **Fabric Gateway SDK**.
  * Authenticates via wallets and connection profiles.

* **CouchDB**

  * Stores world state for peers.
  * Enables indexed log queries (`by_timestamp`, `by_source`, etc.).

* **Docker Compose**

  * Manages containers for the full stack: Fabric peers/orderers, CouchDB, API, CA, and optional tools (Explorer, reverse proxy).

---

## 🧠 Design Principles

* **Separation of Concerns:**
  Each layer (Network, App, Infra) has its own lifecycle, security context, and configuration.

* **Security by Design:**
  Private keys, MSPs, and crypto materials are isolated and never committed.

* **Scalability:**
  Additional organizations or peers can be added easily by extending the `Network/` configuration.

* **Extensibility:**
  The same architecture can evolve to Kubernetes or cloud-native deployments without changing app logic.

---

## 📚 Documentation

See [`Documentation/architecture.md`](./Documentation/architecture.md) for detailed design,
including sequence diagrams and Fabric network topology.

---

## ✨ Next Steps

* Add CI/CD workflows for build, test, and linting.
* Implement Fabric Explorer or a custom dashboard.
* Extend LogContract for fine-grained log filtering and pagination.
