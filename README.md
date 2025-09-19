<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <title>Aplikasi Pencatat Hutang</title>
  <style>
    body { font-family: Arial, sans-serif; background: #f4f4f4; margin: 0; padding: 20px; }
    h1 { text-align: center; color: #333; }
    .form { background: #fff; padding: 15px; border-radius: 8px; margin-bottom: 20px; box-shadow: 0 2px 5px rgba(0,0,0,0.1); }
    input, button { padding: 8px; margin: 5px 0; width: 100%; border: 1px solid #ccc; border-radius: 5px; }
    button { background: #2196f3; color: white; cursor: pointer; border: none; }
    button:hover { background: #1976d2; }
    table { width: 100%; border-collapse: collapse; background: #fff; box-shadow: 0 2px 5px rgba(0,0,0,0.1); }
    th, td { border: 1px solid #ddd; padding: 10px; text-align: center; }
    th { background: #2196f3; color: white; }
    .total { margin-top: 15px; padding: 10px; background: #fff; border-radius: 8px; font-weight: bold; text-align: right; box-shadow: 0 2px 5px rgba(0,0,0,0.1); }
    .print-btn { margin-top: 15px; background: #4caf50; }
    .print-btn:hover { background: #388e3c; }
    @media print {
      .form, .print-btn { display: none; }
      body { background: #fff; }
    }
  </style>
</head>
<body>
  <h1>Aplikasi Pencatat Hutang</h1>

  <div class="form">
    <label>Nama</label>
    <input type="text" id="nama">
    <label>Jumlah Hutang (Rp)</label>
    <input type="number" id="jumlah">
    <label>Keterangan</label>
    <input type="text" id="keterangan">
    <button onclick="tambahHutang()">Tambah</button>
  </div>

  <table>
    <thead>
      <tr>
        <th>Nama</th>
        <th>Jumlah (Rp)</th>
        <th>Keterangan</th>
        <th>Aksi</th>
      </tr>
    </thead>
    <tbody id="listHutang"></tbody>
  </table>

  <div class="total">
    Total Hutang: Rp <span id="totalHutang">0</span>
  </div>

  <button class="print-btn" onclick="cetakHutang()">🖨 Cetak Daftar Hutang</button>

  <script>
    let hutang = JSON.parse(localStorage.getItem("hutang")) || [];

    function simpan() {
      localStorage.setItem("hutang", JSON.stringify(hutang));
      tampilkan();
    }

    function tambahHutang() {
      const nama = document.getElementById("nama").value.trim();
      const jumlah = document.getElementById("jumlah").value.trim();
      const keterangan = document.getElementById("keterangan").value.trim();

      if (nama && jumlah) {
        hutang.push({ nama, jumlah: parseInt(jumlah), keterangan });
        simpan();
        document.getElementById("nama").value = "";
        document.getElementById("jumlah").value = "";
        document.getElementById("keterangan").value = "";
      } else {
        alert("Isi nama dan jumlah hutang dulu!");
      }
    }

    function hapusHutang(index) {
      if (confirm("Yakin mau hapus data ini?")) {
        hutang.splice(index, 1);
        simpan();
      }
    }

    function tampilkan() {
      const list = document.getElementById("listHutang");
      list.innerHTML = "";
      let total = 0;

      hutang.forEach((item, i) => {
        total += item.jumlah;
        list.innerHTML += `
          <tr>
            <td>${item.nama}</td>
            <td>Rp ${item.jumlah.toLocaleString()}</td>
            <td>${item.keterangan || "-"}</td>
            <td><button onclick="hapusHutang(${i})">Hapus</button></td>
          </tr>
        `;
      });

      document.getElementById("totalHutang").textContent = total.toLocaleString();
    }

    function cetakHutang() {
      window.print();
    }

    tampilkan();
  </script>
</body>
</html>
