# PRD — Gabinete 360: Sistema de Cadastro e Gestão de Lideranças

> Versão: 1.0 | Data: 2026-05-20
> Autor: Omega ♾️ | Revisão: Denalth
> Destino: Atlas (Dev Sênior)

## 1. Visão Geral

Sistema de gestão de lideranças políticas com cadastro, consulta, atualização e exportação de dados. O sistema nasce como uma interface de cadastro simples e evolui para um dashboard dinâmico com módulos.

## 2. Stack Técnica

| Camada | Tecnologia | Motivo |
|--------|-----------|--------|
| Frontend | Next.js 15 (App Router) | Modular, escalável, SSR/SSG |
| UI | Tailwind CSS + shadcn/ui | Componentes profissionais, rápido |
| Backend/DB | Supabase (já existente) | DB + Auth + Storage + RLS |
| Auth | Supabase Auth (Magic Link / Email OTP) | Sem senha, PIN no email |
| Export PDF | @react-pdf/renderer ou jsPDF | Geração local |
| Export Excel | xlsx (SheetJS) | Geração local |
| Deploy | Local + Tailscale | Sem custo, sem exposição pública |
| Charts | Recharts (futuro dashboard) | Leve, React-native |

### Acesso via Tailscale
- App roda na máquina local (porta 3000)
- Tailscale Serve expõe `https://gabinete360.tail-xxxx.ts.net`
- Participantes acessam via link Tailscale (sem VPN client, só navegador)
- Alternativa: Tailscale Funnel para acesso externo temporário

## 3. Autenticação

### Fase 1 (atual)
- Login via email + Magic Link (PIN chega no email)
- Todos os usuários autenticados veem tudo
- Usuário admin (Denalth) pode gerenciar outros

### Fase 2 (futuro)
- RLS por responsável: cada um vê só suas lideranças/municípios
- Perfis: admin, responsável, visualizador
- Tabela `tb_perfis_usuarios` já existe pra isso

### Fluxo
1. Usuário acessa URL Tailscale
2. Digita email
3. Recebe PIN no email
4. Digita PIN → autenticado
5. Redirecionado pro dashboard

## 4. Funcionalidades — MVP

### 4.1 Dashboard (home)
- Cards com resumo: total lideranças, por status, por responsável
- Contadores: votos prometidos (Dayany / Reginauro), municípios foco
- Link rápido pra ações (nova liderança, exportar)

### 4.2 CRUD Lideranças (tb_cadastro_liderancas)
**Listagem:**
- Tabela paginada com busca (nome, município, telefone)
- Filtros: status, responsável, município, tipo de liderança
- Colunas: Nome, Município, Responsável, Status, Votos Dayany, Votos Reginauro, Telefone

**Cadastro (novo):**
- Formulário com campos da tabela
- Dropdowns populados das tabelas auxiliares (municípios, responsáveis, status, tipos)
- Validação: nome obrigatório, município obrigatório
- Telefones: até 4 campos

**Edição:**
- Mesmo formulário do cadastro, preenchido
- Botão de soft delete (marca `deleted=true`, não remove)

**Visualização:**
- Ficha completa da liderança (todos os campos + foto se houver)
- Histórico de atualizações (created_at, updated_at)

### 4.3 CRUD Auxiliares
- Tabelas simples: responsáveis, status, tipos, subtipos, cargos
- Interface de administração (só admin)
- Adicionar/remover/editar registros

### 4.4 Acordos Financeiros
- Listagem por município
- Editar valor total e parcelas
- Status de pagamento

### 4.5 Exportação
**Da vw_cadastro_completo:**
- Exportar PDF (tabela formatada com cabeçalhos amigáveis)
- Exportar XLSX (planilha com todas as colunas)
- Filtros aplicados são respeitados na exportação

**Da vw_relatorio_final:**
- Mesma licação: PDF e XLSX

**Personalização:**
- Selecionar responsável → exportar só as lideranças dele
- Selecionar município → exportar só aquele município
- Selecionar status → exportar só aquele status

## 5. Módulos Futuros (não no MVP)

- **Dashboard dinâmico** com gráficos (Recharts)
- **Mapa** por município (Leaflet/Mapbox)
- **Agenda** de contatos e follow-ups
- **Notificações** (email/WhatsApp) para cobranças
- **Relatórios automáticos** por período
- **Importação** de planilhas Excel (bulk insert)
- **Auditoria** (audit_log já existe)
- **App mobile** (PWA ou React Native)

## 6. Banco de Dados

### Tabelas em uso (tb_*)
Já mapeadas em `docs/gabinete_360/schema.md`

### Melhorias sugeridas (não bloqueantes pro MVP)
- `tb_acordos_financeiros` ligar por UUID (municipio_id) ao invés de nome
- Limpar tabelas antigas (sem prefixo tb_)
- Adicionar RLS policies para Fase 2

### Views existentes
- `vw_cadastro_completo` — já com labels amigáveis
- `vw_relatorio_final` — versão resumida

## 7. Estrutura do Projeto

```
gabinete-360/
├── src/
│   ├── app/
│   │   ├── layout.tsx          # Layout com auth
│   │   ├── page.tsx            # Dashboard
│   │   ├── login/page.tsx      # Magic link auth
│   │   ├── liderancas/
│   │   │   ├── page.tsx        # Listagem
│   │   │   ├── novo/page.tsx   # Cadastro
│   │   │   └── [id]/page.tsx   # Detalhe/Edição
│   │   ├── acordos/page.tsx    # Acordos financeiros
│   │   ├── admin/
│   │   │   ├── responsaveis/   # CRUD responsáveis
│   │   │   ├── municipios/     # Gestão municípios
│   │   │   └── usuarios/       # Gestão usuários
│   │   └── exportar/page.tsx   # Exportação
│   ├── components/
│   │   ├── ui/                 # shadcn components
│   │   ├── liderancas/         # Componentes específicos
│   │   └── layout/             # Sidebar, header, etc
│   ├── lib/
│   │   ├── supabase.ts         # Client Supabase
│   │   ├── export-pdf.ts       # Lógica PDF
│   │   └── export-xlsx.ts      # Lógica Excel
│   └── types/
│       └── database.ts         # Tipos do schema
├── public/
├── .env.local                  # Supabase URL + anon key
├── next.config.js
├── tailwind.config.js
└── package.json
```

## 8. Configuração Supabase

### Variáveis de ambiente (.env.local)
```
NEXT_PUBLIC_SUPABASE_URL=https://pumsmythpwidsfjhyxhk.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=<anon_key_não_service_role>
SUPABASE_SERVICE_ROLE_KEY=<service_role_key>
```

⚠️ **Importante:** O frontend deve usar `anon_key`, não `service_role`. RLS protege os dados. `service_role` só no server-side quando necessário.

### Auth (Supabase Dashboard)
- Habilitar Email Auth
- Habilitar Magic Link
- Configurar SMTP (ou usar o default do Supabase)
- Site URL: `https://gabinete360.tail-xxxx.ts.net`

## 9. Critérios de Aceite

- [ ] Login via magic link funciona
- [ ] Listar lideranças com busca e filtros
- [ ] Cadastrar nova liderança com dropdowns
- [ ] Editar liderança existente
- [ ] Soft delete de liderança
- [ ] Exportar vw_cadastro_completo em PDF
- [ ] Exportar vw_cadastro_completo em XLSX
- [ ] Exportar vw_relatorio_final em PDF e XLSX
- [ ] Filtros por responsável/município/status na exportação
- [ ] CRUD de responsáveis, status, tipos (admin)
- [ ] Dashboard com cards de resumo
- [ ] Acessível via Tailscale
- [ ] Responsivo (funciona em mobile/tablet)

## 10. Referências

- Schema completo: `docs/gabinete_360/schema.md`
- Supabase project: `pumsmythpwidsfjhyxhk`
- Bitwarden: "Supabase API - Political Dashboard" (service_role key)
- Tailscale: ceopssrv240 (100.107.139.44)
