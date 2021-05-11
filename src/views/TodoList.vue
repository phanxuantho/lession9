<template>
<div class="container">
<div class="form-row">
  <div class="col-12">
  <label for="title" class="label-for-input font-weight-bold text-left d-block">Tìm kiếm</label>
  <input id="search"  v-model="formQuery.search" @keyup="handleFilterTaskByText(formQuery.search)" type="text" class="form-control" placeholder="Tìm kiếm nhanh ghi chú của bạn">
  </div>
  <div class="col-8">
  <label for="title" class="label-for-input font-weight-bold text-left d-block">Tiêu đề</label>
  <input id="title" v-model="items.title" type="text" class="form-control"  placeholder="Tiêu đề">
  </div>
  <div class="col-4">
    <label for="title" class="label-for-input font-weight-bold text-left d-block">Danh mục</label>
    <select class="custom-select mr-sm-2" v-model="selectedCate">
<!--       <option value="1">Không có</option>
      <option value="2">Du lịch</option>
      <option value="3">Ăn uống</option>
      <option value="4">Học code</option> -->
      <option v-for="(item,idx) in categories" :key="idx" :value="item.id">{{item.text}}</option>


  </select>
  </div>
  <div class="col-12">
    <label for="title" class="label-for-input font-weight-bold text-left d-block">Nội dung</label>
    <textarea id="Content" v-model="items.content" class="form-control"   placeholder="Nội dung" rows="3"></textarea>
  </div>
  <div class="col-12 mt-4"><button type="submit" class="btn btn-success w-100" :disabled="BtnDisable" @click="HandleClick()">Thêm</button></div>
  
  <div class="col-12 mt-3 d-flex align-items-center">
    <div v-for="(item_c, idx) in categories" :key="idx" :value="item_c.id">
      <input type="checkbox" v-model="formQuery.cateQuery" :value="item_c.id" :id="`cate-${item_c.id}`" />
      <label :for="`cate-${item_c.id}`" class="mb-0 mr-3">{{ item_c.text }}</label>
    </div>
  </div>
  
  <div class="col-12 mt-4">

    <TaskList :todo-list="filterList || todoList" :categories="categories" :handle-update-task ="handleUpdateTask"/>
  </div>
</div>
</div>
</template>
<style>

</style>
<script>
import TaskList from "@/components/Shared/TaskList.vue";
export default {
 
  data() {
    return {
      categories: [
        {
            id: 1,
            text: "Không có"
        },
        {
            id: 2,
            text: "Du lịch"
        },
        {
            id: 3,
            text: "Ăn uống"
        },       
        {
            id: 4,
            text: "Học code"
        }
      ],
      items: {},
      selectedCate: null,
      BtnDisable: false,
      todoList: [],
      status: 1,
     // search: "",
      filterList: null,
      refQuery: null,
      //cateQuery: [],
      formQuery: {
          search: "",
          cateQuery: [],
        },
        /* items: [
            {
              id: 1,
              title: "",
              category: "",
              content:"",
              time: "",
            }
        ], */
      
    };
  },
components: {
TaskList,
},
watch: {
 /*   search(){
    if (this.refQuery) {
      clearTimeout(this.refQuery);
    }
    this.refQuery = setTimeout(() => {
      this.handleFilterTaskByText(this.search); // hàm tìm kiếm
      this.refQuery = null;
    }, 700);
   
    //console.log(this.refQuery);
  }, */
    
  /* cateQuery(){
  console.log(this.cateQuery);
  }, */

formQuery: {
    deep: true,
    handler() {
      if (this.refQuery) {
        clearTimeout(this.refQuery);
      }
      this.refQuery = setTimeout(() => {
        this.handleFilterTask();
        this.refQuery = null;
      }, 700);
    },
},


},
methods: {
 
  HandleClick(){
    this.BtnDisable=true;
   // let params = { ...this.items }; // spread ES6
   //params.category = this.selectedCate;
   this.items.category = this.selectedCate;
   this.items.time = new Date;
   this.items.id = new Date().getTime();
   this.items.status = this.status;
    setTimeout(() => {
    this.BtnDisable=false;
    this.todoList.push(this.items);
    this.ResetForm();
    },1000)
  
  },
  ResetForm(){
  this.items = {};
  this.selectedCate = null;
   },
  
 handleUpdateTask(type, payload) {
        // ví dụ cực kì cơ bản của kiến trúc flux - được sử dụng nhiều trong các state management
        // combine vào 1 hàm và xử lý
        switch (type) {
          case "finish":
            this.handleFinishTask(payload);
            break;
          case "redo":
            this.handleRedoTask(payload);
            break;
          case "remove":
            this.handleRemoveTask(payload);
            break;
          default:
            break;
        }
      }, 
  
  handleFinishTask(_task) {
        if (!_task) {
          return;
        }
        let ar1 = [...this.todoList];
        // copy -> filter -> thay đổi phần tử đó -> filter -> change
        // copy -> tìm index -> splice tại vị trí đó
        ar1.forEach(o => {
          if (o.id === _task.id) {
            o.status = 2;
          }
        });
        this.todoList = [...ar1];
   },
   
handleRedoTask(_task) {
  console.log("on redo");
  if (!_task) {
    return;
  }
  let ar1 = [...this.todoList];
  ar1.forEach(o => {
    if (o.id === _task.id) {
      o.status = 1;
    }
  });
  this.todoList = [...ar1];
},
handleRemoveTask(_task) {
  if (!_task) {
    return;
  }
  this.todoList = [
    ...this.todoList.filter(o => {
      return o.id !== _task.id;
    }),
  ];
},
setText(_value) {
  this.myText = _value;
  console.log(_value);
},

handleFilterTaskByText(_text) {
  if (!_text) {
    this.filterList = null;
    return;
  }
  this.filterList = [
    ...this.todoList.filter(o => {
      return o.title.includes(_text);
    }),
  ];
},
handleCurrentCateFilter(event) {
  let index = this.category.findIndex(o => {
    return o === event.target.value;
  });
  index === -1 ? this.category.push(event.target.value) : this.category.splice(index, 1);
  console.log(this.category);
},



handleFilterTask() {
 // console.log("imma filting task with ", this.formQuery.cateQuery);
  
if (this.formQuery.cateQuery.length === 0 && this.formQuery.search === "") {
    this.filterList = null;
    return;
}

let a =  this.formQuery.cateQuery.length;
let b =  this.formQuery.search;
this.filterList = [
    ...this.todoList.filter(o => {
      if(a === 0){
        return o.title.includes(this.formQuery.search);
      }
      else if (b === ""){
        return this.formQuery.cateQuery.indexOf(o.category) !== -1;
      }
      else{
        return this.formQuery.cateQuery.indexOf(o.category) !== -1 && o.title.includes(this.formQuery.search);
      }
    }),
]; 

/* let ar2 = [];
for (let i = 0; i < this.formQuery.cateQuery.length; i++){
    ar2 =  [...this.todoList.filter(o => {
    return o.category === this.formQuery.cateQuery[i];   
    }),];
    this.filterList.push(ar2);
} */




    // return  (a !== 0) ? (this.formQuery.cateQuery.indexOf(o.category) !== -1):true && (b !== "") ? (o.title.includes(this.formQuery.search)):true;
    //return this.formQuery.cateQuery.indexOf(o.category) !== -1 && o.title.includes(this.formQuery.search);


//console.log(a,"---",b);
/* this.filterList = [
    ...this.todoList.filter(o => {
      return  this.formQuery.cateQuery.indexOf(o.category) !== -1 && o.title.includes(this.formQuery.search);
    }),
]; */


/* this.filterList = [
    ...this.todoList.filter(o => {
      return o.title.includes(this.formQuery.search);
    }),
]; */







/*   this.filterList = [
    ...this.todoList.filter(o => {
        this.formQuery.cateQuery.forEach(i => {
        return o.category === i;     
        });
    }),
  ];  
  console.log(this.filterList); */
/* let ar2 = null;
this.filterList = null;
for (let i = 0; i < this.formQuery.cateQuery.length; i++){ */
  /* this.filterList.push(
        this.todoList.filter(o => {
        return o.category === this.formQuery.cateQuery[i];   
        }),
  ); */ //ar2 = [];
       // ar2 = null;
/*         ar2 =  [...this.todoList.filter(o => {
        return o.category === this.formQuery.cateQuery[i];   
        }),];
        console.log(ar2);
       this.filterList.push(ar2);
         

} */
 
//console.log("Filter: ",this.filterList); 

/* 
var arr1 = [1,2,3,4],
    arr2 = [2,4],
    res = arr1.filter(item => !arr2.includes(item));
console.log(res); */


},

handleResetFilter() {
  this.formQuery = {
    textQuery: "",
    cateQuery: [],
  };
},
},
};
</script>
