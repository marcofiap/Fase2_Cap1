# 📊 Modelo Entidade-Relacionamento - Sistema de Monitoramento Agrícola

Este documento descreve o modelo entidade-relacionamento (MER) atualizado para o sistema de monitoramento agrícola, incluindo sensores, calibração, culturas e aplicação de água.

---

## 🗂️ Entidades

### 🌾 `cultura`
Armazena informações sobre as culturas agrícolas.

| Campo              | Tipo         | Descrição                        |
|-------------------|--------------|----------------------------------|
| `id_cultura`       | NUMERIC(5)   | Identificador da cultura (PK)    |
| `descricao_cultura`| VARCHAR(30)  | Nome ou descrição da cultura     |
| `area_plantada`    | NUMERIC(10,2)| Área total plantada (em hectares)|

---

### 📟 `sensor`
Representa os sensores instalados no campo.

| Campo         | Tipo         | Descrição                              |
|--------------|--------------|----------------------------------------|
| `id_sensor`   | NUMERIC(6)   | Identificador do sensor (PK)           |
| `id_cultura`  | NUMERIC(5)   | Cultura monitorada (FK → `cultura`)    |
| `numero_serie`| VARCHAR(30)  | Número de série do sensor              |

---

### 🧪 `tipo_sensor`
Define os tipos de sensores disponíveis.

| Campo             | Tipo         | Descrição                                 |
|------------------|--------------|-------------------------------------------|
| `id_tipo_sensor`  | NUMERIC(3)   | Identificador do tipo (PK)                |
| `id_unidade_medida`| NUMERIC(3) | Unidade usada (FK → `unidade_medida`)     |
| `descricao`       | VARCHAR(30)  | Descrição do tipo de sensor               |

---

### 📐 `unidade_medida`
Unidades de medida utilizadas pelos sensores.

| Campo              | Tipo         | Descrição                              |
|-------------------|--------------|----------------------------------------|
| `id_unidade_medida`| NUMERIC(3)   | Identificador da unidade (PK)          |
| `descricao`        | VARCHAR(35)  | Nome da unidade                        |
| `abreviacao`       | VARCHAR(5)   | Ex: °C, pH, %                           |

---

### 💧 `agua_aplicada`
Registra aplicações de água feitas via sensores.

| Campo                       | Tipo          | Descrição                                 |
|----------------------------|---------------|-------------------------------------------|
| `id_agua_aplicada`          | NUMERIC(10)   | Identificador da aplicação (PK)           |
| `id_sensor`                 | NUMERIC(6)    | Sensor utilizado (FK → `sensor`)          |
| `volume_agua_aplicada`      | DECIMAL(7,4)  | Volume aplicado (litros)                  |
| `data_hora_inicio_aplicacao`| Timestamp     | Início da aplicação                       |
| `data_hora_fim_aplicacao`   | Timestamp     | Fim da aplicação                          |

---

### 📈 `monitoramento`
Dados registrados periodicamente pelos sensores.

| Campo                   | Tipo         | Descrição                                 |
|------------------------|--------------|-------------------------------------------|
| `id_monitoramento`      | NUMERIC(20)  | Identificador da leitura (PK)             |
| `id_sensor`             | NUMERIC(6)   | Sensor relacionado (FK → `sensor`)        |
| `valor`                 | NUMERIC(7,2) | Valor registrado                          |
| `data_hora_monitoramento`| Timestamp   | Data/hora da leitura                      |

---

## 🔁 Entidades Associativas

### 🧩 `sensor_tipo_sensor`
Relacionamento N:N entre sensores e tipos de sensores.

| Campo            | Tipo        | Descrição                              |
|------------------|-------------|----------------------------------------|
| `id_tipo_sensor` | NUMERIC(3)  | FK para `tipo_sensor` (PK composta)    |
| `id_sensor`      | NUMERIC(6)  | FK para `sensor` (PK composta)         |

---

## 🧷 Entidades Fracas

### 🧪 `calibracao_sensor`
Registra eventos de calibração de sensores — **entidade fraca**.

| Campo             | Tipo          | Descrição                                 |
|------------------|---------------|-------------------------------------------|
| `id_sensor`       | NUMERIC(6)    | Sensor calibrado (PK composta / FK)       |
| `data_calibracao` | Timestamp     | Data da calibração (PK composta)          |
| `fator_calibracao`| DECIMAL(6,3)  | Fator aplicado na calibração              |
| `responsavel`     | VARCHAR(50)   | Responsável pela calibração               |

> **Observação:** A chave primária da entidade depende de `id_sensor`, caracterizando-a como **entidade fraca**.

---

## 🔗 Relacionamentos

- `sensor` ↔ `cultura`: muitos sensores podem pertencer a uma cultura.
- `sensor` ↔ `monitoramento`: um sensor gera múltiplos monitoramentos.
- `sensor` ↔ `agua_aplicada`: um sensor pode estar vinculado a várias aplicações.
- `sensor` ↔ `tipo_sensor`: relacionamento N:N via `sensor_tipo_sensor`.
- `tipo_sensor` ↔ `unidade_medida`: cada tipo de sensor mede uma unidade.
- `sensor` ↔ `calibracao_sensor`: uma calibragem existe apenas em função de um sensor.

---

## 📝 Considerações Finais

- Todos os campos obrigatórios estão sinalizados com `*` no diagrama original.
