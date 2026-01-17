<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<title>Desa Digital Karyasari</title>
<meta name="viewport" content="width=device-width, initial-scale=1">

<style>
body {
  font-family: Arial, sans-serif;
  background: #f4f6f8;
  margin: 0;
}
header {
  background: #1e7dd7;
  color: white;
  padding: 20px;
  text-align: center;
}
.container {
  max-width: 900px;
  margin: 30px auto;
  background: white;
  padding: 25px;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0,0,0,0.1);
}
label {
  font-weight: bold;
  margin-top: 15px;
  display: block;
}
input, select, textarea {
  width: 100%;
  padding: 10px;
  margin-top: 5px;
  border-radius: 5px;
  border: 1px solid #ccc;
}
button {
  margin-top: 20px;
  padding: 12px;
  font-size: 16px;
  background: #1e7dd7;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}
button:hover {
  background: #155fa0;
}
.surat-box {
  margin-top: 30px;
  padding: 20px;
  border: 1px solid #333;
}
footer {
  text-align: center;
  margin: 20px;
  color: #666;
}
</style>
</head>

<body>

<header>
<h1>Desa Digital Karyasari</h1>
<p>Sistem Pelayanan Surat Menyurat Desa</p>
</header>

<div class="container">

<h2>Input Surat (Petugas Desa)</h2>

<label>Jenis Surat</label>
<select id="jenisSurat">
  <option value="">-- Pilih --</option>
  <option value="SKTM">Surat Keterangan Tidak Mampu</option>
  <option value="SKU">Surat Keterangan Usaha</option>
  <option value="DOM">Surat Keterangan Domisili</option>
</select>

<label>Nama Warga</label>
<input type="text" id="nama">

<label>NIK</label>
<input type="text" id="nik">

<label>Alamat</label>
<textarea id="alamat"></textarea>

<label>Keperluan</label>
<textarea id="keperluan"></textarea>

<button onclick="buatSurat()">Buat Surat</button>

<div id="hasilSurat"></div>

</div>

<footer>
© 2026 Desa Karyasari
</footer>

<script>
function romawi(bulan) {
  const r = ["I","II","III","IV","V","VI","VII","VIII","IX","X","XI","XII"];
  return r[bulan];
}

function buatSurat() {
  let jenis = document.getElementById("jenisSurat").value;
  let nama = document.getElementById("nama").value;
  let nik = document.getElementById("nik").value;
  let alamat = document.getElementById("alamat").value;
  let keperluan = document.getElementById("keperluan").value;

  if (!jenis || !nama || !nik) {
    alert("Lengkapi data!");
    return;
  }

  let data = JSON.parse(localStorage.getItem("nomorSurat")) || {};
  let tahun = new Date().getFullYear();
  let bulan = romawi(new Date().getMonth());
  let key = jenis + tahun;

  data[key] = (data[key] || 0) + 1;
  localStorage.setItem("nomorSurat", JSON.stringify(data));

  let nomor = String(data[key]).padStart(3, "0");
  let noSurat = `${nomor}/Ds.2001/${jenis}/${bulan}/${tahun}`;

  document.getElementById("hasilSurat").innerHTML = `
  <div class="surat-box">
    <h3 style="text-align:center">SURAT KETERANGAN</h3>
    <p style="text-align:center">Nomor: <b>${noSurat}</b></p>
    <p>Yang bertanda tangan di bawah ini menerangkan bahwa:</p>
    <table>
      <tr><td>Nama</td><td>: ${nama}</td></tr>
      <tr><td>NIK</td><td>: ${nik}</td></tr>
      <tr><td>Alamat</td><td>: ${alamat}</td></tr>
    </table>
    <p>Surat ini dibuat untuk keperluan:</p>
    <p><b>${keperluan}</b></p>
    <p>Demikian surat ini dibuat untuk dipergunakan sebagaimana mestinya.</p>
    <p style="text-align:right">Karyasari, ${new Date().toLocaleDateString("id-ID")}</p>
    <button onclick="window.print()">Cetak Surat</button>
  </div>
  `;
}
</script>

</body>
</html>
