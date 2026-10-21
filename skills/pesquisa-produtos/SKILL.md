---
name: pesquisa-produtos
description: Pesquisa produtos em sites de e-commerce e catálogo utilizando automação de navegador (Playwright/MCP) e gera relatórios consolidados em Markdown seguindo estratégia progressiva de localização.
---

# Skill: Pesquisa de Produtos em E-commerce / Catálogos

## 1. Objetivo
Pesquisar produtos em um site de vendas ou catálogo online e gerar um resumo estruturado e padronizado em formato Markdown contendo os dados essenciais de cada item encontrado.

---

## 2. Entradas
A skill recebe os seguintes parâmetros para orientar a pesquisa:
- **Site de destino (`url`):** Endereço web do e-commerce ou catálogo (ex.: `https://books.toscrape.com/`).
- **Termos de busca (`query_terms`):** Lista de palavras-chave, nomes de produtos ou títulos a pesquisar. Quando a plataforma estiver em outro idioma, incluir os nomes originais / traduzidos pertinentes.
- **Filtros opcionais (`filters`):** Critérios como faixa de preço, ordenação, categorias preferenciais, idioma ou disponibilidade em estoque.
- **Quantidade de resultados (`limit`):** Limite máximo de produtos a coletar por termo (padrão sugerido: até 5 por termo).

---

## 3. Passo a Passo (Estratégia Progressiva de Busca)

Para localizar produtos de forma eficiente e resiliente a diferentes layouts de site, seguir rigorosamente a ordem hierárquica abaixo:

1. **Acesso Inicial:**
   - Navegar até a URL base do site via ferramenta de automação (ex.: Playwright).
   - Inspecionar a página principal para mapear elementos de navegação e busca.

2. **Estratégia 1 — Campo de Busca:**
   - Verificar se o site possui formulário ou campo de busca (`<input type="search">`, `input[name*="search"]`, etc.).
   - Se existir: digitar o termo de pesquisa, submeter o formulário e analisar os resultados retornados.
   - Coletar até a quantidade solicitada de itens compatíveis.

3. **Estratégia 2 — Categoria Mais Provável (Fallback se não houver busca):**
   - Caso o site não possua campo de busca ou a busca falhe:
   - Identificar o menu de categorias ou taxonomia lateral/superior.
   - Navegar diretamente para a categoria tematicamente mais aderente ao produto (ex.: *Science Fiction* para ficção científica, *Childrens* para infantil).
   - Se o termo puder pertencer a mais de uma categoria correlata, verificar ordenadamente as alternativas mais prováveis.

4. **Estratégia 3 — Paginação Limitada do Catálogo Geral:**
   - Se o produto não for encontrado na categoria ou caso não haja divisão clara por categorias:
   - Percorrer no máximo **5 páginas** do catálogo geral da loja/site.
   - Interromper a paginação imediatamente ao atingir o limite de 5 páginas.

5. **Extração de Dados dos Produtos Encontrados:**
   - Para cada item localizado (até o limite estabelecido por termo), extrair:
     - Título / Nome completo
     - Preço
     - Disponibilidade em estoque
     - Avaliação / Classificação (ex.: estrelas ou nota)
     - Categoria ou seção
     - Link do produto
   - Para o **primeiro resultado** de cada termo: acessar a página de detalhes e gerar um resumo conciso da descrição em português (máximo de 5 linhas).

6. **Tratamento de Não Localizados:**
   - Se o item não for encontrado após as 3 estratégias, registrar formalmente como **"não encontrado"**, discriminando expressamente onde e como as buscas foram executadas.

---

## 4. Regras e Restrições

- **NUNCA inventar resultados:** Não alucinar dados, preços, disponibilidades ou produtos inexistentes. Se o item não constar no site após o processo, declarar estritamente como "não encontrado".
- **Privacidade e Proteção de Dados:** Não coletar nem registrar dados pessoais de vendedores, avaliadores ou compradores (nomes, contatos, endereços, e-mails ou CPF/CNPJ).
- **Respeito aos Limites de Navegação:** Respeitar rigidamente o limite máximo de 5 páginas no catálogo geral para evitar sobrecarga ou loops desnecessários.
- **Tradução e Nomes Originais:** Considerar que sites em idioma estrangeiro podem requerer o termo em sua grafia original ou título em inglês/nativo.
- **Resumo Conforme Especificação:** O resumo da descrição do primeiro resultado não deve exceder 5 linhas.

---

## 5. Formato do Arquivo de Saída (`Markdown`)

O arquivo gerado deve seguir a estrutura abaixo:

```markdown
# Resultado da Pesquisa de Produtos

**Site:** [Nome do Site](URL)  
**Data:** AAAA-MM-DD  
**Status Geral:** X de Y termos localizados

---

## Detalhes da Execução da Busca

- **Campo de busca:** [Disponível e utilizado / Inexistente no site]
- **Categorias consultadas:** [Categorias navegadas]
- **Paginação geral:** [Páginas verificadas até o limite estipulado]

---

## Resultados por Termo

### Termo: <Nome do Termo> (Original: <Nome no idioma do site>)

- **Status:** [Encontrado / Não encontrado]
- **Localização:** [Categoria / Página de Busca / Catálogo Pág. X / Onde foi procurado caso não encontrado]

#### Produto 1 (Principal)
- **Título:** <Nome completo do produto>
- **Preço:** <Valor com moeda original>
- **Disponibilidade:** <Em estoque / Esgotado / Qtd>
- **Avaliação:** <Nota ou estrelas>
- **Categoria:** <Nome da categoria>
- **Link:** [<Título>](<URL>)
- **Resumo da descrição (máx. 5 linhas):**
  > <Resumo em português sintetizando os pontos principais em até 5 linhas>

#### Produto 2..N (se houver até o limite)
- **Título:** ...
- **Preço:** ...
- **Disponibilidade:** ...
- **Avaliação:** ...
- **Categoria:** ...
- **Link:** ...
```
