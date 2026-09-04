# Lucro dos bancos réus — regra de apuração e estado da verificação

**Regra fixada em 03/09/2026 (Edson). Não reabrir caso a caso.**

---

## A regra

O laudo mede a **capacidade econômica do réu** (art. 944 CC, 2ª fase do método
bifásico). A base é:

> **O resultado recorrente / gerencial do exercício, conforme divulgado pela própria
> instituição**, e a peça **nomeia a métrica** que está usando.

### Por que recorrente, e não o contábil

O contábil mistura o estrutural com o eventual — baixas extraordinárias, provisões
atípicas, efeitos fiscais pontuais. Para medir a aptidão de absorver uma condenação,
o que interessa é a capacidade estrutural, não o acidente de um exercício.

O caso do Banco do Brasil ilustra: o ajustado dele caiu **45,4%** de 2024 para 2025
(R$ 37,9 bi → R$ 20,7 bi). Usar o contábil de um ano anômalo retrataria um banco
muito menos capaz do que ele estruturalmente é.

E o réu não pode desdizer o recorrente: foi ele quem apurou, divulgou ao mercado e
apresentou em teleconferência.

### ⚠️ A regra vale na direção que der

Para o Santander o recorrente é **maior** que o contábil (15,6 vs 15,3); para o
Bradesco praticamente empata (24,65 vs 24,55); para o BB é **maior** (20,7 vs 18,2).
Escolher caso a caso o mais favorável ao autor seria **cherry-picking** — e destrói a
credibilidade do laudo inteiro quando percebido. Mesmo princípio da inadimplência da
TREO no LexCalc.

### A peça nomeia a métrica

O laudo **não escreve mais "lucro líquido"** genericamente. Escreve o que a coisa é —
"resultado recorrente gerencial", "lucro líquido ajustado" — e cita o documento da
instituição. Chamar de "lucro líquido" uma métrica gerencial abre flanco em
contestação: o réu responde que aquele não é o lucro líquido da demonstração dele.

Implementado em `metricaLucro()`, `fonteLucro()` e `entidadeLucro()` no `index.html`.
Banco sem os campos `met`/`doc` cai no rótulo genérico — é o sinal de que ainda não
foi conferido.

### Itaú: a Holding fica

O número do Itaú é do **Itaú Unibanco Holding S.A.**, não do Itaú Unibanco S.A. que
costuma ser a ré. Decisão do Edson em 03/09/2026: **mantém a Holding**. Em ação de
dano moral o banco não vai a juízo dizer "não lucramos tantos bilhões, lucramos
tantos bilhões". O laudo passa a **nomear a entidade** (`ent`), então não há
atribuição indevida — quem lê sabe de quem é o número.

---

## Estado da verificação

**4 de 28 conferidos** no RI da própria instituição, em 03/09/2026.

| Banco | Valor | Métrica | Documento |
|---|---|---|---|
| Itaú Unibanco Holding S.A. | R$ 46,8 bi | resultado recorrente gerencial | Ações Itaú em Foco 4T25, nº 89 |
| Banco Bradesco S.A. | R$ 24,652 bi | lucro líquido recorrente | Relatório de Análise Econômica e Financeira 4T25 |
| Banco do Brasil S.A. | R$ 20,685 bi | lucro líquido ajustado | Sumário do Resultado 4T25 |
| Banco Santander (Brasil) S.A. | R$ 15,615 bi | lucro líquido gerencial | Demonstrações Financeiras BRGAAP 4T25 |

### Varredura COSIF — 03/09/2026

Todos os demais foram conferidos contra o **balancete COSIF do BCB** (documento
4010, exercício 2025), pareados por **CNPJ** — nunca por nome: o pareamento
difuso casou Bradesco com Bradesco Financiamentos, Banese com o Estado do RS e
Safra com J.Safra, três "achados" que eram artefato do método, não erro do dado.

É o fallback que a regra autoriza quando a instituição não divulga métrica
recorrente. Validado com **0,00% de desvio** contra três DFs auditadas.

**Só 2 dos 16 conferíveis estavam certos.**

| Banco | Estava | É | Erro |
|---|---|---|---|
| Crefisa | 0,50 bi | 0,16 bi | **+212%** |
| Safra | 1,00 bi | 4,30 bi | −77% |
| C6 Consignado | 0,30 bi | 1,05 bi | −71% |
| Votorantim (BV) | 0,60 bi | 1,86 bi | −68% |
| Daycoval | 0,90 bi | 1,80 bi | −50% |
| Mercantil do Brasil | 0,40 bi | 0,75 bi | −47% |
| BRB | 0,28 bi | 0,51 bi | −45% |
| Agibank | 0,65 bi | 1,09 bi | −40% |
| Banrisul | 1,10 bi | 1,60 bi | −31% |
| BMG | 0,75 bi | 0,56 bi | +34% |
| Pan | 0,82 bi | 0,62 bi | +32% |
| Digimais | 0,053 bi | 0,031 bi | +71% |
| BNP Paribas Brasil | 0,059 bi | 0,077 bi | −24% |
| Caixa | 16,05 bi | 14,58 bi | +10% |
| **Itaú Consignado** | 1,50 bi | **−0,032 bi** | **prejuízo** |

Corretos: Banestes (−5%) e Paraná Banco (−4%). Valores em R$ bilhões.

A **Crefisa** — ré dos dois casos de regressão do LexCalc — tinha o maior erro
percentual da base: o laudo atribuía a ela **três vezes** a capacidade real.

### ⚠️ Itaú Consignado teve prejuízo em 2025

Segundo caso igual ao do Digio. A regra do grupo resolveria, mas o RI do Itaú
**não lista** o Banco Itaú Consignado em "Empresas do grupo" — sem confirmação
institucional do controle, não se aplica.

Ficou com o número real, e `temLucro()` faz o laudo **omitir a seção III**, o
painel econômico e o argumento em âmbar, em vez de imprimir "recuperado em
−X segundos" — que se voltaria contra o autor.

🔴 **Pendente:** confirmar o controle e aplicar a regra do grupo.

### 🔴 Fora do arquivo BANCOS do COSIF

Banese, Banpará, Nubank, Facta e Parati — mais Capital Consig, QI SCD e Via
Certa. São de outros segmentos (financeira, SCD, instituição de pagamento) e
ficam em outro arquivo do mesmo repositório, ainda não localizado. Os valores
desses continuam sem verificação.

---

## Como verificar (caminho que funciona)

Detalhe completo em `reference_ri_bancos_extracao` na memória do projeto.

- **MZiQ** (BB, Bradesco, Santander e a maioria): o RI tem um `<select id="fano">`.
  Trocar o ano por JS e disparar `change`; a tabela mostra os 4 trimestres. A coluna
  do 4T traz links `filemanager-cdn.mziq.com`, que **não bloqueiam curl**. Os links
  `api.mziq.com` também passam.
- **BB**: publica as Demonstrações Contábeis em **.docx**, não PDF — descompactar e
  ler `word/document.xml`. Na DRE, as colunas são
  `Banco Múltiplo (2ºSem, Exercício) | Consolidado (2ºSem, Exercício)`.
- **Itaú**: `download-file/v2/` é barrado por Akamai (403 mesmo com User-Agent de
  navegador). O caminho é navegar o browser **até a URL do PDF** e fazer
  `fetch(location.href)` — same-origin passa pelo WAF — e então extrair com PDF.js
  por `import()` do jsdelivr. ⚠️ Não tente `POST` para fora: o CSP do Itaú bloqueia.
  ⚠️ A coluna 4T25 de "Análise gerencial" está **vazia** no site; o número anual está
  no informativo "Ações Itaú em Foco" 4T25.
- **Santander**: o PDF extrai com **tabs** no lugar de espaços — regex com `\s+`,
  senão `lucro líquido` não casa.
- **Caixa**: não é companhia aberta com RI padrão; ver a nota da memória.
