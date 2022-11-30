<template>
  <div class="m-paging">
    <div class="m-paging-left">
      Tổng số: <b>{{ totalEmployee }}</b> bản ghi

      <div class="paging-right_count">
        {{ this.recordStart }} - <b>{{ }}</b> bản ghi
      </div>
    </div>
    <div class="m-paging-right">
      <div class="m-paging-right-option">
        
        <div class="dropdownlist">
          <div class="title-pagging-list" >
            Số bản ghi/trang: {{filter.pageSize }}
          </div>
          <button class="btn-pagination" @click="toggleList">
            <div
              class="m-icon-arrow-dropdown m-icon-16"
              style="align-self: center"
            ></div>
          </button>
          <div
            v-if="isShowList"
            class="dropdownlist__data numbers-record-in-a-page bottom-100"
          >
            <div
              class="data-item"
              v-for="(item, index) in options"
              :key="index"
              @click="selectNumberRecord(item)"
            >
              {{ item }}
            </div>
          </div>
        </div>

        
      </div>
      <div class="m-paging-right-page">
        <!-- <button class="m-btn-first">Trước</button> -->
        <button
          :disabled="filter.pageNumber == 1"
          class="m-btn-first"
          @click="selectFrontPage">Trước</button>
          
        <button :class="{'selected': ischoose}">{{filter.pageNumber}}</button>
        <button
          :disabled="filter.pageNumber == totalPage"
          class="m-btn-last"
          @click="selectNextPage">Sau</button>
      </div>
    </div>
  </div>
</template>

<script>
import { mapActions, mapState } from "vuex";
export default {
  computed: mapState({
    filter: (state) => state.filter,
    totalEmployee: (state) => state.totalEmployee,
    totalPage : (state) => state.totalPage,

  }),
  data() {
    return {
      isShowList: false,
      f: false,
      ischoose:true,
      options: [
        "10 ",
        "20 ",
        "30 ",
        "50 ",
        "100 ",
      ],
    };
  },
  methods: {
    ...mapActions(["setFilter"]),
    ...mapActions(["getEmployees"]),
    toggleList() {
      console.log("this.toggleList");
      this.isShowList = !this.isShowList;
    },
    selectNumberRecord(pageSize) {
      const me = this;
      pageSize = pageSize.split(" ")[0];
      me.setFilter({
        pageSize: pageSize,
        pageNumber: 1,
        employeeFilter: me.filter.employeeFilter,
      });
      me.getEmployees();
      me.toggleList();
    },
    selectFrontPage() {
      const me = this;
      me.setFilter({
        pageSize: me.filter.pageSize,
        pageNumber: me.filter.pageNumber - 1,
        employeeFilter: me.filter.employeeFilter,
      });
      me.getEmployees();
    },
    // selectedNumberPage(){
    //   const me = this;
    
    // },

    numperPagingOfPage(){
      const me = this;
      if(me.filter.pageSize < me.totalEmployee ){
       me.y=me.filter.pageSize;
        me.x=me.filter.pageSize - 1;
      }
    },
    
    selectNextPage(){
          const me = this;
          me.setFilter({
              pageSize: me.filter.pageSize,
              pageNumber: me.filter.pageNumber + 1,
              employeeFilter: me.filter.employeeFilter
          });
          me.getEmployees();
    }
  },
};
</script>
<style scoped>
@import url("../css/components/paging.css");
.m-btn-first,.m-btn-last:hover{
  cursor: pointer;
}
</style>