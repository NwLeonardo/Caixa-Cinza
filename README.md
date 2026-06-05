# Introdução
Esta documentação detalha a execução de testes de integração e testes de Caixa Cinza em uma API de autenticação em nuvem. Utilizou-se a infraestrutura do Supabase como backend-as-a-service (BaaS) e o Postman Desktop para simulação, envio de requisições e validação das respostas HTTP.

# Objetivo da Atividade
Compreender e aplicar testes de caixa cinza em APIs de autenticação, avaliando o comportamento do endpoint, validação de status HTTP, análise de payloads JSON, headers de segurança e tratamento de erros através de cenários de sucesso e falha.

# Configuração do Supabase
O ambiente foi configurado através da criação de um novo projeto na plataforma Supabase.
* **Autenticação:** Foi habilitado o provedor nativo de e-mail e senha.
* **Criação de Utilizador:** Um utilizador válido foi gerado diretamente no painel do Supabase para servir como massa de dados para o teste.
* **Objetivo:** Estabelecer um backend real e funcional, obtendo credenciais exclusivas (API URL e API Key anon) para responder às requisições de forma autêntica.

**Evidências:**
![Painel Supabase](prints/print1_supabase.png)
![Autenticação Habilitada](prints/print2_providers.png)
![Utilizador Criado](prints/print3_user.png)
![Credenciais API](prints/print4_api.png)

# Configuração do Postman
A ferramenta foi configurada utilizando o isolamento de dados sensíveis em variáveis de ambiente.
* **Workspace:** Criado um workspace dedicado, nomeado "Atividade Testes".
* **Environment:** Criado o ambiente "Supabase Env".
* **Finalidade das Variáveis:** Garantir a segurança das credenciais (API Key e senhas) e facilitar a manutenção da URL base. O Postman injeta essas variáveis nas requisições no momento do disparo.

**Evidências:**
![Postman Workspace](prints/print5_workspace.png)
![Postman Variáveis](prints/print6_env.png)

# Configuração das Requisições
Foi elaborada uma requisição HTTP para simular um login via client-side.
* **Endpoint:** `/auth/v1/token?grant_type=password`
* **Método:** `POST`
* **Finalidade:** Solicitar a validação de credenciais e obter um JWT (Access Token) válido.

**Exemplo de Header utilizado:**
| Header | Valor |
| :--- | :--- |
| `apikey` | `{{api_key}}` |
| `Content-Type` | `application/json` |

**Exemplo do Body JSON utilizado:**
{
  "email": "{{email}}",
  "password": "{{password}}"
}

**Evidência da requisição montada:**
![Postman Requisição](prints/print7_req.png)

# Execução dos Testes
Foram executados 5 cenários focados em validar a resposta da API frente a diferentes inputs inseridos no Body JSON.

**Tabela de Cenários:**
| Cenário | Entrada Utilizada | Resultado Esperado | Resultado Obtido | Status |
| :--- | :--- | :--- | :--- | :--- |
| Login válido | Utilizador e senha corretos | Status 200 + access token | Status 200 OK | OK |
| Senha incorreta | Senha "000000" | Erro de autenticação | Erro 400 (Invalid login) | OK |
| Utilizador inexistente | E-mail falso | Acesso negado | Erro 400 (Invalid login) | OK |
| Campos vazios | Body JSON vazio {} | Erro de validação | Erro 400 (Bad Request) | OK |
| Credenciais inválidas | E-mail sem "@" | Mensagem de erro | Erro 400 (Validation) | OK |

**Evidências dos Testes:**
![Teste 200 OK - Sucesso](prints/print8_sucesso.png)
![Teste Erro - Senha Incorreta](prints/print9_erro_senha.png)
![Teste Erro - Utilizador Inexistente](prints/print10_erro_user.png)
![Teste Erro - Campos Vazios](prints/print11_erro_vazio.png)
![Teste Erro - E-mail Inválido](prints/print12_erro_email.png)

# Registro dos Testes
Os testes foram formalmente registrados numa planilha de acompanhamento.
* **Importância:** A documentação formal garante a rastreabilidade da qualidade do software, permitindo que a equipa de desenvolvimento reproduza exatamente as condições testadas.
* **Falhas Identificadas:** Não houve falhas sistémicas (bugs) na API; ela tratou todos os cenários de erro provocados intencionalmente de forma correta, retornando o Status 400.

**Evidência do Registro:**
![Planilha Preenchida](prints/print13_planilha.png)

# Resultados Obtidos
A API do Supabase apresentou um comportamento consistente. Para credenciais corretas, gerou o Access Token adequadamente. Para entradas incorretas, protegeu o sistema retornando mensagens de erro padronizadas e sem expor informações sensíveis do servidor.

# Conclusão
* **Execução:** Todos os cenários obrigatórios foram executados e documentados com sucesso.
* **Autenticação:** O fluxo funcionou conforme os padrões de segurança esperados para APIs REST.
* **Dificuldades:** A principal dificuldade inicial esteve no mapeamento correto das variáveis de ambiente e na garantia de que o utilizador de teste estava com o status de e-mail confirmado no backend.
* **Melhorias:** O processo poderia ser otimizado utilizando a aba de "Tests" do Postman com scripts em JavaScript para automatizar a validação dos *Status Codes*, evitando a verificação manual.
* **Importância do Caixa Cinza:** A atividade demonstrou que, ao conhecer os inputs esperados (JSON) e a estrutura dos headers, é possível testar a resiliência e a segurança de uma API de forma profunda, mesmo sem acesso direto ao código-fonte interno do Supabase.
