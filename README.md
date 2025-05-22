# Menu-kantin
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <title>Menu Kantin</title>
  <style>
    body { font-family: Arial, sans-serif; padding: 20px; background: #f5f5f5; }
    h1 { text-align: center; }
    .menu-item { border: 1px solid #ddd; padding: 15px; margin: 10px 0; background: #fff; }
    button { padding: 10px 15px; background-color: green; color: white; border: none; cursor: pointer; }
    input[type="number"] { width: 50px; }
  </style>
</head>
<body>
  <h1>Menu Makanan</h1>
  <div id="menu-container"></div>

  <hr>
  <h3>Total: RM <span id="total">0.00</span></h3>
  <button onclick="checkout()">Pesan Sekarang</button>

  <div id="payment-method" style="display:none;">
    <h3>Pilih Metode Pembayaran:</h3>
    <label><input type="radio" name="metode" value="COD" checked> Bayar di Tempat (COD)</label><br>
    <label><input type="radio" name="metode" value="Online"> Bayar Online (FPX/TNG)</label><br><br>
    <button onclick="confirmOrder()">Konfirmasi Pesanan</button>
  </div>

  <script>
    const menu = [
      { id: 1, nama: "Nasi Lemak", harga: 5.0, stok: 10 },
      { id: 2, nama: "Mee Goreng", harga: 4.5, stok: 8 },
      { id: 3, nama: "Teh Tarik", harga: 2.0, stok: 15 }
    ];

    const order = {};

    function renderMenu() {
      const container = document.getElementById('menu-container');
      menu.forEach(item => {
        const div = document.createElement('div');
        div.className = 'menu-item';
        div.innerHTML = `
          <strong>${item.nama}</strong><br>
          Harga: RM ${item.harga.toFixed(2)}<br>
          Stok: ${item.stok}<br>
          Jumlah: <input type="number" min="0" max="${item.stok}" value="0" onchange="updateOrder(${item.id}, this.value)">
        `;
        container.appendChild(div);
      });
    }

    function updateOrder(id, qty) {
      const item = menu.find(m => m.id === id);
      order[id] = {
        nama: item.nama,
        harga: item.harga,
        jumlah: parseInt(qty)
      };
      updateTotal();
    }

    function updateTotal() {
      let total = 0;
      Object.values(order).forEach(item => {
        if (item.jumlah > 0) total += item.jumlah * item.harga;
      });
      document.getElementById('total').innerText = total.toFixed(2);
    }

    function checkout() {
      document.getElementById('payment-method').style.display = 'block';
    }

    function confirmOrder() {
      const metode = document.querySelector('input[name="metode"]:checked').value;
      let pesan = "Pesanan Anda:\n";
      Object.values(order).forEach(item => {
        if (item.jumlah > 0) {
          pesan += `${item.nama} x${item.jumlah} = RM ${(item.jumlah * item.harga).toFixed(2)}\n`;
        }
      });
      pesan += `\nMetode Pembayaran: ${metode}`;
      alert(pesan);

      // Untuk sistem nyata, bisa kirim ke WhatsApp atau database
      // window.location.href = `https://wa.me/60123456789?text=${encodeURIComponent(pesan)}`;
    }

    renderMenu();
  </script>
</body>
</html>
