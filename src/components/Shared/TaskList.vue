<template>
  <div class="w-100 row mx-0">
    <div v-for="task in todoList" :key="task.id" class="alert col-xl-3 col-lg-4 col-md-6" :class="task.status === 1?'alert-primary':'alert-danger'" role="alert">
      <p class="title font-weight-bold mb-2">{{ task.title }} - {{get_categories(task.category)}}</p>
      <span class="timer">{{ task.time }}</span>
      <p class="content-p mt-3">{{ task.content }}</p>

      <div class="btn-group" role="group" aria-label="Basic example">
         <button v-if="task.status === 1" type="button" class="btn btn-success" @click="updateTask('finish', task)">Hoàn Thành</button>
        <button v-else type="button" class="btn btn-success" @click="updateTask('redo', task)">Đặt lại</button>
        <button type="button" class="btn btn-danger ml-3" @click="updateTask('remove', task)">Xóa</button>
      </div>
    </div>
     
  </div>
</template>

<script>
export default {
  props: ["todoList", "categories", "handleUpdateTask"],
  data (){
    return{
      
    };
  },
  methods: {

  get_categories(_Id){
        return this.categories.filter(o => {
          return o.id === _Id;
        }).length
          ? this.categories.filter(o => {
              return o.id === _Id;
            })[0].text
          : "Không có danh mục";
  },
  updateTask(_type, _task) {
        this.handleUpdateTask(_type, _task);
 },
  
  }
}
</script>

<style>

</style>