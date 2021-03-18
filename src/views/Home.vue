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
            :ref="`${key + index}`"
            v-on:keyup.up="handleArrow('up')"
            v-on:keyup.down="handleArrow('down')"
            v-on:keyup.right="handleArrow('right')"
            v-on:keyup.left="handleArrow('left')"
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
    handleArrow(whyArrow) {
      let idxAwal = 0;
      let idxAkhir = this.data.length;
      let nmColumn = Object.keys(this.data[0]);
      let idx = this.active.index;
      let key = this.active.key;
      if (whyArrow === "up") {
        if (idx - 1 < idxAwal) return;

        let idxCustom = idx - 1;
        this.$nextTick(() => this.setUnactive(idx, key));
        this.$refs[key + idxCustom][0].focus();
        this.$nextTick(() => this.setActive(idxCustom, key));
      }
      if (whyArrow === "down") {
        if (idx + 1 === idxAkhir) return;

        let idxCustom = idx + 1;
        this.$nextTick(() => this.setUnactive(idx, key));
        this.$refs[key + idxCustom][0].focus();
        this.$nextTick(() => this.setActive(idxCustom, key));
      }
      if (whyArrow === "right") {
        if (nmColumn.indexOf(key) === idxAkhir - 1) return;

        let keyCustom = nmColumn[nmColumn.indexOf(key) + 1];
        this.$nextTick(() => this.setUnactive(idx, key));
        this.$refs[keyCustom + idx][0].focus();
        this.$nextTick(() => this.setActive(idx, keyCustom));
      }
      if (whyArrow === "left") {
        if (nmColumn.indexOf(key) - 1 < idxAwal) return;

        let keyCustom = nmColumn[nmColumn.indexOf(key) - 1];
        this.$nextTick(() => this.setUnactive(idx, key));
        this.$refs[keyCustom + idx][0].focus();
        this.$nextTick(() => this.setActive(idx, keyCustom));
      }
    },
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
          nama: "KAMBING",
          kelas: "12",
          noabsen: "17",
        },
        {
          nama: "KUMBANG",
          kelas: "12",
          noabsen: "27",
        },
        {
          nama: "GAJAH",
          kelas: "12",
          noabsen: "1",
        },
      ];
    },
  },
};
</script>
