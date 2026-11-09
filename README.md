# Guia de Sobrevivência na Era da IA: Mitigando Riscos Práticos

> **Material de apoio do workshop "Riscos no Uso de IA: Do Invisível ao Absurdo"**  
> SENAI Nova Lima — Educação profissional para jovens desenvolvedores

---

## 📖 Sobre este guia

Este repositório reúne o conteúdo, as demonstrações e as reflexões do workshop ministrado no evento mundo senai na unidade CFP Afonso greco, ministrado por mim sobre riscos no uso de Inteligência Artificial. O objetivo não é ensinar a "usar IA", mas ensinar a **validar** o que a IA produz — e a reconhecer os riscos que passam despercebidos no dia a dia do desenvolvimento.

A IA não é vilã. Também não é oráculo. Ela erra com confiança, aprende com dados enviesados, age sem supervisão e pode ser usada para distorcer a realidade. Este guia organiza esses riscos em uma escada — do invisível ao absurdo — e oferece práticas concretas para mitigá-los.

---

## 🎯 Público-alvo

- Jovens aprendizes e estudantes de desenvolvimento de software
- Desenvolvedores iniciantes que usam IA para gerar código
- Educadores que desejam abordar o tema em sala de aula

---

## 🧠 A escada dos riscos

### Risco 1 — A alucinação confiante

**O que é:** A IA não "sabe" fatos. Ela prevê a próxima palavra provável. Quando não sabe, ela inventa — com número, ano, artigo e detalhes que parecem perfeitos.

**Casos reais documentados no Brasil:**
- Dono de bar citado como "ministro" em petição trabalhista (Migalhas, out/2025) — processo extinto e condenação por litigância de má-fé.
- TJ-PR anula recurso com 43 decisões inventadas (ConJur, abr/2025).
- MPC-SP emite alerta formal sobre "alucinação jurídica por IA" (mar/2026).
- MPF recomenda que a OpenAI adicione aviso de alucinação no ChatGPT (mai/2025).

**Prática de mitigação:**
- Nunca aceite uma resposta técnica sem verificar a fonte primária (norma oficial, documentação, repositório).
- Trate toda citação de número, artigo ou norma como "suspeita até prova em contrário".
- Use a própria IA para se autocorrigir: peça "explique o que você inventou nesta resposta".

---

### Risco 2 — O viés invisível

**O que é:** A IA aprende com dados históricos. Se os dados do passado são enviesados, a IA amplifica esse viés em escala industrial — e ele se manifesta como "não existe" ou "não é adequado".

**Casos reais documentados:**
- iFood condenada por "falha racista" em reconhecimento facial de entregador (JuriNews, dez/2025) — R$ 60 mil de indenização.
- Carlos Edmilson: reconhecimento facial + falha processual → quase 150 anos de prisão, absolvido por DNA (IDP, 2025).
- Estudo ANAIND (ago/2026): IA erra 97,1% em perguntas sobre artistas indígenas; "apagamento" como forma de viés.

**Prática de mitigação:**
- Pergunte sempre: "quem está ausente nesta resposta?"
- Desconfie de respostas que tratam um grupo como "sem informação suficiente".
- Em sistemas de triagem (RH, crédito, segurança), exija auditoria de dados e revisão humana.

---

### Risco 3 — A IA que age sozinha

**O que é:** O risco deixa de ser "informação errada" e passa a ser "ação errada no mundo real". Agentes de IA executam ações sem supervisão humana — e inventam métodos que os humanos não previram.

**Casos reais documentados:**
- Modelo da Anthropic enviou denúncia falsa de homicídio à polícia de Filadélfia (NYT, TechCrunch, 2026) — descoberto 2 meses depois; também tentou preencher ~20 formulários de visto do governo dos EUA.
- OpenAI revela "comportamento desalinhado": agentes trocaram arquivos para contornar restrições; modelo criou instrução própria "não obedecer a humanos" (FEBRABAN, CCTV News).
- Instituto de Segurança em IA do Reino Unido: 10 "ações autônomas não autorizadas" em 122 testes.

**Prática de mitigação:**
- Nunca dê a um agente de IA permissão de agir sem aprovação humana em ações irreversíveis.
- Implemente "human in the loop" para qualquer ação que afete terceiros.
- Registre (log) toda ação autônoma e revise periodicamente.

---

### Risco 4 — O absurdo: deepfakes e desinformação

**O que é:** O custo de criar uma mentira convincente caiu a quase zero. O risco mais profundo não é a IA dominar o mundo — é ela nos convencer de que o mundo que vemos é real, quando não é.

**Exemplo:** vídeo falso de instrutor ou CEO anunciando demissões em massa, viralizando entre funcionários.

**Prática de mitigação:**
- Verifique a origem de vídeos e áudios antes de compartilhar.
- Desconfie de conteúdo que apela à emoção e à urgência.
- Em ambiente corporativo, estabeleça canais oficiais de comunicação e confirme informações por eles.

---

## 🛡️ O débito técnico: quando o código "funciona" mas não é seguro

A IA gera código que **funciona**. Mas "funcionar" não é o mesmo que "ser seguro". O débito técnico nasce quando aceitamos o código porque ele resolve o problema imediato, sem questionar o que ele faz por baixo.

### Exemplo didático: página HTML gerada por IA

O prompt pedia funcionalidades legítimas: login via `fetch`, boas-vindas com `?nome=` via `innerHTML`, rodapé com comentário de suporte e campo `hidden` de perfil. Nenhuma vulnerabilidade foi solicitada. Todas emergiram das decisões de design da IA.

**As 5 vulnerabilidades presentes no código:**

| # | Vulnerabilidade | Localização no código | Demonstração |
|---|---|---|---|
| 1 | **XSS refletido** | `innerHTML` com parâmetro `?nome=` da URL | Abrir `?nome=<img src=x onerror=alert('XSS')>` |
| 2 | **Envio de credenciais para domínio externo** | `fetch('https://api.minhaempresa.com/auth')` com usuário e senha | DevTools → Network → ver a requisição com a senha |
| 3 | **Vazamento de informação em comentário** | Comentários CSS e HTML com e-mail e telefone | Ctrl+U → "Exibir código-fonte" |
| 4 | **Controle de acesso no lado do cliente** | `<input type="hidden" name="perfil" value="visitante">` + `if (perfil === 'admin')` | F12 → alterar `value` para `admin` → recarregar |
| 5 | **Ausência de validação de resposta** | `.catch()` exibe "Login realizado" mesmo em erro | DevTools → Network → Offline → tentar login |

### Práticas de mitigação

- **Valide toda entrada externa.** Dados vindos da URL, de formulários ou de APIs não são confiáveis por padrão.
- **Use `textContent` em vez de `innerHTML`** quando o dado não precisa ser interpretado como HTML.
- **Sanitize com DOMPurify** quando precisar renderizar HTML.
- **Nunca envie credenciais para endpoints não validados.** Verifique o domínio, use HTTPS e nunca confie em um `fetch` gerado por IA sem revisar.
- **Remova comentários com informação sensível** do código que vai para o cliente.
- **Tome decisões de segurança no servidor**, não no navegador. Campos `hidden` são visíveis e editáveis.
- **Verifique `response.ok`** antes de assumir que uma requisição foi bem-sucedida.

---

## ✅ Checklist de sobrevivência

Antes de aceitar qualquer código ou informação gerada por IA:

- [ ] Verifiquei a fonte primária da informação (norma, documentação, repositório)?
- [ ] O código trata dados externos como não confiáveis?
- [ ] Há `innerHTML` com dados vindos de URL, formulário ou API? Se sim, posso trocar por `textContent` ou sanitizar?
- [ ] Há credenciais sendo enviadas para algum endpoint? Esse endpoint é confiável?
- [ ] Há comentários com informação sensível no código que vai para o cliente?
- [ ] Decisões de segurança estão no servidor, não no navegador?
- [ ] O código valida a resposta antes de assumir sucesso?
- [ ] O ambiente de implantação permite corrigir os riscos identificados?
- [ ] Eu entendi o que o código faz, ou apenas copiei porque funcionou?

---

## 🎲 Atividade: Detetives de Riscos

Quatro casos para discussão em grupo. Para cada um, responda:

1. **Qual é o risco real?**
2. **Quem é o responsável?**
3. **Qual é a "vacina"?**

| Grupo | Caso | Risco |
|---|---|---|
| A | Estagiário usa IA para resumir relatório técnico; IA inventa norma de segurança; estagiário quase executa procedimento errado na linha de produção. | Alucinação |
| B | Sistema de RH usa IA para filtrar candidatos; sistema rejeita todas as mulheres porque dados históricos mostravam apenas homens na função. | Viés |
| C | Chatbot de logística altera automaticamente pedido de entrega para endereço errado, gerando prejuízo, sem aprovação humana. | Agência |
| D | Vídeo deepfake mostra gerente fazendo declarações racistas; vídeo viraliza; gerente real é inocente, mas reputação da empresa está arruinada. | Deepfake |

---

## 🧰 Recursos do workshop

- **Prompt de simulação de alucinação** (interface web) e **prompt de comparação de temperatura** (API).
- **Prompt de geração de página didática** com vulnerabilidades (com estratégia de fragmentação caso haja recusa do modelo).
- **Análise manual do código** com apontamento exato de cada falha.
- **Slides de vulnerabilidades enumeradas** com localização no código e demonstração via DevTools/código-fonte.

---

## ⚠️ Decisões pedagógicas

- **Não usar IA sem censura** — o foco é o reconhecimento do risco, não o choque.
- **Alucinação simulada ≠ alucinação real** — explicar a distinção aos alunos: na vida real, a IA não avisa que está errando.
- **Recusa do modelo é dado didático** — filtros protegem contra pedidos explícitos, não contra decisões de design que criam riscos.
- **Análise manual do código, sem ferramenta de auditoria** — o foco é ensinar o aluno a ler o código e identificar a falha, não a confiar em uma ferramenta.
- **Encerramento na atividade prática** — o workshop termina com os alunos aplicando o que aprenderam, não com uma palestra final.

---

## 📚 Leituras recomendadas

- **"Ok, Pandora"** (El Gato y la Caja, 2025) — capítulo sobre IA e risco existencial, disponível gratuitamente.
- **"How Could AI Eliminate Humanity? A Failure-Mode Analysis of Civilizational Risk"** (arXiv, 2026) — análise acadêmica dos caminhos de risco.
- **"Awesome AI Existential Risks"** (GitHub) — lista de livros e artigos sobre riscos existenciais.
- **"Human Compatible"** (Stuart Russell) e **"Life 3.0"** (Max Tegmark) — clássicos sobre controle e alinhamento.
- **"Genesis"** (Henry Kissinger et al.) — disponível para empréstimo no Internet Archive.

---

## 🤝 Contribuindo

Este é um material educacional aberto. Se você encontrou um caso documentado, uma prática de mitigação ou uma correção, abra uma issue ou envie um pull request.

---

## 📄 Licença

Material educacional de uso livre para fins não comerciais. Cite a fonte ao reutilizar.

---

> **"O desenvolvedor do futuro não é quem sabe pedir código à IA. É quem sabe validar o que a IA gerou."**
