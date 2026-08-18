# Documentação de Prompt Engineering e Inserção de Features

Este documento detalha as configurações de IA e os testes de curadoria de contexto realizados para a implementação do campo "Garantia (meses)" no sistema.

---

## Seção 2. System Prompt Completo

Nesta etapa, configuramos a IA para atuar como um Engenheiro de Software Sênior, estabelecendo regras estritas de formatação, stack de desenvolvimento e segurança (validação e sanitização de dados).

![Configuração das Instruções de Sistema](./As\ instrucoes\ que\ foram\ passadas\ para\ a\ IA\ fazer\ o\ few-shot_3.png)

---

## Seção 3. Técnica de Prompt Engineering Aplicada

Aplicamos a técnica de **Few-Shot Prompting**, fornecendo exemplos claros de entrada e saída esperada. O objetivo foi garantir que a IA simulasse uma API de catálogo, retornando **exclusivamente** o formato JSON necessário para o auto-preenchimento no frontend, sem textos redundantes.

![Prompt Few-Shot e resposta em JSON](./O\ que\ foi\ pedido\ para\ a\ IA\ em\ few-shot_3.png)

---

## Seção 4. Teste de Curadoria de Contexto

Para avaliar a eficiência da IA, realizamos a solicitação de alteração no código utilizando duas abordagens de contexto diferentes. Abaixo está a planilha de resumo dos artefatos gerados:

| Abordagem de Contexto | Arquivo Alvo | Descrição da Alteração | Imagem de Referência |
| :--- | :--- | :--- | :--- |
| **Contexto Sujo** | `index.html` | Inserção do input de Garantia | `HTM prompt (sujo)_3.png` |
| **Contexto Sujo** | `style.css` | Ajuste de Grid (3 colunas) | `CSS prompt (sujo)..._3.png` |
| **Contexto Sujo** | `script.js` | Captura do valor e reset | `JS prompt (sujo) 1 e 2` |
| **Contexto Limpo** | `index.html` | Inserção do input ao lado de Estoque | `HTM prompt (limpo)_3.png` |
| **Contexto Limpo** | `script.js` | Update do payload e fetch API | `JS prompt (limpo) 1, 2 e 3` |

### A) Respostas geradas com Contexto Sujo (Código inteiro)

Neste cenário, enviamos os arquivos completos do frontend para a IA processar e identificar sozinha onde as mudanças deveriam ocorrer.

**1. Alteração no HTML (Seção de Valores):**
![HTML Contexto Sujo](./IA\ adicionando\ o\ que\ foi\ pedido\ no\ HTM\ prompt\ (sujo)_3.png)

**2. Alteração no CSS (Alinhamento de 3 colunas):**
![CSS Contexto Sujo](./IA\ adicionando\ o\ que\ foi\ pedido\ no\ CSS\ prompt\ (sujo)\ e\ que\ foi\ alterado_3.png)

**3. Alteração no JavaScript (Lógica de salvamento e reset):**
![JS Parte 1 Contexto Sujo](./IA\ adicionando\ o\ que\ foi\ pedido\ no\ JS\ prompt\ (sujo)\ 1_3.png)
![JS Parte 2 Contexto Sujo](./IA\ adicionando\ o\ que\ foi\ pedido\ no\ JS\ prompt\ (sujo)\ 2_3.png)

---

### B) Respostas geradas com Contexto Limpo (Trecho curado)

Neste cenário, enviamos apenas as funções e os blocos de interface estritamente necessários. Isso reduziu o uso de tokens e resultou em respostas mais focadas e rápidas.

**1. Alteração no HTML (Grid focado):**
![HTML Contexto Limpo](./IA\ adicionando\ o\ que\ foi\ pedido\ no\ HTM\ prompt\ (limpo)_3.png)

**2. Alteração no JavaScript (Captura de Variável e Payload):**
![JS Parte 1 Contexto Limpo](./IA\ adicionando\ o\ que\ foi\ pedido\ no\ JS\ prompt\ (limpo)\ 1_3.png)
![JS Parte 2 Contexto Limpo](./IA\ adicionando\ o\ que\ foi\ pedido\ no\ JS\ prompt\ (limpo)\ 2_3.png)

**3. Alteração no JavaScript (Feedback de Sucesso e Limpeza do Modal):**
![JS Parte 3 Contexto Limpo](./IA\ adicionando\ o\ que\ foi\ pedido\ no\ JS\ prompt\ (limpo)\ 3_3.png)
