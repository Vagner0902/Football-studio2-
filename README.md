# Football-studio2-
<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <title>Football Studio - Estratégia de Cartas</title>
  <style>
    body { font-family: Arial; margin: 20px; }
    table, th, td { border: 1px solid #ccc; border-collapse: collapse; padding: 6px; }
    th { background-color: #f2f2f2; }
    input { width: 60px; text-transform: uppercase; }
    #suggestion { margin-top: 20px; font-size: 18px; font-weight: bold; }
    .highlight { background-color: #d4ffd4; }
    button { margin-top: 10px; margin-right: 10px; padding: 5px 10px; }
  </style>
</head>
<body>

<h2>Football Studio - Leitura de Cartas</h2>
<table id="roundsTable">
  <thead>
    <tr>
      <th>Rodada</th>
      <th>Carta Home</th>
      <th>Carta Away</th>
      <th>Resultado (H/A)</th>
      <th>Tipo Home</th>
      <th>Tipo Away</th>
      <th>Tendência Esperada</th>
      <th>Padrão</th>
    </tr>
  </thead>
  <tbody>
    <!-- Geração automática das 10 linhas -->
    <script>
      for (let i = 1; i <= 10; i++) {
        document.write(`
          <tr>
            <td>${i}</td>
            <td><input id="home${i}" oninput="analyze()"></td>
            <td><input id="away${i}" oninput="analyze()"></td>
            <td><input id="result${i}" maxlength="1" oninput="validateResult(this); analyze();"></td>
            <td id="typeHome${i}"></td>
            <td id="typeAway${i}"></td>
            <td id="expected${i}"></td>
            <td id="pattern${i}"></td>
          </tr>
        `);
      }
    </script>
  </tbody>
</table>

<button onclick="resetTable()">Resetar Planilha</button>

<div id="suggestion"></div>

<script>
  function getCardType(card) {
    card = card.toUpperCase();
    if (["2", "3", "4", "5", "6"].includes(card)) return "Baixo";
    if (["7", "8", "9", "10"].includes(card)) return "Alto";
    if (["J", "Q", "K", "A"].includes(card)) return "Letra";
    return "-";
  }

  function expectedWinner(homeType, awayType) {
    if (homeType === "Alto" && awayType === "Baixo") return "H";
    if (homeType === "Baixo" && awayType === "Alto") return "A";
    if (homeType === "Letra" && awayType === "Letra") return "Quebra";
    if (homeType === "Baixo" && awayType === "Baixo") return "Quebra";
    if (homeType === "Letra" && awayType === "Alto") return "H";
    if (homeType === "Alto" && awayType === "Letra") return "A";
    if (homeType === "Letra" && awayType === "Baixo") return "H";
    if (homeType === "Baixo" && awayType === "Letra") return "A";
    return "-";
  }

  function validateResult(input) {
    let val = input.value.toUpperCase();
    if (val !== "H" && val !== "A") input.value = "";
    else input.value = val;
  }

  function analyze() {
    let respeita = 0, quebra = 0;
    for (let i = 1; i <= 10; i++) {
      let h = document.getElementById(`home${i}`).value.trim();
      let a = document.getElementById(`away${i}`).value.trim();
      let r = document.getElementById(`result${i}`).value.trim().toUpperCase();

      let th = getCardType(h);
      let ta = getCardType(a);
      let expect = expectedWinner(th, ta);
      let pattern = "-";

      if ((r === "H" || r === "A") && expect !== "-") {
        pattern = (r === expect) ? "Respeita" : "Quebra";
        if (pattern === "Respeita") respeita++;
        else quebra++;
      }

      document.getElementById(`typeHome${i}`).innerText = th;
      document.getElementById(`typeAway${i}`).innerText = ta;
      document.getElementById(`expected${i}`).innerText = expect;
      document.getElementById(`pattern${i}`).innerText = pattern;
    }

    if (respeita + quebra === 0) {
      document.getElementById("suggestion").innerText = "";
      return;
    }

    let suggestion = (respeita >= quebra) ? "Seguir Tendência" : "Apostar Contrário";
    document.getElementById("suggestion").innerHTML = `Sugestão: <span class="highlight">${suggestion}</span>`;
  }

  function resetTable() {
    for (let i = 1; i <= 10; i++) {
      document.getElementById(`home${i}`).value = "";
      document.getElementById(`away${i}`).value = "";
      document.getElementById(`result${i}`).value = "";
      document.getElementById(`typeHome${i}`).innerText = "";
      document.getElementById(`typeAway${i}`).innerText = "";
      document.getElementById(`expected${i}`).innerText = "";
      document.getElementById(`pattern${i}`).innerText = "";
    }
    document.getElementById("suggestion").innerText = "";
  }
</script>

</body>
</html>
