const playersDiv = document.getElementById("players");
const leaderboardBody = document.querySelector("#leaderboard tbody");

let players = [];

const PLAYER_COUNT = 4; // change to 2–6 if desired

for (let i = 0; i < PLAYER_COUNT; i++) {
  players.push({ name: `Player ${i+1}`, total: 0 });
}

function renderPlayers() {
  playersDiv.innerHTML = "";
  players.forEach((p, i) => {
    const div = document.createElement("div");
    div.className = "player";
    div.innerHTML = `
      <h3><input value="${p.name}" onchange="players[${i}].name=this.value"></h3>
      <label>Protected Peddles</label><input type="number" id="prot${i}" value="0"><br>
      <label>Unprotected Peddles</label><input type="number" id="risk${i}" value="0"><br>
      <label>Highest Peddle in Hand</label><input type="number" id="hand${i}" value="0"><br>
      <label>Sold Out</label><input type="number" id="sold${i}" value="0"><br>
      <label>Doublecrossed</label><input type="number" id="dbl${i}" value="0"><br>
      <label>Utterly Wiped Out</label><input type="number" id="wipe${i}" value="0"><br>
      <label>Has Banker</label><input type="checkbox" id="banker${i}">
    `;
    playersDiv.appendChild(div);
  });
}

renderPlayers();

document.getElementById("scoreBtn").onclick = () => {
  let round = [];
  let bankerIndex = -1;

  players.forEach((p,i)=>{
    if(document.getElementById(`banker${i}`).checked) bankerIndex = i;
  });

  players.forEach((p,i)=>{
    let protectedVal = Number(document.getElementById(`prot${i}`).value);
    let riskVal = Number(document.getElementById(`risk${i}`).value);
    let handVal = Number(document.getElementById(`hand${i}`).value);
    let sold = Number(document.getElementById(`sold${i}`).value);
    let dbl = Number(document.getElementById(`dbl${i}`).value);
    let wipe = Number(document.getElementById(`wipe${i}`).value);

    let paranoia = sold*25000 + dbl*50000 + wipe*100000;
    let net = protectedVal + riskVal - paranoia - handVal;

    round.push({net, risk:riskVal});
  });

  if(bankerIndex !== -1){
    players.forEach((p,i)=>{
      if(i!==bankerIndex && round[i].risk > 0){
        let skim = round[i].risk * 0.10;
        round[i].net -= skim;
        round[bankerIndex].net += skim;
      }
    });
  }

  let max = Math.max(...round.map(r=>r.net));
  let winner = round.findIndex(r=>r.net===max);
  round[winner].net += 25000;

  players.forEach((p,i)=>{
    p.total += round[i].net;
  });

  leaderboardBody.innerHTML="";
  players.forEach((p,i)=>{
    const row=document.createElement("tr");
    row.innerHTML=`
      <td>${p.name}</td>
      <td>$${round[i].net.toLocaleString()}</td>
      <td>$${p.total.toLocaleString()}</td>
    `;
    leaderboardBody.appendChild(row);
  });
};
