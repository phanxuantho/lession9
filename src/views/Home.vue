<template>
  <div class="home">
   <!-- <table>
      <thead>
          <tr>
              <th v-for="(column, index) in columns" :key="index"> {{column}}</th>
          </tr>
      </thead>
      <tbody>
          <tr v-for="(item, index) in items" :key="index">
              <td v-for="(column, indexColumn) in columns" :key="indexColumn">{{item[column]}}</td>
          </tr>
      </tbody>
    </table> -->

           <table>
             
              <tbody>
                  <tr v-for="(item, index) in items" :key="index">
                      <td v-for="(column, indexColumn) in columns" :key="indexColumn">{{item.title}}</td>
                  </tr>
              </tbody>
            </table>

    
    <button type="button"  @click="CreateTable()" >Tạo Bảng</button>
    <button type="button"  @click="AddRows()" >Thêm hàng</button>
    <button type="button"  @click="AddColumns()" >Thêm cột</button><br><br>
    <button type="button"  @click="DeleteTable()" >Xóa bảng</button><br>
    <button type="button"  @click="DeleteColumns()" >Xóa cột</button><input type="text" v-model="location_c"><br>
    <button type="button"  @click="DeleteRows()" >Xóa hàng</button> <input type="text" v-model="location_r"><br>
    

  </div>
  
</template>

<style>

tbody {color: blue;}


table, th, td {
  border: 1px solid black;
}
</style>


<script>

export default {
  data() {
    return {
      items: [
            /* {
                'id':'1',
               'title': 'Nguyễn Văn A',
            },
            {
                'id':'2',
               'title': 'Nguyễn Văn B',
            } */
        ],
      columns: [ 1, 2],
      id: 1,
      id_c:2,
      location_r: 0,
      location_c: 0,
    };
  },

methods: {
    CreateTable() {
    const _this = this;
    let i;
    for (i = 0; i < 2; i++) {
          this.items.push({
          'id': _this.id,
          'title': _this.id,
          });
        _this.id++;
    }
      
    },
    AddRows() {
      const _this = this;
      this.items.push({
        'id': _this.id,
        'title': _this.id,
      });
      _this.id++;
    },
    AddColumns() {
      const _this = this;
      this.columns.push(_this.id_c);
      _this.id_c++;
    },
    DeleteRows() {
      this.items.splice(this.location_r,1);
     this.location_r = 0; 

    },
    DeleteColumns() {
      this.columns.splice(this.location_c,1);
     this.location_c = 0; 
      
    },
    DeleteTable(){
      this.columns=[];
      this.items=[];
    }
  },
};



</script>
