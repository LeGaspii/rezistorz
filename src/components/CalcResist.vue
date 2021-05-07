<script>

export default ({
  // eslint-disable-next-line no-undef
  data() {
    return {
      closestValuesForE: {
        keys: [12, 24, 48, 96, 192],
        EList: ['E12', 'E24', 'E48', 'E96', 'E192'],
        values: [0, 0, 0, 0, 0],
      },
      ResistValue: parseFloat(Math.random(1) * 10).toFixed(2),
    };
  },
  methods: {
    resistanceValues() {
      document.querySelectorAll('#table tr').forEach((e) => { e.remove(); });
      let arrayE = [];
      let i;
      let t;
      if (this.ResistValue > 10 || this.ResistValue < 1) {
        window.alert('The input must be between 1 and 10!');
        this.ResistValue = 1.5;
      }
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
      this.addARow('results', '%error vs target resistor', this.error(this.closestValuesForE.values, this.ResistValue));
      document.querySelectorAll('#table td').forEach((e) => { e.classList.add('border', 'p-2', 'rounded'); });
      document.querySelectorAll('#series td').forEach((e) => { e.classList.add('bg-warning'); });
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
  mounted() {
    document.addEventListener('keyup', (e) => {
      if (e.key === 'Enter') {
        this.resistanceValues();
      }
    });
  },
});
</script>

<template>
<div class="">
  <h1 class="mb-2 text-responsive">
      Looking after a standard value resistor ?
  </h1>
      <p>Type a number between 1 and 10.</p>
      <p>(For {{ Math.round(ResistValue * 100) }} kohms type {{ ResistValue }})</p>
  <div class="p-2 rounded">
    <div class="input-group mb-2">
      <input  class="form-control" id="myNumber"
              v-model="ResistValue"
              min="1" max="10"
              type='number'
              step="0.01">
      <button class="btn btn-primary"
              @click="resistanceValues()"
              >
        Find target resistor
      </button>
    </div>
      <div class="">
      <table id="table" class="table table-striped">
      <thead id="series"></thead>
      <tbody id="results"></tbody>
      </table>
      </div>
    </div>
</div>
</template>

<style scoped>
.text-responsive {
  font-size: calc(100% + 1vw + 1vh);
}
</style>
