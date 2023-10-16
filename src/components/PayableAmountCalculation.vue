<template>
  <div>
    <div class="bg-blue-500 py-1 px-4 mt-4 lg:mt-0">
      <p class="text-lg font-bold text-gray-100">Payable Amount</p>
    </div>
    <div class="overflow-x-auto app-scroll-bar">
      <table class="min-w-full">
        <colgroup>
          <col span="1" style="width: 20%" />
          <col span="1" style="width: 14%" />
          <col span="1" style="width: 23%" />
          <col span="1" style="width: 23%" />
        </colgroup>
        <thead class="bg-[#1e344e]">
          <th class="text-left pl-2">For</th>
          <th class="left-align-td text-center">Rate</th>
          <th class="right-align-td">Payable</th>
          <th class="right-align-td">Remaining</th>
        </thead>
        <tbody>
          <tr>
            <td class="px-2 py-3 border-b border-gray-500 text-sm">
              First <span class="font-bold">3.5</span> Lakh
            </td>
            <td class="text-center px-2 py-3 border-b border-gray-500">0</td>
            <td class="text-right px-2 py-3 border-b border-gray-500">0</td>
            <td class="text-right px-2 py-3 border-b border-gray-500">
              {{ remainingAmountAfterFirstThreeLakh }}
            </td>
          </tr>
          <tr>
            <td class="px-2 py-3 border-b border-gray-500 text-sm">
              Next <span class="font-bold">1</span> Lakh
            </td>
            <td class="text-center px-2 py-3 border-b border-gray-500">5%</td>
            <td class="text-right px-2 py-3 border-b border-gray-500">
              {{ nextOneLakhTaxableIncome.payableAmount }}
            </td>
            <td class="text-right px-2 py-3 border-b border-gray-500">
              {{ nextOneLakhTaxableIncome.remainingAmount }}
            </td>
          </tr>
          <tr>
            <td class="px-2 py-3 border-b border-gray-500 text-sm">
              Next <span class="font-bold">3</span> Lakh
            </td>
            <td class="text-center px-2 py-3 border-b border-gray-500">10%</td>
            <td class="text-right px-2 py-3 border-b border-gray-500">
              {{ nextThreeLakhTaxableIncome.payableAmount }}
            </td>
            <td class="text-right px-2 py-3 border-b border-gray-500">
              {{ nextThreeLakhTaxableIncome.remainingAmount }}
            </td>
          </tr>
          <tr>
            <td class="px-2 py-3 border-b border-gray-500 text-sm">
              Next <span class="font-bold">4</span> Lakh
            </td>
            <td class="text-center px-2 py-3 border-b border-gray-500">15%</td>
            <td class="text-right px-2 py-3 border-b border-gray-500">
              {{ nextFourLakhTaxableIncome.payableAmount }}
            </td>
            <td class="text-right px-2 py-3 border-b border-gray-500">
              {{ nextFourLakhTaxableIncome.remainingAmount }}
            </td>
          </tr>
          <tr>
            <td class="px-2 py-3 border-b border-gray-500 text-sm">
              Next <span class="font-bold">5</span> Lakh
            </td>
            <td class="text-center px-2 py-3 border-b border-gray-500">20%</td>
            <td class="text-right px-2 py-3 border-b border-gray-500">
              {{ nextFiveLakhTaxableIncome.payableAmount }}
            </td>
            <td class="text-right px-2 py-3 border-b border-gray-500">
              {{ nextFiveLakhTaxableIncome.remainingAmount }}
            </td>
          </tr>
          <tr>
            <td class="px-2 py-3 border-b border-gray-500 text-sm">Rest Amount</td>
            <td class="text-center px-2 py-3 border-b border-gray-500">25%</td>
            <td class="text-right px-2 py-3 border-b border-gray-500">
              {{ restTaxableIncome }}
            </td>
            <td class="text-right px-2 py-3 border-b border-gray-500">0</td>
          </tr>
          <tr>
            <td colspan="4" class="w-full py-2 bg-[#1e344e] pl-3 font-semibold text-right pr-1 md:pr-3">
              <span class="text-[16px] text-blue-300">Total Payable : &nbsp;</span>
              <span>{{ totalPayable }}</span>
            </td>
          </tr>
          <tr class="bg-[#1e344e]">
            <td colspan="4" class="py-2">
              <div class="flex justify-between items-start px-2">
                <label class="text-[15px] text-blue-400 font-bold" for="">Allowable Investment :</label>
                <p>{{ allowedInvestedAmount }}</p>
              </div>
            </td>
          </tr>
          <tr class="bg-[#1e344e]">
            <td colspan="4" class="py-2">
              <div class="flex justify-between items-start px-2">
                <label class="text-[15px] text-blue-400 font-bold" for="" style="padding-top:6px">Total Investment :</label>
                <input
                  v-model.number="investedAmount"
                  type="number"
                  class="focus:outline-none app-secondary-bg py-1 px-3 border border-gray-500 rounded-sm text-right w-1/3"
                />
              </div>
            </td>
          </tr>
          <tr class="bg-[#1e344e]">
            <td colspan="4" class="py-2">
              <div class="flex justify-between items-start px-2">
                <p class="text-[15px] text-blue-400 font-bold">Redemption for investment :</p>
                <p class="text-right pr-3">{{ applicableDiscount }}</p>
              </div>
            </td>
          </tr>
          <tr class="bg-[#1e344e]">
            <td colspan="4" class="pt-2 pb-4">
              <div class="flex justify-between items-start px-2">
                <p class="text-[15px] text-blue-400 font-bold">AIT :</p>
                <input
                  v-model="aitAmount"
                  type="number"
                  class="focus:outline-none app-secondary-bg py-1 px-3 border border-gray-500 rounded-sm text-right w-1/3"
                />
              </div>
            </td>
          </tr>
          <tr class="bg-[#80a1df29]">
            <td colspan="4" class="py-5">
              <div class="flex justify-between items-start px-2">
                <p class="text-[15px] md:text-[17px] font-semibold text-[#4cccff]">
                  Final Payable Amount :
                </p>
                <p class="text-right font-bold text-xl text-[#dab53e] pr-0 md:pr-3">
                  {{ actualTotalPayableAmount }}
                </p>
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class="mt-5 pb-4 pr-3 flex justify-center sm:justify-end">
      <button
        @click="resetData"
        class="bg-red-600 text-white-500 text-[14px] sm:text-[16px] border border-red-600 font-semibold rounded-sm px-6 py-1 mr-3">
        Reset
      </button>
      <button
        @click="showPrintPreview"
        class="bg-teal-600 text-white text-[14px] sm:text-[16px] font-semibold rounded-sm px-6 py-1">
        Print Preview
      </button>
    </div>
    <div v-if="printPreview">
      <PrintingView
        :closePreview="closePreview"
        :salaryData="props.salaryData"
        :noPrint="props.nonPrintable"
        :totalTaxableAmount="props.totalTaxableAmount"
        :remainingAmountAfterFirstThreeLakh="remainingAmountAfterFirstThreeLakh"
        :nextOneLakhTaxableIncome="nextOneLakhTaxableIncome"
        :nextThreeLakhTaxableIncome="nextThreeLakhTaxableIncome"
        :nextFourLakhTaxableIncome="nextFourLakhTaxableIncome"
        :nextFiveLakhTaxableIncome="nextFiveLakhTaxableIncome"
        :restTaxableIncome="restTaxableIncome"
        :totalPayable="totalPayable"
        :investedAmount="investedAmount"
        :allowedInvestedAmount="allowedInvestedAmount"
        :applicableDiscount="applicableDiscount"
        :aitAmount="aitAmount"
        :actualTotalPayableAmount="actualTotalPayableAmount"
      />
    </div>
  </div>
  <div>


  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import PrintingView from "./PrintingView.vue";

const props = defineProps({
  nonPrintable: String,
  totalTaxableAmount: Number,
  showModal: Boolean
});

const investedAmount = ref("");
const aitAmount = ref("");
const printPreview = ref(false);

const closePreview = () => {
  if (printPreview.value === true) {
    printPreview.value = false;
  }
};

const remainingAmountAfterFirstThreeLakh = computed(() => {
  return props.totalTaxableAmount >= 350000 ? props.totalTaxableAmount - 350000 : 0;
});

// For first one lakh
const nextOneLakhTaxableIncome = computed(() => {
  let payableAmount = 0;
  let remainingAmount = 0;

  if (remainingAmountAfterFirstThreeLakh.value < 100000) {
    payableAmount = Math.round((remainingAmountAfterFirstThreeLakh.value * 5) / 100);
    remainingAmount = 0;
  }
  if (remainingAmountAfterFirstThreeLakh.value >= 100000) {
    payableAmount = Math.round((100000 * 5) / 100);
    remainingAmount = remainingAmountAfterFirstThreeLakh.value - 100000;
  }

  return {
    payableAmount: payableAmount,
    remainingAmount: remainingAmount,
  };
});

// For next three lakh
const nextThreeLakhTaxableIncome = computed(() => {
  let payableAmount = 0;
  let remainingAmount = 0;
  if (nextOneLakhTaxableIncome.value.remainingAmount < 300000) {
    payableAmount = Math.round(
      (nextOneLakhTaxableIncome.value.remainingAmount * 10) / 100
    );
    remainingAmount = 0;
  }
  if (nextOneLakhTaxableIncome.value.remainingAmount >= 300000) {
    payableAmount = Math.round((300000 * 10) / 100);
    remainingAmount = nextOneLakhTaxableIncome.value.remainingAmount - 300000;
  }

  return {
    payableAmount: payableAmount,
    remainingAmount: remainingAmount,
  };
});

// For next four lakh
const nextFourLakhTaxableIncome = computed(() => {
  let payableAmount = 0;
  let remainingAmount = 0;
  if (nextThreeLakhTaxableIncome.value.remainingAmount < 400000) {
    payableAmount = Math.round(
      (nextThreeLakhTaxableIncome.value.remainingAmount * 15) / 100
    );
    remainingAmount = 0;
  }
  if (nextThreeLakhTaxableIncome.value.remainingAmount >= 400000) {
    payableAmount = Math.round((400000 * 15) / 100);
    remainingAmount = nextThreeLakhTaxableIncome.value.remainingAmount - 400000;
  }

  return {
    payableAmount: payableAmount,
    remainingAmount: remainingAmount,
  };
});

// For next five lakh
const nextFiveLakhTaxableIncome = computed(() => {
  let payableAmount = 0;
  let remainingAmount = 0;
  if (nextFourLakhTaxableIncome.value.remainingAmount < 500000) {
    payableAmount = Math.round(
      (nextFourLakhTaxableIncome.value.remainingAmount * 20) / 100
    );
    remainingAmount = 0;
  }
  if (nextFourLakhTaxableIncome.value.remainingAmount >= 500000) {
    payableAmount = Math.round((500000 * 20) / 100);
    remainingAmount = nextFourLakhTaxableIncome.value.remainingAmount - 500000;
  }

  return {
    payableAmount: payableAmount,
    remainingAmount: remainingAmount,
  };
});

const restTaxableIncome = computed(() => {
  return nextFiveLakhTaxableIncome.value.remainingAmount > 0
    ? Math.round((nextFiveLakhTaxableIncome.value.remainingAmount * 25) / 100)
    : 0;
});

const totalPayable = computed(() => {
  return (
    nextOneLakhTaxableIncome.value.payableAmount +
    nextThreeLakhTaxableIncome.value.payableAmount +
    nextFourLakhTaxableIncome.value.payableAmount +
    nextFiveLakhTaxableIncome.value.payableAmount +
    restTaxableIncome.value
  );
});

const allowedInvestedAmount = computed(() => {
  return props.totalTaxableAmount >= 300000
    ? Math.round((props.totalTaxableAmount * 20) / 100)
    : 0;
});

const applicableDiscount = computed(() => {
  let discount = 0;
  if (investedAmount.value <= allowedInvestedAmount.value) {
    discount = Math.round((+investedAmount.value * 15) / 100);
  }
  if (investedAmount.value > allowedInvestedAmount.value) {
    discount = Math.round((allowedInvestedAmount.value * 15) / 100);
  }
  return discount;
});

const actualTotalPayableAmount = computed(() => {
  return totalPayable.value - (applicableDiscount.value + +aitAmount.value);
});

const resetData = () => {
  alert("Are you sure, you want to reset?")
  location.reload()
}

const showPrintPreview = () => {
  printPreview.value = !printPreview.value ? true : false
  window.scrollTo({ top: 0, behavior: 'smooth' });
}
</script>

<style scoped>

.bg-gray-800-opacity {
  background-color: #040507d0;
}

/* animation for vue transition tag */

.fade-up-down-enter-active {
  transition: all 0.3s ease;
}

.fade-up-down-leave-active {
  transition: all 0.3s ease;
}

.fade-up-down-enter {
  transform: translateY(10%);
  opacity: 0;
}

.fade-up-down-leave-to {
  transform: translateY(10%);
  opacity: 0;
}

.fade-enter-active {
  -webkit-transition: opacity 2s;
  transition: opacity 0.3s;
}

.fade-leave-active {
  transition: opacity 0.3s;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
