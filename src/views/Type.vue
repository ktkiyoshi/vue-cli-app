<template>
  <div>
    <table>
      <tr>
        <td class="none">&nbsp;</td>
        <th v-for="type in types" :key="type.id"
          :style="{ 'background-color' : '#' + type.color }">
          {{ type.short_name }}
        </th>
      </tr>
      <tr v-for="type in types" :key="type.id">
        <th :style="{ 'background-color' : '#' + type.color }">
          {{ type.short_name }}
        </th>
        <td v-for="(relation, idx) in type.attack_relations" :key="idx"
          :class="[
            relation.effect_rate == 2? 'red': '',
            relation.effect_rate == 0.5? 'blue': '',
            relation.effect_rate == 0? 'bold': ''
          ]"
        >
          {{ toSymbol(relation.effect_rate) }}
        </td>
      </tr>
    </table>
  </div>
</template>
<script>
import axios from 'axios'
export default {
  name: 'type',
  data: function() {
    return {
      types: []
    }
  },
  created: function () {
    axios.get(process.env.VUE_APP_DOMAIN + '/v1/types')
      .then((response) => {
        this.types = response.data;
      })
  },
  methods: {
    toSymbol(rate) {
      switch(rate) {
        case 2:   rate = "●";
                  break;
        case 0.5: rate = "▲";
                  break;
        case 0:   rate = "×";
                  break;
        default:  rate = "";
      }
      return rate;
    }
  },
}
</script>
<style scoped>
* {
  font-family: 'Hiragino Kaku Gothic Pro','ヒラギノ角ゴ Pro W3','メイリオ',Meiryo,'ＭＳ Ｐゴシック',sans-serif;
}
table {
  border-collapse:separate;
  border-spacing: 3px;
  margin: 0 auto;
}
table th,table td {
  border-radius: 3px;
  text-align: center;
  padding: 7px 10px;
}
table th {
  background-color: #c79852;
  color: white;
  border:solid 1px #927141;
}
table td {
  background-color: #e4d4bc;
  border:solid 1px #af9d85;
}
.none {
  background-color: white;
  border: none;
}
.red  { color: red; }
.blue { color: blue; }
.bold { font-weight: bold; }
</style>