<template>
  <div>
    <table>
      <tr>
        <th v-for="col in columns" :key="col.value">
          {{ col.text }}
        </th>
      </tr>
      <tr v-for="(row, index) in data" :key="index + 'hai'">
        <td class="numbering">{{ index + 1 }}</td>
        <td
          :class="{ active: isActive(index, key) }"
          v-for="(value, key) in row"
          :key="'input' + key"
        >
          <v-text-field
            dense
            solo
            @focus="setActive(index, key)"
            @blur="setUnactive(index, key)"
            :flat="!isActive(index, key)"
            type="text"
            v-model="data[index][key]"
            hide-details="auto"
          ></v-text-field>
        </td>
      </tr>
    </table>
    {{ active }}
  </div>
</template>
<style lang="scss" scoped>
table,
th,
td {
  border: 1px solid black;
  border-collapse: collapse;
}
td.active {
  border: 1px double blue !important;
  box-shadow: inset 0 0 0 100px blue;
}
td.numbering,
th {
  background-color: grey;
}
</style>
<script>
export default {
  name: "editable",
  data() {
    return {
      active: {},
      columns: [],
      data: [],
    };
  },
  mounted() {
    this.init();
  },
  methods: {
    setActive(index, key) {
      this.active = {
        index: index,
        key: key,
      };
    },
    isActive(index, key) {
      if (index == this.active.index && key == this.active.key) return true;
      else return false;
    },
    setUnactive() {
      this.active = {};
    },
    init() {
      this.columns = [
        {
          text: "No",
          value: "no",
        },
        {
          text: "Nama",
          value: "nama",
        },
        {
          text: "Kelas",
          value: "kelas",
        },
        {
          text: "No. Absen",
          value: "noabsen",
        },
      ];
      this.data = [
        {
          nama: "Hanan",
          kelas: "12",
          noabsen: "17",
        },
        {
          nama: "Caca",
          kelas: "12",
          noabsen: "27",
        },
        {
          nama: "Angga",
          kelas: "12",
          noabsen: "1",
        },
      ];
    },
  },
};
</script>
