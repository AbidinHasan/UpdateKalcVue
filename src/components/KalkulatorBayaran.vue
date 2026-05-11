<script setup>
import { ref, watch } from "vue";

const ISI = 500;

const pcs = ref("");
const box = ref("");
const detailList = ref([]);
const total = ref("");
const bagi = ref("");
const hasil = ref("");
const collectedResults = ref([]);
const totalCollected = ref(0);
const sembunyikan = ref(false);

// Segmen
const segments = [
  { limit: 30000, price: 5.1, name: "Segmen 1" },
  { limit: 20000, price: 5.35, name: "Segmen 2" },
  { limit: 20000, price: 5.74, name: "Segmen 3" },
  { limit: 30000, price: 6.12, name: "Segmen 4" },
  { limit: Infinity, price: 6.41, name: "Segmen 5" },
];

// Watch: Update PCS ketika BOX berubah
watch(box, (newBoxValue) => {
  if (newBoxValue !== "") {
    pcs.value = parseFloat((newBoxValue * ISI).toFixed(3));
  }
});

// Watch: Update BOX ketika PCS berubah (tanpa pembulatan)
watch(pcs, (newPcsValue) => {
  if (newPcsValue !== "") {
    box.value = parseFloat((newPcsValue / ISI).toFixed(3));
  }
});

// Format currency ke Rupiah
const formatRupiah = (number) => {
  return new Intl.NumberFormat("id-ID", {
    style: "currency",
    currency: "IDR",
    minimumFractionDigits: 0,
  }).format(number);
};

// Format number dengan koma
const formatNumber = (number) => {
  return new Intl.NumberFormat("id-ID", {
    minimumFractionDigits: 3,
    maximumFractionDigits: 3,
  }).format(number);
};

const hitung = () => {
  if (pcs.value === "" || pcs.value === 0) {
    alert("Masukkan jumlah pcs terlebih dahulu!");
    return;
  }

  detailList.value = []; // Reset detail
  let sisa = parseFloat(pcs.value);
  let totalNilai = 0;

  // Proses setiap segmen
  segments.forEach((segment) => {
    if (sisa > 0) {
      const qty = Math.min(sisa, segment.limit);
      const nilai = qty * segment.price;
      totalNilai += nilai;
      sisa -= qty;

      detailList.value.push({
        name: segment.name,
        qty: qty,
        price: segment.price,
        total: nilai,
      });
    }
  });

  // Format dan display hasil
  total.value = formatRupiah(totalNilai);
  bagi.value = `Total ÷ 3 = ${formatRupiah(totalNilai / 3)}`;
  hasil.value = totalNilai / 3;
};
const jumlahHasil = () => {
  if (hasil.value !== "") {
    collectedResults.value.push(parseFloat(hasil.value));
    totalCollected.value = collectedResults.value.reduce(
      (sum, val) => sum + val,
      0,
    );
  }
  sembunyikan.value = true;
};
const reset = () => {
  pcs.value = "";
  box.value = "";
  detailList.value = [];
  total.value = "Clingg🧹 Bersihh !!!";
  bagi.value = "";
};
const reset2 = () => {
  collectedResults.value = [];
  totalCollected.value = 0;
  sembunyikan.value = false;
};
</script>

<template>
  <section id="center">
    <div>
      <h1>Kalkulator Bayaran</h1>
    </div>
    <input
      v-model.number="box"
      class="inputAngka"
      type="number"
      placeholder="Masukkan jumlah box"
      step="0.001"
      @keyup.enter="hitung"
    />
    <input
      v-model.number="pcs"
      class="inputAngka"
      type="number"
      placeholder="Masukkan jumlah pcs"
      step="0.001"
      @keyup.enter="hitung"
    />

    <button type="button" class="counter" @click="hitung">Hitung</button>

    <button type="button" class="counter" @click="reset">Hapus</button>
  </section>

  <div class="ticks"></div>

  <section id="next-steps">
    <div id="docs">
      <h2 v-if="hasil">Detail Perhitungan</h2>
      <ul id="detail">
        <li v-for="item in detailList" :key="item.name">
          {{ item.name }}: {{ formatNumber(item.qty) }} × Rp {{ item.price }} =
          {{ formatRupiah(item.total) }}
        </li>
      </ul>
    </div>
    <div id="social">
      <h2 v-if="hasil">Hasil</h2>
      <h3>{{ total }}</h3>
      <p>{{ bagi }}</p>
      <button v-if="bagi" class="counter" @click="jumlahHasil">
        Jumlahkan Hasil
      </button>
    </div>
  </section>

  <div class="ticks"></div>
  <section v-if="hasil" id="center">
    <h2>Total Kumulatif</h2>
    <button v-if="sembunyikan" class="counter" @click="reset2">Reset</button>
    <p>
      Total bayaran yang dikumpulkan:
      {{ formatRupiah(totalCollected) }}
    </p>
    <ul id="kolesi-hasil">
      <li v-for="(result, index) in collectedResults" :key="index">
        Hasil {{ index + 1 }}: {{ formatRupiah(result) }}
      </li>
    </ul>
  </section>

  <div class="ticks"></div>
  <section id="spacer"></section>
</template>
