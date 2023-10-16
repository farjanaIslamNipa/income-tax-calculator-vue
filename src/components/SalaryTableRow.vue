<template>
    <td class="px-3 py-2 text-sm w-2/12">
        <span>{{ salary.month }},</span>
        <span v-if="salary.month == 'January' || salary.month == 'February' || salary.month == 'March' || salary.month == 'April' || salary.month == 'May' || salary.month == 'June'">&nbsp;
            {{ (new Date().getFullYear()) }}
        </span>
        <span v-if="salary.month == 'July' || salary.month == 'August' || salary.month == 'September' || salary.month == 'October' || salary.month == 'November' || salary.month == 'December'">&nbsp;
            {{ (new Date().getFullYear()) - 1 }}
        </span>
    </td>
    <td class="common-td">
        <input v-model.trim="salaryInfo.grossSalary" type="number" class="bg-transparent focus:outline-none py-2 border border-gray-500 rounded-sm px-3 text-right w-full  print-preview-gross-salary">
    </td>
    <td class="common-td">
        <div class="input-group-wrapper">
        <input v-model.trim="salaryInfo.basicPercentage" type="number" class="common-input w-2/3">
        <input v-model="percentage" readonly type="text" class="percentage-input w-1/3">
        <input v-model="salaryInfo.basicSalary" readonly type="number" class="common-input text-right w-full">
        </div>
    </td>
    <td class="common-td">
        <div class="input-group-wrapper w-full">
        <input v-model.trim="salaryInfo.homeRentPercentage" type="number" class="common-input w-2/3">
        <input v-model="percentage" readonly type="text" class="percentage-input w-1/3">
        <input v-model="salaryInfo.homeRent" readonly type="number" class="common-input text-right w-full">
        </div>
    </td>
    <td class="common-td">
        <div class="input-group-wrapper w-full">
        <input v-model.trim="salaryInfo.medicalPercentage" type="number" class="common-input w-2/3">
        <input v-model="percentage" readonly type="text" class="percentage-input w-1/3">
        <input v-model="salaryInfo.medicalAllowance" readonly type="number" class="common-input text-right w-full">
        </div>
    </td>
    <td class="common-td">
        <div class="input-group-wrapper w-full">
        <input v-model.trim="salaryInfo.conveyancePercentage" type="number" class="common-input w-2/3">
        <input v-model="percentage" readonly type="text" class="percentage-input w-1/3">
        <input v-model="salaryInfo.conveyanceAllowance" readonly type="number" class="common-input text-right w-full">
        </div>
    </td>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue';

const percentage = ref('%')

const { salary, resetValue } = defineProps({
    salary: Object
})

const salaryInfo = ref({})

onMounted(() => {salaryInfo.value = salary})

watch(salary, currentSalary => {
    if(currentSalary.grossSalary && currentSalary.grossSalary > 0){
        salaryInfo.value.basicSalary =  Math.round(salaryInfo.value.grossSalary * salaryInfo.value.basicPercentage / 100)
        salaryInfo.value.homeRent =  Math.round(salaryInfo.value.grossSalary * salaryInfo.value.homeRentPercentage / 100)
        salaryInfo.value.medicalAllowance =  Math.round(salaryInfo.value.grossSalary * salaryInfo.value.medicalPercentage / 100)
        salaryInfo.value.conveyanceAllowance =  Math.round(salaryInfo.value.grossSalary * salaryInfo.value.conveyancePercentage / 100)
    }else {
        salaryInfo.value.basicSalary = ''
        salaryInfo.value.homeRent = ''
        salaryInfo.value.medicalAllowance = ''
        salaryInfo.value.conveyanceAllowance = ''
    }
})

</script>

<style scoped>

</style>