<script>
  import DropDown from './elements/DropDown.vue';
  import InputText from './elements/InputText.vue';
  import TableResults from './TableResults.vue';
  import { serverUrl } from '../../settings';
  import axios from 'axios';

   export default {
        data() {
          return {
            family: '',
            searchStart: false,
            mainDepOptions: Object,
            mainSubDepOptions: Object,
            subSubDepOptions: Object,
            errorMessage: '',
            searchResults: null,
            finalDep: '',
            mainDep: '',
            subDep: '',
            subSubDep: '',
          }
        },
        components: {
          DropDown,
          InputText,
          TableResults,
        },
        methods:{
          depList0(){
             axios.get(serverUrl+'/tel/deps/?level=0')
                .then(response => {
                    this.mainDepOptions = response.data;
                })
                .catch(error => {
                        this.errorMessage = error //'خطایی در گرفتن اطلاعات کاربر رخ داد'; //error.data
                });
          },
          depList1(sdep){
             axios.get(serverUrl+'/tel/deps/?level=1'+'&super-dep='+sdep)
                .then(response => {
                    this.mainSubDepOptions = response.data;
                })
                .catch(error => {
                        this.errorMessage = error //'خطایی در گرفتن اطلاعات کاربر رخ داد'; //error.data
                });
          },
          depList2(sdep){
             axios.get(serverUrl+'/tel/deps/?level=2'+'&super-dep='+sdep)
                .then(response => {
                    this.subSubDepOptions = response.data;
                })
                .catch(error => {
                        this.errorMessage = error //'خطایی در گرفتن اطلاعات کاربر رخ داد'; //error.data
                });
          },
          updateFamily(newValue) {
            this.family = newValue;
            return
          },
          updateErrorArray(){
          },
          updateDep(newValue){
            if (newValue && newValue != '0'){
              this.mainDep = newValue;
              this.finalDep = newValue;
              this.mainSubDepOptions = this.depList1(this.finalDep);
            }
            else {
              this.mainDep = '';
              this.finalDep = '';
            }
          },
          updateSubDep(newValue){
            if (newValue && newValue != '0'){
              this.subDep = newValue;
              this.finalDep = newValue;
              this.subSubDepOptions = this.depList2(this.finalDep);
            } 
            else {
              this.subDep = '';
              this.updateDep(this.mainDep);
            }
          },
          updateSubSubDep(newValue){
            if (newValue && newValue != '0'){
              this.subSubDep = newValue;
              this.finalDep = newValue;
            }
            else {
              this.subSubDep = '';
              this.updateSubDep(this.subDep);
            }
          },
          submitSearch(){
            let q = this.family;
            let dep = this.finalDep;
            this.searchStart = true;
            let url = serverUrl+'/tel/byname/';
            if (q && dep){
              url = url + '?q=' + q + '&dep=' + dep;
            } else if (q && !dep){
              url = url + '?q=' + q;
            } else if (!q && dep) {
              url = url + '?dep=' + dep;
            }
            axios.get(url)
                .then(response => {
                    this.searchResults = response.data;
                    this.searchStart = false;
                })
                .catch(error => {
                        this.errorMessage = error //'خطایی در گرفتن اطلاعات کاربر رخ داد'; //error.data
                        this.searchStart = false;
              });
          },
          updatefamily(newValue) {
            this.family = newValue
          },
          submitSearchEnter(){
            $event.target.blur();
            this.submitSearch();
          },
        },
        created(){
          this.depList0();
        },
   }
</script>

<template>
   <div class="flex flex-col items-center">
    <div class="text-xl text-gray-600 bg-gray-50 font-farsi font-bold hover:text-gray-900 
                  hover:ring-1 m-5 border-2 p-6 w-2/3" dir="rtl">
          
          <p class="mt-0 mb-5 text-xl text-gray-600 m-5 p-2 text-center">
              <span class="bg-red-50 py-2 px-6 font-farsi hover:text-gray-900 hover:ring-1">
                  محدوده جستجو
              </span>
          </p>
        <div class="grid grid-cols-3 gap-4 font-farsi  items-center items-stretch">
          <DropDown label_title="واحد اصلی"  :options="mainDepOptions"
                            @onChangeValue="updateDep" :order=1 />
          <DropDown label_title="زیر واحد اصلی"  :options="mainSubDepOptions"
                            @onChangeValue="updateSubDep" :order=2 />
          <DropDown label_title="زیر واحد فرعی"  :options="subSubDepOptions"
                            @onChangeValue="updateSubSubDep" :order=3 />
        </div>
      </div>

      <div class="text-xl text-gray-600 bg-gray-50 font-farsi font-bold hover:text-gray-900 
                  hover:ring-1 m-5 border-2 p-6 w-2/3" dir="rtl">
          
          <p class="mt-0 mb-5 text-xl text-gray-600 m-5 p-2 text-center">
              <span class="bg-red-50 py-2 px-6 font-farsi hover:text-gray-900 hover:ring-1">
                  قسمتی از نام یا نام خانوادگی را بنویسید
              </span>
          </p>
        <div class="grid grid-cols-1 gap-4 font-farsi">
          <InputText  v-model:value="family" @onChangeValue="updatefamily"
                          input_placeholder="نژادمحمدی" @keyup.enter="$event.target.blur()"
                          :order=1 @onStateChange="updateErrorArray"/>
          <div class="flex justify-center">
              <button type="button" @click="submitSearch"
                  class="block w-1/2 mt-7 px-7 py-3 bg-blue-600 text-white font-medium text-sm 
                  leading-snug uppercase rounded shadow-md hover:bg-blue-700 hover:shadow-lg 
                  focus:bg-blue-700 focus:shadow-lg focus:outline-none focus:ring-0 active:bg-blue-800 
                  active:shadow-lg transition duration-150 ease-in-out disabled:bg-gray-500">               
                  <svg v-if="searchStart" role="status" class="inline float-center w-4 h-4 mr-3 text-white animate-spin" viewBox="0 0 100 101" fill="none" xmlns="http://www.w3.org/2000/svg">
                  <path d="M100 50.5908C100 78.2051 77.6142 100.591 50 100.591C22.3858 100.591 0 78.2051 0 50.5908C0 22.9766 22.3858 0.59082 50 0.59082C77.6142 0.59082 100 22.9766 100 50.5908ZM9.08144 50.5908C9.08144 73.1895 27.4013 91.5094 50 91.5094C72.5987 91.5094 90.9186 73.1895 90.9186 50.5908C90.9186 27.9921 72.5987 9.67226 50 9.67226C27.4013 9.67226 9.08144 27.9921 9.08144 50.5908Z" fill="#E5E7EB"/>
                  <path d="M93.9676 39.0409C96.393 38.4038 97.8624 35.9116 97.0079 33.5539C95.2932 28.8227 92.871 24.3692 89.8167 20.348C85.8452 15.1192 80.8826 10.7238 75.2124 7.41289C69.5422 4.10194 63.2754 1.94025 56.7698 1.05124C51.7666 0.367541 46.6976 0.446843 41.7345 1.27873C39.2613 1.69328 37.813 4.19778 38.4501 6.62326C39.0873 9.04874 41.5694 10.4717 44.0505 10.1071C47.8511 9.54855 51.7191 9.52689 55.5402 10.0491C60.8642 10.7766 65.9928 12.5457 70.6331 15.2552C75.2735 17.9648 79.3347 21.5619 82.5849 25.841C84.9175 28.9121 86.7997 32.2913 88.1811 35.8758C89.083 38.2158 91.5421 39.6781 93.9676 39.0409Z" fill="currentColor"/>
                  </svg>
                  <span  v-if="!searchStart">
                    جستجو
                  </span>
              </button>
            </div>
        </div>
      </div>
      <TableResults :rows="searchResults"/>
    </div>
      
</template>
