<template>
  <div id="nonPrintable" class="px-2 md:px-5 pt-6 pb-10 app-primary-text bg-[#DFDFDF] min-h-screen">
    <div class="">
      <div class="block lg:grid grid-cols-12 gap-5 space-y-1">
        <div class="col-span-12 lg:col-span-4 2xl:col-span-3">
          <div class="flex gap-2 items-end">
            <label for="" class="app-secondary-text leading-none text-[14px] xl:text-[15px] w-[120px] lg:w-auto">Assessment:</label>
            <select
              v-model="assessmentYear"
              class="focus:outline-none bg-transparent px-1 border-b border-gray-500 rounded-sm capitalize text-blue-800 w-[220px] lg:w-[240px] xl:w-[250px] ">
              <option :value="assessmentYearTwo" class="text-[#1c2a3a]">{{ assessmentYearTwo }}</option>
              <option :value="assessmentYearOne" class="text-[#1c2a3a]">{{ assessmentYearOne }}</option>
            </select>
          </div>
        </div>
        <div class="col-span-12 lg:col-span-4 2xl:col-span-3">
          <div class="flex gap-2 items-end">
            <label for="" class="app-secondary-text leading-none text-[14px] xl:text-[15px] w-[120px] lg:w-auto">Name:</label>
            <input
              v-model="employeeName"
              type="text"
              class="focus:outline-none bg-transparent px-1 border-b border-gray-500 rounded-sm capitalize text-blue-800 w-[220px] lg:w-[240px] xl:w-[250px]"
            />
          </div>
        </div>
        <div class="col-span-12 lg:col-span-4 2xl:col-span-3">
          <div class="flex gap-2 items-end">
            <label for="" class="app-secondary-text leading-none text-[14px] xl:text-[15px] w-[120px] lg:w-auto">Company:</label>
            <input
              v-model="companyName"
              type="text"
              class="focus:outline-none bg-transparent px-1 border-b border-gray-500 rounded-sm capitalize text-blue-800 w-[220px] lg:w-[240px] xl:w-[250px]"
            />
          </div>
        </div>
        <div class="col-span-12 lg:col-span-4 2xl:col-span-3">
          <div class="flex gap-2 items-end">
            <label for="" class="app-secondary-text leading-none text-[14px] xl:text-[15px] w-[120px] lg:w-auto">Designation:</label>
            <input
              v-model="designation"
              type="text"
              class="focus:outline-none bg-transparent px-1 border-b border-gray-500 rounded-sm capitalize text-blue-800 w-[220px] lg:w-[240px] xl:w-[250px]"
            />
          </div>
        </div>
      </div>
    </div>
    <div class="grid grid-cols-12 gap-4 mt-6">
      <div class="xl:col-span-9 lg:col-span-8 col-span-12">
        <div class="app-secondary-bg">
          <div class="py-1 bg-gold capitalize">
            <h3 class="text-lg md:text-xl font-bold text-center text-gray-700">
              salary information
            </h3>
          </div>
          <div class="overflow-x-auto app-scroll-bar">
            <table class="w-[954px] xl:w-full">
              <colgroup>
                <col span="1" style="width: 16%" />
                <col span="1" style="width: 12%" />
                <col span="1" style="width: 18%" />
                <col span="1" style="width: 18%" />
                <col span="1" style="width: 18%" />
                <col span="1" style="width: 18%" />
              </colgroup>
              <thead class="bg-[#1e344e]">
                <tr>
                  <th scope="col" class="left-align-td">Month</th>
                  <th scope="col" class="left-align-td">Gross Salary</th>
                  <th scope="col" class="left-align-td">Basic</th>
                  <th scope="col" class="left-align-td">Home rent</th>
                  <th scope="col" class="left-align-td">Medical</th>
                  <th scope="col" class="left-align-td">Conveyance</th>
                </tr>
              </thead>
              <tbody>
                <tr
                  v-for="salary in salaryData"
                  :key="salary.id"
                  class="border-b border-gray-600"
                >
                  <SalaryTableRow :salary="salary" />
                </tr>
                <tr>
                  <td class="px-3 py-2 text-sm w-2/12">Festival Bonus<br>& Other Income</td>
                  <td class="common-td">
                    <input
                      v-model="festiveBonusOne"
                      type="number"
                      class="bg-transparent focus:outline-none py-2 border border-gray-500 rounded-sm px-3 text-right w-full  print-preview-gross-salary"
                    />
                  </td>
                  <td></td>
                  <td></td>
                  <td></td>
                  <td></td>
                </tr>
              </tbody>
              <tfoot>
                <tr class="bg-blue-500">
                  <td scope="col" class="left-align-td">Total</td>
                  <td scope="col" class="right-align-td"></td>
                  <td scope="col" class="right-align-td">
                    {{ getSum.totalBasicSalary }}
                  </td>
                  <td scope="col" class="right-align-td">
                    {{ getSum.totalHomeRent }}
                  </td>
                  <td scope="col" class="right-align-td">
                    {{ getSum.totalMedicalAllowance }}
                  </td>
                  <td scope="col" class="right-align-td">
                    {{ getSum.totalConveyanceAllowance }}
                  </td>
                </tr>
                <tr class="bg-blue-900">
                  <td colspan="2" class="left-align-td">Tax Exemption</td>
                  <td scope="col" class="right-align-td">0</td>
                  <td scope="col" class="right-align-td">
                    {{ homeRentTaxExemption }}
                  </td>
                  <td scope="col" class="right-align-td">
                    {{ medicalTaxExemption }}
                  </td>
                  <td scope="col" class="right-align-td">
                    {{ conveyanceTaxExemption }}
                  </td>
                </tr>
                <tr class="bg-gray-600">
                  <td colspan="2" class="left-align-td">Taxable</td>
                  <td scope="col" class="right-align-td">{{ taxableBasicSalary }}</td>
                  <td scope="col" class="right-align-td">
                    {{ taxableHomeRent }}
                  </td>
                  <td scope="col" class="right-align-td">
                    {{ taxableMedicalAllowance }}
                  </td>
                  <td scope="col" class="right-align-td">
                    {{ taxableConveyanceAllowance }}
                  </td>
                </tr>
              </tfoot>
            </table>
          </div>
        </div>
        <div class="bg-[#1c2a3a] py-4 px-4">
          <div class="block md:flex justify-between items-center">
            <div>
              <p
                class="font-semibold mt-5 md:mt-0 text-left sm:text-right md:text-left"
              >
                <span class="text-[15px] lg:text-lg text-blue-300"
                  >Taxable Income :&nbsp;</span
                >
                <span class="text-xl">{{ totalTaxableAmount }}</span>
              </p>
            </div>
          </div>
        </div>
      </div>
      <div class="xl:col-span-3 lg:col-span-4 col-span-12">
        <div class="app-secondary-bg">
          <PayableAmountCalculation
            nonPrintable="nonPrintable"
            :totalTaxableAmount="totalTaxableAmount"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import sourceData from "../../data.json";
import SalaryTableRow from "@/components/SalaryTableRow.vue";
import PayableAmountCalculation from "@/components/PayableAmountCalculation.vue";
import { ref, computed, provide } from "vue";

const salaryData = ref([...sourceData]);
const employeeName = ref("");
const designation = ref("");
const department = ref("");
const companyName = ref("");
const festiveBonusOne = ref("");
const festiveBonusTwo = ref("");
const festiveBonusThree = ref("");


const getSum = computed(() => {
  let totalGrossSalary = 0;
  let totalBasicSalary = 0;
  let totalHomeRent = 0;
  let totalMedicalAllowance = 0;
  let totalConveyanceAllowance = 0;

  for (let i = 0; i < salaryData.value.length; i++) {
    if (salaryData.value[i].grossSalary > 0) {
      totalGrossSalary += salaryData.value[i].grossSalary;
      totalBasicSalary += salaryData.value[i].basicSalary;
      totalHomeRent += salaryData.value[i].homeRent;
      totalMedicalAllowance += salaryData.value[i].medicalAllowance;
      totalConveyanceAllowance += salaryData.value[i].conveyanceAllowance;
    }
  }

  return {
    totalGrossSalary: totalGrossSalary,
    totalBasicSalary: totalBasicSalary,
    totalHomeRent: totalHomeRent,
    totalMedicalAllowance: totalMedicalAllowance,
    totalConveyanceAllowance: totalConveyanceAllowance,
  };
});

// Calculation of Tax Exemption
// Home Rent
const homeRentTaxExemption = computed(() => {
  let getHomeRentTaxExemption = Math.round((getSum.value.totalBasicSalary * 50) / 100);
  if (getHomeRentTaxExemption < 300000) {
    return getHomeRentTaxExemption;
  } else {
    return 300000;
  }
});

console.log(typeof homeRentTaxExemption.value, "type");
// Medical
const medicalTaxExemption = computed(() => {
  let getMedicalTaxExemption = Math.round((getSum.value.totalBasicSalary * 10) / 100);
  if (getMedicalTaxExemption < 120000) {
    return getMedicalTaxExemption;
  } else {
    return 120000;
  }
});

// Conveyance
const conveyanceTaxExemption = computed(() => {
  if (getSum.value.totalConveyanceAllowance < 30000) {
    return 0;
  } else {
    return 30000;
  }
});

// Calculation of Taxable Amount
// Basic Salary
const taxableBasicSalary = computed(() => {
  let taxableBasic = 0;
  if (getSum.value.totalBasicSalary) {
    taxableBasic = getSum.value.totalBasicSalary - 0;
  }
  return taxableBasic;
});

// Home Rent
const taxableHomeRent = computed(() => {
  return getSum.value.totalHomeRent - homeRentTaxExemption.value;
});

// Medical
const taxableMedicalAllowance = computed(() => {
  return getSum.value.totalMedicalAllowance - medicalTaxExemption.value;
});

// Conveyance
const taxableConveyanceAllowance = computed(() => {
  return getSum.value.totalConveyanceAllowance - conveyanceTaxExemption.value;
});

// Calculation of Total Taxable Amount
const totalTaxableAmount = computed(() => {
  let getTotalTaxableAmount = 0;
  // if (taxableBasicSalary.value) {
  //   getTotalTaxableAmount =
  //     taxableBasicSalary.value +
  //     taxableHomeRent.value +
  //     taxableMedicalAllowance.value +
  //     taxableConveyanceAllowance.value +
  //     +festiveBonusOne.value +
  //     +festiveBonusTwo.value;
  // }

  if (
    (festiveBonusOne.value && festiveBonusOne.value > 0) ||
    (festiveBonusTwo.value && festiveBonusTwo.value > 0) ||
    (festiveBonusThree.value && festiveBonusThree.value > 0)
  ) {
    getTotalTaxableAmount =
      taxableBasicSalary.value +
      taxableHomeRent.value +
      taxableMedicalAllowance.value +
      taxableConveyanceAllowance.value +
      +festiveBonusOne.value +
      +festiveBonusTwo.value + 
      +festiveBonusThree.value;
  } else {
    getTotalTaxableAmount =
      taxableBasicSalary.value +
      taxableHomeRent.value +
      taxableMedicalAllowance.value +
      taxableConveyanceAllowance.value;
  }
  return getTotalTaxableAmount;
});

const assessmentYearOne = computed(() => {
  return ((new Date().getFullYear() - 1) + ' - ' + (new Date().getFullYear()))
})
const assessmentYearTwo = computed(() => {
  return ((new Date().getFullYear()) + ' - ' + (parseInt(new Date().getFullYear()) + 1))
})

const assessmentYear = ref(assessmentYearTwo.value);

provide('salaryData', salaryData)
provide('employeeName', employeeName)
provide('designation', designation)
provide('department', department)
provide('companyName', companyName)
provide('assessmentYear', assessmentYear)
provide('getSum', getSum)
provide('festiveBonusOne', festiveBonusOne)
provide('festiveBonusTwo', festiveBonusTwo)
provide('festiveBonusThree', festiveBonusThree)
provide("homeRentTaxExemption", homeRentTaxExemption)
provide("medicalTaxExemption", medicalTaxExemption)
provide("conveyanceTaxExemption", conveyanceTaxExemption)
provide("taxableBasicSalary", taxableBasicSalary)
provide("taxableHomeRent", taxableHomeRent)
provide("taxableMedicalAllowance", taxableMedicalAllowance)
provide("taxableConveyanceAllowance", taxableConveyanceAllowance)
</script>

<style scoped>

</style>
