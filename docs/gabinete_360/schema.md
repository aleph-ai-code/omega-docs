# Gabinete 360 — Schema Atualizado

> Última atualização: 2026-05-20
> Supabase: pumsmythpwidsfjhyxhk
> Ω · Denalth

## Overview

- **363 lideranças** cadastradas
- **184 municípios**
- **5 responsáveis** por contato
- **184 acordos financeiros** (1 por município)

---

## Tabelas Principais (tb_*)

### tb_cadastro_liderancas (363 registros)
| Coluna | Tipo | Descrição |
|--------|------|-----------|
| id | UUID | PK |
| nome_completo | text | Nome da liderança |
| municipio | text | Nome denormalizado |
| municipio_id | UUID | FK → tb_municipios.id |
| resp_contato | text | Nome denormalizado |
| resp_contato_id | UUID | FK → tb_responsaveis_contato.id |
| status_lideranca | text | Status denormalizado |
| status_lideranca_id | UUID | FK → tb_status_liderancas.id |
| tipo_lideranca | text | Denormalizado |
| tipo_lideranca_id | UUID | FK → tb_tipos_lideranca.id |
| sub_tipo_lideranca | text | Denormalizado |
| sub_tipo_lideranca_id | UUID | FK → tb_sub_tipos_lideranca.id |
| cargo_candidado | text | Denormalizado |
| cargo_candidado_id | UUID | FK → tb_cargos_candidatos.id |
| status_voto_dayany | text | Denormalizado |
| status_voto_dayany_id | UUID | FK → tb_status_votos.id |
| status_voto_reginauro | text | Denormalizado |
| status_voto_reginauro_id | UUID | FK → tb_status_votos.id |
| votos_dayany | text | Votos prometidos Dayany |
| votos_reginauro | text | Votos prometidos Reginauro |
| telefone_1 a telefone_4 | text | Contatos |
| acordo_financeiro | text | Denormalizado |
| photo_url | text | Foto |
| origem_arquivo | text | Arquivo de origem |
| observacao_status | text | Observações |
| deleted | boolean | Soft delete |
| deleted_at | timestamp | Data delete |
| created_at | timestamp | |
| updated_at | timestamp | |

### tb_municipios (184 registros)
| Coluna | Tipo | Descrição |
|--------|------|-----------|
| id | UUID | PK |
| municipios | text | Nome do município |
| voters_count | integer | Eleitores |
| votes_2022 | integer | Votos 2022 |
| expected_votes_2026 | integer | Meta 2026 |
| reached_votes | integer | Votos alcançados |
| sub_region | text | Região (ex: Cariri) |
| is_focus | boolean | Município foco |
| top_candidates_past | text | Candidatos anteriores |
| created_at / updated_at | timestamp | |

### tb_responsaveis_contato (5 registros)
JUSSARA, RUDI, IGOR, FELIPE, CAPITÃO

### tb_acordos_financeiros (184 registros)
| Coluna | Tipo |
|--------|------|
| id | UUID |
| municipio | text |
| valor_total | integer |
| ultima_atualizacao | timestamp |

### tb_parcelas_acordos (3 registros)
| Coluna | Tipo |
|--------|------|
| id | UUID |
| acordo_id | UUID (FK) |
| mes_referencia | text |
| valor_parcela | integer |
| status_pagamento | text |

## Tabelas Auxiliares (lookup)

| Tabela | Registros | Valores |
|--------|-----------|---------|
| tb_status_liderancas | 3 | FECHADO, COBRAR, ? |
| tb_status_votos | 2 | ?, ? |
| tb_tipos_lideranca | 3 | ?, ?, ? |
| tb_sub_tipos_lideranca | 3 | ?, ?, ? |
| tb_cargos_candidatos | 3 | ?, ?, ? |
| tb_perfis_usuarios | 1 | email, role, municipio_vinculado_id |
| tb_feature_flags | 3 | features toggle |

## Views

### vw_cadastro_completo
Colunas amigáveis: Nome, Município, Responsável, Status, Tipo, Telefone, Votos (Dayany/Reginauro), Acordo Financeiro, Observações

### vw_relatorio_final
Versão resumida da vw_cadastro_completo (sem telefone, com Tipo/Cargo combinado)

## Tabelas Antigas (a limpar)
- cadastro_liderancas, Municipality, User, Agreement, Agenda, Travel, etc.
- Views auxiliares: aux_resp_contato, aux_status_lideranca, aux_tipo_lideranca
- ContentDirection, Directory, Evaluation, Evidence, Neighborhood

## Relacionamentos

```
tb_cadastro_liderancas
  ├── municipio_id → tb_municipios.id
  ├── resp_contato_id → tb_responsaveis_contato.id
  ├── status_lideranca_id → tb_status_liderancas.id
  ├── tipo_lideranca_id → tb_tipos_lideranca.id
  ├── sub_tipo_lideranca_id → tb_sub_tipos_lideranca.id
  ├── cargo_candidado_id → tb_cargos_candidatos.id
  ├── status_voto_dayany_id → tb_status_votos.id
  └── status_voto_reginauro_id → tb_status_votos.id

tb_acordos_financeiros
  └── municipio → tb_municipios.municipios (por nome, não FK)

tb_parcelas_acordos
  └── acordo_id → tb_acordos_financeiros.id

tb_perfis_usuarios
  ├── municipio_vinculado_id → tb_municipios.id
  └── responsavel_id → tb_responsaveis_contato.id
```

## Notas
- Campos denormalizados (municipio, resp_contato, status_lideranca, etc.) são redundantes com os _id — provavelmente pra simplificar queries/views
- Views já existem com labels amigáveis
- tb_acordos_financeiros liga por NOME de município (não UUID) — oportunidade de melhoria
