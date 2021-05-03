<template>
<div>
  <h2>Resistances</h2>
  <calc-resist/>
    <calc-resist/>
</div>
</template>

<script>
import CalcResist from '../components/CalcResist.vue';

export default ({
  components: {
    CalcResist,
  },
  data() {
    return {
      closestValuesForE: {
        keys: [12, 24, 48, 96, 192],
        EList: ['E12', 'E24', 'E48', 'E96', 'E192'],
        values: [0, 0, 0, 0, 0],
      },
      ResistValue: 1.5,
    };
  },
  methods: {
    resistanceValues() {
      document.querySelectorAll('#table tr').forEach((e) => { e.remove(); });
      let arrayE = [];
      let i;
      let t;
      // eslint-disable-next-line no-restricted-syntax
      for (i in this.closestValuesForE.keys) {
        if (i) {
          const E = this.closestValuesForE.keys[i];
          for (t = 0; t <= E; t += 1) {
            if (E <= 24) {
              arrayE[arrayE.length] = Math.round((10 ** (t / E)) * 10) / 10;
            } else {
              arrayE[arrayE.length] = Math.round((10 ** (t / E)) * 100) / 100;
            }
          }
          this.closestValuesForE.values[i] = this.closest(arrayE, this.ResistValue, false);
          arrayE = [];
        }
      }

      this.addARow('series', 'Series', this.closestValuesForE.EList);
      this.closest(this.closestValuesForE.values, this.ResistValue, true);
      this.addARow('results', 'Closest value for each series', this.closestValuesForE.values);
      this.addARow('results', '%error', this.error(this.closestValuesForE.values, this.ResistValue));
      document.querySelectorAll('#table td').forEach((e) => { e.classList.add('border-4', 'px-4', 'py-4'); });
      document.querySelectorAll('#series td').forEach((e) => { e.classList.add('bg-yellow-200'); });
    },

    closest(array, num, fillClosestRow) {
      let i = 0;
      let minDiff = 1000;
      let ans;
      let savedI = 0;
      const closestRow = [0, 0, 0, 0, 0];
      // eslint-disable-next-line no-restricted-syntax
      for (i in array) {
        if (i) {
          const m = Math.abs(num - array[i]);
          if (m < minDiff) {
            minDiff = m;
            ans = array[i];
            savedI = i;
          }
        }
      }
      closestRow[savedI] = ans;
      if (fillClosestRow) {
        this.addARow('results', 'Closest Resistor (R1)', closestRow);
      }
      return ans;
    },

    error(items, target) {
      const ans = [];
      let i = 0;
      items.forEach((item) => {
        ans[i] = Math.abs(Math.round((100 - ((item * 100) / target)) * 100) / 100);
        i += 1;
      });
      return ans;
    },

    addARow(tableId, explanations, items) {
      const table = document.getElementById(tableId);
      const rowNode = document.createElement('tr');

      let cellNode = document.createElement('td');
      let textNode = document.createTextNode(explanations);
      cellNode.appendChild(textNode);
      rowNode.appendChild(cellNode);

      items.forEach((item) => {
        cellNode = document.createElement('td');
        if (item !== 0) {
          textNode = document.createTextNode(item);
          cellNode.appendChild(textNode);
        }
        rowNode.appendChild(cellNode);
      });
      table.appendChild(rowNode);
    },
  },
});
</script>
