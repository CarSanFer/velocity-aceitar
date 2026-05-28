# Velocity

Controlo de empreitada periódico (semanal/quinzenal) para fiscalização de obra.
Single-file app, bilingue PT/EN, com histórico persistido no Supabase e KPIs em tempo real (execução TN vs. cronograma, desvio, TC aprovados/por aprovar, em dívida, feeling).

## Stack

| Camada | Tecnologia |
|---|---|
| Frontend | HTML + CSS + Vanilla JS (single-file, sem build) |
| Persistência | Supabase (PostgreSQL + REST/PostgREST) |
| Deploy | Vercel (static hosting) |
| Domínio | `velocity.aceitar.pt` |

## Configuração

`URL` e `publishable key` do Supabase estão no objeto `SB` em `index.html` (a publishable key é pública por design — a segurança vive nas políticas RLS).

RLS atual (MVP): CRUD anónimo aberto. Trocar por políticas com `auth.uid()` quando abrir a utilizadores externos.

## Schema

Tabela `public.relatorios` em `db/schema.sql`:
- Cabeçalho indexável: `obra`, `data`, `autor`, `ptipo`, `pini`, `pfim`, `feel`
- Corpo: `payload` JSONB (estado completo do formulário, evolui sem `ALTER TABLE`)
- Trigger de `updated_at`; índices em `obra`, `data desc`, `(obra, pfim desc)`

## Versão

Badge dourado na nav, formato `vYY.MM.DD.HHMM` (hora de Lisboa). Bump a cada deploy.

## Roadmap curto

1. Auth (Supabase Auth) + RLS por utilizador/obra
2. Integração no SEGO via shim no caixote *Fiscalização*
3. Visualização agregada multi-obra (próxima iteração)
