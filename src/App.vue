<template>
  <v-app>
    <v-main>
      <v-container fill-height>
        <v-row align="center">
          <v-col cols="5">
            <v-textarea
              v-model="netlist"
              outlined
              rows="20"
              dense
              no-resize
              placeholder="Add your netlist here..."
            />
          </v-col>
          <v-col
            cols="2"
          >
            <v-btn
              color="primary"
              @click="calculate"
            >
              <v-icon left>mdi-calculator</v-icon>
              Calculate
            </v-btn>
          </v-col>
          <v-col cols="5">
            <v-card>

              <v-simple-table>
                <tbody>
                  <tr
                    v-for="(resultLine, i) in result"
                    :key="i"
                  >
                    <td><b>{{ resultLine.type}} {{ resultLine.name }}</b></td>
                    <td>
                      {{ formatValue(resultLine) }}
                    </td>
                  </tr>
                </tbody>
              </v-simple-table>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import { CircuitFactory } from '@mariosimao/nodal-analysis-core';

export default {
  name: 'App',

  components: {
  },

  data: () => ({
    netlist: '',
    result: [],
  }),

  methods: {
    calculate() {
      const circuit = CircuitFactory.fromNetlist(this.netlist);

      this.result = circuit.nodalAnalysis();
    },
    formatValue(resultLine) {
      const { re, im } = resultLine.value;
      const { unit } = resultLine;

      if (re === 0 && im === 0) {
        return `0 ${unit}`;
      }

      if (re === 0) {
        return `${im.toFixed(3)}i ${unit}`;
      }

      if (im === 0) {
        return `${re.toFixed(3)} ${unit}`;
      }

      return `${re} ${im}i ${unit}`;
    },
  },
};
</script>
