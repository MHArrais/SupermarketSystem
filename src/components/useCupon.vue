<template>
  <div id="app">
    <div class="items-list">
      <div class="logo-container">
        <img src="../assets/logo.jpg" alt="Logo do Supermercado">
      </div>
      <h2>Lista de Itens Disponíveis</h2>
      <ul>
        <li v-for="item in mockItems" :key="item.id">
          ID: {{ item.id }} - {{ item.descricao }}
        </li>
      </ul>
    </div>
    <div class="item-info">
  <h2>Informações do Item</h2>
  <input 
    type="number" 
    v-model="itemId" 
    placeholder="Digite o ID do item"
  >
  <button @click="buscarItem">Buscar</button>
  
  <!-- Exibe informações se o item for encontrado -->
    <div v-if="itemEncontrado">
      <p>Descrição: {{ itemEncontrado.descricao }}</p>
      <p>Valor: R$ {{ itemEncontrado.valor.toFixed(2) }}</p>
      <p>Unidade: {{ itemEncontrado.unidade }}</p>
      
      <!-- Botão para adicionar item à lista -->
      <button @click="adicionarItem">Adicionar à Lista</button>
    </div>

    <!-- Exibe mensagem de erro se o item não for encontrado -->
    <p v-if="mostrarErro" class="not-found">Item não encontrado.</p>  
    </div>

  <!-- Lista de itens adicionados -->
  <div class="cart">
      <h2>Carrinho de Compras</h2>
      <ul v-if="listaDeItens.length > 0">
        <li v-for="(item, index) in listaDeItens" :key="index">
          <p>{{ item.descricao }}</p>
          <p>Valor Unitário: R$ {{ item.valor.toFixed(2) }}</p>
          <p>Quantidade: 
            <button @click="diminuirQuantidade(index)">-</button>
            {{ item.quantidade }}
            <button @click="aumentarQuantidade(index)">+</button>
          </p>
          <p>Subtotal: R$ {{ (item.valor * item.quantidade).toFixed(2) }}</p>
        </li>
      </ul>
      <p v-else>O carrinho está vazio.</p>
    </div>

</div>
</template>

<script>
export default {
  name: "SupermarketSystem",
  data() {
    return {
      mockItems: [
        { id: 1, descricao: "Arroz 5kg", valor: 25.00, unidade: "pacote" },
        { id: 2, descricao: "Feijão 1kg", valor: 8.50, unidade: "pacote" },
        { id: 3, descricao: "Açúcar 2kg", valor: 6.75, unidade: "pacote" },
        { id: 4, descricao: "Óleo 900ml", valor: 10.00, unidade: "unidade" },
      ],
      itemId: null,
      itemEncontrado: null,
      mostrarErro: false, // Novo estado para controlar a exibição da mensagem de erro
      listaDeItens: [], // Lista de itens adicionados
    };
  },
  methods: {
    buscarItem() {
      this.itemEncontrado = this.mockItems.find(item => item.id === Number(this.itemId)) || null;
      this.mostrarErro = !this.itemEncontrado; // Mostra erro somente se o item não foi encontrado
    },

    adicionarItem() {
      if (this.itemEncontrado) {
        // Verifica se o item já está na lista
        const itemExistente = this.listaDeItens.find(
          (item) => item.id === this.itemEncontrado.id
        );
        if (itemExistente) {
          // Aumenta a quantidade do item já existente
          itemExistente.quantidade++;
        } else {
          // Adiciona o item como um novo na lista
          this.listaDeItens.push({ ...this.itemEncontrado, quantidade: 1 });
        }
      }
    },

    aumentarQuantidade(index) {
      this.listaDeItens[index].quantidade++;
    },
    diminuirQuantidade(index) {
      if (this.listaDeItens[index].quantidade > 1) {
        this.listaDeItens[index].quantidade--;
      } else {
        // Remove o item da lista se a quantidade for reduzida a 0
        this.listaDeItens.splice(index, 1);
      }
    },

  },
};
</script>

<style scoped>
/* Estilo básico */
body {
  font-family: Arial, sans-serif;
  margin: 20px;
}

h1 {
  text-align: center;
  color: #0044cc;
}

#app {
  width: 800px;
  height: 400px;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.items-list, .item-info, .cart {
  background-color: #f4f4f4;
  border: 1px solid #ccc;
  border-radius: 8px;
  padding: 20px;
}

.logo-container {
  text-align: center;
  margin-bottom: 20px;
}

.logo-container img {
  max-width: 150px;
  height: auto;
}

.items-list ul {
  list-style: none;
  padding: 0;
}

.items-list li {
  margin-bottom: 10px;
}

.item-info {
  text-align: center;
}

.item-info p {
  margin: 10px 0;
  font-size: 18px;
}

input[type="number"] {
  width: 50%;
  padding: 8px;
  margin: 10px 0;
  border: 1px solid #ccc;
  border-radius: 4px;
}

button {
  background-color: #0044cc;
  color: white;
  padding: 8px 15px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #003399;
}

.cart ul {
  list-style: none;
  padding: 0;
}

.cart li {
  margin-bottom: 20px;
  border-bottom: 1px solid #ccc;
  padding-bottom: 10px;
}

.not-found {
  color: red;
}
</style>
