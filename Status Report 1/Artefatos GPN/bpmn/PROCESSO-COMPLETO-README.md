# BPMN AS-IS COMPLETO - Processo de Contratação IFPE

## Visão Geral

Este documento descreve o processo **COMPLETO** de contratação do IFPE, desde o planejamento anual (1 ano antes) até a publicação do edital, incluindo todas as etapas de análise, aprovação e validação.

## Estrutura do Processo

### POOL 1: IFPE CAMPUS

#### Lane 1: Setor Requisitante
**Responsabilidade**: Iniciar o processo e formalizar demanda

**Fluxo**:
1. **Start Event** (1 ano antes)
2. **Registrar no PCA** (Plano de Contratação Anual)
3. **Identificar necessidade de contratação**
4. **Elaborar DFD** (Documento de Formalização da Demanda)

#### Lane 2: Equipe de Planejamento
**Responsabilidade**: Fase Interna - preparação técnica do processo

**Fluxo**:
5. **Realizar Estudo Técnico Preliminar**
6. **Realizar Pesquisa de Preço**
7. **Elaborar Edital**
8. **Corrigir documentos** (quando necessário - loop)

#### Lane 3: Coordenação de Compras
**Responsabilidade**: Gerenciar e publicar o processo licitatório

**Fluxo**:
9. **Gerenciar processo licitatório**
10. **Publicar Edital**
11. **End Event** (Edital publicado)

---

### POOL 2: IFPE REITORIA

#### Lane 1: Setor de Compras (DLC - Diretoria de Licitações e Contratos)
**Responsabilidade**: Análise de conformidade dos documentos

**Fluxo**:
1. **📧 Analisar documentação** (Setor de Compras)
2. **Gateway**: Documentos conformes?
   - **SIM** → Segue para decisão sobre Procuradoria
   - **NÃO** → Solicitar ajustes (retorna para Campus)
3. **Gateway**: Precisa Procuradoria?
   - **SIM** → Envia para análise jurídica
   - **NÃO** → Vai direto para aprovação da autoridade
     - Casos: Dispensa de licitação, Inexigibilidade, Parecer referencial

#### Lane 2: Procuradoria Jurídica
**Responsabilidade**: Análise jurídica (quando necessária)

**Fluxo**:
4. **Realizar análise jurídica**
5. **Gateway**: Parecer aprovado?
   - **SIM** → Emitir parecer jurídico
   - **NÃO** → Solicitar ajustes (retorna para Campus)

#### Lane 3: Autoridade Competente
**Responsabilidade**: Aprovação final do processo

**Fluxo**:
6. **Gateway**: Recebeu parecer? (convergência dos fluxos)
7. **Gateway**: Valor > 1 milhão?
   - **SIM** → Reitor aprova (precisa passar pela DLC)
   - **NÃO** → Diretor Geral aprova

---

## Fluxo Completo Detalhado

```
┌─────────────────────────────────────────────────────────────────┐
│ FASE 1: PLANEJAMENTO ANUAL (1 ano antes)                       │
└─────────────────────────────────────────────────────────────────┘

START (1 ano antes)
   ↓
Registrar no PCA (Plano de Contratação Anual)
   ↓
Identificar necessidade de contratação
   ↓
Elaborar DFD (Documento de Formalização da Demanda)

┌─────────────────────────────────────────────────────────────────┐
│ FASE 2: FASE INTERNA - PREPARAÇÃO TÉCNICA                      │
└─────────────────────────────────────────────────────────────────┘

Realizar Estudo Técnico Preliminar
   ↓
Realizar Pesquisa de Preço
   ↓
Elaborar Edital
   ↓ (Message Flow)
─────────────────────────────────────────────────────────────────

┌─────────────────────────────────────────────────────────────────┐
│ FASE 3: ANÁLISE REITORIA - SETOR DE COMPRAS (DLC)              │
└─────────────────────────────────────────────────────────────────┘

📧 Analisar documentação (Setor de Compras)
   ↓
[Gateway] Documentos conformes?
   ├─ SIM → [Gateway] Precisa Procuradoria?
   │         ├─ SIM → Realizar análise jurídica
   │         │         ↓
   │         │      [Gateway] Parecer aprovado?
   │         │         ├─ SIM → 📧 Emitir parecer jurídico
   │         │         │           ↓ (Message Flow)
   │         │         │        [Gateway] Recebeu parecer
   │         │         │           ↓
   │         │         │        [Gateway] Valor > 1 milhão?
   │         │         │           ├─ SIM → Aprovar (Reitor)
   │         │         │           │         ↓ (Message Flow)
   │         │         │           │      ─────────────────────
   │         │         │           │      CAMPUS - COORDENAÇÃO
   │         │         │           │         ↓
   │         │         │           │      Gerenciar processo
   │         │         │           │         ↓
   │         │         │           │      Publicar Edital
   │         │         │           │         ↓
   │         │         │           │      END
   │         │         │           │
   │         │         │           └─ NÃO → Aprovar (Diretor Geral)
   │         │         │                     ↓ (Message Flow)
   │         │         │                  ─────────────────────
   │         │         │                  CAMPUS - COORDENAÇÃO
   │         │         │                     ↓
   │         │         │                  Gerenciar processo
   │         │         │                     ↓
   │         │         │                  Publicar Edital
   │         │         │                     ↓
   │         │         │                  END
   │         │         │
   │         │         └─ NÃO → 📧 Solicitar ajustes
   │         │                   ↓ (Message Flow)
   │         │                ─────────────────────────────
   │         │                CAMPUS - EQUIPE PLANEJAMENTO
   │         │                   ↓
   │         │                Corrigir documentos
   │         │                   ↓
   │         │                Elaborar Edital (loop)
   │         │
   │         └─ NÃO (Dispensa/Inexigibilidade/Parecer Referencial)
   │               ↓
   │            [Gateway] Recebeu parecer
   │               ↓
   │            [Gateway] Valor > 1 milhão?
   │               ├─ SIM → Aprovar (Reitor) → Gerenciar → Publicar → END
   │               └─ NÃO → Aprovar (Diretor Geral) → Gerenciar → Publicar → END
   │
   └─ NÃO → 📧 Solicitar ajustes
               ↓ (Message Flow)
            ─────────────────────────────
            CAMPUS - EQUIPE PLANEJAMENTO
               ↓
            Corrigir documentos
               ↓
            Elaborar Edital (loop)
```

---

## Regras de Negócio Importantes

### 1. Plano de Contratação Anual (PCA)
- **Quando**: 1 ano antes da contratação
- **Onde**: Sistema de compras
- **Responsável**: Setor Requisitante

### 2. Fase Interna
- **Responsável**: Pode ser:
  - Próprio requisitante
  - Setor de contratações
  - Ordenador de despesa
- **Equipe**: Equipe de planejamento trabalha a partir do DFD

### 3. Setor de Compras (DLC)
- **Função**: O que chamávamos de "Auditoria Interna" é na verdade o **Setor de Compras**
- **Responsabilidade**: Análise de conformidade dos documentos

### 4. Procuradoria Jurídica
- **Quando é necessária**: 
  - É fase **anterior** à aprovação da Autoridade Competente
  - Análise jurídica obrigatória na maioria dos casos
  
- **Quando NÃO é necessária** (Orientação AGU):
  - ✓ Dispensa de licitação
  - ✓ Inexigibilidade
  - ✓ Parecer referencial

### 5. Autoridade Competente

#### Quem pode ser:
- **Reitor** (contratos > 1 milhão)
- **Diretor Geral** (contratos ≤ 1 milhão)

#### Regras:
- **Contratos > 1 milhão**: SEMPRE aprovados pelo Reitor
- **Reitor só aprova**: Se o processo passar pela DLC
- **DLC**: Diretoria de Licitações e Contratos

### 6. Coordenador de Contratos
- **Quando surge**: APÓS a licitação ter sido vencida
- **Função**: Receber e gerenciar o contrato pós-licitação
- **Nota**: Não aparece neste BPMN AS-IS (foco é até publicação do edital)

---

## Message Flows (Comunicação entre Pools)

1. **Campus → Reitoria**: Elaborar Edital → Analisar Setor Compras
   - Envio da documentação para análise

2. **Reitoria → Campus**: Solicitar Ajustes → Corrigir Documentos
   - Retorno para correções quando há não conformidades

3. **Reitoria → Reitoria**: Gateway Procuradoria → Análise Jurídica
   - Encaminhamento interno quando precisa análise jurídica

4. **Reitoria → Reitoria**: Emitir Parecer → Gateway Aprovação
   - Retorno do parecer jurídico para aprovação da autoridade

5. **Reitoria → Campus**: Gateway Valor → Gerenciar Processo
   - Após aprovação, retorna para Campus publicar

---

## Gateways (Pontos de Decisão)

### Gateway 1: Documentos Conformes?
- **Local**: Setor de Compras (DLC)
- **SIM**: Avança para decisão sobre Procuradoria
- **NÃO**: Solicita ajustes e retorna para Campus

### Gateway 2: Precisa Procuradoria?
- **Local**: Setor de Compras (DLC)
- **SIM**: Envia para análise jurídica
- **NÃO**: Pula Procuradoria (casos: dispensa, inexigibilidade, parecer referencial)

### Gateway 3: Parecer Aprovado?
- **Local**: Procuradoria Jurídica
- **SIM**: Emite parecer e segue para aprovação
- **NÃO**: Solicita ajustes e retorna para Campus

### Gateway 4: Recebeu Parecer?
- **Local**: Autoridade Competente
- **Função**: Convergência dos fluxos (com ou sem Procuradoria)

### Gateway 5: Valor > 1 milhão?
- **Local**: Autoridade Competente
- **SIM**: Reitor aprova (via DLC)
- **NÃO**: Diretor Geral aprova

---

## Diferenças entre AS-IS e TO-BE

### Problemas no AS-IS atual:

1. **Processo manual e demorado**
   - Trocas de emails e documentos físicos
   - Sem rastreamento automatizado

2. **Loops de correção custosos**
   - Retornos frequentes para ajustes
   - Sem validações prévias

3. **Falta de transparência**
   - Difícil acompanhar status do processo
   - Sem histórico centralizado

4. **Duplicação de esforços**
   - Retrabalho em documentação
   - Análises redundantes

### Melhorias esperadas no TO-BE (com Observantia):

✅ Sistema automatizado de validações  
✅ Templates padronizados  
✅ Checklist de conformidade automático  
✅ Verificação Lei 14.133/2021 integrada  
✅ Rastreamento e histórico completo  
✅ Notificações automáticas  
✅ Redução de loops de correção  

---

## Estatísticas do Processo

- **Tempo total**: ~12-18 meses (do PCA até publicação)
- **Etapas Campus**: 10 atividades + 3 decisões
- **Etapas Reitoria**: 7 atividades + 5 decisões
- **Message Flows**: 5 comunicações entre pools
- **Loops de correção**: 2 principais (conformidade e parecer)

---

## Arquivos Relacionados

- `flow-as-is-completo.bpmn`: Este processo completo
- `observantia.bpmn`: Processo TO-BE com sistema Observantia
- `CORRECOES_BPMN.md`: Documentação de correções TO-BE

---

## Compatibilidade

✅ **100% compatível com bpmn.io**  
✅ **BPMN 2.0 compliant**  
✅ **Todas as regras de negócio documentadas**  
✅ **Gateways com ambos os fluxos (SIM/NÃO)**  
✅ **Message flows corretamente implementados**
