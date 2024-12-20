Documentação do Código
O código fornecido implementa um Sistema de Estacionamento simples utilizando HTML, CSS e JavaScript para registrar veículos que entram e saem do estacionamento, salvar dados no localStorage, e exibir estatísticas e informações sobre o estacionamento. A interface inclui modais para entrada e saída de veículos, pesquisa de registros e exibição de dados em uma tabela.

Estrutura do Código
HTML
A estrutura HTML define os elementos de interface, como:

Botões para ações como registrar entrada, saída, excluir todos os registros e salvar os dados.
Modais para registrar a entrada e saída de veículos.
Campo de pesquisa e tabela para exibir informações dos veículos estacionados.
Estatísticas para mostrar o total de veículos e os ganhos totais.
CSS
A estilização inclui:

Layout responsivo com centralização de elementos.
Estilo de botões, tabelas e modais para melhorar a interação do usuário.
Design limpo e organizado, com foco na usabilidade.
JavaScript
A parte do JavaScript gerencia as funcionalidades interativas e o armazenamento dos dados:

Armazenamento no localStorage: Utiliza localStorage para salvar os dados dos veículos e os ganhos totais. Isso permite que as informações sejam persistentes entre sessões do navegador.
Funções principais:
saveData(): Salva os dados no localStorage.
showModal(), closeModal(): Exibe e fecha os modais de entrada e saída de veículos.
registerEntry(), registerExit(): Registra a entrada e saída de veículos, calculando o tempo e o valor a ser pago.
updateTable(): Atualiza a tabela HTML para exibir os dados mais recentes.
clearAll(): Exclui todos os registros.
saveToFile(): Salva os dados em um arquivo de texto.
Exemplo Explicativo - Como Funciona o Armazenamento e as Vantagens
O sistema usa o localStorage, que permite salvar dados no navegador do usuário de forma persistente. Isso significa que mesmo se o usuário fechar o navegador, as informações ainda estarão disponíveis quando ele retornar à página.

Vantagens do localStorage
Persistência de Dados: Os dados são mantidos mesmo após o fechamento do navegador ou atualização da página.
Facilidade de Implementação: Não é necessário configurar um servidor para armazenamento, já que o localStorage é armazenado localmente no navegador.
Acessibilidade Rápida: O acesso ao localStorage é feito diretamente pelo JavaScript, o que permite que o sistema seja rápido e eficiente para armazenar e recuperar dados.
Exemplo de Uso - Como Salvar, Excluir e Recuperar Dados
1. Salvar Dados no localStorage
No código, a função saveData() é responsável por salvar os dados dos veículos e os ganhos totais no localStorage:

javascript
Copiar código
function saveData() {
    localStorage.setItem("vehicles", JSON.stringify(vehicles));
    localStorage.setItem("totalEarnings", totalEarnings.toFixed(2));
}
Isso converte a lista de veículos para uma string JSON e a salva no localStorage. O valor de ganhos totais é salvo de forma numérica.

2. Recuperar Dados do localStorage
Ao carregar a página, o código recupera os dados salvos no localStorage:

javascript
Copiar código
let vehicles = JSON.parse(localStorage.getItem("vehicles")) || [];
let totalEarnings = parseFloat(localStorage.getItem("totalEarnings")) || 0;
Isso garante que, mesmo após o fechamento da página, os dados anteriores serão recuperados e mostrados na interface.

3. Excluir Dados do localStorage
A função clearAll() é usada para excluir todos os registros e limpar o localStorage:

javascript
Copiar código
function clearAll() {
    if (confirm("Deseja realmente excluir todos os registros?")) {
        vehicles = [];
        totalEarnings = 0;
        saveData(); // Re-salva os dados no localStorage após a exclusão
        updateTable();
    }
}
Aqui, todos os registros de veículos são removidos, e os dados são salvos novamente no localStorage para garantir que a tabela seja limpa.

4. Imprimir os Dados em HTML
A função updateTable() atualiza a tabela HTML com os dados mais recentes dos veículos:

javascript
Copiar código
function updateTable() {
    const tableBody = document.getElementById("vehicleTable");
    tableBody.innerHTML = vehicles
        .map((v) => {
            const elapsedTime =
                v.status === "Estacionado"
                    ? formatElapsedTime(Date.now() - v.entryTime)
                    : "-";
            const currentValue =
                v.status === "Estacionado"
                    ? ((Date.now() - v.entryTime) / (1000 * 60 * 60)) * v.rate
                    : v.earnings.toFixed(2);

            return `
                <tr>
                    <td>${v.plate}</td>
                    <td>${v.model}</td>
                    <td>${v.client}</td>
                    <td>${elapsedTime}</td>
                    <td>R$ ${currentValue}</td>
                    <td>${v.status}</td>
                </tr>
            `;
        })
        .join("");
}
Aqui, cada veículo é exibido em uma linha da tabela, com os dados de placa, modelo, cliente, tempo de estacionamento, valor a pagar e status (se ainda está estacionado ou já saiu).

5. Exemplo de Salvamento para Arquivo
A função saveToFile() permite ao usuário baixar os dados como um arquivo de texto:

javascript
Copiar código
function saveToFile() {
    const data = {
        vehicles,
        totalVehicles: vehicles.filter((v) => v.status === "Estacionado").length,
        totalEarnings: totalEarnings.toFixed(2),
    };
    const blob = new Blob([JSON.stringify(data, null, 2)], { type: "text/plain" });
    const link = document.createElement("a");
    link.href = URL.createObjectURL(blob);
    link.download = "dados.txt";
    link.click();
}
Ao clicar no botão "Salvar", o navegador cria um arquivo de texto contendo os dados do estacionamento, que pode ser baixado pelo usuário.

Conclusão
Esse sistema simples de estacionamento é um exemplo de como utilizar JavaScript e o localStorage para criar aplicativos interativos com dados persistentes. O uso do localStorage permite que os dados do estacionamento sejam salvos entre sessões de navegador, oferecendo uma experiência consistente ao usuário.

![tala01](https://github.com/user-attachments/assets/3a0b9f85-0986-4a29-a71a-84e00143e65a)

![tela02](https://github.com/user-attachments/assets/1ddb93d5-2ffb-467e-bb10-ace4799131fe)

![TELA03](https://github.com/user-attachments/assets/25b66a01-ef2e-4662-aa05-9fd5265015b8)

![TELA04](https://github.com/user-attachments/assets/19e00c7f-cf3a-4ca4-8190-682ae65ad132)



<h1>Aqui está um outro exemplo simples de como salvar, excluir, editar e imprimir dados no localStorage</h1>
Vamos trabalhar com um sistema que armazena o nome e a idade de uma pessoa. A seguir, o código inclui funcionalidades para adicionar, editar e excluir esses dados, além de exibir as informações salvas na página HTML.

Exemplo de Código:
html
Copiar código
<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Exemplo de LocalStorage</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 20px;
      background-color: #f4f4f9;
    }
    h1 {
      text-align: center;
    }
    .container {
      max-width: 500px;
      margin: 0 auto;
      background: white;
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    }
    .input-group {
      margin-bottom: 15px;
    }
    input[type="text"], input[type="number"] {
      width: 100%;
      padding: 10px;
      border-radius: 5px;
      border: 1px solid #ccc;
    }
    button {
      padding: 10px 15px;
      border: none;
      border-radius: 5px;
      background-color: #4CAF50;
      color: white;
      font-size: 16px;
      cursor: pointer;
    }
    button.delete {
      background-color: #f44336;
    }
    .output {
      margin-top: 20px;
    }
  </style>
</head>
<body>

  <h1>Gerenciamento de Dados</h1>
  <div class="container">
    <div class="input-group">
      <label for="name">Nome:</label>
      <input type="text" id="name" placeholder="Digite seu nome">
    </div>
    <div class="input-group">
      <label for="age">Idade:</label>
      <input type="number" id="age" placeholder="Digite sua idade">
    </div>
    <button onclick="saveData()">Salvar</button>
    <button onclick="editData()">Editar</button>
    <button class="delete" onclick="deleteData()">Excluir</button>

    <div class="output">
      <h3>Dados Salvos:</h3>
      <p id="outputName">Nome: Não definido</p>
      <p id="outputAge">Idade: Não definida</p>
    </div>
  </div>

  <script>
    // Função para salvar os dados no localStorage
    function saveData() {
      const name = document.getElementById("name").value.trim();
      const age = document.getElementById("age").value.trim();

      if (name && age) {
        localStorage.setItem("name", name);
        localStorage.setItem("age", age);
        alert("Dados salvos com sucesso!");
        updateData();
      } else {
        alert("Por favor, preencha ambos os campos!");
      }
    }

    // Função para editar os dados no localStorage
    function editData() {
      const name = document.getElementById("name").value.trim();
      const age = document.getElementById("age").value.trim();

      if (name && age) {
        localStorage.setItem("name", name);
        localStorage.setItem("age", age);
        alert("Dados atualizados com sucesso!");
        updateData();
      } else {
        alert("Por favor, preencha ambos os campos!");
      }
    }

    // Função para excluir os dados do localStorage
    function deleteData() {
      localStorage.removeItem("name");
      localStorage.removeItem("age");
      alert("Dados excluídos!");
      updateData();
    }

    // Função para atualizar os dados na página HTML
    function updateData() {
      const name = localStorage.getItem("name");
      const age = localStorage.getItem("age");

      if (name && age) {
        document.getElementById("outputName").textContent = "Nome: " + name;
        document.getElementById("outputAge").textContent = "Idade: " + age;
      } else {
        document.getElementById("outputName").textContent = "Nome: Não definido";
        document.getElementById("outputAge").textContent = "Idade: Não definida";
      }
    }

    // Inicializa a página com os dados do localStorage
    updateData();
  </script>
</body>
</html>
Explicação:
HTML:

Criamos um formulário simples com campos para nome e idade e botões para salvar, editar e excluir os dados.
A área de output exibe os dados salvos ou uma mensagem indicando que não há dados definidos.
CSS:

O estilo é básico e centrado para dar uma aparência limpa e organizada. A página tem botões estilizados para as ações de salvar, editar e excluir.
JavaScript:

saveData(): Salva os dados no localStorage com as chaves "name" e "age".
editData(): Atualiza os dados no localStorage com novos valores.
deleteData(): Remove os dados do localStorage.
updateData(): Atualiza a exibição dos dados na página HTML, buscando-os diretamente do localStorage.
Ao carregar a página, a função updateData() é chamada para exibir os dados salvos no navegador (se existirem).
Funcionalidade:
Quando o usuário preenche os campos e clica no botão "Salvar", os dados são armazenados no localStorage.
O botão "Editar" atualiza os dados armazenados no localStorage.
O botão "Excluir" remove os dados do localStorage.
Os dados salvos são sempre exibidos na área de output na página.
Como Funciona o Armazenamento:
Salvar: Quando o usuário clica em "Salvar", os dados são convertidos em strings e armazenados no localStorage usando localStorage.setItem().
Editar: Os dados são atualizados no localStorage se o usuário quiser modificar as informações.
Excluir: A função deleteData() remove os dados do localStorage usando localStorage.removeItem().
Esse exemplo é simples, mas serve como base para aplicações mais complexas onde você precisa persistir dados localmente.






