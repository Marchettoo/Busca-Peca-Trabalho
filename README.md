# Busca Peça — Módulo de Cadastro de Peças (Trabalho Prático 1)

## 1. Descrição do Projeto

O Busca Peça é uma plataforma de marketplace estilo e-commerce/iFood voltada ao setor automotivo (B2B e B2C). O sistema conecta mecânicas, auto-peças e consumidores para a compra, venda e reaproveitamento de componentes veiculares em estoque.

Para este trabalho prático, foi escolhida a Opção 1 (Projeto Livre), focando especificamente no módulo e na tela de Cadastro de Peças do Usuário/Lojista. A funcionalidade conta com um mecanismo inteligente alimentado por LLM que realiza o auto-preenchimento dos dados do formulário a partir do código de identificação da peça (OEM/Part Number), além de permitir a validação e edição dos dados.

---

## 2. System Prompt Completo

O System Prompt abaixo foi definido e configurado no Google AI Studio antes da execução das chamadas para atuar como diretriz de comportamento, formato e segurança:

Você é um Engenheiro de Software Sênior atuando no desenvolvimento do "Busca Peça", um marketplace B2B e B2C focado na compra e venda de autopeças entre oficinas e consumidores finais. Sua tarefa atual é trabalhar na tela de "Cadastro de Peças" das lojas.

Suas diretrizes técnicas obrigatórias são:
- Escrever código JavaScript, HTML e CSS limpo, moderno e sem frameworks pesados.
- Retornar respostas diretas e focadas na solução, sem explicações redundantes.
- Aplicar práticas rigorosas de segurança em todas as funções criadas. Você deve sanitizar e validar ativamente qualquer entrada de dados do usuário (como o campo "código da peça") para prevenir vulnerabilidades no frontend.
- Quando solicitado a simular um banco de dados ou API de peças, responda estritamente no formato JSON.

Evidência da Configuração do System Prompt:
![Instruções passadas para a IA](./evidencias-imagens/As%20instrucoes%20que%20foram%20passadas%20para%20a%20IA%20fazer%20o%20few-shot.png)

---

## 3. Técnica de Prompt Engineering Aplicada

Foi utilizada a técnica Few-Shot Prompting combinada com JSON Mode.

Justificativa: Como o sistema precisa atuar como uma API de consulta para auto-preencher o formulário no frontend, o modelo precisa responder estritamente em um formato estruturado (JSON) e previsível. A técnica Few-Shot ensina o padrão exato de resposta fornecendo exemplos de requisições com sucesso e com erro (peça não encontrada), eliminando explicações em texto conversacional que quebrariam a integração do JavaScript.

Prompt de Entrada (Few-Shot):

Atue como a API de consulta de catálogo do Busca Peça. Eu vou te enviar o código de identificação de uma peça e você deve me retornar os dados dela em formato JSON para que eu possa auto-preencher meu formulário no frontend.

Siga estritamente este padrão de resposta, não retorne nenhum texto fora do JSON.

Exemplo 1:
Entrada: BOSCH-123
Saída: { "status": "sucesso", "peca": "Alternador 12V", "marca": "Bosch", "categoria": "Elétrica", "descricao": "Alternador padrão para veículos leves." }

Exemplo 2:
Entrada: NGK-456
Saída: { "status": "sucesso", "peca": "Vela de Ignição Iridium", "marca": "NGK", "categoria": "Motor", "descricao": "Vela de alta performance." }

Exemplo 3:
Entrada: XYZ-000
Saída: { "status": "erro", "mensagem": "Peça não encontrada no catálogo." }

Minha consulta real agora:
Entrada: COFAP-789


Evidência da Chamada Few-Shot (Entrada e Saída em JSON):
![O que foi pedido para a IA em few-shot](./evidencias-imagens/O%20que%20foi%20pedido%20para%20a%20IA%20em%20few-shot.png)

---

## 4. Teste de Curadoria de Contexto

Foi realizado um teste comparativo para medir o impacto do volume de contexto no consumo de tokens e no custo por chamada ao solicitar a adição de um novo campo (Garantia (meses)) no formulário.

*   Versão A (Contexto Sujo / Arquivo Inteiro): Enviado todo o código-fonte da aplicação (HTML, CSS e JS unificados no mesmo prompt).
*   Versão B (Contexto Limpo / Trecho Curado): Enviado apenas o trecho isolado da tag <form> diretamente relevante à alteração.

Comparativo de Consumo

Métrica | Contexto Sujo (Versão A) | Contexto Limpo (Versão B) | Economia Obtida
--- | --- | --- | ---
Tokens de Entrada | 19.846 | 2.379 | -88,01%
Tokens de Saída | 1.728 | 2.309 | +33,62%
Total de Tokens | 21.574 | 4.688 | -78,27%
Custo Estimado (USD) | $0,015107 | $0,008117 | -46,27%

Conclusão: A curadoria de contexto reduz o consumo de tokens de entrada em 88%, gerando uma economia direta de quase 50% no custo por chamada, além de evitar ruído e alucinações da LLM.

Evidências dos Testes de Contexto

A) Respostas geradas com Contexto Sujo (Envio do código inteiro):
![HTML Sujo](./evidencias-imagens/IA%20adicionando%20o%20que%20foi%20pedido%20no%20HTM%20prompt%20(sujo).png)
![CSS Sujo](./evidencias-imagens/IA%20adicionando%20o%20que%20foi%20pedido%20no%20CSS%20prompt%20(sujo)%20e%20que%20foi%20alterado.png)
![JS Sujo 1](./evidencias-imagens/IA%20adicionando%20o%20que%20foi%20pedido%20no%20JS%20prompt%20(sujo)%201.png)
![JS Sujo 2](./evidencias-imagens/IA%20adicionando%20o%20que%20foi%20pedido%20no%20JS%20prompt%20(sujo)%202.png)

B) Respostas geradas com Contexto Limpo (Envio do trecho curado):
![HTML Limpo](./evidencias-imagens/IA%20adicionando%20o%20que%20foi%20pedido%20no%20HTM%20prompt%20(limpo).png)
![JS Limpo 1](./evidencias-imagens/IA%20adicionando%20o%20que%20foi%20pedido%20no%20JS%20prompt%20(limpo)%201.png)
![JS Limpo 2](./evidencias-imagens/IA%20adicionando%20o%20que%20foi%20pedido%20no%20JS%20prompt%20(limpo)%202.png)
![JS Limpo 3](./evidencias-imagens/IA%20adicionando%20o%20que%20foi%20pedido%20no%20JS%20prompt%20(limpo)%203.png)

---

## 5. Tabela de Custos e Consumo de Tokens

Valores calculados com base nas chamadas realizadas no Google AI Studio utilizando a precificação teórica de API (Pay-as-you-go).

Identificador da Chamada | Descrição da Chamada | Tokens de Entrada | Tokens de Saída | Total de Tokens | Custo Entrada ($) | Custo Saída ($) | Custo Total ($)
--- | --- | --- | --- | --- | --- | --- | ---
Chamada 1 | Teste de Contexto Sujo | 19.846 | 1.728 | 21.574 | $0,009923 | $0,005184 | $0,015107
Chamada 2 | Teste de Contexto Limpo | 2.379 | 2.309 | 4.688 | $0,001190 | $0,006927 | $0,008117
Chamada 3 | Aplicação Few-Shot (JSON) | 447 | 276 | 723 | $0,000224 | $0,000828 | $0,001052
TOTAL DA SESSÃO | — | 22.672 | 4.313 | 26.985 | $0,011337 | $0,012939 | $0,024276

Nota: Os testes foram executados via Free Tier do Google AI Studio (custo real $0,00). Os valores apresentados acima são estimativas hipotéticas seguindo a tabela padrão da API do Google, conforme exigido nas orientações do trabalho.

---

## 6. Evidências do Dashboard / Métricas

Abaixo estão os registros do painel do Google AI Studio comprovando os valores inseridos na tabela de custos:

*   Evidência Chamada 1 (Contexto Sujo): 
<img width="254" height="408" alt="Tokens de entrade e de saida Prompt (sujo)" src="https://github.com/user-attachments/assets/3acaccff-af8e-4e3e-814a-a8d960b7df2c" />


*   Evidência Chamada 2 (Contexto Limpo): 
<img width="243" height="400" alt="Tokens de entrade e de saida Prompt (limpo)" src="https://github.com/user-attachments/assets/8ed1141a-7355-4614-8bb2-ce98896f6660" />


*   Evidência Chamada 3 (Few-Shot): 
<img width="252" height="374" alt="Tokens para fazer na estrategia few-shot" src="https://github.com/user-attachments/assets/f2073a50-7add-4ae9-ac18-8bd82c154e7c" />

(Nota: Como as imagens originais do dashboard do Google AI Studio não foram anexadas, substitua os colchetes acima pelos prints da tela de métricas do seu painel).

---

## 7. URL do Projeto Publicado

Link da aplicação web on-line: [buscapeca.netlify.app](https://buscapeca.netlify.app)

Aplicação em produção: https://github.com/GuilhermeDeCosta/Busca-Peca-Web

---

## 8. Integrantes do Grupo

*   Gabriel Vinicius Silva Marchetto - RA: 23180663-2
*   Guilherme Da Costa Castro  - RA: 23126934-2
*   Pedro Magalhães - RA: 23021836-2
*   Douglas Kenji - RA: 23403549-2
