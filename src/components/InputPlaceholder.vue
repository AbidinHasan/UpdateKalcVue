<style scoped>
.input-group {
  position: relative;
}

.input {
  border: solid 1.5px #9e9e9e;
  border-radius: 1rem;
  background: none;
  padding: 1rem;
  font-size: 1rem;
  color: #f5f5f5;
  transition: border 150ms cubic-bezier(0.4, 0, 0.2, 1);
}

.user-label {
  position: absolute;
  left: 15px;
  color: #e8e8e8;
  pointer-events: none;
  transform: translateY(1rem);
  transition: 150ms cubic-bezier(0.4, 0, 0.2, 1);
}

.input:focus,
input:valid {
  outline: none;
  border: 1.5px solid var(--accent);
}

.input:focus ~ label,
input:valid ~ label {
  transform: translateY(-50%) scale(0.8);
  background-color: var(--bg);
  padding: 0 0.2em;
  color: var(--accent);
}
</style>

<template>
  <div class="input-group">
    <input
      required=""
      type="number"
      step="0.001"
      autocomplete="off"
      class="input"
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
    />
    <label class="user-label">{{ nama }}</label>
  </div>
</template>

<script setup>
defineProps({
  nama: String,
  modelValue: String,
});
</script>
