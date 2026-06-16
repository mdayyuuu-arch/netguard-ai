<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NetGuard AI</title>
<style>
  body { background: #0a0a0a; color: #00ff88; font-family: monospace; padding: 20px; }
  h1 { text-align: center; color: #00ff88; font-size: 1.8em; }
  .stats { display: flex; gap: 20px; justify-content: center; margin: 20px 0; }
  .stat-box { background: #111; border: 1px solid #00ff88; padding: 15px 25px; border-radius: 8px; text-align: center; }
  .stat-box h2 { margin: 0; font-size: 2em; }
  .stat-box p { margin: 5px 0 0; color: #888; font-size: 0.8em; }
  table { width: 100%; border-collapse: collapse; margin-top: 20px; }
  th { background: #00ff88; color: #000; padding: 10px; }
  td { padding: 8px 10px; border-bottom: 1px solid #222; font-size: 0.85em; }
  .suspicious { background: #2a0000; color: #ff4444; }
  .normal { color: #00ff88; }
  .badge-s { background: #ff4444; color: #fff; padding: 2px 8px; border-radius: 4px; font-size: 0.75em; }
  .badge-n { background: #00ff88; color: #000; padding: 2px 8px; border-radius: 4px; font-size: 0.75em; }
</style>
</head>
<body>
<h1>🛡️ NetGuard AI — Network Security Dashboard</h1>

<div class="stats">
  <div class="stat-box"><h2 id="total">0</h2><p>Total Packets</p></div>
  <div class="stat-box"><h2 id="suspicious" style="color:#ff4444">0</h2><p>Suspicious</p></div>
  <div class="stat-box"><h2 id="normal">0</h2><p>Normal</p></div>
  <div class="stat-box"><h2 id="threat">0%</h2><p>Threat Level</p></div>
</div>

<table>
  <thead><tr><th>Frame</th><th>Source MAC</th><th>Size (bytes)</th><th>Status</th></tr></thead>
  <tbody id="tableBody"></tbody>
</table>

<script>
const packets = [
  {"frame":"1","src":"73:00:c2:be:6d:17","size":4186,"status":"SUSPICIOUS"},
  {"frame":"2","src":"52:67:60:9e:da:8a","size":3141,"status":"NORMAL"},
  {"frame":"3","src":"a0:1f:e5:77:b0:12","size":4981,"status":"SUSPICIOUS"},
  {"frame":"4","src":"52:ab:ad:ef:48:a3","size":1331,"status":"NORMAL"},
  {"frame":"5","src":"40:65:22:fb:64:fe","size":2818,"status":"NORMAL"},
  {"frame":"10","src":"11:22:33:44:55:66","size":4200,"status":"SUSPICIOUS"},
  {"frame":"15","src":"aa:bb:cc:dd:ee:ff","size":1200,"status":"NORMAL"},
  {"frame":"20","src":"12:34:56:78:90:ab","size":3900,"status":"SUSPICIOUS"},
  {"frame":"25","src":"fe:dc:ba:98:76:54","size":800,"status":"NORMAL"},
  {"frame":"30","src":"a1:b2:c3:d4:e5:f6","size":4500,"status":"SUSPICIOUS"}
];

const tbody = document.getElementById('tableBody');
let suspiciousCount = 0;

packets.forEach(p => {
  const isSus = p.status === 'SUSPICIOUS';
  if(isSus) suspiciousCount++;
  tbody.innerHTML += `
    <tr class="${isSus ? 'suspicious' : ''}">
      <td>${p.frame}</td>
      <td>${p.src}</td>
      <td>${p.size}</td>
      <td><span class="${isSus ? 'badge-s' : 'badge-n'}">${p.status}</span></td>
    </tr>`;
});

document.getElementById('total').textContent = packets.length;
document.getElementById('suspicious').textContent = suspiciousCount;
document.getElementById('normal').textContent = packets.length - suspiciousCount;
document.getElementById('threat').textContent = Math.round((suspiciousCount/packets.length)*100) + '%';
</script>
</body>
</html>
