# BPMN AS-IS - Processo de Contratação IFPE

## Descrição
Este arquivo BPMN representa o processo **AS-IS** (como é atualmente) de contratação do IFPE, baseado no diagrama fornecido.

## Estrutura do Processo

### POOL 1: IFPE Campus

#### Lane 1: Demandante/Interessado
**Objetivo**: Identificar e iniciar a demanda de contratação

**Elementos**:
- **Start Event**: Demanda para contrato
- **Task**: Identificar necessidade

#### Lane 2: Interessado/Demandante
**Objetivo**: Levantar requisitos e definir escopo

**Elementos**:
- **Task**: Levantar as necessidades do requisitante
- **Gateway**: Análise das necessidades (ponto de decisão)
- **Task**: Definir produto/serviço e valor consultado

#### Lane 3: Coordenador de contratos
**Objetivo**: Elaborar documentação e coordenar processo

**Elementos**:
- **Task**: Elaborar documentos
- **Send Task**: Enviar para a Reitoria (📧)
- **Task**: Conferir documentos
- **Receive Task**: Revisar pedido (📧)
- **Send Task**: Permitir Edital (📧)
- **End Event**: Publicar Edital

### POOL 2: IFPE Reitoria

#### Lane 1: Auditoria Interna
**Objetivo**: Verificar conformidade dos documentos

**Elementos**:
- **Receive Task**: Verificar conformidade dos documentos (📧)
- **Gateway**: Documentos ok? (ponto de decisão)
  - **SIM**: Segue para Procuradoria
  - **NÃO**: Solicita correções
- **Task**: Solicitar correção ou falta
- **Send Task**: Enviar ao Coordenador (📧)

#### Lane 2: Procuradoria
**Objetivo**: Realizar análise jurídica

**Elementos**:
- **Task**: Realizar análise jurídica
- **Gateway**: Documentos juridicamente ok? (ponto de decisão)
  - **SIM**: Emite parecer
  - **NÃO**: Permite correção e depois emite parecer
- **Task**: Permitir correção
- **Send Task**: Emitir parecer (📧)

## Message Flows (Comunicação entre Pools)

1. **Campus → Reitoria**: 
   - De: Enviar para a Reitoria
   - Para: Verificar conformidade dos documentos
   - Motivo: Submeter documentação para análise

2. **Reitoria → Campus**: 
   - De: Enviar ao Coordenador
   - Para: Conferir documentos
   - Motivo: Solicitar correções/ajustes

3. **Reitoria → Campus**: 
   - De: Emitir parecer
   - Para: Revisar pedido
   - Motivo: Retornar aprovação jurídica

## Fluxo Completo

```
START (Demanda para contrato)
   ↓
Identificar necessidade
   ↓
Levantar necessidades do requisitante
   ↓
[Gateway] Análise das necessidades
   ↓
Definir produto/serviço e valor consultado
   ↓
Elaborar documentos
   ↓
📧 Enviar para a Reitoria
   ↓ (Message Flow)
─────────────────────────────────────────
REITORIA - AUDITORIA INTERNA
   ↓
📧 Verificar conformidade dos documentos
   ↓
[Gateway] Documentos ok?
   ├─ SIM → Realizar análise jurídica (Procuradoria)
   │         ↓
   │      [Gateway] Documentos juridicamente ok?
   │         ├─ SIM → 📧 Emitir parecer
   │         │           ↓ (Message Flow)
   │         │        ─────────────────────────────
   │         │        CAMPUS - COORDENADOR
   │         │           ↓
   │         │        📧 Revisar pedido
   │         │
   │         └─ NÃO → Permitir correção
   │                     ↓
   │                  📧 Emitir parecer
   │                     ↓ (Message Flow)
   │                  ─────────────────────────────
   │                  CAMPUS - COORDENADOR
   │                     ↓
   │                  📧 Revisar pedido
   │
   └─ NÃO → Solicitar correção ou falta
               ↓
            📧 Enviar ao Coordenador
               ↓ (Message Flow)
            ─────────────────────────────
            CAMPUS - COORDENADOR
               ↓
            Conferir documentos
               ↓
            📧 Revisar pedido

─────────────────────────────────────────
CAMPUS - COORDENADOR (continuação)
   ↓
📧 Permitir Edital
   ↓
END (Publicar Edital)
```

## Elementos de Notação

- **Task** (retângulo): Atividade manual
- **Send Task** (📧): Envio de mensagem/documento
- **Receive Task** (📧): Recebimento de mensagem/documento
- **Gateway** (losango): Ponto de decisão (XOR)
- **Message Flow** (linha pontilhada): Comunicação entre pools

## Problemas Identificados no AS-IS

1. **Falta de loop de correção explícito**: Quando há não conformidade, não há fluxo de retorno claramente definido
2. **Gateway sem todos os casos**: O gateway "Análise das necessidades" não tem fluxos alternativos
3. **Falta de paralelismo**: Algumas atividades poderiam ser executadas em paralelo
4. **Sem rastreamento de tempo**: Não há timers ou eventos de tempo
5. **Comunicação manual**: Uso de emails pode causar delays

## Próximos Passos (TO-BE)

Para o processo TO-BE, considerar:
- Sistema automatizado (Observantia) substituindo emails
- Loops de correção explícitos
- Validações automáticas
- Templates padronizados
- Checklists de conformidade
- Histórico e versionamento

## Compatibilidade

✅ **100% compatível com bpmn.io**
✅ Pronto para importação em ferramentas BPMN
✅ Validado segundo BPMN 2.0

## Arquivos Relacionados

- `flow-as-is.bpmn`: Este arquivo AS-IS
- `observantia.bpmn`: Processo TO-BE com sistema Observantia
- `CORRECOES_BPMN.md`: Documentação de correções TO-BE
