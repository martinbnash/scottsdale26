# scottsdale26

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <title>Scottsdale 26 Championship</title>

  <style>
    :root {
      --bg: #0b1220;
      --panel: #172033;
      --card: #243247;
      --line: #40516a;
      --text: #f8fafc;
      --muted: #a9b6c8;
      --blue: #3b82f6;
      --red: #ef4444;
      --gold: #fbbf24;
      --green: #22c55e;
    }

    * {
      box-sizing: border-box;
    }

    body {
      margin: 0;
      background: linear-gradient(160deg, #0b1220, #111c2f);
      color: var(--text);
      font-family: Arial, Helvetica, sans-serif;
      min-height: 100vh;
    }

    header {
      text-align: center;
      padding: 25px 15px 15px;
      border-bottom: 1px solid var(--line);
    }

    h1 {
      margin: 0;
      font-size: clamp(28px, 6vw, 44px);
    }

    .subtitle {
      color: var(--muted);
      margin-top: 6px;
    }

    .tabs {
      display: flex;
      justify-content: center;
      gap: 8px;
      flex-wrap: wrap;
      padding: 20px 10px 10px;
    }

    .tab {
      border: 1px solid var(--line);
      background: var(--panel);
      color: var(--muted);
      border-radius: 10px;
      padding: 12px 18px;
      font-weight: bold;
      cursor: pointer;
    }

    .tab.active {
      background: var(--gold);
      color: #111827;
      border-color: var(--gold);
    }

    .course {
      text-align: center;
      color: var(--muted);
      margin-bottom: 15px;
    }

    .board {
      max-width: 1100px;
      margin: auto;
      padding: 0 15px 50px;
    }

    .instructions {
      text-align: center;
      background: var(--panel);
      border: 1px solid var(--gold);
      color: #fde68a;
      padding: 12px;
      border-radius: 10px;
      margin-bottom: 15px;
    }

    .matches {
      display: grid;
      gap: 15px;
    }

    .match {
      background: var(--panel);
      border: 1px solid var(--line);
      border-radius: 15px;
      padding: 15px;
    }

    .match-title {
      text-align: center;
      color: var(--gold);
      font-weight: bold;
      margin-bottom: 12px;
      font-size: 18px;
    }

    .match-row {
      display: grid;
      grid-template-columns: 1fr 60px 1fr;
      gap: 12px;
      align-items: center;
    }

    .team-slot {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 8px;
    }

    .slot {
      min-height: 55px;
      background: #0f172a;
      border: 2px dashed var(--line);
      border-radius: 10px;
      display: flex;
      align-items: center;
      justify-content: center;
      text-align: center;
      padding: 8px;
      font-weight: bold;
      cursor: pointer;
    }

    .clayton-slot {
      border-color: var(--blue);
    }

    .ryan-slot {
      border-color: var(--red);
    }

    .vs {
      text-align: center;
      font-weight: bold;
      color: var(--muted);
    }

    .rosters {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 15px;
      margin-top: 25px;
    }

    .roster {
      background: var(--panel);
      border: 1px solid var(--line);
      border-radius: 15px;
      padding: 15px;
    }

    .roster h2 {
      text-align: center;
      margin-top: 0;
    }

    .blue-title {
      color: #93c5fd;
    }

    .red-title {
      color: #fca5a5;
    }

    .players {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 8px;
    }

    .player {
      border-radius: 10px;
      padding: 14px 8px;
      font-weight: bold;
      cursor: pointer;
      background: var(--card);
      color: white;
    }

    .clayton-player {
      border: 2px solid var(--blue);
    }

    .ryan-player {
      border: 2px solid var(--red);
    }

    .player.selected {
      border-color: var(--gold);
      color: var(--gold);
      box-shadow: 0 0 0 2px rgba(251,191,36,.3);
    }

    .player.used {
      opacity: .35;
      cursor: not-allowed;
    }

    .controls {
      text-align: center;
      margin-top: 20px;
    }

    .controls button {
      border: 0;
      padding: 12px 20px;
      border-radius: 10px;
      font-weight: bold;
      cursor: pointer;
      margin: 4px;
    }

    .clear {
      background: #475569;
      color: white;
    }

    .reset {
      background: #991b1b;
      color: white;
    }

    @media (max-width: 650px) {
      .match-row {
        grid-template-columns: 1fr 35px 1fr;
      }

      .team-slot {
        grid-template-columns: 1fr;
      }

      .rosters {
        grid-template-columns: 1fr;
      }
    }
  </style>
</head>

<body>

<header>
  <h1>SCOTTSDALE 26 CHAMPIONSHIP</h1>
  <div class="subtitle">Captain Pairings Board</div>
</header>

<div class="tabs" id="tabs"></div>

<div class="course" id="course"></div>

<div class="board">

  <div class="instructions" id="instructions">
    Select a player, then tap an empty pairing slot.
  </div>

  <div class="matches" id="matches"></div>

  <div class="rosters">

    <div class="roster">
      <h2 class="blue-title">CLAYTON'S TEAM</h2>
      <div class="players" id="claytonPlayers"></div>
    </div>

    <div class="roster">
      <h2 class="red-title">RYAN'S TEAM</h2>
      <div class="players" id="ryanPlayers"></div>
    </div>

  </div>

  <div class="controls">
    <button class="clear" onclick="clearRound()">Clear Current Round</button>
    <button class="reset" onclick="resetAll()">Reset All Rounds</button>
  </div>

</div>

<script>

const rounds = [
  {
    name: "Round 1",
    course: "Dinosaur Mountain • September 24"
  },
  {
    name: "Round 2",
    course: "We-Ko-Pa Cholla • September 25 AM"
  },
  {
    name: "Round 3",
    course: "We-Ko-Pa Saguaro • September 25 PM"
  },
  {
    name: "Round 4",
    course: "We-Ko-Pa Saguaro • September 26"
  }
];

const claytonTeam = [
  "Clayton",
  "Marty",
  "Raymond",
  "Brian",
  "Michael",
  "Aaron"
];

const ryanTeam = [
  "Ryan",
  "Jon",
  "Ryan D",
  "Brad",
  "Brent",
  "Travis"
];

let currentRound = 0;
let selectedPlayer = null;

let pairings = JSON.parse(
  localStorage.getItem("scottsdale26Pairings")
) || {};

function getRoundData() {

  if (!pairings[currentRound]) {

    pairings[currentRound] = [
      { clayton: ["", ""], ryan: ["", ""] },
      { clayton: ["", ""], ryan: ["", ""] },
      { clayton: ["", ""], ryan: ["", ""] }
    ];

  }

  return pairings[currentRound];
}

function saveData() {

  localStorage.setItem(
    "scottsdale26Pairings",
    JSON.stringify(pairings)
  );

}

function render() {

  renderTabs();

  document.getElementById("course").textContent =
    rounds[currentRound].course;

  renderMatches();
  renderPlayers();

}

function renderTabs() {

  document.getElementById("tabs").innerHTML =
    rounds.map((round, index) => `

      <button
        class="tab ${index === currentRound ? "active" : ""}"
        onclick="changeRound(${index})"
      >

        ${round.name}

      </button>

    `).join("");

}

function renderMatches() {

  const roundData = getRoundData();

  document.getElementById("matches").innerHTML =
    roundData.map((match, matchIndex) => `

      <div class="match">

        <div class="match-title">

          MATCH ${matchIndex + 1}

        </div>

        <div class="match-row">

          <div class="team-slot">

            ${match.clayton.map((player, slotIndex) => `

              <div
                class="slot clayton-slot"
                onclick="placePlayer(${matchIndex}, 'clayton', ${slotIndex})"
              >

                ${player || "Tap to place"}

              </div>

            `).join("")}

          </div>

          <div class="vs">VS</div>

          <div class="team-slot">

            ${match.ryan.map((player, slotIndex) => `

              <div
                class="slot ryan-slot"
                onclick="placePlayer(${matchIndex}, 'ryan', ${slotIndex})"
              >

                ${player || "Tap to place"}

              </div>

            `).join("")}

          </div>

        </div>

      </div>

    `).join("");

}

function getUsedPlayers() {

  return getRoundData()
    .flatMap(match => [
      ...match.clayton,
      ...match.ryan
    ])
    .filter(player => player);

}

function renderPlayers() {

  const usedPlayers = getUsedPlayers();

  document.getElementById("claytonPlayers").innerHTML =
    claytonTeam.map(player => playerButton(
      player,
      "clayton-player",
      usedPlayers
    )).join("");

  document.getElementById("ryanPlayers").innerHTML =
    ryanTeam.map(player => playerButton(
      player,
      "ryan-player",
      usedPlayers
    )).join("");

}

function playerButton(player, teamClass, usedPlayers) {

  const used = usedPlayers.includes(player);

  return `

    <button
      class="
        player
        ${teamClass}
        ${selectedPlayer === player ? "selected" : ""}
        ${used ? "used" : ""}
      "

      onclick="selectPlayer('${player}')"

      ${used ? "disabled" : ""}

    >

      ${player}

    </button>

  `;

}

function selectPlayer(player) {

  selectedPlayer =
    selectedPlayer === player
      ? null
      : player;

  render();

}

function placePlayer(matchIndex, team, slotIndex) {

  if (!selectedPlayer) {

    alert(
      "Select a player first."
    );

    return;

  }

  if (
    team === "clayton" &&
    !claytonTeam.includes(selectedPlayer)
  ) {

    alert(
      "That player belongs on Ryan's team."
    );

    return;

  }

  if (
    team === "ryan" &&
    !ryanTeam.includes(selectedPlayer)
  ) {

    alert(
      "That player belongs on Clayton's team."
    );

    return;

  }

  const roundData = getRoundData();

  roundData[matchIndex][team][slotIndex] =
    selectedPlayer;

  selectedPlayer = null;

  saveData();

  render();

}

function changeRound(index) {

  currentRound = index;

  selectedPlayer = null;

  render();

}

function clearRound() {

  if (!confirm("Clear this round?")) {
    return;
  }

  pairings[currentRound] = [
    { clayton: ["", ""], ryan: ["", ""] },
    { clayton: ["", ""], ryan: ["", ""] },
    { clayton: ["", ""], ryan: ["", ""] }
  ];

  selectedPlayer = null;

  saveData();

  render();

}

function resetAll() {

  if (!confirm("Reset all four rounds?")) {
    return;
  }

  pairings = {};

  selectedPlayer = null;

  saveData();

  render();

}

render();

</script>

</body>
</html>