# 🧭 Checkpoint 1 - Navegação com Jetpack Compose

> Projeto focado na demonstração de fluxo de navegação, interface de usuário e tráfego de dados no Android utilizando a biblioteca Jetpack Compose Navigation.

## 📱 Descrição do Projeto
O **Navegação entre telas** é um aplicativo Android desenvolvido em Kotlin. O sistema é composto por quatro telas principais projetadas para simular um fluxo real de navegação de um usuário:
* Uma tela inicial de acesso (**Login**)
* Um painel central de opções (**Menu**)
* Duas telas de detalhes (**Perfil** e **Pedidos**)

## 🎯 Objetivo da Prova
Demonstrar domínio técnico e entendimento prático do código existente, provando a capacidade de expandir a arquitetura do aplicativo de maneira consistente e de documentar as alterações feitas no fluxo de navegação.

---

## 🛠️ Evoluções Implementadas

Abaixo estão detalhadas as quatro principais implementações feitas na arquitetura de rotas do aplicativo:

### 1. Passagem de parâmetros obrigatórios na tela de Perfil
Foi criada a necessidade de a tela de Perfil receber o nome do usuário para ser renderizada adequadamente.
* A rota no `NavHost` foi alterada para `perfil/{nome}`.
* Na `MenuScreen`, o botão agora aciona a navegação injetando a string desejada (ex: `"perfil/Gabriel Augusto"`).
* O `NavHost` intercepta essa rota, extrai o valor string através do método `it.arguments?.getString("nome")` e repassa para a função composable `PerfilScreen`, que atualiza seu texto de cabeçalho dinamicamente.

### 2. Passagem de parâmetros opcionais na tela de Pedidos
A rota antiga `composable(route = "pedidos")` era estática. Ela apenas abria a tela e não esperava receber nenhum dado.
* A rota foi alterada para aceitar um parâmetro chamado cliente: `route = "pedidos?cliente={cliente}"`. O uso do `?` e do `=` define que este é um parâmetro opcional (*Query Parameter*). Se fosse obrigatório, seria utilizado apenas `/`.
* Como o parâmetro é opcional, é necessário instruir o Compose sobre o que fazer se a tela for chamada sem ele. A propriedade `defaultValue = "Cliente Genérico"` garante que o aplicativo não quebre, injetando esse texto padrão caso a rota base (`"pedidos"`) seja acionada.
* Na chamada da `PedidosScreen`, o valor é extraído usando `it.arguments?.getString("cliente")` e passado adiante.

### 3. Inserção de valor em parâmetro opcional
Antes, ao clicar no botão de pedidos, o `NavController` era instruído a abrir apenas a rota base `"pedidos"`. Com a rota recém-configurada para aceitar um argumento opcional contendo um valor padrão, navegar apenas para `"pedidos"` fazia com que a tela de destino usasse o valor de segurança de forma estática (o usuário veria *"PEDIDOS - Cliente Genérico"*).
* Para comprovar o dinamismo, a instrução de navegação foi atualizada para injetar um valor ativo. O botão agora envia o dado explícito, fazendo com que o sistema sobreponha o `defaultValue` e exiba a informação real na interface.

### 4. Passagem de múltiplos parâmetros entre telas
Para permitir o tráfego de dados complexos, a estrutura da URL foi alterada para receber duas informações distintas simultaneamente.
* A nova rota `route = "perfil/{nome}/{idade}"` exige que os dados venham separados por uma barra `/`. Ambas as informações são obrigatórias neste formato.
* O bloco `arguments = listOf(...)` foi utilizado para informar ao sistema sobre os tipos de dados que estão trafegando.
* A instrução `navArgument("idade") { type = NavType.IntType }` garante que a informação da idade seja rigorosamente convertida e tratada como um número Inteiro (`Int`) no aplicativo, e não como uma simples *String*.
* Por fim, com os métodos `getString("nome")` e `getInt("idade")`, os dois valores são resgatados de forma tipada e enviados para renderização.
