# Correções Realizadas no BPMN Observantia

## Problemas Identificados no Arquivo Original

### 1. **Gateway_Conformidade sem fluxo "Não"**
- **Problema**: O gateway tinha apenas o fluxo "Sim" definido
- **Impacto**: Processo ficava travado quando documento não estava conforme
- **Correção**: Adicionado fluxo "Não" que envia para Task_NotificarPendencias

### 2. **Fluxos Duplicados**
- **Problema**: Elementos duplicados (Task_AnalisarJuridica, Gateway_Parecer, Task_SolicitarAjustes)
- **Impacto**: Estrutura XML inválida e confusa
- **Correção**: Removidas duplicações, mantendo apenas uma definição de cada elemento

### 3. **Message Flows Incorretos**
- **Problema**: Message flows apontavam de/para elementos errados
- **Impacto**: Comunicação entre pools não funcionava corretamente
- **Correção**: Reconfigurados os 4 message flows principais

### 4. **Fluxos Circulares**
- **Problema**: Alguns fluxos terminavam no mesmo elemento de origem (Flow_F16 duplicado)
- **Impacto**: Loops infinitos no processo
- **Correção**: Removidos fluxos circulares, garantindo progressão linear

### 5. **Incoming/Outgoing Incompletos**
- **Problema**: Tasks sem todos os incoming/outgoing definidos
- **Impacto**: Ferramenta BPMN não consegue validar o modelo
- **Correção**: Todos os elementos agora têm incoming/outgoing completos

## Estrutura Correta do Processo

### POOL CAMPUS

#### Lane: Requisitante
1. **Start Event**: Nova demanda
2. **Task_RegistrarDemanda**: Registrar demanda no Observantia
3. **Task_ComplementarInformacoes**: Complementar informações

#### Lane: Sistema Observantia
4. **Task_ValidarCampos**: Validar campos obrigatórios
5. **Task_AplicarTemplate**: Aplicar template padronizado
6. **Task_ChecklistConformidade**: Executar checklist de conformidade
7. **Task_VerificarLegislacao**: Verificar Lei 14.133/2021 e normas IFPE
8. **Task_GerarRelatorio**: Gerar relatório de conformidade
9. **Task_NotificarPendencias**: Notificar pendências (recebe de Auditoria quando NÃO conforme)
10. **Task_GerarDocFinal**: Gerar PDF assinável
11. **Task_RegistrarHistorico**: Registrar histórico e versionamento

#### Lane: Coordenador de Contratos
12. **Task_ElaborarMinuta**: Elaborar minuta do documento
13. **Task_RevisarDoc**: Revisar documento com base no relatório
14. **Task_CorrigirNaoConf**: Corrigir não conformidades

#### Lane: Gestor / Aprovador
15. **Task_Aprovar**: Aprovar documento (recebe da Auditoria quando conforme)
16. **Gateway_Aprovacao**: Documento aprovado?
    - **Sim** → Task_GerarDocFinal
    - **Não** → Task_CorrigirNaoConf
17. **Task_PublicarSEI**: Publicar no SEI
18. **End Event**: Documento publicado

### POOL REITORIA

#### Lane: Procuradoria Jurídica
1. **Task_AnalisarJuridica**: Realizar análise jurídica (recebe de Campus)
2. **Gateway_Parecer**: Parecer aprovado?
    - **Sim** → Task_VerificarConf (Auditoria)
    - **Não** → Task_SolicitarAjustes

3. **Task_SolicitarAjustes**: Solicitar ajustes via Observantia (envia para Campus)

#### Lane: Auditoria Interna
4. **Task_VerificarConf**: Verificar conformidade final
5. **Gateway_Conformidade**: Documento conforme?
    - **Sim** → Envia para Task_Aprovar (Campus - via message flow)
    - **Não** → Envia para Task_NotificarPendencias (Campus - via message flow)

## Message Flows (Comunicação entre Pools)

1. **Campus → Reitoria**: Task_RevisarDoc → Task_AnalisarJuridica
   - Coordenador envia documento revisado para análise jurídica

2. **Reitoria → Campus**: Task_SolicitarAjustes → Task_CorrigirNaoConf
   - Procuradoria solicita correções ao coordenador

3. **Reitoria → Campus**: Gateway_Conformidade (Sim) → Task_Aprovar
   - Auditoria aprova e envia para gestor aprovar

4. **Reitoria → Campus**: Gateway_Conformidade (Não) → Task_NotificarPendencias
   - Auditoria identifica não conformidades e sistema notifica requisitante

## Fluxo Completo do Processo

```
START
  ↓
Registrar Demanda
  ↓
Validar Campos
  ↓
Aplicar Template
  ↓
Elaborar Minuta
  ↓
Executar Checklist
  ↓
Verificar Legislação
  ↓
Gerar Relatório
  ↓
Revisar Documento
  ↓ (Message Flow)
Realizar Análise Jurídica (REITORIA)
  ↓
Parecer Aprovado?
  ├─ SIM → Verificar Conformidade Final (REITORIA)
  │          ↓
  │        Conforme?
  │          ├─ SIM → (Message Flow) → Aprovar Documento
  │          │                           ↓
  │          │                         Aprovado?
  │          │                           ├─ SIM → Gerar PDF → Publicar SEI → Registrar Histórico → END
  │          │                           └─ NÃO → Corrigir → Revisar (loop)
  │          │
  │          └─ NÃO → (Message Flow) → Notificar Pendências
  │                                       ↓
  │                                     Complementar Informações
  │                                       ↓
  │                                     Corrigir → Revisar (loop)
  │
  └─ NÃO → Solicitar Ajustes
              ↓ (Message Flow)
            Corrigir → Revisar (loop)
```

## Arquivos Gerados

1. **observantia_backup.bpmn**: Backup do arquivo original com problemas
2. **observantia_v2.bpmn**: Primeira tentativa de correção (com alguns problemas)
3. **observantia_final.bpmn**: Versão final corrigida e validada
4. **CORRECOES_BPMN.md**: Este documento explicativo

## Recomendações

1. Use o arquivo **observantia_final.bpmn** como versão oficial
2. Abra em ferramenta BPMN (Camunda Modeler, bpmn.io) para visualização
3. Valide com a equipe se os fluxos estão de acordo com a realidade do processo
4. Considere adicionar eventos intermediários (timers, mensagens) se necessário

## Validações Realizadas

✅ Todos os gateways têm fluxos Sim e Não  
✅ Não há elementos duplicados  
✅ Todos os tasks têm incoming e outgoing  
✅ Message flows conectam corretamente os dois pools  
✅ Não há fluxos circulares incorretos  
✅ Estrutura XML válida  
✅ Lanes organizadas por responsabilidade  
✅ Nomenclatura dos tasks inicia com verbos
