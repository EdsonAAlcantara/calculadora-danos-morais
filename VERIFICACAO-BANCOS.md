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

### 🔴 24 bancos ainda NÃO verificados

Caixa, Nu Pagamentos, Itaú Consignado, Bradesco Financiamentos, Banrisul, Safra,
Daycoval, Pan, BMG, Agibank, Votorantim, Crefisa, Facta, Mercantil, Banestes,
Aymoré, C6 Consignado, BRB, Banpará, Parati, Paraná Banco, Banese, BNP Paribas,
Digimais.

**Isso não é formalidade.** O Banco do Brasil estava na tabela com **R$ 27,0 bi** —
valor que não corresponde a nenhuma métrica publicada por ele: erra **+30%** contra a
gerencial e **+49%** contra a contábil. Não há base para presumir que os outros 24
estejam certos.

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
