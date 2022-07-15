<script>
import Buttons from './elements/Buttons.vue';
    export default {
  components: { Buttons },

        props: {
            rows: null,
            offset: 0,
        },
    }
</script>

<template>
    <div dir="rtl" class="font-farsi sm:-mx-6 lg:-mx-8">
        <div class="py-2 sm:px-6 lg:px-8">
            <div class="">
              <table class="table-fixed border">
                <thead class="bg-white border-b">
                    <tr>
                    <th scope="col" class="text-sm font-medium text-gray-900 px-6 py-4 text-right" width="5%">
                        #
                    </th>
                    <th scope="col" class="text-sm font-medium font-bold text-gray-900 px-6 py-4 text-right">
                        نام و نام خانوادگی
                    </th>
                    <th scope="col" class="text-sm font-medium font-bold text-gray-900 px-6 py-4 text-right" width="10%">
                        شماره داخلی
                    </th>
                    <th scope="col" class="text-sm font-medium font-bold text-gray-900 px-6 py-4 text-right">
                        شماره تلفن کامل                    
                    </th>
                    <th scope="col" class="text-sm font-medium font-bold text-gray-900 px-6 py-4 text-right">
                        پست
                    </th>
                     <th scope="col" class="text-sm font-medium font-bold text-gray-900 px-6 py-4 text-right">
                        واحد سازمانی
                    </th>
                    <th scope="col" class="text-sm font-medium font-bold text-gray-900 px-6 py-4 text-right">
                        مسئولیت
                    </th>
                     <th scope="col" class="text-sm font-medium font-bold text-gray-900 px-6 py-4 text-right" width="8%">
                        توضیحات
                    </th>

                    </tr>
                </thead>
                <tbody>
                    <tr class="bg-gray-100 border-b
                                hover:text-gray-900 hover:ring-1 mb-20" 
                        v-for="(row, index) in rows" :key="index">
                        <td class="px-6 py-4 text-sm font-medium text-gray-800">{{ index+offset+1 }}</td>
                        <td class="text-sm text-gray-800 font-light px-6 py-4">
                            {{ row.full_name }}
                        </td>
                        <td class="text-sm text-gray-800 font-light px-6 py-4">
                            {{ row.extension }}
                        </td>
                        <td class="text-sm text-gray-800 font-light px-6 py-4">
                            {{ row.complete_number }}
                        </td>
                        <td class="text-sm text-gray-800 font-light px-6 py-4">
                            {{ row.position }}
                        </td>
                        <td class="text-sm text-gray-800 font-light px-6 py-4">
                            {{ row.dep }}
                        </td>
                        <td class="text-sm text-gray-800 font-light px-6 py-4 break-word">
                            {{ row.duties }}
                        </td>
                        <td class="text-sm text-gray-800 font-light px-6 py-4">
                           {{ row.description }}
                        </td>
                    </tr>
                </tbody>
                </table>
            </div>
        </div>
    </div>
</template>>

<style>
</style>