<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Cantina Aliança - UNISA</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background-color: #e6f0ff;
      color: #003366;
    }
    .container {
      max-width: 400px;
      margin: 50px auto;
      background: white;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    }
    h1, h2 {
      text-align: center;
      color: #0055aa;
    }
    input, select, button {
      width: 100%;
      padding: 10px;
      margin: 10px 0;
      border: 1px solid #ccc;
      border-radius: 5px;
    }
    button {
      background-color: #0055aa;
      color: white;
      font-weight: bold;
      cursor: pointer;
    }
    button:hover {
      background-color: #003f80;
    }
    .hidden {
      display: none;
    }
    ul {
      list-style-type: none;
      padding: 0;
    }
    li {
      margin: 5px 0;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    .remove {
      background-color: #cc0000;
      padding: 5px;
      border: none;
      border-radius: 5px;
      color: white;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <div id="login" class="container">
    <h1>Login - Cantina Aliança</h1>
    <input type="text" id="usuario" placeholder="Usuário/RA">
    <input type="password" id="senha" placeholder="Senha">
    <button onclick="login()">Entrar</button>
    <button onclick="mostrarCadastro()">Cadastrar</button>
  </div>

  <div id="cadastro" class="container hidden">
    <h1>Cadastro</h1>
    <input type="text" id="nome" placeholder="Nome">
    <input type="number" id="idade" placeholder="Idade">
    <input type="text" id="ra" placeholder="RA">
    <select id="campo">
      <option value="UNISA Morumbi">UNISA Morumbi</option>
      <option value="UNISA Adolfo">UNISA Adolfo</option>
    </select>
    <input type="password" id="novaSenha" placeholder="Senha">
    <button onclick="cadastrar()">Finalizar Cadastro</button>
  </div>

  <div id="cardapio" class="container hidden">
    <h1>Cardápio</h1>
    <h2>Lanches</h2>
    <button onclick="adicionarItem('X-Burguer', 10.00)">X-Burguer - R$10,00</button>
    <button onclick="adicionarItem('X-Salada', 12.00)">X-Salada - R$12,00</button>
    <button onclick="adicionarItem('Salgado', 5.00)">Salgado - R$5,00</button>
    <button onclick="adicionarItem('Coxinha', 8.00)">Coxinha - R$8,00</button>
    <h2>Bebidas</h2>
    <button onclick="adicionarItem('Coca 350ml', 5.00)">Coca 350ml - R$5,00</button>
    <button onclick="adicionarItem('Coca Zero 350ml', 5.50)">Coca Zero 350ml - R$5,50</button>
    <button onclick="adicionarItem('Guaraná 350ml', 4.50)">Guaraná 350ml - R$4,50</button>
    <button onclick="adicionarItem('Suco Natural 500ml', 8.00)">Suco Natural 500ml - R$8,00</button>
    <button onclick="adicionarItem('Água 510ml', 4.00)">Água 510ml - R$4,00</button>
    <h2>Itens Selecionados</h2>
    <ul id="listaItens"></ul>
    <h2>Total: R$ <span id="total">0.00</span></h2>
    <button onclick="irPagamento()">Forma de Pagamento</button>
    <button onclick="apagarPedido()" style="background-color: #cc0000">Apagar Pedido</button>
  </div>

  <div id="pagamento" class="container hidden">
    <h1>Pagamento</h1>
    <p>Total da compra: R$ <span id="valorFinal"></span></p>
    <button onclick="pagar('dinheiro')">Dinheiro</button>
    <button onclick="mostrarCartao('debito')">Cartão de Débito</button>
    <button onclick="mostrarCartao('credito')">Cartão de Crédito</button>
    <button onclick="pagarPix()">Pix</button>
  </div>

  <div id="cartao" class="container hidden">
    <h1>Pagamento com Cartão</h1>
    <input type="text" id="numeroCartao" placeholder="Número do Cartão (11 dígitos)">
    <input type="text" id="validade" placeholder="Validade (MM/AA)">
    <input type="text" id="nomeCliente" placeholder="Nome do Cliente">
    <button onclick="finalizarCartao()">Finalizar Pedido</button>
  </div>

  <div id="finalizacao" class="container hidden">
    <h1>Pedido Finalizado</h1>
    <p id="mensagemFinal"></p>
    <p>Sua senha: <strong id="senhaGerada"></strong></p>
  </div>

  <script>
    let total = 0;
    let itens = [];

    function login() {
      const user = document.getElementById('usuario').value;
      const pass = document.getElementById('senha').value;
      if (user === 'ADMIN' && pass === '123456') {
        document.getElementById('login').classList.add('hidden');
        document.getElementById('cardapio').classList.remove('hidden');
      } else {
        alert('Usuário ou senha inválidos.');
      }
    }

    function mostrarCadastro() {
      document.getElementById('login').classList.add('hidden');
      document.getElementById('cadastro').classList.remove('hidden');
    }

    function cadastrar() {
      const nome = document.getElementById('nome').value;
      const idade = document.getElementById('idade').value;
      const ra = document.getElementById('ra').value;
      const campo = document.getElementById('campo').value;
      const senha = document.getElementById('novaSenha').value;

      if (nome && idade && ra && campo && senha) {
        alert('Cadastro realizado com sucesso!');
        document.getElementById('cadastro').classList.add('hidden');
        document.getElementById('login').classList.remove('hidden');
      } else {
        alert('Preencha todos os campos.');
      }
    }

    function adicionarItem(nome, preco) {
      itens.push({ nome, preco });
      total += preco;
      atualizarCarrinho();
    }

    function atualizarCarrinho() {
      const lista = document.getElementById('listaItens');
      lista.innerHTML = '';
      itens.forEach((item, index) => {
        const li = document.createElement('li');
        li.innerHTML = `${item.nome} - R$${item.preco.toFixed(2)} <button class='remove' onclick='removerItem(${index})'>X</button>`;
        lista.appendChild(li);
      });
      document.getElementById('total').innerText = total.toFixed(2);
    }

    function removerItem(index) {
      total -= itens[index].preco;
      itens.splice(index, 1);
      atualizarCarrinho();
    }

    function apagarPedido() {
      itens = [];
      total = 0;
      atualizarCarrinho();
    }

    function irPagamento() {
      document.getElementById('cardapio').classList.add('hidden');
      document.getElementById('pagamento').classList.remove('hidden');
      document.getElementById('valorFinal').innerText = total.toFixed(2);
    }

    function pagar(metodo) {
      document.getElementById('pagamento').classList.add('hidden');
      document.getElementById('finalizacao').classList.remove('hidden');
      document.getElementById('mensagemFinal').innerText = metodo === 'dinheiro' ? 'Vá até o caixa para efetuar o pagamento.' : 'Aguarde sua senha.';
      document.getElementById('senhaGerada').innerText = gerarSenha();
    }

    function mostrarCartao(tipo) {
      document.getElementById('pagamento').classList.add('hidden');
      document.getElementById('cartao').classList.remove('hidden');
    }

    function finalizarCartao() {
      const numero = document.getElementById('numeroCartao').value;
      const validade = document.getElementById('validade').value;
      const nome = document.getElementById('nomeCliente').value;
      if (numero.length === 11 && validade.length === 5 && nome) {
        document.getElementById('cartao').classList.add('hidden');
        document.getElementById('finalizacao').classList.remove('hidden');
        document.getElementById('mensagemFinal').innerText = 'Aguarde sua senha.';
        document.getElementById('senhaGerada').innerText = gerarSenha();
      } else {
        alert('Preencha os dados corretamente.');
      }
    }

    function pagarPix() {
      document.getElementById('pagamento').classList.add('hidden');
      alert('Escaneie o QR Code para pagar.');
      document.getElementById('finalizacao').classList.remove('hidden');
      document.getElementById('mensagemFinal').innerText = 'Aguarde sua senha.';
      document.getElementById('senhaGerada').innerText = gerarSenha();
    }

    function gerarSenha() {
      return Math.floor(1000 + Math.random() * 9000);
    }
  </script>
</body>
</html>
