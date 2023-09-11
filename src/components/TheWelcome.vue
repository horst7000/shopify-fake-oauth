<template>
  <article>
    <table>
      <tbody>
        <tr>
          <td>last part of url /abc123 (from Shop Admin)</td>
          <td><input placeholder="abc123" v-model="shopId"></td>
        </tr>
        <tr>
          <td></td>
          <td>{{ block1 }}</td>
        </tr>
        <tr>
          <td>App API Key (from .env or partner/app-setup)</td>
          <td><input :placeholder=placeholderAPIKey v-model="apiKey"></td>
        </tr>
        <tr>
          <td></td>
          <td>{{ block2 }}</td>
        </tr>
        <tr>
          <td>from shop.app.toml</td>
          <td><input placeholder="read_checkouts, read_orders, write_orders" v-model="access_scopes"></td>
        </tr>
        <tr>
          <td></td>
          <td>{{ block3 }}</td>
        </tr>
        <tr>
          <td>host or tunnel (from partner/app-setup)</td>
          <td><input placeholder="trio-suspended-obesity-licence.trycloudflare.com" v-model="host"></td>
        </tr>
        <tr>
          <td></td>
          <td>{{ block4 }}</td>
        </tr>
      </tbody>
    </table>
    <button @click="generate">generate</button>
    <input v-model="output" :aria-invalid=invalidOutput>
  </article>
</template>

<script setup>
import { ref, watchEffect } from 'vue';

/* ---------------- props ------------------- */
/* ---------------- data -------------------- */
const placeholderAPIKey = Math.random().toString(16).slice(2)+Math.random().toString(16).slice(2);

/* ---------------- refs -------------------- */
const shopId        = ref(localStorage.getItem('shopId')||"");
const block1        = ".myshopify.com/admin/oauth/authorize?client_id="
const apiKey        = ref(localStorage.getItem('apiKey')||"");
const block2        = "&scope="
const access_scopes = ref(localStorage.getItem('access_scopes')||"");
const block3        = "&redirect_uri=https://"
const host          = ref(localStorage.getItem('host')||"");
const block4        = "/api/auth/callback";

const output        = ref("");
const invalidOutput = ref("");

/* ---------------- computed ---------------- */

/* ---------------- functions --------------- */
function generate() {
  output.value = `${shopId.value}.myshopify.com/admin/oauth/authorize?client_id=${apiKey.value}`;
  output.value += `&scope=${commaReplaced(access_scopes.value)}&redirect_uri=https://${host.value.replace(/\//g,"")}/api/auth/callback`;
  validate();
}

function commaReplaced(text) {
  return encodeURIComponent(text);
}

function validate() {
  invalidOutput.value = !shopId.value || !apiKey.value || !access_scopes.value || !host.value ? true : "";
}

/* ---------------- watchers ---------------- */
watchEffect(() => localStorage.setItem('shopId', shopId.value));
watchEffect(() => localStorage.setItem('apiKey', apiKey.value));
watchEffect(() => localStorage.setItem('access_scopes', access_scopes.value));
watchEffect(() => localStorage.setItem('host', host.value));

/* ------------- lifecycle hooks ------------ */
</script>

<style>
td:nth-of-type(2) {  
  font-family: Consolas, Menlo, Monaco, Lucida Console, Liberation Mono, DejaVu Sans Mono;
  border-left: 1px solid var(--table-border-color);
}
input {
  --spacing: 0;
}
</style>