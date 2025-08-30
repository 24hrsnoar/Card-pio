<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<title>Totem Cliente</title>
<style>
body {
  font-family: 'Segoe UI', sans-serif;
  margin: 0;
  padding: 0;
  background: linear-gradient(135deg, #ffe0b2, #ff8a65);
  text-align: center;
  color: #333;
}
header {
  background: #ff6f61;
  color: white;
  font-size: 28px;
  font-weight: bold;
  padding: 25px;
  text-shadow: 1px 1px 2px rgba(0,0,0,0.2);
}
.produtos {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(160px, 1fr));
  gap: 15px;
  padding: 15px;
}
.produto {
  background: white;
  border-radius: 15px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.25);
  overflow: hidden;
  transition: transform 0.3s, box-shadow 0.3s;
  cursor: pointer;
}
.produto:hover {
  transform: scale(1.05);
  box-shadow: 0 8px 20px rgba(0,0,0,0.35);
}
.produto img {
  width: 100%;
  height: 130px;
  object-fit: cover;
}
.produto h3 { margin: 8px 0 5px; font-size:16px; }
.produto p { font-weight:bold; color:#ff6f61; font-size:14px; }

.pedido {
  margin-top: 20px;
  background: rgba(255,255,255,0.95);
  padding: 18px;
  border-radius: 12px;
  box-shadow: 0 3px 10px rgba(0,0,0,0.2);
  display: inline-block;
  width: 90%;
}
input, button {
  padding: 12px;
  margin: 10px 0;
  border-radius: 8px;
  border: 1px solid #ccc;
  width: 95%;
  font-size: 16px;
}
button {
  background: #ff6f61;
  color: white;
  font-weight: bold;
  cursor: pointer;
  border: none;
  animation: pulse 2s infinite;
}
button:hover { background: #e55b50; }
@keyframes pulse {
  0% { box-shadow: 0 0 0 0 rgba(255,111,97,0.7); }
  70% { box-shadow: 0 0 0 10px rgba(255,111,97,0); }
  100% { box-shadow: 0 0 0 0 rgba(255,111,97,0); }
}
ul { list-style:none; padding-left:0; margin-top:10px; font-size:14px; }
</style>
</head>
<body>
<header>🍔 Totem Cliente</header>

<section class="produtos" id="listaProdutos"></section>

<div class="pedido">
<h2>📝 Seu Pedido</h2>
<ul id="itensPedido"></ul>
<input type="text" id="nomeCliente" placeholder="Digite seu nome" required>
<button onclick="enviarPedido()">Enviar para WhatsApp</button>
</div>

<script>
const produtos = JSON.parse(localStorage.getItem("produtos")) || [];
let carrinho = [];

function carregarProdutos(){
  const lista=document.getElementById("listaProdutos");
  if(produtos.length===0){lista.innerHTML="<p>⚠️ Nenhum produto disponível ainda.</p>";return;}
  lista.innerHTML="";
  produtos.forEach((p,i)=>{
    const div=document.createElement("div");
    div.className="produto";
    div.innerHTML=`
      <img src="${p.imagem}" alt="${p.nome}">
      <h3>${p.nome}</h3>
      <p>R$ ${p.preco}</p>
    `;
    div.onclick=()=>adicionarCarrinho(i);
    lista.appendChild(div);
  });
}

function adicionarCarrinho(i){
  const p=produtos[i];
  carrinho.push(p);
  atualizarItensPedido();
  mostrarAnimacao(p.nome);
}

function mostrarAnimacao(nome){
  const msg=document.createElement("div");
  msg.textContent=`✅ ${nome} adicionado!`;
  msg.style.position="fixed";
  msg.style.top="15px";
  msg.style.left="50%";
  msg.style.transform="translateX(-50%)";
  msg.style.background="#4caf50";
  msg.style.color="#fff";
  msg.style.padding="10px 20px";
  msg.style.borderRadius="10px";
  msg.style.boxShadow="0 3px 6px rgba(0,0,0,0.3)";
  msg.style.zIndex="999";
  document.body.appendChild(msg);
  setTimeout(()=>{msg.remove();},1500);
}

function atualizarItensPedido(){
  const ul=document.getElementById("itensPedido");
  ul.innerHTML="";
  carrinho.forEach(p=>{
    const ul.appendChildli=document.createElement("li");
    li.textContent=`${p.nome} - R$ ${p.preco}`;
    (li);
  });
}

function enviarPedido(){
  const nome=document.getElementById("nomeCliente").value;
  if(!nome){alert("Digite seu nome!");return;}
  if(carrinho.length===0){alert("Adicione pelo menos 1 produto!");return;}
  let msg=`*Pedido de ${nome}:*\n\n`;
  carrinho.forEach(p=>{msg+=`🍴 ${p.nome} - R$ ${p.preco}\n`;});
  const numero="5588981521087";
  const url=`https://wa.me/${numero}?text=${encodeURIComponent(msg)}`;
  window.open(url,"_blank");
  carrinho=[];atualizarItensPedido();document.getElementById("nomeCliente").value="";
}

carregarProdutos();
</script>
</body>
</html>
