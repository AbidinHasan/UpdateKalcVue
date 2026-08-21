<script setup>
import { ref, watch, onMounted } from "vue";
import InputPlaceholder from "../components/InputPlaceholder.vue";
import TombolScrollUp from "../components/ScrollUpButton.vue";
import TulisanJudul from "../components/Text.vue";

const ISI = 500;

const pcs = ref("");
const box = ref("");
const detailList = ref([]);
const total = ref("");
const bagi3 = ref("");
const bagi2 = ref("");
const hasil2 = ref("");
const hasil3 = ref("");
const collectedResults = ref([]);
const boxCollected = ref([]);
const infoBagi = ref([]);
const totalCollected = ref(0);
const sembunyikan = ref(false);
const tampilkanHasil = ref(false);
const tmblUP = ref(false);
const errorKosong = ref(false);

const handleScroll = () => {
  tmblUP.value = window.scrollY > 200;
};

onMounted(() => {
  window.addEventListener("scroll", handleScroll);
});

const scrollHasil = () => {
  setTimeout(() => {
    document.getElementById("social").scrollIntoView({
      behavior: "smooth",
    });
  }, 100); // Delay untuk memastikan hasil sudah dirender
};

const scrollTotal = () => {
  setTimeout(() => {
    document.getElementById("kolesi-hasil").scrollIntoView({
      behavior: "smooth",
    });
  }, 100); // Delay untuk memastikan hasil sudah dirender
};

const tutupTooltip = () => {
  setTimeout(() => {
    errorKosong.value = false;
  }, 2000); // Delay untuk menutup tooltip setelah 2 detik
};

const scrollHapus = () => {
  setTimeout(() => {
    document.getElementById("center").scrollIntoView({
      behavior: "smooth",
    });
  }, 100); // Delay untuk memastikan hasil sudah dirender
};

const TombolKeAtas = () => {
  window.scrollTo({
    top: 0,
    behavior: "smooth",
  });
};
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
    errorKosong.value = false;
  }
});

// Watch: Update BOX ketika PCS berubah (tanpa pembulatan)
watch(pcs, (newPcsValue) => {
  if (newPcsValue !== "") {
    box.value = parseFloat((newPcsValue / ISI).toFixed(3));
    errorKosong.value = false;
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
    errorKosong.value = true;
    tutupTooltip();
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
  bagi2.value = `${formatRupiah(totalNilai / 2)}`;
  bagi3.value = `${formatRupiah(totalNilai / 3)}`;
  hasil2.value = totalNilai / 2;
  hasil3.value = totalNilai / 3;
  tampilkanHasil.value = true;
  scrollHasil();
};
const jumlahHasil2 = () => {
  if (hasil2.value !== "") {
    collectedResults.value.push(parseFloat(hasil2.value));
    boxCollected.value.push(parseFloat(box.value));
    infoBagi.value.push("Hasil dibagi 2");
    totalCollected.value = collectedResults.value.reduce(
      (sum, val) => sum + val,
      0,
    );
  }
  sembunyikan.value = true;
  scrollTotal();
};
const jumlahHasil3 = () => {
  if (hasil3.value !== "") {
    collectedResults.value.push(parseFloat(hasil3.value));
    boxCollected.value.push(parseFloat(box.value));
    infoBagi.value.push("Hasil dibagi 3");
    totalCollected.value = collectedResults.value.reduce(
      (sum, val) => sum + val,
      0,
    );
  }
  sembunyikan.value = true;
  scrollTotal();
};

//Hapus bagian Atas
const reset = () => {
  pcs.value = "";
  box.value = "";
  detailList.value = [];
  total.value = "";
  bagi2.value = "";
  bagi3.value = "";
  tampilkanHasil.value = false;
  scrollHapus();
};

// Hapus Toal bayaran yg dijumlahkan
const reset2 = () => {
  collectedResults.value = [];
  boxCollected.value = [];
  infoBagi.value = [];
  totalCollected.value = 0;
  sembunyikan.value = false;
  scrollHapus();
  hasil2.value = false;
};
</script>

<template>
  <TombolScrollUp v-if="tmblUP" @click="TombolKeAtas" class="btn-up" />
  <section id="center">
    <div id="atas">
      <TulisanJudul />
    </div>
    <div class="input-wrapper">
      <InputPlaceholder
        nama="Masukkan Jumlah Box"
        v-model.number="box"
        @keyup.enter="hitung"
      />
    </div>
    <div class="input-wrapper">
      <InputPlaceholder
        nama="Masukkan Jumlah Pcs"
        v-model.number="pcs"
        @keyup.enter="hitung"
      />
      <span v-if="errorKosong" class="tooltip"> Isi Dulu Dong </span>
    </div>
    <button class="counter" @click="hitung">Hitung</button>

    <button class="klikhapus" @click="reset">Hapus</button>
  </section>

  <div class="ticks"></div>

  <section id="next-steps">
    <div v-if="tampilkanHasil" id="docs">
      <h2>Detail Perhitungan</h2>
      <ul id="detail">
        <li v-for="item in detailList" :key="item.name">
          {{ item.name }}: {{ item.qty }} × Rp {{ item.price }} =
          {{ formatRupiah(item.total) }}
        </li>
      </ul>
    </div>
    <div v-if="tampilkanHasil" id="social">
      <h2>Hasil</h2>
      <h3 style="color: #72cf9f">{{ total }}</h3>
      <h2>Pilih Hasil dibagi 2</h2>
      <button v-if="bagi2" class="counter" @click="jumlahHasil2">
        {{ bagi2 }}</button
      ><br />
      <p>atau</p>
      <h2>Pilih Hasil dibagi 3</h2>
      <button v-if="bagi3" class="counter" @click="jumlahHasil3">
        {{ bagi3 }}
      </button>
    </div>
  </section>

  <div class="ticks"></div>
  <section v-if="hasil2" class="koleksi">
    <h2>Total Bayaran</h2>
    <ul id="kolesi-hasil">
      <li v-for="(result, index) in collectedResults" :key="index">
        Day {{ index + 1 }}: {{ boxCollected[index] }} box |
        {{ infoBagi[index] }} | {{ formatRupiah(result) }}
      </li>
    </ul>
    <p>===============================</p>
    <p>Total bayaran yang dikumpulkan:</p>
    <p style="color: #72cf9f; font-size: 1.5em; font-weight: bold">
      {{ formatRupiah(totalCollected) }}
    </p>
    <button v-if="sembunyikan" class="klikhapus" @click="reset2">
      Bersihkan Total Bayaran
    </button>
  </section>

  <div class="ticks"></div>
  <section id="spacer"></section>
</template>
