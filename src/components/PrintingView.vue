<template>
  <div id="printArea" class="p-2 sm:p-5 absolute left-0 right-0 top-0 text-black w-full overflow-hidden">
    <div class="bg-white px-4 pt-6 pb-10 mx-auto print-preview-width">
      <div class="flex justify-between gap-4">
          <div class="space-y-1">
            <p class="text-[15px] font-bold">Assessment: <span class="ml-1 font-semibold capitalize">{{ assessmentYear }}</span></p>
            <p class="text-[15px] font-bold">Name: <span class="ml-1 font-semibold capitalize">{{ employeeName }}</span></p>
            <p class="text-[15px] font-bold">Company: <span class="ml-1 font-semibold capitalize">{{ companyName }}</span></p>
            <p class="text-[15px] font-bold">Designation: <span class="ml-1 font-semibold capitalize">{{ designation }}</span></p>
          </div>
          <div class="block sm:flex justify-end gap-2">
            <div><button @click="print" class="bg-teal-600 text-white text-[13px] sm:text-sm font-semibold rounded-sm w-16 sm:w-24 my-1 md:my-0 py-1">Print</button></div>
            <div><button @click="props.closePreview" class="bg-gray-600 text-white text-[13px] sm:text-sm font-semibold rounded-sm w-16 sm:w-24 my-1 md:my-0 py-1">Cancel</button></div>
          </div>
      </div>

      <div>
        <div class="py-1 capitalize">
          <h3 class="text-lg font-bold text-center text-gray-700">salary information</h3>
        </div>
        <div class="table-overflow">
            <table class="border table-width">
                <colgroup>
                  <col span="1" style="width: 16%" />
                  <col span="1" style="width: 12%" />
                  <col span="1" style="width: 18%" />
                  <col span="1" style="width: 18%" />
                  <col span="1" style="width: 18%" />
                  <col span="1" style="width: 18%" />
                </colgroup>
                <thead>
                    <tr class="border-t border-b bg-blue-100 print-color">
                    <th scope="col" class="left-align-td">Month</th>
                    <th scope="col" class="left-align-td text-center">Gross Salary</th>
                    <th scope="col" class="left-align-td text-center">Basic</th>
                    <th scope="col" class="left-align-td text-center">Home rent</th>
                    <th scope="col" class="left-align-td text-center">Medical</th>
                    <th scope="col" class="left-align-td text-center">Conveyance</th>
                  </tr>
                </thead>
                <tbody>
                    <tr
                    v-for="salary in salaryData"
                    :key="salary.id"     
                    class="border-b print-preview-input"
                  >
                  <td class="px-3 text-[13px] w-2/12">
                    <span>{{ salary.month }},</span>
                    <span v-if="salary.month == 'January' || salary.month == 'February' || salary.month == 'March' || salary.month == 'April' || salary.month == 'May' || salary.month == 'June'">&nbsp;{{ (new Date().getFullYear())}}</span>
                    <span v-if="salary.month == 'July' || salary.month == 'August' || salary.month == 'September' || salary.month == 'October' || salary.month == 'November' || salary.month == 'December'">&nbsp;{{ (new Date().getFullYear()) - 1 }}</span>
                  </td>
                  <td class="text-[13px] border-l border-r">
                      <input v-model.trim="salary.grossSalary" readonly type="number" class="bg-transparent focus:outline-none border border-gray-500 rounded-sm px-3 text-right w-full  print-preview-gross-salary">
                  </td>
                  <td class="text-[13px] border-l border-r">
                      <div class="input-group-wrapper py-1">
                      <input v-model.trim="salary.basicPercentage" readonly type="number" class="focus:outline-none w-[40%] px-3">
                      <input value="%" readonly type="text" class="focus:outline-none px-3 w-1/3">
                      <input v-model="salary.basicSalary" readonly type="number" class="focus:outline-none px-3 text-right w-full">
                      </div>
                  </td>
                  <td class="text-[13px] border-l border-r">
                      <div class="input-group-wrapper py-1 w-full">
                      <input v-model.trim="salary.homeRentPercentage" readonly type="number" class="focus:outline-none w-[40%] px-3">
                      <input value="%" readonly type="text" class="focus:outline-none px-3 w-1/3">
                      <input v-model="salary.homeRent" readonly type="number" class="focus:outline-none px-3 text-right w-full">
                      </div>
                  </td>
                  <td class="text-[13px] border-l border-r">
                      <div class="input-group-wrapper py-1 w-full">
                      <input v-model.trim="salary.medicalPercentage" readonly type="number" class="focus:outline-none w-[40%] px-3">
                      <input value="%" readonly type="text" class="focus:outline-none px-3 w-1/3">
                      <input v-model="salary.medicalAllowance" readonly type="number" class="focus:outline-none px-3 text-right w-full">
                      </div>
                  </td>
                  <td class="text-[13px] border-l">
                      <div class="input-group-wrapper py-1 w-full">
                      <input v-model.trim="salary.conveyancePercentage" readonly type="number" class="focus:outline-none px-3 w-[40%]">
                      <input value="%" readonly type="text" class="focus:outline-none px-3 w-2/3">
                      <input v-model="salary.conveyanceAllowance" readonly type="number" class="focus:outline-none px-3 text-right w-full">
                      </div>
                  </td>
                  </tr>
                </tbody>
                <tfoot>
                  <tr class="bg-[#7f92d738] print-color">
                    <td scope="col" class="text-sm font-bold px-2 py-1 text-left">Total</td>
                    <td scope="col" class="text-[13px] font-bold px-2 py-1 text-center">
                      {{ getSum.totalGrossSalary }}
                    </td>
                    <td scope="col" class="text-[13px] font-bold px-2 py-1 text-right">
                      {{ getSum.totalBasicSalary }}
                    </td>
                    <td scope="col" class="text-[13px] font-bold px-2 py-1 text-right">
                      {{ getSum.totalHomeRent }}
                    </td>
                    <td scope="col" class="text-[13px] font-bold px-2 py-1 text-right">
                      {{ getSum.totalMedicalAllowance }}
                    </td>
                    <td scope="col" class="text-[13px] font-bold px-2 py-1 text-right">
                      {{ getSum.totalConveyanceAllowance }}
                    </td>
                  </tr>
                  <tr class="bg-[#7297cf70] print-color">
                    <td colspan="2" class="text-sm font-bold px-2 py-1 text-left">Tax Exemption</td>
                    <td scope="col" class="text-[13px] font-bold px-2 py-1 text-right">0</td>
                    <td scope="col" class="text-[13px] font-bold px-2 py-1 text-right">
                      {{ homeRentTaxExemption }}
                    </td>
                    <td scope="col" class="text-[13px] font-bold px-2 py-1 text-right">
                      {{ medicalTaxExemption }}
                    </td>
                    <td scope="col" class="text-[13px] font-bold px-2 py-1 text-right">
                      {{ conveyanceTaxExemption }}
                    </td>
                  </tr>
                  <tr class="bg-[#7297cfb8] print-color">
                    <td colspan="2" class="text-sm font-bold px-2 py-1 text-left">Taxable</td>
                    <td scope="col" class="text-[13px] font-bold px-2 py-1 text-right">{{ taxableBasicSalary }}</td>
                    <td scope="col" class="text-[13px] font-bold px-2 py-1 text-right">
                      {{ taxableHomeRent }}
                    </td>
                    <td scope="col" class="text-[13px] font-bold px-2 py-1 text-right">
                      {{ taxableMedicalAllowance }}
                    </td>
                    <td scope="col" class="text-[13px] font-bold px-2 py-1 text-right">
                      {{ taxableConveyanceAllowance }}
                    </td>
                  </tr>
                </tfoot>
            </table>
        </div>
      </div>
      <div class="mt-8 mb-6 bg-[#e9e6e6] print-color p-2">
        <div class="block sm:flex items-center justify-between space-y-1 sm:space-y-0">
          <p class="font-semibold text-[14px]">Festive Bonus & Other Incomes:&nbsp;&nbsp;<span class="font-bold">{{ festiveBonusOne ? festiveBonusOne : 0 }}</span></p>
          <p class="font-bold text-[14px]">Total Taxable Income:&nbsp;&nbsp;<span class="font-bold">{{ props.totalTaxableAmount }}</span></p>
        </div>
      </div>
      <div class="gap-10 printing-grid">
        <div class="w-full">
          <div class="py-1 lg:mt-0">
            <p class="text-[17px] font-bold">Payable Amount</p>
          </div>
          <div class="">
            <table class="w-full text-[13px] border">
              <thead class="border-t border-b bg-blue-100 print-color">
                <th class="text-left py-1 px-2">For</th>
                <th class=" text-center py-1 px-2">Rate</th>
                <th class="text-right py-1 px-2">Payable</th>
                <th class="text-right py-1 px-2">Remaining</th>
              </thead>
              <tbody>
                <tr>
                  <td class="px-2 py-2 border-b text-sm">
                    First <span class="font-bold">3</span> Lakh
                  </td>
                  <td class="text-center px-2 py-2 border-b">0</td>
                  <td class="text-right px-2 py-2 border-b">0</td>
                  <td class="text-right px-2 py-2 border-b">
                    {{ props.remainingAmountAfterFirstThreeLakh }}
                  </td>
                </tr>
                <tr>
                  <td class="px-2 py-2 border-b text-sm">
                    Next <span class="font-bold">1</span> Lakh
                  </td>
                  <td class="text-center px-2 py-2 border-b">5%</td>
                  <td class="text-right px-2 py-2 border-b">
                    {{ props.nextOneLakhTaxableIncome.payableAmount }}
                  </td>
                  <td class="text-right px-2 py-2 border-b">
                    {{ props.nextOneLakhTaxableIncome.remainingAmount }}
                  </td>
                </tr>
                <tr>
                  <td class="px-2 py-2 border-b text-sm">
                    Next <span class="font-bold">3</span> Lakh
                  </td>
                  <td class="text-center px-2 py-2 border-b">10%</td>
                  <td class="text-right px-2 py-2 border-b">
                    {{ props.nextThreeLakhTaxableIncome.payableAmount }}
                  </td>
                  <td class="text-right px-2 py-2 border-b">
                    {{ props.nextThreeLakhTaxableIncome.remainingAmount }}
                  </td>
                </tr>
                <tr>
                  <td class="px-2 py-2 border-b text-sm">
                    Next <span class="font-bold">4</span> Lakh
                  </td>
                  <td class="text-center px-2 py-2 border-b">15%</td>
                  <td class="text-right px-2 py-2 border-b">
                    {{ props.nextFourLakhTaxableIncome.payableAmount }}
                  </td>
                  <td class="text-right px-2 py-2 border-b">
                    {{ props.nextFourLakhTaxableIncome.remainingAmount }}
                  </td>
                </tr>
                <tr>
                  <td class="px-2 py-2 border-b text-sm">
                    Next <span class="font-bold">5</span> Lakh
                  </td>
                  <td class="text-center px-2 py-2 border-b">20%</td>
                  <td class="text-right px-2 py-2 border-b">
                    {{ props.nextFiveLakhTaxableIncome.payableAmount }}
                  </td>
                  <td class="text-right px-2 py-2 border-b">
                    {{ props.nextFiveLakhTaxableIncome.remainingAmount }}
                  </td>
                </tr>
                <tr>
                  <td class="px-2 py-2 text-sm font-bold border-b">Rest Amount</td>
                  <td class="text-center px-2 py-2 border-b">25%</td>
                  <td class="text-right px-2 py-2 border-b">
                    {{ props.restTaxableIncome }}
                  </td>
                  <td class="text-right px-2 py-2 border-b">0</td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
        <div class="mt-8 w-full">
          <table class="w-full">
              <tbody>
                  <tr>
                      <td><p class="font-bold text-[16px] py-1">Total Payable:</p></td>
                      <td><p class="font-bold text-[16px] py-1 text-right">{{ props.totalPayable }}</p></td>
                  </tr>
                  <tr>
                      <td><p class="font-semibold text-[14px] py-1">Allowable Investment:</p></td>
                      <td><p class="font-semibold text-[14px] py-1 text-right">{{ props.allowedInvestedAmount ? props.allowedInvestedAmount : 0 }}</p></td>
                  </tr>
                  <tr>
                      <td><p class="font-semibold text-[14px] py-1">Total Investment:</p></td>
                      <td><p class="font-semibold text-[14px] py-1 text-right">{{ props.investedAmount ? props.investedAmount : 0 }}</p></td>
                  </tr>
                  <tr>
                      <td><p class="font-semibold text-[14px] py-1">Redemption for Investment:</p></td>
                      <td><p class="font-semibold text-[14px] py-1 text-right">{{ props.applicableDiscount ? props.applicableDiscount : 0 }}</p></td>
                  </tr>
                  <tr>
                      <td><p class="font-bold text-[14px] py-1">AIT:</p></td>
                      <td><p class="font-bold text-[14px] py-1 text-right">{{ props.aitAmount ? props.aitAmount : 0 }}</p></td>
                  </tr>
                  <tr>
                    <td class="border-t pt-2">
                      <p class="font-bold text-[17px] text-blue-800 print-color">Final Payable Amount:</p>
                    </td>
                    <td class="border-t pt-2">
                      <p class="font-bold text-[17px] text-blue-800 text-right print-color">{{ props.actualTotalPayableAmount }}</p>
                    </td>
                  </tr>
              </tbody>
          </table>
        </div>
      </div>
  
    </div>
  </div>
</template>

<script setup>
import { inject } from 'vue';

const props = defineProps({
  closePreview: Function,
  noPrint: String,
  totalTaxableAmount: Number,
  remainingAmountAfterFirstThreeLakh: Number,
  nextOneLakhTaxableIncome: Object,
  nextThreeLakhTaxableIncome: Object,
  nextFourLakhTaxableIncome: Object,
  nextFiveLakhTaxableIncome: Object,
  restTaxableIncome: Number,
  totalPayable: Number,
  investedAmount: Number,
  allowedInvestedAmount: Number,
  applicableDiscount: Number,
  aitAmount: Number,
  actualTotalPayableAmount: Number,
});

const print = () => {
  document.getElementById(props.noPrint).className += " noPrint";
  window.print();
};

const salaryData = inject('salaryData')
const employeeName = inject('employeeName')
const designation = inject('designation')
const department = inject('department')
const companyName = inject('companyName')
const assessmentYear = inject('assessmentYear')
const getSum = inject('getSum')
const festiveBonusOne = inject('festiveBonusOne')
const festiveBonusTwo = inject('festiveBonusTwo')
const festiveBonusThree = inject('festiveBonusThree')
const homeRentTaxExemption = inject('homeRentTaxExemption')
const medicalTaxExemption = inject('medicalTaxExemption')
const conveyanceTaxExemption = inject('conveyanceTaxExemption')
const taxableBasicSalary = inject('taxableBasicSalary')
const taxableHomeRent = inject('taxableHomeRent')
const taxableMedicalAllowance = inject('taxableMedicalAllowance')
const taxableConveyanceAllowance = inject('taxableConveyanceAllowance')

</script>

