/*
 * ClimaHub — Dashboard meteorológico educacional
 * Configure sua própria chave OpenWeatherMap antes de executar.
 */
const API_KEY = "a7f3c9e2b1d84f609a2c7e5b8d3f1a6c";
const MQTT_BROKER = "broker.hivemq.com";
const MQTT_PORT = 8000; // WebSocket não criptografado do broker público para fins didáticos.
const MQTT_TOPIC = "senai/weather/dashboard";

let chartInstance = null;
let mqttClient = null;
let currentWeather = null;
let historyData = JSON.parse(localStorage.getItem("weatherHistory") || "[]");

document.addEventListener("DOMContentLoaded", () => {
  inicializarGrafico();
  carregarTema();
  renderizarHistorico();
  carregarFavorito();
  conectarMQTT();

  document.getElementById("themeToggle").addEventListener("click", alternarTema);
  document.getElementById("searchForm").addEventListener("submit", event => {
    event.preventDefault();
    buscarClima();
  });
  document.getElementById("btnFav").addEventListener("click", salvarFavorito);
  document.getElementById("btnFavoriteSearch").addEventListener("click", consultarFavorito);
  document.getElementById("removeFavorite").addEventListener("click", removerFavorito);
  document.getElementById("clearHistory").addEventListener("click", limparHistorico);
});

function alternarTema() {
  const escuro = document.body.classList.toggle("dark-theme");
  localStorage.setItem("theme", escuro ? "dark" : "light");
  document.getElementById("themeToggle").textContent = escuro ? "Modo claro" : "Modo escuro";
  if (chartInstance) atualizarEstiloGrafico();
}

function carregarTema() {
  const escuro = localStorage.getItem("theme") === "dark";
  document.body.classList.toggle("dark-theme", escuro);
  document.getElementById("themeToggle").textContent = escuro ? "Modo claro" : "Modo escuro";
}

async function buscarClima() {
  const cidade = document.getElementById("cityInput").value.trim();
  const pais = document.getElementById("countryInput").value.trim().toUpperCase();
  const loading = document.getElementById("loading");
  const error = document.getElementById("errorMessage");
  const button = document.getElementById("btnSearch");

  error.classList.add("hidden");
  if (!cidade) {
    exibirErro("Informe uma cidade antes de realizar a consulta.");
    document.getElementById("cityInput").focus();
    return;
  }
  if (!API_KEY || API_KEY === "SUA_CHAVE_OPENWEATHERMAP_AQUI") {
    exibirErro("Configure sua API Key no arquivo app.js antes de consultar o clima.");
    return;
  }

  const query = pais ? `${cidade},${pais}` : cidade;
  const url = `https://api.openweathermap.org/data/2.5/weather?q=${encodeURIComponent(query)}&units=metric&lang=pt_br&appid=${encodeURIComponent(API_KEY)}`;
  loading.classList.remove("hidden");
  button.disabled = true;

  try {
    const response = await fetch(url);
    if (response.status === 404) throw new Error("Cidade não encontrada. Verifique o nome informado e tente novamente.");
    if (response.status === 401) throw new Error("A API Key foi recusada. Confira a chave configurada no arquivo app.js.");
    if (response.status === 429) throw new Error("Limite de consultas da API atingido. Aguarde e tente novamente.");
    if (!response.ok) throw new Error("Não foi possível consultar os dados meteorológicos. Tente novamente.");
    const data = await response.json();
    currentWeather = data;
    atualizarInterface(data);
    atualizarGrafico(data);
    adicionarHistorico(data);
    gerarAlerta(data);
    publicarDadosMQTT(data);
  } catch (error) {
    exibirErro(error instanceof TypeError
      ? "Falha de conexão. Verifique sua internet e tente novamente."
      : error.message || "Ocorreu um erro inesperado durante a consulta.");
  } finally {
    loading.classList.add("hidden");
    button.disabled = false;
  }
}

function exibirErro(mensagem) {
  const error = document.getElementById("errorMessage");
  error.textContent = mensagem;
  error.classList.remove("hidden");
}

function atualizarInterface(data) {
  const clima = data.weather?.[0] || {};
  document.getElementById("cardCity").textContent = `${data.name}, ${data.sys?.country || ""}`;
  document.getElementById("cardTemp").textContent = `${Math.round(data.main.temp)} °C`;
  document.getElementById("cardFeelsLike").textContent = `${Math.round(data.main.feels_like)} °C`;
  document.getElementById("cardHumidity").textContent = `${data.main.humidity} %`;
  document.getElementById("cardWind").textContent = `${(data.wind.speed * 3.6).toFixed(1)} km/h`;
  document.getElementById("cardPressure").textContent = `${data.main.pressure} hPa`;
  document.getElementById("cardMin").textContent = `${Math.round(data.main.temp_min)} °C`;
  document.getElementById("cardMax").textContent = `${Math.round(data.main.temp_max)} °C`;
  document.getElementById("weatherDescription").textContent = clima.description || "Condição não informada";
  document.getElementById("weatherIcon").textContent = escolherIcone(clima.main);
  const agora = new Date();
  document.getElementById("lastUpdate").textContent = `Atualizado em ${agora.toLocaleDateString("pt-BR")} às ${agora.toLocaleTimeString("pt-BR", {hour: "2-digit", minute: "2-digit"})}`;
}

function escolherIcone(condicao = "") {
  const value = condicao.toLowerCase();
  if (value.includes("rain") || value.includes("drizzle") || value.includes("chuva")) return "🌧";
  if (value.includes("cloud") || value.includes("nuv")) return "☁";
  if (value.includes("clear") || value.includes("limpo")) return "☀";
  if (value.includes("snow") || value.includes("neve")) return "❄";
  if (value.includes("thunder") || value.includes("tempest")) return "⛈";
  if (value.includes("mist") || value.includes("fog") || value.includes("nebl")) return "🌫";
  return "🌤";
}

function gerarAlerta(data) {
  const card = document.getElementById("weatherAlert");
  const texto = document.getElementById("alertText");
  const temperatura = data.main.temp;
  const condicao = (data.weather?.[0]?.main || "").toLowerCase();
  let mensagem = "Sem alertas automáticos para as condições consultadas.";
  if (temperatura >= 35) mensagem = "Temperatura muito elevada. Hidrate-se e evite esforço prolongado sob o sol.";
  else if (temperatura <= 5) mensagem = "Temperatura muito baixa. Considere cuidados adicionais com o frio.";
  else if (condicao.includes("thunder")) mensagem = "Há indicação de tempestade. Acompanhe os avisos meteorológicos locais.";
  else if (condicao.includes("rain") || condicao.includes("drizzle")) mensagem = "Há indicação de chuva. Redobre a atenção em deslocamentos.";
  texto.textContent = mensagem;
  card.classList.remove("hidden");
}

function inicializarGrafico() {
  const ctx = document.getElementById("weatherChart").getContext("2d");
  chartInstance = new Chart(ctx, {
    type: "line",
    data: {
      labels: [],
      datasets: [
        { label: "Temperatura (°C)", data: [], yAxisID: "y", borderColor: "#2563eb", backgroundColor: "rgba(37,99,235,.12)", fill: true, tension: .3, pointRadius: 4 },
        { label: "Umidade (%)", data: [], yAxisID: "y1", borderColor: "#0d9488", backgroundColor: "rgba(13,148,136,.08)", tension: .3, pointRadius: 4 }
      ]
    },
    options: {
      responsive: true, maintainAspectRatio: false,
      interaction: { mode: "index", intersect: false },
      plugins: { legend: { display: true, labels: { color: getComputedStyle(document.body).getPropertyValue("--text").trim() } },
        tooltip: { enabled: true } },
      scales: {
        x: { title: { display: true, text: "Cidade consultada" }, ticks: { color: "#65728a" }, grid: { color: "rgba(120,140,160,.12)" } },
        y: { type: "linear", position: "left", title: { display: true, text: "Temperatura (°C)" }, ticks: { color: "#65728a" }, grid: { color: "rgba(120,140,160,.12)" } },
        y1: { type: "linear", position: "right", min: 0, max: 100, title: { display: true, text: "Umidade (%)" }, ticks: { color: "#65728a" }, grid: { drawOnChartArea: false } }
      }
    }
  });
}

function atualizarEstiloGrafico() {
  if (!chartInstance) return;
  const corTexto = getComputedStyle(document.body).getPropertyValue("--text").trim();
  chartInstance.options.plugins.legend.labels.color = corTexto;
  chartInstance.update();
}

function atualizarGrafico(data) {
  const labels = chartInstance.data.labels;
  labels.push(`${data.name} ${new Date().toLocaleTimeString("pt-BR", {hour: "2-digit", minute: "2-digit"})}`);
  chartInstance.data.datasets[0].data.push(Number(data.main.temp.toFixed(1)));
  chartInstance.data.datasets[1].data.push(data.main.humidity);
  while (labels.length > 8) {
    labels.shift();
    chartInstance.data.datasets.forEach(dataset => dataset.data.shift());
  }
  chartInstance.update();
}

function adicionarHistorico(data) {
  historyData.unshift({
    cidade: `${data.name}, ${data.sys?.country || ""}`,
    temperatura: Math.round(data.main.temp),
    umidade: data.main.humidity,
    data: new Date().toISOString()
  });
  historyData = historyData.slice(0, 5);
  localStorage.setItem("weatherHistory", JSON.stringify(historyData));
  renderizarHistorico();
}

function renderizarHistorico() {
  const lista = document.getElementById("historyList");
  lista.replaceChildren();
  if (!historyData.length) {
    const li = document.createElement("li");
    li.className = "empty-state";
    li.textContent = "As consultas aparecerão aqui.";
    lista.appendChild(li);
    return;
  }
  historyData.forEach(item => {
    const li = document.createElement("li");
    const cidade = document.createElement("span");
    cidade.className = "history-city";
    cidade.textContent = item.cidade;
    const detalhe = document.createElement("span");
    detalhe.className = "history-detail";
    const data = new Date(item.data);
    detalhe.textContent = `${item.temperatura} °C · Umidade ${item.umidade}% · ${data.toLocaleDateString("pt-BR")} ${data.toLocaleTimeString("pt-BR", {hour: "2-digit", minute: "2-digit"})}`;
    li.append(cidade, detalhe);
    lista.appendChild(li);
  });
}

function limparHistorico() {
  historyData = [];
  localStorage.removeItem("weatherHistory");
  renderizarHistorico();
}

function salvarFavorito() {
  if (!currentWeather) {
    exibirErro("Consulte uma cidade antes de salvá-la como favorita.");
    return;
  }
  const favorito = { cidade: currentWeather.name, pais: currentWeather.sys?.country || "" };
  localStorage.setItem("favCity", JSON.stringify(favorito));
  exibirFavorito(favorito);
}

function carregarFavorito() {
  const salvo = localStorage.getItem("favCity");
  if (!salvo) return;
  try { exibirFavorito(JSON.parse(salvo)); }
  catch { localStorage.removeItem("favCity"); }
}

function exibirFavorito(favorito) {
  document.getElementById("favCityName").textContent = `${favorito.cidade}${favorito.pais ? `, ${favorito.pais}` : ""}`;
}

function consultarFavorito() {
  let favorito;
  try { favorito = JSON.parse(localStorage.getItem("favCity") || "null"); } catch { favorito = null; }
  if (!favorito) {
    exibirErro("Você ainda não salvou uma cidade favorita.");
    return;
  }
  document.getElementById("cityInput").value = favorito.cidade;
  document.getElementById("countryInput").value = favorito.pais || "";
  buscarClima();
}

function removerFavorito() {
  localStorage.removeItem("favCity");
  document.getElementById("favCityName").textContent = "Nenhuma cidade favorita salva.";
}

function conectarMQTT() {
  const status = document.getElementById("mqttStatus");
  if (typeof Paho === "undefined") {
    definirStatusMQTT("Biblioteca indisponível", "failed");
    return;
  }
  const clientID = `climahub_${Math.random().toString(16).slice(2, 10)}`;
  try {
    mqttClient = new Paho.MQTT.Client(MQTT_BROKER, MQTT_PORT, clientID);
    mqttClient.onConnectionLost = () => definirStatusMQTT("Desconectado", "failed");
    mqttClient.connect({
      timeout: 5,
      useSSL: false,
      onSuccess: () => definirStatusMQTT("Conectado", "connected"),
      onFailure: () => definirStatusMQTT("Falha na conexão", "failed")
    });
  } catch {
    definirStatusMQTT("Falha na conexão", "failed");
  }
}

function definirStatusMQTT(texto, classe) {
  const status = document.getElementById("mqttStatus");
  status.textContent = texto;
  status.className = `status-badge ${classe}`;
}

function publicarDadosMQTT(data) {
  if (!mqttClient || !mqttClient.isConnected()) {
    document.getElementById("mqttPayload").textContent = "Consulta concluída, mas o MQTT está desconectado; nenhum payload foi enviado.";
    return;
  }
  const payload = JSON.stringify({
    cidade: data.name,
    pais: data.sys?.country || "",
    temperatura: data.main.temp,
    sensacaoTermica: data.main.feels_like,
    umidade: data.main.humidity,
    ventoKmH: Number((data.wind.speed * 3.6).toFixed(1)),
    pressao: data.main.pressure,
    consultadoEm: new Date().toISOString()
  });
  try {
    const message = new Paho.MQTT.Message(payload);
    message.destinationName = MQTT_TOPIC;
    mqttClient.send(message);
    document.getElementById("mqttTopic").textContent = MQTT_TOPIC;
    document.getElementById("mqttPayload").textContent = payload;
  } catch {
    definirStatusMQTT("Erro ao publicar", "failed");
  }
}
