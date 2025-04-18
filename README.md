# 🌾 MER - Sistema de Agricultura Inteligente

Este documento descreve o **Modelo Entidade Relacionamento (MER)** utilizado para controle de sensores agrícolas, aplicação de água e vitaminas, calibração e monitoramento de culturas.

---

## 📌 Visão Geral

O sistema permite:

- Gerenciar sensores e tipos de sensores com unidades de medida.
- Aplicar água automaticamente (via sensores) ou manualmente (sem sensor).
- Monitorar dados registrados por sensores.
- Aplicar vitaminas em culturas com controle de quantidade e responsável.
- Calibrar sensores com histórico.
- Associar sensores a culturas plantadas.

---

## 🧩 Entidades e Atributos

### 🔹 `unidade_medida`

| Campo              | Tipo        |
|--------------------|-------------|
| `id_unidade_medida`| NUMERIC(3)  |
| `descricao`        | VARCHAR(35) |
| `abreviacao`       | VARCHAR(5)  |

---

### 🔹 `tipo_sensor`

| Campo               | Tipo        |
|---------------------|-------------|
| `id_tipo_sensor`    | NUMERIC(3)  |
| `id_unidade_medida` | NUMERIC(3)  |
| `descricao`         | VARCHAR(30) |

---

### 🔹 `sensor`

| Campo          | Tipo         |
|----------------|--------------|
| `id_sensor`    | NUMERIC(6)   |
| `id_cultura`   | NUMERIC(5)   |
| `numero_serie` | VARCHAR(30)  |
| `ativo`        | BOOLEAN(1)   |

---

### 🔹 `sensor_tipo_sensor` (associativa)

| Campo             | Tipo        |
|-------------------|-------------|
| `id_sensor`       | NUMERIC(6)  |
| `id_tipo_sensor`  | NUMERIC(3)  |

---

### 🔹 `cultura`

| Campo              | Tipo         |
|--------------------|--------------|
| `id_cultura`       | NUMERIC(5)   |
| `descricao_cultura`| VARCHAR(30)  |
| `ativo`            | BOOLEAN(1)   |
| `area_plantada`    | NUMERIC(10,2)|

---

### 🔹 `monitoramento`

| Campo                    | Tipo         |
|--------------------------|--------------|
| `id_monitoramento`       | NUMERIC(20)  |
| `id_sensor`              | NUMERIC(6)   |
| `valor`                  | NUMERIC(7,2) |
| `data_hora_monitoramento`| TIMESTAMP    |

---

### 🔹 `calibracao_sensor` (entidade fraca)

| Campo             | Tipo         |
|-------------------|--------------|
| `id_sensor`       | NUMERIC(6)   |
| `data_calibracao` | TIMESTAMP    |
| `fator_calibracao`| DECIMAL(6,3) |
| `responsavel`     | VARCHAR(50)  |

---

### 🔹 `agua_aplicada`

| Campo                        | Tipo         | Observações |
|------------------------------|--------------|-------------|
| `id_agua_aplicada`           | NUMERIC(10)  | PK          |
| `id_sensor`                  | NUMERIC(6)   | FK opcional se aplicação for manual |
| `id_cultura`                 | NUMERIC(5)   | FK          |
| `volume_agua_aplicada`       | DECIMAL(7,4) | obrigatório |
| `data_hora_inicio_aplicacao` | TIMESTAMP    | obrigatório |
| `data_hora_fim_aplicacao`    | TIMESTAMP    | obrigatório |
| `agua_aplicada_manual`       | BOOLEAN      | padrão: false |
| `responsavel`                | VARCHAR(50)  |
| `observacao`                 | TEXT         |

---

### 🔹 `vitaminas`

| Campo              | Tipo         |
|--------------------|--------------|
| `id_vitamina`      | NUMERIC(6)   |
| `id_unidade_medida`| NUMERIC(3)   |
| `nome`             | VARCHAR(30)  |
| `descricao`        | VARCHAR(150) |
| `ativo`            | BOOLEAN(1)   |

---

### 🔹 `vitaminas_aplicadas`

| Campo                  | Tipo           |
|------------------------|----------------|
| `id_vitamina_aplicada` | NUMERIC(10)    |
| `id_vitamina`          | NUMERIC(6)     |
| `id_cultura`           | NUMERIC(5)     |
| `data_aplicacao`       | TIMESTAMP      |
| `quantidade`           | DECIMAL(10,2)  |
| `responsavel`          | VARCHAR(50)    |
| `observacao`           | TEXT           |

---

## 🔗 Relacionamentos

### 🌱 `sensor` relaciona-se com:
- `cultura`: N:1
- `sensor_tipo_sensor`: N:N (via associativa)
- `monitoramento`: 1:N
- `agua_aplicada`: 1:N (se aplicação automatizada)
- `calibracao_sensor`: 1:N (entidade fraca)

### 💧 `agua_aplicada` pode:
- Referenciar um `sensor` (automatizado)
- Ou referenciar uma `cultura` (manual)
- Controlado por `agua_aplicada_manual`

### 🧪 `vitaminas_aplicadas` relaciona:
- `vitaminas` → `vitaminas_aplicadas`: 1:N
- `cultura` → `vitaminas_aplicadas`: 1:N

### 📐 `tipo_sensor` relaciona-se com:
- `unidade_medida`: N:1
- `sensor_tipo_sensor`: 1:N

### 🧮 `vitaminas` e `tipo_sensor` compartilham:
- A mesma `unidade_medida`, via FK

### ⚖️ unidade_medida relaciona-se com:
- `tipo_sensor`: 1:N
- `vitaminas`: 1:N

---

## ✅ Integridade e Flexibilidade

- Permite registros históricos de monitoramento e calibração.
- Aceita sensores sem tipo e aplicações manuais de água.
- Usa entidades fortes, fracas e associativas.
- Tabelas normalizadas.

---

## 📎 Direitos

Este projeto foi desenvolvido para fins acadêmicos. Não é permitido o uso comercial sem autorização prévia do autor.
